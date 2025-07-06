# uMod Plugins Dataset - Code Abstractions

Generated: 2025-07-06 20:39:06
Total plugins: 956

## 1Logger

```csharp
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Libraries.Covalence;
using System;
using System.Collections.Generic;
using System.IO;
using System.Linq;
using System.Net;
using System.Net.Security;

Oxide.Plugins
[Info("Logger", "Frizen", "1.0.0")]
[Description("Отправляет логи всех комманд сервера")]
public class Logger : RustPlugin
{
    [JsonProperty("Токен от группы ВК(От группы будут идти сообщения в беседу.Вам нужно добавить свою группу в беседу!)")]
    public string Token;
    [JsonProperty("ID беседы для группы")]
    public string ChatID;
    private Configuration config;
    public class Configuration
    {
        [JsonProperty(PropertyName = "Логировать сообщения в чате (true/false)")]
        public bool LogChat { get; set; }
        [JsonProperty(PropertyName = "Логировать использования комманд (true/false)")]
        public bool LogCommands { get; set; }
        [JsonProperty(PropertyName = "Логировать заходы (true/false)")]
        public bool LogConnections { get; set; }
        [JsonProperty(PropertyName = "Логировать выходы (true/false)")]
        public bool LogDisconnections { get; set; }
        [JsonProperty(PropertyName = "Вывод логов вк и сохранение файла с логами (true/false)")]
        public bool LogToConsole { get; set; }
        [JsonProperty("Причина кика за VPN")]
        public string KickPlayerMessage;
        [JsonProperty("Причина кика за AdminAbuse")]
        public string AdminAbuse;
        [JsonProperty(PropertyName = "Логировать по дням (true/false)")]
        public bool RotateLogs { get; set; }
        [JsonProperty(PropertyName = "Лист комманд (полные или краткие)")]
        public List<string> CommandList { get; set; }
        [JsonProperty("Список SteamID которых не нужно проверять")]
        public List<ulong> IgnoreList { get; set; }
        [JsonProperty(PropertyName = "Тип списка команд (blacklist or whitelist)")]
        public string CommandListType { get; set; }
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private new void LoadDefaultMessages();
    private const string commandReason;
    private const string permReason;
    private void Init();
    private void OnUserChat(IPlayer player, string message);
    private void OnUserConnected(IPlayer player);
    private void OnUserDisconnected(IPlayer player);
    private void OnRconCommand(IPEndPoint ip, string command, string[] args);
    private object OnServerCommand(ConsoleSystem.Arg arg);
    private void OnUserCommand(IPlayer player, string command, string[] args);
    private void ReasonCommand(IPlayer player, string command, string[] args);
    private string Lang(string key, string id, object[] args);
    private void AddLocalizedCommand(string key, string command);
     void VKSendMessage(string Message);
    private void Log(string filename, string key, object[] args);
    private void Message(IPlayer player, string key, object[] args);
     void OnPlayerConnected(BasePlayer player);
}

public class Configuration
{
    [JsonProperty(PropertyName = "Логировать сообщения в чате (true/false)")]
    public bool LogChat { get; set; }
    [JsonProperty(PropertyName = "Логировать использования комманд (true/false)")]
    public bool LogCommands { get; set; }
    [JsonProperty(PropertyName = "Логировать заходы (true/false)")]
    public bool LogConnections { get; set; }
    [JsonProperty(PropertyName = "Логировать выходы (true/false)")]
    public bool LogDisconnections { get; set; }
    [JsonProperty(PropertyName = "Вывод логов вк и сохранение файла с логами (true/false)")]
    public bool LogToConsole { get; set; }
    [JsonProperty("Причина кика за VPN")]
    public string KickPlayerMessage;
    [JsonProperty("Причина кика за AdminAbuse")]
    public string AdminAbuse;
    [JsonProperty(PropertyName = "Логировать по дням (true/false)")]
    public bool RotateLogs { get; set; }
    [JsonProperty(PropertyName = "Лист комманд (полные или краткие)")]
    public List<string> CommandList { get; set; }
    [JsonProperty("Список SteamID которых не нужно проверять")]
    public List<ulong> IgnoreList { get; set; }
    [JsonProperty(PropertyName = "Тип списка команд (blacklist or whitelist)")]
    public string CommandListType { get; set; }
}


```

---

## 1PlayerWear

```csharp
using System;
using System.Collections.Generic;
using Oxide.Core;
using Newtonsoft.Json;

Oxide.Plugins
[Info("PlayerWear", "Drop Dead", "1.0.0")]
public class PlayerWear : RustPlugin
{
    public Dictionary<ulong, string> command;
    private void SaveData();
    private void LoadData();
    public class items
    {
        [JsonProperty("Шортнейм")]
        public string shortname;
        [JsonProperty("Количество")]
        public int amount;
        [JsonProperty("Скин")]
        public ulong skinid;
        [JsonProperty("Контейнер (wear/belt/main)")]
        public string container;
        [JsonProperty("Заблокировать перемещение?")]
        public bool move;
        [JsonProperty("Заблокировать дроп?")]
        public bool drop;
    }

    private PluginConfig cfg;
    public class PluginConfig
    {
        [JsonProperty("Предметы по группам")]
        public Dictionary<string, List<items>> perms;
    }

    private void Init();
    protected override void LoadDefaultConfig();
     void OnServerInitialized();
     void OnPlayerConnected(BasePlayer player);
     void CheckPlayer(BasePlayer player);
     void OnPlayerRespawned(BasePlayer player);
     object CanMoveItem(Item item, PlayerInventory playerLoot, uint targetContainer, int targetSlot, int amount);
     object OnItemAction(Item item, string action, BasePlayer player);
    [ChatCommand("event")]
     void EventCmd(BasePlayer player, string commands, string[] args);
}

public class items
{
    [JsonProperty("Шортнейм")]
    public string shortname;
    [JsonProperty("Количество")]
    public int amount;
    [JsonProperty("Скин")]
    public ulong skinid;
    [JsonProperty("Контейнер (wear/belt/main)")]
    public string container;
    [JsonProperty("Заблокировать перемещение?")]
    public bool move;
    [JsonProperty("Заблокировать дроп?")]
    public bool drop;
}

public class PluginConfig
{
    [JsonProperty("Предметы по группам")]
    public Dictionary<string, List<items>> perms;
}


```

---

## 1RatesController

```csharp
using Oxide.Core;
using System;
using System.Collections.Generic;
using System.Linq;
using UnityEngine;

Oxide.Plugins
[Info("RatesController", "Vlad-00003", "2.2.4")]
[Description("Rate and Time controller - can change rates based on current game time")]
 class RatesController : RustPlugin
{
    private bool isDay;
    private string PickUpDayString;
    private string PickUpNightString;
    private string GatherDayString;
    private string GatherNightString;
    private string QuerryDayString;
    private string QuerryNightString;
    private string LootDayString;
    private string LootNightString;
    private string SmeltDayString;
    private string SmeltNightString;
    private ItemDefinition hqmo;
    private string DefaultRatesCfg;
    private string CustomRatesCfg;
    private string DefaultPickupRatesCfg;
    private string DefaultGatherRatesCfg;
    private string DefaultSmeltRatesCfg;
    private string DefaultQuerryRatesCfg;
    private string PrefixCfg;
    private string PrefixColorCfg;
    private string UseLootMultyplierCfg;
    private string BlacklistedLootCfgOld;
    private string ListedLootCfg;
    private string IsBlacklistCfg;
    private string DayStartCfg;
    private string DayLenghtCfg;
    private string NightStartCfg;
    private string NightLenghtCfg;
    private string WarnChatCfg;
    private string CoalRateDayCfg;
    private string CoalRateNightCfg;
    private string CoalChanceDayCfg;
    private string CoalChanceNightCfg;
    private string MoreHQMCfg;
    private List<string> AvaliableMods;
    private Dictionary<string, float> SmeltBackup;
     Dictionary<uint, EntityLootetrs> CratesCD;
    private class EntityLootetrs
    {
        public List<ulong> Looters;
        public DateTime NextUpdate;
        public EntityLootetrs();
        public bool IsUpdateBlocked(BasePlayer player);
        public void AddLooter(ulong looter, int minutes);
    }

    private double CoalRate;
    private int CoalChance;
    private double SmeltRate;
    private double GatherRate;
    private double PickupRate;
    private double QuerryRate;
    private double LootRate;
    private string Prefix;
    private bool UseLootMultyplier;
    private string PrefixColor;
    private float DayStart;
    private float NightStart;
    private uint DayLenght;
    private uint NightLenght;
    private bool WarnChat;
    private bool MoreHQM;
    private Dictionary<string, double> DefaultRates;
    private Dictionary<string, Dictionary<string, double>> CustomRates;
    private double CoalRateDay;
    private double CoalRateNight;
    private int CoalChanceDay;
    private int CoalChanceNight;
    private Dictionary<string, double> DefaultSmeltRates;
    private Dictionary<string, double> DefaultGatherRates;
    private Dictionary<string, double> DefaultPickupRates;
    private Dictionary<string, double> DefaultQuerryRates;
    private List<string> ListedLoot;
    private bool IsBlackList;
    protected override void LoadDefaultConfig();
     void LoadConfigValues();
     void LoadMessages();
     void OnServerInitialized();
     void Unload();
    private TOD_Time timeComponent;
    private bool Frozen;
    private uint componentSearchAttempts;
    private void GetTimeComponent();
    private bool ProgressTime { get; set; }
    private void OnHour();
     void UpdateDayLenght(uint Lenght, bool night);
    public float CurrentHour { get; set; }
     void OnContainerDropItems(ItemContainer container);
     void OnLootEntity(BasePlayer player, BaseEntity entity);
    private void UpdateLoot(LootContainer container, double rate, ulong pIayerid);
    private bool ListedLootContains(Item item);
    private void UpdateFurnaces();
     float CookingTemperature(BaseOven.TemperatureType temperature);
     object OnOvenToggle(BaseOven oven, BasePlayer player);
     void StartCooking(BaseOven oven, double ovenMultiplier);
     Item FindBurnable(BaseOven oven);
    private void OnDayStart();
    private void OnNightStart();
    private void RatesToChat();
     void OnCollectiblePickup(Item item, BasePlayer player);
     void OnGrowableGathered(GrowableEntity plant, Item item, BasePlayer player, ulong playerid);
     void OnEntityKill(BaseNetworkable entity);
     void OnDispenserBonus(ResourceDispenser disp, BasePlayer player, Item item);
     void OnDispenserGather(ResourceDispenser dispenser, BaseEntity entity, Item item);
     void OnQuarryGather(MiningQuarry quarry, Item item);
     void OnFuelConsume(BaseOven oven, Item fuel, ItemModBurnable burnable);
     void UpdateSmeltTime();
    [ConsoleCommand("env.freeze")]
     void TimeFreeze(ConsoleSystem.Arg arg);
    [ConsoleCommand("env.unfreeze")]
     void TimeUnFreeze(ConsoleSystem.Arg arg);
    [ConsoleCommand("rates.show")]
     void ShowRates(ConsoleSystem.Arg arg);
    [ChatCommand("rates")]
    private void ShowRatesChat(BasePlayer player, string command, string[] args);
    private void GetConfig(string Key, T var);
    private void SendToChat(BasePlayer Player, string Message);
    private void SendToChat(string Message);
     string GetMsg(string key, object userID);
     double GetUserRates(string steamId, string RateType);
     double GetUserRates(ulong steamId, string RateType);
     Dictionary<string, object> CreatePerms(List<string> mods, double rate);
}

private class EntityLootetrs
{
    public List<ulong> Looters;
    public DateTime NextUpdate;
    public EntityLootetrs();
    public bool IsUpdateBlocked(BasePlayer player);
    public void AddLooter(ulong looter, int minutes);
}


```

---

## AAlertRaid

```csharp
using System;
using System.Collections;
using System.Collections.Generic;
using System.Linq;
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Plugins;
using UnityEngine;
using CompanionServer;
using Oxide.Ext.Discord;
using Oxide.Ext.Discord.Attributes;
using Oxide.Ext.Discord.Entities.Messages;
using Oxide.Ext.Discord.Entities.Channels;
using Oxide.Ext.Discord.Entities.Guilds;
using Oxide.Ext.Discord.Entities.Gatway;
using Oxide.Ext.Discord.Entities.Gatway.Events;
using Oxide.Ext.Discord.Entities.Messages.Embeds;
using Oxide.Ext.Discord.Entities.Permissions;
using Oxide.Ext.Discord.Entities;
using System.Text.RegularExpressions;
using Oxide.Ext.Discord.Builders.MessageComponents;
using Oxide.Ext.Discord.Entities.Interactions.MessageComponents;
using Oxide.Ext.Discord.Entities.Interactions;
using Oxide.Ext.Discord.Entities.Users;
using Oxide.Core.Libraries.Covalence;
using ru = Oxide.Game.Rust;
using ConVar;

Oxide.Plugins
[Info("AAlertRaid", "fermens", "0.0.72")]
public class AAlertRaid : RustPlugin
{
    const bool fermensEN;
    private static PluginConfig config;
    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    protected override void SaveConfig();
     class VK
    {
        [JsonProperty(fermensEN ? "Enable?" : "Включить?")]
        public bool enable;
        [JsonProperty(fermensEN ? "API" : "API от группы")]
        public string api;
        [JsonProperty(fermensEN ? "Link to the group" : "Ссылка на группу")]
        public string link;
        [JsonProperty(fermensEN ? "Cooldown for sending" : "Кд на отправку")]
        public float cooldown;
    }

     class RUSTPLUS
    {
        [JsonProperty(fermensEN ? "Enable?" : "Включить?")]
        public bool enable;
        [JsonProperty(fermensEN ? "Cooldown for sending" : "Кд на отправку")]
        public float cooldown;
    }

     class INGAME
    {
        [JsonProperty(fermensEN ? "Enable?" : "Включить?")]
        public bool enable;
        [JsonProperty(fermensEN ? "Cooldown for sending" : "Кд на отправку")]
        public float cooldown;
        [JsonProperty(fermensEN ? "Send game effect when notification are received" : "Эффект при получении уведомления")]
        public string effect;
        [JsonProperty(fermensEN ? "Time after the UI is destroyed" : "Время, через которое пропадает UI [секунды]")]
        public float destroy;
        [JsonProperty("UI")]
        public string UI;
    }

     class DISCORD
    {
        [JsonProperty(fermensEN ? "Enable?" : "Включить?")]
        public bool enable;
        [JsonProperty(fermensEN ? "Cooldown for sending" : "Кд на отправку")]
        public float cooldown;
        [JsonProperty(fermensEN ? "Invitation link" : "Приглашение в группу")]
        public string link;
        [JsonProperty(fermensEN ? "Token (https://discordapp.com/developers/applications)" : "Токен бота (https://discordapp.com/developers/applications)")]
        public string token;
        [JsonProperty(fermensEN ? "Channel ID, where the player will take the code to confirm the profile" : "ID канала, гле игрок будет брать код, для подтверджения профиля")]
        public string channel;
        [JsonProperty(fermensEN ? "Info text" : "Дискорд канал с получением кода - текст")]
        public string channeltext;
        [JsonProperty(fermensEN ? "Info text - line color on the left" : "Дискорд канал с получением кода - цвет линии слева (https://gist.github.com/thomasbnt/b6f455e2c7d743b796917fa3c205f812#file-code_colors_discordjs-md)")]
        public uint channelcolor;
        [JsonProperty(fermensEN ? "Text on button" : "Дискорд канал с получением кода - кнопка")]
        public string channelbutton;
        [JsonProperty(fermensEN ? "Reply after button click" : "Дискорд канал с получением кода - ответ")]
        public string channelex;
        [JsonProperty(fermensEN ? "Don't touch this field" : "Дискорд канал с получением кода - ID сообщения (не трогаем! сам заполнится!)")]
        public string channelmessageid;
    }

     class TELEGRAM
    {
        [JsonProperty(fermensEN ? "Enable?" : "Включить?")]
        public bool enable;
        [JsonProperty(fermensEN ? "Cooldown for sending" : "Кд на отправку")]
        public float cooldown;
        [JsonProperty(fermensEN ? "Bot tag" : "Тэг бота")]
        public string bottag;
        [JsonProperty(fermensEN ? "Token" : "Токен")]
        public string token;
    }

     class UIMenu
    {
        [JsonProperty(fermensEN ? "Background color" : "Цвет фона")]
        public string background;
        [JsonProperty(fermensEN ? "Strip color" : "Цвет полоски")]
        public string stripcolor;
        [JsonProperty(fermensEN ? "Rectangular container background color" : "Цвет фона прямоугольного контейнера")]
        public string rectangularcolor;
        [JsonProperty(fermensEN ? "Button text color" : "Цвет текста в кнопке")]
        public string buttoncolortext;
        [JsonProperty(fermensEN ? "Text color" : "Цвет текста")]
        public string textcolor;
        [JsonProperty(fermensEN ? "Green button color" : "Цвет зелёной кнопки")]
        public string greenbuttoncolor;
        [JsonProperty(fermensEN ? "Red button color" : "Цвет красной кнопки")]
        public string redbuttoncolor;
        [JsonProperty(fermensEN ? "Gray button color" : "Цвет серой кнопки")]
        public string graybuttoncolor;
        [JsonProperty(fermensEN ? "Header text color" : "Цвет текста заголовка")]
        public string headertextcolor;
        [JsonProperty(fermensEN ? "Error text color" : "Цвет текста ошибки")]
        public string errortextcolor;
        [JsonProperty(fermensEN ? "Text color of <exit> and <back> buttons" : "Цвет текста кнопок <выход> и <назад>")]
        public string colortextexit;
        [JsonProperty(fermensEN ? "Rectangular container text color" : "Цвет текст прямоугольного контейнера")]
        public string rectangulartextcolor;
        [JsonProperty(fermensEN ? "The color of the text with hints at the bottom of the screen" : "Цвет текста с подсказками внизу экрана")]
        public string hintstextcolor;
        [JsonProperty(fermensEN ? "Abbreviations and their colors" : "Аббревиатуры и их цвета")]
        public UIMainMenu uIMainMenu;
    }

     class UIMainMenu
    {
        [JsonProperty(fermensEN ? "Abbreviation for telegram" : "Аббревиатура для телеграма")]
        public string abr_telegram;
        [JsonProperty(fermensEN ? "Telegram icon color" : "Цвет иконки телеграма")]
        public string color_telegram;
        [JsonProperty(fermensEN ? "Abbreviation for vk.com" : "Аббревиатура для вконтакте")]
        public string abr_vk;
        [JsonProperty(fermensEN ? "Vk.com icon color" : "Цвет иконки вконтакте")]
        public string color_vk;
        [JsonProperty(fermensEN ? "Abbreviation for rust+" : "Аббревиатура для rust+")]
        public string abr_rustplus;
        [JsonProperty(fermensEN ? "Rust+ icon color" : "Цвет иконки rust+")]
        public string color_rustplus;
        [JsonProperty(fermensEN ? "Abbreviation for discord" : "Аббревиатура для дискорда")]
        public string abr_discord;
        [JsonProperty(fermensEN ? "Discord icon color" : "Цвет иконки дискорда")]
        public string color_discord;
        [JsonProperty(fermensEN ? "Abbreviation for in game" : "Аббревиатура для графическое отображение в игре")]
        public string abr_ui;
        [JsonProperty(fermensEN ? "In game icon color" : "Цвет иконки графическое отображение в игре")]
        public string color_ui;
    }

    private class PluginConfig
    {
        [JsonProperty(fermensEN ? "Server name, will using for alerts" : "Название сервера - для оповещений")]
        public string servername;
        [JsonProperty(fermensEN ? "Raid alert works only for those who have permission" : "Оповещение о рейде работает только для тех, у кого есть разрешение")]
        public bool needpermission;
        [JsonProperty(fermensEN ? "VK.com" : "Оповещание о рейде в ВК")]
        public VK vk;
        [JsonProperty(fermensEN ? "Rust+" : "Оповещание о рейде в Rust+")]
        public RUSTPLUS rustplus;
        [JsonProperty(fermensEN ? "In game" : "Оповещание о рейде в игре")]
        public INGAME ingame;
        [JsonProperty(fermensEN ? "Discord" : "Оповещание о рейде в дискорд")]
        public DISCORD discord;
        [JsonProperty(fermensEN ? "Telegram" : "Оповещание о рейде в телеграм")]
        public TELEGRAM telegram { get; set; }
        [JsonProperty(fermensEN ? "Menu UI" : "Настройка UI")]
        public UIMenu ui { get; set; }
        [JsonProperty(fermensEN ? "Additional list" : "Дополнительный список предметов, которые учитывать")]
        public string[] spisok;
        [JsonProperty(fermensEN ? "Notification when usual items are destroyed" : "Оповещение при уничтожении обычных предметов")]
        public bool extralist;
        public static PluginConfig DefaultConfig();
    }

    private static string[] _spisok;
    private readonly DiscordSettings _discordSettings;
    private DiscordGuild _guild;
    [DiscordClient]
     DiscordClient Client;
    private void CreateClient();
    private void OnDiscordInteractionCreated(DiscordInteraction interaction);
    private void HandleAcceptLinkButton(DiscordInteraction interaction, DiscordUser user);
    private void OnDiscordGatewayReady(GatewayReadyEvent ready);
    private void CloseClient();
    private void CREATECHANNEL(string dsid, string text);
    private void SENDMESSAGE(string dsid, string text);
    public List<ActionRowComponent> CreateComponents(string button);
    private readonly List<Regex> _regexTags;
    private readonly List<string> _tags;
    private string STRIP(string original);
    private DiscordChannel GetChannel(string id);
     string connect;
     string FON;
     string MAIN;
     string UI;
     string IF2;
     string IF2A;
     string BTN;
     string ER;
     string IBLOCK;
     string MAINH;
     string AG;
     string EXIT;
     string BACK;
     class Storage
    {
        public string vk;
        public string telegram;
        public ulong discord;
        public bool rustplus;
        public bool ingamerust { get; set; }
    }

    private Storage GetStorage(ulong userid);
    private void SaveStorage(BasePlayer player);
    private IEnumerator Saving(string userid, Storage storage);
     Dictionary<ulong, Storage> datas;
    private void GetRequestTelegram(string reciverID, string msg, BasePlayer player, bool accept);
    private IEnumerator GetCallbackTelegram(int code, string response, string id, BasePlayer player, bool accept);
    const string connects;
     class ALERT
    {
        public DateTime gamecooldown;
        public DateTime rustpluscooldown;
        public DateTime vkcooldown;
        public DateTime discordcooldown;
        public DateTime vkcodecooldown;
        public DateTime telegramcooldown;
        public DateTime telegramcodecooldown;
    }

    private static Dictionary<ulong, ALERT> alerts;
     class CODE
    {
        public string id;
        public ulong gameid;
    }

    private Dictionary<string, CODE> VKCODES;
    private Dictionary<ulong, string> DISCORDCODES;
    private void GetRequest(string reciverID, string msg, BasePlayer player, string num);
    private void SendError(BasePlayer player, string key);
    private IEnumerator GetCallbackVK(int code, string response, string id, BasePlayer player, string num);
    private string perm;
    [PluginReference]
     Plugin BMenu;
    private void callcommandrn(BasePlayer player, string command, string[] arg);
    private bool HasAcces(string id);
    private void OpenMenu(BasePlayer player, bool first);
     class C
    {
        public string min;
        public string max;
    }

     Dictionary<int, C> _caddele;
    private void AddElementUI(BasePlayer player, string name, string color, string button, string command, string ico, string icocolor, int num);
     Dictionary<ulong, string> write;
    [ConsoleCommand("raid.input")]
     void ccmdopeinput(ConsoleSystem.Arg arg);
    private void SendError2(BasePlayer player, string key);
    [ConsoleCommand("raid.ingame")]
     void raplsgame(ConsoleSystem.Arg arg);
    [ConsoleCommand("raid.rustplus")]
     void rapls(ConsoleSystem.Arg arg);
    [ConsoleCommand("raid.discordadd")]
     void ccmdadiscoradd(ConsoleSystem.Arg arg);
    [ConsoleCommand("raid.acceptds")]
     void raidacceptds(ConsoleSystem.Arg arg);
    [ConsoleCommand("raid.discorddelete")]
     void vdiscorddelete(ConsoleSystem.Arg arg);
    [ConsoleCommand("raid.tgdelete")]
     void rgdelete(ConsoleSystem.Arg arg);
    [ConsoleCommand("raid.tgadd")]
     void ccmdtgadd(ConsoleSystem.Arg arg);
    [ConsoleCommand("raid.accepttg")]
     void ccmdaccepttg(ConsoleSystem.Arg arg);
    [ConsoleCommand("raid.vkdelete")]
     void vkdelete(ConsoleSystem.Arg arg);
    [ConsoleCommand("raid.vkadd")]
     void ccmdavkadd(ConsoleSystem.Arg arg);
    [ConsoleCommand("raid.accept")]
     void ccmdaccept(ConsoleSystem.Arg arg);
    [ConsoleCommand("raid.send")]
     void ccmdopesendt(ConsoleSystem.Arg arg);
    private string RANDOMNUM();
    private void Unload();
    private void OnServerInitialized();
     class FERMENS
    {
        public string fon;
        public string main;
        public string ui;
        public string if2;
        public string if2a;
        public string btn;
        public string er;
        public string iblock;
        public string mainh;
        public string exit;
        public string back;
        public string ag;
        public Dictionary<string, string> messagesEN;
        public Dictionary<string, string> messagesRU;
    }

     void LoadData();
     IEnumerator GetCallback(int code, string response);
    private void OnEntityDeath(BaseCombatEntity entity, HitInfo info);
    private IEnumerator Alerting(BaseCombatEntity entity, BasePlayer player, int tt);
     List<ulong> block;
    private void ALERTPLAYER(ulong ID, string name, string quad, string connect, string destroy, string attackerid);
    private Dictionary<ulong, Timer> timal;
    private string GetMessage(string key, string userId);
    private static Dictionary<string, Vector3> Grids;
    private void CreateSpawnGrid();
    private string GetNameGrid(Vector3 pos);
}

 class VK
{
    [JsonProperty(fermensEN ? "Enable?" : "Включить?")]
    public bool enable;
    [JsonProperty(fermensEN ? "API" : "API от группы")]
    public string api;
    [JsonProperty(fermensEN ? "Link to the group" : "Ссылка на группу")]
    public string link;
    [JsonProperty(fermensEN ? "Cooldown for sending" : "Кд на отправку")]
    public float cooldown;
}

 class RUSTPLUS
{
    [JsonProperty(fermensEN ? "Enable?" : "Включить?")]
    public bool enable;
    [JsonProperty(fermensEN ? "Cooldown for sending" : "Кд на отправку")]
    public float cooldown;
}

 class INGAME
{
    [JsonProperty(fermensEN ? "Enable?" : "Включить?")]
    public bool enable;
    [JsonProperty(fermensEN ? "Cooldown for sending" : "Кд на отправку")]
    public float cooldown;
    [JsonProperty(fermensEN ? "Send game effect when notification are received" : "Эффект при получении уведомления")]
    public string effect;
    [JsonProperty(fermensEN ? "Time after the UI is destroyed" : "Время, через которое пропадает UI [секунды]")]
    public float destroy;
    [JsonProperty("UI")]
    public string UI;
}

 class DISCORD
{
    [JsonProperty(fermensEN ? "Enable?" : "Включить?")]
    public bool enable;
    [JsonProperty(fermensEN ? "Cooldown for sending" : "Кд на отправку")]
    public float cooldown;
    [JsonProperty(fermensEN ? "Invitation link" : "Приглашение в группу")]
    public string link;
    [JsonProperty(fermensEN ? "Token (https://discordapp.com/developers/applications)" : "Токен бота (https://discordapp.com/developers/applications)")]
    public string token;
    [JsonProperty(fermensEN ? "Channel ID, where the player will take the code to confirm the profile" : "ID канала, гле игрок будет брать код, для подтверджения профиля")]
    public string channel;
    [JsonProperty(fermensEN ? "Info text" : "Дискорд канал с получением кода - текст")]
    public string channeltext;
    [JsonProperty(fermensEN ? "Info text - line color on the left" : "Дискорд канал с получением кода - цвет линии слева (https://gist.github.com/thomasbnt/b6f455e2c7d743b796917fa3c205f812#file-code_colors_discordjs-md)")]
    public uint channelcolor;
    [JsonProperty(fermensEN ? "Text on button" : "Дискорд канал с получением кода - кнопка")]
    public string channelbutton;
    [JsonProperty(fermensEN ? "Reply after button click" : "Дискорд канал с получением кода - ответ")]
    public string channelex;
    [JsonProperty(fermensEN ? "Don't touch this field" : "Дискорд канал с получением кода - ID сообщения (не трогаем! сам заполнится!)")]
    public string channelmessageid;
}

 class TELEGRAM
{
    [JsonProperty(fermensEN ? "Enable?" : "Включить?")]
    public bool enable;
    [JsonProperty(fermensEN ? "Cooldown for sending" : "Кд на отправку")]
    public float cooldown;
    [JsonProperty(fermensEN ? "Bot tag" : "Тэг бота")]
    public string bottag;
    [JsonProperty(fermensEN ? "Token" : "Токен")]
    public string token;
}

 class UIMenu
{
    [JsonProperty(fermensEN ? "Background color" : "Цвет фона")]
    public string background;
    [JsonProperty(fermensEN ? "Strip color" : "Цвет полоски")]
    public string stripcolor;
    [JsonProperty(fermensEN ? "Rectangular container background color" : "Цвет фона прямоугольного контейнера")]
    public string rectangularcolor;
    [JsonProperty(fermensEN ? "Button text color" : "Цвет текста в кнопке")]
    public string buttoncolortext;
    [JsonProperty(fermensEN ? "Text color" : "Цвет текста")]
    public string textcolor;
    [JsonProperty(fermensEN ? "Green button color" : "Цвет зелёной кнопки")]
    public string greenbuttoncolor;
    [JsonProperty(fermensEN ? "Red button color" : "Цвет красной кнопки")]
    public string redbuttoncolor;
    [JsonProperty(fermensEN ? "Gray button color" : "Цвет серой кнопки")]
    public string graybuttoncolor;
    [JsonProperty(fermensEN ? "Header text color" : "Цвет текста заголовка")]
    public string headertextcolor;
    [JsonProperty(fermensEN ? "Error text color" : "Цвет текста ошибки")]
    public string errortextcolor;
    [JsonProperty(fermensEN ? "Text color of <exit> and <back> buttons" : "Цвет текста кнопок <выход> и <назад>")]
    public string colortextexit;
    [JsonProperty(fermensEN ? "Rectangular container text color" : "Цвет текст прямоугольного контейнера")]
    public string rectangulartextcolor;
    [JsonProperty(fermensEN ? "The color of the text with hints at the bottom of the screen" : "Цвет текста с подсказками внизу экрана")]
    public string hintstextcolor;
    [JsonProperty(fermensEN ? "Abbreviations and their colors" : "Аббревиатуры и их цвета")]
    public UIMainMenu uIMainMenu;
}

 class UIMainMenu
{
    [JsonProperty(fermensEN ? "Abbreviation for telegram" : "Аббревиатура для телеграма")]
    public string abr_telegram;
    [JsonProperty(fermensEN ? "Telegram icon color" : "Цвет иконки телеграма")]
    public string color_telegram;
    [JsonProperty(fermensEN ? "Abbreviation for vk.com" : "Аббревиатура для вконтакте")]
    public string abr_vk;
    [JsonProperty(fermensEN ? "Vk.com icon color" : "Цвет иконки вконтакте")]
    public string color_vk;
    [JsonProperty(fermensEN ? "Abbreviation for rust+" : "Аббревиатура для rust+")]
    public string abr_rustplus;
    [JsonProperty(fermensEN ? "Rust+ icon color" : "Цвет иконки rust+")]
    public string color_rustplus;
    [JsonProperty(fermensEN ? "Abbreviation for discord" : "Аббревиатура для дискорда")]
    public string abr_discord;
    [JsonProperty(fermensEN ? "Discord icon color" : "Цвет иконки дискорда")]
    public string color_discord;
    [JsonProperty(fermensEN ? "Abbreviation for in game" : "Аббревиатура для графическое отображение в игре")]
    public string abr_ui;
    [JsonProperty(fermensEN ? "In game icon color" : "Цвет иконки графическое отображение в игре")]
    public string color_ui;
}

private class PluginConfig
{
    [JsonProperty(fermensEN ? "Server name, will using for alerts" : "Название сервера - для оповещений")]
    public string servername;
    [JsonProperty(fermensEN ? "Raid alert works only for those who have permission" : "Оповещение о рейде работает только для тех, у кого есть разрешение")]
    public bool needpermission;
    [JsonProperty(fermensEN ? "VK.com" : "Оповещание о рейде в ВК")]
    public VK vk;
    [JsonProperty(fermensEN ? "Rust+" : "Оповещание о рейде в Rust+")]
    public RUSTPLUS rustplus;
    [JsonProperty(fermensEN ? "In game" : "Оповещание о рейде в игре")]
    public INGAME ingame;
    [JsonProperty(fermensEN ? "Discord" : "Оповещание о рейде в дискорд")]
    public DISCORD discord;
    [JsonProperty(fermensEN ? "Telegram" : "Оповещание о рейде в телеграм")]
    public TELEGRAM telegram { get; set; }
    [JsonProperty(fermensEN ? "Menu UI" : "Настройка UI")]
    public UIMenu ui { get; set; }
    [JsonProperty(fermensEN ? "Additional list" : "Дополнительный список предметов, которые учитывать")]
    public string[] spisok;
    [JsonProperty(fermensEN ? "Notification when usual items are destroyed" : "Оповещение при уничтожении обычных предметов")]
    public bool extralist;
    public static PluginConfig DefaultConfig();
}

 class Storage
{
    public string vk;
    public string telegram;
    public ulong discord;
    public bool rustplus;
    public bool ingamerust { get; set; }
}

 class ALERT
{
    public DateTime gamecooldown;
    public DateTime rustpluscooldown;
    public DateTime vkcooldown;
    public DateTime discordcooldown;
    public DateTime vkcodecooldown;
    public DateTime telegramcooldown;
    public DateTime telegramcodecooldown;
}

 class CODE
{
    public string id;
    public ulong gameid;
}

 class C
{
    public string min;
    public string max;
}

 class FERMENS
{
    public string fon;
    public string main;
    public string ui;
    public string if2;
    public string if2a;
    public string btn;
    public string er;
    public string iblock;
    public string mainh;
    public string exit;
    public string back;
    public string ag;
    public Dictionary<string, string> messagesEN;
    public Dictionary<string, string> messagesRU;
}


```

---

## AAlertRaid-1.0.0

```csharp
using System;
using System.Collections;
using System.Collections.Generic;
using System.Linq;
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Plugins;
using UnityEngine;
using CompanionServer;
using Oxide.Ext.Discord.Entities;
using System.Text.RegularExpressions;
using Oxide.Core.Libraries.Covalence;
using ru = Oxide.Game.Rust;
using ConVar;
using Oxide.Ext.Discord.Builders;
using Oxide.Ext.Discord.Clients;
using Oxide.Ext.Discord.Connections;
using Oxide.Ext.Discord.Interfaces;

Oxide.Plugins
[Info("AAlertRaid", "Raid", "1.0.0")]
public class AAlertRaid : RustPlugin, IDiscordPlugin
{
    const bool fermensEN;
    private static PluginConfig config;
    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    protected override void SaveConfig();
     class VK
    {
        [JsonProperty(fermensEN ? "Enable?" : "Включить?")]
        public bool enable;
        [JsonProperty(fermensEN ? "API" : "API от группы")]
        public string api;
        [JsonProperty(fermensEN ? "Link to the group" : "Ссылка на группу")]
        public string link;
        [JsonProperty(fermensEN ? "Cooldown for sending" : "Кд на отправку")]
        public float cooldown;
    }

     class RUSTPLUS
    {
        [JsonProperty(fermensEN ? "Enable?" : "Включить?")]
        public bool enable;
        [JsonProperty(fermensEN ? "Cooldown for sending" : "Кд на отправку")]
        public float cooldown;
    }

     class INGAME
    {
        [JsonProperty(fermensEN ? "Enable?" : "Включить?")]
        public bool enable;
        [JsonProperty(fermensEN ? "Cooldown for sending" : "Кд на отправку")]
        public float cooldown;
        [JsonProperty(fermensEN ? "Send game effect when notification are received" : "Эффект при получении уведомления")]
        public string effect;
        [JsonProperty(fermensEN ? "Time after the UI is destroyed" : "Время, через которое пропадает UI [секунды]")]
        public float destroy;
        [JsonProperty("UI")]
        public string UI;
    }

     class DISCORD
    {
        [JsonProperty(fermensEN ? "Enable?" : "Включить?")]
        public bool enable;
        [JsonProperty(fermensEN ? "Cooldown for sending" : "Кд на отправку")]
        public float cooldown;
        [JsonProperty(fermensEN ? "Invitation link" : "Приглашение в группу")]
        public string link;
        [JsonProperty(fermensEN ? "Token (https://discordapp.com/developers/applications)" : "Токен бота (https://discordapp.com/developers/applications)")]
        public string token;
        [JsonProperty(fermensEN ? "Channel ID, where the player will take the code to confirm the profile" : "ID канала, гле игрок будет брать код, для подтверджения профиля")]
        public string channel;
        [JsonProperty(fermensEN ? "Info text" : "Дискорд канал с получением кода - текст")]
        public string channeltext;
        [JsonProperty(fermensEN ? "Info text - line color on the left" : "Дискорд канал с получением кода - цвет линии слева (https://gist.github.com/thomasbnt/b6f455e2c7d743b796917fa3c205f812#file-code_colors_discordjs-md)")]
        public uint channelcolor;
        [JsonProperty(fermensEN ? "Text on button" : "Дискорд канал с получением кода - кнопка")]
        public string channelbutton;
        [JsonProperty(fermensEN ? "Reply after button click" : "Дискорд канал с получением кода - ответ")]
        public string channelex;
        [JsonProperty(fermensEN ? "Don't touch this field" : "Дискорд канал с получением кода - ID сообщения (не трогаем! сам заполнится!)")]
        public string channelmessageid;
    }

     class TELEGRAM
    {
        [JsonProperty(fermensEN ? "Enable?" : "Включить?")]
        public bool enable;
        [JsonProperty(fermensEN ? "Cooldown for sending" : "Кд на отправку")]
        public float cooldown;
        [JsonProperty(fermensEN ? "Bot tag" : "Тэг бота")]
        public string bottag;
        [JsonProperty(fermensEN ? "Token" : "Токен")]
        public string token;
    }

     class UIMenu
    {
        [JsonProperty(fermensEN ? "Background color" : "Цвет фона")]
        public string background;
        [JsonProperty(fermensEN ? "Strip color" : "Цвет полоски")]
        public string stripcolor;
        [JsonProperty(fermensEN ? "Rectangular container background color" : "Цвет фона прямоугольного контейнера")]
        public string rectangularcolor;
        [JsonProperty(fermensEN ? "Button text color" : "Цвет текста в кнопке")]
        public string buttoncolortext;
        [JsonProperty(fermensEN ? "Text color" : "Цвет текста")]
        public string textcolor;
        [JsonProperty(fermensEN ? "Green button color" : "Цвет зелёной кнопки")]
        public string greenbuttoncolor;
        [JsonProperty(fermensEN ? "Red button color" : "Цвет красной кнопки")]
        public string redbuttoncolor;
        [JsonProperty(fermensEN ? "Gray button color" : "Цвет серой кнопки")]
        public string graybuttoncolor;
        [JsonProperty(fermensEN ? "Header text color" : "Цвет текста заголовка")]
        public string headertextcolor;
        [JsonProperty(fermensEN ? "Error text color" : "Цвет текста ошибки")]
        public string errortextcolor;
        [JsonProperty(fermensEN ? "Text color of <exit> and <back> buttons" : "Цвет текста кнопок <выход> и <назад>")]
        public string colortextexit;
        [JsonProperty(fermensEN ? "Rectangular container text color" : "Цвет текст прямоугольного контейнера")]
        public string rectangulartextcolor;
        [JsonProperty(fermensEN ? "The color of the text with hints at the bottom of the screen" : "Цвет текста с подсказками внизу экрана")]
        public string hintstextcolor;
        [JsonProperty(fermensEN ? "Abbreviations and their colors" : "Аббревиатуры и их цвета")]
        public UIMainMenu uIMainMenu;
    }

     class UIMainMenu
    {
        [JsonProperty(fermensEN ? "Abbreviation for telegram" : "Аббревиатура для телеграма")]
        public string abr_telegram;
        [JsonProperty(fermensEN ? "Telegram icon color" : "Цвет иконки телеграма")]
        public string color_telegram;
        [JsonProperty(fermensEN ? "Abbreviation for vk.com" : "Аббревиатура для вконтакте")]
        public string abr_vk;
        [JsonProperty(fermensEN ? "Vk.com icon color" : "Цвет иконки вконтакте")]
        public string color_vk;
        [JsonProperty(fermensEN ? "Abbreviation for rust+" : "Аббревиатура для rust+")]
        public string abr_rustplus;
        [JsonProperty(fermensEN ? "Rust+ icon color" : "Цвет иконки rust+")]
        public string color_rustplus;
        [JsonProperty(fermensEN ? "Abbreviation for discord" : "Аббревиатура для дискорда")]
        public string abr_discord;
        [JsonProperty(fermensEN ? "Discord icon color" : "Цвет иконки дискорда")]
        public string color_discord;
        [JsonProperty(fermensEN ? "Abbreviation for in game" : "Аббревиатура для графическое отображение в игре")]
        public string abr_ui;
        [JsonProperty(fermensEN ? "In game icon color" : "Цвет иконки графическое отображение в игре")]
        public string color_ui;
    }

    private class PluginConfig
    {
        [JsonProperty(fermensEN ? "Server name, will using for alerts" : "Название сервера - для оповещений")]
        public string servername;
        [JsonProperty(fermensEN ? "Raid alert works only for those who have permission" : "Оповещение о рейде работает только для тех, у кого есть разрешение")]
        public bool needpermission;
        [JsonProperty(fermensEN ? "VK.com" : "Оповещание о рейде в ВК")]
        public VK vk;
        [JsonProperty(fermensEN ? "Rust+" : "Оповещание о рейде в Rust+")]
        public RUSTPLUS rustplus;
        [JsonProperty(fermensEN ? "In game" : "Оповещание о рейде в игре")]
        public INGAME ingame;
        [JsonProperty(fermensEN ? "Discord" : "Оповещание о рейде в дискорд")]
        public DISCORD discord;
        [JsonProperty(fermensEN ? "Telegram" : "Оповещание о рейде в телеграм")]
        public TELEGRAM telegram { get; set; }
        [JsonProperty(fermensEN ? "Menu UI" : "Настройка UI")]
        public UIMenu ui { get; set; }
        [JsonProperty(fermensEN ? "Additional list" : "Дополнительный список предметов, которые учитывать")]
        public string[] spisok;
        [JsonProperty(fermensEN ? "Notification when usual items are destroyed" : "Оповещение при уничтожении обычных предметов")]
        public bool extralist;
        public static PluginConfig DefaultConfig();
    }

    private static string[] _spisok;
    private readonly BotConnection _discordSettings;
    private DiscordGuild _guild;
    public DiscordClient Client { get; set; }
    private void CreateClient();
    private void OnDiscordInteractionCreated(DiscordInteraction interaction);
    private void HandleAcceptLinkButton(DiscordInteraction interaction, DiscordUser user);
    private void OnDiscordGatewayReady(GatewayReadyEvent ready);
    private void CloseClient();
    private void CREATECHANNEL(string dsid, string text);
    private void SENDMESSAGE(string dsid, string text);
    public List<ActionRowComponent> CreateComponents(string button);
    private readonly List<Regex> _regexTags;
    private readonly List<string> _tags;
    private string STRIP(string original);
    private DiscordChannel GetChannel(string id);
     string connect;
     string FON;
     string MAIN;
     string UI;
     string IF2;
     string IF2A;
     string BTN;
     string ER;
     string IBLOCK;
     string MAINH;
     string AG;
     string EXIT;
     string BACK;
     class Storage
    {
        public string vk;
        public string telegram;
        public ulong discord;
        public bool rustplus;
        public bool ingamerust { get; set; }
    }

    private Storage GetStorage(ulong userid);
    private void SaveStorage(BasePlayer player);
    private IEnumerator Saving(string userid, Storage storage);
     Dictionary<ulong, Storage> datas;
    private void GetRequestTelegram(string reciverID, string msg, BasePlayer player, bool accept);
    private IEnumerator GetCallbackTelegram(int code, string response, string id, BasePlayer player, bool accept);
    const string connects;
     class ALERT
    {
        public DateTime gamecooldown;
        public DateTime rustpluscooldown;
        public DateTime vkcooldown;
        public DateTime discordcooldown;
        public DateTime vkcodecooldown;
        public DateTime telegramcooldown;
        public DateTime telegramcodecooldown;
    }

    private static Dictionary<ulong, ALERT> alerts;
     class CODE
    {
        public string id;
        public ulong gameid;
    }

    private Dictionary<string, CODE> VKCODES;
    private Dictionary<ulong, string> DISCORDCODES;
    private void GetRequest(string reciverID, string msg, BasePlayer player, string num);
    private void SendError(BasePlayer player, string key);
    private IEnumerator GetCallbackVK(int code, string response, string id, BasePlayer player, string num);
    private string perm;
    [PluginReference]
     Plugin BMenu;
    private void callcommandrn(BasePlayer player, string command, string[] arg);
    private bool HasAcces(string id);
    private void OpenMenu(BasePlayer player, bool first);
     class C
    {
        public string min;
        public string max;
    }

     Dictionary<int, C> _caddele;
    private void AddElementUI(BasePlayer player, string name, string color, string button, string command, string ico, string icocolor, int num);
     Dictionary<ulong, string> write;
    [ConsoleCommand("raid.input")]
     void ccmdopeinput(ConsoleSystem.Arg arg);
    private void SendError2(BasePlayer player, string key);
    [ConsoleCommand("raid.ingame")]
     void raplsgame(ConsoleSystem.Arg arg);
    [ConsoleCommand("raid.rustplus")]
     void rapls(ConsoleSystem.Arg arg);
    [ConsoleCommand("raid.discordadd")]
     void ccmdadiscoradd(ConsoleSystem.Arg arg);
    [ConsoleCommand("raid.acceptds")]
     void raidacceptds(ConsoleSystem.Arg arg);
    [ConsoleCommand("raid.discorddelete")]
     void vdiscorddelete(ConsoleSystem.Arg arg);
    [ConsoleCommand("raid.tgdelete")]
     void rgdelete(ConsoleSystem.Arg arg);
    [ConsoleCommand("raid.tgadd")]
     void ccmdtgadd(ConsoleSystem.Arg arg);
    [ConsoleCommand("raid.accepttg")]
     void ccmdaccepttg(ConsoleSystem.Arg arg);
    [ConsoleCommand("raid.vkdelete")]
     void vkdelete(ConsoleSystem.Arg arg);
    [ConsoleCommand("raid.vkadd")]
     void ccmdavkadd(ConsoleSystem.Arg arg);
    [ConsoleCommand("raid.accept")]
     void ccmdaccept(ConsoleSystem.Arg arg);
    [ConsoleCommand("raid.send")]
     void ccmdopesendt(ConsoleSystem.Arg arg);
    private string RANDOMNUM();
    private void Unload();
    private void OnServerInitialized();
    public Dictionary<string, string> messagesEN;
    public Dictionary<string, string> messagesRU;
     IEnumerator GetCallback();
    private void OnEntityDeath(BaseCombatEntity entity, HitInfo info);
    private IEnumerator Alerting(BaseCombatEntity entity, BasePlayer player, int tt);
     List<ulong> block;
    private void ALERTPLAYER(ulong ID, string name, string quad, string connect, string destroy, string attackerid);
    private Dictionary<ulong, Timer> timal;
    private string GetMessage(string key, string userId);
    private static Dictionary<string, Vector3> Grids;
    private void CreateSpawnGrid();
    private string GetNameGrid(Vector3 pos);
}

 class VK
{
    [JsonProperty(fermensEN ? "Enable?" : "Включить?")]
    public bool enable;
    [JsonProperty(fermensEN ? "API" : "API от группы")]
    public string api;
    [JsonProperty(fermensEN ? "Link to the group" : "Ссылка на группу")]
    public string link;
    [JsonProperty(fermensEN ? "Cooldown for sending" : "Кд на отправку")]
    public float cooldown;
}

 class RUSTPLUS
{
    [JsonProperty(fermensEN ? "Enable?" : "Включить?")]
    public bool enable;
    [JsonProperty(fermensEN ? "Cooldown for sending" : "Кд на отправку")]
    public float cooldown;
}

 class INGAME
{
    [JsonProperty(fermensEN ? "Enable?" : "Включить?")]
    public bool enable;
    [JsonProperty(fermensEN ? "Cooldown for sending" : "Кд на отправку")]
    public float cooldown;
    [JsonProperty(fermensEN ? "Send game effect when notification are received" : "Эффект при получении уведомления")]
    public string effect;
    [JsonProperty(fermensEN ? "Time after the UI is destroyed" : "Время, через которое пропадает UI [секунды]")]
    public float destroy;
    [JsonProperty("UI")]
    public string UI;
}

 class DISCORD
{
    [JsonProperty(fermensEN ? "Enable?" : "Включить?")]
    public bool enable;
    [JsonProperty(fermensEN ? "Cooldown for sending" : "Кд на отправку")]
    public float cooldown;
    [JsonProperty(fermensEN ? "Invitation link" : "Приглашение в группу")]
    public string link;
    [JsonProperty(fermensEN ? "Token (https://discordapp.com/developers/applications)" : "Токен бота (https://discordapp.com/developers/applications)")]
    public string token;
    [JsonProperty(fermensEN ? "Channel ID, where the player will take the code to confirm the profile" : "ID канала, гле игрок будет брать код, для подтверджения профиля")]
    public string channel;
    [JsonProperty(fermensEN ? "Info text" : "Дискорд канал с получением кода - текст")]
    public string channeltext;
    [JsonProperty(fermensEN ? "Info text - line color on the left" : "Дискорд канал с получением кода - цвет линии слева (https://gist.github.com/thomasbnt/b6f455e2c7d743b796917fa3c205f812#file-code_colors_discordjs-md)")]
    public uint channelcolor;
    [JsonProperty(fermensEN ? "Text on button" : "Дискорд канал с получением кода - кнопка")]
    public string channelbutton;
    [JsonProperty(fermensEN ? "Reply after button click" : "Дискорд канал с получением кода - ответ")]
    public string channelex;
    [JsonProperty(fermensEN ? "Don't touch this field" : "Дискорд канал с получением кода - ID сообщения (не трогаем! сам заполнится!)")]
    public string channelmessageid;
}

 class TELEGRAM
{
    [JsonProperty(fermensEN ? "Enable?" : "Включить?")]
    public bool enable;
    [JsonProperty(fermensEN ? "Cooldown for sending" : "Кд на отправку")]
    public float cooldown;
    [JsonProperty(fermensEN ? "Bot tag" : "Тэг бота")]
    public string bottag;
    [JsonProperty(fermensEN ? "Token" : "Токен")]
    public string token;
}

 class UIMenu
{
    [JsonProperty(fermensEN ? "Background color" : "Цвет фона")]
    public string background;
    [JsonProperty(fermensEN ? "Strip color" : "Цвет полоски")]
    public string stripcolor;
    [JsonProperty(fermensEN ? "Rectangular container background color" : "Цвет фона прямоугольного контейнера")]
    public string rectangularcolor;
    [JsonProperty(fermensEN ? "Button text color" : "Цвет текста в кнопке")]
    public string buttoncolortext;
    [JsonProperty(fermensEN ? "Text color" : "Цвет текста")]
    public string textcolor;
    [JsonProperty(fermensEN ? "Green button color" : "Цвет зелёной кнопки")]
    public string greenbuttoncolor;
    [JsonProperty(fermensEN ? "Red button color" : "Цвет красной кнопки")]
    public string redbuttoncolor;
    [JsonProperty(fermensEN ? "Gray button color" : "Цвет серой кнопки")]
    public string graybuttoncolor;
    [JsonProperty(fermensEN ? "Header text color" : "Цвет текста заголовка")]
    public string headertextcolor;
    [JsonProperty(fermensEN ? "Error text color" : "Цвет текста ошибки")]
    public string errortextcolor;
    [JsonProperty(fermensEN ? "Text color of <exit> and <back> buttons" : "Цвет текста кнопок <выход> и <назад>")]
    public string colortextexit;
    [JsonProperty(fermensEN ? "Rectangular container text color" : "Цвет текст прямоугольного контейнера")]
    public string rectangulartextcolor;
    [JsonProperty(fermensEN ? "The color of the text with hints at the bottom of the screen" : "Цвет текста с подсказками внизу экрана")]
    public string hintstextcolor;
    [JsonProperty(fermensEN ? "Abbreviations and their colors" : "Аббревиатуры и их цвета")]
    public UIMainMenu uIMainMenu;
}

 class UIMainMenu
{
    [JsonProperty(fermensEN ? "Abbreviation for telegram" : "Аббревиатура для телеграма")]
    public string abr_telegram;
    [JsonProperty(fermensEN ? "Telegram icon color" : "Цвет иконки телеграма")]
    public string color_telegram;
    [JsonProperty(fermensEN ? "Abbreviation for vk.com" : "Аббревиатура для вконтакте")]
    public string abr_vk;
    [JsonProperty(fermensEN ? "Vk.com icon color" : "Цвет иконки вконтакте")]
    public string color_vk;
    [JsonProperty(fermensEN ? "Abbreviation for rust+" : "Аббревиатура для rust+")]
    public string abr_rustplus;
    [JsonProperty(fermensEN ? "Rust+ icon color" : "Цвет иконки rust+")]
    public string color_rustplus;
    [JsonProperty(fermensEN ? "Abbreviation for discord" : "Аббревиатура для дискорда")]
    public string abr_discord;
    [JsonProperty(fermensEN ? "Discord icon color" : "Цвет иконки дискорда")]
    public string color_discord;
    [JsonProperty(fermensEN ? "Abbreviation for in game" : "Аббревиатура для графическое отображение в игре")]
    public string abr_ui;
    [JsonProperty(fermensEN ? "In game icon color" : "Цвет иконки графическое отображение в игре")]
    public string color_ui;
}

private class PluginConfig
{
    [JsonProperty(fermensEN ? "Server name, will using for alerts" : "Название сервера - для оповещений")]
    public string servername;
    [JsonProperty(fermensEN ? "Raid alert works only for those who have permission" : "Оповещение о рейде работает только для тех, у кого есть разрешение")]
    public bool needpermission;
    [JsonProperty(fermensEN ? "VK.com" : "Оповещание о рейде в ВК")]
    public VK vk;
    [JsonProperty(fermensEN ? "Rust+" : "Оповещание о рейде в Rust+")]
    public RUSTPLUS rustplus;
    [JsonProperty(fermensEN ? "In game" : "Оповещание о рейде в игре")]
    public INGAME ingame;
    [JsonProperty(fermensEN ? "Discord" : "Оповещание о рейде в дискорд")]
    public DISCORD discord;
    [JsonProperty(fermensEN ? "Telegram" : "Оповещание о рейде в телеграм")]
    public TELEGRAM telegram { get; set; }
    [JsonProperty(fermensEN ? "Menu UI" : "Настройка UI")]
    public UIMenu ui { get; set; }
    [JsonProperty(fermensEN ? "Additional list" : "Дополнительный список предметов, которые учитывать")]
    public string[] spisok;
    [JsonProperty(fermensEN ? "Notification when usual items are destroyed" : "Оповещение при уничтожении обычных предметов")]
    public bool extralist;
    public static PluginConfig DefaultConfig();
}

 class Storage
{
    public string vk;
    public string telegram;
    public ulong discord;
    public bool rustplus;
    public bool ingamerust { get; set; }
}

 class ALERT
{
    public DateTime gamecooldown;
    public DateTime rustpluscooldown;
    public DateTime vkcooldown;
    public DateTime discordcooldown;
    public DateTime vkcodecooldown;
    public DateTime telegramcooldown;
    public DateTime telegramcodecooldown;
}

 class CODE
{
    public string id;
    public ulong gameid;
}

 class C
{
    public string min;
    public string max;
}


```

---

## AAlertRaidEN

```csharp
using System;
using System.Collections;
using System.Collections.Generic;
using System.Linq;
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Plugins;
using UnityEngine;
using CompanionServer;
using Oxide.Ext.Discord;
using Oxide.Ext.Discord.Attributes;
using Oxide.Ext.Discord.Entities.Messages;
using Oxide.Ext.Discord.Entities.Channels;
using Oxide.Ext.Discord.Entities.Guilds;
using Oxide.Ext.Discord.Entities.Gatway;
using Oxide.Ext.Discord.Entities.Gatway.Events;
using Oxide.Ext.Discord.Entities.Messages.Embeds;
using Oxide.Ext.Discord.Entities.Permissions;
using Oxide.Ext.Discord.Entities;
using System.Text.RegularExpressions;
using Oxide.Ext.Discord.Builders.MessageComponents;
using Oxide.Ext.Discord.Entities.Interactions.MessageComponents;
using Oxide.Ext.Discord.Entities.Interactions;
using Oxide.Ext.Discord.Entities.Users;
using Oxide.Core.Libraries.Covalence;
using ru = Oxide.Game.Rust;
using ConVar;

Oxide.Plugins
[Info("AAlertRaidEN", "shoprust.ru", "1.0.0")]
public class AAlertRaidEN : RustPlugin
{
    const bool fermensEN;
    private static PluginConfig config;
    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    protected override void SaveConfig();
     class VK
    {
        [JsonProperty(fermensEN ? "Enable?" : "Включить?")]
        public bool enable;
        [JsonProperty(fermensEN ? "API" : "API от группы")]
        public string api;
        [JsonProperty(fermensEN ? "Link to the group" : "Ссылка на группу")]
        public string link;
        [JsonProperty(fermensEN ? "Cooldown for sending" : "Кд на отправку")]
        public float cooldown;
    }

     class RUSTPLUS
    {
        [JsonProperty(fermensEN ? "Enable?" : "Включить?")]
        public bool enable;
        [JsonProperty(fermensEN ? "Cooldown for sending" : "Кд на отправку")]
        public float cooldown;
    }

     class INGAME
    {
        [JsonProperty(fermensEN ? "Enable?" : "Включить?")]
        public bool enable;
        [JsonProperty(fermensEN ? "Cooldown for sending" : "Кд на отправку")]
        public float cooldown;
        [JsonProperty(fermensEN ? "Send game effect when notification are received" : "Эффект при получении уведомления")]
        public string effect;
        [JsonProperty(fermensEN ? "Time after the UI is destroyed" : "Время, через которое пропадает UI [секунды]")]
        public float destroy;
        [JsonProperty("UI")]
        public string UI;
    }

     class DISCORD
    {
        [JsonProperty(fermensEN ? "Enable?" : "Включить?")]
        public bool enable;
        [JsonProperty(fermensEN ? "Cooldown for sending" : "Кд на отправку")]
        public float cooldown;
        [JsonProperty(fermensEN ? "Invitation link" : "Приглашение в группу")]
        public string link;
        [JsonProperty(fermensEN ? "Token (https://discordapp.com/developers/applications)" : "Токен бота (https://discordapp.com/developers/applications)")]
        public string token;
        [JsonProperty(fermensEN ? "Channel ID, where the player will take the code to confirm the profile" : "ID канала, гле игрок будет брать код, для подтверджения профиля")]
        public string channel;
        [JsonProperty(fermensEN ? "Info text" : "Дискорд канал с получением кода - текст")]
        public string channeltext;
        [JsonProperty(fermensEN ? "Info text - line color on the left" : "Дискорд канал с получением кода - цвет линии слева (https://gist.github.com/thomasbnt/b6f455e2c7d743b796917fa3c205f812#file-code_colors_discordjs-md)")]
        public uint channelcolor;
        [JsonProperty(fermensEN ? "Text on button" : "Дискорд канал с получением кода - кнопка")]
        public string channelbutton;
        [JsonProperty(fermensEN ? "Reply after button click" : "Дискорд канал с получением кода - ответ")]
        public string channelex;
        [JsonProperty(fermensEN ? "Don't touch this field" : "Дискорд канал с получением кода - ID сообщения (не трогаем! сам заполнится!)")]
        public string channelmessageid;
    }

     class TELEGRAM
    {
        [JsonProperty(fermensEN ? "Enable?" : "Включить?")]
        public bool enable;
        [JsonProperty(fermensEN ? "Cooldown for sending" : "Кд на отправку")]
        public float cooldown;
        [JsonProperty(fermensEN ? "Bot tag" : "Тэг бота")]
        public string bottag;
        [JsonProperty(fermensEN ? "Token" : "Токен")]
        public string token;
    }

     class UIMenu
    {
        [JsonProperty(fermensEN ? "Background color" : "Цвет фона")]
        public string background;
        [JsonProperty(fermensEN ? "Strip color" : "Цвет полоски")]
        public string stripcolor;
        [JsonProperty(fermensEN ? "Rectangular container background color" : "Цвет фона прямоугольного контейнера")]
        public string rectangularcolor;
        [JsonProperty(fermensEN ? "Button text color" : "Цвет текста в кнопке")]
        public string buttoncolortext;
        [JsonProperty(fermensEN ? "Text color" : "Цвет текста")]
        public string textcolor;
        [JsonProperty(fermensEN ? "Green button color" : "Цвет зелёной кнопки")]
        public string greenbuttoncolor;
        [JsonProperty(fermensEN ? "Red button color" : "Цвет красной кнопки")]
        public string redbuttoncolor;
        [JsonProperty(fermensEN ? "Gray button color" : "Цвет серой кнопки")]
        public string graybuttoncolor;
        [JsonProperty(fermensEN ? "Header text color" : "Цвет текста заголовка")]
        public string headertextcolor;
        [JsonProperty(fermensEN ? "Error text color" : "Цвет текста ошибки")]
        public string errortextcolor;
        [JsonProperty(fermensEN ? "Text color of <exit> and <back> buttons" : "Цвет текста кнопок <выход> и <назад>")]
        public string colortextexit;
        [JsonProperty(fermensEN ? "Rectangular container text color" : "Цвет текст прямоугольного контейнера")]
        public string rectangulartextcolor;
        [JsonProperty(fermensEN ? "The color of the text with hints at the bottom of the screen" : "Цвет текста с подсказками внизу экрана")]
        public string hintstextcolor;
        [JsonProperty(fermensEN ? "Abbreviations and their colors" : "Аббревиатуры и их цвета")]
        public UIMainMenu uIMainMenu;
    }

     class UIMainMenu
    {
        [JsonProperty(fermensEN ? "Abbreviation for telegram" : "Аббревиатура для телеграма")]
        public string abr_telegram;
        [JsonProperty(fermensEN ? "Telegram icon color" : "Цвет иконки телеграма")]
        public string color_telegram;
        [JsonProperty(fermensEN ? "Abbreviation for vk.com" : "Аббревиатура для вконтакте")]
        public string abr_vk;
        [JsonProperty(fermensEN ? "Vk.com icon color" : "Цвет иконки вконтакте")]
        public string color_vk;
        [JsonProperty(fermensEN ? "Abbreviation for rust+" : "Аббревиатура для rust+")]
        public string abr_rustplus;
        [JsonProperty(fermensEN ? "Rust+ icon color" : "Цвет иконки rust+")]
        public string color_rustplus;
        [JsonProperty(fermensEN ? "Abbreviation for discord" : "Аббревиатура для дискорда")]
        public string abr_discord;
        [JsonProperty(fermensEN ? "Discord icon color" : "Цвет иконки дискорда")]
        public string color_discord;
        [JsonProperty(fermensEN ? "Abbreviation for in game" : "Аббревиатура для графическое отображение в игре")]
        public string abr_ui;
        [JsonProperty(fermensEN ? "In game icon color" : "Цвет иконки графическое отображение в игре")]
        public string color_ui;
    }

    private class PluginConfig
    {
        [JsonProperty(fermensEN ? "Server name, will using for alerts" : "Название сервера - для оповещений")]
        public string servername;
        [JsonProperty(fermensEN ? "Raid alert works only for those who have permission" : "Оповещение о рейде работает только для тех, у кого есть разрешение")]
        public bool needpermission;
        [JsonProperty(fermensEN ? "VK.com" : "Оповещание о рейде в ВК")]
        public VK vk;
        [JsonProperty(fermensEN ? "Rust+" : "Оповещание о рейде в Rust+")]
        public RUSTPLUS rustplus;
        [JsonProperty(fermensEN ? "In game" : "Оповещание о рейде в игре")]
        public INGAME ingame;
        [JsonProperty(fermensEN ? "Discord" : "Оповещание о рейде в дискорд")]
        public DISCORD discord;
        [JsonProperty(fermensEN ? "Telegram" : "Оповещание о рейде в телеграм")]
        public TELEGRAM telegram { get; set; }
        [JsonProperty(fermensEN ? "Menu UI" : "Настройка UI")]
        public UIMenu ui { get; set; }
        [JsonProperty(fermensEN ? "Additional list" : "Дополнительный список предметов, которые учитывать")]
        public string[] spisok;
        [JsonProperty(fermensEN ? "Notification when usual items are destroyed" : "Оповещение при уничтожении обычных предметов")]
        public bool extralist;
        public static PluginConfig DefaultConfig();
    }

    private static string[] _spisok;
    private readonly DiscordSettings _discordSettings;
    private DiscordGuild _guild;
    [DiscordClient]
     DiscordClient Client;
    private void CreateClient();
    private void OnDiscordInteractionCreated(DiscordInteraction interaction);
    private void HandleAcceptLinkButton(DiscordInteraction interaction, DiscordUser user);
    private void OnDiscordGatewayReady(GatewayReadyEvent ready);
    private void CloseClient();
    private void CREATECHANNEL(string dsid, string text);
    private void SENDMESSAGE(string dsid, string text);
    public List<ActionRowComponent> CreateComponents(string button);
    private readonly List<Regex> _regexTags;
    private readonly List<string> _tags;
    private string STRIP(string original);
    private DiscordChannel GetChannel(string id);
     string connect;
     string FON;
     string MAIN;
     string UI;
     string IF2;
     string IF2A;
     string BTN;
     string ER;
     string IBLOCK;
     string MAINH;
     string AG;
     string EXIT;
     string BACK;
     class Storage
    {
        public string vk;
        public string telegram;
        public ulong discord;
        public bool rustplus;
        public bool ingamerust { get; set; }
    }

    private Storage GetStorage(ulong userid);
    private void SaveStorage(BasePlayer player);
    private IEnumerator Saving(string userid, Storage storage);
     Dictionary<ulong, Storage> datas;
    private void GetRequestTelegram(string reciverID, string msg, BasePlayer player, bool accept);
    private IEnumerator GetCallbackTelegram(int code, string response, string id, BasePlayer player, bool accept);
    const string connects;
     class ALERT
    {
        public DateTime gamecooldown;
        public DateTime rustpluscooldown;
        public DateTime vkcooldown;
        public DateTime discordcooldown;
        public DateTime vkcodecooldown;
        public DateTime telegramcooldown;
        public DateTime telegramcodecooldown;
    }

    private static Dictionary<ulong, ALERT> alerts;
     class CODE
    {
        public string id;
        public ulong gameid;
    }

    private Dictionary<string, CODE> VKCODES;
    private Dictionary<ulong, string> DISCORDCODES;
    private void GetRequest(string reciverID, string msg, BasePlayer player, string num);
    private void SendError(BasePlayer player, string key);
    private IEnumerator GetCallbackVK(int code, string response, string id, BasePlayer player, string num);
    private string perm;
    [PluginReference]
     Plugin BMenu;
    private void callcommandrn(BasePlayer player, string command, string[] arg);
    private bool HasAcces(string id);
    private void OpenMenu(BasePlayer player, bool first);
     class C
    {
        public string min;
        public string max;
    }

     Dictionary<int, C> _caddele;
    private void AddElementUI(BasePlayer player, string name, string color, string button, string command, string ico, string icocolor, int num);
     Dictionary<ulong, string> write;
    [ConsoleCommand("raid.input")]
     void ccmdopeinput(ConsoleSystem.Arg arg);
    private void SendError2(BasePlayer player, string key);
    [ConsoleCommand("raid.ingame")]
     void raplsgame(ConsoleSystem.Arg arg);
    [ConsoleCommand("raid.rustplus")]
     void rapls(ConsoleSystem.Arg arg);
    [ConsoleCommand("raid.discordadd")]
     void ccmdadiscoradd(ConsoleSystem.Arg arg);
    [ConsoleCommand("raid.acceptds")]
     void raidacceptds(ConsoleSystem.Arg arg);
    [ConsoleCommand("raid.discorddelete")]
     void vdiscorddelete(ConsoleSystem.Arg arg);
    [ConsoleCommand("raid.tgdelete")]
     void rgdelete(ConsoleSystem.Arg arg);
    [ConsoleCommand("raid.tgadd")]
     void ccmdtgadd(ConsoleSystem.Arg arg);
    [ConsoleCommand("raid.accepttg")]
     void ccmdaccepttg(ConsoleSystem.Arg arg);
    [ConsoleCommand("raid.vkdelete")]
     void vkdelete(ConsoleSystem.Arg arg);
    [ConsoleCommand("raid.vkadd")]
     void ccmdavkadd(ConsoleSystem.Arg arg);
    [ConsoleCommand("raid.accept")]
     void ccmdaccept(ConsoleSystem.Arg arg);
    [ConsoleCommand("raid.send")]
     void ccmdopesendt(ConsoleSystem.Arg arg);
    private string RANDOMNUM();
    private void Unload();
    private void OnServerInitialized();
    public Dictionary<string, string> messagesEN;
    public Dictionary<string, string> messagesRU;
     IEnumerator GetCallback();
    private void OnEntityDeath(BaseCombatEntity entity, HitInfo info);
    private IEnumerator Alerting(BaseCombatEntity entity, BasePlayer player, int tt);
     List<ulong> block;
    private void ALERTPLAYER(ulong ID, string name, string quad, string connect, string destroy, string attackerid);
    private Dictionary<ulong, Timer> timal;
    private string GetMessage(string key, string userId);
    private static Dictionary<string, Vector3> Grids;
    private void CreateSpawnGrid();
    private string GetNameGrid(Vector3 pos);
}

 class VK
{
    [JsonProperty(fermensEN ? "Enable?" : "Включить?")]
    public bool enable;
    [JsonProperty(fermensEN ? "API" : "API от группы")]
    public string api;
    [JsonProperty(fermensEN ? "Link to the group" : "Ссылка на группу")]
    public string link;
    [JsonProperty(fermensEN ? "Cooldown for sending" : "Кд на отправку")]
    public float cooldown;
}

 class RUSTPLUS
{
    [JsonProperty(fermensEN ? "Enable?" : "Включить?")]
    public bool enable;
    [JsonProperty(fermensEN ? "Cooldown for sending" : "Кд на отправку")]
    public float cooldown;
}

 class INGAME
{
    [JsonProperty(fermensEN ? "Enable?" : "Включить?")]
    public bool enable;
    [JsonProperty(fermensEN ? "Cooldown for sending" : "Кд на отправку")]
    public float cooldown;
    [JsonProperty(fermensEN ? "Send game effect when notification are received" : "Эффект при получении уведомления")]
    public string effect;
    [JsonProperty(fermensEN ? "Time after the UI is destroyed" : "Время, через которое пропадает UI [секунды]")]
    public float destroy;
    [JsonProperty("UI")]
    public string UI;
}

 class DISCORD
{
    [JsonProperty(fermensEN ? "Enable?" : "Включить?")]
    public bool enable;
    [JsonProperty(fermensEN ? "Cooldown for sending" : "Кд на отправку")]
    public float cooldown;
    [JsonProperty(fermensEN ? "Invitation link" : "Приглашение в группу")]
    public string link;
    [JsonProperty(fermensEN ? "Token (https://discordapp.com/developers/applications)" : "Токен бота (https://discordapp.com/developers/applications)")]
    public string token;
    [JsonProperty(fermensEN ? "Channel ID, where the player will take the code to confirm the profile" : "ID канала, гле игрок будет брать код, для подтверджения профиля")]
    public string channel;
    [JsonProperty(fermensEN ? "Info text" : "Дискорд канал с получением кода - текст")]
    public string channeltext;
    [JsonProperty(fermensEN ? "Info text - line color on the left" : "Дискорд канал с получением кода - цвет линии слева (https://gist.github.com/thomasbnt/b6f455e2c7d743b796917fa3c205f812#file-code_colors_discordjs-md)")]
    public uint channelcolor;
    [JsonProperty(fermensEN ? "Text on button" : "Дискорд канал с получением кода - кнопка")]
    public string channelbutton;
    [JsonProperty(fermensEN ? "Reply after button click" : "Дискорд канал с получением кода - ответ")]
    public string channelex;
    [JsonProperty(fermensEN ? "Don't touch this field" : "Дискорд канал с получением кода - ID сообщения (не трогаем! сам заполнится!)")]
    public string channelmessageid;
}

 class TELEGRAM
{
    [JsonProperty(fermensEN ? "Enable?" : "Включить?")]
    public bool enable;
    [JsonProperty(fermensEN ? "Cooldown for sending" : "Кд на отправку")]
    public float cooldown;
    [JsonProperty(fermensEN ? "Bot tag" : "Тэг бота")]
    public string bottag;
    [JsonProperty(fermensEN ? "Token" : "Токен")]
    public string token;
}

 class UIMenu
{
    [JsonProperty(fermensEN ? "Background color" : "Цвет фона")]
    public string background;
    [JsonProperty(fermensEN ? "Strip color" : "Цвет полоски")]
    public string stripcolor;
    [JsonProperty(fermensEN ? "Rectangular container background color" : "Цвет фона прямоугольного контейнера")]
    public string rectangularcolor;
    [JsonProperty(fermensEN ? "Button text color" : "Цвет текста в кнопке")]
    public string buttoncolortext;
    [JsonProperty(fermensEN ? "Text color" : "Цвет текста")]
    public string textcolor;
    [JsonProperty(fermensEN ? "Green button color" : "Цвет зелёной кнопки")]
    public string greenbuttoncolor;
    [JsonProperty(fermensEN ? "Red button color" : "Цвет красной кнопки")]
    public string redbuttoncolor;
    [JsonProperty(fermensEN ? "Gray button color" : "Цвет серой кнопки")]
    public string graybuttoncolor;
    [JsonProperty(fermensEN ? "Header text color" : "Цвет текста заголовка")]
    public string headertextcolor;
    [JsonProperty(fermensEN ? "Error text color" : "Цвет текста ошибки")]
    public string errortextcolor;
    [JsonProperty(fermensEN ? "Text color of <exit> and <back> buttons" : "Цвет текста кнопок <выход> и <назад>")]
    public string colortextexit;
    [JsonProperty(fermensEN ? "Rectangular container text color" : "Цвет текст прямоугольного контейнера")]
    public string rectangulartextcolor;
    [JsonProperty(fermensEN ? "The color of the text with hints at the bottom of the screen" : "Цвет текста с подсказками внизу экрана")]
    public string hintstextcolor;
    [JsonProperty(fermensEN ? "Abbreviations and their colors" : "Аббревиатуры и их цвета")]
    public UIMainMenu uIMainMenu;
}

 class UIMainMenu
{
    [JsonProperty(fermensEN ? "Abbreviation for telegram" : "Аббревиатура для телеграма")]
    public string abr_telegram;
    [JsonProperty(fermensEN ? "Telegram icon color" : "Цвет иконки телеграма")]
    public string color_telegram;
    [JsonProperty(fermensEN ? "Abbreviation for vk.com" : "Аббревиатура для вконтакте")]
    public string abr_vk;
    [JsonProperty(fermensEN ? "Vk.com icon color" : "Цвет иконки вконтакте")]
    public string color_vk;
    [JsonProperty(fermensEN ? "Abbreviation for rust+" : "Аббревиатура для rust+")]
    public string abr_rustplus;
    [JsonProperty(fermensEN ? "Rust+ icon color" : "Цвет иконки rust+")]
    public string color_rustplus;
    [JsonProperty(fermensEN ? "Abbreviation for discord" : "Аббревиатура для дискорда")]
    public string abr_discord;
    [JsonProperty(fermensEN ? "Discord icon color" : "Цвет иконки дискорда")]
    public string color_discord;
    [JsonProperty(fermensEN ? "Abbreviation for in game" : "Аббревиатура для графическое отображение в игре")]
    public string abr_ui;
    [JsonProperty(fermensEN ? "In game icon color" : "Цвет иконки графическое отображение в игре")]
    public string color_ui;
}

private class PluginConfig
{
    [JsonProperty(fermensEN ? "Server name, will using for alerts" : "Название сервера - для оповещений")]
    public string servername;
    [JsonProperty(fermensEN ? "Raid alert works only for those who have permission" : "Оповещение о рейде работает только для тех, у кого есть разрешение")]
    public bool needpermission;
    [JsonProperty(fermensEN ? "VK.com" : "Оповещание о рейде в ВК")]
    public VK vk;
    [JsonProperty(fermensEN ? "Rust+" : "Оповещание о рейде в Rust+")]
    public RUSTPLUS rustplus;
    [JsonProperty(fermensEN ? "In game" : "Оповещание о рейде в игре")]
    public INGAME ingame;
    [JsonProperty(fermensEN ? "Discord" : "Оповещание о рейде в дискорд")]
    public DISCORD discord;
    [JsonProperty(fermensEN ? "Telegram" : "Оповещание о рейде в телеграм")]
    public TELEGRAM telegram { get; set; }
    [JsonProperty(fermensEN ? "Menu UI" : "Настройка UI")]
    public UIMenu ui { get; set; }
    [JsonProperty(fermensEN ? "Additional list" : "Дополнительный список предметов, которые учитывать")]
    public string[] spisok;
    [JsonProperty(fermensEN ? "Notification when usual items are destroyed" : "Оповещение при уничтожении обычных предметов")]
    public bool extralist;
    public static PluginConfig DefaultConfig();
}

 class Storage
{
    public string vk;
    public string telegram;
    public ulong discord;
    public bool rustplus;
    public bool ingamerust { get; set; }
}

 class ALERT
{
    public DateTime gamecooldown;
    public DateTime rustpluscooldown;
    public DateTime vkcooldown;
    public DateTime discordcooldown;
    public DateTime vkcodecooldown;
    public DateTime telegramcooldown;
    public DateTime telegramcodecooldown;
}

 class CODE
{
    public string id;
    public ulong gameid;
}

 class C
{
    public string min;
    public string max;
}


```

---

## AAlertRaidEN-2.0.1

```csharp
using System;
using System.Collections;
using System.Collections.Generic;
using System.Linq;
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Plugins;
using UnityEngine;
using CompanionServer;
using Oxide.Ext.Discord.Entities;
using System.Text.RegularExpressions;
using Oxide.Core.Libraries.Covalence;
using ru = Oxide.Game.Rust;
using ConVar;
using Oxide.Ext.Discord.Builders;
using Oxide.Ext.Discord.Clients;
using Oxide.Ext.Discord.Connections;
using Oxide.Ext.Discord.Constants;
using Oxide.Ext.Discord.Extensions;
using Oxide.Ext.Discord.Interfaces;
using Oxide.Ext.Discord.Libraries;
using Oxide.Ext.Discord.Logging;

Oxide.Plugins
[Info("AAlertRaidEN", "fermens", "2.0.1")]
public class AAlertRaidEN : RustPlugin, IDiscordPlugin
{
    const bool fermensEN;
    private static PluginConfig config;
    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    protected override void SaveConfig();
     class VK
    {
        [JsonProperty(fermensEN ? "Enable?" : "Включить?")]
        public bool enable;
        [JsonProperty(fermensEN ? "API" : "API от группы")]
        public string api;
        [JsonProperty(fermensEN ? "Link to the group" : "Ссылка на группу")]
        public string link;
        [JsonProperty(fermensEN ? "Cooldown for sending" : "Кд на отправку")]
        public float cooldown;
    }

     class RUSTPLUS
    {
        [JsonProperty(fermensEN ? "Enable?" : "Включить?")]
        public bool enable;
        [JsonProperty(fermensEN ? "Cooldown for sending" : "Кд на отправку")]
        public float cooldown;
    }

     class INGAME
    {
        [JsonProperty(fermensEN ? "Enable?" : "Включить?")]
        public bool enable;
        [JsonProperty(fermensEN ? "Cooldown for sending" : "Кд на отправку")]
        public float cooldown;
        [JsonProperty(fermensEN ? "Send game effect when notification are received" : "Эффект при получении уведомления")]
        public string effect;
        [JsonProperty(fermensEN ? "Time after the UI is destroyed" : "Время, через которое пропадает UI [секунды]")]
        public float destroy;
        [JsonProperty("UI")]
        public string UI;
    }

     class DISCORD
    {
        [JsonProperty(fermensEN ? "Enable?" : "Включить?")]
        public bool enable;
        [JsonProperty(fermensEN ? "Cooldown for sending" : "Кд на отправку")]
        public float cooldown;
        [JsonProperty(fermensEN ? "Invitation link" : "Приглашение в группу")]
        public string link;
        [JsonProperty(fermensEN ? "Token (https://discordapp.com/developers/applications)" : "Токен бота (https://discordapp.com/developers/applications)")]
        public string token;
        [JsonProperty(fermensEN ? "Channel ID, where the player will take the code to confirm the profile" : "ID канала, гле игрок будет брать код, для подтверджения профиля")]
        public string channel;
        [JsonProperty(fermensEN ? "Server ID" : "ID Сервера")]
        public ulong server;
        [JsonProperty(fermensEN ? "Info text" : "Дискорд канал с получением кода - текст")]
        public string channeltext;
        [JsonProperty(fermensEN ? "Info text - line color on the left" : "Дискорд канал с получением кода - цвет линии слева (https://gist.github.com/thomasbnt/b6f455e2c7d743b796917fa3c205f812#file-code_colors_discordjs-md)")]
        public uint channelcolor;
        [JsonProperty(fermensEN ? "Text on button" : "Дискорд канал с получением кода - кнопка")]
        public string channelbutton;
        [JsonProperty(fermensEN ? "Reply after button click" : "Дискорд канал с получением кода - ответ")]
        public string channelex;
        [JsonProperty(fermensEN ? "Don't touch this field" : "Дискорд канал с получением кода - ID сообщения (не трогаем! сам заполнится!)")]
        public string channelmessageid;
    }

     class TELEGRAM
    {
        [JsonProperty(fermensEN ? "Enable?" : "Включить?")]
        public bool enable;
        [JsonProperty(fermensEN ? "Cooldown for sending" : "Кд на отправку")]
        public float cooldown;
        [JsonProperty(fermensEN ? "Bot tag" : "Тэг бота")]
        public string bottag;
        [JsonProperty(fermensEN ? "Token" : "Токен")]
        public string token;
    }

     class UIMenu
    {
        [JsonProperty(fermensEN ? "Background color" : "Цвет фона")]
        public string background;
        [JsonProperty(fermensEN ? "Strip color" : "Цвет полоски")]
        public string stripcolor;
        [JsonProperty(fermensEN ? "Rectangular container background color" : "Цвет фона прямоугольного контейнера")]
        public string rectangularcolor;
        [JsonProperty(fermensEN ? "Button text color" : "Цвет текста в кнопке")]
        public string buttoncolortext;
        [JsonProperty(fermensEN ? "Text color" : "Цвет текста")]
        public string textcolor;
        [JsonProperty(fermensEN ? "Green button color" : "Цвет зелёной кнопки")]
        public string greenbuttoncolor;
        [JsonProperty(fermensEN ? "Red button color" : "Цвет красной кнопки")]
        public string redbuttoncolor;
        [JsonProperty(fermensEN ? "Gray button color" : "Цвет серой кнопки")]
        public string graybuttoncolor;
        [JsonProperty(fermensEN ? "Header text color" : "Цвет текста заголовка")]
        public string headertextcolor;
        [JsonProperty(fermensEN ? "Error text color" : "Цвет текста ошибки")]
        public string errortextcolor;
        [JsonProperty(fermensEN ? "Text color of <exit> and <back> buttons" : "Цвет текста кнопок <выход> и <назад>")]
        public string colortextexit;
        [JsonProperty(fermensEN ? "Rectangular container text color" : "Цвет текст прямоугольного контейнера")]
        public string rectangulartextcolor;
        [JsonProperty(fermensEN ? "The color of the text with hints at the bottom of the screen" : "Цвет текста с подсказками внизу экрана")]
        public string hintstextcolor;
        [JsonProperty(fermensEN ? "Abbreviations and their colors" : "Аббревиатуры и их цвета")]
        public UIMainMenu uIMainMenu;
    }

     class UIMainMenu
    {
        [JsonProperty(fermensEN ? "Abbreviation for telegram" : "Аббревиатура для телеграма")]
        public string abr_telegram;
        [JsonProperty(fermensEN ? "Telegram icon color" : "Цвет иконки телеграма")]
        public string color_telegram;
        [JsonProperty(fermensEN ? "Abbreviation for vk.com" : "Аббревиатура для вконтакте")]
        public string abr_vk;
        [JsonProperty(fermensEN ? "Vk.com icon color" : "Цвет иконки вконтакте")]
        public string color_vk;
        [JsonProperty(fermensEN ? "Abbreviation for rust+" : "Аббревиатура для rust+")]
        public string abr_rustplus;
        [JsonProperty(fermensEN ? "Rust+ icon color" : "Цвет иконки rust+")]
        public string color_rustplus;
        [JsonProperty(fermensEN ? "Abbreviation for discord" : "Аббревиатура для дискорда")]
        public string abr_discord;
        [JsonProperty(fermensEN ? "Discord icon color" : "Цвет иконки дискорда")]
        public string color_discord;
        [JsonProperty(fermensEN ? "Abbreviation for in game" : "Аббревиатура для графическое отображение в игре")]
        public string abr_ui;
        [JsonProperty(fermensEN ? "In game icon color" : "Цвет иконки графическое отображение в игре")]
        public string color_ui;
    }

    private class PluginConfig
    {
        [JsonProperty(fermensEN ? "Server name, will using for alerts" : "Название сервера - для оповещений")]
        public string servername;
        [JsonProperty(fermensEN ? "Raid alert works only for those who have permission" : "Оповещение о рейде работает только для тех, у кого есть разрешение")]
        public bool needpermission;
        [JsonProperty(fermensEN ? "VK.com" : "Оповещание о рейде в ВК")]
        public VK vk;
        [JsonProperty(fermensEN ? "Rust+" : "Оповещание о рейде в Rust+")]
        public RUSTPLUS rustplus;
        [JsonProperty(fermensEN ? "In game" : "Оповещание о рейде в игре")]
        public INGAME ingame;
        [JsonProperty(fermensEN ? "Discord" : "Оповещание о рейде в дискорд")]
        public DISCORD discord;
        [JsonProperty(fermensEN ? "Telegram" : "Оповещание о рейде в телеграм")]
        public TELEGRAM telegram { get; set; }
        [JsonProperty(fermensEN ? "Menu UI" : "Настройка UI")]
        public UIMenu ui { get; set; }
        [JsonProperty(fermensEN ? "Additional list" : "Дополнительный список предметов, которые учитывать")]
        public string[] spisok;
        [JsonProperty(fermensEN ? "Notification when usual items are destroyed" : "Оповещение при уничтожении обычных предметов")]
        public bool extralist;
        public static PluginConfig DefaultConfig();
    }

    private static string[] _spisok;
    private readonly BotConnection _discordSettings;
    private DiscordGuild _guild;
    public DiscordClient Client { get; set; }
    private void CreateClient();
    private void DiscordCreateMessage(DiscordChannel channel, List<DiscordEmbed> embeds, List<ActionRowComponent> components);
    [HookMethod(DiscordExtHooks.OnDiscordInteractionCreated)]
    private void OnDiscordInteractionCreated(DiscordInteraction interaction);
    private void HandleAcceptLinkButton(DiscordInteraction interaction, DiscordUser user);
    private void CloseClient();
    private void CREATECHANNEL(string dsid, string text);
    private void SENDMESSAGE(string dsid, string text);
    public List<ActionRowComponent> CreateComponents(string button);
    private readonly List<Regex> _regexTags;
    private readonly List<string> _tags;
    private string STRIP(string original);
    private DiscordChannel GetChannel(string id);
     string connect;
     string FON;
     string MAIN;
     string UI;
     string IF2;
     string IF2A;
     string BTN;
     string ER;
     string IBLOCK;
     string MAINH;
     string AG;
     string EXIT;
     string BACK;
     class Storage
    {
        public string vk;
        public string telegram;
        public ulong discord;
        public bool rustplus;
        public bool ingamerust { get; set; }
    }

    private Storage GetStorage(ulong userid);
    private void SaveStorage(BasePlayer player);
    private IEnumerator Saving(string userid, Storage storage);
     Dictionary<ulong, Storage> datas;
    private void GetRequestTelegram(string reciverID, string msg, BasePlayer player, bool accept);
    private IEnumerator GetCallbackTelegram(int code, string response, string id, BasePlayer player, bool accept);
    const string connects;
     class ALERT
    {
        public DateTime gamecooldown;
        public DateTime rustpluscooldown;
        public DateTime vkcooldown;
        public DateTime discordcooldown;
        public DateTime vkcodecooldown;
        public DateTime telegramcooldown;
        public DateTime telegramcodecooldown;
    }

    private static Dictionary<ulong, ALERT> alerts;
     class CODE
    {
        public string id;
        public ulong gameid;
    }

    private Dictionary<string, CODE> VKCODES;
    private Dictionary<ulong, string> DISCORDCODES;
    private void GetRequest(string reciverID, string msg, BasePlayer player, string num);
    private void SendError(BasePlayer player, string key);
    private IEnumerator GetCallbackVK(int code, string response, string id, BasePlayer player, string num);
    private string perm;
    [PluginReference]
     Plugin BMenu;
    private void callcommandrn(BasePlayer player, string command, string[] arg);
    private bool HasAcces(string id);
    private void OpenMenu(BasePlayer player, bool first);
     class C
    {
        public string min;
        public string max;
    }

     Dictionary<int, C> _caddele;
    private void AddElementUI(BasePlayer player, string name, string color, string button, string command, string ico, string icocolor, int num);
     Dictionary<ulong, string> write;
    [ConsoleCommand("raid.input")]
     void ccmdopeinput(ConsoleSystem.Arg arg);
    private void SendError2(BasePlayer player, string key);
    [ConsoleCommand("raid.ingame")]
     void raplsgame(ConsoleSystem.Arg arg);
    [ConsoleCommand("raid.rustplus")]
     void rapls(ConsoleSystem.Arg arg);
    [ConsoleCommand("raid.discordadd")]
     void ccmdadiscoradd(ConsoleSystem.Arg arg);
    [ConsoleCommand("raid.acceptds")]
     void raidacceptds(ConsoleSystem.Arg arg);
    [ConsoleCommand("raid.discorddelete")]
     void vdiscorddelete(ConsoleSystem.Arg arg);
    [ConsoleCommand("raid.tgdelete")]
     void rgdelete(ConsoleSystem.Arg arg);
    [ConsoleCommand("raid.tgadd")]
     void ccmdtgadd(ConsoleSystem.Arg arg);
    [ConsoleCommand("raid.accepttg")]
     void ccmdaccepttg(ConsoleSystem.Arg arg);
    [ConsoleCommand("raid.vkdelete")]
     void vkdelete(ConsoleSystem.Arg arg);
    [ConsoleCommand("raid.vkadd")]
     void ccmdavkadd(ConsoleSystem.Arg arg);
    [ConsoleCommand("raid.accept")]
     void ccmdaccept(ConsoleSystem.Arg arg);
    [ConsoleCommand("raid.send")]
     void ccmdopesendt(ConsoleSystem.Arg arg);
    private string RANDOMNUM();
    private void Unload();
    private void OnServerInitialized();
    public Dictionary<string, string> messagesEN;
    public Dictionary<string, string> messagesRU;
     IEnumerator GetCallback();
    private void OnEntityDeath(BaseCombatEntity entity, HitInfo info);
    private IEnumerator Alerting(BaseCombatEntity entity, BasePlayer player, int tt);
     List<ulong> block;
    private void ALERTPLAYER(ulong ID, string name, string quad, string connect, string destroy, string attackerid);
    private Dictionary<ulong, Timer> timal;
    private string GetMessage(string key, string userId);
    private static Dictionary<string, Vector3> Grids;
    private void CreateSpawnGrid();
    private string GetNameGrid(Vector3 pos);
}

 class VK
{
    [JsonProperty(fermensEN ? "Enable?" : "Включить?")]
    public bool enable;
    [JsonProperty(fermensEN ? "API" : "API от группы")]
    public string api;
    [JsonProperty(fermensEN ? "Link to the group" : "Ссылка на группу")]
    public string link;
    [JsonProperty(fermensEN ? "Cooldown for sending" : "Кд на отправку")]
    public float cooldown;
}

 class RUSTPLUS
{
    [JsonProperty(fermensEN ? "Enable?" : "Включить?")]
    public bool enable;
    [JsonProperty(fermensEN ? "Cooldown for sending" : "Кд на отправку")]
    public float cooldown;
}

 class INGAME
{
    [JsonProperty(fermensEN ? "Enable?" : "Включить?")]
    public bool enable;
    [JsonProperty(fermensEN ? "Cooldown for sending" : "Кд на отправку")]
    public float cooldown;
    [JsonProperty(fermensEN ? "Send game effect when notification are received" : "Эффект при получении уведомления")]
    public string effect;
    [JsonProperty(fermensEN ? "Time after the UI is destroyed" : "Время, через которое пропадает UI [секунды]")]
    public float destroy;
    [JsonProperty("UI")]
    public string UI;
}

 class DISCORD
{
    [JsonProperty(fermensEN ? "Enable?" : "Включить?")]
    public bool enable;
    [JsonProperty(fermensEN ? "Cooldown for sending" : "Кд на отправку")]
    public float cooldown;
    [JsonProperty(fermensEN ? "Invitation link" : "Приглашение в группу")]
    public string link;
    [JsonProperty(fermensEN ? "Token (https://discordapp.com/developers/applications)" : "Токен бота (https://discordapp.com/developers/applications)")]
    public string token;
    [JsonProperty(fermensEN ? "Channel ID, where the player will take the code to confirm the profile" : "ID канала, гле игрок будет брать код, для подтверджения профиля")]
    public string channel;
    [JsonProperty(fermensEN ? "Server ID" : "ID Сервера")]
    public ulong server;
    [JsonProperty(fermensEN ? "Info text" : "Дискорд канал с получением кода - текст")]
    public string channeltext;
    [JsonProperty(fermensEN ? "Info text - line color on the left" : "Дискорд канал с получением кода - цвет линии слева (https://gist.github.com/thomasbnt/b6f455e2c7d743b796917fa3c205f812#file-code_colors_discordjs-md)")]
    public uint channelcolor;
    [JsonProperty(fermensEN ? "Text on button" : "Дискорд канал с получением кода - кнопка")]
    public string channelbutton;
    [JsonProperty(fermensEN ? "Reply after button click" : "Дискорд канал с получением кода - ответ")]
    public string channelex;
    [JsonProperty(fermensEN ? "Don't touch this field" : "Дискорд канал с получением кода - ID сообщения (не трогаем! сам заполнится!)")]
    public string channelmessageid;
}

 class TELEGRAM
{
    [JsonProperty(fermensEN ? "Enable?" : "Включить?")]
    public bool enable;
    [JsonProperty(fermensEN ? "Cooldown for sending" : "Кд на отправку")]
    public float cooldown;
    [JsonProperty(fermensEN ? "Bot tag" : "Тэг бота")]
    public string bottag;
    [JsonProperty(fermensEN ? "Token" : "Токен")]
    public string token;
}

 class UIMenu
{
    [JsonProperty(fermensEN ? "Background color" : "Цвет фона")]
    public string background;
    [JsonProperty(fermensEN ? "Strip color" : "Цвет полоски")]
    public string stripcolor;
    [JsonProperty(fermensEN ? "Rectangular container background color" : "Цвет фона прямоугольного контейнера")]
    public string rectangularcolor;
    [JsonProperty(fermensEN ? "Button text color" : "Цвет текста в кнопке")]
    public string buttoncolortext;
    [JsonProperty(fermensEN ? "Text color" : "Цвет текста")]
    public string textcolor;
    [JsonProperty(fermensEN ? "Green button color" : "Цвет зелёной кнопки")]
    public string greenbuttoncolor;
    [JsonProperty(fermensEN ? "Red button color" : "Цвет красной кнопки")]
    public string redbuttoncolor;
    [JsonProperty(fermensEN ? "Gray button color" : "Цвет серой кнопки")]
    public string graybuttoncolor;
    [JsonProperty(fermensEN ? "Header text color" : "Цвет текста заголовка")]
    public string headertextcolor;
    [JsonProperty(fermensEN ? "Error text color" : "Цвет текста ошибки")]
    public string errortextcolor;
    [JsonProperty(fermensEN ? "Text color of <exit> and <back> buttons" : "Цвет текста кнопок <выход> и <назад>")]
    public string colortextexit;
    [JsonProperty(fermensEN ? "Rectangular container text color" : "Цвет текст прямоугольного контейнера")]
    public string rectangulartextcolor;
    [JsonProperty(fermensEN ? "The color of the text with hints at the bottom of the screen" : "Цвет текста с подсказками внизу экрана")]
    public string hintstextcolor;
    [JsonProperty(fermensEN ? "Abbreviations and their colors" : "Аббревиатуры и их цвета")]
    public UIMainMenu uIMainMenu;
}

 class UIMainMenu
{
    [JsonProperty(fermensEN ? "Abbreviation for telegram" : "Аббревиатура для телеграма")]
    public string abr_telegram;
    [JsonProperty(fermensEN ? "Telegram icon color" : "Цвет иконки телеграма")]
    public string color_telegram;
    [JsonProperty(fermensEN ? "Abbreviation for vk.com" : "Аббревиатура для вконтакте")]
    public string abr_vk;
    [JsonProperty(fermensEN ? "Vk.com icon color" : "Цвет иконки вконтакте")]
    public string color_vk;
    [JsonProperty(fermensEN ? "Abbreviation for rust+" : "Аббревиатура для rust+")]
    public string abr_rustplus;
    [JsonProperty(fermensEN ? "Rust+ icon color" : "Цвет иконки rust+")]
    public string color_rustplus;
    [JsonProperty(fermensEN ? "Abbreviation for discord" : "Аббревиатура для дискорда")]
    public string abr_discord;
    [JsonProperty(fermensEN ? "Discord icon color" : "Цвет иконки дискорда")]
    public string color_discord;
    [JsonProperty(fermensEN ? "Abbreviation for in game" : "Аббревиатура для графическое отображение в игре")]
    public string abr_ui;
    [JsonProperty(fermensEN ? "In game icon color" : "Цвет иконки графическое отображение в игре")]
    public string color_ui;
}

private class PluginConfig
{
    [JsonProperty(fermensEN ? "Server name, will using for alerts" : "Название сервера - для оповещений")]
    public string servername;
    [JsonProperty(fermensEN ? "Raid alert works only for those who have permission" : "Оповещение о рейде работает только для тех, у кого есть разрешение")]
    public bool needpermission;
    [JsonProperty(fermensEN ? "VK.com" : "Оповещание о рейде в ВК")]
    public VK vk;
    [JsonProperty(fermensEN ? "Rust+" : "Оповещание о рейде в Rust+")]
    public RUSTPLUS rustplus;
    [JsonProperty(fermensEN ? "In game" : "Оповещание о рейде в игре")]
    public INGAME ingame;
    [JsonProperty(fermensEN ? "Discord" : "Оповещание о рейде в дискорд")]
    public DISCORD discord;
    [JsonProperty(fermensEN ? "Telegram" : "Оповещание о рейде в телеграм")]
    public TELEGRAM telegram { get; set; }
    [JsonProperty(fermensEN ? "Menu UI" : "Настройка UI")]
    public UIMenu ui { get; set; }
    [JsonProperty(fermensEN ? "Additional list" : "Дополнительный список предметов, которые учитывать")]
    public string[] spisok;
    [JsonProperty(fermensEN ? "Notification when usual items are destroyed" : "Оповещение при уничтожении обычных предметов")]
    public bool extralist;
    public static PluginConfig DefaultConfig();
}

 class Storage
{
    public string vk;
    public string telegram;
    public ulong discord;
    public bool rustplus;
    public bool ingamerust { get; set; }
}

 class ALERT
{
    public DateTime gamecooldown;
    public DateTime rustpluscooldown;
    public DateTime vkcooldown;
    public DateTime discordcooldown;
    public DateTime vkcodecooldown;
    public DateTime telegramcooldown;
    public DateTime telegramcodecooldown;
}

 class CODE
{
    public string id;
    public ulong gameid;
}

 class C
{
    public string min;
    public string max;
}


```

---

## AbsolutCombat

```csharp
using System.Collections.Generic;
using System;
using System.Linq;
using UnityEngine;
using Oxide.Core.Plugins;
using Oxide.Core.Configuration;
using Oxide.Game.Rust.Cui;
using Oxide.Core;
using System.Collections;
using System.IO;
using System.Text;
using Oxide.Core.Libraries.Covalence;
using System.Reflection;

Oxide.Plugins
[Info("AbsolutCombat", "Absolut", "2.2.0", ResourceId = 2103)]
 class AbsolutCombat : RustPlugin
{
    [PluginReference]
     Plugin EventManager;
    [PluginReference]
     Plugin ServerRewards;
    [PluginReference]
     Plugin Economics;
    [PluginReference]
     Plugin BetterChat;
    [PluginReference]
     ImageLibrary ImageLibrary;
     Gear_Weapon_Data gwData;
    private DynamicConfigFile GWData;
     SavedPlayers playerData;
    private DynamicConfigFile PlayerData;
     string TitleColor;
     string MsgColor;
    private Dictionary<ulong, Timer> PlayerGearSetTimer;
    private Dictionary<ulong, Timer> PlayerWeaponSetTimer;
    private Dictionary<string, Timer> timers;
    private Dictionary<ulong, PurchaseItem> PendingPurchase;
    private Dictionary<ulong, PurchaseItem> SetSelection;
    private Dictionary<ulong, Dictionary<string, Dictionary<string, List<string>>>> WeaponSelection;
    private List<ACPlayer> ACPlayers;
    private Dictionary<ulong, GearCollectionCreation> NewGearCollection;
    private Dictionary<ulong, WeaponCollectionCreation> NewWeaponCollection;
    private Dictionary<ulong, string> SavingCollection;
    private Dictionary<ulong, screen> ACUIInfo;
     class screen
    {
        public bool open;
        public bool admin;
        public string GearSet;
        public string WeaponSet;
        public int GearIndex;
        public int WeaponIndex;
    }

    private readonly string corpsePrefab;
    private uint corpsePrefabId;
     void Loaded();
     void Unload();
     void OnServerInitialized();
    private void OnPlayerInit(BasePlayer player);
    private void OnPlayerRespawned(BasePlayer player);
    private void OnEntityDeath(BaseEntity entity, HitInfo hitInfo);
    private void SRAction(ulong ID, int amount, string action);
    private void ECOAction(ulong ID, int amount, string action);
     object OnBetterChat(IPlayer iplayer, string message);
    private object OnPlayerChat(ConsoleSystem.Arg arg);
    private void CollectionCreationChat(BasePlayer player, string[] Args);
     void OnEntitySpawned(BaseNetworkable entity);
    private void OnLootEntity(BasePlayer looter, BaseEntity target);
     void OnPlantGather(PlantEntity Plant, Item item, BasePlayer player);
     void OnCollectiblePickup(Item item, BasePlayer player);
     void OnDispenserGather(ResourceDispenser Dispenser, BaseEntity entity, Item item);
     object OnItemCraft(ItemCraftTask task, BasePlayer crafter);
    public string GetImage(string shortname, ulong skin);
    private void InitializeACPlayer(BasePlayer player);
     void CheckSets(ACPlayer player);
    private object CheckPoints(ulong ID);
     void DestroyACPlayer(ACPlayer player);
     ACPlayer GetACPlayer(BasePlayer player);
    private string GetLang(string msg);
    private void GetSendMSG(BasePlayer player, string message, string arg1, string arg2, string arg3);
    private string GetMSG(string message, string arg1, string arg2, string arg3);
    static void TPPlayer(BasePlayer player, Vector3 destination);
    static void StartSleeping(BasePlayer player);
    [ConsoleCommand("OpenACUI")]
    private void cmdOpenACUI(ConsoleSystem.Arg arg);
    private void OpenACUI(BasePlayer player);
    public void DestroyUI(BasePlayer player);
    public void DestroyACPanel(BasePlayer player);
    public void Broadcast(string message, string userid);
    private void SendDeathNote(BasePlayer player, BasePlayer victim);
    private void SaveCollect(BasePlayer player);
    private void ExitSetCreation(BasePlayer player);
     bool isAuth(BasePlayer player);
    private List<BasePlayer> FindPlayer(string arg);
    [ConsoleCommand("addmoney")]
    private void cmdaddmoney(ConsoleSystem.Arg arg);
    [ConsoleCommand("takemoney")]
    private void cmdtakemoney(ConsoleSystem.Arg arg);
    [ConsoleCommand("addkills")]
    private void cmdaddkills(ConsoleSystem.Arg arg);
    [ConsoleCommand("takekills")]
    private void cmdtakekills(ConsoleSystem.Arg arg);
    [ChatCommand("addmoney")]
    private void chatAddMoney(BasePlayer player, string command, string[] args);
    [ChatCommand("addkills")]
    private void chatAddKills(BasePlayer player, string command, string[] args);
    [ChatCommand("takekills")]
    private void chatTakeKills(BasePlayer player, string command, string[] args);
    [ChatCommand("takemoney")]
    private void chattakemoney(BasePlayer player, string command, string[] args);
    private string PanelAC;
    private string GLPanel;
    private string WLPanel;
    private string GPanel;
    private string WPanel;
    private string APanel;
    private string PanelOnScreen;
    private string PanelPurchaseConfirmation;
    private string PanelStats;
    public class UI
    {
        static public CuiElementContainer CreateElementContainer(string panelName, string color, string aMin, string aMax, bool cursor);
        static public void CreatePanel(CuiElementContainer container, string panel, string color, string aMin, string aMax, bool cursor);
        static public void CreateLabel(CuiElementContainer container, string panel, string color, string text, int size, string aMin, string aMax, TextAnchor align);
        static public void CreateButton(CuiElementContainer container, string panel, string color, string text, int size, string aMin, string aMax, string command, TextAnchor align);
        static public void LoadImage(CuiElementContainer container, string panel, string png, string aMin, string aMax);
        static public void CreateTextOverlay(CuiElementContainer container, string panel, string text, string color, int size, string aMin, string aMax, TextAnchor align, float fadein);
        static public void CreateTextOutline(CuiElementContainer element, string panel, string colorText, string colorOutline, string text, int size, string DistanceA, string DistanceB, string aMin, string aMax, TextAnchor align);
    }

    private Dictionary<string, string> UIColors;
    private Dictionary<string, string> TextColors;
    private Dictionary<Slot, Vector2> LeftGearSlotPos;
    private Dictionary<Slot, Vector2> RightGearSlotPos;
    private Dictionary<Slot, Vector2> GearSlotPos;
    private Dictionary<Slot, Vector2> WeaponSlotPos;
    private Dictionary<Slot, Vector2> MainAttachmentSlotsPos;
    private Dictionary<Slot, Vector2> SecondaryAttachmentSlotsPos;
    private Dictionary<Slot, Vector2> AccessoriesSlotsPos;
    private Dictionary<Slot, Vector2> AmmunitionSlotsPos;
     void ACPanel(BasePlayer player);
     void GearListPanel(BasePlayer player);
     void WeaponListPanel(BasePlayer player);
    private float[] CalcSetButtons(int number);
     void GearPanel(BasePlayer player);
     void WeaponPanel(BasePlayer player);
    private void AttachmentPanel(BasePlayer player);
    private void PurchaseConfirmation(BasePlayer player, string item);
     void OnScreen(BasePlayer player, string msg);
     void PlayerHUD(BasePlayer player);
    private void SelectIfFree(BasePlayer player, string item, string type);
    private void NumberPad(BasePlayer player, string cmd, string title, string item, string type);
    private void CreateNumberPadButton(CuiElementContainer container, string panelName, int i, int number, string command, string item, string type);
    private float[] CalcButtonPos(int number);
    private float[] CalcNumButtonPos(int number);
    [ConsoleCommand("UI_AddItemAttributes")]
    private void cmdUI_AddGearAttributes(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_RemoveItem")]
    private void cmdUI_RemoveItem(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_SelectCollectionItem")]
    private void cmdUI_SelectGear(ConsoleSystem.Arg arg);
    private void SelectGearSlotItem(BasePlayer player, Slot slot, string type, int page);
    [ConsoleCommand("UI_AddItem")]
    private void cmdUI_AddItem(ConsoleSystem.Arg arg);
    private void SelectSkin(BasePlayer player, string item, string type);
    [ConsoleCommand("UI_SetItemKillRequirement")]
    private void cmdUI_SetGearKillRequirement(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_SetItemPrice")]
    private void cmdUI_SetGearPrice(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_SetCollectionCost")]
    private void cmdUI_SetCollectionCost(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_SetCollectionKills")]
    private void cmdUI_SetCollectionKills(ConsoleSystem.Arg arg);
    private void SetCollectionName(BasePlayer player);
    [ConsoleCommand("UI_Free")]
    private void cmdUI_Free(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_ChangeGearSet")]
    private void cmdUI_ChangeGearSet(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_ChangeWeaponSet")]
    private void cmdUI_ChangeWeaponSet(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_SwitchAdminView")]
    private void cmdUI_SwitchAdminView(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_GearIndexShownChange")]
    private void cmdUI_GearIndexShownChange(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_WeaponIndexShownChange")]
    private void cmdUI_WeaponIndexShownChange(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_DestroyPurchaseConfirmation")]
    private void cmdUI_DestroyPurchaseConfirmation(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_ProcessAttachment")]
    private void cmdUI_ProcessAttachment(ConsoleSystem.Arg arg);
     void ProcessAttachment(BasePlayer player, string request, string set, string weapon, string attachment);
    [ConsoleCommand("UI_ProcessSelection")]
    private void cmdUI_ProcessSelection(ConsoleSystem.Arg arg);
     void ProcessSelection(BasePlayer player, string type, string set);
    [ConsoleCommand("UI_PurchasingPanel")]
    private void cmdPurchasePanel(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_PrepPurchase")]
    private void cmdUI_PrepPurchase(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_Purchase")]
    private void cmdUI_Purchase(ConsoleSystem.Arg arg);
     void Purchase(BasePlayer player, string item);
    [ConsoleCommand("UI_SaveCollect")]
    private void cmdUI_SaveSet(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_CreateGearSet")]
    private void cmdUI_CreateGearSet(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_CancelGearSet")]
    private void cmdUI_CancelGearSet(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_CancelWeaponSet")]
    private void cmdUI_CancelWeaponSet(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_CreateWeaponSet")]
    private void cmdUI_CreateWeaponSet(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_DeleteGearSet")]
    private void cmdUI_DeleteGearSet(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_DeleteWeaponSet")]
    private void cmdUI_DeleteWeaponSet(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_SelectSkin")]
    private void cmdUI_SelectSkin(ConsoleSystem.Arg arg);
    private void SelectSet(BasePlayer player, string name);
    private void TimerPlayerGearSetselection(BasePlayer player);
    private void SelectWeapons(BasePlayer player);
    private void TimerPlayerWeaponselection(BasePlayer player);
    private void GiveSet(BasePlayer player);
    private void GiveWeapon(BasePlayer player);
    private Item BuildWeapon(Weapon weapon, BasePlayer player);
    private Item BuildAmmo(string shortname, int amount);
    private Item BuildSet(Gear gear);
    private Item BuildItem(string shortname, int amount, ulong skin);
    public void GiveItem(BasePlayer player, Item item, string container);
    public void CreateGearSet(BasePlayer player);
    public void CreateWeaponSet(BasePlayer player);
    [Serializable]
     class ACPlayer : MonoBehaviour
    {
        public BasePlayer player;
        public int kills;
        public int deaths;
        public int money;
        public Dictionary<string, List<string>> PlayerGearSets;
        public Dictionary<string, Dictionary<string, List<string>>> PlayerWeaponSets;
        public string currentGearSet;
        public string currentWeaponSet;
        public Dictionary<string, List<string>> CurrentWeapons;
        public Dictionary<string, int> GearSetKills;
        public Dictionary<string, int> WeaponSetKills;
         void Awake();
    }

     class Gear_Weapon_Data
    {
        public Dictionary<string, GearSet> GearSets;
        public Dictionary<string, WeaponSet> WeaponSets;
        public Dictionary<Slot, List<string>> Items;
        public Dictionary<string, Dictionary<ulong, uint>> Images;
    }

     class SavedPlayers
    {
        public Dictionary<ulong, SavedPlayer> players;
        public Dictionary<ulong, SavedPlayer> priorSave;
    }

     class SavedPlayer
    {
        public int kills;
        public int deaths;
        public int money;
        public string lastgearSet;
        public string lastweaponSet;
        public Dictionary<string, List<string>> lastWeapons;
        public Dictionary<string, List<string>> PlayerGearSets;
        public Dictionary<string, Dictionary<string, List<string>>> PlayerWeaponSets;
        public Dictionary<string, int> GearSetKills;
        public Dictionary<string, int> WeaponSetKills;
    }

     class GearSet
    {
        public int index;
        public int cost;
        public int killsrequired;
        public List<Gear> set;
    }

     class WeaponCollectionCreation
    {
        public string setname;
        public CreationWeaponSet collection;
    }

     class GearCollectionCreation
    {
        public string setname;
        public CreationGearSet collection;
    }

     class CreationGearSet
    {
        public int index;
        public int cost;
        public int killsrequired;
        public bool free;
        public Dictionary<string, Gear> set;
    }

     class CreationWeaponSet
    {
        public int index;
        public int cost;
        public int killsrequired;
        public bool free;
        public string currentweapon;
        public Dictionary<string, Weapon> set;
    }

     class WeaponSet
    {
        public int index;
        public int cost;
        public int killsrequired;
        public List<Weapon> set;
    }

     class Attachment
    {
        public bool free;
        public string shortname;
        public int cost;
        public int killsrequired;
        public Slot slot;
        public string location;
    }

     class PurchaseItem
    {
        public Dictionary<string, Gear> gear;
        public Dictionary<string, Weapon> weapon;
        public Dictionary<Slot,Dictionary<string, Attachment>> attachment;
        public string setname;
        public bool set;
        public bool weaponpurchase;
        public bool gearpurchase;
        public bool attachmentpurchase;
        public string attachmentName;
        public int setprice;
        public int setkillrequirement;
    }

     class Gear
    {
        public bool free;
        public string shortname;
        public Slot slot;
        public int price;
        public ulong skin;
        public int killsrequired;
        public int amount;
        public string container;
    }

     class Weapon
    {
        public bool free;
        public string shortname;
        public ulong skin;
        public Slot slot;
        public string container;
        public int killsrequired;
        public int price;
        public int amount;
        public int ammo;
        public string ammoType;
        public Dictionary<string, Attachment> attachments;
    }

    private void SaveLoop();
    private void InfoLoop();
    private void CondLoop();
     object AddMoney(ulong TargetID, int amount, bool notify, ulong RequestorID);
     object TakeMoney(ulong TargetID, int amount, bool notify, ulong RequestorID);
     object AddKills(ulong TargetID, int amount, string type, string collection, ulong RequestorID);
     object TakeKills(ulong TargetID, int amount, string type, string collection, ulong RequestorID);
    private Dictionary<Slot, List<string>> DefaultItems;
    private Dictionary<string, Attachment> DefaultAttachments;
    private Dictionary<string, GearSet> DefaultGearSets;
    private Dictionary<string, WeaponSet> DefaultWeaponSets;
     void SaveACPlayer(ACPlayer player);
     void SaveData();
     void LoadData();
     void LoadDefaultItemsList();
    private ConfigData configData;
     class ConfigData
    {
        public int InfoInterval { get; set; }
        public int KillReward { get; set; }
        public bool BroadcastDeath { get; set; }
        public bool UseEnviroControl { get; set; }
        public int SetCooldown { get; set; }
        public bool PersistentCondition { get; set; }
        public string MenuKeyBinding { get; set; }
        public bool UseServerRewards { get; set; }
        public bool UseEconomics { get; set; }
        public int DefaultAmmoReloads { get; set; }
        public bool UseAccessories { get; set; }
    }

    private void LoadVariables();
    protected override void LoadDefaultConfig();
    private void LoadConfigVariables();
     void SaveConfig(ConfigData config);
     Dictionary<string, string> messages;
}

 class screen
{
    public bool open;
    public bool admin;
    public string GearSet;
    public string WeaponSet;
    public int GearIndex;
    public int WeaponIndex;
}

public class UI
{
    static public CuiElementContainer CreateElementContainer(string panelName, string color, string aMin, string aMax, bool cursor);
    static public void CreatePanel(CuiElementContainer container, string panel, string color, string aMin, string aMax, bool cursor);
    static public void CreateLabel(CuiElementContainer container, string panel, string color, string text, int size, string aMin, string aMax, TextAnchor align);
    static public void CreateButton(CuiElementContainer container, string panel, string color, string text, int size, string aMin, string aMax, string command, TextAnchor align);
    static public void LoadImage(CuiElementContainer container, string panel, string png, string aMin, string aMax);
    static public void CreateTextOverlay(CuiElementContainer container, string panel, string text, string color, int size, string aMin, string aMax, TextAnchor align, float fadein);
    static public void CreateTextOutline(CuiElementContainer element, string panel, string colorText, string colorOutline, string text, int size, string DistanceA, string DistanceB, string aMin, string aMax, TextAnchor align);
}

[Serializable]
 class ACPlayer : MonoBehaviour
{
    public BasePlayer player;
    public int kills;
    public int deaths;
    public int money;
    public Dictionary<string, List<string>> PlayerGearSets;
    public Dictionary<string, Dictionary<string, List<string>>> PlayerWeaponSets;
    public string currentGearSet;
    public string currentWeaponSet;
    public Dictionary<string, List<string>> CurrentWeapons;
    public Dictionary<string, int> GearSetKills;
    public Dictionary<string, int> WeaponSetKills;
     void Awake();
}

 class Gear_Weapon_Data
{
    public Dictionary<string, GearSet> GearSets;
    public Dictionary<string, WeaponSet> WeaponSets;
    public Dictionary<Slot, List<string>> Items;
    public Dictionary<string, Dictionary<ulong, uint>> Images;
}

 class SavedPlayers
{
    public Dictionary<ulong, SavedPlayer> players;
    public Dictionary<ulong, SavedPlayer> priorSave;
}

 class SavedPlayer
{
    public int kills;
    public int deaths;
    public int money;
    public string lastgearSet;
    public string lastweaponSet;
    public Dictionary<string, List<string>> lastWeapons;
    public Dictionary<string, List<string>> PlayerGearSets;
    public Dictionary<string, Dictionary<string, List<string>>> PlayerWeaponSets;
    public Dictionary<string, int> GearSetKills;
    public Dictionary<string, int> WeaponSetKills;
}

 class GearSet
{
    public int index;
    public int cost;
    public int killsrequired;
    public List<Gear> set;
}

 class WeaponCollectionCreation
{
    public string setname;
    public CreationWeaponSet collection;
}

 class GearCollectionCreation
{
    public string setname;
    public CreationGearSet collection;
}

 class CreationGearSet
{
    public int index;
    public int cost;
    public int killsrequired;
    public bool free;
    public Dictionary<string, Gear> set;
}

 class CreationWeaponSet
{
    public int index;
    public int cost;
    public int killsrequired;
    public bool free;
    public string currentweapon;
    public Dictionary<string, Weapon> set;
}

 class WeaponSet
{
    public int index;
    public int cost;
    public int killsrequired;
    public List<Weapon> set;
}

 class Attachment
{
    public bool free;
    public string shortname;
    public int cost;
    public int killsrequired;
    public Slot slot;
    public string location;
}

 class PurchaseItem
{
    public Dictionary<string, Gear> gear;
    public Dictionary<string, Weapon> weapon;
    public Dictionary<Slot,Dictionary<string, Attachment>> attachment;
    public string setname;
    public bool set;
    public bool weaponpurchase;
    public bool gearpurchase;
    public bool attachmentpurchase;
    public string attachmentName;
    public int setprice;
    public int setkillrequirement;
}

 class Gear
{
    public bool free;
    public string shortname;
    public Slot slot;
    public int price;
    public ulong skin;
    public int killsrequired;
    public int amount;
    public string container;
}

 class Weapon
{
    public bool free;
    public string shortname;
    public ulong skin;
    public Slot slot;
    public string container;
    public int killsrequired;
    public int price;
    public int amount;
    public int ammo;
    public string ammoType;
    public Dictionary<string, Attachment> attachments;
}

 class ConfigData
{
    public int InfoInterval { get; set; }
    public int KillReward { get; set; }
    public bool BroadcastDeath { get; set; }
    public bool UseEnviroControl { get; set; }
    public int SetCooldown { get; set; }
    public bool PersistentCondition { get; set; }
    public string MenuKeyBinding { get; set; }
    public bool UseServerRewards { get; set; }
    public bool UseEconomics { get; set; }
    public int DefaultAmmoReloads { get; set; }
    public bool UseAccessories { get; set; }
}


```

---

## AbsolutGifts

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

Oxide.Plugins
[Info("AbsolutGifts", "Absolut", "1.1.1", ResourceId = 2159)]
 class AbsolutGifts : RustPlugin
{
    [PluginReference]
     ImageLibrary ImageLibrary;
    [PluginReference]
     Plugin ServerRewards;
    [PluginReference]
     Plugin Economics;
    [PluginReference]
     Plugin AbsolutCombat;
     GiftData agdata;
    private DynamicConfigFile AGData;
     int GlobalTime;
     string TitleColor;
     string MsgColor;
    private Dictionary<string, Timer> timers;
    private Dictionary<ulong, GiftCreation> giftprep;
    private Dictionary<ulong, int> TimeSinceLastGift;
    private Dictionary<ulong, int> NextGift;
    private Dictionary<ulong, int> CurrentGift;
    private Dictionary<ulong, List<Gift>> Objective;
    private Dictionary<ulong, Vector3> AFK;
     void Loaded();
     void Unload();
     void OnServerInitialized();
    private void OnPlayerDisconnected(BasePlayer player);
    private void OnPlayerInit(BasePlayer player);
    private void InitializePlayer(BasePlayer player);
    private void DestroyPlayer(BasePlayer player);
    static double GrabCurrentTime();
    public string GetImage(string shortname, ulong skin);
    public bool AddImage(string url, string shortname, ulong skin);
    private void CancelGiftCreation(BasePlayer player);
    public void DestroyGiftPanel(BasePlayer player);
     void ChangeGlobalTime();
    private void CheckPlayers();
     bool CheckAFK(BasePlayer player);
    private void InitializeGiftObjective(BasePlayer player);
    private void ResetGifts(BasePlayer player);
    private void GiveGift(BasePlayer player);
    private void CreateNumberPadButton(CuiElementContainer container, string panelName, int i, int number, string command);
    private string GetLang(string msg);
    private void GetSendMSG(BasePlayer player, string message, string arg1, string arg2, string arg3, string arg4);
    private string GetMSG(string message, string arg1, string arg2, string arg3, string arg4);
     bool isAuth(BasePlayer player);
    private string PanelGift;
    public class UI
    {
        static public CuiElementContainer CreateElementContainer(string panel, string color, string aMin, string aMax, bool cursor);
        static public void CreatePanel(CuiElementContainer element, string panel, string color, string aMin, string aMax, bool cursor);
        static public void CreateLabel(CuiElementContainer element, string panel, string color, string text, int size, string aMin, string aMax, TextAnchor align);
        static public void CreateButton(CuiElementContainer element, string panel, string color, string text, int size, string aMin, string aMax, string command, TextAnchor align);
        static public void LoadImage(CuiElementContainer element, string panel, string png, string aMin, string aMax);
        static public void CreateTextOutline(CuiElementContainer element, string panel, string colorText, string colorOutline, string text, int size, string DistanceA, string DistanceB, string aMin, string aMax, TextAnchor align);
        static public void CreateTextOverlay(CuiElementContainer element, string panel, string text, string color, int size, string aMin, string aMax, TextAnchor align, float fadein);
    }

    private Dictionary<string, string> UIColors;
    private Dictionary<string, string> TextColors;
     void GiftPanel(BasePlayer player, int page);
    private void CreateGifts(BasePlayer player, int step, int page);
     void ManageGifts(BasePlayer player, int page);
    private void CreateGiftMenuEntry(CuiElementContainer container, string panelName, int ID, int num, List<int> completed, BasePlayer player);
    private void AdminPanel(BasePlayer player);
    private void NumberPad(BasePlayer player, string cmd, string title);
    private float[] GiftPos(int number);
    private float[] FilterButton(int number);
    private float[] BackgroundButton(int number);
    private float[] CalcButtonPos(int number);
    private float[] CalcNumButtonPos(int number);
    [ChatCommand("gift")]
    private void cmdgift(BasePlayer player, string command, string[] args);
    [ConsoleCommand("UI_GiftMenu")]
    private void cmdUI_GiftMenu(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_SelectTime")]
    private void cmdUI_SelectTime(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_VIP")]
    private void cmdUI_VIP(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_ManageGifts")]
    private void cmdUI_ManageGifts(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_CreateGifts")]
    private void cmdUI_CreateGifts(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_RemoveGift")]
    private void cmdUI_RemoveGift(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_FinalizeGift")]
    private void cmdUI_FinalizeGift(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_DestroyGiftPanel")]
    private void cmdUI_DestroyGiftPanel(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_CancelGiftCreation")]
    private void cmdUI_CancelListing(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_SelectGift")]
    private void cmdUI_SelectGift(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_SelectAmount")]
    private void cmdUI_SelectPriceAmount(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_AdminsPanel")]
    private void cmdUI_AdminMenu(ConsoleSystem.Arg arg);
    private void SaveLoop();
    private void InfoLoop();
    private void SetBoxFullNotification(string ID);
     class GiftData
    {
        public Dictionary<int, GiftCollection> Gifts;
        public Dictionary<ulong, playerdata> Players;
    }

     class GiftCollection
    {
        public bool vip;
        public List<Gift> gifts;
    }

     class Gift
    {
        public int ID;
        public int amount;
        public bool SR;
        public bool Eco;
        public bool AC;
    }

     class playerdata
    {
        public int PlayerTime;
        public List<int> ReceivedGifts;
        public double Lastconnection;
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
    private ConfigData configData;
     class ConfigData
    {
        public int InfoInterval { get; set; }
        public bool NoAFK { get; set; }
    }

    private void LoadVariables();
    protected override void LoadDefaultConfig();
    private void LoadConfigVariables();
     void SaveConfig(ConfigData config);
     Dictionary<string, string> messages;
}

public class UI
{
    static public CuiElementContainer CreateElementContainer(string panel, string color, string aMin, string aMax, bool cursor);
    static public void CreatePanel(CuiElementContainer element, string panel, string color, string aMin, string aMax, bool cursor);
    static public void CreateLabel(CuiElementContainer element, string panel, string color, string text, int size, string aMin, string aMax, TextAnchor align);
    static public void CreateButton(CuiElementContainer element, string panel, string color, string text, int size, string aMin, string aMax, string command, TextAnchor align);
    static public void LoadImage(CuiElementContainer element, string panel, string png, string aMin, string aMax);
    static public void CreateTextOutline(CuiElementContainer element, string panel, string colorText, string colorOutline, string text, int size, string DistanceA, string DistanceB, string aMin, string aMax, TextAnchor align);
    static public void CreateTextOverlay(CuiElementContainer element, string panel, string text, string color, int size, string aMin, string aMax, TextAnchor align, float fadein);
}

 class GiftData
{
    public Dictionary<int, GiftCollection> Gifts;
    public Dictionary<ulong, playerdata> Players;
}

 class GiftCollection
{
    public bool vip;
    public List<Gift> gifts;
}

 class Gift
{
    public int ID;
    public int amount;
    public bool SR;
    public bool Eco;
    public bool AC;
}

 class playerdata
{
    public int PlayerTime;
    public List<int> ReceivedGifts;
    public double Lastconnection;
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
}


```

---

## AbsolutMarket

```csharp
using System.Collections.Generic;
using System;
using System.Linq;
using UnityEngine;
using Oxide.Core.Plugins;
using Oxide.Core.Configuration;
using Oxide.Game.Rust.Cui;
using Oxide.Core;
using System.Collections;
using System.IO;

Oxide.Plugins
[Info("AbsolutMarket", "Absolut", "1.3.0", ResourceId = 2118)]
 class AbsolutMarket : RustPlugin
{
    [PluginReference]
     Plugin ServerRewards;
    static GameObject webObject;
    static UnityImages uImage;
    static UnityBackgrounds uBackground;
     MarketData mData;
    private DynamicConfigFile MData;
     AMImages imgData;
    private DynamicConfigFile IMGData;
     Backgrounds bkData;
    private DynamicConfigFile BKData;
     class Backgrounds
    {
        public Dictionary<string, string> PendingBackgrounds;
    }

     string TitleColor;
     string MsgColor;
    private List<ulong> MenuState;
    private List<ulong> SettingBox;
    private Dictionary<string, Timer> timers;
    private Dictionary<ulong, List<AMItem>> PlayerBoxContents;
    private Dictionary<ulong, AMItem> SalesItemPrep;
    private Dictionary<ulong, List<Item>> PlayerInventory;
    private Dictionary<ulong, List<Item>> TransferableItems;
    private Dictionary<ulong, List<Item>> ItemsToTransfer;
    private Dictionary<ulong, bool> PlayerPurchaseApproval;
    private Dictionary<ulong, AMItem> SaleProcessing;
     void Loaded();
     void Unload();
    private void OnPlayerDisconnected(BasePlayer player);
    private void OnPlayerInit(BasePlayer player);
     void OnServerInitialized();
     void OnEntityBuilt(Planner planner, GameObject gameobject);
    private void OnEntityDeath(BaseEntity entity, HitInfo hitInfo);
    private object OnPlayerChat(ConsoleSystem.Arg arg);
    private StorageContainer GetTradeBox(ulong Buyer);
    private bool GetTradeBoxContents(BasePlayer player);
    private bool BoxCheck(BasePlayer player, uint item);
    private void AddMessages(ulong player, string message, string arg1, string arg2, string arg3, string arg4);
    private void SendMessages(BasePlayer player);
    private IEnumerable<AMItem> GetItems(ItemContainer container);
    private IEnumerable<Item> GetItemsOnly(ItemContainer container);
    private void XferPurchase(ulong buyer, uint ID, ItemContainer from, ItemContainer to);
    private void XferCost(Item item, BasePlayer player, uint Listing);
    private Item BuildCostItems(string shortname, int amount);
     void OnItemRemovedFromContainer(ItemContainer cont, Item item);
    private void RemoveListing(ulong seller, string name, uint ID, string reason);
    private void CancelListing(BasePlayer player);
    private void SRAction(ulong ID, int amount, string action);
    private object CheckPoints(ulong ID);
    private void NumberPad(BasePlayer player, string cmd);
    private void CreateNumberPadButton(CuiElementContainer container, string panelName, int i, int number, string command);
    private string GetLang(string msg);
    private void GetSendMSG(BasePlayer player, string message, string arg1, string arg2, string arg3, string arg4);
    private string GetMSG(string message, string arg1, string arg2, string arg3, string arg4);
    public void DestroyMarketPanel(BasePlayer player);
    public void DestroyPurchaseScreen(BasePlayer player);
    private void ToggleMarketScreen(BasePlayer player);
     bool isAuth(BasePlayer player);
    private string PanelMarket;
    private string PanelPurchase;
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
     void MarketMainScreen(BasePlayer player, int page, Category cat);
    private void CreateMarketListingButton(CuiElementContainer container, string panelName, AMItem item, string listingimg, string costimg, bool seller, int num);
    private void CreateMarketListingButtonSimple(CuiElementContainer container, string panelName, AMItem item, string listingimg, string costimg, bool seller, int num);
     void MarketSellScreen(BasePlayer player, int page);
    private void SellItems(BasePlayer player, int step, int page);
    private void PurchaseConfirmation(BasePlayer player, uint index);
    private void TradeBoxConfirmation(BasePlayer player, uint ID);
    private void AdminPanel(BasePlayer player);
    private void MarketBackgroundMenu(BasePlayer player, int page);
    private void BlackListing(BasePlayer player, int page, string action);
    private float[] MarketEntryPos(int number);
    private float[] FilterButton(int number);
    private float[] BackgroundButton(int number);
    private float[] CalcButtonPos(int number);
    private float[] CalcNumButtonPos(int number);
    [ConsoleCommand("UI_SaveTradeBox")]
    private void cmdUI_SaveTradeBox(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_CancelTradeBox")]
    private void cmdUI_CancelTradeBox(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_DestroyMarketPanel")]
    private void cmdUI_DestroyBoxConfirmation(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_DestroyPurchaseScreen")]
    private void cmdUI_DestroyPurchaseScreen(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_ToggleMarketScreen")]
    private void cmdUI_ToggleMarketScreen(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_SelectSalesItem")]
    private void cmdUI_SelectSalesItem(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_SelectSR")]
    private void cmdUI_SelectSR(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_SRAmount")]
    private void cmdUI_SRAmount(ConsoleSystem.Arg arg);
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
    [ConsoleCommand("UI_AdminPanel")]
    private void cmdUI_AdminPanel(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_MarketSellScreen")]
    private void cmdUI_MarketSellScreen(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_Mode")]
    private void cmdUI_Mode(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_SetBoxMode")]
    private void cmdUI_SetBoxMode(ConsoleSystem.Arg arg);
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
    private void SetBoxFullNotification(string ID);
     class MarketData
    {
        public Dictionary<uint, AMItem> MarketListings;
        public Dictionary<ulong, uint> TradeBox;
        public Dictionary<ulong, string> background;
        public Dictionary<ulong, bool> mode;
        public Dictionary<ulong, List<Unsent>> OutstandingMessages;
        public List<string> Blacklist;
        public Dictionary<ulong, string> names;
    }

     class AMImages
    {
        public Dictionary<Category, Dictionary<string, Dictionary<ulong, uint>>> SavedImages;
        public Dictionary<string, uint> SavedBackgrounds;
    }

     class Unsent
    {
        public string message;
        public string arg1;
        public string arg2;
        public string arg3;
        public string arg4;
    }

     class AMItem
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

     class QueueImage
    {
        public string url;
        public string shortname;
        public ulong skinid;
        public Category cat;
        public QueueImage(string ur, Category ct, string st, ulong sk);
    }

     class UnityImages : MonoBehaviour
    {
         AbsolutMarket filehandler;
        const int MaxActiveLoads;
        static readonly List<QueueImage> QueueList;
        static byte activeLoads;
        private MemoryStream stream;
        public void SetDataDir(AbsolutMarket am);
        public void Add(string url, Category cat, string shortname, ulong skinid);
         void Next();
        private void ClearStream();
         IEnumerator WaitForRequest(WWW www, QueueImage info);
    }

     class QueueBackground
    {
        public string url;
        public string name;
        public QueueBackground(string ur, string nm);
    }

     class UnityBackgrounds : MonoBehaviour
    {
         AbsolutMarket filehandler;
        const int MaxActiveLoads;
        static readonly List<QueueBackground> QueueList;
        static byte activeLoads;
        private MemoryStream stream;
        public void SetDataDir(AbsolutMarket am);
        public void Add(string url, string name);
         void Next();
        private void ClearStream();
         IEnumerator WaitForRequest(WWW www, QueueBackground info);
    }

    [ConsoleCommand("getimages")]
    private void cmdgetimages(ConsoleSystem.Arg arg);
    private void Getimages();
    [ConsoleCommand("refreshimages")]
    private void cmdrefreshimages(ConsoleSystem.Arg arg);
    private void Refreshimages();
    [ConsoleCommand("getbackgrounds")]
    private void cmdgetbackgrounds(ConsoleSystem.Arg arg);
    private void GetBackgrounds();
    [ConsoleCommand("refreshbackgrounds")]
    private void cmdrefreshbackgrounds(ConsoleSystem.Arg arg);
    private void RefreshBackgrounds();
    private void SaveBackgrounds();
    private Dictionary<Category, Dictionary<string, Dictionary<ulong, string>>> urls;
    private Dictionary<string, string> defaultBackgrounds;
     void SaveData();
     void LoadData();
    private ConfigData configData;
     class ConfigData
    {
        public string MarketMenuKeyBinding { get; set; }
        public bool UseUniqueNames { get; set; }
        public bool ServerRewards { get; set; }
        public int InfoInterval { get; set; }
    }

    private void LoadVariables();
    protected override void LoadDefaultConfig();
    private void LoadConfigVariables();
     void SaveConfig(ConfigData config);
     Dictionary<string, string> messages;
}

 class Backgrounds
{
    public Dictionary<string, string> PendingBackgrounds;
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

 class MarketData
{
    public Dictionary<uint, AMItem> MarketListings;
    public Dictionary<ulong, uint> TradeBox;
    public Dictionary<ulong, string> background;
    public Dictionary<ulong, bool> mode;
    public Dictionary<ulong, List<Unsent>> OutstandingMessages;
    public List<string> Blacklist;
    public Dictionary<ulong, string> names;
}

 class AMImages
{
    public Dictionary<Category, Dictionary<string, Dictionary<ulong, uint>>> SavedImages;
    public Dictionary<string, uint> SavedBackgrounds;
}

 class Unsent
{
    public string message;
    public string arg1;
    public string arg2;
    public string arg3;
    public string arg4;
}

 class AMItem
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

 class QueueImage
{
    public string url;
    public string shortname;
    public ulong skinid;
    public Category cat;
    public QueueImage(string ur, Category ct, string st, ulong sk);
}

 class UnityImages : MonoBehaviour
{
     AbsolutMarket filehandler;
    const int MaxActiveLoads;
    static readonly List<QueueImage> QueueList;
    static byte activeLoads;
    private MemoryStream stream;
    public void SetDataDir(AbsolutMarket am);
    public void Add(string url, Category cat, string shortname, ulong skinid);
     void Next();
    private void ClearStream();
     IEnumerator WaitForRequest(WWW www, QueueImage info);
}

 class QueueBackground
{
    public string url;
    public string name;
    public QueueBackground(string ur, string nm);
}

 class UnityBackgrounds : MonoBehaviour
{
     AbsolutMarket filehandler;
    const int MaxActiveLoads;
    static readonly List<QueueBackground> QueueList;
    static byte activeLoads;
    private MemoryStream stream;
    public void SetDataDir(AbsolutMarket am);
    public void Add(string url, string name);
     void Next();
    private void ClearStream();
     IEnumerator WaitForRequest(WWW www, QueueBackground info);
}

 class ConfigData
{
    public string MarketMenuKeyBinding { get; set; }
    public bool UseUniqueNames { get; set; }
    public bool ServerRewards { get; set; }
    public int InfoInterval { get; set; }
}


```

---

## AbsolutSorter

```csharp
using Facepunch;
using System;
using System.Collections.Generic;
using System.Globalization;
using System.Linq;
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Configuration;
using Oxide.Game.Rust.Cui;
using Oxide.Core.Plugins;
using UnityEngine;

Oxide.Plugins
[Info("AbsolutSorter", "k1lly0u", "2.0.3")]
[Description("Sort items from your inventory into designated storage containers with the click of a button")]
 class AbsolutSorter : RustPlugin
{
    private StoredData storedData;
    private DynamicConfigFile data;
    [PluginReference]
     Plugin NoEscape;
     Plugin SkinBox;
    private ItemCategory[] itemCategories;
    private bool wipeDetected;
    private List<ulong> hiddenPlayers;
    private const string SORTING_UI;
    private const string PERMISSION_ALLOW;
    private const string PERMISSION_LOOTALL;
    private const string PERMISSION_ALLOWDUMP;
    private const string BG_COLOR;
    private const string BUTTON_COLOR;
    private const string BUTTON_SELECTED_COLOR;
    private const string BUTTON_EXIT_COLOR;
    private const int ALLOWED_CATEGORIES;
    private void Loaded();
    private void OnServerInitialized();
    private void OnNewSave(string filename);
    private void OnServerSave();
    private void OnPlayerDisconnected(BasePlayer player);
    private void OnPlayerRespawned(BasePlayer player);
    private void OnLootEntity(BasePlayer player, StorageContainer container);
    private void OnLootEntityEnd(BasePlayer player, BaseCombatEntity entity);
    private void Unload();
    private void BroadcastInformation();
    private void ArrangeSort(BasePlayer player, uint id);
    private void ThisSort(BasePlayer player, uint id);
    private void NearbySort(BasePlayer player, uint id);
    private void DumpAll(BasePlayer player);
    private void LootAll(BasePlayer player);
    private List<Item> GetPlayerItems(BasePlayer player);
    private bool IsHidden(BasePlayer player);
    public static class UI
    {
        public static CuiElementContainer Container(string panelName, string color, UI4 dimensions, bool useCursor, string parent);
        public static void Panel(CuiElementContainer container, string panel, string color, UI4 dimensions, bool cursor);
        public static void Label(CuiElementContainer container, string panel, string text, int size, UI4 dimensions, TextAnchor align);
        public static void Button(CuiElementContainer container, string panel, string color, string text, int size, UI4 dimensions, string command, TextAnchor align);
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

    private void CreateSortingPanel(BasePlayer player, uint id);
    private void SelectCategories(BasePlayer player, uint id);
    private void SelectItems(BasePlayer player, uint id, int page);
    private UI4 GetPosition(int number, int columns, float xOffset, float width, float yOffset, float height, float horizontalSpacing, float verticalSpacing);
    private int ColumnNumber(int max, int count);
    [ConsoleCommand("asui.mainmenu")]
    private void ccmdMainmenu(ConsoleSystem.Arg arg);
    [ConsoleCommand("asui.opencategories")]
    private void ccmdOpenCategories(ConsoleSystem.Arg arg);
    [ConsoleCommand("asui.openitems")]
    private void ccmdOpenItems(ConsoleSystem.Arg arg);
    [ConsoleCommand("asui.selectcategory")]
    private void ccmdSelectCategory(ConsoleSystem.Arg arg);
    [ConsoleCommand("asui.selectitem")]
    private void ccmdSelectItem(ConsoleSystem.Arg arg);
    [ConsoleCommand("asui.selectitempage")]
    private void ccmdSelectItemPage(ConsoleSystem.Arg arg);
    [ConsoleCommand("asui.sort")]
    private void ccmdSort(ConsoleSystem.Arg arg);
    [ConsoleCommand("asui.destroy")]
    private void ccmdDestroy(ConsoleSystem.Arg arg);
    [ConsoleCommand("asui.hide")]
    private void ccmdHide(ConsoleSystem.Arg arg);
    [ConsoleCommand("asui.help")]
    private void ccmdHelp(ConsoleSystem.Arg arg);
    [ChatCommand("sorthelp")]
    private void cmdSortHelp(BasePlayer player, string command, string[] args);
    private void SendHelpText(BasePlayer player);
    private ConfigData configData;
    private class ConfigData
    {
        [JsonProperty(PropertyName = "Allowed containers (short prefab name)")]
        public string[] AllowedBoxes { get; set; }
        [JsonProperty(PropertyName = "Sorting radius")]
        public float SortingRadius { get; set; }
        [JsonProperty(PropertyName = "Include hotbar items when sorting")]
        public bool IncludeBelt { get; set; }
        [JsonProperty(PropertyName = "Require building privilege to use sorting")]
        public bool RequirePrivilege { get; set; }
        [JsonProperty(PropertyName = "Help notification interval (seconds, set to 0 to disable)")]
        public int NotificationInterval { get; set; }
        public Oxide.Core.VersionNumber Version { get; set; }
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    private ConfigData GetBaseConfig();
    protected override void SaveConfig();
    private void UpdateConfigValues();
    private void SaveData();
    private void LoadData();
    internal class StoredData
    {
        public Hash<uint, BoxData> boxes;
        public BoxData CreateSortingBox(uint id);
        public bool IsSortingBox(uint id, BoxData data);
        public void RemoveSortingBox(uint id);
        public class BoxData
        {
            public int categories;
            public List<string> allowedItems;
            public void AddCategory(ItemCategory category);
            public void RemoveCategory(ItemCategory category);
            public bool HasCategory(ItemCategory category);
            public bool HasAnyCategory();
            public bool AcceptsItem(ItemDefinition itemDefinition);
            public void AddItem(string shortname);
            public bool HasItem(string shortname);
            public void RemoveItem(string shortname);
        }

    }

    private string msg(string key, ulong playerId);
    private Dictionary<string, string> Messages;
}

public static class UI
{
    public static CuiElementContainer Container(string panelName, string color, UI4 dimensions, bool useCursor, string parent);
    public static void Panel(CuiElementContainer container, string panel, string color, UI4 dimensions, bool cursor);
    public static void Label(CuiElementContainer container, string panel, string text, int size, UI4 dimensions, TextAnchor align);
    public static void Button(CuiElementContainer container, string panel, string color, string text, int size, UI4 dimensions, string command, TextAnchor align);
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
    [JsonProperty(PropertyName = "Allowed containers (short prefab name)")]
    public string[] AllowedBoxes { get; set; }
    [JsonProperty(PropertyName = "Sorting radius")]
    public float SortingRadius { get; set; }
    [JsonProperty(PropertyName = "Include hotbar items when sorting")]
    public bool IncludeBelt { get; set; }
    [JsonProperty(PropertyName = "Require building privilege to use sorting")]
    public bool RequirePrivilege { get; set; }
    [JsonProperty(PropertyName = "Help notification interval (seconds, set to 0 to disable)")]
    public int NotificationInterval { get; set; }
    public Oxide.Core.VersionNumber Version { get; set; }
}

internal class StoredData
{
    public Hash<uint, BoxData> boxes;
    public BoxData CreateSortingBox(uint id);
    public bool IsSortingBox(uint id, BoxData data);
    public void RemoveSortingBox(uint id);
    public class BoxData
    {
        public int categories;
        public List<string> allowedItems;
        public void AddCategory(ItemCategory category);
        public void RemoveCategory(ItemCategory category);
        public bool HasCategory(ItemCategory category);
        public bool HasAnyCategory();
        public bool AcceptsItem(ItemDefinition itemDefinition);
        public void AddItem(string shortname);
        public bool HasItem(string shortname);
        public void RemoveItem(string shortname);
    }

}

public class BoxData
{
    public int categories;
    public List<string> allowedItems;
    public void AddCategory(ItemCategory category);
    public void RemoveCategory(ItemCategory category);
    public bool HasCategory(ItemCategory category);
    public bool HasAnyCategory();
    public bool AcceptsItem(ItemDefinition itemDefinition);
    public void AddItem(string shortname);
    public bool HasItem(string shortname);
    public void RemoveItem(string shortname);
}


```

---

## AC

```csharp
using UnityEngine;
using Newtonsoft.Json;
using Oxide.Core.Libraries;
using System.Collections.Generic;
using Time = UnityEngine.Time;
using System.Linq;

Oxide.Plugins
[Info("AntiCheat", "playermodel", "1.0.0")]
 class AC : RustPlugin
{
    public class PlayerDetect
    {
        public string SteamId { get; set; }
        public int Detects { get; set; }
        public string DetectType { get; set; }
    }

     class DetectSystem
    {
        static List<PlayerDetect> repository;
        public static int Count(string SteamId, string DetectType);
        public static bool Increase(string SteamId, string DetectType);
        public static void Drop(string SteamId, string DetectType);
    }

     class AcConfig
    {
        public string DiscordWebHook;
        public string BanReason;
        public int AntiFakeAdminInterval;
        public bool BanForBulletTeleport;
        public bool BanForInvalidDistance;
        public bool BanForFastKill;
        public bool BanForAntiAimDetectType1;
        public bool BanForWalkOnWater;
        public bool BanForIvalidHitMaterial;
        public bool BanForAutoFarm;
        public bool ShootWhileMounted;
    }

     AcConfig config;
    protected override void LoadDefaultConfig();
     void SendDetectDiscord(string content, string detectType);
     void SendProjectileLog(string content, string detectType, HitInfo info, BasePlayer attacker);
     bool IsInteger(double number);
     void Init();
     void OnServerInitialized();
     void OnWeaponFired(BaseProjectile projectile, BasePlayer player, ItemModProjectile mod, ProtoBuf.ProjectileShoot projectiles);
     object OnPlayerLand(BasePlayer player, float num);
     object OnPlayerTick(BasePlayer player, PlayerTick msg, bool wasPlayerStalled);
     object OnPlayerAttack(BasePlayer attacker, HitInfo info);
}

public class PlayerDetect
{
    public string SteamId { get; set; }
    public int Detects { get; set; }
    public string DetectType { get; set; }
}

 class DetectSystem
{
    static List<PlayerDetect> repository;
    public static int Count(string SteamId, string DetectType);
    public static bool Increase(string SteamId, string DetectType);
    public static void Drop(string SteamId, string DetectType);
}

 class AcConfig
{
    public string DiscordWebHook;
    public string BanReason;
    public int AntiFakeAdminInterval;
    public bool BanForBulletTeleport;
    public bool BanForInvalidDistance;
    public bool BanForFastKill;
    public bool BanForAntiAimDetectType1;
    public bool BanForWalkOnWater;
    public bool BanForIvalidHitMaterial;
    public bool BanForAutoFarm;
    public bool ShootWhileMounted;
}


```

---

## Achievements

```csharp
using Oxide.Game.Rust.Cui;
using System;
using System.Collections.Generic;
using System.Linq;
using Newtonsoft.Json;
using Network;
using UnityEngine;
using UnityEngine.UI;
using Oxide.Game.Rust.Cui;
using Oxide.Core;
using Oxide.Core.Configuration;
using System.IO;
using Oxide.Core.Plugins;

Oxide.Plugins
[Info("Achievments", "FADERUST", "1.0.1")]
 class Achievements : RustPlugin
{
    public class Achievs
    {
        public string name;
        public Dictionary<string, bool> dostij;
        public Dictionary<string, int> dones;
        public string uid;
    }

    public double Xmin;
    public double Xmin2;
    public double ymin;
    public double ymin2;
    public double Xbutton;
    public Dictionary<string, Achievs> achievementses;
    public double Xbutton2;
    public double ybutton;
    public double ybutton2;
    public Dictionary<ulong, Dictionary<string, int>> pagea;
    public Dictionary<string, Achievs> achievs;
     DynamicConfigFile players_File;
    [PluginReference]
     Plugin RustShop;
     Plugin Case;
    public void GiveCase(BasePlayer player, int casecount);
    public void AddBalance(ulong userid, int Amount);
    [ChatCommand("ach")]
     void CommandAchievments(BasePlayer player);
    public Dictionary<string, int> getallachivments(BasePlayer player);
     void getgiveresource(BasePlayer player, string name);
    public string getnameitem(string name);
     void OnDispenserGather(ResourceDispenser dispenser, BaseEntity entity, Item item);
    private void dostijeniecompletehud(BasePlayer player, string items);
     void OnPlayerDie(BasePlayer killed, HitInfo info);
     void OnDispenserBonus(ResourceDispenser dispenser, BasePlayer player, Item item);
    public void achievslistadd(BasePlayer player, string name, int count);
     void OnEntityDeath(BaseCombatEntity entity, HitInfo info);
    public string getcompletetext(string items);
     void OnItemCraft(ItemCraftTask item);
    [ConsoleCommand("page2open")]
     void page2(ConsoleSystem.Arg arg);
    private string gettext(string key, int count);
    private int getmax(string name);
     void OnEntityBuilt(Planner plan, GameObject go);
    [ConsoleCommand("createachivmenu")]
     void achivscreatemenu(ConsoleSystem.Arg arg);
    [ConsoleCommand("takeprize")]
     void takeprize(ConsoleSystem.Arg arg);
    public void createachievmentsmenu(BasePlayer player);
     void achievsplayeradd(BasePlayer player, Dictionary<string, bool> dostij, Dictionary<string, int> dones);
     void OnPlayerInit(BasePlayer player);
     void OnServerIntializied();
}

public class Achievs
{
    public string name;
    public Dictionary<string, bool> dostij;
    public Dictionary<string, int> dones;
    public string uid;
}


```

---

## ACode-1.0.0

```csharp
using System.Collections.Generic;
using System.Reflection;
using Oxide.Core;
using Oxide.Core.Configuration;
using UnityEngine;

Oxide.Plugins
[Info("ACode", "A0001", "1.0.0")]
[Description("Установка последнего введенного кода")]
 class ACode : RustPlugin
{
    private readonly FieldInfo hasCodeField;
     DynamicConfigFile saveFile;
     Dictionary<ulong, string> userCode;
     void LoadData();
     void OnServerSave();
     void SaveData();
     void OnServerInitialized();
     void Unload();
     object CanChangeCode(BasePlayer player, CodeLock codeLock, string newCode, bool isGuestCode);
     void OnItemDeployed(Deployer deployer, BaseEntity entity);
     Dictionary<string, string> Messages;
}


```

---

## AdminChat

```csharp
using System.Collections.Generic;
using System.Reflection;
using System;
using System.Data;
using UnityEngine;
using Oxide.Core;
using Oxide.Core.Plugins;

Oxide.Plugins
[Info("Admin Chat", "LaserHydra", "1.3.0", ResourceId = 1123)]
[Description("Chat with admins only")]
 class AdminChat : RustPlugin
{
     void Loaded();
    protected override void LoadDefaultConfig();
    [ChatCommand("a")]
     void cmdAdminChat(BasePlayer player, string cmd, string[] args);
     BasePlayer GetPlayer(string searchedPlayer, BasePlayer executer, string prefix);
     string ListToString(List<string> list);
     void BroadcastChat(string prefix, string msg);
     void SendChatMessage(BasePlayer player, string prefix, string msg);
     void SendAdminMessage(string name, string msg);
}


```

---

## AdminCheck

```csharp
using System;
using System.Collections.Generic;
using System.IO;
using System.Runtime.Remoting.Messaging;
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Plugins;

Oxide.Plugins
[Info("AdminCheck", "Frizen/Kaidoz", "1.0.0")]
public class AdminCheck : RustPlugin
{
    private List<string> permissions;
     string enhancedban;
     string enhancedkick;
    public string Token;
    public string ChatID;
    [PluginReference]
    private Plugin Vanish;
    private Configuration config;
    public class Configuration
    {
        [JsonProperty("Список SteamID которых не нужно проверять на админку")]
        public List<ulong> IgnoreList { get; set; }
        [JsonProperty("Причина кика за AdminAbuse")]
        public string AdminAbuse;
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
     void VKSendMessage(string Message);
     void OnServerInitialized();
    private object OnServerCommand(ConsoleSystem.Arg arg);
     bool hasPermission(BasePlayer player);
     bool hasPermission(BasePlayer player, string permissionName);
     void OnPlayerConnected(BasePlayer player);
     void RemoveAuth(BasePlayer player);
    public bool VanishCheck(BasePlayer player);
     void CheckAdmin(BasePlayer player);
}

public class Configuration
{
    [JsonProperty("Список SteamID которых не нужно проверять на админку")]
    public List<ulong> IgnoreList { get; set; }
    [JsonProperty("Причина кика за AdminAbuse")]
    public string AdminAbuse;
}


```

---

## AdminESP

```csharp
using Oxide.Core;
using Oxide.Game.Rust.Cui;
using System;
using System.Collections.Generic;
using System.Diagnostics.CodeAnalysis;
using System.Globalization;
using UnityEngine;

Oxide.Plugins
[Info("AdminESP", "discord.gg/9vyTXsJyKR", "0.0.11")]
public class AdminESP : RustPlugin
{
    static Dictionary<ulong, PlayerSetting> PlayerSettings;
    public class PlayerSetting
    {
        public bool Enabled;
        public float UpdateTime;
        public int PlayerDistance;
        public bool ShowAdmins;
        public bool DrawNames;
        public bool DrawBoxes;
        public bool DrawEyeLine;
        public int EyeLineDistance;
    }

    [ConsoleCommand("adminesp.toggle")]
     void cmdConsoleEnabledESP(ConsoleSystem.Arg args);
    [ConsoleCommand("adminespUI_toggle")]
     void cmdConsoleEnabledESPUI(ConsoleSystem.Arg args);
    [ChatCommand("ae")]
     void cmdAdminESP(BasePlayer player, string command, string[] args);
    static AdminESP ins;
     string AdminPermission;
     void OnServerInitialized();
     void OnPlayerConnected(BasePlayer player);
    public List<BasePlayer> statsUIDisabled;
    public static string Main;
     void CrateButtonMenu(BasePlayer player);
    [ConsoleCommand("adminEsp_mainmenu")]
     void cmdMainMenuESP(ConsoleSystem.Arg args);
     void CrateMainMenu(BasePlayer player);
     void OnPlayerDisconected(BasePlayer player);
    static void DestroyAll();
     void LoadData();
     void DestroyUI(BasePlayer player);
     void Unload();
     void SaveData();
     class ESPPlayer : BaseEntity
    {
         BasePlayer player;
         PlayerSetting data;
        public float LastUpdate;
         void Awake();
        public void Init(PlayerSetting dates);
         void FixedUpdate();
         void DDraw(string type, BasePlayer target, string messages);
         void SetPlayerFlag(BasePlayer player, BasePlayer.PlayerFlags f, bool b);
        public void DestroyComponent();
         void OnDestroy();
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

    [SuppressMessage("ReSharper", "CompareOfFloatsByEqualityOperator")]
    private static List<Position> GetPositions(int colums, int rows, float colPadding, float rowPadding, bool columsFirst, ulong playerid);
    public string ParseColorFromRGBA(string cssColor);
}

public class PlayerSetting
{
    public bool Enabled;
    public float UpdateTime;
    public int PlayerDistance;
    public bool ShowAdmins;
    public bool DrawNames;
    public bool DrawBoxes;
    public bool DrawEyeLine;
    public int EyeLineDistance;
}

 class ESPPlayer : BaseEntity
{
     BasePlayer player;
     PlayerSetting data;
    public float LastUpdate;
     void Awake();
    public void Init(PlayerSetting dates);
     void FixedUpdate();
     void DDraw(string type, BasePlayer target, string messages);
     void SetPlayerFlag(BasePlayer player, BasePlayer.PlayerFlags f, bool b);
    public void DestroyComponent();
     void OnDestroy();
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

## AdminLogger

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

## AdminMenu

```csharp
using System;
using System.Collections.Generic;
using System.Globalization;
using Newtonsoft.Json;
using Oxide.Core.Libraries.Covalence;
using Oxide.Game.Rust.Cui;
using Oxide.Core;
using Oxide.Core.Configuration;
using Oxide.Core.Plugins;
using UnityEngine;
using System.Linq;

Oxide.Plugins
[Info("AdminMenu", "k1lly0u", "0.1.30", ResourceId = 0)]
 class AdminMenu : RustPlugin
{
    private StoredData storedData;
    private DynamicConfigFile data;
    private static AdminMenu ins;
    private Dictionary<string, string> uiColors;
    private Dictionary<ItemType, List<KeyValuePair<string, ItemDefinition>>> itemList;
    private Dictionary<ulong, SelectionData> selectData;
    private Dictionary<ulong, GroupData> groupCreator;
    private Hash<ulong, Timer> popupTimers;
    private string[] charFilter;
    private List<KeyValuePair<string, bool>> permissionList;
    private class SelectionData
    {
        public MenuType menuType;
        public string subType;
        public string selectDesc;
        public string returnCommand;
        public string target1_Name;
        public string target1_Id;
        public string target2_Name;
        public string target2_Id;
        public string character;
        public bool requireTarget1;
        public bool requireTarget2;
        public bool isOnline;
        public bool isGroup;
        public bool forceOnline;
        public int pageNum;
        public int listNum;
    }

    private class GroupData
    {
        public string name;
        public string title;
        public string rank;
    }

    private void Loaded();
    private void OnServerInitialized();
    private void OnPlayerInit(BasePlayer player);
    private void OnPlayerDisconnected(BasePlayer player);
    private void OnPermissionRegistered(string name, Plugin owner);
    private void OnPluginUnloaded(Plugin plugin);
    private void OnServerSave();
    private void Unload();
    public class UI
    {
        static public CuiElementContainer Container(string panelName, string color, string aMin, string aMax, bool useCursor, string parent);
        static public void Panel(CuiElementContainer container, string panel, string color, string aMin, string aMax, bool cursor);
        static public void Label(CuiElementContainer container, string panel, string text, int size, string aMin, string aMax, TextAnchor align);
        static public void Button(CuiElementContainer container, string panel, string color, string text, int size, string aMin, string aMax, string command, TextAnchor align);
        static public void Input(CuiElementContainer container, string panel, string color, string text, int size, string command, string aMin, string aMax);
        static public string Color(string hexColor, float alpha);
    }

    const string UIMain;
    const string UIElement;
    const string UIPopup;
    private void OpenAdminMenu(BasePlayer player);
    private void CreateMenuButtons(CuiElementContainer container, MenuType menuType, string playerId);
    private void CreateSubMenu(CuiElementContainer container, MenuType menuType, string playerId, string subType);
    private void CreateMenuPermissions(BasePlayer player, int page, string filter);
    private void CreateMenuGroups(BasePlayer player, GroupSub subType, int page, string filter);
    private void CreateMenuCommands(BasePlayer player, CommSub subType, int page, ItemType itemType);
    private void CreateCommandEntry(CuiElementContainer container, CommSub subType, int page, string playerId);
    private void CreateGiveMenu(CuiElementContainer container, ItemType itemType, int page, string playerId);
    private void OpenSelectionMenu(BasePlayer player, SelectType selectType, object objList, bool sortList);
    private void OpenPermissionMenu(BasePlayer player, string groupOrUserId, string playerName, string description, int page, string filter);
    private void OpenGroupMenu(BasePlayer player, string userId, string userName, string description, int page);
    private void CreateCharacterFilter(CuiElementContainer container, ulong playerId, string currentCharacter, string returnCommand);
    private float[] CalculateButtonPos(int number);
    private float[] CalculateButtonPosVert(int number);
    private float[] CalculateItemPos(int number);
    private string StripTags(string str);
    private void PopupMessage(BasePlayer player, string message);
    [ConsoleCommand("amui.runcommand")]
    private void ccmdUIRunCommand(ConsoleSystem.Arg arg);
    [ConsoleCommand("amui.filterchar")]
    private void ccmdFilterChar(ConsoleSystem.Arg arg);
    [ConsoleCommand("amui.registergroup")]
    private void ccmdRegisterGroup(ConsoleSystem.Arg arg);
    [ConsoleCommand("amui.selectforpermission")]
    private void ccmdSelectPermission(ConsoleSystem.Arg arg);
    [ConsoleCommand("amui.selectforgroup")]
    private void ccmdSelectGroup(ConsoleSystem.Arg arg);
    [ConsoleCommand("amui.selectremovegroup")]
    private void ccmdSelectRemoveGroup(ConsoleSystem.Arg arg);
    [ConsoleCommand("amui.togglepermission")]
    private void ccmdTogglePermission(ConsoleSystem.Arg arg);
    [ConsoleCommand("amui.togglegroup")]
    private void ccmdToggleGroup(ConsoleSystem.Arg arg);
    [ConsoleCommand("amui.makeselection")]
    private void ccmdMakeSelection(ConsoleSystem.Arg arg);
    [ConsoleCommand("amui.switchelement")]
    private void ccmdUISwitch(ConsoleSystem.Arg arg);
    [ConsoleCommand("amui.giveitem")]
    private void ccmdGiveItem(ConsoleSystem.Arg arg);
    private void SetUIColors();
    private List<string> GetGroups();
    private bool CreateGroup(string name, string title, int rank);
    private void RemoveGroup(string name);
    private void AddToGroup(string userId, string groupId);
    private void RemoveFromGroup(string userId, string groupId);
    private bool HasGroup(string userId, string groupId);
    private List<string> GetPermissions();
    private void GrantPermission(string groupOrID, string perm, bool isGroup);
    private void RevokePermission(string groupOrID, string perm, bool isGroup);
    private bool HasPermission(string groupOrID, string perm, bool isGroup);
    private void DestroyUI(BasePlayer player);
    private T ParseType(string type);
    private bool IsDivisable(int number);
    private void UpdatePermissionList();
    [ChatCommand("admin")]
    private void cmdAdmin(BasePlayer player, string command, string[] args);
    private ConfigData configData;
    private class Colors
    {
        [JsonProperty(PropertyName = "Panel - Dark")]
        public UIColor Panel1 { get; set; }
        [JsonProperty(PropertyName = "Panel - Medium")]
        public UIColor Panel2 { get; set; }
        [JsonProperty(PropertyName = "Panel - Light")]
        public UIColor Panel3 { get; set; }
        [JsonProperty(PropertyName = "Button - Primary")]
        public UIColor Button1 { get; set; }
        [JsonProperty(PropertyName = "Button - Secondary")]
        public UIColor Button2 { get; set; }
        [JsonProperty(PropertyName = "Button - Selected")]
        public UIColor Button3 { get; set; }
        public class UIColor
        {
            public string Color { get; set; }
            public float Alpha { get; set; }
        }

    }

    private class CommandEntry
    {
        public string Name { get; set; }
        public string Command { get; set; }
        public string Description { get; set; }
    }

    private class ConfigData
    {
        public Colors Colors { get; set; }
        [JsonProperty(PropertyName = "Chat Command List")]
        public List<CommandEntry> ChatCommands { get; set; }
        [JsonProperty(PropertyName = "Console Command List")]
        public List<CommandEntry> ConsoleCommands { get; set; }
    }

    private void LoadVariables();
    protected override void LoadDefaultConfig();
    private void LoadConfigVariables();
    private void SaveConfig(ConfigData config);
    private void SaveData();
    private void LoadData();
    private class StoredData
    {
        public Hash<string, double> offlinePlayers;
        public void AddOfflinePlayer(string userId);
        public void OnPlayerInit(string userId);
        public void RemoveOldPlayers();
        public List<IPlayer> GetOfflineList();
        public double CurrentTime();
    }

    private string msg(string key, string playerId);
     Dictionary<string, string> Messages;
}

private class SelectionData
{
    public MenuType menuType;
    public string subType;
    public string selectDesc;
    public string returnCommand;
    public string target1_Name;
    public string target1_Id;
    public string target2_Name;
    public string target2_Id;
    public string character;
    public bool requireTarget1;
    public bool requireTarget2;
    public bool isOnline;
    public bool isGroup;
    public bool forceOnline;
    public int pageNum;
    public int listNum;
}

private class GroupData
{
    public string name;
    public string title;
    public string rank;
}

public class UI
{
    static public CuiElementContainer Container(string panelName, string color, string aMin, string aMax, bool useCursor, string parent);
    static public void Panel(CuiElementContainer container, string panel, string color, string aMin, string aMax, bool cursor);
    static public void Label(CuiElementContainer container, string panel, string text, int size, string aMin, string aMax, TextAnchor align);
    static public void Button(CuiElementContainer container, string panel, string color, string text, int size, string aMin, string aMax, string command, TextAnchor align);
    static public void Input(CuiElementContainer container, string panel, string color, string text, int size, string command, string aMin, string aMax);
    static public string Color(string hexColor, float alpha);
}

private class Colors
{
    [JsonProperty(PropertyName = "Panel - Dark")]
    public UIColor Panel1 { get; set; }
    [JsonProperty(PropertyName = "Panel - Medium")]
    public UIColor Panel2 { get; set; }
    [JsonProperty(PropertyName = "Panel - Light")]
    public UIColor Panel3 { get; set; }
    [JsonProperty(PropertyName = "Button - Primary")]
    public UIColor Button1 { get; set; }
    [JsonProperty(PropertyName = "Button - Secondary")]
    public UIColor Button2 { get; set; }
    [JsonProperty(PropertyName = "Button - Selected")]
    public UIColor Button3 { get; set; }
    public class UIColor
    {
        public string Color { get; set; }
        public float Alpha { get; set; }
    }

}

public class UIColor
{
    public string Color { get; set; }
    public float Alpha { get; set; }
}

private class CommandEntry
{
    public string Name { get; set; }
    public string Command { get; set; }
    public string Description { get; set; }
}

private class ConfigData
{
    public Colors Colors { get; set; }
    [JsonProperty(PropertyName = "Chat Command List")]
    public List<CommandEntry> ChatCommands { get; set; }
    [JsonProperty(PropertyName = "Console Command List")]
    public List<CommandEntry> ConsoleCommands { get; set; }
}

private class StoredData
{
    public Hash<string, double> offlinePlayers;
    public void AddOfflinePlayer(string userId);
    public void OnPlayerInit(string userId);
    public void RemoveOldPlayers();
    public List<IPlayer> GetOfflineList();
    public double CurrentTime();
}


```

---

## AdminMenu (1)

```csharp
using System;
using System.Collections.Generic;
using System.Globalization;
using Newtonsoft.Json;
using Oxide.Core.Libraries.Covalence;
using Oxide.Game.Rust.Cui;
using Oxide.Core;
using Oxide.Core.Configuration;
using Oxide.Core.Plugins;
using UnityEngine;
using System.Linq;

Oxide.Plugins
[Info("AdminMenu", "k1lly0u", "0.1.32", ResourceId = 0)]
 class AdminMenu : RustPlugin
{
    private StoredData storedData;
    private DynamicConfigFile data;
    private static AdminMenu ins;
    private Dictionary<string, string> uiColors;
    private Dictionary<ItemType, List<KeyValuePair<string, ItemDefinition>>> itemList;
    private Dictionary<ulong, SelectionData> selectData;
    private Dictionary<ulong, GroupData> groupCreator;
    private Hash<ulong, Timer> popupTimers;
    private string[] charFilter;
    private List<KeyValuePair<string, bool>> permissionList;
    private class SelectionData
    {
        public MenuType menuType;
        public string subType;
        public string selectDesc;
        public string returnCommand;
        public string target1_Name;
        public string target1_Id;
        public string target2_Name;
        public string target2_Id;
        public string character;
        public bool requireTarget1;
        public bool requireTarget2;
        public bool isOnline;
        public bool isGroup;
        public bool forceOnline;
        public int pageNum;
        public int listNum;
    }

    private class GroupData
    {
        public string name;
        public string title;
        public string rank;
    }

    private void Loaded();
    private void OnServerInitialized();
    private void OnPlayerInit(BasePlayer player);
    private void OnPlayerDisconnected(BasePlayer player);
    private void OnPermissionRegistered(string name, Plugin owner);
    private void OnPluginUnloaded(Plugin plugin);
    private void OnServerSave();
    private void Unload();
    public class UI
    {
        static public CuiElementContainer Container(string panelName, string color, string aMin, string aMax, bool useCursor, string parent);
        static public void Panel(CuiElementContainer container, string panel, string color, string aMin, string aMax, bool cursor);
        static public void Label(CuiElementContainer container, string panel, string text, int size, string aMin, string aMax, TextAnchor align);
        static public void Button(CuiElementContainer container, string panel, string color, string text, int size, string aMin, string aMax, string command, TextAnchor align);
        static public void Input(CuiElementContainer container, string panel, string color, string text, int size, string command, string aMin, string aMax);
        static public string Color(string hexColor, float alpha);
    }

    const string UIMain;
    const string UIElement;
    const string UIPopup;
    private void OpenAdminMenu(BasePlayer player);
    private void CreateMenuButtons(CuiElementContainer container, MenuType menuType, string playerId);
    private void CreateSubMenu(CuiElementContainer container, MenuType menuType, string playerId, string subType);
    private void CreateMenuPermissions(BasePlayer player, int page, string filter);
    private void CreateMenuGroups(BasePlayer player, GroupSub subType, int page, string filter);
    private void CreateMenuCommands(BasePlayer player, CommSub subType, int page, ItemType itemType);
    private void CreateCommandEntry(CuiElementContainer container, CommSub subType, int page, string playerId);
    private void CreateGiveMenu(CuiElementContainer container, ItemType itemType, int page, string playerId);
    private void OpenSelectionMenu(BasePlayer player, SelectType selectType, object objList, bool sortList);
    private void OpenPermissionMenu(BasePlayer player, string groupOrUserId, string playerName, string description, int page, string filter);
    private void OpenGroupMenu(BasePlayer player, string userId, string userName, string description, int page);
    private void CreateCharacterFilter(CuiElementContainer container, ulong playerId, string currentCharacter, string returnCommand);
    private float[] CalculateButtonPos(int number);
    private float[] CalculateButtonPosVert(int number);
    private float[] CalculateItemPos(int number);
    private string StripTags(string str);
    private void PopupMessage(BasePlayer player, string message);
    [ConsoleCommand("amui.runcommand")]
    private void ccmdUIRunCommand(ConsoleSystem.Arg arg);
    [ConsoleCommand("amui.filterchar")]
    private void ccmdFilterChar(ConsoleSystem.Arg arg);
    [ConsoleCommand("amui.registergroup")]
    private void ccmdRegisterGroup(ConsoleSystem.Arg arg);
    [ConsoleCommand("amui.selectforpermission")]
    private void ccmdSelectPermission(ConsoleSystem.Arg arg);
    [ConsoleCommand("amui.selectforgroup")]
    private void ccmdSelectGroup(ConsoleSystem.Arg arg);
    [ConsoleCommand("amui.selectremovegroup")]
    private void ccmdSelectRemoveGroup(ConsoleSystem.Arg arg);
    [ConsoleCommand("amui.togglepermission")]
    private void ccmdTogglePermission(ConsoleSystem.Arg arg);
    [ConsoleCommand("amui.togglegroup")]
    private void ccmdToggleGroup(ConsoleSystem.Arg arg);
    [ConsoleCommand("amui.makeselection")]
    private void ccmdMakeSelection(ConsoleSystem.Arg arg);
    [ConsoleCommand("amui.switchelement")]
    private void ccmdUISwitch(ConsoleSystem.Arg arg);
    [ConsoleCommand("amui.giveitem")]
    private void ccmdGiveItem(ConsoleSystem.Arg arg);
    private void SetUIColors();
    private List<string> GetGroups();
    private bool CreateGroup(string name, string title, int rank);
    private void RemoveGroup(string name);
    private void AddToGroup(string userId, string groupId);
    private void RemoveFromGroup(string userId, string groupId);
    private bool HasGroup(string userId, string groupId);
    private List<string> GetPermissions();
    private void GrantPermission(string groupOrID, string perm, bool isGroup);
    private void RevokePermission(string groupOrID, string perm, bool isGroup);
    private bool HasPermission(string groupOrID, string perm, bool isGroup);
    private void DestroyUI(BasePlayer player);
    private T ParseType(string type);
    private bool IsDivisable(int number);
    private void UpdatePermissionList();
    [ChatCommand("admin")]
    private void cmdAdmin(BasePlayer player, string command, string[] args);
    private ConfigData configData;
    private class Colors
    {
        [JsonProperty(PropertyName = "Panel - Dark")]
        public UIColor Panel1 { get; set; }
        [JsonProperty(PropertyName = "Panel - Medium")]
        public UIColor Panel2 { get; set; }
        [JsonProperty(PropertyName = "Panel - Light")]
        public UIColor Panel3 { get; set; }
        [JsonProperty(PropertyName = "Button - Primary")]
        public UIColor Button1 { get; set; }
        [JsonProperty(PropertyName = "Button - Secondary")]
        public UIColor Button2 { get; set; }
        [JsonProperty(PropertyName = "Button - Selected")]
        public UIColor Button3 { get; set; }
        public class UIColor
        {
            public string Color { get; set; }
            public float Alpha { get; set; }
        }

    }

    private class CommandEntry
    {
        public string Name { get; set; }
        public string Command { get; set; }
        public string Description { get; set; }
    }

    private class ConfigData
    {
        public Colors Colors { get; set; }
        [JsonProperty(PropertyName = "Chat Command List")]
        public List<CommandEntry> ChatCommands { get; set; }
        [JsonProperty(PropertyName = "Console Command List")]
        public List<CommandEntry> ConsoleCommands { get; set; }
    }

    private void LoadVariables();
    protected override void LoadDefaultConfig();
    private void LoadConfigVariables();
    private void SaveConfig(ConfigData config);
    private void SaveData();
    private void LoadData();
    private class StoredData
    {
        public Hash<string, double> offlinePlayers;
        public void AddOfflinePlayer(string userId);
        public void OnPlayerInit(string userId);
        public void RemoveOldPlayers();
        public List<IPlayer> GetOfflineList();
        public double CurrentTime();
    }

    private string msg(string key, string playerId);
     Dictionary<string, string> Messages;
}

private class SelectionData
{
    public MenuType menuType;
    public string subType;
    public string selectDesc;
    public string returnCommand;
    public string target1_Name;
    public string target1_Id;
    public string target2_Name;
    public string target2_Id;
    public string character;
    public bool requireTarget1;
    public bool requireTarget2;
    public bool isOnline;
    public bool isGroup;
    public bool forceOnline;
    public int pageNum;
    public int listNum;
}

private class GroupData
{
    public string name;
    public string title;
    public string rank;
}

public class UI
{
    static public CuiElementContainer Container(string panelName, string color, string aMin, string aMax, bool useCursor, string parent);
    static public void Panel(CuiElementContainer container, string panel, string color, string aMin, string aMax, bool cursor);
    static public void Label(CuiElementContainer container, string panel, string text, int size, string aMin, string aMax, TextAnchor align);
    static public void Button(CuiElementContainer container, string panel, string color, string text, int size, string aMin, string aMax, string command, TextAnchor align);
    static public void Input(CuiElementContainer container, string panel, string color, string text, int size, string command, string aMin, string aMax);
    static public string Color(string hexColor, float alpha);
}

private class Colors
{
    [JsonProperty(PropertyName = "Panel - Dark")]
    public UIColor Panel1 { get; set; }
    [JsonProperty(PropertyName = "Panel - Medium")]
    public UIColor Panel2 { get; set; }
    [JsonProperty(PropertyName = "Panel - Light")]
    public UIColor Panel3 { get; set; }
    [JsonProperty(PropertyName = "Button - Primary")]
    public UIColor Button1 { get; set; }
    [JsonProperty(PropertyName = "Button - Secondary")]
    public UIColor Button2 { get; set; }
    [JsonProperty(PropertyName = "Button - Selected")]
    public UIColor Button3 { get; set; }
    public class UIColor
    {
        public string Color { get; set; }
        public float Alpha { get; set; }
    }

}

public class UIColor
{
    public string Color { get; set; }
    public float Alpha { get; set; }
}

private class CommandEntry
{
    public string Name { get; set; }
    public string Command { get; set; }
    public string Description { get; set; }
}

private class ConfigData
{
    public Colors Colors { get; set; }
    [JsonProperty(PropertyName = "Chat Command List")]
    public List<CommandEntry> ChatCommands { get; set; }
    [JsonProperty(PropertyName = "Console Command List")]
    public List<CommandEntry> ConsoleCommands { get; set; }
}

private class StoredData
{
    public Hash<string, double> offlinePlayers;
    public void AddOfflinePlayer(string userId);
    public void OnPlayerInit(string userId);
    public void RemoveOldPlayers();
    public List<IPlayer> GetOfflineList();
    public double CurrentTime();
}


```

---

## AdminMenu-1.1.11

```csharp
using HarmonyLib;
using Oxide.Core.Plugins;
using System;
using System.Collections.Generic;
using System.Reflection;
using Network;
using Newtonsoft.Json;
using System.Collections;
using System.ComponentModel;
using System.Linq;
using UnityEngine;
using Oxide.Game.Rust.Cui;
using UnityEngine.UI;
using Facepunch;
using Oxide.Core;
using Oxide.Core.Libraries.Covalence;
using Rust;
using System.IO;
using System.Text.RegularExpressions;
using Oxide.Core.Libraries;
using Oxide.Game.Rust.Libraries;
using System.Xml;
using System.Text;
using System.Globalization;
using Newtonsoft.Json.Linq;

Oxide.Plugins
[Info("AdminMenu", "0xF // dsc.gg/0xf-plugins", "1.1.11")]
[Description("Multifunctional in-game admin menu.")]
public class AdminMenu : RustPlugin
{
    [PluginReference]
    private Plugin ImageLibrary;
    private Plugin Economics;
    private Plugin ServerRewards;
    private Plugin Clans;
    private const string PERMISSION_USE;
    private const string PERMISSION_FULLACCESS;
    private const string PERMISSION_CONVARS;
    private const string PERMISSION_PERMISSIONMANAGER;
    private const string PERMISSION_PLUGINMANAGER;
    private const string PERMISSION_GIVE;
    private const string PERMISSION_USERINFO_IP;
    private const string PERMISSION_USERINFO_STEAMINFO;
    private const string ADMINMENU_IMAGEBASE64;
    private static Dictionary<string, string> HEADERS;
    private static FieldInfo PERMISSIONS_DICTIONARY_FIELD;
    private static FieldInfo CONSOLECOMMANDS_DICTIONARY_FIELD;
    private static FieldInfo CONSOLECOMMAND_CALLBACK_FIELD;
    private static FieldInfo PLUGINCALLBACK_PLUGIN_FIELD;
    private static FieldInfo CHATCOMMANDS_DICTIONARY_FIELD;
    private static FieldInfo CHATCOMMAND_PLUGIN_FIELD;
    private static AdminMenu Instance;
    private static Dictionary<string, Panel> panelList;
    private static Dictionary<ulong, SteamInfo> cachedSteamInfo;
    private static string ADMINMENU_IMAGECRC;
    private MainMenu mainMenu;
    private Dictionary<string, string> defaultLang;
    private Dictionary<PlayerLoot, Item> viewingBackpacks;
    static Configuration config;
    public AdminMenu();
    public class ButtonArray : List<T>
    {
        public ButtonArray();
        public ButtonArray(IEnumerable<T> collection);
        public IEnumerable<T> GetAllowedButtons(Connection connection);
        public IEnumerable<T> GetAllowedButtons(string userId);
    }

    public class ButtonGrid : List<ButtonGrid<T>.Item>
    {
        public class Item
        {
            public int row;
            public int column;
            public T button;
            public Item(int row, int column, T button);
        }

        public IEnumerable<Item> GetAllowedButtons(Connection connection);
        public IEnumerable<Item> GetAllowedButtons(string userId);
    }

    public class ButtonArray : ButtonArray<Button>
    {
    }

    public class Button
    {
        private string label;
        private string permission;
        public string Command { get; set; }
        public string[] Args { get; set; }
        public Label Label { get; set; }
        public virtual int FontSize { get; set; }
        public ButtonStyle Style { get; set; }
        public string Permission { get; set; }
        public string FullCommand { get; set; }
        public Button();
        public Button(string label, string command, string[] args);
        public virtual Button.State GetState(ConnectionData connectionData);
        public virtual void SetState(ConnectionData connectionData, Button.State state);
        public bool UserHasPermission(Connection connection);
        public bool UserHasPermission(string userId);
        public virtual bool IsPressed(ConnectionData connectionData);
        public virtual bool IsHidden(ConnectionData connectionData);
        internal static Dictionary<string, Button> all;
    }

    public class ButtonStyle : ICloneable
    {
        public string BackgroundColor { get; set; }
        public string ActiveBackgroundColor { get; set; }
        public string TextColor { get; set; }
        public object Clone();
        public static ButtonStyle Default { get; set; }
    }

    public class CategoryButton : Button
    {
        public override int FontSize { get; set; }
        public CategoryButton(string label, string command, string[] args);
    }

    public class HideButton : Button
    {
        public HideButton(string label, string command, string[] args);
        public override bool IsHidden(ConnectionData connectionData);
    }

    public class ToggleButton : Button
    {
        public ToggleButton(string label, string command, string[] args);
        public virtual void Toggle(ConnectionData connectionData);
    }

    public class ConditionToggleButton : ToggleButton
    {
        public Func<ConnectionData, bool> Condition { get; set; }
        public ConditionToggleButton(string label, string command, string[] args);
        public override State GetState(ConnectionData connectionData);
        public override void Toggle(ConnectionData connectionData);
    }

    [JsonObject(MemberSerialization.OptIn)]
    public class BaseCustomButton
    {
        [JsonProperty]
        public string Label { get; set; }
        [JsonProperty("Execution as server")]
        public bool ExecutionAsServer { get; set; }
        [JsonProperty("Commands")]
        public string[] Commands { get; set; }
        [JsonProperty("Commands for Toggled state")]
        public string[] ToggledStateCommands { get; set; }
        [JsonProperty]
        public string Permission { get; set; }
        [JsonProperty]
        public ButtonStyle Style { get; set; }
        [JsonProperty]
        public int[] Position { get; set; }
        protected virtual string BaseCommand { get; set; }
        private Button _button;
        public Button Button { get; set; }
        private Button GetButton();
    }

    public class QMCustomButton : BaseCustomButton
    {
        protected override string BaseCommand { get; set; }
        [JsonProperty("Bulk sending of command to each player. Available values: None, Online, Offline, Everyone")]
        [JsonConverter(typeof(Newtonsoft.Json.Converters.StringEnumConverter))]
        public Recievers PlayerReceivers { get; set; }
    }

    public class UserInfoCustomButton : BaseCustomButton
    {
        protected override string BaseCommand { get; set; }
    }

    public class Configuration
    {
        [JsonProperty(PropertyName = "Text under the ADMIN MENU")]
        public string Subtext { get; set; }
        [JsonProperty(PropertyName = "Button to hook (X | F | OFF)")]
        [JsonConverter(typeof(Newtonsoft.Json.Converters.StringEnumConverter))]
        public ButtonHook ButtonToHook { get; set; }
        [JsonProperty(PropertyName = "Chat command to show admin menu")]
        public string ChatCommand { get; set; }
        [JsonProperty(PropertyName = "Theme")]
        [JsonConverter(typeof(ThemeConverter))]
        public Theme Theme { get; set; }
        [JsonProperty(PropertyName = "Custom Quick Buttons")]
        public List<QMCustomButton> CustomQuickButtons { get; set; }
        [JsonProperty(PropertyName = "User Custom Buttons")]
        public List<UserInfoCustomButton> UserInfoCustomButtons { get; set; }
        [JsonProperty(PropertyName = "Give menu item presets (add your custom items for easy give)")]
        public List<ItemPreset> GiveItemPresets { get; set; }
        [JsonProperty(PropertyName = "Favorite Plugins")]
        public HashSet<string> FavoritePlugins { get; set; }
        [JsonProperty(PropertyName = "Logs Properties")]
        public LogsProperties Logs { get; set; }
        [JsonIgnore]
        public Dictionary<int, string[]> HashedCommands { get; set; }
        public static Configuration DefaultConfig();
        public class ItemPreset
        {
            [JsonProperty(PropertyName = "Short Name")]
            public string ShortName { get; set; }
            [JsonProperty(PropertyName = "Skin Id")]
            public ulong SkinId { get; set; }
            [JsonProperty(PropertyName = "Name")]
            public string Name { get; set; }
            [JsonProperty(PropertyName = "Category Name")]
            public string Category { get; set; }
            [JsonIgnore]
            public static ItemPreset Example { get; set; }
        }

        public class LogsProperties
        {
            [JsonProperty(PropertyName = "Discord Webhook URL for logs (https://support.discord.com/hc/en-us/articles/228383668-Intro-to-Webhooks)")]
            public string WebhookURL { get; set; }
            [JsonProperty(PropertyName = "Give")]
            public bool Give { get; set; }
            [JsonProperty(PropertyName = "Admin teleports")]
            public bool AdminTeleport { get; set; }
            [JsonProperty(PropertyName = "Spectate")]
            public bool Spectate { get; set; }
            [JsonProperty(PropertyName = "Heal")]
            public bool Heal { get; set; }
            [JsonProperty(PropertyName = "Kill")]
            public bool Kill { get; set; }
            [JsonProperty(PropertyName = "Look inventory")]
            public bool LookInventory { get; set; }
            [JsonProperty(PropertyName = "Strip inventory")]
            public bool StripInventory { get; set; }
            [JsonProperty(PropertyName = "Blueprints")]
            public bool Blueprints { get; set; }
            [JsonProperty(PropertyName = "Mute/Unmute")]
            public bool MuteUnmute { get; set; }
            [JsonProperty(PropertyName = "Toggle Creative")]
            public bool ToggleCreative { get; set; }
            [JsonProperty(PropertyName = "Cuff")]
            public bool Cuff { get; set; }
            [JsonProperty(PropertyName = "Kick the player")]
            public bool Kick { get; set; }
            [JsonProperty(PropertyName = "Ban the player")]
            public bool Ban { get; set; }
            [JsonProperty(PropertyName = "Using custom buttons")]
            public bool CustomButtons { get; set; }
            [JsonProperty(PropertyName = "Spawn entities")]
            public bool SpawnEntities { get; set; }
            [JsonProperty(PropertyName = "Set time")]
            public bool SetTime { get; set; }
            [JsonProperty(PropertyName = "ConVars")]
            public bool ConVars { get; set; }
            [JsonProperty(PropertyName = "Plugin Manager")]
            public bool PluginManager { get; set; }
        }

    }

    public class ConnectionData
    {
        public Connection connection;
        public MainMenu currentMainMenu;
        public Panel currentPanel;
        public Sidebar currentSidebar;
        public Content currentContent;
        public Translator15 translator;
        public Dictionary<string, object> userData;
        public ConnectionData(BasePlayer player);
        public ConnectionData(Connection connection);
        public ConnectionUI UI { get; set; }
        public bool IsAdminMenuDisplay { get; set; }
        public bool IsDestroyed { get; set; }
        public void Init();
        public void ShowAdminMenu();
        public void HideAdminMenu();
        public ConnectionData OpenPanel(string panelName);
        public void SetSidebar(Sidebar sidebar);
        public void ShowPanelContent(Content content);
        public void ShowPanelContent(string contentId);
        public void Dispose();
        public static Dictionary<Connection, ConnectionData> all;
        public static ConnectionData Get(Connection connection);
        public static ConnectionData Get(BasePlayer player);
        public static ConnectionData GetOrCreate(Connection connection);
        public static ConnectionData GetOrCreate(BasePlayer player);
    }

    public class ConnectionUI
    {
         Connection connection;
         ConnectionData connectionData;
        public ConnectionUI(ConnectionData connectionData);
        public void ShowAdminMenu();
        public void HideAdminMenu();
        public void DestroyAdminMenu();
        public void DestroyAll();
        public void AddSidebar(CUI.Element element, Sidebar sidebar);
        private void AddNavButtons(CUI.Element element, MainMenu mainMenu);
        public void UpdateNavButtons(MainMenu mainMenu);
        public void RenderOverlayOpenButton();
        public void RenderMainMenu(MainMenu mainMenu);
        public void RenderPanel(Panel panel);
        public void RenderContent(Content content);
    }

    public static class Extensions
    {
        public static string ToCuiString(Color color);
    }

    [HarmonyPatch(typeof(BasePlayer), "Tick_Spectator")]
    private static class SpectatorStaff
    {
        private static bool Prefix(BasePlayer __instance);
    }

    public class Label
    {
        private static readonly Regex richTextRegex;
         string label;
         string langKey;
        public Label(string label);
        public override string ToString();
        public string Localize(string userId);
        public string Localize(Connection connection);
    }

    public class MainMenu
    {
        public ButtonArray NavButtons { get; set; }
    }

    public class Panel
    {
        public virtual Sidebar Sidebar { get; set; }
        public virtual Dictionary<string, Content> Content { get; set; }
        public Content DefaultContent { get; set; }
        public Content TryGetContent(string id);
        public virtual void OnOpen(ConnectionData connectionData);
        public virtual void OnClose(ConnectionData connectionData);
    }

    public class UserInfoPanel : Panel
    {
    }

    public class PermissionPanel : Panel
    {
        public override Sidebar Sidebar { get; set; }
        public override void OnClose(ConnectionData connectionData);
    }

    public class Sidebar
    {
        public ButtonArray<CategoryButton> CategoryButtons { get; set; }
        public int? AutoActivateCategoryButtonIndex { get; set; }
    }

    public class SteamInfo
    {
        public string Location { get; set; }
        public string[] Avatars { get; set; }
        public string RegistrationDate { get; set; }
        public string RustHours { get; set; }
        public override string ToString();
    }

    public class Translator15
    {
        public static void Init(string language);
        private static Dictionary<string, Dictionary<string, string>> translations;
        public class Phrase : Translate.Phrase
        {
            private Translator15 translator;
            public Phrase(Translator15 translator);
            public override string translated { get; set; }
        }

        protected ulong userID;
        public Translator15(ulong userID);
        public Translator15.Phrase Convert(Translate.Phrase phrase);
        private string GetLanguage();
        public string Get(string key, string def);
        private Dictionary<Translate.Phrase, Translator15.Phrase> phraseCache;
    }

    public class CUI
    {
        private static readonly Dictionary<Font, string> FontToString;
        private static readonly Dictionary<TextAnchor, string> TextAnchorToString;
        private static readonly Dictionary<VerticalWrapMode, string> VWMToString;
        private static readonly Dictionary<Image.Type, string> ImageTypeToString;
        private static readonly Dictionary<InputField.LineType, string> LineTypeToString;
        private static readonly Dictionary<ScrollRect.MovementType, string> MovementTypeToString;
        public static class Defaults
        {
            public const string VectorZero;
            public const string VectorOne;
            public const string Color;
            public const string OutlineColor;
            public const string Sprite;
            public const string Material;
            public const string IconMaterial;
            public const Image.Type ImageType;
            public const CUI.Font Font;
            public const int FontSize;
            public const TextAnchor Align;
            public const VerticalWrapMode VerticalOverflow;
            public const InputField.LineType LineType;
        }

        public static Color GetColor(string colorStr);
        public static string GetColorString(Color color);
        public static void AddUI(Connection connection, string json);
        private static void SerializeType(ICuiComponent component, JsonWriter jsonWriter);
        private static void SerializeField(string key, object value, object defaultValue, JsonWriter jsonWriter);
        private static void SerializeField(string key, CuiScrollbar scrollbar, JsonWriter jsonWriter);
        private static void SerializeComponent(ICuiComponent IComponent, JsonWriter jsonWriter);
        [JsonObject(MemberSerialization.OptIn)]
        public class Element : CuiElement
        {
            public new string Name { get; set; }
            public Element ParentElement { get; set; }
            public virtual List<Element> Container { get; set; }
            public ComponentList Components { get; set; }
            [JsonProperty("name")]
            public string JsonName { get; set; }
            public Element();
            public Element(Element parent);
            public CUI.Element AssignParent(Element parent);
            public Element AddDestroy(string elementName);
            public Element AddDestroySelfAttribute();
            public virtual void WriteJson(JsonWriter jsonWriter);
            public Element Add(Element element);
            public Element AddEmpty(string name);
            public Element AddUpdateElement(string name);
            public Element AddText(string text, string color, CUI.Font font, int fontSize, TextAnchor align, VerticalWrapMode overflow, string anchorMin, string anchorMax, string offsetMin, string offsetMax, string name);
            public Element AddOutlinedText(string text, string color, CUI.Font font, int fontSize, TextAnchor align, VerticalWrapMode overflow, string outlineColor, int outlineWidth, string anchorMin, string anchorMax, string offsetMin, string offsetMax, string name);
            public Element AddInputfield(string command, string text, string color, CUI.Font font, int fontSize, TextAnchor align, InputField.LineType lineType, CUI.InputType inputType, bool @readonly, bool autoFocus, bool isPassword, int charsLimit, string anchorMin, string anchorMax, string offsetMin, string offsetMax, string name);
            public Element AddPanel(string color, string sprite, string material, Image.Type imageType, string anchorMin, string anchorMax, string offsetMin, string offsetMax, bool cursorEnabled, bool keyboardEnabled, string name);
            public Element AddButton(string command, string close, string color, string sprite, string material, Image.Type imageType, string anchorMin, string anchorMax, string offsetMin, string offsetMax, string name);
            public Element AddImage(string content, string color, string material, string anchorMin, string anchorMax, string offsetMin, string offsetMax, string name);
            public Element AddHImage(string content, string color, string anchorMin, string anchorMax, string offsetMin, string offsetMax, string name);
            public Element AddIcon(int itemId, ulong skin, string color, string sprite, string material, Image.Type imageType, string anchorMin, string anchorMax, string offsetMin, string offsetMax, string name);
            public Element AddContainer(string anchorMin, string anchorMax, string offsetMin, string offsetMax, string name);
            public CUI.Element WithRect(string anchorMin, string anchorMax, string offsetMin, string offsetMax);
            public CUI.Element WithFade(float @in, float @out);
            public void AddComponents(ICuiComponent[] components);
            public CUI.Element WithComponents(ICuiComponent[] components);
            public CUI.Element CreateChild(string name, ICuiComponent[] components);
            public static CUI.Element Create(string name, ICuiComponent[] components);
            public class ComponentList : List<ICuiComponent>
            {
                public T Get();
                public ComponentList AddImage(string color, string sprite, string material, Image.Type imageType, int itemId, ulong skinId);
                public ComponentList AddRawImage(string content, string color, string sprite, string material);
                public ComponentList AddButton(string command, string close, string color, string sprite, string material, Image.Type imageType);
                public ComponentList AddText(string text, string color, CUI.Font font, int fontSize, TextAnchor align, VerticalWrapMode overflow);
                public ComponentList AddInputfield(string command, string text, string color, CUI.Font font, int fontSize, TextAnchor align, InputField.LineType lineType, CUI.InputType inputType, bool @readonly, bool autoFocus, bool isPassword, int charsLimit);
                public ComponentList AddScrollView(bool horizontal, CuiScrollbar horizonalScrollbar, bool vertical, CuiScrollbar verticalScrollbar, bool inertia, ScrollRect.MovementType movementType, float decelerationRate, float elasticity, float scrollSensitivity, string anchorMin, string anchorMax, string offsetMin, string offsetMax);
                public ComponentList AddOutline(string color, int width);
                public ComponentList AddNeedsKeyboard();
                public ComponentList AddNeedsCursor();
                public ComponentList AddCountdown(string command, int endTime, int startTime, int step);
            }

        }

        public static class ElementContructor
        {
            public static CUI.Element CreateText(string text, string color, CUI.Font font, int fontSize, TextAnchor align, VerticalWrapMode overflow, string anchorMin, string anchorMax, string offsetMin, string offsetMax, string name);
            public static CUI.Element CreateOutlinedText(string text, string color, CUI.Font font, int fontSize, TextAnchor align, VerticalWrapMode overflow, string outlineColor, int outlineWidth, string anchorMin, string anchorMax, string offsetMin, string offsetMax, string name);
            public static CUI.Element CreateInputfield(string command, string text, string color, CUI.Font font, int fontSize, TextAnchor align, InputField.LineType lineType, CUI.InputType inputType, bool @readonly, bool autoFocus, bool isPassword, int charsLimit, string anchorMin, string anchorMax, string offsetMin, string offsetMax, string name);
            public static CUI.Element CreateButton(string command, string close, string color, string sprite, string material, Image.Type imageType, string anchorMin, string anchorMax, string offsetMin, string offsetMax, string name);
            public static CUI.Element CreatePanel(string color, string sprite, string material, Image.Type imageType, string anchorMin, string anchorMax, string offsetMin, string offsetMax, bool cursorEnabled, bool keyboardEnabled, string name);
            public static CUI.Element CreateImage(string content, string color, string material, string anchorMin, string anchorMax, string offsetMin, string offsetMax, string name);
            public static CUI.Element CreateIcon(int itemId, ulong skin, string color, string sprite, string material, Image.Type imageType, string anchorMin, string anchorMax, string offsetMin, string offsetMax, string name);
            public static Element CreateContainer(string anchorMin, string anchorMax, string offsetMin, string offsetMax, string name);
        }

        public class Root : Element
        {
            public bool wasRendered;
            private static StringBuilder stringBuilder;
            public Root();
            public Root(string rootObjectName);
            public override List<Element> Container { get; set; }
            public string ToJson(List<Element> elements);
            public string ToJson();
            public void Render(Connection connection);
            public void Render(BasePlayer player);
            public void Update(Connection connection);
            public void Update(BasePlayer player);
        }

    }

    public class CenteredTextContent : TextContent
    {
        public CenteredTextContent();
    }

    public class Content
    {
        public virtual void Render(ConnectionData connectionData);
        protected virtual void Render(CUI.Element root, ConnectionData connectionData, Dictionary<string, object> userData);
        public virtual void LoadDefaultUserData(Dictionary<string, object> userData);
        public virtual void RestoreUserData(Dictionary<string, object> userData);
    }

    public class ConvarsContent : Content
    {
        private static readonly Label SEARCH_LABEL;
        public static readonly Button SAVE_BUTTON;
        private static List<Timer> sequentialLoad;
        public override void LoadDefaultUserData(Dictionary<string, object> userData);
        public override void RestoreUserData(Dictionary<string, object> userData);
        public void StopSequentialLoad();
        protected override void Render(CUI.Element root, ConnectionData connectionData, Dictionary<string, object> userData);
        public void OpenSearch(Connection connection);
    }

    public class GiveMenuContent : Content
    {
        private static readonly Label NAME_LABEL;
        private static readonly Label SKINID_LABEL;
        private static readonly Label AMOUNT_LABEL;
        private static readonly Label GIVE_LABEL;
        private static readonly Label BLUEPRINT_LABEL;
        private static readonly Label SEARCH_LABEL;
        private static readonly Label ITEM_COUNTER_LABEL;
        private static List<Timer> sequentialLoad;
        public override void LoadDefaultUserData(Dictionary<string, object> userData);
        public override void RestoreUserData(Dictionary<string, object> userData);
        public void StopSequentialLoad();
        protected override void Render(CUI.Element root, ConnectionData connectionData, Dictionary<string, object> userData);
        private void Popup(CUI.Element root, ConnectionData connectionData, Dictionary<string, object> userData);
        public void OpenSearch(Connection connection);
    }

    public class GroupInfoContent : Content
    {
        private static readonly Label GROUPNAME_LABEL;
        private static readonly Label USERS_LABEL;
        private static readonly CloneGroupPopup CloneGroupPopup;
        private static readonly RemoveGroupPopup ConfirmRemovePopup;
        public ButtonArray[] buttons;
        public override void LoadDefaultUserData(Dictionary<string, object> userData);
        protected override void Render(CUI.Element root, ConnectionData connectionData, Dictionary<string, object> userData);
        public void RemoveConfirmPopup(ConnectionData connectionData);
    }

    public class GroupListContent : Content
    {
        private static readonly Label CREATEGROUP_LABEL;
        private static readonly CreateGroupPopup CreateGroupPopup;
        public override void LoadDefaultUserData(Dictionary<string, object> userData);
        protected override void Render(CUI.Element root, ConnectionData connectionData, Dictionary<string, object> userData);
    }

    public class NewPermissionManagerContent : Content
    {
        const float buttonHeight;
        const float halfbuttonHeight;
        const float lineThickness;
        const float halfLineThickness;
        private static Label GROUP_LABEL;
        private static Label USER_LABEL;
        private static Label GROUPS_LABEL;
        private static Label PERMISSIONS_LABEL;
        private static Label MANAGE_LABEL;
        public override void LoadDefaultUserData(Dictionary<string, object> userData);
        protected override void Render(CUI.Element root, ConnectionData connectionData, Dictionary<string, object> userData);
        private static void AddButtonBranch(CUI.Element parent, List<CUI.Root> buttons, int x, int y, int totalY, string name);
        public static void AddGroupsBranch(CUI.Element parent, ConnectionData connectionData, int x, int y, int totalY);
        public static void AddPluginsBranch(CUI.Element parent, Dictionary<string, object> userData, int x, int y, int totalY);
        public static void AddPermissionsBranch(CUI.Element parent, Dictionary<string, object> userData, string pluginName, int x, int y, int totalY);
    }

    public class PlayerListContent : Content
    {
        private static readonly Label SEARCH_LABEL;
        private static List<Timer> sequentialLoad;
        public override void LoadDefaultUserData(Dictionary<string, object> userData);
        public override void RestoreUserData(Dictionary<string, object> userData);
        public void StopSequentialLoad();
        protected override void Render(CUI.Element root, ConnectionData connectionData, Dictionary<string, object> userData);
        public void OpenSearch(Connection connection);
    }

    public class PluginManagerContent : Content
    {
        private static readonly MethodInfo GetPluginMethod;
        public static readonly Button COMMANDS_BUTTON;
        public static readonly Button PERMISSIONS_BUTTON;
        public static readonly Button LOAD_BUTTON;
        public static readonly Button UNLOAD_BUTTON;
        public static readonly Button RELOAD_BUTTON;
        public static readonly Button RELOADALL_BUTTON;
        public static readonly Button FAVORITE_BUTTON;
        public static readonly Button UNFAVORITE_BUTTON;
        private static readonly Label SEARCH_LABEL;
        private static List<Timer> sequentialLoad;
        private const string BAR_BACKGROUND_COLOR;
        private const string BAR_BACKGROUND_COLOR_LAST;
        public override void LoadDefaultUserData(Dictionary<string, object> userData);
        public override void RestoreUserData(Dictionary<string, object> userData);
        public void StopSequentialLoad();
        public void UpdateForPlugin(ConnectionData connectionData, string pluginName);
        private void AddButtons(CUI.Element root, string pluginName, Plugin plugin, ConnectionData connectionData, int count);
        protected override void Render(CUI.Element root, ConnectionData connectionData, Dictionary<string, object> userData);
        public void OpenSearch(Connection connection);
    }

    public class QuickMenuContent : Content
    {
        public ButtonGrid<Button> buttonGrid;
        protected override void Render(CUI.Element root, ConnectionData connectionData, Dictionary<string, object> userData);
    }

    public class TextContent : Content
    {
        public string text;
        public CUI.Font font;
        public TextAnchor align;
        public Vector2 margin;
        public int fontSize;
        public bool allowCopy;
        protected override void Render(CUI.Element root, ConnectionData connectionData, Dictionary<string, object> userData);
    }

    public class UserInfoContent : Content
    {
        private static readonly Label HEALTH_LABEL;
        private static readonly Label GRID_LABEL;
        private static readonly Label CONNECTIONTIME_LABEL;
        private static readonly Label BALANCE_LABEL;
        private static readonly Label CLAN_LABEL;
        private static readonly Label UNKNOWN_LABEL;
        private static readonly Label NOTCONNECTED_LABEL;
        private static readonly Label STEAMINFO_LOCATION_LABEL;
        private static readonly Label STEAMINFO_REGISTRATION_LABEL;
        private static readonly Label STEAMINFO_HOURSINRUST_LABEL;
        private static readonly KickPopup kickPopup;
        private static readonly BanPopup banPopup;
        public ButtonGrid<Button> buttonGrid;
        public static string GetDisplayName(IPlayer player);
        protected override void Render(CUI.Element root, ConnectionData connectionData, Dictionary<string, object> userData);
        public void ShowKickPopup(ConnectionData connectionData);
        public void ShowBanPopup(ConnectionData connectionData);
    }

    public class Theme : Dictionary<string, Color>
    {
        public static class KeyCollection
        {
            public static string PANEL_SIDEBAR_BACKGROUND;
            public static string PANEL_SIDEBAR_SELECTED;
            public static string POPUP_HEADER;
            public static string POPUP_HEADER_TEXT;
        }

        public string GetColorString(string key);
    }

    public class ThemeConverter : JsonConverter
    {
        public override void WriteJson(JsonWriter writer, object value, JsonSerializer serializer);
        public override bool CanConvert(Type objectType);
        public override object ReadJson(JsonReader reader, Type objectType, object existingValue, JsonSerializer serializer);
        public override bool CanRead { get; set; }
    }

    public static class Themes
    {
        public static Theme dark;
        public static Theme current;
        public static Theme CurrentTheme { get; set; }
        static Themes();
    }

    public class BanPopup : BasePopup
    {
        private static readonly Label REASON_LABEL;
        private static readonly Button BAN_BUTTON;
        public BanPopup();
        public static void LoadDefaultUserData(Dictionary<string, object> userData);
        public override CUI.Element AddUI(CUI.Element parent, ConnectionData connectionData, Dictionary<string, object> userData);
    }

    public abstract class BasePopup : IUIModule
    {
        public float Width { get; set; }
        public float Height { get; set; }
        public Color Color { get; set; }
        public List<IUIModule> Modules { get; set; }
        public string CloseCommand { get; set; }
        public virtual CUI.Element AddUI(CUI.Element parent, ConnectionData connectionData, Dictionary<string, object> userData);
    }

    public class CloneGroupPopup : BasePopup
    {
        private static readonly Label CGP_CLONE_LABEL;
        private static readonly Label CGP_NAME_LABEL;
        private static readonly Label CGP_TITLE_LABEL;
        private static readonly Label CGP_CLONEUSERS_LABEL;
        public CloneGroupPopup();
        public static void LoadDefaultUserData(Dictionary<string, object> userData);
        public override CUI.Element AddUI(CUI.Element parent, ConnectionData connectionData, Dictionary<string, object> userData);
    }

    public class CreateGroupPopup : BasePopup
    {
        private static readonly Label CGP_NAME_LABEL;
        private static readonly Label CGP_TITLE_LABEL;
        private static readonly Label CGP_CREATE_LABEL;
        public CreateGroupPopup();
        public static void LoadDefaultUserData(Dictionary<string, object> userData);
        public override CUI.Element AddUI(CUI.Element parent, ConnectionData connectionData, Dictionary<string, object> userData);
    }

    public class HeaderModule : IUIModule
    {
        public Label Label { get; set; }
        public CUI.Font Font { get; set; }
        public float Height { get; set; }
        public int FontSize { get; set; }
        public Color TextColor { get; set; }
        public Color BackgroundColor { get; set; }
        public HeaderModule(string label, float height, CUI.Font font, int fontSize);
        public CUI.Element AddUI(CUI.Element parent, ConnectionData connectionData, Dictionary<string, object> userData);
    }

    public class KickPopup : BasePopup
    {
        private static readonly Label REASON_LABEL;
        private static readonly Button KICK_BUTTON;
        public KickPopup();
        public static void LoadDefaultUserData(Dictionary<string, object> userData);
        public override CUI.Element AddUI(CUI.Element parent, ConnectionData connectionData, Dictionary<string, object> userData);
    }

    public class Popup : BasePopup
    {
        public override CUI.Element AddUI(CUI.Element root, ConnectionData connectionData, Dictionary<string, object> userData);
    }

    public class RemoveGroupPopup : BasePopup
    {
        private static readonly Label REMOVECONFIRM_LABEL;
        private static readonly Label REMOVE_LABEL;
        private static readonly Label CANCEL_LABEL;
        public RemoveGroupPopup();
        public override CUI.Element AddUI(CUI.Element parent, ConnectionData connectionData, Dictionary<string, object> userData);
    }

    public class TextPopup : BasePopup
    {
        public string title;
        public string text;
        public TextPopup(string title, string text);
        public override CUI.Element AddUI(CUI.Element parent, ConnectionData connectionData, Dictionary<string, object> userData);
    }

    private void adminmenu_chatcmd(BasePlayer player);
    [ConsoleCommand("adminmenu")]
    private void adminmenu_cmd(ConsoleSystem.Arg arg);
    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private bool CanUseCommand(Connection connection, string permission);
    private string FormatCommand(string command, BasePlayer admin);
    private string FormatCommandToUser(string command, BasePlayer user, BasePlayer admin);
    private void HandleCommand(Connection connection, string command, string[] args);
     void Init();
     void OnServerInitialized();
     void Loaded();
     void Unload();
     void OnPlayerDisconnected(BasePlayer player, string reason);
     void OnPluginLoaded(Plugin plugin);
     void OnPluginUnloaded(Plugin plugin);
     object OnMapMarkerAdd(BasePlayer player, ProtoBuf.MapNote note);
     void OnPlayerLootEnd(PlayerLoot loot);
     object CanMoveItem(Item item, PlayerInventory playerInventory, ItemContainerId targetContainer, int targetSlot, int amount, ItemMoveModifier itemMoveModifier);
     void FormatMainMenu();
    private void OpenUserInfoAtCrosshair(BasePlayer player);
    private void ToggleMenu(BasePlayer player);
    private void RequestSteamInfo(ulong steamId, SteamInfo steamInfo, Action<SteamInfo> callback, bool force_update);
    public long GetTimestamp(string timestampSTR, long def);
    private bool CanUseAdminMenu(Connection connection);
    private bool CanUseAdminMenu(BasePlayer player);
    public bool UserHasPermission(string userId, string permission);
    private void LogToDiscord(Connection author, string title, string content, string thumbnail, string image);
    private void DiscordSendMessageCallback(int code, string message);
    private BasePlayer GetSpectatingTarget(BasePlayer spectator);
    private string[] GetPermissions(string pluginName);
    private string[] GetConsoleCommands(string pluginName);
    private string[] GetChatCommands(string pluginName);
     void AddViewingBackpack(PlayerLoot loot, Item item);
     void RemoveViewingBackpack(PlayerLoot loot);
     void ViewContainer(BasePlayer player, ItemContainer container);
     void UpdateOneActiveImageElement(Connection connection, string baseId, int activeButtonIndex, int buttonCount, string activeColor, string disableColor);
     void UpdateOneActiveTextElement(Connection connection, string baseId, int activeButtonIndex, int buttonCount, string activeColor, string disableColor);
     void UpdateOneActiveElement(Connection connection, string baseId, int activeButtonIndex, int buttonCount, string activeColor, string disableColor, bool isImage);
     void FormatPanelList();
    private void swapseats_hook(ConsoleSystem.Arg arg);
    private void lighttoggle_hook(ConsoleSystem.Arg arg);
}

public class ButtonArray : List<T>
{
    public ButtonArray();
    public ButtonArray(IEnumerable<T> collection);
    public IEnumerable<T> GetAllowedButtons(Connection connection);
    public IEnumerable<T> GetAllowedButtons(string userId);
}

public class ButtonGrid : List<ButtonGrid<T>.Item>
{
    public class Item
    {
        public int row;
        public int column;
        public T button;
        public Item(int row, int column, T button);
    }

    public IEnumerable<Item> GetAllowedButtons(Connection connection);
    public IEnumerable<Item> GetAllowedButtons(string userId);
}

public class Item
{
    public int row;
    public int column;
    public T button;
    public Item(int row, int column, T button);
}

public class ButtonArray : ButtonArray<Button>
{
}

public class Button
{
    private string label;
    private string permission;
    public string Command { get; set; }
    public string[] Args { get; set; }
    public Label Label { get; set; }
    public virtual int FontSize { get; set; }
    public ButtonStyle Style { get; set; }
    public string Permission { get; set; }
    public string FullCommand { get; set; }
    public Button();
    public Button(string label, string command, string[] args);
    public virtual Button.State GetState(ConnectionData connectionData);
    public virtual void SetState(ConnectionData connectionData, Button.State state);
    public bool UserHasPermission(Connection connection);
    public bool UserHasPermission(string userId);
    public virtual bool IsPressed(ConnectionData connectionData);
    public virtual bool IsHidden(ConnectionData connectionData);
    internal static Dictionary<string, Button> all;
}

public class ButtonStyle : ICloneable
{
    public string BackgroundColor { get; set; }
    public string ActiveBackgroundColor { get; set; }
    public string TextColor { get; set; }
    public object Clone();
    public static ButtonStyle Default { get; set; }
}

public class CategoryButton : Button
{
    public override int FontSize { get; set; }
    public CategoryButton(string label, string command, string[] args);
}

public class HideButton : Button
{
    public HideButton(string label, string command, string[] args);
    public override bool IsHidden(ConnectionData connectionData);
}

public class ToggleButton : Button
{
    public ToggleButton(string label, string command, string[] args);
    public virtual void Toggle(ConnectionData connectionData);
}

public class ConditionToggleButton : ToggleButton
{
    public Func<ConnectionData, bool> Condition { get; set; }
    public ConditionToggleButton(string label, string command, string[] args);
    public override State GetState(ConnectionData connectionData);
    public override void Toggle(ConnectionData connectionData);
}

[JsonObject(MemberSerialization.OptIn)]
public class BaseCustomButton
{
    [JsonProperty]
    public string Label { get; set; }
    [JsonProperty("Execution as server")]
    public bool ExecutionAsServer { get; set; }
    [JsonProperty("Commands")]
    public string[] Commands { get; set; }
    [JsonProperty("Commands for Toggled state")]
    public string[] ToggledStateCommands { get; set; }
    [JsonProperty]
    public string Permission { get; set; }
    [JsonProperty]
    public ButtonStyle Style { get; set; }
    [JsonProperty]
    public int[] Position { get; set; }
    protected virtual string BaseCommand { get; set; }
    private Button _button;
    public Button Button { get; set; }
    private Button GetButton();
}

public class QMCustomButton : BaseCustomButton
{
    protected override string BaseCommand { get; set; }
    [JsonProperty("Bulk sending of command to each player. Available values: None, Online, Offline, Everyone")]
    [JsonConverter(typeof(Newtonsoft.Json.Converters.StringEnumConverter))]
    public Recievers PlayerReceivers { get; set; }
}

public class UserInfoCustomButton : BaseCustomButton
{
    protected override string BaseCommand { get; set; }
}

public class Configuration
{
    [JsonProperty(PropertyName = "Text under the ADMIN MENU")]
    public string Subtext { get; set; }
    [JsonProperty(PropertyName = "Button to hook (X | F | OFF)")]
    [JsonConverter(typeof(Newtonsoft.Json.Converters.StringEnumConverter))]
    public ButtonHook ButtonToHook { get; set; }
    [JsonProperty(PropertyName = "Chat command to show admin menu")]
    public string ChatCommand { get; set; }
    [JsonProperty(PropertyName = "Theme")]
    [JsonConverter(typeof(ThemeConverter))]
    public Theme Theme { get; set; }
    [JsonProperty(PropertyName = "Custom Quick Buttons")]
    public List<QMCustomButton> CustomQuickButtons { get; set; }
    [JsonProperty(PropertyName = "User Custom Buttons")]
    public List<UserInfoCustomButton> UserInfoCustomButtons { get; set; }
    [JsonProperty(PropertyName = "Give menu item presets (add your custom items for easy give)")]
    public List<ItemPreset> GiveItemPresets { get; set; }
    [JsonProperty(PropertyName = "Favorite Plugins")]
    public HashSet<string> FavoritePlugins { get; set; }
    [JsonProperty(PropertyName = "Logs Properties")]
    public LogsProperties Logs { get; set; }
    [JsonIgnore]
    public Dictionary<int, string[]> HashedCommands { get; set; }
    public static Configuration DefaultConfig();
    public class ItemPreset
    {
        [JsonProperty(PropertyName = "Short Name")]
        public string ShortName { get; set; }
        [JsonProperty(PropertyName = "Skin Id")]
        public ulong SkinId { get; set; }
        [JsonProperty(PropertyName = "Name")]
        public string Name { get; set; }
        [JsonProperty(PropertyName = "Category Name")]
        public string Category { get; set; }
        [JsonIgnore]
        public static ItemPreset Example { get; set; }
    }

    public class LogsProperties
    {
        [JsonProperty(PropertyName = "Discord Webhook URL for logs (https://support.discord.com/hc/en-us/articles/228383668-Intro-to-Webhooks)")]
        public string WebhookURL { get; set; }
        [JsonProperty(PropertyName = "Give")]
        public bool Give { get; set; }
        [JsonProperty(PropertyName = "Admin teleports")]
        public bool AdminTeleport { get; set; }
        [JsonProperty(PropertyName = "Spectate")]
        public bool Spectate { get; set; }
        [JsonProperty(PropertyName = "Heal")]
        public bool Heal { get; set; }
        [JsonProperty(PropertyName = "Kill")]
        public bool Kill { get; set; }
        [JsonProperty(PropertyName = "Look inventory")]
        public bool LookInventory { get; set; }
        [JsonProperty(PropertyName = "Strip inventory")]
        public bool StripInventory { get; set; }
        [JsonProperty(PropertyName = "Blueprints")]
        public bool Blueprints { get; set; }
        [JsonProperty(PropertyName = "Mute/Unmute")]
        public bool MuteUnmute { get; set; }
        [JsonProperty(PropertyName = "Toggle Creative")]
        public bool ToggleCreative { get; set; }
        [JsonProperty(PropertyName = "Cuff")]
        public bool Cuff { get; set; }
        [JsonProperty(PropertyName = "Kick the player")]
        public bool Kick { get; set; }
        [JsonProperty(PropertyName = "Ban the player")]
        public bool Ban { get; set; }
        [JsonProperty(PropertyName = "Using custom buttons")]
        public bool CustomButtons { get; set; }
        [JsonProperty(PropertyName = "Spawn entities")]
        public bool SpawnEntities { get; set; }
        [JsonProperty(PropertyName = "Set time")]
        public bool SetTime { get; set; }
        [JsonProperty(PropertyName = "ConVars")]
        public bool ConVars { get; set; }
        [JsonProperty(PropertyName = "Plugin Manager")]
        public bool PluginManager { get; set; }
    }

}

public class ItemPreset
{
    [JsonProperty(PropertyName = "Short Name")]
    public string ShortName { get; set; }
    [JsonProperty(PropertyName = "Skin Id")]
    public ulong SkinId { get; set; }
    [JsonProperty(PropertyName = "Name")]
    public string Name { get; set; }
    [JsonProperty(PropertyName = "Category Name")]
    public string Category { get; set; }
    [JsonIgnore]
    public static ItemPreset Example { get; set; }
}

public class LogsProperties
{
    [JsonProperty(PropertyName = "Discord Webhook URL for logs (https://support.discord.com/hc/en-us/articles/228383668-Intro-to-Webhooks)")]
    public string WebhookURL { get; set; }
    [JsonProperty(PropertyName = "Give")]
    public bool Give { get; set; }
    [JsonProperty(PropertyName = "Admin teleports")]
    public bool AdminTeleport { get; set; }
    [JsonProperty(PropertyName = "Spectate")]
    public bool Spectate { get; set; }
    [JsonProperty(PropertyName = "Heal")]
    public bool Heal { get; set; }
    [JsonProperty(PropertyName = "Kill")]
    public bool Kill { get; set; }
    [JsonProperty(PropertyName = "Look inventory")]
    public bool LookInventory { get; set; }
    [JsonProperty(PropertyName = "Strip inventory")]
    public bool StripInventory { get; set; }
    [JsonProperty(PropertyName = "Blueprints")]
    public bool Blueprints { get; set; }
    [JsonProperty(PropertyName = "Mute/Unmute")]
    public bool MuteUnmute { get; set; }
    [JsonProperty(PropertyName = "Toggle Creative")]
    public bool ToggleCreative { get; set; }
    [JsonProperty(PropertyName = "Cuff")]
    public bool Cuff { get; set; }
    [JsonProperty(PropertyName = "Kick the player")]
    public bool Kick { get; set; }
    [JsonProperty(PropertyName = "Ban the player")]
    public bool Ban { get; set; }
    [JsonProperty(PropertyName = "Using custom buttons")]
    public bool CustomButtons { get; set; }
    [JsonProperty(PropertyName = "Spawn entities")]
    public bool SpawnEntities { get; set; }
    [JsonProperty(PropertyName = "Set time")]
    public bool SetTime { get; set; }
    [JsonProperty(PropertyName = "ConVars")]
    public bool ConVars { get; set; }
    [JsonProperty(PropertyName = "Plugin Manager")]
    public bool PluginManager { get; set; }
}

public class ConnectionData
{
    public Connection connection;
    public MainMenu currentMainMenu;
    public Panel currentPanel;
    public Sidebar currentSidebar;
    public Content currentContent;
    public Translator15 translator;
    public Dictionary<string, object> userData;
    public ConnectionData(BasePlayer player);
    public ConnectionData(Connection connection);
    public ConnectionUI UI { get; set; }
    public bool IsAdminMenuDisplay { get; set; }
    public bool IsDestroyed { get; set; }
    public void Init();
    public void ShowAdminMenu();
    public void HideAdminMenu();
    public ConnectionData OpenPanel(string panelName);
    public void SetSidebar(Sidebar sidebar);
    public void ShowPanelContent(Content content);
    public void ShowPanelContent(string contentId);
    public void Dispose();
    public static Dictionary<Connection, ConnectionData> all;
    public static ConnectionData Get(Connection connection);
    public static ConnectionData Get(BasePlayer player);
    public static ConnectionData GetOrCreate(Connection connection);
    public static ConnectionData GetOrCreate(BasePlayer player);
}

public class ConnectionUI
{
     Connection connection;
     ConnectionData connectionData;
    public ConnectionUI(ConnectionData connectionData);
    public void ShowAdminMenu();
    public void HideAdminMenu();
    public void DestroyAdminMenu();
    public void DestroyAll();
    public void AddSidebar(CUI.Element element, Sidebar sidebar);
    private void AddNavButtons(CUI.Element element, MainMenu mainMenu);
    public void UpdateNavButtons(MainMenu mainMenu);
    public void RenderOverlayOpenButton();
    public void RenderMainMenu(MainMenu mainMenu);
    public void RenderPanel(Panel panel);
    public void RenderContent(Content content);
}

public static class Extensions
{
    public static string ToCuiString(Color color);
}

[HarmonyPatch(typeof(BasePlayer), "Tick_Spectator")]
private static class SpectatorStaff
{
    private static bool Prefix(BasePlayer __instance);
}

public class Label
{
    private static readonly Regex richTextRegex;
     string label;
     string langKey;
    public Label(string label);
    public override string ToString();
    public string Localize(string userId);
    public string Localize(Connection connection);
}

public class MainMenu
{
    public ButtonArray NavButtons { get; set; }
}

public class Panel
{
    public virtual Sidebar Sidebar { get; set; }
    public virtual Dictionary<string, Content> Content { get; set; }
    public Content DefaultContent { get; set; }
    public Content TryGetContent(string id);
    public virtual void OnOpen(ConnectionData connectionData);
    public virtual void OnClose(ConnectionData connectionData);
}

public class UserInfoPanel : Panel
{
}

public class PermissionPanel : Panel
{
    public override Sidebar Sidebar { get; set; }
    public override void OnClose(ConnectionData connectionData);
}

public class Sidebar
{
    public ButtonArray<CategoryButton> CategoryButtons { get; set; }
    public int? AutoActivateCategoryButtonIndex { get; set; }
}

public class SteamInfo
{
    public string Location { get; set; }
    public string[] Avatars { get; set; }
    public string RegistrationDate { get; set; }
    public string RustHours { get; set; }
    public override string ToString();
}

public class Translator15
{
    public static void Init(string language);
    private static Dictionary<string, Dictionary<string, string>> translations;
    public class Phrase : Translate.Phrase
    {
        private Translator15 translator;
        public Phrase(Translator15 translator);
        public override string translated { get; set; }
    }

    protected ulong userID;
    public Translator15(ulong userID);
    public Translator15.Phrase Convert(Translate.Phrase phrase);
    private string GetLanguage();
    public string Get(string key, string def);
    private Dictionary<Translate.Phrase, Translator15.Phrase> phraseCache;
}

public class Phrase : Translate.Phrase
{
    private Translator15 translator;
    public Phrase(Translator15 translator);
    public override string translated { get; set; }
}

public class CUI
{
    private static readonly Dictionary<Font, string> FontToString;
    private static readonly Dictionary<TextAnchor, string> TextAnchorToString;
    private static readonly Dictionary<VerticalWrapMode, string> VWMToString;
    private static readonly Dictionary<Image.Type, string> ImageTypeToString;
    private static readonly Dictionary<InputField.LineType, string> LineTypeToString;
    private static readonly Dictionary<ScrollRect.MovementType, string> MovementTypeToString;
    public static class Defaults
    {
        public const string VectorZero;
        public const string VectorOne;
        public const string Color;
        public const string OutlineColor;
        public const string Sprite;
        public const string Material;
        public const string IconMaterial;
        public const Image.Type ImageType;
        public const CUI.Font Font;
        public const int FontSize;
        public const TextAnchor Align;
        public const VerticalWrapMode VerticalOverflow;
        public const InputField.LineType LineType;
    }

    public static Color GetColor(string colorStr);
    public static string GetColorString(Color color);
    public static void AddUI(Connection connection, string json);
    private static void SerializeType(ICuiComponent component, JsonWriter jsonWriter);
    private static void SerializeField(string key, object value, object defaultValue, JsonWriter jsonWriter);
    private static void SerializeField(string key, CuiScrollbar scrollbar, JsonWriter jsonWriter);
    private static void SerializeComponent(ICuiComponent IComponent, JsonWriter jsonWriter);
    [JsonObject(MemberSerialization.OptIn)]
    public class Element : CuiElement
    {
        public new string Name { get; set; }
        public Element ParentElement { get; set; }
        public virtual List<Element> Container { get; set; }
        public ComponentList Components { get; set; }
        [JsonProperty("name")]
        public string JsonName { get; set; }
        public Element();
        public Element(Element parent);
        public CUI.Element AssignParent(Element parent);
        public Element AddDestroy(string elementName);
        public Element AddDestroySelfAttribute();
        public virtual void WriteJson(JsonWriter jsonWriter);
        public Element Add(Element element);
        public Element AddEmpty(string name);
        public Element AddUpdateElement(string name);
        public Element AddText(string text, string color, CUI.Font font, int fontSize, TextAnchor align, VerticalWrapMode overflow, string anchorMin, string anchorMax, string offsetMin, string offsetMax, string name);
        public Element AddOutlinedText(string text, string color, CUI.Font font, int fontSize, TextAnchor align, VerticalWrapMode overflow, string outlineColor, int outlineWidth, string anchorMin, string anchorMax, string offsetMin, string offsetMax, string name);
        public Element AddInputfield(string command, string text, string color, CUI.Font font, int fontSize, TextAnchor align, InputField.LineType lineType, CUI.InputType inputType, bool @readonly, bool autoFocus, bool isPassword, int charsLimit, string anchorMin, string anchorMax, string offsetMin, string offsetMax, string name);
        public Element AddPanel(string color, string sprite, string material, Image.Type imageType, string anchorMin, string anchorMax, string offsetMin, string offsetMax, bool cursorEnabled, bool keyboardEnabled, string name);
        public Element AddButton(string command, string close, string color, string sprite, string material, Image.Type imageType, string anchorMin, string anchorMax, string offsetMin, string offsetMax, string name);
        public Element AddImage(string content, string color, string material, string anchorMin, string anchorMax, string offsetMin, string offsetMax, string name);
        public Element AddHImage(string content, string color, string anchorMin, string anchorMax, string offsetMin, string offsetMax, string name);
        public Element AddIcon(int itemId, ulong skin, string color, string sprite, string material, Image.Type imageType, string anchorMin, string anchorMax, string offsetMin, string offsetMax, string name);
        public Element AddContainer(string anchorMin, string anchorMax, string offsetMin, string offsetMax, string name);
        public CUI.Element WithRect(string anchorMin, string anchorMax, string offsetMin, string offsetMax);
        public CUI.Element WithFade(float @in, float @out);
        public void AddComponents(ICuiComponent[] components);
        public CUI.Element WithComponents(ICuiComponent[] components);
        public CUI.Element CreateChild(string name, ICuiComponent[] components);
        public static CUI.Element Create(string name, ICuiComponent[] components);
        public class ComponentList : List<ICuiComponent>
        {
            public T Get();
            public ComponentList AddImage(string color, string sprite, string material, Image.Type imageType, int itemId, ulong skinId);
            public ComponentList AddRawImage(string content, string color, string sprite, string material);
            public ComponentList AddButton(string command, string close, string color, string sprite, string material, Image.Type imageType);
            public ComponentList AddText(string text, string color, CUI.Font font, int fontSize, TextAnchor align, VerticalWrapMode overflow);
            public ComponentList AddInputfield(string command, string text, string color, CUI.Font font, int fontSize, TextAnchor align, InputField.LineType lineType, CUI.InputType inputType, bool @readonly, bool autoFocus, bool isPassword, int charsLimit);
            public ComponentList AddScrollView(bool horizontal, CuiScrollbar horizonalScrollbar, bool vertical, CuiScrollbar verticalScrollbar, bool inertia, ScrollRect.MovementType movementType, float decelerationRate, float elasticity, float scrollSensitivity, string anchorMin, string anchorMax, string offsetMin, string offsetMax);
            public ComponentList AddOutline(string color, int width);
            public ComponentList AddNeedsKeyboard();
            public ComponentList AddNeedsCursor();
            public ComponentList AddCountdown(string command, int endTime, int startTime, int step);
        }

    }

    public static class ElementContructor
    {
        public static CUI.Element CreateText(string text, string color, CUI.Font font, int fontSize, TextAnchor align, VerticalWrapMode overflow, string anchorMin, string anchorMax, string offsetMin, string offsetMax, string name);
        public static CUI.Element CreateOutlinedText(string text, string color, CUI.Font font, int fontSize, TextAnchor align, VerticalWrapMode overflow, string outlineColor, int outlineWidth, string anchorMin, string anchorMax, string offsetMin, string offsetMax, string name);
        public static CUI.Element CreateInputfield(string command, string text, string color, CUI.Font font, int fontSize, TextAnchor align, InputField.LineType lineType, CUI.InputType inputType, bool @readonly, bool autoFocus, bool isPassword, int charsLimit, string anchorMin, string anchorMax, string offsetMin, string offsetMax, string name);
        public static CUI.Element CreateButton(string command, string close, string color, string sprite, string material, Image.Type imageType, string anchorMin, string anchorMax, string offsetMin, string offsetMax, string name);
        public static CUI.Element CreatePanel(string color, string sprite, string material, Image.Type imageType, string anchorMin, string anchorMax, string offsetMin, string offsetMax, bool cursorEnabled, bool keyboardEnabled, string name);
        public static CUI.Element CreateImage(string content, string color, string material, string anchorMin, string anchorMax, string offsetMin, string offsetMax, string name);
        public static CUI.Element CreateIcon(int itemId, ulong skin, string color, string sprite, string material, Image.Type imageType, string anchorMin, string anchorMax, string offsetMin, string offsetMax, string name);
        public static Element CreateContainer(string anchorMin, string anchorMax, string offsetMin, string offsetMax, string name);
    }

    public class Root : Element
    {
        public bool wasRendered;
        private static StringBuilder stringBuilder;
        public Root();
        public Root(string rootObjectName);
        public override List<Element> Container { get; set; }
        public string ToJson(List<Element> elements);
        public string ToJson();
        public void Render(Connection connection);
        public void Render(BasePlayer player);
        public void Update(Connection connection);
        public void Update(BasePlayer player);
    }

}

public static class Defaults
{
    public const string VectorZero;
    public const string VectorOne;
    public const string Color;
    public const string OutlineColor;
    public const string Sprite;
    public const string Material;
    public const string IconMaterial;
    public const Image.Type ImageType;
    public const CUI.Font Font;
    public const int FontSize;
    public const TextAnchor Align;
    public const VerticalWrapMode VerticalOverflow;
    public const InputField.LineType LineType;
}

[JsonObject(MemberSerialization.OptIn)]
public class Element : CuiElement
{
    public new string Name { get; set; }
    public Element ParentElement { get; set; }
    public virtual List<Element> Container { get; set; }
    public ComponentList Components { get; set; }
    [JsonProperty("name")]
    public string JsonName { get; set; }
    public Element();
    public Element(Element parent);
    public CUI.Element AssignParent(Element parent);
    public Element AddDestroy(string elementName);
    public Element AddDestroySelfAttribute();
    public virtual void WriteJson(JsonWriter jsonWriter);
    public Element Add(Element element);
    public Element AddEmpty(string name);
    public Element AddUpdateElement(string name);
    public Element AddText(string text, string color, CUI.Font font, int fontSize, TextAnchor align, VerticalWrapMode overflow, string anchorMin, string anchorMax, string offsetMin, string offsetMax, string name);
    public Element AddOutlinedText(string text, string color, CUI.Font font, int fontSize, TextAnchor align, VerticalWrapMode overflow, string outlineColor, int outlineWidth, string anchorMin, string anchorMax, string offsetMin, string offsetMax, string name);
    public Element AddInputfield(string command, string text, string color, CUI.Font font, int fontSize, TextAnchor align, InputField.LineType lineType, CUI.InputType inputType, bool @readonly, bool autoFocus, bool isPassword, int charsLimit, string anchorMin, string anchorMax, string offsetMin, string offsetMax, string name);
    public Element AddPanel(string color, string sprite, string material, Image.Type imageType, string anchorMin, string anchorMax, string offsetMin, string offsetMax, bool cursorEnabled, bool keyboardEnabled, string name);
    public Element AddButton(string command, string close, string color, string sprite, string material, Image.Type imageType, string anchorMin, string anchorMax, string offsetMin, string offsetMax, string name);
    public Element AddImage(string content, string color, string material, string anchorMin, string anchorMax, string offsetMin, string offsetMax, string name);
    public Element AddHImage(string content, string color, string anchorMin, string anchorMax, string offsetMin, string offsetMax, string name);
    public Element AddIcon(int itemId, ulong skin, string color, string sprite, string material, Image.Type imageType, string anchorMin, string anchorMax, string offsetMin, string offsetMax, string name);
    public Element AddContainer(string anchorMin, string anchorMax, string offsetMin, string offsetMax, string name);
    public CUI.Element WithRect(string anchorMin, string anchorMax, string offsetMin, string offsetMax);
    public CUI.Element WithFade(float @in, float @out);
    public void AddComponents(ICuiComponent[] components);
    public CUI.Element WithComponents(ICuiComponent[] components);
    public CUI.Element CreateChild(string name, ICuiComponent[] components);
    public static CUI.Element Create(string name, ICuiComponent[] components);
    public class ComponentList : List<ICuiComponent>
    {
        public T Get();
        public ComponentList AddImage(string color, string sprite, string material, Image.Type imageType, int itemId, ulong skinId);
        public ComponentList AddRawImage(string content, string color, string sprite, string material);
        public ComponentList AddButton(string command, string close, string color, string sprite, string material, Image.Type imageType);
        public ComponentList AddText(string text, string color, CUI.Font font, int fontSize, TextAnchor align, VerticalWrapMode overflow);
        public ComponentList AddInputfield(string command, string text, string color, CUI.Font font, int fontSize, TextAnchor align, InputField.LineType lineType, CUI.InputType inputType, bool @readonly, bool autoFocus, bool isPassword, int charsLimit);
        public ComponentList AddScrollView(bool horizontal, CuiScrollbar horizonalScrollbar, bool vertical, CuiScrollbar verticalScrollbar, bool inertia, ScrollRect.MovementType movementType, float decelerationRate, float elasticity, float scrollSensitivity, string anchorMin, string anchorMax, string offsetMin, string offsetMax);
        public ComponentList AddOutline(string color, int width);
        public ComponentList AddNeedsKeyboard();
        public ComponentList AddNeedsCursor();
        public ComponentList AddCountdown(string command, int endTime, int startTime, int step);
    }

}

public class ComponentList : List<ICuiComponent>
{
    public T Get();
    public ComponentList AddImage(string color, string sprite, string material, Image.Type imageType, int itemId, ulong skinId);
    public ComponentList AddRawImage(string content, string color, string sprite, string material);
    public ComponentList AddButton(string command, string close, string color, string sprite, string material, Image.Type imageType);
    public ComponentList AddText(string text, string color, CUI.Font font, int fontSize, TextAnchor align, VerticalWrapMode overflow);
    public ComponentList AddInputfield(string command, string text, string color, CUI.Font font, int fontSize, TextAnchor align, InputField.LineType lineType, CUI.InputType inputType, bool @readonly, bool autoFocus, bool isPassword, int charsLimit);
    public ComponentList AddScrollView(bool horizontal, CuiScrollbar horizonalScrollbar, bool vertical, CuiScrollbar verticalScrollbar, bool inertia, ScrollRect.MovementType movementType, float decelerationRate, float elasticity, float scrollSensitivity, string anchorMin, string anchorMax, string offsetMin, string offsetMax);
    public ComponentList AddOutline(string color, int width);
    public ComponentList AddNeedsKeyboard();
    public ComponentList AddNeedsCursor();
    public ComponentList AddCountdown(string command, int endTime, int startTime, int step);
}

public static class ElementContructor
{
    public static CUI.Element CreateText(string text, string color, CUI.Font font, int fontSize, TextAnchor align, VerticalWrapMode overflow, string anchorMin, string anchorMax, string offsetMin, string offsetMax, string name);
    public static CUI.Element CreateOutlinedText(string text, string color, CUI.Font font, int fontSize, TextAnchor align, VerticalWrapMode overflow, string outlineColor, int outlineWidth, string anchorMin, string anchorMax, string offsetMin, string offsetMax, string name);
    public static CUI.Element CreateInputfield(string command, string text, string color, CUI.Font font, int fontSize, TextAnchor align, InputField.LineType lineType, CUI.InputType inputType, bool @readonly, bool autoFocus, bool isPassword, int charsLimit, string anchorMin, string anchorMax, string offsetMin, string offsetMax, string name);
    public static CUI.Element CreateButton(string command, string close, string color, string sprite, string material, Image.Type imageType, string anchorMin, string anchorMax, string offsetMin, string offsetMax, string name);
    public static CUI.Element CreatePanel(string color, string sprite, string material, Image.Type imageType, string anchorMin, string anchorMax, string offsetMin, string offsetMax, bool cursorEnabled, bool keyboardEnabled, string name);
    public static CUI.Element CreateImage(string content, string color, string material, string anchorMin, string anchorMax, string offsetMin, string offsetMax, string name);
    public static CUI.Element CreateIcon(int itemId, ulong skin, string color, string sprite, string material, Image.Type imageType, string anchorMin, string anchorMax, string offsetMin, string offsetMax, string name);
    public static Element CreateContainer(string anchorMin, string anchorMax, string offsetMin, string offsetMax, string name);
}

public class Root : Element
{
    public bool wasRendered;
    private static StringBuilder stringBuilder;
    public Root();
    public Root(string rootObjectName);
    public override List<Element> Container { get; set; }
    public string ToJson(List<Element> elements);
    public string ToJson();
    public void Render(Connection connection);
    public void Render(BasePlayer player);
    public void Update(Connection connection);
    public void Update(BasePlayer player);
}

public class CenteredTextContent : TextContent
{
    public CenteredTextContent();
}

public class Content
{
    public virtual void Render(ConnectionData connectionData);
    protected virtual void Render(CUI.Element root, ConnectionData connectionData, Dictionary<string, object> userData);
    public virtual void LoadDefaultUserData(Dictionary<string, object> userData);
    public virtual void RestoreUserData(Dictionary<string, object> userData);
}

public class ConvarsContent : Content
{
    private static readonly Label SEARCH_LABEL;
    public static readonly Button SAVE_BUTTON;
    private static List<Timer> sequentialLoad;
    public override void LoadDefaultUserData(Dictionary<string, object> userData);
    public override void RestoreUserData(Dictionary<string, object> userData);
    public void StopSequentialLoad();
    protected override void Render(CUI.Element root, ConnectionData connectionData, Dictionary<string, object> userData);
    public void OpenSearch(Connection connection);
}

public class GiveMenuContent : Content
{
    private static readonly Label NAME_LABEL;
    private static readonly Label SKINID_LABEL;
    private static readonly Label AMOUNT_LABEL;
    private static readonly Label GIVE_LABEL;
    private static readonly Label BLUEPRINT_LABEL;
    private static readonly Label SEARCH_LABEL;
    private static readonly Label ITEM_COUNTER_LABEL;
    private static List<Timer> sequentialLoad;
    public override void LoadDefaultUserData(Dictionary<string, object> userData);
    public override void RestoreUserData(Dictionary<string, object> userData);
    public void StopSequentialLoad();
    protected override void Render(CUI.Element root, ConnectionData connectionData, Dictionary<string, object> userData);
    private void Popup(CUI.Element root, ConnectionData connectionData, Dictionary<string, object> userData);
    public void OpenSearch(Connection connection);
}

public class GroupInfoContent : Content
{
    private static readonly Label GROUPNAME_LABEL;
    private static readonly Label USERS_LABEL;
    private static readonly CloneGroupPopup CloneGroupPopup;
    private static readonly RemoveGroupPopup ConfirmRemovePopup;
    public ButtonArray[] buttons;
    public override void LoadDefaultUserData(Dictionary<string, object> userData);
    protected override void Render(CUI.Element root, ConnectionData connectionData, Dictionary<string, object> userData);
    public void RemoveConfirmPopup(ConnectionData connectionData);
}

public class GroupListContent : Content
{
    private static readonly Label CREATEGROUP_LABEL;
    private static readonly CreateGroupPopup CreateGroupPopup;
    public override void LoadDefaultUserData(Dictionary<string, object> userData);
    protected override void Render(CUI.Element root, ConnectionData connectionData, Dictionary<string, object> userData);
}

public class NewPermissionManagerContent : Content
{
    const float buttonHeight;
    const float halfbuttonHeight;
    const float lineThickness;
    const float halfLineThickness;
    private static Label GROUP_LABEL;
    private static Label USER_LABEL;
    private static Label GROUPS_LABEL;
    private static Label PERMISSIONS_LABEL;
    private static Label MANAGE_LABEL;
    public override void LoadDefaultUserData(Dictionary<string, object> userData);
    protected override void Render(CUI.Element root, ConnectionData connectionData, Dictionary<string, object> userData);
    private static void AddButtonBranch(CUI.Element parent, List<CUI.Root> buttons, int x, int y, int totalY, string name);
    public static void AddGroupsBranch(CUI.Element parent, ConnectionData connectionData, int x, int y, int totalY);
    public static void AddPluginsBranch(CUI.Element parent, Dictionary<string, object> userData, int x, int y, int totalY);
    public static void AddPermissionsBranch(CUI.Element parent, Dictionary<string, object> userData, string pluginName, int x, int y, int totalY);
}

public class PlayerListContent : Content
{
    private static readonly Label SEARCH_LABEL;
    private static List<Timer> sequentialLoad;
    public override void LoadDefaultUserData(Dictionary<string, object> userData);
    public override void RestoreUserData(Dictionary<string, object> userData);
    public void StopSequentialLoad();
    protected override void Render(CUI.Element root, ConnectionData connectionData, Dictionary<string, object> userData);
    public void OpenSearch(Connection connection);
}

public class PluginManagerContent : Content
{
    private static readonly MethodInfo GetPluginMethod;
    public static readonly Button COMMANDS_BUTTON;
    public static readonly Button PERMISSIONS_BUTTON;
    public static readonly Button LOAD_BUTTON;
    public static readonly Button UNLOAD_BUTTON;
    public static readonly Button RELOAD_BUTTON;
    public static readonly Button RELOADALL_BUTTON;
    public static readonly Button FAVORITE_BUTTON;
    public static readonly Button UNFAVORITE_BUTTON;
    private static readonly Label SEARCH_LABEL;
    private static List<Timer> sequentialLoad;
    private const string BAR_BACKGROUND_COLOR;
    private const string BAR_BACKGROUND_COLOR_LAST;
    public override void LoadDefaultUserData(Dictionary<string, object> userData);
    public override void RestoreUserData(Dictionary<string, object> userData);
    public void StopSequentialLoad();
    public void UpdateForPlugin(ConnectionData connectionData, string pluginName);
    private void AddButtons(CUI.Element root, string pluginName, Plugin plugin, ConnectionData connectionData, int count);
    protected override void Render(CUI.Element root, ConnectionData connectionData, Dictionary<string, object> userData);
    public void OpenSearch(Connection connection);
}

public class QuickMenuContent : Content
{
    public ButtonGrid<Button> buttonGrid;
    protected override void Render(CUI.Element root, ConnectionData connectionData, Dictionary<string, object> userData);
}

public class TextContent : Content
{
    public string text;
    public CUI.Font font;
    public TextAnchor align;
    public Vector2 margin;
    public int fontSize;
    public bool allowCopy;
    protected override void Render(CUI.Element root, ConnectionData connectionData, Dictionary<string, object> userData);
}

public class UserInfoContent : Content
{
    private static readonly Label HEALTH_LABEL;
    private static readonly Label GRID_LABEL;
    private static readonly Label CONNECTIONTIME_LABEL;
    private static readonly Label BALANCE_LABEL;
    private static readonly Label CLAN_LABEL;
    private static readonly Label UNKNOWN_LABEL;
    private static readonly Label NOTCONNECTED_LABEL;
    private static readonly Label STEAMINFO_LOCATION_LABEL;
    private static readonly Label STEAMINFO_REGISTRATION_LABEL;
    private static readonly Label STEAMINFO_HOURSINRUST_LABEL;
    private static readonly KickPopup kickPopup;
    private static readonly BanPopup banPopup;
    public ButtonGrid<Button> buttonGrid;
    public static string GetDisplayName(IPlayer player);
    protected override void Render(CUI.Element root, ConnectionData connectionData, Dictionary<string, object> userData);
    public void ShowKickPopup(ConnectionData connectionData);
    public void ShowBanPopup(ConnectionData connectionData);
}

public class Theme : Dictionary<string, Color>
{
    public static class KeyCollection
    {
        public static string PANEL_SIDEBAR_BACKGROUND;
        public static string PANEL_SIDEBAR_SELECTED;
        public static string POPUP_HEADER;
        public static string POPUP_HEADER_TEXT;
    }

    public string GetColorString(string key);
}

public static class KeyCollection
{
    public static string PANEL_SIDEBAR_BACKGROUND;
    public static string PANEL_SIDEBAR_SELECTED;
    public static string POPUP_HEADER;
    public static string POPUP_HEADER_TEXT;
}

public class ThemeConverter : JsonConverter
{
    public override void WriteJson(JsonWriter writer, object value, JsonSerializer serializer);
    public override bool CanConvert(Type objectType);
    public override object ReadJson(JsonReader reader, Type objectType, object existingValue, JsonSerializer serializer);
    public override bool CanRead { get; set; }
}

public static class Themes
{
    public static Theme dark;
    public static Theme current;
    public static Theme CurrentTheme { get; set; }
    static Themes();
}

public class BanPopup : BasePopup
{
    private static readonly Label REASON_LABEL;
    private static readonly Button BAN_BUTTON;
    public BanPopup();
    public static void LoadDefaultUserData(Dictionary<string, object> userData);
    public override CUI.Element AddUI(CUI.Element parent, ConnectionData connectionData, Dictionary<string, object> userData);
}

public abstract class BasePopup : IUIModule
{
    public float Width { get; set; }
    public float Height { get; set; }
    public Color Color { get; set; }
    public List<IUIModule> Modules { get; set; }
    public string CloseCommand { get; set; }
    public virtual CUI.Element AddUI(CUI.Element parent, ConnectionData connectionData, Dictionary<string, object> userData);
}

public class CloneGroupPopup : BasePopup
{
    private static readonly Label CGP_CLONE_LABEL;
    private static readonly Label CGP_NAME_LABEL;
    private static readonly Label CGP_TITLE_LABEL;
    private static readonly Label CGP_CLONEUSERS_LABEL;
    public CloneGroupPopup();
    public static void LoadDefaultUserData(Dictionary<string, object> userData);
    public override CUI.Element AddUI(CUI.Element parent, ConnectionData connectionData, Dictionary<string, object> userData);
}

public class CreateGroupPopup : BasePopup
{
    private static readonly Label CGP_NAME_LABEL;
    private static readonly Label CGP_TITLE_LABEL;
    private static readonly Label CGP_CREATE_LABEL;
    public CreateGroupPopup();
    public static void LoadDefaultUserData(Dictionary<string, object> userData);
    public override CUI.Element AddUI(CUI.Element parent, ConnectionData connectionData, Dictionary<string, object> userData);
}

public class HeaderModule : IUIModule
{
    public Label Label { get; set; }
    public CUI.Font Font { get; set; }
    public float Height { get; set; }
    public int FontSize { get; set; }
    public Color TextColor { get; set; }
    public Color BackgroundColor { get; set; }
    public HeaderModule(string label, float height, CUI.Font font, int fontSize);
    public CUI.Element AddUI(CUI.Element parent, ConnectionData connectionData, Dictionary<string, object> userData);
}

public class KickPopup : BasePopup
{
    private static readonly Label REASON_LABEL;
    private static readonly Button KICK_BUTTON;
    public KickPopup();
    public static void LoadDefaultUserData(Dictionary<string, object> userData);
    public override CUI.Element AddUI(CUI.Element parent, ConnectionData connectionData, Dictionary<string, object> userData);
}

public class Popup : BasePopup
{
    public override CUI.Element AddUI(CUI.Element root, ConnectionData connectionData, Dictionary<string, object> userData);
}

public class RemoveGroupPopup : BasePopup
{
    private static readonly Label REMOVECONFIRM_LABEL;
    private static readonly Label REMOVE_LABEL;
    private static readonly Label CANCEL_LABEL;
    public RemoveGroupPopup();
    public override CUI.Element AddUI(CUI.Element parent, ConnectionData connectionData, Dictionary<string, object> userData);
}

public class TextPopup : BasePopup
{
    public string title;
    public string text;
    public TextPopup(string title, string text);
    public override CUI.Element AddUI(CUI.Element parent, ConnectionData connectionData, Dictionary<string, object> userData);
}


```

---

## AdminPanel

```csharp
using System.Collections.Generic;
using System;
using Oxide.Game.Rust.Cui;
using Oxide.Core.Plugins;
using UnityEngine;
using Oxide.Core;
using System.IO;
using System.Net;
using System.Collections;
using System.Linq;
using Rust;

Oxide.Plugins
[Info("AdminPanel", "austinv900", "1.2.2", ResourceId = 2034)]
 class AdminPanel : RustPlugin
{
    [PluginReference]
     Plugin Vanish;
    [PluginReference]
     Plugin Godmode;
    [PluginReference]
     Plugin AdminRadar;
    [PluginReference]
     Plugin NTeleportation;
    [PluginReference]
     Plugin EnhancedBanSystem;
    const string permAP;
     void Init();
     string serverBanner;
     string adminZoneCords;
     string PanelPosMin;
     string PanelPosMax;
     string btnInactColor;
     string btnActColor;
     bool ToggleMode;
    protected override void LoadDefaultConfig();
     void LoadDefaultMessages();
     void OnPlayerSleepEnded(BasePlayer player);
     void OnPlayerInit(BasePlayer player);
    [ConsoleCommand("adminpanel")]
     void ccmdAdminPanel(ConsoleSystem.Arg arg);
    [ChatCommand("adminpanel")]
     void cmdAdminPanel(BasePlayer player, string command, string[] args);
     ImageCache ImageAssets;
     GameObject AdminPanelObject;
    private void cacheImage();
    public class ImageCache : MonoBehaviour
    {
        public Dictionary<string, string> imageFiles;
        public List<Queue> queued;
        public class Queue
        {
            public string url { get; set; }
            public string name { get; set; }
        }

        private void OnDestroy();
        public void getImage(string name, string url);
         IEnumerator WaitForRequest(Queue queue);
        public void process();
    }

     void download();
    public string fetchImage(string name);
     void AdminGui(BasePlayer player);
     void Unload();
     T GetConfig(string name, T defaultValue);
     bool IsAdmin(string id);
     bool IsAllowed(string id, string perm);
     string Lang(string key, string id, object[] args);
     void Reply(BasePlayer player, string Getmessage);
}

public class ImageCache : MonoBehaviour
{
    public Dictionary<string, string> imageFiles;
    public List<Queue> queued;
    public class Queue
    {
        public string url { get; set; }
        public string name { get; set; }
    }

    private void OnDestroy();
    public void getImage(string name, string url);
     IEnumerator WaitForRequest(Queue queue);
    public void process();
}

public class Queue
{
    public string url { get; set; }
    public string name { get; set; }
}


```

---

## AdminProtect

```csharp
using System;
using System.Collections.Generic;
using Newtonsoft.Json;

Oxide.Plugins
[Info("AdminProtect", "Ryamkk", "1.0.0")]
public class AdminProtect : RustPlugin
{
    public Configuration config;
    public class Configuration
    {
        [JsonProperty("Список SteamID админов которым разрешено заходить на сервер")]
        public List<ulong> AdminProtectList;
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
     void OnPlayerInit(BasePlayer player);
    [ConsoleCommand("admin.protect")]
     void CmdACIgnore(ConsoleSystem.Arg arg);
    private string URLEncode(string input);
    private void SendVKLogs(string msg, object[] args);
}

public class Configuration
{
    [JsonProperty("Список SteamID админов которым разрешено заходить на сервер")]
    public List<ulong> AdminProtectList;
}


```

---

## AdminProtection

```csharp
using System.Reflection;
using System.Linq;
using Oxide.Core;
using Rust;
using System;
using System.Collections.Generic;

Oxide.Plugins
[Info("AdminProtection", "4seti", "0.6.2", ResourceId = 869)]
public class AdminProtection : RustPlugin
{
    private void Log(string message);
    private void Warn(string message);
    private void Error(string message);
     void ReplyChat(BasePlayer player, string msg);
    static FieldInfo developerIDs;
    private Dictionary<ulong, ProtectionStatus> protData;
    private Dictionary<ulong, DateTime> antiSpam;
    private string ChatName;
    private Hash<ulong, ProtectionStatus> gods;
    private Dictionary<string, string> APHelper;
     Dictionary<string, string> defMsg;
     void Loaded();
    protected override void LoadDefaultConfig();
     void LoadVariables();
    private T GetConfig(string name, T defaultValue);
     void OnServerInitialized();
     void LoadData();
     void SaveData();
    [ChatCommand("apdev")]
     void cmdAPDev(BasePlayer player, string cmd, string[] args);
    [ChatCommand("apdebug")]
     void cmdAPDebug(BasePlayer player, string cmd, string[] args);
    private bool becameDev(BasePlayer player);
    private void setMetabolizm(BasePlayer player, bool Enabling);
    [ChatCommand("aplist")]
     void cmdAPList(BasePlayer player, string cmd, string[] args);
    [ChatCommand("ap")]
     void cmdToggleAP(BasePlayer player, string cmd, string[] args);
     void OnPlayerInit(BasePlayer player);
    private List<BasePlayer> FindPlayerByName(string playerName);
    private List<BasePlayer> FindPlayerByID(ulong playerID);
    private bool IsAllDigits(string s);
     void OnPlayerLoot(PlayerLoot lootInventory, UnityEngine.Object entry);
    private HitInfo OnEntityTakeDamage(BaseCombatEntity entity, HitInfo hitInfo);
     void SendHelpText(BasePlayer player);
    public class ProtectionStatus
    {
        public string Name;
        public msgType MsgType;
        public string Enabler;
        public bool isDebug;
        public HealthData HealthData;
        public ProtectionStatus();
        public ProtectionStatus(msgType msgType, string name, string admName, bool isdebug);
    }

}

public class ProtectionStatus
{
    public string Name;
    public msgType MsgType;
    public string Enabler;
    public bool isDebug;
    public HealthData HealthData;
    public ProtectionStatus();
    public ProtectionStatus(msgType msgType, string name, string admName, bool isdebug);
}


```

---

## AdminRadar

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using UnityEngine;
using Oxide.Core;
using Oxide.Core.Plugins;

Oxide.Plugins
[Info("Player Radar", "Austinv900 & Speedy2M", "2.0.3", ResourceId = 978)]
[Description("Allows admins to have a Radar to help detect cheaters")]
 class AdminRadar : RustPlugin
{
    [PluginReference]
     Plugin Godmode;
    [PluginReference]
     Plugin Vanish;
    public AdminRadar Return();
    private static AdminRadar Instance;
     Dictionary<string, PlSettings> LoadedData;
     Dictionary<string, string> NameList;
     List<string> FilterList;
     List<string> ActiveRadars;
     class Radar : MonoBehaviour
    {
         BasePlayer player;
         Vector3 bodyheight;
         int arrowheight;
         int arrowsize;
         Vector3 textheight;
         Dictionary<string, string> ExtMessages;
         Dictionary<string, string> Messages;
        public string filter;
        public float RefreshTime;
        public bool ExtDetails;
        public float setdistance;
        public Dictionary<string, string> PlayerNameList;
        public bool players;
        public bool sleepers;
        public bool npcs;
        public bool storages;
        public bool toolcupboards;
        public bool playerbox;
        public bool arrows;
         AdminRadar ar;
         void Awake();
         void radar();
         string FindOwner(ulong id);
         bool SpectateCheck(BasePlayer player, BasePlayer target);
         string replacement(string name);
    }

     void Init();
     void Unload();
     void OnServerSave();
     void OnPlayerDisconnected(BasePlayer player);
     string permAllowed;
     bool playerRadar;
     bool ShowExtData;
     string ChatIcon;
     string ChatPrefix;
     string ConfigVersion { get; set; }
     bool Tplayer;
     bool Tstorage;
     bool Tsleeper;
     bool Ttoolcupboard;
     bool Tnpc;
     bool Tall;
     string defaultFilter;
     float defaultAllInvoke;
     float defaultAllDistance;
     float defaultPlayerInvoke;
     float defaultPlayerMaxDistance;
     float defaultSleeperInvoke;
     float defaultSleeperMaxDistance;
     float defaultstorageInvoke;
     float defaultStorageMaxDistance;
     float defaultToolCupboardInvoke;
     float defaultToolCupboardMaxDistance;
     float defaultNPCInvoke;
     float defaultNPCMaxDistance;
     float limitAllInvokeHigh;
     float limitAllInvokeLow;
     float limitPlayerInvokeHigh;
     float limitPlayerInvokeLow;
     float limitSleeperInvokeHigh;
     float limitSleeperInvokeLow;
     float limitstorageInvokeHigh;
     float limitstorageInvokeLow;
     float limitToolCupboardInvokeHigh;
     float limitToolCupboardInvokeLow;
     float limitNPCInvokeHigh;
     float limitNPCInvokeLow;
     float limitAllDistanceHigh;
     float limitAllDistanceLow;
     float limitPlayerDistanceHigh;
     float limitPlayerDistanceLow;
     float limitSleeperDistanceHigh;
     float limitSleeperDistanceLow;
     float limitstorageDistanceHigh;
     float limitstorageDistanceLow;
     float limitToolCupboardDistanceHigh;
     float limitToolCupboardDistanceLow;
     float limitNPCDistanceHigh;
     float limitNPCDistanceLow;
     bool radarboxs;
     bool radararrows;
    protected override void LoadDefaultConfig();
     void LoadMessages();
    [ChatCommand("radar")]
     void ccmdRadar(BasePlayer player, string command, string[] args);
     void ToggleRadar(BasePlayer player, string filter);
     class PlSettings
    {
        public string filter;
        public float playerinvoke;
        public float sleeperinvoke;
        public float storageinvoke;
        public float toolcupboardinvoke;
        public float npcinvoke;
        public float allinvoke;
        public float playerdistance;
        public float sleeperdistance;
        public float storagedistance;
        public float toolcupboarddistance;
        public float npcdistance;
        public float alldistance;
    }

     StoredData storedData;
     class StoredData
    {
        public Dictionary<string, PlSettings> SavedData;
    }

     void LoadSavedData();
     void SaveLoadedData();
     void CreatePlayerData(string id);
     float distanceClamp(string filter, float distance);
     float invokeClamp(string filter, float invokes);
     string filterValidation(string filter);
     string settingValidation(string arg);
     float SelectPlayerInvoke(string id, string filter);
     float SelectPlayerDistance(string id, string filter);
     void LoadNameList();
     void LoadFilterList();
     void SendMessage(BasePlayer player, string message);
     string Lang(string key, string id, object[] args);
     bool IsRadar(string id);
     bool Allowed(BasePlayer player);
    private bool HasPlayerData(string id);
     string ListToString(List<T> list, int first, string seperator);
     void SetConfig(object[] args);
     T GetConfig(T defaultVal, object[] args);
     bool RadarList(string list);
    private void SendHelpText(BasePlayer player);
}

 class Radar : MonoBehaviour
{
     BasePlayer player;
     Vector3 bodyheight;
     int arrowheight;
     int arrowsize;
     Vector3 textheight;
     Dictionary<string, string> ExtMessages;
     Dictionary<string, string> Messages;
    public string filter;
    public float RefreshTime;
    public bool ExtDetails;
    public float setdistance;
    public Dictionary<string, string> PlayerNameList;
    public bool players;
    public bool sleepers;
    public bool npcs;
    public bool storages;
    public bool toolcupboards;
    public bool playerbox;
    public bool arrows;
     AdminRadar ar;
     void Awake();
     void radar();
     string FindOwner(ulong id);
     bool SpectateCheck(BasePlayer player, BasePlayer target);
     string replacement(string name);
}

 class PlSettings
{
    public string filter;
    public float playerinvoke;
    public float sleeperinvoke;
    public float storageinvoke;
    public float toolcupboardinvoke;
    public float npcinvoke;
    public float allinvoke;
    public float playerdistance;
    public float sleeperdistance;
    public float storagedistance;
    public float toolcupboarddistance;
    public float npcdistance;
    public float alldistance;
}

 class StoredData
{
    public Dictionary<string, PlSettings> SavedData;
}


```

---

## AdminTime

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

## AdminToggle

```csharp
using System.Collections.Generic;
using System.Reflection;
using System.Linq;
using Oxide.Core;
using System;

Oxide.Plugins
[Info("Admin Toggle", "LaserHydra", "1.0.2", ResourceId = 1371)]
[Description("Toggle your admin status")]
 class AdminToggle : RustPlugin
{
    readonly FieldInfo displayname;
     class Data
    {
        public Dictionary<string, AdminData> AdminData;
    }

     Data data;
     class AdminData
    {
        public string PlayerName;
        public string AdminName;
        public bool EnabledPlayerMode;
        public AdminData(BasePlayer player);
        public AdminData();
    }

     void Loaded();
     void LoadConfig();
    protected override void LoadDefaultConfig();
     void SaveData();
     void OnPlayerInit(BasePlayer player);
     bool CheckPermission(BasePlayer player);
    [ChatCommand("setplayername")]
     void SetPlayerName(BasePlayer player, string cmd, string[] args);
    [ChatCommand("toggleadmin")]
     void ToggleAdmin(BasePlayer player);
     BasePlayer GetPlayer(string searchedPlayer, BasePlayer executer, string prefix);
     string ListToString(List<string> list, int first, string seperator);
     void SetConfig(string Arg1, object Arg2, object Arg3, object Arg4);
     void BroadcastChat(string prefix, string msg);
     void SendChatMessage(BasePlayer player, string prefix, string msg);
}

 class Data
{
    public Dictionary<string, AdminData> AdminData;
}

 class AdminData
{
    public string PlayerName;
    public string AdminName;
    public bool EnabledPlayerMode;
    public AdminData(BasePlayer player);
    public AdminData();
}


```

---

## AdvAirstrike

```csharp
using System;
using System.Collections.Generic;
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Configuration;
using Oxide.Core.Plugins;
using UnityEngine;
using System.Linq;
using Rust;
using Facepunch;
using System.Collections;

Oxide.Plugins
[Info("Advanced Airstrike", "k1lly0u", "0.1.75")]
 class AdvAirstrike : RustPlugin
{
    [PluginReference]
     Plugin Economics;
     Plugin ServerRewards;
     Plugin Spawns;
     Plugin NoEscape;
     StoredData storedData;
    private DynamicConfigFile data;
    private Dictionary<ulong, StrikeType> toggleList;
    private Dictionary<string, int> shortnameToId;
    private Dictionary<string, string> shortnameToDn;
    private Hash<ulong, double> playerCooldowns;
    private List<StrikeType> availableTypes;
    private List<RadiationEntry> radiationZones;
    private List<BaseEntity> strikeSignals;
    private static AdvAirstrike ins;
    const string cargoPlanePrefab;
    const string basicRocket;
    const string fireRocket;
    const string tankShell;
    const string heliExplosion;
    const string fireball;
    const string c4Explosion;
    const string debris;
    const string bulletExp;
    const string fireCannonEffect;
    static int layerMaskExpl;
    private void Loaded();
    private void OnServerInitialized();
    private void Unload();
    private void OnServerSave();
    private void OnExplosiveDropped(BasePlayer player, BaseEntity entity);
    private void OnExplosiveThrown(BasePlayer player, BaseEntity entity);
    private class NapalmPlane : BasePlane
    {
        private void Awake();
        public override void LaunchRocket();
    }

    private class NukePlane : BasePlane
    {
        private void Awake();
        public override void LaunchRocket();
    }

    private class SpectrePlane : BasePlane
    {
        private bool inOrbit;
        private float flightRadius;
        private Vector3 beginOrbit;
        private float distance;
        private float speed;
        private int rotations;
        private bool isLeaning;
        private void Awake();
        public override void InitializeFlightPath(Vector3 targetPos, StrikeType strikeType);
        public override void MovePlane();
        private void Rotate();
        public override void CanFireRockets();
        public override void OnAmmunitionDepleted();
    }

    private class BasePlane : MonoBehaviour
    {
        internal CargoPlane entity;
        internal RocketOptions rocketOptions;
        internal CannonOptions cannonOptions;
        internal GunOptions gunOptions;
        internal FireType fireType;
        internal StrikeType strikeType;
        internal Vector3 startPos;
        internal Vector3 endPos;
        internal Vector3 targetPos;
        internal float secondsToTake;
        internal float secondsTaken;
        internal bool isFiring;
        internal int projectilesFired;
        internal int projectilesFiredGun;
        internal string forcedType;
        internal float fireDistance;
        private AutoTurret autoTurret;
        private void Awake();
        private void Update();
        private void OnDestroy();
        public virtual void InitializeFlightPath(Vector3 targetPos, StrikeType strikeType);
        internal void CheckFireType(StrikeType strikeType);
        public virtual void CanFireRockets();
        public virtual void MovePlane();
        public void GetFlightData(Vector3 startPos, Vector3 endPos, float secondsToTake);
        public void SetFlightData(Vector3 startPos, Vector3 endPos, Vector3 targetPos, float secondsToTake);
        internal void TryFireWeapons();
        public virtual void LaunchRocket();
        public virtual void OnAmmunitionDepleted();
        internal BaseEntity FireCannon();
        internal BaseEntity FireRocket();
        internal void FireGun();
        private void UpdateFacingToTarget();
        internal float GetRandom(float accuracy);
    }

    private class RocketMonitor : MonoBehaviour
    {
         StrikeType type;
        public void SetType(StrikeType type);
        private void OnDestroy();
         FireBall SpawnFireball(Vector3 position);
        private void InitializeZone(Vector3 location);
    }

    public class NukeExplosion
    {
        private int count;
        private Vector3 position;
        private List<KeyValuePair<int, int>> blastRadius;
        public NukeExplosion();
        public NukeExplosion(Vector3 position);
        private void BeginExplosion();
        private void Next();
         IEnumerator CreateRing();
        private void ExplosionRing(Vector3 position, float radius, int amount);
        private Vector3 RandomCircle(Vector3 center, float radius, int angle);
    }

    public class RadiationEntry
    {
        public RadiationZone zone;
        public Timer time;
    }

    public class RadiationZone : MonoBehaviour
    {
         SphereCollider collider;
        private void Awake();
        public void Activate(float radius, float amount);
        private void OnDestroy();
    }

    private void DestroyZone(RadiationEntry zone);
    private void SendStrike(BasePlayer player, StrikeType type, Vector3 position, string targetName);
    private void PrepareRandomStrikes();
    private void CallRandomStrike();
    private void CallStrike(Vector3 position, string ownerName);
    private void CallSquad(Vector3 position, string ownerName);
    private void CallNuke(Vector3 position, string ownerName);
    private void CallNapalm(Vector3 position, string ownerName);
    private void CallSpectre(Vector3 position, string ownerName);
    private bool CanBuyStrike(BasePlayer player, StrikeType type);
    private void BuyStrike(BasePlayer player, StrikeType type, Vector3 targetPosition, string targetName);
    private bool CanCallStrike(BasePlayer player);
    private bool CanStrikeTarget(BasePlayer player, BasePlayer target);
    private double GrabCurrentTime();
    private CargoPlane CreatePlane();
    private bool isStrikePlane(CargoPlane plane);
    private bool isStrikeSignal(BaseEntity entity);
    private bool HasPermission(BasePlayer player, string perm);
    private Vector3 GetRandomPosition();
    private string FormatTime(double time);
    private void AddCooldownData(BasePlayer player, StrikeType type);
    private List<BasePlayer> FindPlayer(string arg);
    private T ParseType(string type);
    [ChatCommand("strike")]
     void cmdAirstrike(BasePlayer player, string command, string[] args);
    [ConsoleCommand("strike")]
     void ccmdAirstrike(ConsoleSystem.Arg arg);
    private ConfigData configData;
     class RocketOptions
    {
        [JsonProperty(PropertyName = "Speed of the rocket")]
        public float Speed { get; set; }
        [JsonProperty(PropertyName = "Damage modifier")]
        public float Damage { get; set; }
        [JsonProperty(PropertyName = "Accuracy of rocket (a lower number is more accurate)")]
        public float Accuracy { get; set; }
        [JsonProperty(PropertyName = "Accuracy of spectre rocket (a lower number is more accurate)")]
        public float SpectreAccuracy { get; set; }
        [JsonProperty(PropertyName = "Interval between rockets (seconds)")]
        public float Interval { get; set; }
        [JsonProperty(PropertyName = "Interval between rockets (seconds) (Spectre Strike)")]
        public float IntervalSpectre { get; set; }
        [JsonProperty(PropertyName = "Type of rocket (Normal, Napalm)")]
        public string Type { get; set; }
        [JsonProperty(PropertyName = "Use both rocket types")]
        public bool Mixed { get; set; }
        [JsonProperty(PropertyName = "Chance of a fire rocket (when using both types)")]
        public int FireChance { get; set; }
        [JsonProperty(PropertyName = "Amount of rockets to fire")]
        public int Amount { get; set; }
        [JsonProperty(PropertyName = "Napalm lifetime (seconds) (Napalm Strike)")]
        public float FireLife { get; set; }
        [JsonProperty(PropertyName = "Radiation lifetime (seconds) (Nuke Strike)")]
        public float RadDuration { get; set; }
        [JsonProperty(PropertyName = "Radiation intensity (Nuke Strike)")]
        public float RadIntensity { get; set; }
        [JsonProperty(PropertyName = "Radiation radius (Nuke Strike)")]
        public float RadRadius { get; set; }
        [JsonProperty(PropertyName = "Damage radius (Nuke Strike)")]
        public float NukeRadius { get; set; }
        [JsonProperty(PropertyName = "Damage amount (Nuke Strike)")]
        public float NukeAmount { get; set; }
    }

     class CannonOptions
    {
        [JsonProperty(PropertyName = "Damage modifier")]
        public float Damage { get; set; }
        [JsonProperty(PropertyName = "Accuracy of shell (a lower number is more accurate)")]
        public float Accuracy { get; set; }
        [JsonProperty(PropertyName = "Accuracy of spectre shells (a lower number is more accurate)")]
        public float SpectreAccuracy { get; set; }
        [JsonProperty(PropertyName = "Interval between shells (seconds)")]
        public float Interval { get; set; }
        [JsonProperty(PropertyName = "Interval between shells (seconds) (Spectre Strike)")]
        public float IntervalSpectre { get; set; }
        [JsonProperty(PropertyName = "Amount of shells to fire")]
        public int Amount { get; set; }
    }

     class GunOptions
    {
        [JsonProperty(PropertyName = "Amount to fire")]
        public int Amount { get; set; }
        [JsonProperty(PropertyName = "Bullet fire rate")]
        public float FireRate { get; set; }
        [JsonProperty(PropertyName = "Is explosive ammunition")]
        public bool IsExplosive { get; set; }
        [JsonProperty(PropertyName = "Accuracy of bullet spread")]
        public float Accuracy { get; set; }
        [JsonProperty(PropertyName = "Damage of bullets")]
        public float Damage { get; set; }
    }

     class CooldownOptions
    {
        [JsonProperty(PropertyName = "Use cooldown timers")]
        public bool Enabled { get; set; }
        [JsonProperty(PropertyName = "Use a global cooldown for each type")]
        public bool Combine { get; set; }
        [JsonProperty(PropertyName = "Global cooldown time")]
        public int Time { get; set; }
        [JsonProperty(PropertyName = "Cooldown time that commands can be used on other players")]
        public int PlayerTime { get; set; }
        [JsonProperty(PropertyName = "Strike cooldown times (seconds)")]
        public Dictionary<string, int> Times { get; set; }
    }

     class PlaneOptions
    {
        [JsonProperty(PropertyName = "Flight speed (meters per second)")]
        public float Speed { get; set; }
        [JsonProperty(PropertyName = "Distance from target to engage")]
        public float Distance { get; set; }
        [JsonProperty(PropertyName = "Flight radius (Spectre Strike)")]
        public float FlightRadius { get; set; }
    }

     class BuyOptions
    {
        [JsonProperty(PropertyName = "Can purchase standard strike")]
        public bool StrikeEnabled { get; set; }
        [JsonProperty(PropertyName = "Can purchase squad strike")]
        public bool SquadEnabled { get; set; }
        [JsonProperty(PropertyName = "Can purchase napalm strike")]
        public bool NapalmEnabled { get; set; }
        [JsonProperty(PropertyName = "Can purchase nuke strike")]
        public bool NukeEnabled { get; set; }
        [JsonProperty(PropertyName = "Can purchase spectre strike")]
        public bool SpectreEnabled { get; set; }
        [JsonProperty(PropertyName = "Require permission to purchase strike")]
        public bool PermissionStrike { get; set; }
        [JsonProperty(PropertyName = "Require permission to purchase squad strike")]
        public bool PermissionSquad { get; set; }
        [JsonProperty(PropertyName = "Require permission to purchase napalm strike")]
        public bool PermissionNapalm { get; set; }
        [JsonProperty(PropertyName = "Require permission to purchase nuke strike")]
        public bool PermissionNuke { get; set; }
        [JsonProperty(PropertyName = "Require permission to purchase spectre strike")]
        public bool PermissionSpectre { get; set; }
        [JsonProperty(PropertyName = "Cost to purchase a standard strike (shortname, amount)")]
        public Dictionary<string, int> StrikeCost { get; set; }
        [JsonProperty(PropertyName = "Cost to purchase a squad strike (shortname, amount)")]
        public Dictionary<string, int> SquadCost { get; set; }
        [JsonProperty(PropertyName = "Cost to purchase a napalm strike (shortname, amount)")]
        public Dictionary<string, int> NapalmCost { get; set; }
        [JsonProperty(PropertyName = "Cost to purchase a nuke strike (shortname, amount)")]
        public Dictionary<string, int> NukeCost { get; set; }
        [JsonProperty(PropertyName = "Cost to purchase a spectre strike (shortname, amount)")]
        public Dictionary<string, int> SpectreCost { get; set; }
    }

     class OtherOptions
    {
        [JsonProperty(PropertyName = "Allow players to call strikes using co-ordinates")]
        public bool AllowCoords { get; set; }
        [JsonProperty(PropertyName = "Broadcast strikes to chat")]
        public bool Broadcast { get; set; }
        [JsonProperty(PropertyName = "Broadcast player names when a strike is called")]
        public bool BroadcastNames { get; set; }
        [JsonProperty(PropertyName = "Can call standard strikes using a supply signal")]
        public bool SignalStrike { get; set; }
        [JsonProperty(PropertyName = "Can call squad strikes using a supply signal")]
        public bool SignalSquad { get; set; }
        [JsonProperty(PropertyName = "Can call napalm strikes using a supply signal")]
        public bool SignalNapalm { get; set; }
        [JsonProperty(PropertyName = "Can call nuke strikes using a supply signal")]
        public bool SignalNuke { get; set; }
        [JsonProperty(PropertyName = "Can call spectre strikes using a supply signal")]
        public bool SignalSpectre { get; set; }
        [JsonProperty(PropertyName = "Random airstrike locations (Spawnfile name)")]
        public string RandomStrikeLocations { get; set; }
        [JsonProperty(PropertyName = "Random airstrike types (Strike, Squad, Napalm, Nuke, Spectre)")]
        public List<string> RandomStrikes { get; set; }
        [JsonProperty(PropertyName = "Random timer (minimum, maximum. In seconds)")]
        public int[] RandomTimer { get; set; }
        [JsonProperty(PropertyName = "NoEscape - Prevent strikes against RaidBlocked players")]
        public bool AgainstRB { get; set; }
        [JsonProperty(PropertyName = "NoEscape - Prevent strikes from RaidBlocked players")]
        public bool FromRB { get; set; }
        [JsonProperty(PropertyName = "NoEscape - Prevent strikes against CombatBlocked players")]
        public bool AgainstCB { get; set; }
        [JsonProperty(PropertyName = "NoEscape - Prevent strikes from CombatBlocked players")]
        public bool FromCB { get; set; }
    }

     class ConfigData
    {
        [JsonProperty(PropertyName = "Strike Weapon Types (Rocket, Bullet, Cannon, Combined)")]
        public Dictionary<StrikeType, string> Types { get; set; }
        [JsonProperty(PropertyName = "Rocket Options")]
        public RocketOptions Rocket { get; set; }
        [JsonProperty(PropertyName = "Cannon Options")]
        public CannonOptions Cannon { get; set; }
        [JsonProperty(PropertyName = "Strike Gun Options")]
        public Dictionary<StrikeType, GunOptions> Gun { get; set; }
        [JsonProperty(PropertyName = "Cooldown Options")]
        public CooldownOptions Cooldown { get; set; }
        [JsonProperty(PropertyName = "Plane Options")]
        public PlaneOptions Plane { get; set; }
        [JsonProperty(PropertyName = "Purchase Options")]
        public BuyOptions Buy { get; set; }
        [JsonProperty(PropertyName = "Other Options")]
        public OtherOptions Other { get; set; }
        public Oxide.Core.VersionNumber Version { get; set; }
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    private ConfigData GetBaseConfig();
    protected override void SaveConfig();
    private void UpdateConfigValues();
    private void SaveData();
    private void LoadData();
    private void ClearData();
    private class StoredData
    {
        public Dictionary<ulong, CooldownData> cooldowns;
    }

    private class CooldownData
    {
        public double strikeCd;
        public double squadCd;
        public double napalmCd;
        public double nukeCd;
        public double spectreCd;
        public double globalCd;
    }

    private string msg(string key, string playerId);
     Dictionary<string, string> Messages;
}

private class NapalmPlane : BasePlane
{
    private void Awake();
    public override void LaunchRocket();
}

private class NukePlane : BasePlane
{
    private void Awake();
    public override void LaunchRocket();
}

private class SpectrePlane : BasePlane
{
    private bool inOrbit;
    private float flightRadius;
    private Vector3 beginOrbit;
    private float distance;
    private float speed;
    private int rotations;
    private bool isLeaning;
    private void Awake();
    public override void InitializeFlightPath(Vector3 targetPos, StrikeType strikeType);
    public override void MovePlane();
    private void Rotate();
    public override void CanFireRockets();
    public override void OnAmmunitionDepleted();
}

private class BasePlane : MonoBehaviour
{
    internal CargoPlane entity;
    internal RocketOptions rocketOptions;
    internal CannonOptions cannonOptions;
    internal GunOptions gunOptions;
    internal FireType fireType;
    internal StrikeType strikeType;
    internal Vector3 startPos;
    internal Vector3 endPos;
    internal Vector3 targetPos;
    internal float secondsToTake;
    internal float secondsTaken;
    internal bool isFiring;
    internal int projectilesFired;
    internal int projectilesFiredGun;
    internal string forcedType;
    internal float fireDistance;
    private AutoTurret autoTurret;
    private void Awake();
    private void Update();
    private void OnDestroy();
    public virtual void InitializeFlightPath(Vector3 targetPos, StrikeType strikeType);
    internal void CheckFireType(StrikeType strikeType);
    public virtual void CanFireRockets();
    public virtual void MovePlane();
    public void GetFlightData(Vector3 startPos, Vector3 endPos, float secondsToTake);
    public void SetFlightData(Vector3 startPos, Vector3 endPos, Vector3 targetPos, float secondsToTake);
    internal void TryFireWeapons();
    public virtual void LaunchRocket();
    public virtual void OnAmmunitionDepleted();
    internal BaseEntity FireCannon();
    internal BaseEntity FireRocket();
    internal void FireGun();
    private void UpdateFacingToTarget();
    internal float GetRandom(float accuracy);
}

private class RocketMonitor : MonoBehaviour
{
     StrikeType type;
    public void SetType(StrikeType type);
    private void OnDestroy();
     FireBall SpawnFireball(Vector3 position);
    private void InitializeZone(Vector3 location);
}

public class NukeExplosion
{
    private int count;
    private Vector3 position;
    private List<KeyValuePair<int, int>> blastRadius;
    public NukeExplosion();
    public NukeExplosion(Vector3 position);
    private void BeginExplosion();
    private void Next();
     IEnumerator CreateRing();
    private void ExplosionRing(Vector3 position, float radius, int amount);
    private Vector3 RandomCircle(Vector3 center, float radius, int angle);
}

public class RadiationEntry
{
    public RadiationZone zone;
    public Timer time;
}

public class RadiationZone : MonoBehaviour
{
     SphereCollider collider;
    private void Awake();
    public void Activate(float radius, float amount);
    private void OnDestroy();
}

 class RocketOptions
{
    [JsonProperty(PropertyName = "Speed of the rocket")]
    public float Speed { get; set; }
    [JsonProperty(PropertyName = "Damage modifier")]
    public float Damage { get; set; }
    [JsonProperty(PropertyName = "Accuracy of rocket (a lower number is more accurate)")]
    public float Accuracy { get; set; }
    [JsonProperty(PropertyName = "Accuracy of spectre rocket (a lower number is more accurate)")]
    public float SpectreAccuracy { get; set; }
    [JsonProperty(PropertyName = "Interval between rockets (seconds)")]
    public float Interval { get; set; }
    [JsonProperty(PropertyName = "Interval between rockets (seconds) (Spectre Strike)")]
    public float IntervalSpectre { get; set; }
    [JsonProperty(PropertyName = "Type of rocket (Normal, Napalm)")]
    public string Type { get; set; }
    [JsonProperty(PropertyName = "Use both rocket types")]
    public bool Mixed { get; set; }
    [JsonProperty(PropertyName = "Chance of a fire rocket (when using both types)")]
    public int FireChance { get; set; }
    [JsonProperty(PropertyName = "Amount of rockets to fire")]
    public int Amount { get; set; }
    [JsonProperty(PropertyName = "Napalm lifetime (seconds) (Napalm Strike)")]
    public float FireLife { get; set; }
    [JsonProperty(PropertyName = "Radiation lifetime (seconds) (Nuke Strike)")]
    public float RadDuration { get; set; }
    [JsonProperty(PropertyName = "Radiation intensity (Nuke Strike)")]
    public float RadIntensity { get; set; }
    [JsonProperty(PropertyName = "Radiation radius (Nuke Strike)")]
    public float RadRadius { get; set; }
    [JsonProperty(PropertyName = "Damage radius (Nuke Strike)")]
    public float NukeRadius { get; set; }
    [JsonProperty(PropertyName = "Damage amount (Nuke Strike)")]
    public float NukeAmount { get; set; }
}

 class CannonOptions
{
    [JsonProperty(PropertyName = "Damage modifier")]
    public float Damage { get; set; }
    [JsonProperty(PropertyName = "Accuracy of shell (a lower number is more accurate)")]
    public float Accuracy { get; set; }
    [JsonProperty(PropertyName = "Accuracy of spectre shells (a lower number is more accurate)")]
    public float SpectreAccuracy { get; set; }
    [JsonProperty(PropertyName = "Interval between shells (seconds)")]
    public float Interval { get; set; }
    [JsonProperty(PropertyName = "Interval between shells (seconds) (Spectre Strike)")]
    public float IntervalSpectre { get; set; }
    [JsonProperty(PropertyName = "Amount of shells to fire")]
    public int Amount { get; set; }
}

 class GunOptions
{
    [JsonProperty(PropertyName = "Amount to fire")]
    public int Amount { get; set; }
    [JsonProperty(PropertyName = "Bullet fire rate")]
    public float FireRate { get; set; }
    [JsonProperty(PropertyName = "Is explosive ammunition")]
    public bool IsExplosive { get; set; }
    [JsonProperty(PropertyName = "Accuracy of bullet spread")]
    public float Accuracy { get; set; }
    [JsonProperty(PropertyName = "Damage of bullets")]
    public float Damage { get; set; }
}

 class CooldownOptions
{
    [JsonProperty(PropertyName = "Use cooldown timers")]
    public bool Enabled { get; set; }
    [JsonProperty(PropertyName = "Use a global cooldown for each type")]
    public bool Combine { get; set; }
    [JsonProperty(PropertyName = "Global cooldown time")]
    public int Time { get; set; }
    [JsonProperty(PropertyName = "Cooldown time that commands can be used on other players")]
    public int PlayerTime { get; set; }
    [JsonProperty(PropertyName = "Strike cooldown times (seconds)")]
    public Dictionary<string, int> Times { get; set; }
}

 class PlaneOptions
{
    [JsonProperty(PropertyName = "Flight speed (meters per second)")]
    public float Speed { get; set; }
    [JsonProperty(PropertyName = "Distance from target to engage")]
    public float Distance { get; set; }
    [JsonProperty(PropertyName = "Flight radius (Spectre Strike)")]
    public float FlightRadius { get; set; }
}

 class BuyOptions
{
    [JsonProperty(PropertyName = "Can purchase standard strike")]
    public bool StrikeEnabled { get; set; }
    [JsonProperty(PropertyName = "Can purchase squad strike")]
    public bool SquadEnabled { get; set; }
    [JsonProperty(PropertyName = "Can purchase napalm strike")]
    public bool NapalmEnabled { get; set; }
    [JsonProperty(PropertyName = "Can purchase nuke strike")]
    public bool NukeEnabled { get; set; }
    [JsonProperty(PropertyName = "Can purchase spectre strike")]
    public bool SpectreEnabled { get; set; }
    [JsonProperty(PropertyName = "Require permission to purchase strike")]
    public bool PermissionStrike { get; set; }
    [JsonProperty(PropertyName = "Require permission to purchase squad strike")]
    public bool PermissionSquad { get; set; }
    [JsonProperty(PropertyName = "Require permission to purchase napalm strike")]
    public bool PermissionNapalm { get; set; }
    [JsonProperty(PropertyName = "Require permission to purchase nuke strike")]
    public bool PermissionNuke { get; set; }
    [JsonProperty(PropertyName = "Require permission to purchase spectre strike")]
    public bool PermissionSpectre { get; set; }
    [JsonProperty(PropertyName = "Cost to purchase a standard strike (shortname, amount)")]
    public Dictionary<string, int> StrikeCost { get; set; }
    [JsonProperty(PropertyName = "Cost to purchase a squad strike (shortname, amount)")]
    public Dictionary<string, int> SquadCost { get; set; }
    [JsonProperty(PropertyName = "Cost to purchase a napalm strike (shortname, amount)")]
    public Dictionary<string, int> NapalmCost { get; set; }
    [JsonProperty(PropertyName = "Cost to purchase a nuke strike (shortname, amount)")]
    public Dictionary<string, int> NukeCost { get; set; }
    [JsonProperty(PropertyName = "Cost to purchase a spectre strike (shortname, amount)")]
    public Dictionary<string, int> SpectreCost { get; set; }
}

 class OtherOptions
{
    [JsonProperty(PropertyName = "Allow players to call strikes using co-ordinates")]
    public bool AllowCoords { get; set; }
    [JsonProperty(PropertyName = "Broadcast strikes to chat")]
    public bool Broadcast { get; set; }
    [JsonProperty(PropertyName = "Broadcast player names when a strike is called")]
    public bool BroadcastNames { get; set; }
    [JsonProperty(PropertyName = "Can call standard strikes using a supply signal")]
    public bool SignalStrike { get; set; }
    [JsonProperty(PropertyName = "Can call squad strikes using a supply signal")]
    public bool SignalSquad { get; set; }
    [JsonProperty(PropertyName = "Can call napalm strikes using a supply signal")]
    public bool SignalNapalm { get; set; }
    [JsonProperty(PropertyName = "Can call nuke strikes using a supply signal")]
    public bool SignalNuke { get; set; }
    [JsonProperty(PropertyName = "Can call spectre strikes using a supply signal")]
    public bool SignalSpectre { get; set; }
    [JsonProperty(PropertyName = "Random airstrike locations (Spawnfile name)")]
    public string RandomStrikeLocations { get; set; }
    [JsonProperty(PropertyName = "Random airstrike types (Strike, Squad, Napalm, Nuke, Spectre)")]
    public List<string> RandomStrikes { get; set; }
    [JsonProperty(PropertyName = "Random timer (minimum, maximum. In seconds)")]
    public int[] RandomTimer { get; set; }
    [JsonProperty(PropertyName = "NoEscape - Prevent strikes against RaidBlocked players")]
    public bool AgainstRB { get; set; }
    [JsonProperty(PropertyName = "NoEscape - Prevent strikes from RaidBlocked players")]
    public bool FromRB { get; set; }
    [JsonProperty(PropertyName = "NoEscape - Prevent strikes against CombatBlocked players")]
    public bool AgainstCB { get; set; }
    [JsonProperty(PropertyName = "NoEscape - Prevent strikes from CombatBlocked players")]
    public bool FromCB { get; set; }
}

 class ConfigData
{
    [JsonProperty(PropertyName = "Strike Weapon Types (Rocket, Bullet, Cannon, Combined)")]
    public Dictionary<StrikeType, string> Types { get; set; }
    [JsonProperty(PropertyName = "Rocket Options")]
    public RocketOptions Rocket { get; set; }
    [JsonProperty(PropertyName = "Cannon Options")]
    public CannonOptions Cannon { get; set; }
    [JsonProperty(PropertyName = "Strike Gun Options")]
    public Dictionary<StrikeType, GunOptions> Gun { get; set; }
    [JsonProperty(PropertyName = "Cooldown Options")]
    public CooldownOptions Cooldown { get; set; }
    [JsonProperty(PropertyName = "Plane Options")]
    public PlaneOptions Plane { get; set; }
    [JsonProperty(PropertyName = "Purchase Options")]
    public BuyOptions Buy { get; set; }
    [JsonProperty(PropertyName = "Other Options")]
    public OtherOptions Other { get; set; }
    public Oxide.Core.VersionNumber Version { get; set; }
}

private class StoredData
{
    public Dictionary<ulong, CooldownData> cooldowns;
}

private class CooldownData
{
    public double strikeCd;
    public double squadCd;
    public double napalmCd;
    public double nukeCd;
    public double spectreCd;
    public double globalCd;
}


```

---

## AFKChecker

```csharp
using System;
using System.Collections.Generic;
using Newtonsoft.Json;
using UnityEngine;

Oxide.Plugins
[Info("AFKChecker", "Автор Hougan, дополнил FREDWAY и Ryamkk", "0.0.1")]
[Description("Проверяет игрока на длительность отсутствия за игрокй")]
public class AFKChecker : RustPlugin
{
    private string CheckPermission;
    private string IgnorePermission;
    private int Interval;
    private ulong senderID;
    private string header;
    private string prefixcolor;
    private string prefixsize;
    private void LoadDefaultConfig();
    [JsonProperty("Словарь хранящий время АФК игроков")]
    private Dictionary<ulong, Player> afkDictionary;
    private class Player
    {
        [JsonProperty("Время без движений")]
        public int AFKTime;
        [JsonProperty("Прочие действия в этом интервале")]
        public bool Actions;
        [JsonProperty("Последняя позиция")]
        public Vector3 LastPosition;
    }

    private void OnServerInitialized();
    private void OnPlayerDisconnected(BasePlayer player, string reason);
    private void OnPlayerChat(ConsoleSystem.Arg args);
    private void OnLootEntity(BasePlayer player, BaseEntity entity);
    private void OnLootEntityEnd(BasePlayer player, BaseCombatEntity entity);
    private void TrackAFK();
    [ChatCommand("afkcheck")]
    private void cmdAFKCheck(BasePlayer player, string command, string[] args);
    private bool IsAFK(BasePlayer player);
    private List<BasePlayer> FindPlayers(string nameOrId);
    public void ReplyWithHelper(BasePlayer player, string message, string[] args);
    private void GetConfig(string menu, string Key, T var);
}

private class Player
{
    [JsonProperty("Время без движений")]
    public int AFKTime;
    [JsonProperty("Прочие действия в этом интервале")]
    public bool Actions;
    [JsonProperty("Последняя позиция")]
    public Vector3 LastPosition;
}


```

---

## AimTrain

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using UnityEngine;
using Oxide.Core;
using Newtonsoft.Json;
using Oxide.Core.Plugins;
using System.Collections;
using Oxide.Game.Rust.Cui;
using UI = Oxide.Plugins.AimTrainUI.UIMethods;
using Anchor = Oxide.Plugins.AimTrainUI.Anchor;

Oxide.Plugins
[Info("AimTrain", "iAciid", "1.1.2")]
public class AimTrain : RustPlugin
{
    [PluginReference]
     Plugin Kits;
     Plugin NoEscape;
     Configuration config;
     StoredData storedData;
     Dictionary<string, Arena> ArenasCache;
     Dictionary<ulong, PlayerData> PlayersCache;
     Dictionary<ulong, string> EditArena;
     List<ulong> NoAmmo;
    public static AimTrain Instance;
     string MainContainer;
     string StatsContainer;
     CuiElementContainer CachedContainer;
    public class Configuration
    {
        [JsonProperty(PropertyName = "Enable AimTrain")]
        public bool EnableAimTrain;
        [JsonProperty(PropertyName = "Use permissons for Arena")]
        public bool ArenaPermission;
        [JsonProperty(PropertyName = "Needs empty inventory to join")]
        public bool IgnoreInv;
        [JsonProperty(PropertyName = "Enable UI")]
        public bool EnableUI;
        [JsonProperty(PropertyName = "Use NoEscape Raid/Combatblock")]
        public bool UseNoEscape;
        [JsonProperty(PropertyName = "Bot Names")]
        public List<string> BotNames;
        public static Configuration DefaultConfig();
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    protected override void LoadDefaultMessages();
    public class Position
    {
        public float PosX;
        public float PosY;
        public float PosZ;
        public Position();
        public Position(float x, float y, float z);
        public Vector3 ToVector3();
    }

    public class Arena
    {
        public bool Enabled;
        public string PlayerKit;
        public List<string> Kits;
        public int BotCount;
        public bool BotMoving;
        public Dictionary<int, Position> SpawnsBot;
        public Dictionary<int, Position> SpawnsPlayer;
        [JsonIgnore]
        public int Players;
        public Arena();
    }

    public class PlayerData
    {
        public string Arena;
        public Position position;
        public int Hits;
        public int Bullets;
        public int Headshots;
        public PlayerData();
    }

    public class StoredData
    {
        public Dictionary<string, Arena> Arenas;
        public StoredData();
    }

     void ClearBots(string arenaName);
     void DeleteAll();
     void SaveCacheData();
     IEnumerator ChangeBotAmount(int bots, string arenaName);
     void MovePlayer(BasePlayer player, Vector3 pos);
     void ChangeBotCount(int amount, string arenaName);
     void CreateBot(string arenaName);
     string Lang(string key, string id, object[] args);
     void JoinAT(BasePlayer player, string arenaName);
     void LeaveAT(BasePlayer player);
     void StripPlayer(BasePlayer player);
     void StripContainer(ItemContainer container);
     string AmmoStatus(ulong playerID);
     bool IsAimTraining(ulong playerID);
     void Init();
     object CanTrade(BasePlayer player);
     object CanTeleport(BasePlayer player);
     object CanBank(BasePlayer player);
     void OnPlayerAttack(BasePlayer attacker, HitInfo info);
     void OnWeaponFired(BaseProjectile projectile, BasePlayer player);
     void OnItemDropped(Item item, BaseEntity entity);
     void OnPlayerDisconnected(BasePlayer player, string reason);
     object OnPlayerDie(BasePlayer player, HitInfo info);
     object CanBeWounded(BasePlayer player, HitInfo info);
     void OnServerSave();
     void Unload();
    public class Bot : MonoBehaviour
    {
         BasePlayer bot;
        public Boolean _isLerping;
         Vector3 startPos;
         Vector3 endPos;
         float timeTakenDuringLerp;
        public float minSpeed;
        public float maxSpeed;
         float lastDelta;
        public string arena;
         void SetViewAngle(Quaternion viewAngles);
         void Start();
         void StartLerping();
        public float GetGroundY(Vector3 position);
         void FixedUpdate();
    }

     void UpdateTimer(BasePlayer player);
     void NewBorder(Anchor Min, Anchor Max);
     CuiElementContainer DrawTimer(BasePlayer player);
     void ConstructUi();
    [ChatCommand("at")]
     void cmdAT(BasePlayer player, string command, string[] args);
    [ChatCommand("at_edit")]
     void cmdATEdit(BasePlayer player, string command, string[] args);
    [ChatCommand("aimtrain")]
     void cmdAimTrain(BasePlayer player, string command, string[] args);
    [ConsoleCommand("LeaveAT")]
     void cmdUILeaveAT(ConsoleSystem.Arg arg);
    [ConsoleCommand("AmmoAT")]
     void cmdUIAmmoAT(ConsoleSystem.Arg arg);
    [ConsoleCommand("ResetAT")]
     void cmdUIResetAT(ConsoleSystem.Arg arg);
}

public class Configuration
{
    [JsonProperty(PropertyName = "Enable AimTrain")]
    public bool EnableAimTrain;
    [JsonProperty(PropertyName = "Use permissons for Arena")]
    public bool ArenaPermission;
    [JsonProperty(PropertyName = "Needs empty inventory to join")]
    public bool IgnoreInv;
    [JsonProperty(PropertyName = "Enable UI")]
    public bool EnableUI;
    [JsonProperty(PropertyName = "Use NoEscape Raid/Combatblock")]
    public bool UseNoEscape;
    [JsonProperty(PropertyName = "Bot Names")]
    public List<string> BotNames;
    public static Configuration DefaultConfig();
}

public class Position
{
    public float PosX;
    public float PosY;
    public float PosZ;
    public Position();
    public Position(float x, float y, float z);
    public Vector3 ToVector3();
}

public class Arena
{
    public bool Enabled;
    public string PlayerKit;
    public List<string> Kits;
    public int BotCount;
    public bool BotMoving;
    public Dictionary<int, Position> SpawnsBot;
    public Dictionary<int, Position> SpawnsPlayer;
    [JsonIgnore]
    public int Players;
    public Arena();
}

public class PlayerData
{
    public string Arena;
    public Position position;
    public int Hits;
    public int Bullets;
    public int Headshots;
    public PlayerData();
}

public class StoredData
{
    public Dictionary<string, Arena> Arenas;
    public StoredData();
}

public class Bot : MonoBehaviour
{
     BasePlayer bot;
    public Boolean _isLerping;
     Vector3 startPos;
     Vector3 endPos;
     float timeTakenDuringLerp;
    public float minSpeed;
    public float maxSpeed;
     float lastDelta;
    public string arena;
     void SetViewAngle(Quaternion viewAngles);
     void Start();
     void StartLerping();
    public float GetGroundY(Vector3 position);
     void FixedUpdate();
}

Oxide.Plugins.AimTrainUI
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

## AirdropControl

```csharp
using System;
using System.Collections.Generic;
using System.Reflection;
using UnityEngine;
using Oxide.Core;

Oxide.Plugins
[Info("AirdropControl", "Reneb", "1.1.6")]
 class AirdropControl : RustPlugin
{
    private FieldInfo CPstartPos;
    private FieldInfo CPendPos;
    private FieldInfo CPdropped;
    private FieldInfo CPsecondsToTake;
    private FieldInfo CPsecondsTaken;
    private FieldInfo dropPosition;
    private MethodInfo CreateEntity;
    private Vector3 centerPos;
    private float secondsToTake;
    private BaseEntity cargoplane;
    private Dictionary<CargoPlane, Vector3> dropPoint;
    private Dictionary<CargoPlane, int> dropNumber;
    private Dictionary<CargoPlane, double> nextDrop;
    private static readonly DateTime epoch;
    private float dropMinX;
    private float dropMaxX;
    private float dropMinY;
    private float dropMaxY;
    private float dropMinZ;
    private float dropMaxZ;
    private string dropMessage;
    private int dropMinCrates;
    private int dropMaxCrates;
    private float airdropSpeed;
    private bool showDropLocation;
    private bool Changed;
    private System.Random getrandom;
    private object syncLock;
    private int minDropCratesInterval;
    private int maxDropCratesInterval;
    private double nextCheck;
    private List<CargoPlane> RemoveListND;
    private Dictionary<CargoPlane, int> RemoveListNUM;
    private Dictionary<CargoPlane,double> AddDrop;
    private Quaternion defaultRot;
     void Loaded();
     object GetConfig(string menu, string datavalue, object defaultValue);
     void LoadVariables();
     void LoadDefaultConfig();
     int GetRandomNumber(int min, int max);
     double CurrentTime();
     Vector3 FindDropPoint(CargoPlane cargoplane);
     int RandomCrateDrop(CargoPlane cargoplane);
     double RandomDropInterval();
     Vector3 RandomDropPoint();
     void FindStartAndEndPos(Vector3 target, Vector3 startpos, Vector3 endpos, float distance);
     void BroadcastToChat(string msg);
     void OnTick();
     void CheckAirdropDrops();
     void OnEntitySpawned(BaseEntity entity);
     void AllowNextDrop();
    [ConsoleCommand("airdrop.toplayer")]
     void cmdConsoleAirdropToPlayer(ConsoleSystem.Arg arg);
    [ConsoleCommand("airdrop.topos")]
     void cmdConsoleAirdropToPos(ConsoleSystem.Arg arg);
    [ConsoleCommand("airdrop.massdrop")]
     void cmdConsoleAirdropMassDrop(ConsoleSystem.Arg arg);
}


```

---

## AirdropPrecision

```csharp
using UnityEngine;
using System.Reflection;
using System.Collections.Generic;

Oxide.Plugins
[Info("Airdrop Precision", "k1lly0u", "0.1.1", ResourceId = 2074)]
 class AirdropPrecision : RustPlugin
{
    private FieldInfo dropPosition;
     List<Vector3> thrownSignals;
     void OnServerInitialized();
     void OnExplosiveThrown(BasePlayer player, BaseEntity entity);
     void OnEntitySpawned(BaseEntity entity);
}


```

---

## AirdropRandomizer

```csharp
using UnityEngine;
using System.Reflection;

Oxide.Plugins
[Info("Airdrop Randomizer", "k1lly0u", "0.1.1", ResourceId = 1898)]
 class AirdropRandomizer : RustPlugin
{
    private FieldInfo dropPosition;
     void OnServerInitialized();
     void OnEntitySpawned(BaseEntity entity);
    private ConfigData configData;
     class ConfigData
    {
        public float MaxDistance { get; set; }
    }

    private void LoadVariables();
    protected override void LoadDefaultConfig();
    private void LoadConfigVariables();
     void SaveConfig(ConfigData config);
}

 class ConfigData
{
    public float MaxDistance { get; set; }
}


```

---

## AirMarker

```csharp
using System;
using UnityEngine;

Oxide.Plugins
[Info("AirMarker", "Fierrov", "1.0.0")]
internal class AirMarker : RustPlugin
{
    private void OnServerInitialized();
    private void Unload();
     void OnEntitySpawned(SupplyDrop air);
     void OnLootEntity(BasePlayer player, SupplyDrop air);
     class Marker : MonoBehaviour
    {
        private BaseEntity entity;
         MapMarkerGenericRadius mapmarker;
        private void Awake();
        private void InstallMarker();
        public void Kill();
    }

}

 class Marker : MonoBehaviour
{
    private BaseEntity entity;
     MapMarkerGenericRadius mapmarker;
    private void Awake();
    private void InstallMarker();
    public void Kill();
}


```

---

## Airstrike

```csharp
using System;
using System.Collections.Generic;
using UnityEngine;
using Rust;
using System.Reflection;
using Oxide.Core.Plugins;
using Oxide.Core.Configuration;
using Oxide.Core;

Oxide.Plugins
[Info("Airstrike", "k1lly0u", "0.2.32", ResourceId = 1489)]
 class Airstrike : RustPlugin
{
    [PluginReference]
     Plugin Economics;
    private bool changed;
    private float rocketDrop;
    static FieldInfo TimeToTake;
    static FieldInfo StartPos;
    static FieldInfo EndPos;
    static FieldInfo DropPos;
    public Dictionary<ulong, bool> toggleList;
    private List<StrikePlane> StrikePlanes;
     PlayerCooldown pcdData;
    private DynamicConfigFile PCDDATA;
    public class StrikePlane : MonoBehaviour
    {
        public CargoPlane Plane;
        public Vector3 TargetPosition;
        public int FiredRockets;
        public void SpawnPlane(CargoPlane plane, Vector3 position, float speed, bool isSquad, Vector3 offset);
        private Vector3 CalculateSpawnPos(Vector3 offset);
        private void CheckDistance();
        private void FireRockets(CargoPlane strikePlane, Vector3 targetPos);
        private void SpreadRockets();
        private void LaunchRocket(Vector3 targetPos);
    }

    private CargoPlane CreatePlane();
     void Loaded();
     void OnServerInitialized();
     void Unload();
     void OnExplosiveThrown(BasePlayer player, BaseEntity entity);
     void OnEntitySpawned(BaseEntity entity);
    private void RegisterPermissions();
    private void RegisterMessages();
    private void SetFieldInfo();
    private void CheckDependencies();
    public const string cargoPlanePrefab;
    private void DestroyPlane(StrikePlane plane);
    private void DestroyAllPlanes();
    public bool CheckStrikeDrop(SupplyDrop drop);
    private void MassSet(Vector3 position, Vector3 offset, int speed);
    private Vector3 calculateEndPos(Vector3 pos, Vector3 offset);
    private void strikeOnPayment(BasePlayer player, string type);
    private void squadStrikeOnPayment(BasePlayer player, bool type);
    private bool CheckPlayerMoney(BasePlayer player, int amount);
    private bool CheckPlayerData(BasePlayer player);
    static double GrabCurrentTime();
    private void strikeOnPlayer(BasePlayer player);
    private void squadStrikeOnPlayer(BasePlayer player);
    private void strikeOnSmoke(BasePlayer player, Vector3 strikePos);
    private void callStrike(Vector3 position, int speed);
    private void massStrike(Vector3 position);
    private void callPaymentStrike(BasePlayer player);
    private void callPaymentSquadStrike(BasePlayer player);
    private void callRandomStrike(bool single);
     List<BasePlayer> FindPlayer(string arg);
    private bool isStrikePlane(CargoPlane plane);
     bool canChatStrike(BasePlayer player);
     bool canSquadStrike(BasePlayer player);
     bool canSmokeStrike(BasePlayer player);
     bool canBuyStrike(BasePlayer player);
     bool isAuth(ConsoleSystem.Arg arg);
    [ChatCommand("callstrike")]
    private void chatStrike(BasePlayer player, string command, string[] args);
    [ChatCommand("squadstrike")]
    private void chatsquadStrike(BasePlayer player, string command, string[] args);
    [ChatCommand("buystrike")]
    private void chatBuyStrike(BasePlayer player, string command, string[] args);
    [ChatCommand("togglestrike")]
    private void chatToggleStrike(BasePlayer player, string command, string[] args);
    [ConsoleCommand("airstrike")]
     void ccmdAirstrike(ConsoleSystem.Arg arg);
    [ConsoleCommand("squadstrike")]
     void ccmdsquadstrike(ConsoleSystem.Arg arg);
     class PlayerCooldown
    {
        public Dictionary<ulong, PCDInfo> pCooldown;
        public PlayerCooldown();
    }

     class PCDInfo
    {
        public long Cooldown;
        public PCDInfo();
        public PCDInfo(long cd);
    }

     void SaveData();
     void LoadData();
    private static bool broadcastStrikeAll;
    private static bool useSignalStrike;
    private static bool usePaymentStrike;
    private static bool usePaymentSquad;
    private static bool useEconomics;
    private static bool useCooldown;
    private static bool noAdminCooldown;
    private static bool useMixedRockets;
    private static bool changeSupplyStrike;
    private static float rocketSpeed;
    private static float rocketInterval;
    private static float damageModifier;
    private static float rocketSpread;
    private static int rocketAmount;
    private static int planeSpeed;
    private static int planeDistance;
    private static int buyMetal;
    private static int buyFlare;
    private static int buyTarget;
    private static int buyMoney;
    private static int cooldownTime;
    private static int auth;
    private static int fireChance;
    private static string normalRocket;
    private static string fireRocket;
    private static string rocketType;
    protected override void LoadDefaultConfig();
    private void LoadVariables();
    private void LoadConfigVariables();
    private void CheckCfg(string Key, T var);
    private void CheckCfgFloat(string Key, float var);
     object GetConfig(string menu, string datavalue, object defaultValue);
     Dictionary<string, string> messages;
}

public class StrikePlane : MonoBehaviour
{
    public CargoPlane Plane;
    public Vector3 TargetPosition;
    public int FiredRockets;
    public void SpawnPlane(CargoPlane plane, Vector3 position, float speed, bool isSquad, Vector3 offset);
    private Vector3 CalculateSpawnPos(Vector3 offset);
    private void CheckDistance();
    private void FireRockets(CargoPlane strikePlane, Vector3 targetPos);
    private void SpreadRockets();
    private void LaunchRocket(Vector3 targetPos);
}

 class PlayerCooldown
{
    public Dictionary<ulong, PCDInfo> pCooldown;
    public PlayerCooldown();
}

 class PCDInfo
{
    public long Cooldown;
    public PCDInfo();
    public PCDInfo(long cd);
}


```

---

## AliasSystem

```csharp
using System.Collections.Generic;
using Oxide.Game.Rust.Libraries;
using System.Linq;
using Oxide.Core;
using System;

Oxide.Plugins
[Info("Alias System", "LaserHydra", "2.0.0", ResourceId = 1307)]
[Description("Setup alias for chat and console commands")]
 class AliasSystem : RustPlugin
{
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

## AlphaLoot

```csharp
using Facepunch;
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Configuration;
using Oxide.Core.Plugins;
using Steamworks;
using System.Collections.Generic;
using System.Linq;
using UnityEngine;

Oxide.Plugins
[Info("AlphaLoot", "k1lly0u", "3.1.3")]
 class AlphaLoot : RustPlugin
{
    [PluginReference]
     Plugin CustomLootSpawns;
     Plugin EventLoot;
     Plugin FancyDrop;
     Plugin SkinBox;
    private StoredData storedData;
    private StoredData bradleyData;
    private StoredData heliData;
    private DynamicConfigFile data;
    private DynamicConfigFile bradley;
    private DynamicConfigFile heli;
    private DynamicConfigFile skinData;
    private bool updateContainerCapacities;
    private static Hash<string, HashSet<SkinEntry>> weightedSkinIds;
    private static Hash<string, List<ulong>> importedSkinIds;
    private static Hash<string, int> defaultScrapAmounts;
    private const string ADMIN_PERMISSION;
    private const string HELI_CRATE;
    private const string BRADLEY_CRATE;
    private void Loaded();
    private void OnServerInitialized();
    private void OnEntitySpawned(BradleyAPC bradleyApc);
    private void OnEntitySpawned(BaseHelicopter baseHelicopter);
    private object OnCorpsePopulate(BaseEntity entity, LootableCorpse corpse);
    private object OnLootSpawn(LootContainer container);
    private void OnHammerHit(BasePlayer player, HitInfo info);
    private void OnLootEntityEnd(BasePlayer player, StorageContainer container);
    private void Unload();
    private void LoadSkins();
    private void PopulateSkinListFromSkinBox();
    private void PopulateSkinListFromApproved();
    private void RefreshLootContents(ConsoleSystem.Arg arg, bool setDefaultScrap);
    private void PopulateContainerDefinitions(StoredData storedData, StoredData heliData, StoredData bradleyData);
    private bool PopulateLoot(LootContainer container);
    private void PopulateLootContainer(LootContainer container, BaseLootContainerProfile lootProfile);
    private bool PopulateLoot(BaseEntity entity, LootableCorpse corpse);
    private string ToShortName(string name);
    private LootContainer FindContainer(BasePlayer player);
    private object WantsToHandleFancyDropLoot();
    private void AutoUpdateItemLists();
    private void AddSpecifiedItemsToLootTable(string[] args);
    private void AddItemsToLootTable(List<int> items, int additions);
    private void FindItemAndCalculateScoreRecursive(LootSpawn lootSpawn, string shortname, NewLootItem newLootItem, int multiplier);
    private class NewLootItem
    {
        public ItemAmountRanged Item;
        public float Score;
        public bool HasItem { get; set; }
        public void Clear();
    }

    private class ItemList
    {
        public List<int> itemIds;
        public string protocol;
    }

    private class LootCycler : MonoBehaviour
    {
        private LootContainer lootContainer;
        private void Awake();
    }

    [ChatCommand("aloot")]
    private void cmdRepopulateTarget(BasePlayer player, string command, string[] args);
    [ConsoleCommand("al.repopulateall")]
    private void ccmdRepopulateAll(ConsoleSystem.Arg arg);
    [ConsoleCommand("al.additems")]
    private void ccmdAddItemsl(ConsoleSystem.Arg arg);
    [ConsoleCommand("al.search")]
    private void ccmdSearchItem(ConsoleSystem.Arg arg);
    private void FindItemCountRecursive(LootSpawn lootSpawn, string shortname, int count);
    [ConsoleCommand("al.setloottable")]
    private void ccmdChangeConfig(ConsoleSystem.Arg arg);
    [ConsoleCommand("al.setheliloottable")]
    private void ccmdChangeHeliConfig(ConsoleSystem.Arg arg);
    [ConsoleCommand("al.setbradleyloottable")]
    private void ccmdChangeBradleyConfig(ConsoleSystem.Arg arg);
    [ConsoleCommand("al.generatetable")]
    private void ccmdGenerateTable(ConsoleSystem.Arg arg);
    [ConsoleCommand("al.skins")]
    private void ccmdALSkins(ConsoleSystem.Arg arg);
    private static ConfigData configData;
    private class ConfigData
    {
        [JsonProperty(PropertyName = "Auto-update loot tables with new items")]
        public bool AutoUpdate { get; set; }
        [JsonProperty(PropertyName = "Global Loot Multiplier (multiplies all loot amounts by the number specified)")]
        public float GlobalMultiplier { get; set; }
        [JsonProperty(PropertyName = "Apply global and individual loot multipliers to un-stackable items")]
        public bool MultiplyUnstackable { get; set; }
        [JsonProperty(PropertyName = "Loot Table Name")]
        public string ProfileName { get; set; }
        [JsonProperty(PropertyName = "Heli Loot Table Name")]
        public string HeliProfileName { get; set; }
        [JsonProperty(PropertyName = "Bradley Loot Table Name")]
        public string BradleyProfileName { get; set; }
        [JsonProperty(PropertyName = "Amount of crates to drop (Bradley APC - default 3)")]
        public int BradleyCrates { get; set; }
        [JsonProperty(PropertyName = "Amount of crates to drop (Patrol Helicopter - default 4)")]
        public int HelicopterCrates { get; set; }
        [JsonProperty(PropertyName = "Override FancyDrop containers with supply drop profile")]
        public bool OverrideFancyDrop { get; set; }
        [JsonProperty(PropertyName = "Use skins from the SkinBox skin list")]
        public bool UseSkinboxSkins { get; set; }
        [JsonProperty(PropertyName = "Use skins from the approved skin list")]
        public bool UseApprovedSkins { get; set; }
        public Oxide.Core.VersionNumber Version { get; set; }
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    private ConfigData GetBaseConfig();
    protected override void SaveConfig();
    private void UpdateConfigValues();
    private void SaveData();
    private void LoadData();
    private void LoadLootTable();
    private void LoadHeliTable();
    private void LoadBradleyTable();
    private void LoadSkinsData();
    private void SetCapacityLimits();
    public class BaseLootProfile
    {
        public bool Enabled;
        public bool AllowSkinnedItems;
        public float LootMultiplier;
        public int MinScrapAmount;
        public int MaxScrapAmount;
        [JsonIgnore]
        private static ItemDefinition _scrapDefinition;
        [JsonIgnore]
        public ItemDefinition ScrapDefinition { get; set; }
        [JsonIgnore]
        private static ItemDefinition _blueprintBase;
        [JsonIgnore]
        public ItemDefinition BlueprintBaseDefinition { get; set; }
        public int GetScrapAmount();
        public virtual void PopulateLoot(ItemContainer container);
    }

    public class BaseLootContainerProfile : BaseLootProfile
    {
        public bool DestroyOnEmpty;
        public bool ShouldRefreshContents;
        public int MinSecondsBetweenRefresh;
        public int MaxSecondsBetweenRefresh;
    }

    public class SimpleNPCLootProfile : BaseLootProfile
    {
        public int MinimumItems;
        public int MaximumItems;
        public ItemAmountWeighted[] Items;
        public SimpleNPCLootProfile();
        public SimpleNPCLootProfile(SimpleNPCLootProfile lootContainerProfile);
        public override void PopulateLoot(ItemContainer container);
    }

    public class AdvancedNPCLootProfile : BaseLootProfile
    {
        public LootSpawnSlot[] LootSpawnSlots;
        public int MaximumItems;
        public AdvancedNPCLootProfile();
        public AdvancedNPCLootProfile(LootContainer.LootSpawnSlot[] lootSpawnSlots);
        public AdvancedNPCLootProfile(AdvancedNPCLootProfile lootContainerProfile);
        public override void PopulateLoot(ItemContainer container);
    }

    public class SimpleLootContainerProfile : BaseLootContainerProfile
    {
        public int MinimumItems;
        public int MaximumItems;
        public ItemAmountWeighted[] Items;
        public SimpleLootContainerProfile();
        public SimpleLootContainerProfile(SimpleLootContainerProfile lootContainerProfile);
        public override void PopulateLoot(ItemContainer container);
    }

    public class AdvancedLootContainerProfile : BaseLootContainerProfile
    {
        public LootSpawnSlot[] LootSpawnSlots;
        public int MaximumItems;
        public AdvancedLootContainerProfile();
        public AdvancedLootContainerProfile(LootContainer container);
        public AdvancedLootContainerProfile(AdvancedLootContainerProfile lootContainerProfile);
        public override void PopulateLoot(ItemContainer container);
    }

    public class LootSpawnSlot
    {
        public LootSpawn LootDefinition;
        public int NumberToSpawn;
        public float Probability;
        public LootSpawnSlot();
        public LootSpawnSlot(global::LootSpawn lootSpawn, int numberToSpawn, bool hasCondition);
        public LootSpawnSlot(LootContainer.LootSpawnSlot lootSpawnSlot, bool hasCondition);
    }

    public class LootSpawn
    {
        public ItemAmountRanged[] Items;
        public Entry[] SubSpawn;
        public byte[] Node;
        public LootSpawn();
        public LootSpawn(global::LootSpawn lootSpawn, bool hasCondition);
        public void SpawnIntoContainer(ItemContainer container, BaseLootProfile lootProfile);
        private void SubCategoryIntoContainer(ItemContainer container, BaseLootProfile lootProfile);
        public class Entry
        {
            public LootSpawn Category;
            public int Weight;
            public byte[] Node;
        }

    }

    public class ItemAmountWeighted : ItemAmountRanged
    {
        public int Weight;
    }

    public class ItemAmountRanged : ItemAmount
    {
        public float MaxAmount;
        public ItemAmountRanged();
        public ItemAmountRanged(ItemDefinition item, float amount, float maxAmount, bool hasCondition);
        public override float GetAmount(float lootMultiplier);
    }

    public class ItemAmount
    {
        public string Shortname;
        public float BlueprintChance;
        public float MinAmount;
        public ConditionItem Condition;
        [JsonIgnore]
        private int _itemId;
        [JsonIgnore]
        public int ItemID { get; set; }
        public ItemAmount();
        public ItemAmount(ItemDefinition item, float amount, bool hasCondition);
        public virtual float GetAmount(float lootMultiplier);
        public ulong RandomSkinID();
        public float GetConditionFraction();
        public bool WantsBlueprint();
        public class ConditionItem
        {
            public float MinCondition;
            public float MaxCondition;
        }

    }

    public class SkinEntry
    {
        public int Weight;
        public ulong SkinID;
        public SkinEntry();
        public SkinEntry(ulong skinId, int weight);
    }

    private class StoredData
    {
        public Hash<string, SimpleLootContainerProfile> loot_simple;
        public Hash<string, AdvancedLootContainerProfile> loot_advanced;
        public Hash<string, AdvancedNPCLootProfile> npcs_advanced;
        public Hash<string, SimpleNPCLootProfile> npcs_simple;
        public bool IsBaseLootTable;
        public string ProfileName;
        [JsonIgnore]
        public bool IsValid { get; set; }
        [JsonIgnore]
        public bool HasAnyProfiles { get; set; }
        public void CreateDefaultLootProfile(LootContainer container);
        public void CloneLootProfile(string shortname, BaseLootProfile lootContainerProfile);
        public void CreateDefaultLootProfile(string shortPrefabName, LootContainer.LootSpawnSlot[] lootSpawnSlots);
        public bool TryGetLootProfile(string shortname, BaseLootContainerProfile profile);
        public bool TryGetNPCProfile(string shortname, BaseLootProfile profile);
        [JsonIgnore]
        private List<BaseLootContainerProfile> randomList;
        public bool GetRandomLootProfile(BaseLootContainerProfile profile);
        public void RemoveProfile(string shortname);
    }

    private List<string> npcPrefabs;
}

private class NewLootItem
{
    public ItemAmountRanged Item;
    public float Score;
    public bool HasItem { get; set; }
    public void Clear();
}

private class ItemList
{
    public List<int> itemIds;
    public string protocol;
}

private class LootCycler : MonoBehaviour
{
    private LootContainer lootContainer;
    private void Awake();
}

private class ConfigData
{
    [JsonProperty(PropertyName = "Auto-update loot tables with new items")]
    public bool AutoUpdate { get; set; }
    [JsonProperty(PropertyName = "Global Loot Multiplier (multiplies all loot amounts by the number specified)")]
    public float GlobalMultiplier { get; set; }
    [JsonProperty(PropertyName = "Apply global and individual loot multipliers to un-stackable items")]
    public bool MultiplyUnstackable { get; set; }
    [JsonProperty(PropertyName = "Loot Table Name")]
    public string ProfileName { get; set; }
    [JsonProperty(PropertyName = "Heli Loot Table Name")]
    public string HeliProfileName { get; set; }
    [JsonProperty(PropertyName = "Bradley Loot Table Name")]
    public string BradleyProfileName { get; set; }
    [JsonProperty(PropertyName = "Amount of crates to drop (Bradley APC - default 3)")]
    public int BradleyCrates { get; set; }
    [JsonProperty(PropertyName = "Amount of crates to drop (Patrol Helicopter - default 4)")]
    public int HelicopterCrates { get; set; }
    [JsonProperty(PropertyName = "Override FancyDrop containers with supply drop profile")]
    public bool OverrideFancyDrop { get; set; }
    [JsonProperty(PropertyName = "Use skins from the SkinBox skin list")]
    public bool UseSkinboxSkins { get; set; }
    [JsonProperty(PropertyName = "Use skins from the approved skin list")]
    public bool UseApprovedSkins { get; set; }
    public Oxide.Core.VersionNumber Version { get; set; }
}

public class BaseLootProfile
{
    public bool Enabled;
    public bool AllowSkinnedItems;
    public float LootMultiplier;
    public int MinScrapAmount;
    public int MaxScrapAmount;
    [JsonIgnore]
    private static ItemDefinition _scrapDefinition;
    [JsonIgnore]
    public ItemDefinition ScrapDefinition { get; set; }
    [JsonIgnore]
    private static ItemDefinition _blueprintBase;
    [JsonIgnore]
    public ItemDefinition BlueprintBaseDefinition { get; set; }
    public int GetScrapAmount();
    public virtual void PopulateLoot(ItemContainer container);
}

public class BaseLootContainerProfile : BaseLootProfile
{
    public bool DestroyOnEmpty;
    public bool ShouldRefreshContents;
    public int MinSecondsBetweenRefresh;
    public int MaxSecondsBetweenRefresh;
}

public class SimpleNPCLootProfile : BaseLootProfile
{
    public int MinimumItems;
    public int MaximumItems;
    public ItemAmountWeighted[] Items;
    public SimpleNPCLootProfile();
    public SimpleNPCLootProfile(SimpleNPCLootProfile lootContainerProfile);
    public override void PopulateLoot(ItemContainer container);
}

public class AdvancedNPCLootProfile : BaseLootProfile
{
    public LootSpawnSlot[] LootSpawnSlots;
    public int MaximumItems;
    public AdvancedNPCLootProfile();
    public AdvancedNPCLootProfile(LootContainer.LootSpawnSlot[] lootSpawnSlots);
    public AdvancedNPCLootProfile(AdvancedNPCLootProfile lootContainerProfile);
    public override void PopulateLoot(ItemContainer container);
}

public class SimpleLootContainerProfile : BaseLootContainerProfile
{
    public int MinimumItems;
    public int MaximumItems;
    public ItemAmountWeighted[] Items;
    public SimpleLootContainerProfile();
    public SimpleLootContainerProfile(SimpleLootContainerProfile lootContainerProfile);
    public override void PopulateLoot(ItemContainer container);
}

public class AdvancedLootContainerProfile : BaseLootContainerProfile
{
    public LootSpawnSlot[] LootSpawnSlots;
    public int MaximumItems;
    public AdvancedLootContainerProfile();
    public AdvancedLootContainerProfile(LootContainer container);
    public AdvancedLootContainerProfile(AdvancedLootContainerProfile lootContainerProfile);
    public override void PopulateLoot(ItemContainer container);
}

public class LootSpawnSlot
{
    public LootSpawn LootDefinition;
    public int NumberToSpawn;
    public float Probability;
    public LootSpawnSlot();
    public LootSpawnSlot(global::LootSpawn lootSpawn, int numberToSpawn, bool hasCondition);
    public LootSpawnSlot(LootContainer.LootSpawnSlot lootSpawnSlot, bool hasCondition);
}

public class LootSpawn
{
    public ItemAmountRanged[] Items;
    public Entry[] SubSpawn;
    public byte[] Node;
    public LootSpawn();
    public LootSpawn(global::LootSpawn lootSpawn, bool hasCondition);
    public void SpawnIntoContainer(ItemContainer container, BaseLootProfile lootProfile);
    private void SubCategoryIntoContainer(ItemContainer container, BaseLootProfile lootProfile);
    public class Entry
    {
        public LootSpawn Category;
        public int Weight;
        public byte[] Node;
    }

}

public class Entry
{
    public LootSpawn Category;
    public int Weight;
    public byte[] Node;
}

public class ItemAmountWeighted : ItemAmountRanged
{
    public int Weight;
}

public class ItemAmountRanged : ItemAmount
{
    public float MaxAmount;
    public ItemAmountRanged();
    public ItemAmountRanged(ItemDefinition item, float amount, float maxAmount, bool hasCondition);
    public override float GetAmount(float lootMultiplier);
}

public class ItemAmount
{
    public string Shortname;
    public float BlueprintChance;
    public float MinAmount;
    public ConditionItem Condition;
    [JsonIgnore]
    private int _itemId;
    [JsonIgnore]
    public int ItemID { get; set; }
    public ItemAmount();
    public ItemAmount(ItemDefinition item, float amount, bool hasCondition);
    public virtual float GetAmount(float lootMultiplier);
    public ulong RandomSkinID();
    public float GetConditionFraction();
    public bool WantsBlueprint();
    public class ConditionItem
    {
        public float MinCondition;
        public float MaxCondition;
    }

}

public class ConditionItem
{
    public float MinCondition;
    public float MaxCondition;
}

public class SkinEntry
{
    public int Weight;
    public ulong SkinID;
    public SkinEntry();
    public SkinEntry(ulong skinId, int weight);
}

private class StoredData
{
    public Hash<string, SimpleLootContainerProfile> loot_simple;
    public Hash<string, AdvancedLootContainerProfile> loot_advanced;
    public Hash<string, AdvancedNPCLootProfile> npcs_advanced;
    public Hash<string, SimpleNPCLootProfile> npcs_simple;
    public bool IsBaseLootTable;
    public string ProfileName;
    [JsonIgnore]
    public bool IsValid { get; set; }
    [JsonIgnore]
    public bool HasAnyProfiles { get; set; }
    public void CreateDefaultLootProfile(LootContainer container);
    public void CloneLootProfile(string shortname, BaseLootProfile lootContainerProfile);
    public void CreateDefaultLootProfile(string shortPrefabName, LootContainer.LootSpawnSlot[] lootSpawnSlots);
    public bool TryGetLootProfile(string shortname, BaseLootContainerProfile profile);
    public bool TryGetNPCProfile(string shortname, BaseLootProfile profile);
    [JsonIgnore]
    private List<BaseLootContainerProfile> randomList;
    public bool GetRandomLootProfile(BaseLootContainerProfile profile);
    public void RemoveProfile(string shortname);
}


```

---

## AlwaysDay

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

## AnimalRemover

```csharp
using System.Collections.Generic;
using System.Linq;
using UnityEngine;

Oxide.Plugins
[Info("AnimalRemover", "Ankawi", "1.0.2")]
[Description("Allows you to disable specific animals from spawning on the server")]
public class AnimalRemover : RustPlugin
{
    const string BearPrefab;
    const string BoarPrefab;
    const string ChickenPrefab;
    const string HorsePrefab;
    const string StagPrefab;
    const string WolfPrefab;
    new void LoadConfig();
    protected override void LoadDefaultConfig();
     void OnServerInitialized();
     Dictionary<string, IGrouping<string, BaseEntity>> GetAnimals();
     void KillAnimals();
     void OnEntitySpawned(BaseNetworkable entity);
     void SetConfig(object[] args);
}


```

---

## AntiAdminAbuse

```csharp
using System;

Oxide.Plugins
[Info("AntiAdminAbuse", "Norn", 0.2, ResourceId = 12693)]
[Description("Prevent moderator abuse.")]
public class AntiAdminAbuse : RustPlugin
{
     object OnRunCommand(ConsoleSystem.Arg arg);
    protected override void LoadDefaultConfig();
}


```

---

## AntiBandit

```csharp
using Oxide.Core.Plugins;
using Rust;
using System;
using System.Collections.Generic;
using UnityEngine;

Oxide.Plugins
[Info("AntiBandit", "Alphawar", "0.9.1", ResourceId = 1879)]
[Description("Plugin designed to assist servers with RDM (designed for RPG servers)")]
 class AntiBandit : RustPlugin
{
    [PluginReference]
     Plugin RustIOFriendListAPI;
    private List<Timer> tTimers;
    private Hash<ulong, double> PlayerCooldownList;
    private List<Vector3> ExpiredPvPZonesList;
    private List<Vector3> ExpiredRaidZonesList;
    private Hash<Vector3, PVPData> PvPList;
    private Hash<Vector3, RaidDetails> raidZoneList;
     void OnPlayerInit(BasePlayer _player);
     void OnEntityTakeDamage(BaseCombatEntity entity, HitInfo hitinfo);
     void OnEntityDeath(BaseCombatEntity entity, HitInfo info);
    [ChatCommand("antibandit")]
     void chatSettings(BasePlayer player, string cmd, string[] args);
    [ChatCommand("raid")]
     void raidChatHandle(BasePlayer _player);
    [ChatCommand("pvp")]
     void pvpChatHandle(BasePlayer _player, string cmd, string[] args);
    [ConsoleCommand("AntiBandit.Purge")]
     void purgeToggle(ConsoleSystem.Arg arg);
     bool checkPlayerCooldown(BasePlayer _player);
     bool checkServerPvPCooldown();
     bool checkServerRaidCooldown();
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
     void loadPermissions();
    private static int wallCol;
    private static int playerCol;
    private void Loaded();
    private void Unloaded();
     void DestroyTimers();
     bool IsAllowed(BasePlayer player, string perm);
     double GetTimeStamp();
    static void NullifyDamage(HitInfo hitinfo);
    public bool checkEntityInZone(BaseCombatEntity _entity, Vector3 _zoneKey, int _maxDistance);
    public bool checkPlayerInZone(BasePlayer _player, Vector3 _zoneKey, int _maxDistance);
    public bool checkPlayerRegistered(BasePlayer _player, Vector3 _zoneKey);
     bool checkBuildingOwner(BasePlayer _player, BaseCombatEntity _entity);
     bool checkNearPlayerNotFriend(BasePlayer _player, Vector3 _hashPos);
     void registerNearPlayers(Vector3 _hashPos);
     bool zoneCooldownCheck(string _zoneType, Vector3 _zonekey);
     bool zoneTimerCheck(string _zoneType, Vector3 _zonekey);
     ulong FindOwner(BaseEntity entity);
     void createPlayerCooldown(BasePlayer _player);
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
     Dictionary<string, string> messages;
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

## AntiChatFlood

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
[Info("AntiChatFlood", "DylanSMR", "1.0.1")]
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

## AntiCheat

```csharp
using Oxide.Core;
using Oxide.Core.Configuration;
using Oxide.Core.Plugins;
using ProtoBuf;
using Rust;
using System;
using System.Collections.Generic;
using System.Linq;
using UnityEngine;

Oxide.Plugins
[Info("AntiCheat", "OxideBro", "2.2.23")]
 class AntiCheat : RustPlugin
{
    static AntiCheat instance;
     class DataStorage
    {
        public Dictionary<ulong, ADMINDATA> AdminData;
        public DataStorage();
    }

     class ADMINDATA
    {
        public string Name;
        public bool Check;
    }

     DataStorage adata;
    private DynamicConfigFile AdminData;
    static DateTime epoch;
     void LoadData();
    static int b;
     int DetectCountMacros;
     int DetectPerMacros;
     int DetectCountFSH;
     bool SHEnable;
     bool FHEnable;
     bool SHEnabled;
     bool FHEnabled;
     bool MCREnabled;
     bool AIMEnabled;
     bool EnabledSilentAim;
     bool SHKickEnabled;
     bool FHKickEnabled;
     bool AntiRecoilEnabled;
     bool AIMLOCKEnabledBAN;
     bool AIMHACKEnabledBAN;
    static bool SendsLogs;
     float AimPercent;
     float AimPercentOverCount;
    static bool textureenable;
     bool init;
    private List<string> ListWeapons;
    protected override void LoadDefaultConfig();
    private void LoadConfigValues();
    private void GetConfig(string menu, string Key, T var);
    static int PlayerLayer;
    static int constructionColl;
     void Loaded();
     int raycastCount;
     void OnServerInitialized();
     void SaveAllDate();
    [ConsoleCommand("ban.user")]
    private void cmdBan(ConsoleSystem.Arg arg);
    [ConsoleCommand("banlist")]
    private void BanListedPlayers(ConsoleSystem.Arg arg);
    [ConsoleCommand("unban.user")]
    private void UnbanCommand(ConsoleSystem.Arg arg);
     object OnPlayerAttack(BasePlayer player, HitInfo info);
     object CanUserLogin(string name, string id, string ip);
    public bool IsConnected(BasePlayer player);
    public void Kick(BasePlayer player, string reason);
    public bool IsBanned(ulong id);
    public void Ban(ulong id, string reason);
    private readonly Dictionary<ulong, AimLockData> aimlock;
    public class AimLockData
    {
        public int Ticks;
        public string Body;
    }

    private bool IsNPC(BasePlayer player);
    private void OnEntityDeath(BaseCombatEntity entity, HitInfo info);
    private double GrabCurrentTime();
    private double LastAttack;
     object OnEntityTakeDamage(BaseCombatEntity entity, HitInfo info);
    [ChatCommand("ac")]
     void cmdChatDetect(BasePlayer player, string command, string[] args);
    [HookMethod("AimCheck")]
    private void AimCheck(ConsoleSystem.Arg arg);
    [HookMethod("AimCheckServer")]
    private void AimCheckServer(ConsoleSystem.Arg arg);
    [HookMethod("CheckServer")]
    private void CheckServer(ConsoleSystem.Arg arg);
     void Unload();
     void DestroyAll();
     void OnPlayerConnected(BasePlayer player);
     void RefreshPlayer(BasePlayer player);
    private void AutoBan(BasePlayer player, string reason);
    private void CheckFLY(BasePlayer player);
    public class PlayerHack : MonoBehaviour
    {
        public BasePlayer player;
        public Vector3 lastPosition;
        public Vector3 currentDirection;
        public bool isonGround;
        public float Distance3D;
        public float VerticalDistance;
        public float deltaTick;
        public float speedHackDetections;
        public double currentTick;
        public double lastTick;
        public double lastTickFly;
        public double lastTickSpeed;
         void Awake();
        static DateTime epoch;
        static double CurrentTime();
        static List<PlayerHack> fpsCalled;
        static double fpsTime;
        static bool fpsCheckCalled;
        static void CheckForHacks(PlayerHack hack);
        static int fpsIgnore;
         void CheckPlayer();
    }

    static float minSpeedPerSecond;
    static double LogTime();
    static Vector3 lastGroundPosition;
    static void CheckForSpeedHack(PlayerHack hack);
    static int bulletmask;
    static DamageTypeList emptyDamage;
    static Vector3 VectorDown;
    static Hash<BasePlayer, float> lastWallhack;
     Hash<ulong, ColliderCheckTest> playerWallcheck;
    static RaycastHit cachedRaycasthit;
    [HookMethod("WallhackKillCheck")]
    private void WallhackKillCheck(BasePlayer player, BasePlayer attacker, HitInfo hitInfo);
    private void CancelDamage(HitInfo hitinfo);
    private readonly Dictionary<ulong, NoRecoilData> data;
    private readonly Dictionary<ulong, Timer> detections;
    private readonly int detectionDiscardSeconds;
    private readonly int violationProbability;
    private readonly int maximumViolations;
    private readonly Dictionary<string, int> probabilityModifiers;
    private readonly List<string> blacklistedAttachments;
    public class NoRecoilData
    {
        public int Ticks;
        public int Count;
        public int Violations;
    }

     void OnWeaponFired(BaseProjectile projectile, BasePlayer player, ItemModProjectile mod, ProjectileShoot projectiles);
    private void ProcessRecoil(BaseProjectile projectile, BasePlayer player, ItemModProjectile mod, ProjectileShoot projectileShoot, NoRecoilData info, int probModifier, Vector3 eyesDirection);
    static Hash<BasePlayer, int> wallhackDetec;
    public class ColliderCheckTest : MonoBehaviour
    {
        public BasePlayer player;
         Hash<Collider, Vector3> entryPosition;
         SphereCollider col;
        public float teleportedBack;
        public Collider lastCollider;
         void Awake();
        public static BaseEntity GetCollEntity(Vector3 entry, Vector3 exist);
         void OnTriggerExit(Collider col);
         void OnDestroy();
    }

    static void ForcePlayerBack(ColliderCheckTest colcheck, Collider collision, Vector3 entryposition, Vector3 exitposition);
    static Vector3 GetRollBackPosition(Vector3 entryposition, Vector3 exitposition, float distance);
    new static void ForcePlayerPosition(BasePlayer player, Vector3 destination);
    [HookMethod("OnBasePlayerAttacked⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠")]
    private void OnBasePlayerAttacked(BasePlayer player, HitInfo hitInfo);
    public static void msgPlayer(BasePlayer player, string msg);
    public static void msgAll(string msg);
    private void CreateInfo(BasePlayer player);
    private void SaveDataAdmin();
    private void SavePlayerData();
     void SendDetection(BasePlayer player, string msg);
    public Dictionary<ulong, PlayerAntiCheat> PlayersListed;
    public class PlayerAntiCheat
    {
        public string Name { get; set; }
        public int Killed { get; set; }
        public int Deaths { get; set; }
        public int Hits { get; set; }
        public int Heads { get; set; }
        public bool Banned;
        public string Date;
        public string Reason;
        public string BanCreator;
    }

}

 class DataStorage
{
    public Dictionary<ulong, ADMINDATA> AdminData;
    public DataStorage();
}

 class ADMINDATA
{
    public string Name;
    public bool Check;
}

public class AimLockData
{
    public int Ticks;
    public string Body;
}

public class PlayerHack : MonoBehaviour
{
    public BasePlayer player;
    public Vector3 lastPosition;
    public Vector3 currentDirection;
    public bool isonGround;
    public float Distance3D;
    public float VerticalDistance;
    public float deltaTick;
    public float speedHackDetections;
    public double currentTick;
    public double lastTick;
    public double lastTickFly;
    public double lastTickSpeed;
     void Awake();
    static DateTime epoch;
    static double CurrentTime();
    static List<PlayerHack> fpsCalled;
    static double fpsTime;
    static bool fpsCheckCalled;
    static void CheckForHacks(PlayerHack hack);
    static int fpsIgnore;
     void CheckPlayer();
}

public class NoRecoilData
{
    public int Ticks;
    public int Count;
    public int Violations;
}

public class ColliderCheckTest : MonoBehaviour
{
    public BasePlayer player;
     Hash<Collider, Vector3> entryPosition;
     SphereCollider col;
    public float teleportedBack;
    public Collider lastCollider;
     void Awake();
    public static BaseEntity GetCollEntity(Vector3 entry, Vector3 exist);
     void OnTriggerExit(Collider col);
     void OnDestroy();
}

public class PlayerAntiCheat
{
    public string Name { get; set; }
    public int Killed { get; set; }
    public int Deaths { get; set; }
    public int Hits { get; set; }
    public int Heads { get; set; }
    public bool Banned;
    public string Date;
    public string Reason;
    public string BanCreator;
}


```

---

## AntiFoundationStack

```csharp
using System.Collections.Generic;
using UnityEngine;

Oxide.Plugins
[Info("AntiFoundationStack", "Jake_Rich", 0.1)]
[Description("Prevents foundation stacking")]
public class AntiFoundationStack : RustPlugin
{
     void Init();
     Dictionary<string, double> foundationWidth;
     int copyLayer;
     void OnEntityBuilt(Planner plan, GameObject go);
}


```

---

## AntiHack

```csharp
using Network;
using Network.Visibility;
using Oxide.Core;
using Oxide.Core.Libraries;
using Oxide.Core.Plugins;
using Rust;
using System;
using System.Collections.Generic;
using System.Globalization;
using UnityEngine;

Oxide.Plugins
[Info("AntiHack", "OxideBro", "1.0.42")]
public class AntiHack : RustPlugin
{
    [PluginReference]
    private Plugin Discord;
    private static Dictionary<ulong, HashSet<uint>> playersHidenEntities;
    private static int radius;
    private Dictionary<BasePlayer, CurrentLog> currentAdminsLog;
    private Dictionary<ulong, float> lastSpeedAttackAttackTime;
    private Dictionary<ulong, float> lastShootingThroughWallTime;
    public List<BasePlayer> Admins;
    private Dictionary<ulong, int> speedAttackDetections;
    private Dictionary<ulong, int> shootingThroughWallDetections;
    private static bool isSaving;
    private static float minPlayersWallHackDistanceCheck;
    private static float minObjectsWallHackDistanceCheck;
    private static float maxPlayersWallHackDistanceCheck;
    private static float maxObjectsWallHackDistanceCheck;
    private static float tickRate;
    private static int globalMask;
    private static int cM;
    private static int playerWallHackMask;
    private static int entityMask;
    private static int constructionAndDeployed;
    private static Dictionary<ulong, int> playersKicks;
    private static Dictionary<ulong, HackHandler> playersHandlers;
    private static Dictionary<int, Dictionary<int, Chunk>> chunks;
    private static HashSet<Chunk> chunksList;
    private List<string> neededEntities;
    public bool IsConnected(BasePlayer player);
    public void Kick(BasePlayer player, string reason);
    public bool IsBanned(ulong id);
    private static AntiHack instance;
    private bool isLoaded;
    private static bool wallHackPlayersEnabled;
    private static bool wallHackObjectsEnabled;
    private static bool enableFlyHackLog;
    private static bool enableFlyHackCar;
    private static bool enableSpeedHackLog;
    private static bool enableTextureHackLog;
    private static bool enableSpeedAttackLog;
    private static bool enableWallHackAttackLog;
    private static bool needKick;
    private static bool needKickEndKill;
    private static bool needBan;
    private bool configChanged;
    private const int intervalBetweenTextureHackMessages;
    private const int maxFalseFlyDetects;
    private const int maxFalseSpeedDetects;
    private static int maxFlyWarnings;
    private static int maxSpeedWarnings;
    private static bool SendDiscordMessages;
    private static StoredData db;
    private object GetConfig(string menu, string datavalue, object defaultValue);
    protected override void LoadDefaultConfig();
    private void DiscordMessages(string Messages, int type, string Reason);
    private void LoadVariables();
    public static void SaveData();
    public void ShowDetects(BasePlayer player, string[] args);
     void OnDispenserGather(ResourceDispenser dispenser, BaseEntity entity, Item item);
    public static void LogHandler(BasePlayer player, Vector3 lastGroundPos, TemporaryCoordinates temp, bool isSpeedHack);
     void ShowLog(BasePlayer player, string command, string[] args);
    private static HashSet<BaseEntity> GetEntitiesFromAllChunks();
    private static HashSet<BaseEntity> GetEntitiesFromChunksNearPointOptimized(Vector3 point);
    private static Chunk GetChunkFromPoint(Vector3 point);
    private void SetPlayer(BasePlayer player);
    public static bool CanGetReport(BasePlayer player);
    private static void SendReportToOnlineModerators(string report);
    public static BasePlayer FindPlayer(string nameOrIdOrIp);
    private void ShootingThroughWallHanlder(BasePlayer attacker, HitInfo info, float timeNow);
    private void SpeedAttackHandler(BasePlayer attacker, HitInfo info, BaseMelee melee, float timeNow);
     object CanNetworkTo(BaseNetworkable entity, BasePlayer target);
     void OnEntitySpawned(BaseEntity ent, GameObject gameObject);
     void OnEntityKill(BaseNetworkable ent);
    private void Init();
     void OnServerInitialized();
     void OnPlayerDisconnected(BasePlayer player);
     void OnServerSave();
     void OnPlayerConnected(BasePlayer player);
     void OnServerShutdown();
     void Unload();
     void DestroyAll();
     void OnPlayerAttack(BasePlayer attacker, HitInfo info);
    private bool IsPlayerGotImmunity(ulong playerid);
    private class Chunk
    {
        public HashSet<BaseEntity> entities;
        public int x { get; set; }
        public int z { get; set; }
    }

    private class Log
    {
        public List<Detect> detects;
        public int detectsAmount;
        public ulong steamID;
        public string name;
    }

    public class Detect
    {
        public List<Coordinates> coordinates;
    }

    public class TemporaryCoordinates
    {
        public List<Coordinates> coordinates;
    }

    private class StoredData
    {
        public List<Log> logs;
    }

    private class CurrentLog
    {
        public ulong steamID;
        public int detect;
    }

    private class HackHandler : MonoBehaviour
    {
        private int flyWarnings;
        private int textureWarnings;
        private int speedWarnings;
        private float ownTickRate;
        private bool IsFlying;
        private int falseFlyDetects;
        private int falseSpeedDetects;
        private int ping;
        private TemporaryCoordinates temp;
        public HashSet<BaseEntity> hidedEntities;
        public Dictionary<ulong, HashSet<BaseEntity>> hidedPlayersEntities;
        public HashSet<BaseEntity> seenObjects;
        public Vector3 lastPosition;
        private bool IsHided;
        private bool isShowedAll;
        public BasePlayer player;
        private float lastTick;
        private float deltaTime;
        private float flyTime;
        private float flyTimeStart;
        public Vector3 lastGroundPosition;
        public Vector3 playerPreviousPosition;
        private Network.Connection connection;
        static Vector3 ENTlastGroundPosition;
        private void Awake();
        private void Update();
        private void SpeedHackHandler();
        public List<Blacklist> list;
        public class Blacklist
        {
            public Blacklist(string Player, string SteamId);
            public string Player { get; set; }
            public string SteamId { get; set; }
        }

        private void FlyHackHandler();
        private void TextureHackHandler();
        private bool IsInsideFoundation(BaseEntity block);
        private bool IsInsideCave(Collider collider);
        private void CheckSnapshot();
        public void HideAll();
        public void Hide(BaseEntity entity);
        private void Show(BaseEntity entity, bool needRemove);
        private void ShowLines(Vector3 start, Vector3 target, bool isVisible);
        private bool TryLineCast(Vector3 start, Vector3 target, float plusTarget, float plusPlayer);
        private bool IsObjectVisible(Vector3 start, Vector3 target);
        private void WallHackHandler();
        private void PlayerWallHackHandlerSleepers();
        private void PlayerWallHackHandler();
        private bool DoLine(Vector3 start, Vector3 target, float plusTarget, float plusPlayer);
        private bool IsBehindStairs(Vector3 start, Vector3 target);
        private bool IsVisible(BaseEntity target, bool isSleeper);
        private void ShowPlayer(BasePlayer target, bool needRemove, bool isSleeper);
        private void HidePlayer(BasePlayer target, bool isSleeper);
        private void HidePlayersHostile(BasePlayer target);
        private void ShowAllEntities();
        private void CorrectValues();
        private void ReturnBack(Vector3 pos);
        private void CreateLogFlyHack(Vector3 playerPosition);
        private void CreateLogSpeedHack(Vector3 playerPosition, float maxSpeed);
        private void CreateLogTextureHack(Vector3 playerPosition, string objectName);
        private void AddTemp();
        private void Reset();
        private void CrimeHandler(string reason);
        public void Disconnect();
        public void ShowAllPlayers();
        public void Destroy();
    }

}

private class Chunk
{
    public HashSet<BaseEntity> entities;
    public int x { get; set; }
    public int z { get; set; }
}

private class Log
{
    public List<Detect> detects;
    public int detectsAmount;
    public ulong steamID;
    public string name;
}

public class Detect
{
    public List<Coordinates> coordinates;
}

public class TemporaryCoordinates
{
    public List<Coordinates> coordinates;
}

private class StoredData
{
    public List<Log> logs;
}

private class CurrentLog
{
    public ulong steamID;
    public int detect;
}

private class HackHandler : MonoBehaviour
{
    private int flyWarnings;
    private int textureWarnings;
    private int speedWarnings;
    private float ownTickRate;
    private bool IsFlying;
    private int falseFlyDetects;
    private int falseSpeedDetects;
    private int ping;
    private TemporaryCoordinates temp;
    public HashSet<BaseEntity> hidedEntities;
    public Dictionary<ulong, HashSet<BaseEntity>> hidedPlayersEntities;
    public HashSet<BaseEntity> seenObjects;
    public Vector3 lastPosition;
    private bool IsHided;
    private bool isShowedAll;
    public BasePlayer player;
    private float lastTick;
    private float deltaTime;
    private float flyTime;
    private float flyTimeStart;
    public Vector3 lastGroundPosition;
    public Vector3 playerPreviousPosition;
    private Network.Connection connection;
    static Vector3 ENTlastGroundPosition;
    private void Awake();
    private void Update();
    private void SpeedHackHandler();
    public List<Blacklist> list;
    public class Blacklist
    {
        public Blacklist(string Player, string SteamId);
        public string Player { get; set; }
        public string SteamId { get; set; }
    }

    private void FlyHackHandler();
    private void TextureHackHandler();
    private bool IsInsideFoundation(BaseEntity block);
    private bool IsInsideCave(Collider collider);
    private void CheckSnapshot();
    public void HideAll();
    public void Hide(BaseEntity entity);
    private void Show(BaseEntity entity, bool needRemove);
    private void ShowLines(Vector3 start, Vector3 target, bool isVisible);
    private bool TryLineCast(Vector3 start, Vector3 target, float plusTarget, float plusPlayer);
    private bool IsObjectVisible(Vector3 start, Vector3 target);
    private void WallHackHandler();
    private void PlayerWallHackHandlerSleepers();
    private void PlayerWallHackHandler();
    private bool DoLine(Vector3 start, Vector3 target, float plusTarget, float plusPlayer);
    private bool IsBehindStairs(Vector3 start, Vector3 target);
    private bool IsVisible(BaseEntity target, bool isSleeper);
    private void ShowPlayer(BasePlayer target, bool needRemove, bool isSleeper);
    private void HidePlayer(BasePlayer target, bool isSleeper);
    private void HidePlayersHostile(BasePlayer target);
    private void ShowAllEntities();
    private void CorrectValues();
    private void ReturnBack(Vector3 pos);
    private void CreateLogFlyHack(Vector3 playerPosition);
    private void CreateLogSpeedHack(Vector3 playerPosition, float maxSpeed);
    private void CreateLogTextureHack(Vector3 playerPosition, string objectName);
    private void AddTemp();
    private void Reset();
    private void CrimeHandler(string reason);
    public void Disconnect();
    public void ShowAllPlayers();
    public void Destroy();
}

public class Blacklist
{
    public Blacklist(string Player, string SteamId);
    public string Player { get; set; }
    public string SteamId { get; set; }
}


```

---

## AntiHack (1)

```csharp
using Network;
using Network.Visibility;
using Oxide.Core;
using Oxide.Core.Libraries;
using Oxide.Core.Plugins;
using Rust;
using System;
using System.Collections.Generic;
using System.Globalization;
using UnityEngine;

Oxide.Plugins
[Info("AntiHack", "OxideBro", "1.0.0")]
public class AntiHack : RustPlugin
{
    private static Dictionary<ulong, HashSet<uint>> playersHidenEntities;
    private static int radius;
    private Dictionary<BasePlayer, AntiHack.CurrentLog> currentAdminsLog;
    private Dictionary<ulong, float> lastSpeedAttackAttackTime;
    private Dictionary<ulong, float> lastShootingThroughWallTime;
    private Dictionary<ulong, int> speedAttackDetections;
    private Dictionary<ulong, int> shootingThroughWallDetections;
    private static bool isSaving;
    private static bool debug;
    private static float minPlayersWallHackDistanceCheck;
    private static float minObjectsWallHackDistanceCheck;
    private static float maxPlayersWallHackDistanceCheck;
    private static float maxObjectsWallHackDistanceCheck;
    private static float tickRate;
    private static int globalMask;
    private static int cM;
    private static int playerWallHackMask;
    private static int entityMask;
    private static int constructionAndDeployed;
    private static Dictionary<ulong, int> playersKicks;
    private static Dictionary<ulong, AntiHack.HackHandler> playersHandlers;
    private static Dictionary<int, Dictionary<int, AntiHack.Chunk>> chunks;
    private static HashSet<AntiHack.Chunk> chunksList;
    private List<string> neededEntities;
    public bool IsConnected(BasePlayer player);
    public void Kick(BasePlayer player, string reason);
    public bool IsBanned(ulong id);
    private static AntiHack instance;
    private bool isLoaded;
    private static bool wallHackPlayersEnabled;
    private static bool wallHackObjectsEnabled;
    private static bool enableFlyHackLog;
    private static bool enableFlyHackCar;
    private static bool enableSpeedHackLog;
    private static bool enableTextureHackLog;
    private static bool enableSpeedAttackLog;
    private static bool enableWallHackAttackLog;
    private static bool needKick;
    private static bool needKickEndKill;
    private static bool needBan;
    private bool configChanged;
    private const int intervalBetweenTextureHackMessages;
    private const int maxFalseFlyDetects;
    private const int maxFalseSpeedDetects;
    private static int maxFlyWarnings;
    private static int maxSpeedWarnings;
    private static AntiHack.StoredData db;
    private object GetConfig(string menu, string datavalue, object defaultValue);
    protected override void LoadDefaultConfig();
    private void LoadVariables();
    public static void SaveData();
    public void ShowDetects(BasePlayer player, string[] args);
     void OnDispenserGather(ResourceDispenser dispenser, BaseEntity entity, Item item);
    public static void LogHandler(BasePlayer player, Vector3 lastGroundPos, AntiHack.TemporaryCoordinates temp, bool isSpeedHack);
    [HookMethod("ShowLog")]
    private void ShowLog(BasePlayer player, string command, string[] args);
    private static HashSet<BaseEntity> GetEntitiesFromAllChunks();
    private static HashSet<BaseEntity> GetEntitiesFromChunksNearPointOptimized(Vector3 point);
    private static AntiHack.Chunk GetChunkFromPoint(Vector3 point);
    private void SetPlayer(BasePlayer player);
    public static bool CanGetReport(BasePlayer player);
    private static void SendReportToOnlineModerators(string report);
    public static BasePlayer FindPlayer(string nameOrIdOrIp);
    private void ShootingThroughWallHanlder(BasePlayer attacker, HitInfo info, float timeNow);
    private void SpeedAttackHandler(BasePlayer attacker, HitInfo info, BaseMelee melee, float timeNow);
    [HookMethod("CanNetworkTo")]
    private object CanNetworkTo(BaseNetworkable entity, BasePlayer target);
    [HookMethod("OnEntitySpawned")]
    private void OnEntitySpawned(BaseEntity ent, GameObject gameObject);
    [HookMethod("OnEntityKill")]
    private void OnEntityKill(BaseNetworkable ent);
    [HookMethod("Init")]
    private void Init();
    [HookMethod("OnServerInitialized")]
    private void OnServerInitialized();
    [HookMethod("OnPlayerDisconnected")]
    private void OnPlayerDisconnected(BasePlayer player);
    [HookMethod("OnServerSave")]
    private void OnServerSave();
    [HookMethod("OnPlayerInit")]
    private void OnPlayerInit(BasePlayer player);
    [HookMethod("OnServerShutdown")]
    private void OnServerShutdown();
     void Unload();
     void DestroyAll();
    [HookMethod("OnPlayerAttack")]
    private void OnPlayerAttack(BasePlayer attacker, HitInfo info);
    private bool IsPlayerGotImmunity(ulong id);
    private class Chunk
    {
        public HashSet<BaseEntity> entities;
        public int x { get; set; }
        public int z { get; set; }
    }

    private class Log
    {
        public List<AntiHack.Detect> detects;
        public int detectsAmount;
        public ulong steamID;
        public string name;
    }

    public class Detect
    {
        public List<AntiHack.Coordinates> coordinates;
    }

    public class TemporaryCoordinates
    {
        public List<AntiHack.Coordinates> coordinates;
    }

    private class StoredData
    {
        public List<AntiHack.Log> logs;
    }

    private class CurrentLog
    {
        public ulong steamID;
        public int detect;
    }

    private class HackHandler : MonoBehaviour
    {
        private int flyWarnings;
        private int textureWarnings;
        private int speedWarnings;
        private float ownTickRate;
        private bool IsFlying;
        private int falseFlyDetects;
        private int falseSpeedDetects;
        private int ping;
        private AntiHack.TemporaryCoordinates temp;
        public HashSet<BaseEntity> hidedEntities;
        public Dictionary<ulong, HashSet<BaseEntity>> hidedPlayersEntities;
        public HashSet<BaseEntity> seenObjects;
        public Vector3 lastPosition;
        private bool IsHided;
        private bool isShowedAll;
        public BasePlayer player;
        private float lastTick;
        private float deltaTime;
        private float flyTime;
        private float flyTimeStart;
        public Vector3 lastGroundPosition;
        public Vector3 playerPreviousPosition;
        private Network.Connection connection;
        private void Awake();
        private void Update();
        private void SpeedHackHandler();
        public List<Blacklist> list;
        public class Blacklist
        {
            public Blacklist(string Player, string SteamId);
            public string Player { get; set; }
            public string SteamId { get; set; }
        }

        private void FlyHackHandler();
        private void TextureHackHandler();
        private bool IsInsideFoundation(BaseEntity block);
        private bool IsInsideCave(Collider collider);
        private void CheckSnapshot();
        public void HideAll();
        public void Hide(BaseEntity entity);
        private void Show(BaseEntity entity, bool needRemove);
        private void ShowLines(Vector3 start, Vector3 target, bool isVisible);
        private bool TryLineCast(Vector3 start, Vector3 target, float plusTarget, float plusPlayer);
        private bool IsObjectVisible(Vector3 start, Vector3 target);
        private void WallHackHandler();
        private void PlayerWallHackHandlerSleepers();
        private void PlayerWallHackHandler();
        private bool DoLine(Vector3 start, Vector3 target, float plusTarget, float plusPlayer);
        private bool IsBehindStairs(Vector3 start, Vector3 target);
        private bool IsVisible(BaseEntity target, bool isSleeper);
        private void ShowPlayer(BasePlayer target, bool needRemove, bool isSleeper);
        private void HidePlayer(BasePlayer target, bool isSleeper);
        private void HidePlayersHostile(BasePlayer target);
        private void ShowAllEntities();
        private void CorrectValues();
        private void ReturnBack(Vector3 pos);
        private void CreateLogFlyHack(Vector3 playerPosition);
        private void CreateLogSpeedHack(Vector3 playerPosition, float maxSpeed);
        private void CreateLogTextureHack(Vector3 playerPosition, string objectName);
        private void AddTemp();
        private void Reset();
        private void CrimeHandler(string reason);
        public void Disconnect();
        public void ShowAllPlayers();
        public void Destroy();
    }

}

private class Chunk
{
    public HashSet<BaseEntity> entities;
    public int x { get; set; }
    public int z { get; set; }
}

private class Log
{
    public List<AntiHack.Detect> detects;
    public int detectsAmount;
    public ulong steamID;
    public string name;
}

public class Detect
{
    public List<AntiHack.Coordinates> coordinates;
}

public class TemporaryCoordinates
{
    public List<AntiHack.Coordinates> coordinates;
}

private class StoredData
{
    public List<AntiHack.Log> logs;
}

private class CurrentLog
{
    public ulong steamID;
    public int detect;
}

private class HackHandler : MonoBehaviour
{
    private int flyWarnings;
    private int textureWarnings;
    private int speedWarnings;
    private float ownTickRate;
    private bool IsFlying;
    private int falseFlyDetects;
    private int falseSpeedDetects;
    private int ping;
    private AntiHack.TemporaryCoordinates temp;
    public HashSet<BaseEntity> hidedEntities;
    public Dictionary<ulong, HashSet<BaseEntity>> hidedPlayersEntities;
    public HashSet<BaseEntity> seenObjects;
    public Vector3 lastPosition;
    private bool IsHided;
    private bool isShowedAll;
    public BasePlayer player;
    private float lastTick;
    private float deltaTime;
    private float flyTime;
    private float flyTimeStart;
    public Vector3 lastGroundPosition;
    public Vector3 playerPreviousPosition;
    private Network.Connection connection;
    private void Awake();
    private void Update();
    private void SpeedHackHandler();
    public List<Blacklist> list;
    public class Blacklist
    {
        public Blacklist(string Player, string SteamId);
        public string Player { get; set; }
        public string SteamId { get; set; }
    }

    private void FlyHackHandler();
    private void TextureHackHandler();
    private bool IsInsideFoundation(BaseEntity block);
    private bool IsInsideCave(Collider collider);
    private void CheckSnapshot();
    public void HideAll();
    public void Hide(BaseEntity entity);
    private void Show(BaseEntity entity, bool needRemove);
    private void ShowLines(Vector3 start, Vector3 target, bool isVisible);
    private bool TryLineCast(Vector3 start, Vector3 target, float plusTarget, float plusPlayer);
    private bool IsObjectVisible(Vector3 start, Vector3 target);
    private void WallHackHandler();
    private void PlayerWallHackHandlerSleepers();
    private void PlayerWallHackHandler();
    private bool DoLine(Vector3 start, Vector3 target, float plusTarget, float plusPlayer);
    private bool IsBehindStairs(Vector3 start, Vector3 target);
    private bool IsVisible(BaseEntity target, bool isSleeper);
    private void ShowPlayer(BasePlayer target, bool needRemove, bool isSleeper);
    private void HidePlayer(BasePlayer target, bool isSleeper);
    private void HidePlayersHostile(BasePlayer target);
    private void ShowAllEntities();
    private void CorrectValues();
    private void ReturnBack(Vector3 pos);
    private void CreateLogFlyHack(Vector3 playerPosition);
    private void CreateLogSpeedHack(Vector3 playerPosition, float maxSpeed);
    private void CreateLogTextureHack(Vector3 playerPosition, string objectName);
    private void AddTemp();
    private void Reset();
    private void CrimeHandler(string reason);
    public void Disconnect();
    public void ShowAllPlayers();
    public void Destroy();
}

public class Blacklist
{
    public Blacklist(string Player, string SteamId);
    public string Player { get; set; }
    public string SteamId { get; set; }
}


```

---

## AntiLootDespawn

```csharp
using UnityEngine;
using System;
using System.Linq;

Oxide.Plugins
[Info("AntiLootDespawn", "Bamabo", "1.1.0")]
[Description("Change loot despawn time in cupboard radius")]
public class AntiLootDespawn : RustPlugin
{
    public float despawnMultiplier;
    public bool enabled;
     void Init();
     void Unloaded();
     void OnEntitySpawned(BaseEntity entity);
     void SetDespawnTime(DroppedItem item);
    [ConsoleCommand("antilootdespawn.multiplier")]
     void cmdMultiplier(ConsoleSystem.Arg args);
    [ConsoleCommand("antilootdespawn.enabled")]
     void cmdEnabled(ConsoleSystem.Arg args);
    [ConsoleCommand("antilootdespawn")]
     void cmdList(ConsoleSystem.Arg args);
    protected override void LoadDefaultConfig();
     T GetConfigEntry(string configEntry, T defaultValue);
}


```

---

## AntiRaidTower

```csharp
using System;
using System.Collections.Generic;
using Oxide.Core.Plugins;
using Rust;
using UnityEngine;

Oxide.Plugins
[Info("AntiRaidTower", "Calytic @ RustServers.IO", "0.2.2", ResourceId = 1211)]
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
    private void OnEntityTakeDamage(BaseCombatEntity entity, HitInfo hitinfo);
     object CanTeleport(BasePlayer player);
     object canTeleport(BasePlayer player);
     void OnEntityBuilt(Planner planner, GameObject gameObject);
    private T GetConfig(string name, T defaultValue);
     string GetMsg(string key, string userID);
}


```

---

## AntiRB

```csharp
using System.Collections.Generic;
using Facepunch;
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Plugins;
using UnityEngine;
using System;

Oxide.Plugins
[Info("AntiRB", "King", "1.0.0")]
public class AntiRB : RustPlugin
{
    [PluginReference]
    private Plugin NoEscape;
    private Plugin Clans;
    private Plugin MenuAlerts;
    private Plugin ImageLibrary;
    private static AntiRB plugin;
    public Dictionary<string, double> _cooldown;
    private class Data
    {
        public readonly BasePlayer _player;
        public readonly Single _cooldown;
        public readonly Boolean _coin;
        public Data(BasePlayer player, Single cooldown, Boolean isRaidCoin);
    }

    private List<Data> _data;
    private void OnServerInitialized();
    private void Unload();
    private void TimeHandle();
     object OnItemAction(Item item, String action, BasePlayer player);
    private void OnEntityTakeDamage(BaseCombatEntity entity, HitInfo info);
    private void OnPlayerConnected(BasePlayer player);
    private void OnCrateHack(HackableLockedCrate crate);
    private void OnEntityKill(HackableLockedCrate crate);
    private AntiRBComponent AntiRBComp;
    private class AntiRBComponent : FacepunchBehaviour
    {
        private Int32 TotalTime;
        public Int32 CurrentTime;
        public Boolean IsStartHacked;
        public Boolean IsStartEvent;
        public HackableLockedCrate CrateEntity;
        private void Awake();
        public void DestroyComp();
        private void OnDestroy();
        private void UpdateTime();
        public void StartEvent();
        public void EndedEvent();
        private void SpawnChinook();
        private void RemoveChinook();
    }

    private ChinookPointComponent ChinookPointComp;
    private class ChinookPointComponent : FacepunchBehaviour
    {
        private Int32 TotalTime;
        public Int32 CurrentTime;
        public Boolean IsStartHacked;
        public Boolean IsStartEvent;
        public HackableLockedCrate CrateEntity;
        private void Awake();
        public void DestroyComp();
        private void OnDestroy();
        private void UpdateTime();
        public void StartEvent();
        public void EndedEvent();
        private void SpawnChinook();
        private void RemoveChinook();
    }

    static readonly DateTime epoch;
    static Double CurrentTime();
    public String ClanTag(BasePlayer player);
    private void UnblockedPlayer(BasePlayer player);
    private void FillingChinookItem(LootContainer LootContainer);
    [ChatCommand("chinook")]
    private void NewChinookPosition(BasePlayer player, string command, string[] args);
    public string GetGrid(Vector3 pos);
    private static class TimeExtensions
    {
        public static string FormatShortTime(TimeSpan time);
        private static string Format(Int32 units, string form1, string form2, string form3);
    }

    private PluginConfig config;
    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    private void UpdateConfigValues();
    protected override void SaveConfig();
    public class SettingsAntiRBChinook
    {
        [JsonProperty("Использовать этот вариант чинука ?")]
        public Boolean useChinook;
        [JsonProperty("Время открытия чинука (в секундах)")]
        public Int32 timeOpen;
        [JsonProperty("Раз во сколько будет запускаться ивент (в секундах)")]
        public Int32 eventCooldown;
        [JsonProperty("Через сколько заканчивать ивент если чинук никто не залутал (в секундах)")]
        public Int32 eventDestoroyTime;
        [JsonProperty("Местоположение чинука ( Не указывать )")]
        public Vector3 chinookPosition;
    }

    public class SettingsChinookPoint
    {
        [JsonProperty("Использовать этот вариант чинука ?")]
        public Boolean useChinook;
        [JsonProperty("Время открытия чинука (в секундах)")]
        public Int32 timeOpen;
        [JsonProperty("Раз во сколько будет запускаться ивент (в секундах)")]
        public Int32 eventCooldown;
        [JsonProperty("Через сколько заканчивать ивент если чинук никто не залутал (в секундах)")]
        public Int32 eventDestoroyTime;
        [JsonProperty("Местоположение чинука ( Не указывать )")]
        public Vector3 chinookPosition;
        [JsonProperty("ShortName")]
        public String ShortName;
        [JsonProperty("SkinID")]
        public ulong SkinID;
        [JsonProperty("Сколько очков давать за активацию тикета")]
        public Int32 howPoint;
    }

    public class SettingsAntiRB
    {
        [JsonProperty("ShortName")]
        public String ShortName;
        [JsonProperty("SkinID")]
        public ulong SkinID;
        [JsonProperty("Время действия монеты")]
        public Single timeActiveAntiRB;
    }

    public class SettingsDamageCoin
    {
        [JsonProperty("ShortName")]
        public String ShortName;
        [JsonProperty("SkinID")]
        public ulong SkinID;
        [JsonProperty("Время действия монеты")]
        public Single timeActiveDamageCoin;
        [JsonProperty("На сколько умножать урон по постройкам")]
        public Single DamageCoinPerc;
    }

    private class PluginConfig
    {
        [JsonProperty("Настройки анти-рб монеты")]
        public SettingsAntiRB _SettingsAntiRB;
        [JsonProperty("Настройки чинука с очками клана")]
        public SettingsChinookPoint _SettingsChinookPoint;
        [JsonProperty("Настройки чинука с анти рб и монетки на урон")]
        public SettingsAntiRBChinook _SettingsAntiRBChinook;
        [JsonProperty("Настройки монеты на увеления урона по постройкам")]
        public SettingsDamageCoin _SettingsDamageCoin;
        [JsonProperty("Перезарядка использования анти-рб монеты")]
        public Int32 _cooldown;
        [JsonProperty("Config version")]
        public VersionNumber PluginVersion;
        public static PluginConfig DefaultConfig();
    }

}

private class Data
{
    public readonly BasePlayer _player;
    public readonly Single _cooldown;
    public readonly Boolean _coin;
    public Data(BasePlayer player, Single cooldown, Boolean isRaidCoin);
}

private class AntiRBComponent : FacepunchBehaviour
{
    private Int32 TotalTime;
    public Int32 CurrentTime;
    public Boolean IsStartHacked;
    public Boolean IsStartEvent;
    public HackableLockedCrate CrateEntity;
    private void Awake();
    public void DestroyComp();
    private void OnDestroy();
    private void UpdateTime();
    public void StartEvent();
    public void EndedEvent();
    private void SpawnChinook();
    private void RemoveChinook();
}

private class ChinookPointComponent : FacepunchBehaviour
{
    private Int32 TotalTime;
    public Int32 CurrentTime;
    public Boolean IsStartHacked;
    public Boolean IsStartEvent;
    public HackableLockedCrate CrateEntity;
    private void Awake();
    public void DestroyComp();
    private void OnDestroy();
    private void UpdateTime();
    public void StartEvent();
    public void EndedEvent();
    private void SpawnChinook();
    private void RemoveChinook();
}

private static class TimeExtensions
{
    public static string FormatShortTime(TimeSpan time);
    private static string Format(Int32 units, string form1, string form2, string form3);
}

public class SettingsAntiRBChinook
{
    [JsonProperty("Использовать этот вариант чинука ?")]
    public Boolean useChinook;
    [JsonProperty("Время открытия чинука (в секундах)")]
    public Int32 timeOpen;
    [JsonProperty("Раз во сколько будет запускаться ивент (в секундах)")]
    public Int32 eventCooldown;
    [JsonProperty("Через сколько заканчивать ивент если чинук никто не залутал (в секундах)")]
    public Int32 eventDestoroyTime;
    [JsonProperty("Местоположение чинука ( Не указывать )")]
    public Vector3 chinookPosition;
}

public class SettingsChinookPoint
{
    [JsonProperty("Использовать этот вариант чинука ?")]
    public Boolean useChinook;
    [JsonProperty("Время открытия чинука (в секундах)")]
    public Int32 timeOpen;
    [JsonProperty("Раз во сколько будет запускаться ивент (в секундах)")]
    public Int32 eventCooldown;
    [JsonProperty("Через сколько заканчивать ивент если чинук никто не залутал (в секундах)")]
    public Int32 eventDestoroyTime;
    [JsonProperty("Местоположение чинука ( Не указывать )")]
    public Vector3 chinookPosition;
    [JsonProperty("ShortName")]
    public String ShortName;
    [JsonProperty("SkinID")]
    public ulong SkinID;
    [JsonProperty("Сколько очков давать за активацию тикета")]
    public Int32 howPoint;
}

public class SettingsAntiRB
{
    [JsonProperty("ShortName")]
    public String ShortName;
    [JsonProperty("SkinID")]
    public ulong SkinID;
    [JsonProperty("Время действия монеты")]
    public Single timeActiveAntiRB;
}

public class SettingsDamageCoin
{
    [JsonProperty("ShortName")]
    public String ShortName;
    [JsonProperty("SkinID")]
    public ulong SkinID;
    [JsonProperty("Время действия монеты")]
    public Single timeActiveDamageCoin;
    [JsonProperty("На сколько умножать урон по постройкам")]
    public Single DamageCoinPerc;
}

private class PluginConfig
{
    [JsonProperty("Настройки анти-рб монеты")]
    public SettingsAntiRB _SettingsAntiRB;
    [JsonProperty("Настройки чинука с очками клана")]
    public SettingsChinookPoint _SettingsChinookPoint;
    [JsonProperty("Настройки чинука с анти рб и монетки на урон")]
    public SettingsAntiRBChinook _SettingsAntiRBChinook;
    [JsonProperty("Настройки монеты на увеления урона по постройкам")]
    public SettingsDamageCoin _SettingsDamageCoin;
    [JsonProperty("Перезарядка использования анти-рб монеты")]
    public Int32 _cooldown;
    [JsonProperty("Config version")]
    public VersionNumber PluginVersion;
    public static PluginConfig DefaultConfig();
}


```

---

## AntiWounded

```csharp

Oxide.Plugins
[Info("Anti-Wounded", "SkinN", "2.0.0", ResourceId = 1045)]
 class AntiWounded : RustPlugin
{
    private bool CanBeWounded(BasePlayer player, HitInfo info);
}


```

---

## AProtection

```csharp
using System;
using System.Collections.Generic;
using System.Globalization;
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Game.Rust.Cui;
using UnityEngine;

Oxide.Plugins
[Info("AProtection", "Seckret", "1.5.0")]
public class AProtection : RustPlugin
{
    private class IAuthorize
    {
        [JsonIgnore]
        public bool IsAuthed;
        [JsonIgnore]
        public Vector3 LastPosition;
        [JsonIgnore]
        public Timer Timer;
        [JsonProperty("Пароль пользователя")]
        public string Password;
        [JsonProperty("Авторизованные IP адреса")]
        public List<string> AuthedIP;
        public void Authed(BasePlayer player, string ip);
    }

    [JsonProperty("Информация об игроках")]
    private Dictionary<ulong, IAuthorize> StoredData;
    [JsonProperty("Слой с интерфейсом")]
    private static string Layer;
    private void OnServerInitialized();
    private void Unload();
    private object OnServerCommand(ConsoleSystem.Arg arg);
    private void OnPlayerInit(BasePlayer player);
    [ConsoleCommand("auth")]
    private void cmdAuthConsole(ConsoleSystem.Arg args);
    private void ForceAuthorization(BasePlayer player);
    private string GetIP(string input);
    private string FL(string key);
    private static string HexToRustFormat(string hex);
    private void DrawInterface(BasePlayer player);
}

private class IAuthorize
{
    [JsonIgnore]
    public bool IsAuthed;
    [JsonIgnore]
    public Vector3 LastPosition;
    [JsonIgnore]
    public Timer Timer;
    [JsonProperty("Пароль пользователя")]
    public string Password;
    [JsonProperty("Авторизованные IP адреса")]
    public List<string> AuthedIP;
    public void Authed(BasePlayer player, string ip);
}


```

---

## ArenaDeathmatch

```csharp
using System.Collections.Generic;
using System;
using UnityEngine;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using Rust;
using System.Linq;

Oxide.Plugins
[Info("Arena Deathmatch", "Reneb", "1.1.6", ResourceId = 741)]
 class ArenaDeathmatch : RustPlugin
{
    [PluginReference]
     EventManager EventManager;
    [PluginReference]
     Plugin ZoneManager;
    private bool useThisEvent;
    private bool EventStarted;
    private bool Changed;
    public string CurrentKit;
    private List<DeathmatchPlayer> DeathmatchPlayers;
     class DeathmatchPlayer : MonoBehaviour
    {
        public BasePlayer player;
        public int kills;
         void Awake();
    }

     class LeaderBoard
    {
        public string Name;
        public int Kills;
    }

     void Loaded();
     void OnServerInitialized();
     void RegisterGame();
    protected override void LoadDefaultConfig();
     void Unload();
    static string DefaultKit;
    static string EventName;
    static string EventZoneName;
    static string EventSpawnFile;
    static int EventWinKills;
    static float EventStartHealth;
    static Dictionary<string, object> EventZoneConfig;
    static string EventMessageWon;
    static string EventMessageNoMorePlayers;
    static string EventMessageKill;
    static string EventMessageOpenBroadcast;
    static int TokensAddKill;
    static int TokensAddWon;
    private void LoadVariables();
    private void LoadConfigVariables();
    private void CheckCfg(string Key, T var);
    private void CheckCfgFloat(string Key, float var);
     object GetConfig(string menu, string datavalue, object defaultValue);
     object GetEventConfig(string configname);
    private List<DeathmatchPlayer> SortScores();
    private string PlayerMsg(int key, DeathmatchPlayer player);
    private CuiElementContainer CreateScoreboard(BasePlayer player);
    private void RefreshUI();
    private void AddUI(BasePlayer player);
    private void DestroyUI(BasePlayer player);
     void OnSelectEventGamePost(string name);
     void OnEventPlayerSpawn(BasePlayer player);
     object OnSelectSpawnFile(string name);
     void OnSelectEventZone(MonoBehaviour monoplayer, string radius);
     void OnPostZoneCreate(string name);
     object CanEventOpen();
     object CanEventStart();
     object OnEventOpenPost();
     object OnEventCancel();
     object OnEventClosePost();
     object OnEventEndPre();
     object OnEventEndPost();
     object OnEventStartPre();
     object OnEventStartPost();
     object CanEventJoin();
     object OnSelectKit(string kitname);
     object OnEventJoinPost(BasePlayer player);
     object OnEventLeavePost(BasePlayer player);
     void OnEventPlayerAttack(BasePlayer attacker, HitInfo hitinfo);
     void OnEventPlayerDeath(BasePlayer victim, HitInfo hitinfo);
     object EventChooseSpawn(BasePlayer player, Vector3 destination);
     object OnRequestZoneName();
     void AddKill(BasePlayer player, BasePlayer victim);
     void CheckScores(bool timelimitreached);
     void Winner(BasePlayer player);
}

 class DeathmatchPlayer : MonoBehaviour
{
    public BasePlayer player;
    public int kills;
     void Awake();
}

 class LeaderBoard
{
    public string Name;
    public int Kills;
}


```

---

## Authentication

```csharp
using System;
using System.Collections.Generic;

Oxide.Plugins
[Info("Authentication", "JosÃ© Paulo (FaD)", 1.1)]
[Description("Players must enter a password after they wake up or else they'll be kicked.")]
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
     void Loaded();
     void OnPlayerSleepEnded(BasePlayer player);
     void OnPlayerDisconnected(BasePlayer player);
     object OnPlayerChat(ConsoleSystem.Arg arg);
    protected override void LoadDefaultConfig();
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

## AuthLimits

```csharp

Oxide.Plugins
[Info("AuthLimits", "Hougan", "0.0.1")]
[Description("Ограничение на авторизация в шкафах/замках/турелях")]
public class AuthLimits : RustPlugin
{
    private int MaxAuthorize;
    private int DeAuthNumber;
    private void OnCupboardAuthorize(BuildingPrivlidge privilege, BasePlayer player);
    private object OnTurretAuthorize(AutoTurret turret, BasePlayer player);
    private object OnCodeEntered(CodeLock codeLock, BasePlayer player, string code);
}


```

---

## AuthLite

```csharp
using System;
using System.IO;
using System.Collections.Generic;
using System.Collections;
using Network;
using Oxide.Core;
using Oxide.Core.Libraries;
using UnityEngine;
using System.Reflection;

Oxide.Plugins
[Info("AuthLite", "DeathGX & ShadowRemove", "0.5")]
[Description("Automatic id authentication")]
public class AuthLite : RustPlugin
{
     Dictionary<ulong,string> users;
     Dictionary<ulong,string> lastSaved;
     string id;
     string token;
     void Webhook(string msg);
     void OnServerInitialized();
     bool EqualUsers(Dictionary<ulong,string> a, Dictionary<ulong,string> b);
     void OnServerSave();
     object OnUserApprove(Connection conn);
    public static IEnumerator EAC(Connection connection);
    public static IEnumerator FakeSteam(Connection connection);
    public IEnumerator AuthRoutine(Connection connection, ConnectionAuth auth);
}


```

---

## AutoBaseUpgrade

```csharp
using System;
using System.Collections;
using System.Collections.Generic;
using System.Linq;
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using UnityEngine;

Oxide.Plugins
[Info("AutoBaseUpgrade", "", "1.0.7")]
internal class AutoBaseUpgrade : RustPlugin
{
    [PluginReference]
    private Plugin ImageLibrary;
    private Plugin NoEscape;
    private Dictionary<BuildingPrivlidge, BuildSettings> TCList;
    private Configuration _config;
    private const string perm;
    private class Configuration
    {
        [JsonProperty("Allow the repair of deployable objects?")]
        public bool DepRepair;
        [JsonProperty("Upgrade cooldown (seconds)")]
        public Dictionary<string, float> CDList;
        [JsonProperty("Cost Modifier for repairs")]
        public Dictionary<string, float> CostList;
        [JsonProperty("Run upgrade effect")]
        public bool Effect;
        [JsonProperty("AnchorMin")]
        public string AnchorMin;
        [JsonProperty("AnchorMax")]
        public string AnchorMax;
        [JsonProperty("OffsetMin")]
        public string OffsetMin;
        [JsonProperty("OffsetMax")]
        public string OffsetMax;
    }

    protected override void LoadConfig();
    protected override void SaveConfig();
    protected override void LoadDefaultConfig();
    private void OnServerInitialized();
    private void Unload();
    private float GetCD(BasePlayer player);
    private float GetCost(BasePlayer player);
    private bool DoRepair(BasePlayer player, BaseCombatEntity entity, BuildingPrivlidge tc, float cost);
    private IEnumerator RepairProgress(BasePlayer player, BuildingPrivlidge tc);
    private IEnumerator UpdateProgress(BasePlayer player, BuildingPrivlidge tc);
    private class BuildSettings
    {
        public BuildingGrade.Enum currentGrade;
        public Coroutine _cor;
        public Coroutine _corRepair;
        public bool isUpgrade;
        public bool isRepair;
    }

    private bool IsUpgradeBlocked(BuildingBlock block);
    private void PayForUpgrade(BuildingBlock block, BuildingPrivlidge tc, BuildingGrade.Enum grade, List<Item> itemList, BasePlayer initiator);
    private void ShowUI(BasePlayer player, BuildingPrivlidge tc);
    [ConsoleCommand("UI_UPGRADEBASEUP")]
    private void cmdConsoleUI_UPGRADEBASEUP(ConsoleSystem.Arg arg);
    private bool CanUpgrade(BuildingPrivlidge tc, BuildingBlock block, BuildingGrade.Enum iGrade);
    private void OnLootEntity(BasePlayer player, BuildingPrivlidge tc);
    private void OnLootEntityEnd(BasePlayer player, BaseCombatEntity entity);
    private static void Take(IEnumerable<Item> itemList, string shortname, int iAmount);
    protected override void LoadDefaultMessages();
    private string GetMessage(string langKey, string steamID);
    private string GetMessage(string langKey, string steamID, object[] args);
}

private class Configuration
{
    [JsonProperty("Allow the repair of deployable objects?")]
    public bool DepRepair;
    [JsonProperty("Upgrade cooldown (seconds)")]
    public Dictionary<string, float> CDList;
    [JsonProperty("Cost Modifier for repairs")]
    public Dictionary<string, float> CostList;
    [JsonProperty("Run upgrade effect")]
    public bool Effect;
    [JsonProperty("AnchorMin")]
    public string AnchorMin;
    [JsonProperty("AnchorMax")]
    public string AnchorMax;
    [JsonProperty("OffsetMin")]
    public string OffsetMin;
    [JsonProperty("OffsetMax")]
    public string OffsetMax;
}

private class BuildSettings
{
    public BuildingGrade.Enum currentGrade;
    public Coroutine _cor;
    public Coroutine _corRepair;
    public bool isUpgrade;
    public bool isRepair;
}


```

---

## AutoBP

```csharp
using System.Collections.Generic;
using System.Linq;
using System;
using UnityEngine;
using Network;

Oxide.Plugins
[Info("AutoBP", "Ryamkk", "1.0.3")]
public class AutoBP : RustPlugin
{
     void Loaded();
    private void OnServerInitialized();
    private List<string> BPes;
    private void OnPlayerInit(BasePlayer player);
}


```

---

## AutoCleanup

```csharp
using System.Collections.Generic;
using System.Collections;
using System.Linq;
using UnityEngine;
using System;

Oxide.Plugins
[Info("AutoCleanup", "Server-rust.ru fixed by pahan0772", "1.0.8")]
 class AutoCleanup : RustPlugin
{
     bool Changed;
     bool logToConsole;
     bool broadcastToChat;
     bool cleanOnLoad;
     float timerIntervalInSeconds;
     float decayPercentage;
     string commandPermission;
     string excludePermission;
     string cleanupChatCommand;
     string cleanupConsoleCommand;
     List<object> entityList;
     List<object> GetDefaultEntityList();
     void Init();
     void cmdCleanupChatCommand(BasePlayer player);
     void cmdCleanupConsoleCommand(ConsoleSystem.Arg arg);
     void CleanUp();
     void CCleanUp();
     void LoadDefaultMessages();
     void RegisterPermissions();
    protected override void LoadDefaultConfig();
     void LoadVariables();
     object GetConfig(string menu, string dataValue, object defaultValue);
     string Lang(string key, string id, object[] args);
}


```

---

## AutoCleanup-1.0.0

```csharp
using System.Collections.Generic;
using System.Collections;
using System.Linq;
using UnityEngine;
using System;

Oxide.Plugins
[Info("AutoCleanup", "mvrb", "1.0.0")]
 class AutoCleanup : RustPlugin
{
     bool Changed;
     bool logToConsole;
     bool broadcastToChat;
     bool cleanOnLoad;
     float timerIntervalInSeconds;
     float decayPercentage;
     string commandPermission;
     string excludePermission;
     string cleanupChatCommand;
     string cleanupConsoleCommand;
     List<object> entityList;
     List<object> GetDefaultEntityList();
     void Init();
     void cmdCleanupChatCommand(BasePlayer player);
     void cmdCleanupConsoleCommand(ConsoleSystem.Arg arg);
     void CleanUp();
     void LoadDefaultMessages();
     void RegisterPermissions();
    protected override void LoadDefaultConfig();
     void LoadVariables();
     object GetConfig(string menu, string dataValue, object defaultValue);
     string Lang(string key, string id, object[] args);
}


```

---

## AutoClientCommands

```csharp
using System.Collections.Generic;

Oxide.Plugins
[Info("Automatic Client Commands", "k1lly0u", "0.1.0", ResourceId = 0)]
 class AutoClientCommands : RustPlugin
{
     void OnServerInitialized();
     void OnPlayerInit(BasePlayer player);
    private void RunCommands(BasePlayer player);
    [ConsoleCommand("addcommand")]
    private void ccmdAddCommand(ConsoleSystem.Arg arg);
    [ConsoleCommand("removecommand")]
    private void ccmdRemoveCommand(ConsoleSystem.Arg arg);
    private ConfigData configData;
     class ConfigData
    {
        public List<string> Commands { get; set; }
    }

    private void LoadVariables();
    protected override void LoadDefaultConfig();
    private void LoadConfigVariables();
     void SaveConfig(ConfigData config);
}

 class ConfigData
{
    public List<string> Commands { get; set; }
}


```

---

## AutoCodeLock

```csharp
using System;
using System.Collections.Generic;
using System.Collections;
using System.Linq;
using System.Text;
using Oxide.Core;
using Oxide.Core.Plugins;
using UnityEngine;
using ProtoBuf;

Oxide.Plugins
[Info("AutoCodeLock", "FuJiCuRa", "2.2.12", ResourceId = 15)]
[Description("CodeLock & Door automation tools")]
 class AutoCodeLock : RustPlugin
{
    [PluginReference]
     Plugin NoEscape;
     bool Changed;
     bool Initialized;
    static AutoCodeLock ACL;
     StoredData playerPrefs;
     List<ulong> usedConsoleInput;
     DateTime Epoch;
    [ProtoContract(ImplicitFields = ImplicitFields.AllFields)]
     class StoredData
    {
        public Dictionary<ulong,
            PlayerInfo> PlayerInfo;
        public Int32 saveStamp;
        public string lastStorage;
        public StoredData();
    }

    [ProtoContract(ImplicitFields = ImplicitFields.AllFields)]
     class PlayerInfo
    {
        public bool AL;
        public bool DLD;
        public bool DLB;
        public bool DLL;
        public bool DLC;
        public string PC;
        public string GC;
        public bool EGC;
        public bool DDC;
        public float DCD;
        public float LHD;
        public PlayerInfo();
    }

     bool useProtostorageUserdata;
     bool autoUpdateChangedDelays;
     string codelockCommand;
     bool notifyAuthCodeLock;
     string permissionDeployDoor;
     string permissionDeployBox;
     string permissionDeployLocker;
     string permissionDeployCupboard;
     string permissionAutoLock;
     string permissionNoLockNeed;
     string permissionDoorCloser;
     string permissionAll;
     string pluginPrefix;
     string prefixColor;
     string prefixFormat;
     string colorTextMsg;
     string colorCmdUsage;
     string colorON;
     string colorOFF;
     bool adminAutoRights;
     bool autoLock;
     bool deployDoor;
     bool deployBox;
     bool deployLocker;
     bool deployCupboard;
     bool enableGuestCode;
     bool deployDoorCloser;
     float doorCloserMinDelay;
     float doorCloserMaxDelay;
     float defaultCloseDelay;
     float ladderHatchMinDelay;
     float ladderHatchMaxDelay;
     float defaultHatchDelay;
     bool checkPlayerForRaidBlocked;
     bool checkPlayerForCombatBlocked;
     bool denyCloserPickupByPlayer;
     bool denyCloserPickupByAdmin;
     object GetConfig(string menu, string datavalue, object defaultValue);
     void LoadVariables();
    protected override void LoadDefaultConfig();
    protected override void LoadDefaultMessages();
     void Init();
     void Loaded();
     void LoadPlayerData();
     void OnServerInitialized();
     void SetPlayer(BasePlayer player);
     void OnPlayerConnected(BasePlayer player);
     Coroutine _updateCloserDelays;
     Coroutine _updateDoorCodes;
     Coroutine _updateGuestCodes;
     Coroutine _refreshDoorClosers;
     Coroutine _refreshAllDoorCloserDelays;
     void Unload();
     void OnServerSave();
     void SaveData();
     void OnItemDeployed(Deployer deployer, BaseEntity entity);
     void CodeLockPrepare(Deployer deployer, BaseEntity entity, BasePlayer owner);
     string GetOrSetPin(ulong userID);
     string GetOrSetGuest(ulong userID);
     WaitForEndOfFrame waitF;
     IEnumerator RefreshAllDoorCloserDelays();
     IEnumerator RefreshDoorClosers(ConsoleSystem.Arg arg);
    [ConsoleCommand("acl.refreshclosers")]
     void refreshClosers(ConsoleSystem.Arg arg);
     IEnumerator UpdateCloserDelays(BasePlayer player);
     IEnumerator UpdateDoorCodes(BasePlayer player);
     IEnumerator UpdateGuestCodes(BasePlayer player);
     void LockPlacing(BasePlayer player, BaseEntity entity);
     void CloserPlacing(ulong userID, BaseEntity entity);
     void OnEntityBuilt(Planner planner, GameObject obj);
     object CanPickupEntity(BasePlayer player, DoorCloser closer);
     Boolean PlayerHasPerm(string UserIDString, string permissionName, bool isAdmin);
     Boolean PlayerHasUpdatePerm(string UserIDString, bool isAdmin);
     void cCodeLockCommand(ConsoleSystem.Arg arg);
     void CodeLockCommand(BasePlayer player, string command, string[] args);
     void PrintChat(BasePlayer player, string message, bool keepConsole);
}

[ProtoContract(ImplicitFields = ImplicitFields.AllFields)]
 class StoredData
{
    public Dictionary<ulong,
            PlayerInfo> PlayerInfo;
    public Int32 saveStamp;
    public string lastStorage;
    public StoredData();
}

[ProtoContract(ImplicitFields = ImplicitFields.AllFields)]
 class PlayerInfo
{
    public bool AL;
    public bool DLD;
    public bool DLB;
    public bool DLL;
    public bool DLC;
    public string PC;
    public string GC;
    public bool EGC;
    public bool DDC;
    public float DCD;
    public float LHD;
    public PlayerInfo();
}


```

---

## AutoDecay

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

## AutoDoorCloser

```csharp
using System;
using Oxide.Core;
using System.Linq;
using UnityEngine;
using System.Text;
using System.Reflection;
using Oxide.Core.Plugins;
using Oxide.Core.Libraries;
using Oxide.Core.Configuration;
using System.Collections.Generic;
using Oxide.Core.Libraries.Covalence;

Oxide.Plugins
[Info("AutoDoorCloser", "Ryamkk", "1.0.5", ResourceId = 1924)]
[Description("Переработка плагина AutoDoor с oxide. Почти схожая версия плагина с Moscow.ovh :)")]
 class AutoDoorCloser : RustPlugin
{
    [PluginReference]
     Plugin NoEscape;
    readonly DynamicConfigFile dataFile;
     List<string> playerPrefs;
     bool CancelOnKill;
    private string Permission;
    private bool PermissionSupport;
    private int DelayDoor;
    private bool UseRaidBlocker;
    protected override void LoadDefaultConfig();
     void OnDoorOpened(Door door, BasePlayer player);
     void OnServerInitialized();
     void Init();
     bool IsRaidBlocked(string targetId);
    [ChatCommand("ad")]
     void AutoDoorCommand(BasePlayer player, string command, string[] args);
     void LoadDefaultMessages();
     string GetMessage(string key, BasePlayer player, string[] args);
     bool IsAllowed(string id, string perm);
     T GetConfig(string name, T defaultValue);
    public static void GetVariable(DynamicConfigFile config, string name, T value, T defaultValue);
}


```

---

## AutoDoors

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

## AutoFurnaces

```csharp

Oxide.Plugins
[Info("Auto Furnaces", "Waizujin", 1.1)]
[Description("Automatically starts all furnaces after a server restart.")]
public class AutoFurnaces : RustPlugin
{
     void OnServerInitialized();
}


```

---

## AutoGrades

```csharp
using System;
using System.Collections.Generic;
using Oxide.Core;
using Oxide.Core.Libraries.Covalence;

Oxide.Plugins
[Info("Automatic Build Grades", "AlexALX", "0.0.18", ResourceId = 921)]
[Description("Auto update grade on build to what you need")]
public class AutoGrades : CovalencePlugin
{
    private Dictionary<string, Timer> CheckTimer;
    private Dictionary<string, PlayerGrades> playerGrades;
    private bool LoadDefault;
    private bool block;
    private string cmdname;
    private bool allow_timer;
    private int def_timer;
    private class PlayerGrades
    {
        public int Grade { get; set; }
        public int Timer { get; set; }
        public PlayerGrades(int grade, int timer);
    }

    protected override void LoadDefaultConfig();
    private void OnServerInitialized();
    protected override void LoadDefaultMessages();
    private void ReadFromConfig(string Key, T var);
    private bool HasPerm(string steamId, string perm);
    private bool HasAnyPerm(BasePlayer player);
    private string GetMessage(string name, string steamId);
    private int PlayerTimer(string steamId, bool cache);
    private void UpdateTimer(BasePlayer player);
    private int PlayerGrade(string steamId, bool cache);
    private void OnPlayerDisconnected(BasePlayer player);
    private bool CanAffordToUpgrade(BasePlayer player, int grade, BuildingBlock buildingBlock);
    private void OnEntityBuilt(Planner planner, UnityEngine.GameObject gameObject);
    private void BuildGradeCommand(IPlayer player, string command, string[] args);
}

private class PlayerGrades
{
    public int Grade { get; set; }
    public int Timer { get; set; }
    public PlayerGrades(int grade, int timer);
}


```

---

## AutoKit

```csharp
using Newtonsoft.Json;
using System.Collections.Generic;
using UnityEngine;

Oxide.Plugins
[Info("AutoKit", "walkinrey", "1.0.2")]
public class AutoKit : RustPlugin
{
    private Configuration _config;
    public class Configuration
    {
        [JsonProperty("Биомы")]
        public Biomes biomes;
        public class Biomes
        {
            [JsonProperty("Тундра")]
            public Biome tundra;
            [JsonProperty("Пустыня")]
            public Biome dust;
            [JsonProperty("Зима")]
            public Biome winter;
            [JsonProperty("Умеренный")]
            public Biome normal;
        }

        public class Biome
        {
            [JsonProperty("Разрешение для использования")]
            public string permission;
            [JsonProperty("Выдавать набор в этом биоме?")]
            public bool recive;
            [JsonProperty("Очищать инвентарь перед выдачей?")]
            public bool strip;
            [JsonProperty("Предметы к выдаче")]
            public List<ItemConfig> items;
            public class ItemConfig
            {
                [JsonProperty("Отображаемое название")]
                public string name;
                [JsonProperty("Скин ID предмета")]
                public ulong id;
                [JsonProperty("Shortname предмета")]
                public string shortname;
                [JsonProperty("Количество")]
                public int amount;
                [JsonProperty("В какой контейнер помещать? (wear, main, belt)")]
                public string container;
                public ItemConfig(ulong sourceID, string sourceShortname, int sourceAmount, string sourceContainer);
            }

            public Biome(string defaultPermission, bool fillDefaultItem);
        }

        public static Configuration GetDefault();
    }

    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    protected override void LoadConfig();
    private void Loaded();
    private void OnDefaultItemsReceived(PlayerInventory inventory);
    [ChatCommand("autokit")]
    private void chatCmd(BasePlayer player, string command, string[] args);
    private void SetupNewItems(BasePlayer player, string caseStr);
    private static ItemContainer GetContainer(BasePlayer player, string container);
    private void ReciveItems(BasePlayer player, Configuration.Biome biome);
    private static string GetBiome(Vector3 pos);
}

public class Configuration
{
    [JsonProperty("Биомы")]
    public Biomes biomes;
    public class Biomes
    {
        [JsonProperty("Тундра")]
        public Biome tundra;
        [JsonProperty("Пустыня")]
        public Biome dust;
        [JsonProperty("Зима")]
        public Biome winter;
        [JsonProperty("Умеренный")]
        public Biome normal;
    }

    public class Biome
    {
        [JsonProperty("Разрешение для использования")]
        public string permission;
        [JsonProperty("Выдавать набор в этом биоме?")]
        public bool recive;
        [JsonProperty("Очищать инвентарь перед выдачей?")]
        public bool strip;
        [JsonProperty("Предметы к выдаче")]
        public List<ItemConfig> items;
        public class ItemConfig
        {
            [JsonProperty("Отображаемое название")]
            public string name;
            [JsonProperty("Скин ID предмета")]
            public ulong id;
            [JsonProperty("Shortname предмета")]
            public string shortname;
            [JsonProperty("Количество")]
            public int amount;
            [JsonProperty("В какой контейнер помещать? (wear, main, belt)")]
            public string container;
            public ItemConfig(ulong sourceID, string sourceShortname, int sourceAmount, string sourceContainer);
        }

        public Biome(string defaultPermission, bool fillDefaultItem);
    }

    public static Configuration GetDefault();
}

public class Biomes
{
    [JsonProperty("Тундра")]
    public Biome tundra;
    [JsonProperty("Пустыня")]
    public Biome dust;
    [JsonProperty("Зима")]
    public Biome winter;
    [JsonProperty("Умеренный")]
    public Biome normal;
}

public class Biome
{
    [JsonProperty("Разрешение для использования")]
    public string permission;
    [JsonProperty("Выдавать набор в этом биоме?")]
    public bool recive;
    [JsonProperty("Очищать инвентарь перед выдачей?")]
    public bool strip;
    [JsonProperty("Предметы к выдаче")]
    public List<ItemConfig> items;
    public class ItemConfig
    {
        [JsonProperty("Отображаемое название")]
        public string name;
        [JsonProperty("Скин ID предмета")]
        public ulong id;
        [JsonProperty("Shortname предмета")]
        public string shortname;
        [JsonProperty("Количество")]
        public int amount;
        [JsonProperty("В какой контейнер помещать? (wear, main, belt)")]
        public string container;
        public ItemConfig(ulong sourceID, string sourceShortname, int sourceAmount, string sourceContainer);
    }

    public Biome(string defaultPermission, bool fillDefaultItem);
}

public class ItemConfig
{
    [JsonProperty("Отображаемое название")]
    public string name;
    [JsonProperty("Скин ID предмета")]
    public ulong id;
    [JsonProperty("Shortname предмета")]
    public string shortname;
    [JsonProperty("Количество")]
    public int amount;
    [JsonProperty("В какой контейнер помещать? (wear, main, belt)")]
    public string container;
    public ItemConfig(ulong sourceID, string sourceShortname, int sourceAmount, string sourceContainer);
}


```

---

## AutoKit-9.9.9

```csharp
using Newtonsoft.Json;
using System.Collections.Generic;
using System.Linq;
using UnityEngine;
using Oxide.Core;

Oxide.Plugins
[Info("AutoKit", "no name666", "9.9.9")]
public class AutoKit : RustPlugin
{
    private Configuration _config;
    private StoredData _storedData;
    public class StoredData
    {
        public Dictionary<ulong, Dictionary<string, List<SavedLoadout>>> PlayerLoadouts;
    }

    public class SavedLoadout
    {
        public string Name { get; set; }
        public List<Configuration.Biome.ItemConfig> Items { get; set; }
    }

    public class Configuration
    {
        [JsonProperty("Биомы")]
        public Biomes biomes;
        public class Biomes
        {
            [JsonProperty("Тундра")]
            public Biome tundra;
            [JsonProperty("Пустыня")]
            public Biome dust;
            [JsonProperty("Зима")]
            public Biome winter;
            [JsonProperty("Умеренный")]
            public Biome normal;
        }

        public class Biome
        {
            [JsonProperty("Разрешение для использования")]
            public string permission;
            [JsonProperty("Выдавать набор в этом биоме?")]
            public bool recive;
            [JsonProperty("Очищать инвентарь перед выдачей?")]
            public bool strip;
            [JsonProperty("Предметы к выдаче")]
            public List<ItemConfig> items;
            public class ItemConfig
            {
                [JsonProperty("Отображаемое название")]
                public string name;
                [JsonProperty("Скин ID предмета")]
                public ulong id;
                [JsonProperty("Shortname предмета")]
                public string shortname;
                [JsonProperty("Количество")]
                public int amount;
                [JsonProperty("В какой контейнер помещать? (wear, main, belt)")]
                public string container;
                [JsonProperty("Позиция в инвентаре")]
                public int position;
                [JsonProperty("Количество патронов")]
                public int ammo;
                [JsonProperty("Тип патронов")]
                public string ammoType;
                [JsonProperty("Модификации оружия")]
                public Dictionary<string, string> mods;
                public ItemConfig(ulong sourceID, string sourceShortname, int sourceAmount, string sourceContainer, int sourcePosition);
            }

            public Biome(string defaultPermission, bool fillDefaultItem);
        }

        public static Configuration GetDefault();
    }

    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    protected override void LoadConfig();
    private void LoadData();
    private void SaveData();
    private void OnServerSave();
     void Unload();
    private void Loaded();
     void Init();
    private void OnDefaultItemsReceived(PlayerInventory inventory);
    [ChatCommand("autokit")]
    private void chatCmd(BasePlayer player, string command, string[] args);
    private void SavePlayerLoadout(BasePlayer player, string name);
    private void LoadPlayerLoadout(BasePlayer player, string name);
    private void SaveItemsFromContainer(BasePlayer player, ItemContainer container, string containerType, List<Configuration.Biome.ItemConfig> items);
    private void CreateAndGiveItem(BasePlayer player, Configuration.Biome.ItemConfig itemConfig);
    private void SetupNewItems(BasePlayer player, string caseStr);
    private void ReciveItems(BasePlayer player, Configuration.Biome biome);
    private static ItemContainer GetContainer(BasePlayer player, string container);
    private static string GetBiome(Vector3 pos);
}

public class StoredData
{
    public Dictionary<ulong, Dictionary<string, List<SavedLoadout>>> PlayerLoadouts;
}

public class SavedLoadout
{
    public string Name { get; set; }
    public List<Configuration.Biome.ItemConfig> Items { get; set; }
}

public class Configuration
{
    [JsonProperty("Биомы")]
    public Biomes biomes;
    public class Biomes
    {
        [JsonProperty("Тундра")]
        public Biome tundra;
        [JsonProperty("Пустыня")]
        public Biome dust;
        [JsonProperty("Зима")]
        public Biome winter;
        [JsonProperty("Умеренный")]
        public Biome normal;
    }

    public class Biome
    {
        [JsonProperty("Разрешение для использования")]
        public string permission;
        [JsonProperty("Выдавать набор в этом биоме?")]
        public bool recive;
        [JsonProperty("Очищать инвентарь перед выдачей?")]
        public bool strip;
        [JsonProperty("Предметы к выдаче")]
        public List<ItemConfig> items;
        public class ItemConfig
        {
            [JsonProperty("Отображаемое название")]
            public string name;
            [JsonProperty("Скин ID предмета")]
            public ulong id;
            [JsonProperty("Shortname предмета")]
            public string shortname;
            [JsonProperty("Количество")]
            public int amount;
            [JsonProperty("В какой контейнер помещать? (wear, main, belt)")]
            public string container;
            [JsonProperty("Позиция в инвентаре")]
            public int position;
            [JsonProperty("Количество патронов")]
            public int ammo;
            [JsonProperty("Тип патронов")]
            public string ammoType;
            [JsonProperty("Модификации оружия")]
            public Dictionary<string, string> mods;
            public ItemConfig(ulong sourceID, string sourceShortname, int sourceAmount, string sourceContainer, int sourcePosition);
        }

        public Biome(string defaultPermission, bool fillDefaultItem);
    }

    public static Configuration GetDefault();
}

public class Biomes
{
    [JsonProperty("Тундра")]
    public Biome tundra;
    [JsonProperty("Пустыня")]
    public Biome dust;
    [JsonProperty("Зима")]
    public Biome winter;
    [JsonProperty("Умеренный")]
    public Biome normal;
}

public class Biome
{
    [JsonProperty("Разрешение для использования")]
    public string permission;
    [JsonProperty("Выдавать набор в этом биоме?")]
    public bool recive;
    [JsonProperty("Очищать инвентарь перед выдачей?")]
    public bool strip;
    [JsonProperty("Предметы к выдаче")]
    public List<ItemConfig> items;
    public class ItemConfig
    {
        [JsonProperty("Отображаемое название")]
        public string name;
        [JsonProperty("Скин ID предмета")]
        public ulong id;
        [JsonProperty("Shortname предмета")]
        public string shortname;
        [JsonProperty("Количество")]
        public int amount;
        [JsonProperty("В какой контейнер помещать? (wear, main, belt)")]
        public string container;
        [JsonProperty("Позиция в инвентаре")]
        public int position;
        [JsonProperty("Количество патронов")]
        public int ammo;
        [JsonProperty("Тип патронов")]
        public string ammoType;
        [JsonProperty("Модификации оружия")]
        public Dictionary<string, string> mods;
        public ItemConfig(ulong sourceID, string sourceShortname, int sourceAmount, string sourceContainer, int sourcePosition);
    }

    public Biome(string defaultPermission, bool fillDefaultItem);
}

public class ItemConfig
{
    [JsonProperty("Отображаемое название")]
    public string name;
    [JsonProperty("Скин ID предмета")]
    public ulong id;
    [JsonProperty("Shortname предмета")]
    public string shortname;
    [JsonProperty("Количество")]
    public int amount;
    [JsonProperty("В какой контейнер помещать? (wear, main, belt)")]
    public string container;
    [JsonProperty("Позиция в инвентаре")]
    public int position;
    [JsonProperty("Количество патронов")]
    public int ammo;
    [JsonProperty("Тип патронов")]
    public string ammoType;
    [JsonProperty("Модификации оружия")]
    public Dictionary<string, string> mods;
    public ItemConfig(ulong sourceID, string sourceShortname, int sourceAmount, string sourceContainer, int sourcePosition);
}


```

---

## AutoLearnBlueprints

```csharp
using System.Collections.Generic;
using System.Linq;
using Oxide.Core.Plugins;

Oxide.Plugins
[Info("AutoLearnBlueprints", "ApocDev", "1.0.5", ResourceId = 1056)]
public class AutoLearnBlueprints : RustPlugin
{
    private static readonly List<string> DefaultIncludeBps;
    private List<string> LearnBlueprints { get; set; }
    protected override void LoadDefaultConfig();
     void OnServerInitialized();
     void LearnDefaultBlueprints(BasePlayer player);
    [ConsoleCommand("bps.update")]
     void HandleUpdateBlueprintsCommand();
     void OnPlayerInit(BasePlayer player);
}


```

---

## AutomatedSearchlights

```csharp
using Facepunch;
using Rust;
using System.Collections.Generic;
using System.Diagnostics;
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Configuration;
using Oxide.Game.Rust.Cui;
using UnityEngine;
using System.Linq;
using System.Collections;

Oxide.Plugins
[Info("AutomatedSearchlights", "k1lly0u", "0.2.30")]
[Description("Create searchlights that will automatically turn on and follow a variety of objects")]
 class AutomatedSearchlights : RustPlugin
{
    private StoredData storedData;
    private DynamicConfigFile data;
    private static AutomatedSearchlights ins;
    private static LinkManager linkManager;
    private static int ctrlLayerMask;
    private FrameBudgeter frameBudgeter;
    private bool wipeDetected;
    private bool automationEnabled;
    private bool isInitialized;
    private bool nlConsume;
    private const string PERM_USE;
    private const string PERM_IGNORE;
    private const string FX_OFFLINE;
    private const string FX_ONLINE;
    private const string FX_ACQUIRED;
    private const string FX_LOST;
    private const string ASUI_OVERLAY;
    private void Loaded();
    private void OnServerInitialized();
    private void OnNewSave(string filename);
    private void OnServerSave();
    private void OnEntitySpawned(SearchLight searchLight);
    private void OnEntityKill(BaseNetworkable networkable);
    private void OnPlayerDisconnected(BasePlayer player);
    private object OnPlayerCommand(BasePlayer player, string command, string[] args);
    private object OnPlayerTick(BasePlayer player, PlayerTick msg, bool wasPlayerStalled);
    private void Unload();
    private void InitializeAllLinks();
    private void MonitorTime(bool firstLoad);
    private IEnumerator ToggleAllLights(bool status);
    private int GetMaxLights(ulong playerId);
    private void DestroyAllLinks();
    private void OnLinkShutdown(int terminalId);
    private void OnLinkDestroyed(int terminalId);
    private LightManager[] GetAvailableSearchlights(int terminalId);
    private bool IsEntityEnabled(int terminalId, uint managerId);
    private void InitializeController(BasePlayer player, uint managerId, int terminalId);
    private void ToggleAutomation(uint managerId, int terminalId, bool active);
    private string GetHelpString(ulong playerId, bool title);
    private bool AllowPublicAccess();
    private class LinkManager
    {
        public List<LightLink> links;
        public LightLink GetLinkOf(LightManager manager);
        public LightLink GetLinkOf(Controller controller);
        public LightLink GetLinkOf(int terminalId);
        public LightLink GetLinkOf(SearchLight searchLight);
        public void OnEntityDeath(BaseNetworkable networkable);
        public void DestroyAllLinks();
        public class LightLink
        {
            public int TerminalID { get; set; }
            public List<Controller> Controllers { get; set; }
            public List<LightManager> Managers { get; set; }
            public LightLink();
            public LightLink(int terminalId, SearchLight searchLight, string lightName);
            public void AddLightToLink(SearchLight searchLight, int terminalId, string lightName);
            public void InitiateLink(BasePlayer player, uint managerId);
            public void CloseLink(Controller controller, bool isDead);
            public void OnEntityDeath(BaseNetworkable networkable);
            public void OnLinkTerminated(bool isDestroyed);
            private void DestroyLightManagers(bool isDestroyed);
        }

    }

    private class FrameBudgeter : MonoBehaviour
    {
        private double maxMilliseconds;
        private Stopwatch sw;
        private int lastIndex;
        private void Update();
    }

    private class LightManager : MonoBehaviour
    {
        internal static List<LightManager> allManagers;
        public SearchLight Searchlight { get; set; }
        public Controller Controller { get; set; }
        private SphereCollider collider;
        private Rigidbody rb;
        private BaseCombatEntity targetEntity;
        private float maxTargetDistance;
        private int threatLevel;
        private float searchRadius;
        private bool requiresPower;
        private ConfigData.LightOptions.FlickerMode flicker;
        private ConfigData.LightOptions.SearchMode search;
        private ConfigData.DetectionOptions detection;
        private Hash<int, HashSet<BaseCombatEntity>> threats;
        private Hash<int, float> threatRadius;
        private float[] visabilityOffsets;
        private float nextCheckTime;
        private float threatProbabilityRate;
        private float idleTime;
        private float lostTargetTime;
        private Vector3[] searchPattern;
        private int lastSearchPoint;
        private int nextSearchPoint;
        private bool searchForwards;
        private float searchTime;
        private bool resetSearch;
        private bool isSearching;
        public string lightName;
        public int terminalId;
        public bool isDisabled;
        private void Awake();
        internal void DoUpdate();
        private void OnDestroy();
        private void OnTriggerEnter(Collider col);
        private void OnTriggerExit(Collider col);
        private void InitializeSettings();
        private void GenerateSearchPattern();
        private void SetNextSearchPoint();
        private void CalculateThreatProbability();
        private void SetAimTarget(BaseCombatEntity target, int threatRating);
        private void ClearAimTarget();
        private bool IsObjectVisible(BaseCombatEntity obj);
        private Vector3 AimOffset(BaseCombatEntity aimat);
        public void ToggleAutomation(bool status);
        public void AdjustLightRotation(float rotation);
        public void SetController(Controller controller);
        public bool IsEnabled();
        public void ToggleSearchAutomation(bool status);
        public void OnEntityDeath(BaseCombatEntity baseCombatEntity);
        public LightData GetLightData();
        public class LightData
        {
            public uint lightId;
            public int terminalId;
            public string lightName;
            public LightData();
            public LightData(LightManager manager);
        }

    }

     class Controller : RustNET.Controller
    {
        public LightManager manager { get; set; }
        private LinkManager.LightLink link;
        private int spectateIndex;
        private bool switchingTargets;
        private bool wasEnabled;
        private bool canCycle;
        private Vector3 offset;
        public bool lightsOn { get; set; }
        public override void Awake();
        private void FixedUpdate();
        public override void OnDestroy();
        public void SetLightLink(LinkManager.LightLink link);
        public void SetSpectateTarget(int spectateIndex);
        public void UpdateSpectateTarget(int index);
        public void BeginSpectating();
        public void FinishSpectating(bool isDead);
        public override void OnPlayerDeath(HitInfo info);
        private void CreateCameraOverlay();
    }

    private void CreateConsoleWindow(BasePlayer player, int terminalId, int page);
    [ConsoleCommand("automatedsearchlights.toggle")]
    private void ccmdToggle(ConsoleSystem.Arg arg);
    [ConsoleCommand("automatedsearchlights.control")]
    private void ccmdControl(ConsoleSystem.Arg arg);
    [ChatCommand("sl")]
    private void cmdSL(BasePlayer player, string command, string[] args);
    private void LoadDefaultImages(int attempts);
    private void AddImage(string imageName, string fileName);
    private string GetImage(string name);
    private ConfigData configData;
     class ConfigData
    {
        [JsonProperty(PropertyName = "Detection Options")]
        public DetectionOptions Detection { get; set; }
        [JsonProperty(PropertyName = "Management Options")]
        public LightManagement Management { get; set; }
        [JsonProperty(PropertyName = "Light Options")]
        public LightOptions Options { get; set; }
        public class DetectionOptions
        {
            [JsonProperty(PropertyName = "Animals")]
            public DetectSettings Animals { get; set; }
            [JsonProperty(PropertyName = "Enemy players and NPCs")]
            public DetectSettings Enemies { get; set; }
            [JsonProperty(PropertyName = "Friends and owner")]
            public DetectSettings Friends { get; set; }
            [JsonProperty(PropertyName = "Cars and tanks")]
            public DetectSettings Vehicles { get; set; }
            [JsonProperty(PropertyName = "Helicopters")]
            public DetectSettings Helicopters { get; set; }
            public class DetectSettings
            {
                [JsonProperty(PropertyName = "Enable this detection type")]
                public bool Enabled { get; set; }
                [JsonProperty(PropertyName = "Range of detection")]
                public float Radius { get; set; }
                [JsonProperty(PropertyName = "Threat rating for priority targeting (1 - 5, 1 being the highest threat)")]
                public int Threat { get; set; }
            }

        }

        public class LightManagement
        {
            [JsonProperty(PropertyName = "Automatically register light to terminal when placed (if meets other requirements)")]
            public bool AutoRegister { get; set; }
            [JsonProperty(PropertyName = "Allow lights to be set without requiring a terminal")]
            public bool NoTerminal { get; set; }
            [JsonProperty(PropertyName = "Maximum allowed searchlights per base (Permission | Amount)")]
            public Dictionary<string, int> Max { get; set; }
            [JsonProperty(PropertyName = "Maximum distance a searchlight controller can be set away from the terminal")]
            public float DistanceFromTerminal { get; set; }
            [JsonProperty(PropertyName = "Remote Settings")]
            public RemoteOptions Remote { get; set; }
            public class RemoteOptions
            {
                [JsonProperty(PropertyName = "Allow players to cycle through all linked searchlights")]
                public bool CanCycle { get; set; }
                [JsonProperty(PropertyName = "Can players toggle searchlight automation")]
                public bool ToggleEnabled { get; set; }
                [JsonProperty(PropertyName = "Can players control the searchlight remotely")]
                public bool RemoteControl { get; set; }
                [JsonProperty(PropertyName = "Display camera overlay UI")]
                public bool Overlay { get; set; }
                [JsonProperty(PropertyName = "Camera overlay image URL")]
                public string OverlayImage { get; set; }
                [JsonProperty(PropertyName = "Searchlight icon URL for RustNET menu")]
                public string RustNETIcon { get; set; }
            }

        }

        public class LightOptions
        {
            [JsonProperty(PropertyName = "Require power for light automation")]
            public bool RequirePower { get; set; }
            [JsonProperty(PropertyName = "Sunrise hour")]
            public float Sunrise { get; set; }
            [JsonProperty(PropertyName = "Sunset hour")]
            public float Sunset { get; set; }
            [JsonProperty(PropertyName = "Only automate lights at night time")]
            public bool NightOnly { get; set; }
            [JsonProperty(PropertyName = "Search mode")]
            public SearchMode Search { get; set; }
            [JsonProperty(PropertyName = "Flicker mode")]
            public FlickerMode Flicker { get; set; }
            public class SearchMode
            {
                [JsonProperty(PropertyName = "Enable search mode when no targets are visable")]
                public bool Enabled { get; set; }
                [JsonProperty(PropertyName = "Rotation speed of search mode")]
                public float Speed { get; set; }
            }

            public class FlickerMode
            {
                [JsonProperty(PropertyName = "Enable flickering lights when damaged")]
                public bool Enabled { get; set; }
                [JsonProperty(PropertyName = "Percentage of health before flickering starts")]
                public float Health { get; set; }
            }

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
        public LightManager.LightData[] registeredSearchlights;
    }

     string msg(string key, ulong playerId);
     Dictionary<string, string> Messages;
}

private class LinkManager
{
    public List<LightLink> links;
    public LightLink GetLinkOf(LightManager manager);
    public LightLink GetLinkOf(Controller controller);
    public LightLink GetLinkOf(int terminalId);
    public LightLink GetLinkOf(SearchLight searchLight);
    public void OnEntityDeath(BaseNetworkable networkable);
    public void DestroyAllLinks();
    public class LightLink
    {
        public int TerminalID { get; set; }
        public List<Controller> Controllers { get; set; }
        public List<LightManager> Managers { get; set; }
        public LightLink();
        public LightLink(int terminalId, SearchLight searchLight, string lightName);
        public void AddLightToLink(SearchLight searchLight, int terminalId, string lightName);
        public void InitiateLink(BasePlayer player, uint managerId);
        public void CloseLink(Controller controller, bool isDead);
        public void OnEntityDeath(BaseNetworkable networkable);
        public void OnLinkTerminated(bool isDestroyed);
        private void DestroyLightManagers(bool isDestroyed);
    }

}

public class LightLink
{
    public int TerminalID { get; set; }
    public List<Controller> Controllers { get; set; }
    public List<LightManager> Managers { get; set; }
    public LightLink();
    public LightLink(int terminalId, SearchLight searchLight, string lightName);
    public void AddLightToLink(SearchLight searchLight, int terminalId, string lightName);
    public void InitiateLink(BasePlayer player, uint managerId);
    public void CloseLink(Controller controller, bool isDead);
    public void OnEntityDeath(BaseNetworkable networkable);
    public void OnLinkTerminated(bool isDestroyed);
    private void DestroyLightManagers(bool isDestroyed);
}

private class FrameBudgeter : MonoBehaviour
{
    private double maxMilliseconds;
    private Stopwatch sw;
    private int lastIndex;
    private void Update();
}

private class LightManager : MonoBehaviour
{
    internal static List<LightManager> allManagers;
    public SearchLight Searchlight { get; set; }
    public Controller Controller { get; set; }
    private SphereCollider collider;
    private Rigidbody rb;
    private BaseCombatEntity targetEntity;
    private float maxTargetDistance;
    private int threatLevel;
    private float searchRadius;
    private bool requiresPower;
    private ConfigData.LightOptions.FlickerMode flicker;
    private ConfigData.LightOptions.SearchMode search;
    private ConfigData.DetectionOptions detection;
    private Hash<int, HashSet<BaseCombatEntity>> threats;
    private Hash<int, float> threatRadius;
    private float[] visabilityOffsets;
    private float nextCheckTime;
    private float threatProbabilityRate;
    private float idleTime;
    private float lostTargetTime;
    private Vector3[] searchPattern;
    private int lastSearchPoint;
    private int nextSearchPoint;
    private bool searchForwards;
    private float searchTime;
    private bool resetSearch;
    private bool isSearching;
    public string lightName;
    public int terminalId;
    public bool isDisabled;
    private void Awake();
    internal void DoUpdate();
    private void OnDestroy();
    private void OnTriggerEnter(Collider col);
    private void OnTriggerExit(Collider col);
    private void InitializeSettings();
    private void GenerateSearchPattern();
    private void SetNextSearchPoint();
    private void CalculateThreatProbability();
    private void SetAimTarget(BaseCombatEntity target, int threatRating);
    private void ClearAimTarget();
    private bool IsObjectVisible(BaseCombatEntity obj);
    private Vector3 AimOffset(BaseCombatEntity aimat);
    public void ToggleAutomation(bool status);
    public void AdjustLightRotation(float rotation);
    public void SetController(Controller controller);
    public bool IsEnabled();
    public void ToggleSearchAutomation(bool status);
    public void OnEntityDeath(BaseCombatEntity baseCombatEntity);
    public LightData GetLightData();
    public class LightData
    {
        public uint lightId;
        public int terminalId;
        public string lightName;
        public LightData();
        public LightData(LightManager manager);
    }

}

public class LightData
{
    public uint lightId;
    public int terminalId;
    public string lightName;
    public LightData();
    public LightData(LightManager manager);
}

 class Controller : RustNET.Controller
{
    public LightManager manager { get; set; }
    private LinkManager.LightLink link;
    private int spectateIndex;
    private bool switchingTargets;
    private bool wasEnabled;
    private bool canCycle;
    private Vector3 offset;
    public bool lightsOn { get; set; }
    public override void Awake();
    private void FixedUpdate();
    public override void OnDestroy();
    public void SetLightLink(LinkManager.LightLink link);
    public void SetSpectateTarget(int spectateIndex);
    public void UpdateSpectateTarget(int index);
    public void BeginSpectating();
    public void FinishSpectating(bool isDead);
    public override void OnPlayerDeath(HitInfo info);
    private void CreateCameraOverlay();
}

 class ConfigData
{
    [JsonProperty(PropertyName = "Detection Options")]
    public DetectionOptions Detection { get; set; }
    [JsonProperty(PropertyName = "Management Options")]
    public LightManagement Management { get; set; }
    [JsonProperty(PropertyName = "Light Options")]
    public LightOptions Options { get; set; }
    public class DetectionOptions
    {
        [JsonProperty(PropertyName = "Animals")]
        public DetectSettings Animals { get; set; }
        [JsonProperty(PropertyName = "Enemy players and NPCs")]
        public DetectSettings Enemies { get; set; }
        [JsonProperty(PropertyName = "Friends and owner")]
        public DetectSettings Friends { get; set; }
        [JsonProperty(PropertyName = "Cars and tanks")]
        public DetectSettings Vehicles { get; set; }
        [JsonProperty(PropertyName = "Helicopters")]
        public DetectSettings Helicopters { get; set; }
        public class DetectSettings
        {
            [JsonProperty(PropertyName = "Enable this detection type")]
            public bool Enabled { get; set; }
            [JsonProperty(PropertyName = "Range of detection")]
            public float Radius { get; set; }
            [JsonProperty(PropertyName = "Threat rating for priority targeting (1 - 5, 1 being the highest threat)")]
            public int Threat { get; set; }
        }

    }

    public class LightManagement
    {
        [JsonProperty(PropertyName = "Automatically register light to terminal when placed (if meets other requirements)")]
        public bool AutoRegister { get; set; }
        [JsonProperty(PropertyName = "Allow lights to be set without requiring a terminal")]
        public bool NoTerminal { get; set; }
        [JsonProperty(PropertyName = "Maximum allowed searchlights per base (Permission | Amount)")]
        public Dictionary<string, int> Max { get; set; }
        [JsonProperty(PropertyName = "Maximum distance a searchlight controller can be set away from the terminal")]
        public float DistanceFromTerminal { get; set; }
        [JsonProperty(PropertyName = "Remote Settings")]
        public RemoteOptions Remote { get; set; }
        public class RemoteOptions
        {
            [JsonProperty(PropertyName = "Allow players to cycle through all linked searchlights")]
            public bool CanCycle { get; set; }
            [JsonProperty(PropertyName = "Can players toggle searchlight automation")]
            public bool ToggleEnabled { get; set; }
            [JsonProperty(PropertyName = "Can players control the searchlight remotely")]
            public bool RemoteControl { get; set; }
            [JsonProperty(PropertyName = "Display camera overlay UI")]
            public bool Overlay { get; set; }
            [JsonProperty(PropertyName = "Camera overlay image URL")]
            public string OverlayImage { get; set; }
            [JsonProperty(PropertyName = "Searchlight icon URL for RustNET menu")]
            public string RustNETIcon { get; set; }
        }

    }

    public class LightOptions
    {
        [JsonProperty(PropertyName = "Require power for light automation")]
        public bool RequirePower { get; set; }
        [JsonProperty(PropertyName = "Sunrise hour")]
        public float Sunrise { get; set; }
        [JsonProperty(PropertyName = "Sunset hour")]
        public float Sunset { get; set; }
        [JsonProperty(PropertyName = "Only automate lights at night time")]
        public bool NightOnly { get; set; }
        [JsonProperty(PropertyName = "Search mode")]
        public SearchMode Search { get; set; }
        [JsonProperty(PropertyName = "Flicker mode")]
        public FlickerMode Flicker { get; set; }
        public class SearchMode
        {
            [JsonProperty(PropertyName = "Enable search mode when no targets are visable")]
            public bool Enabled { get; set; }
            [JsonProperty(PropertyName = "Rotation speed of search mode")]
            public float Speed { get; set; }
        }

        public class FlickerMode
        {
            [JsonProperty(PropertyName = "Enable flickering lights when damaged")]
            public bool Enabled { get; set; }
            [JsonProperty(PropertyName = "Percentage of health before flickering starts")]
            public float Health { get; set; }
        }

    }

    public Oxide.Core.VersionNumber Version { get; set; }
}

public class DetectionOptions
{
    [JsonProperty(PropertyName = "Animals")]
    public DetectSettings Animals { get; set; }
    [JsonProperty(PropertyName = "Enemy players and NPCs")]
    public DetectSettings Enemies { get; set; }
    [JsonProperty(PropertyName = "Friends and owner")]
    public DetectSettings Friends { get; set; }
    [JsonProperty(PropertyName = "Cars and tanks")]
    public DetectSettings Vehicles { get; set; }
    [JsonProperty(PropertyName = "Helicopters")]
    public DetectSettings Helicopters { get; set; }
    public class DetectSettings
    {
        [JsonProperty(PropertyName = "Enable this detection type")]
        public bool Enabled { get; set; }
        [JsonProperty(PropertyName = "Range of detection")]
        public float Radius { get; set; }
        [JsonProperty(PropertyName = "Threat rating for priority targeting (1 - 5, 1 being the highest threat)")]
        public int Threat { get; set; }
    }

}

public class DetectSettings
{
    [JsonProperty(PropertyName = "Enable this detection type")]
    public bool Enabled { get; set; }
    [JsonProperty(PropertyName = "Range of detection")]
    public float Radius { get; set; }
    [JsonProperty(PropertyName = "Threat rating for priority targeting (1 - 5, 1 being the highest threat)")]
    public int Threat { get; set; }
}

public class LightManagement
{
    [JsonProperty(PropertyName = "Automatically register light to terminal when placed (if meets other requirements)")]
    public bool AutoRegister { get; set; }
    [JsonProperty(PropertyName = "Allow lights to be set without requiring a terminal")]
    public bool NoTerminal { get; set; }
    [JsonProperty(PropertyName = "Maximum allowed searchlights per base (Permission | Amount)")]
    public Dictionary<string, int> Max { get; set; }
    [JsonProperty(PropertyName = "Maximum distance a searchlight controller can be set away from the terminal")]
    public float DistanceFromTerminal { get; set; }
    [JsonProperty(PropertyName = "Remote Settings")]
    public RemoteOptions Remote { get; set; }
    public class RemoteOptions
    {
        [JsonProperty(PropertyName = "Allow players to cycle through all linked searchlights")]
        public bool CanCycle { get; set; }
        [JsonProperty(PropertyName = "Can players toggle searchlight automation")]
        public bool ToggleEnabled { get; set; }
        [JsonProperty(PropertyName = "Can players control the searchlight remotely")]
        public bool RemoteControl { get; set; }
        [JsonProperty(PropertyName = "Display camera overlay UI")]
        public bool Overlay { get; set; }
        [JsonProperty(PropertyName = "Camera overlay image URL")]
        public string OverlayImage { get; set; }
        [JsonProperty(PropertyName = "Searchlight icon URL for RustNET menu")]
        public string RustNETIcon { get; set; }
    }

}

public class RemoteOptions
{
    [JsonProperty(PropertyName = "Allow players to cycle through all linked searchlights")]
    public bool CanCycle { get; set; }
    [JsonProperty(PropertyName = "Can players toggle searchlight automation")]
    public bool ToggleEnabled { get; set; }
    [JsonProperty(PropertyName = "Can players control the searchlight remotely")]
    public bool RemoteControl { get; set; }
    [JsonProperty(PropertyName = "Display camera overlay UI")]
    public bool Overlay { get; set; }
    [JsonProperty(PropertyName = "Camera overlay image URL")]
    public string OverlayImage { get; set; }
    [JsonProperty(PropertyName = "Searchlight icon URL for RustNET menu")]
    public string RustNETIcon { get; set; }
}

public class LightOptions
{
    [JsonProperty(PropertyName = "Require power for light automation")]
    public bool RequirePower { get; set; }
    [JsonProperty(PropertyName = "Sunrise hour")]
    public float Sunrise { get; set; }
    [JsonProperty(PropertyName = "Sunset hour")]
    public float Sunset { get; set; }
    [JsonProperty(PropertyName = "Only automate lights at night time")]
    public bool NightOnly { get; set; }
    [JsonProperty(PropertyName = "Search mode")]
    public SearchMode Search { get; set; }
    [JsonProperty(PropertyName = "Flicker mode")]
    public FlickerMode Flicker { get; set; }
    public class SearchMode
    {
        [JsonProperty(PropertyName = "Enable search mode when no targets are visable")]
        public bool Enabled { get; set; }
        [JsonProperty(PropertyName = "Rotation speed of search mode")]
        public float Speed { get; set; }
    }

    public class FlickerMode
    {
        [JsonProperty(PropertyName = "Enable flickering lights when damaged")]
        public bool Enabled { get; set; }
        [JsonProperty(PropertyName = "Percentage of health before flickering starts")]
        public float Health { get; set; }
    }

}

public class SearchMode
{
    [JsonProperty(PropertyName = "Enable search mode when no targets are visable")]
    public bool Enabled { get; set; }
    [JsonProperty(PropertyName = "Rotation speed of search mode")]
    public float Speed { get; set; }
}

public class FlickerMode
{
    [JsonProperty(PropertyName = "Enable flickering lights when damaged")]
    public bool Enabled { get; set; }
    [JsonProperty(PropertyName = "Percentage of health before flickering starts")]
    public float Health { get; set; }
}

private class StoredData
{
    public LightManager.LightData[] registeredSearchlights;
}


```

---

## AutoMute

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using Oxide.Core;
using Oxide.Core.Configuration;
using UnityEngine;
using Newtonsoft.Json;
using Oxide.Core.Plugins;
using Oxide.Core.Libraries.Covalence;

Oxide.Plugins
[Info("AutoMute", "Oxide Россия - oxide-russia.ru", "2.1.11")]
 class AutoMute : CovalencePlugin
{
     Dictionary<string, int> mutes;
     DynamicConfigFile saveFile;
     void OnServerInitialized();
    private MuteList Chat;
    private class MuteList
    {
        public MuteList(string[] Say, string[] Exclusion, int MuteTime, int AuthLevel, bool AdminBlock);
        [JsonProperty("3. Запрещенные фразы")]
        public string[] Слова { get; set; }
        [JsonProperty("4. Слова-исключения")]
        public string[] Исключения { get; set; }
        [JsonProperty("2. Настройки: Время блокировки чата (сек)")]
        public int MuteTime;
        [JsonProperty("1. Настройки: Блокировать ли администраторов?")]
        public bool AdminBlock;
    }

     void Loaded();
     void Unload();
    protected override void LoadDefaultConfig();
     object OnBetterChat(Dictionary<string, object> data);
     object OnUserChat(IPlayer player, string message);
     void Mute(BasePlayer player);
    private MuteList DefaultConfig();
}

private class MuteList
{
    public MuteList(string[] Say, string[] Exclusion, int MuteTime, int AuthLevel, bool AdminBlock);
    [JsonProperty("3. Запрещенные фразы")]
    public string[] Слова { get; set; }
    [JsonProperty("4. Слова-исключения")]
    public string[] Исключения { get; set; }
    [JsonProperty("2. Настройки: Время блокировки чата (сек)")]
    public int MuteTime;
    [JsonProperty("1. Настройки: Блокировать ли администраторов?")]
    public bool AdminBlock;
}


```

---

## AutoPickup

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

## AutoReply

```csharp
using Oxide.Core;
using System;
using System.Collections.Generic;
using UnityEngine;
using System.Text.RegularExpressions;

Oxide.Plugins
[Info("AutoReply", "4seti [Lunatiq] for Rust Planet", "1.5.0", ResourceId = 908)]
public class AutoReply : RustPlugin
{
    private void Log(string message);
    private void Warn(string message);
    private void Error(string message);
     void ReplyChat(BasePlayer player, string msg);
    private Dictionary<string, Dictionary<ulong, float>> antiSpam;
    private int replyInterval;
    private int minPriveledge;
    private bool forceAdmin;
    private Dictionary<string, string> messages;
    private Dictionary<string, string> defMsg;
     Dictionary<string, Wordgroup> Wordgroups;
    private Dictionary<string, string> attributes;
    private Dictionary<string, string> defAttr;
     void Unload();
    private string ReplyName;
    private Dictionary<char, char> replaceChars;
    private Dictionary<char, char> defChar;
     void Loaded();
    protected override void LoadDefaultConfig();
    private T GetConfig(string name, T defaultValue);
    private void LoadData();
     void SaveData();
     void OnServerInitialized();
    private void LoadVariables();
    private object OnServerCommand(ConsoleSystem.Arg arg);
    private void replyToPlayer(BasePlayer player, string group);
    private string replyBuilder(string text, string playerName);
    [ChatCommand("ar")]
     void cmdAr(BasePlayer player, string cmd, string[] args);
    [ChatCommand("ar_save")]
     void cmdSave(BasePlayer player, string cmd, string[] args);
    [ChatCommand("ar_load")]
     void cmdLoad(BasePlayer player, string cmd, string[] args);
    private bool checkWord(BasePlayer player, string baseWord);
    [ChatCommand("ar_new")]
     void cmdArNew(BasePlayer player, string cmd, string[] args);
    [ChatCommand("ar_reply")]
     void cmdArReply(BasePlayer player, string cmd, string[] args);
    private List<string> checkAttributes(string text);
    [ChatCommand("ar_drop")]
     void cmdArDrop(BasePlayer player, string cmd, string[] args);
    [ChatCommand("ar_match")]
     void cmdArMatch(BasePlayer player, string cmd, string[] args);
    [ChatCommand("ar_remove")]
     void cmdArRemove(BasePlayer player, string cmd, string[] args);
    [ChatCommand("ar_force")]
     void cmdArForce(BasePlayer player, string cmd, string[] args);
    [ChatCommand("ar_word")]
     void cmdArWord(BasePlayer player, string cmd, string[] args);
    [ChatCommand("ar_list")]
     void cmdArList(BasePlayer player, string cmd, string[] args);
    [ChatCommand("ar_info")]
     void cmdArInfo(BasePlayer player, string cmd, string[] args);
    public class Wordgroup
    {
        public List<string> Words;
        public Dictionary<int, string> Replies;
        public bool FullMatch;
        public bool Drop;
        public int Hits;
        public Wordgroup(string baseword, string baseReply);
    }

    public class ReplyEntry
    {
        public string Reply;
        public List<string> ReplyAttr;
        public ReplyEntry();
        public ReplyEntry(string reply, List<string> attrs);
    }

}

public class Wordgroup
{
    public List<string> Words;
    public Dictionary<int, string> Replies;
    public bool FullMatch;
    public bool Drop;
    public int Hits;
    public Wordgroup(string baseword, string baseReply);
}

public class ReplyEntry
{
    public string Reply;
    public List<string> ReplyAttr;
    public ReplyEntry();
    public ReplyEntry(string reply, List<string> attrs);
}


```

---

## AutoResetTargets

```csharp

Oxide.Plugins
[Info("Auto Reset Targets", "Dyceman/Dan", 1.00, ResourceId = 0)]
[Description("Auto Reset Targets")]
public class AutoResetTargets : RustPlugin
{
     void OnEntityTakeDamage(BaseCombatEntity entity, HitInfo info);
}


```

---

## AutoSaver

```csharp
using Oxide.Core;

Oxide.Plugins
[Info("AutoSaver", "walkinrey", "1.0.0")]
[Description("Сохраняет сервер прежде чем он выключится :)")]
 class AutoSaver : RustPlugin
{
     void OnServerShutdown();
}


```

---

## AutoSkinTeam

```csharp
using System.Collections.Generic;
using Oxide.Core.Plugins;
using Newtonsoft.Json.Linq;
using Oxide.Core;

Oxide.Plugins
[Info("AutoSkinTeam", "Feyzi/Toolcub", "1.0.0")]
public class AutoSkinTeam : RustPlugin
{
    [PluginReference]
    private Plugin Clans;
    private HashSet<ulong> disabledChanges;
    private const string PermUse;
    private void Init();
    private void OnServerInitialized();
    private void Unload();
    private void ReskinCommand(BasePlayer player, string command, string[] args);
    private void ReskinToggleCommand(BasePlayer player, string command, string[] args);
    private void SetSkins(BasePlayer owner, JArray members);
    protected override void LoadDefaultMessages();
    private string Lang(string key, string id, object[] args);
    private void LoadData();
    private void SaveData();
}


```

---

## AutoTakeoff

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

## AutoTeleport

```csharp
using System;
using System.Collections.Generic;
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using UnityEngine;

Oxide.Plugins
[Info("Auto Teleport", "Sempai#3239", "1.1.3")]
[Description("https://topplugin.ru/resources")]
public class AutoTeleport : RustPlugin
{
    private const string permUse;
    private const string elemMain;
    private static UISettings ui { get; set; }
    private void Init();
    private void OnTeleportRequested(BasePlayer receiver, BasePlayer caller);
    private void Unload();
    private void cmdControlConsole(ConsoleSystem.Arg arg);
    private void cmdControlChat(BasePlayer player, string command, string[] args);
    private void CheckTeleport(BasePlayer receiver, BasePlayer caller);
    private static void AcceptTeleport(BasePlayer player);
    private void OpenUI(BasePlayer player, DataEntry data);
    private CuiElementContainer CreateButton(string anchorY, string textLabel, string commandArg, bool buttonActive, bool buttonDisabled);
    private static ConfigData config;
    private class ConfigData
    {
        [JsonProperty(PropertyName = "Command")]
        public string[] commands;
        [JsonProperty(PropertyName = "Friends button enabled")]
        public bool enabledFriends;
        [JsonProperty(PropertyName = "Default value for friends")]
        public bool defaultFriends;
        [JsonProperty(PropertyName = "Clan button enabled")]
        public bool enabledClan;
        [JsonProperty(PropertyName = "Default value for clan")]
        public bool defaultClan;
        [JsonProperty(PropertyName = "Team button enabled")]
        public bool enabledTeam;
        [JsonProperty(PropertyName = "Default value for team")]
        public bool defaultTeam;
        [JsonProperty(PropertyName = "UI Settings")]
        public UISettings uiSettings;
    }

    private class UISettings
    {
        [JsonProperty(PropertyName = "[Text] On")]
        public string textOn;
        [JsonProperty(PropertyName = "[Text] Off")]
        public string textOff;
        [JsonProperty(PropertyName = "[Text] Clan")]
        public string textClan;
        [JsonProperty(PropertyName = "[Text] Team")]
        public string textTeam;
        [JsonProperty(PropertyName = "[Text] Deactivated")]
        public string textDeactivated;
        [JsonProperty(PropertyName = "[Text] Friends")]
        public string textFriends;
        [JsonProperty(PropertyName = "[Text] Mail text")]
        public string textMain;
        [JsonProperty(PropertyName = "[Color] Mail panel")]
        public string mainColor;
        [JsonProperty(PropertyName = "[Color] Label text")]
        public string textColor;
        [JsonProperty(PropertyName = "[Color] On/Off Button text")]
        public string buttonTextColor;
        [JsonProperty(PropertyName = "[Color] Button On")]
        public string onColor;
        [JsonProperty(PropertyName = "[Color] Button Off")]
        public string offColor;
        [JsonProperty(PropertyName = "[Color] Button deactivate")]
        public string deactivatedColor;
        [JsonProperty(PropertyName = "[Size] Mail panel X")]
        public int mainSizeX;
        [JsonProperty(PropertyName = "[Size] Mail panel Y")]
        public int mainSizeY;
        [JsonProperty(PropertyName = "[Size] On/Off Button X")]
        public int buttonSizeX;
        [JsonProperty(PropertyName = "[Size] On/Off Button Y")]
        public int buttonSizeY;
        [JsonProperty(PropertyName = "[Size] Close button")]
        public int sizeClose;
        [JsonProperty(PropertyName = "[Font] Mail text")]
        public int mainFont;
        [JsonProperty(PropertyName = "[Font] Label text")]
        public int textFont;
        [JsonProperty(PropertyName = "[Font] On/Off Button text")]
        public int buttonFont;
        [JsonProperty(PropertyName = "[Font] Close button text")]
        public int fontClose;
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private const string filename;
    private bool corruptedData;
    private Timer saveTimer;
    private class DataEntry
    {
        public bool autoClans;
        public bool autoTeam;
        public bool autoFriends;
    }

    private static PluginData Data;
    private class PluginData
    {
        [JsonIgnore]
        public Dictionary<string, DataEntry> cache;
        public Dictionary<string, DataEntry> info;
        public DataEntry Get(object param);
    }

    private void LoadData(string keyName);
    private void SaveData(string keyName);
    [PluginReference]
    private Plugin Clans;
    private string GetPlayerClan(BasePlayer player);
    private bool InSameClan(BasePlayer playeܡ, BasePlayer playeܢ);
    [PluginReference]
    private Plugin Friends;
    private Plugin RustIOFriendListAPI;
    private bool IsFriends(BasePlayer playeܡ, BasePlayer playeܢ);
    private bool IsFriends(ulong iف, ulong iق);
    private static bool InSameTeam(BasePlayer playeܡ, BasePlayer playeܢ);
}

private class ConfigData
{
    [JsonProperty(PropertyName = "Command")]
    public string[] commands;
    [JsonProperty(PropertyName = "Friends button enabled")]
    public bool enabledFriends;
    [JsonProperty(PropertyName = "Default value for friends")]
    public bool defaultFriends;
    [JsonProperty(PropertyName = "Clan button enabled")]
    public bool enabledClan;
    [JsonProperty(PropertyName = "Default value for clan")]
    public bool defaultClan;
    [JsonProperty(PropertyName = "Team button enabled")]
    public bool enabledTeam;
    [JsonProperty(PropertyName = "Default value for team")]
    public bool defaultTeam;
    [JsonProperty(PropertyName = "UI Settings")]
    public UISettings uiSettings;
}

private class UISettings
{
    [JsonProperty(PropertyName = "[Text] On")]
    public string textOn;
    [JsonProperty(PropertyName = "[Text] Off")]
    public string textOff;
    [JsonProperty(PropertyName = "[Text] Clan")]
    public string textClan;
    [JsonProperty(PropertyName = "[Text] Team")]
    public string textTeam;
    [JsonProperty(PropertyName = "[Text] Deactivated")]
    public string textDeactivated;
    [JsonProperty(PropertyName = "[Text] Friends")]
    public string textFriends;
    [JsonProperty(PropertyName = "[Text] Mail text")]
    public string textMain;
    [JsonProperty(PropertyName = "[Color] Mail panel")]
    public string mainColor;
    [JsonProperty(PropertyName = "[Color] Label text")]
    public string textColor;
    [JsonProperty(PropertyName = "[Color] On/Off Button text")]
    public string buttonTextColor;
    [JsonProperty(PropertyName = "[Color] Button On")]
    public string onColor;
    [JsonProperty(PropertyName = "[Color] Button Off")]
    public string offColor;
    [JsonProperty(PropertyName = "[Color] Button deactivate")]
    public string deactivatedColor;
    [JsonProperty(PropertyName = "[Size] Mail panel X")]
    public int mainSizeX;
    [JsonProperty(PropertyName = "[Size] Mail panel Y")]
    public int mainSizeY;
    [JsonProperty(PropertyName = "[Size] On/Off Button X")]
    public int buttonSizeX;
    [JsonProperty(PropertyName = "[Size] On/Off Button Y")]
    public int buttonSizeY;
    [JsonProperty(PropertyName = "[Size] Close button")]
    public int sizeClose;
    [JsonProperty(PropertyName = "[Font] Mail text")]
    public int mainFont;
    [JsonProperty(PropertyName = "[Font] Label text")]
    public int textFont;
    [JsonProperty(PropertyName = "[Font] On/Off Button text")]
    public int buttonFont;
    [JsonProperty(PropertyName = "[Font] Close button text")]
    public int fontClose;
}

private class DataEntry
{
    public bool autoClans;
    public bool autoTeam;
    public bool autoFriends;
}

private class PluginData
{
    [JsonIgnore]
    public Dictionary<string, DataEntry> cache;
    public Dictionary<string, DataEntry> info;
    public DataEntry Get(object param);
}


```

---

## Backpack

```csharp
using System;
using System.Collections;
using System.Collections.Generic;
using System.Globalization;
using System.Linq;
using Newtonsoft.Json;
using Newtonsoft.Json.Serialization;
using Oxide.Core;
using Oxide.Core.Configuration;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using UnityEngine;

Oxide.Plugins
[Info("Backpack", "TopPlugin.ru", "1.0.41")]
public class Backpack : RustPlugin
{
    [PluginReference]
    private Plugin ImageLibrary;
    public Dictionary<ulong, BackpackStorage> opensBackpack;
    public static Backpack ins;
    public string Layer;
    public string LayerBlur;
    [ChatCommand("updatecraft")]
     void chatCmdUpdateCraft(BasePlayer player, string command, string[] args);
    [ConsoleCommand("backpack.give")]
     void chatCmdBackpackGive(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_Backpack")]
     void consoleCommandBackpackUI(ConsoleSystem.Arg arg);
    [ChatCommand("backpack.spawn")]
     void chatCmdBackpackSpawn(BasePlayer player, string command, string[] args);
    private void chatCmdBackpackOpen(BasePlayer player, string command, string[] args);
    private void chatCmdBackpack(BasePlayer player, string command, string[] args);
    private void consoleCmdBackpack(ConsoleSystem.Arg arg);
    private bool? CanWearItem(PlayerInventory inventory, Item item);
     void OnServerInitialized();
     void UpdateData();
     object CanAcceptItem(ItemContainer container, Item item);
     void Unload();
    public Item FindItem(BasePlayer player, int itemID, ulong skinID, int amount);
    public bool HaveItem(BasePlayer player, int itemID, ulong skinID, int amount);
    private IEnumerator DownloadImages();
    private static string HexToRGB(string hex);
     List<SavedItem> SaveItems(List<Item> items);
     bool BackpackHide(uint itemID, ulong playerId);
     void BackpackOpen(BasePlayer player);
     List<Item> RestoreItems(List<SavedItem> sItems);
     Item BuildItem(SavedItem sItem);
     Item BuildWeapon(SavedItem sItem);
     SavedItem SaveItem(Item item);
    public class BackpackStorage : MonoBehaviour
    {
        public StorageContainer container;
        public Item backpack;
        public BasePlayer player;
        public void Initialization(StorageContainer container, Item backpack, BasePlayer player);
        public List<Item> GetItems { get; set; }
        public static StorageContainer CreateContainer(BasePlayer player);
        private void PlayerStoppedLooting(BasePlayer player);
        public void StartLoot();
        public static BackpackStorage Spawn(BasePlayer player);
        public void Close();
        public void Push(List<Item> items);
        public void BlockBackpackSlots(bool state);
    }

     class StoredData
    {
        public Dictionary<uint, List<SavedItem>> backpacks;
    }

    public class SavedItem
    {
        public string shortname;
        public int itemid;
        public float condition;
        public float maxcondition;
        public int amount;
        public int ammoamount;
        public string ammotype;
        public int flamefuel;
        public ulong skinid;
        public string name;
        public bool weapon;
        public float busyTime;
        public bool OnFire;
        public List<SavedItem> mods;
    }

     void SaveData();
     void LoadData();
     StoredData storedData;
    private DynamicConfigFile BackpackData;
    public void CheckConfig();
    protected override void SaveConfig();
    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    public Configuration _config;
    public class Configuration
    {
        [JsonProperty("Консольная команда для открытия рюкзака")]
        public string consoleCommand;
        [JsonProperty("Чат команда для открытия крафта рюкзака")]
        public string chatCommand;
        [JsonProperty("Чат команда для открытия самого рюкзака")]
        public string chatCommandOpen;
        [JsonProperty("Количество слотов в рюкзаке")]
        public int backpackSize;
        [JsonProperty("Название для рюкзака")]
        public string displayName;
        [JsonProperty("СкинИД рюкзака")]
        public ulong skinIdBackpack;
        [JsonProperty("Расположение Data файла")]
        public string fileName;
        [JsonProperty("Необходимые предметы для крафта рюкзака")]
        public List<ItemInfo> items;
    }

    public class ItemInfo
    {
        [JsonProperty("Шортнейм предмета")]
        public string shortname;
        [JsonProperty("Количество предмета")]
        public int amount;
        [JsonProperty("СкинИД предмета")]
        public ulong skinID;
        [JsonProperty("АйтемИД предмета")]
        public int itemID;
    }

}

public class BackpackStorage : MonoBehaviour
{
    public StorageContainer container;
    public Item backpack;
    public BasePlayer player;
    public void Initialization(StorageContainer container, Item backpack, BasePlayer player);
    public List<Item> GetItems { get; set; }
    public static StorageContainer CreateContainer(BasePlayer player);
    private void PlayerStoppedLooting(BasePlayer player);
    public void StartLoot();
    public static BackpackStorage Spawn(BasePlayer player);
    public void Close();
    public void Push(List<Item> items);
    public void BlockBackpackSlots(bool state);
}

 class StoredData
{
    public Dictionary<uint, List<SavedItem>> backpacks;
}

public class SavedItem
{
    public string shortname;
    public int itemid;
    public float condition;
    public float maxcondition;
    public int amount;
    public int ammoamount;
    public string ammotype;
    public int flamefuel;
    public ulong skinid;
    public string name;
    public bool weapon;
    public float busyTime;
    public bool OnFire;
    public List<SavedItem> mods;
}

public class Configuration
{
    [JsonProperty("Консольная команда для открытия рюкзака")]
    public string consoleCommand;
    [JsonProperty("Чат команда для открытия крафта рюкзака")]
    public string chatCommand;
    [JsonProperty("Чат команда для открытия самого рюкзака")]
    public string chatCommandOpen;
    [JsonProperty("Количество слотов в рюкзаке")]
    public int backpackSize;
    [JsonProperty("Название для рюкзака")]
    public string displayName;
    [JsonProperty("СкинИД рюкзака")]
    public ulong skinIdBackpack;
    [JsonProperty("Расположение Data файла")]
    public string fileName;
    [JsonProperty("Необходимые предметы для крафта рюкзака")]
    public List<ItemInfo> items;
}

public class ItemInfo
{
    [JsonProperty("Шортнейм предмета")]
    public string shortname;
    [JsonProperty("Количество предмета")]
    public int amount;
    [JsonProperty("СкинИД предмета")]
    public ulong skinID;
    [JsonProperty("АйтемИД предмета")]
    public int itemID;
}


```

---

## BackupExt

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Reflection;
using Oxide.Core;
using Oxide.Core.Plugins;
using UnityEngine;
using ProtoBuf;
using Network;

Oxide.Plugins
[Info("BackupExt", "Fujikura", "0.1.0")]
 class BackupExt : RustPlugin
{
     bool Changed;
     bool _backup;
     bool _startup;
     string [] backupFolders;
     bool backupOnStartup;
     int numberOfBackups;
     bool backupBroadcast;
     int backupDelay;
     bool useBroadcastDelay;
     string prefix;
     string prefixColor;
     bool useTimer;
     int timerInterval;
     object GetConfig(string menu, string datavalue, object defaultValue);
     void LoadVariables();
     void LoadDefaultMessages();
    protected override void LoadDefaultConfig();
     void Loaded();
     void OnTerrainInitialized();
     void OnServerInitialized();
     void BackupCreate(bool manual);
    [ConsoleCommand("extbackup")]
     void ccmdExtBackup(ConsoleSystem.Arg arg);
     void BackupRun(ConsoleSystem.Arg arg);
     string [] BackupFolders();
     void BroadcastChat(string msg);
}


```

---

## BanSystem1.0.8

```csharp
using Oxide.Core.Configuration;
using Oxide.Core;
using System;
using System.Collections.Generic;
using System.Text.RegularExpressions;
using System.Linq;
using UnityEngine;
using MySql.Data.MySqlClient;
using Newtonsoft.Json;
using System.Text;
using Oxide.Core.Database;
using System.Net;
using static Oxide.Plugins.BanSystem;

Oxide.Plugins
[Info("BanSystem", "MaltrzD", "1.0.8")]
 class BanSystem : RustPlugin
{
    public static DateTime unixEpoch { get; set; }
    private static BanSystem _ins;
     Effect banEffect;
    private Connection _mySqlConnection;
    private readonly Core.MySql.Libraries.MySql _mySql;
    private const string PERM_BAN;
    private const string PERM_KICK;
    private const string PERM_UNBAN;
    private const string DISCORD_BAN_MESSAGE;
    private const string DISCORD_UNBAN_MESSAGE;
    private void Loaded();
    private void Unload();
    private void CanUserLogin(string name, string id, string ipAddress);
    private void OnPlayerConnected(BasePlayer player);
    private void CheckUser(ulong id, string ipAddress);
    private void Save();
    [ChatCommand("ban")]
    private void BanPlayer_ChatCommand(BasePlayer player, string cmd, string[] args);
    [ChatCommand("unban")]
    private void UnbanPlayer_ChatCommand(BasePlayer player, string cmd, string[] args);
    [ChatCommand("kick")]
    private void KickPlayer_ChatCommand(BasePlayer player, string cmd, string[] args);
    [ConsoleCommand("banp")]
    private void BanPlayer_ConsoleCommand(ConsoleSystem.Arg arg);
    [ConsoleCommand("unbanp")]
    private void Unban_ConsoleCommand(ConsoleSystem.Arg arg);
    [ConsoleCommand("checkban")]
    private void CheckBanBySteamId_ConsoleCommand(ConsoleSystem.Arg arg);
    [ConsoleCommand("checkbanip")]
    private void CheckBansIp_ConsoleCommand(ConsoleSystem.Arg arg);
    [ConsoleCommand("kick")]
    private void KickPlayer_ConsoleCommand(ConsoleSystem.Arg arg);
    private string GetFormattedTime(string time);
    private DateTime ConvertUnixTimeToDateTime(long unixTime);
    private long GetTimeFromString(DateTime currentTime, string timeString);
    private bool IsTime(string input);
    private BasePlayer FindPlayer(string filter);
    private BasePlayer FindPlayer(ulong steamId);
    public static class PermissionService
    {
        public static void RegisterPermissions(List<string> perms);
        public static void RegisterPermission(string perm);
        public static bool HasPermission(string uid, string perm);
    }

    private void Dlog(string discordWebHook, string message);
    private string GetMessage(string key, object[] args);
    public bool IsSteamID(string input);
    private DynamicConfigFile PlayerData_File;
    private Dictionary<ulong, PlayerData> PlayerDatas;
    private ConfigData _config;
    private PlayerData? GetPlayerData(ulong userId);
    private string? GetPlayerNameById(ulong userId);
    private string? GetPlayerIpById(ulong userId);
    private void UpdatePlayerData(BasePlayer player);
    private void LoadPlayerNickNames();
    private void SavePlayerNickNames();
    public class ConfigData
    {
        [JsonProperty("Основные настройки")]
        public MainSettings Main;
        [JsonProperty("Настройка MySql")]
        public DataBaseSettings DataBase;
        [JsonProperty("Настройка дискорд")]
        public DiscordSettings Discord;
        [JsonProperty("Настройки обхода блокировки")]
        public BanEvadeSetting BanEvade;
        [JsonProperty("Настройка звука при блокировке")]
        public SoundSetting Sound;
        [JsonProperty("Сообщения")]
        public Language Lang;
        public class MainSettings
        {
            [JsonProperty("Дефолтная причина блокировки (если не будет указана)")]
            public string defaultBanReason;
            [JsonProperty("Сообщение при бане в чат [true/да | false/нет]")]
            public bool alertOnBan;
            [JsonProperty("Сообщение при кике в чат [true/да | false/нет]")]
            public bool alertOnKick;
            [JsonProperty("Сообщение при разбане в чат [true/да | false/нет]")]
            public bool alertOnUnban;
        }

        public class DiscordSettings
        {
            [JsonProperty("Вебхук банов")]
            public string discordWebHook_Bans;
            [JsonProperty("Вебхук разбанов")]
            public string discordWebHook_UnBans;
        }

        public class DataBaseSettings
        {
            [JsonProperty("Хост")]
            public string server;
            [JsonProperty("Порт")]
            public string port;
            [JsonProperty("Пользователь")]
            public string username;
            [JsonProperty("Пароль")]
            public string password;
            [JsonProperty("Название бд")]
            public string database;
        }

        public class BanEvadeSetting
        {
            [JsonProperty("Включить блокировку обхода бана")]
            public bool IsEnable;
            [JsonProperty("Время блокировки")]
            public string BanTime;
            [JsonProperty("Причина блокировки")]
            public string Reason;
        }

        public class Language
        {
            [JsonProperty("Сообщение при бане")]
            public string BanMessage;
            [JsonProperty("Сообщение при кике")]
            public string KickMessage;
            [JsonProperty("Сообщение при разбане")]
            public string UnbanMessage;
            [JsonProperty("Сообщение при заходе на сервер")]
            public string BannedMessage;
            [JsonProperty("Сообщение игроку при бане")]
            public string KickbannedMessage;
            public string ReplaceArgs(string msg, object[] args);
        }

        public class SoundSetting
        {
            [JsonProperty("Проигрывать звук всем игрокам при блокировке")]
            public bool Play;
            [JsonProperty("Путь до префаба звука")]
            public string Prefab;
        }

    }

    protected override void LoadDefaultConfig();
     void SaveConfig(object config);
     void ReadConfig();
    private Ban BanPlayer(ulong userId, BasePlayer actorPlayer, string actor, string reason, long time, string playerIp, string formattedTime);
    private string KickPlayer(string filter, string reason);
    private void GetBanBySteamID(ulong steamID, Action<Ban> callback);
    private void GetBansByIP(string ip, Action<List<Ban>> callback);
    private void GetBanByIP(string ip, Action<Ban> callback);
    private void UnbanBySteamID(ulong steamID, Action<bool> callback, string actor, bool notify);
    public class Ban
    {
        public ulong SteamID;
        public string Name;
        public string Ip;
        public string Actor;
        public string Reason;
        public DateTime BanTime;
        public long ExpiredTime;
        public Ban(ulong steamId, string name, string ip, string actor, string reason, DateTime banTime, long expiredTime);
    }

    public class PlayerData
    {
        public string Name;
        public string Ip;
    }

}

public static class PermissionService
{
    public static void RegisterPermissions(List<string> perms);
    public static void RegisterPermission(string perm);
    public static bool HasPermission(string uid, string perm);
}

public class ConfigData
{
    [JsonProperty("Основные настройки")]
    public MainSettings Main;
    [JsonProperty("Настройка MySql")]
    public DataBaseSettings DataBase;
    [JsonProperty("Настройка дискорд")]
    public DiscordSettings Discord;
    [JsonProperty("Настройки обхода блокировки")]
    public BanEvadeSetting BanEvade;
    [JsonProperty("Настройка звука при блокировке")]
    public SoundSetting Sound;
    [JsonProperty("Сообщения")]
    public Language Lang;
    public class MainSettings
    {
        [JsonProperty("Дефолтная причина блокировки (если не будет указана)")]
        public string defaultBanReason;
        [JsonProperty("Сообщение при бане в чат [true/да | false/нет]")]
        public bool alertOnBan;
        [JsonProperty("Сообщение при кике в чат [true/да | false/нет]")]
        public bool alertOnKick;
        [JsonProperty("Сообщение при разбане в чат [true/да | false/нет]")]
        public bool alertOnUnban;
    }

    public class DiscordSettings
    {
        [JsonProperty("Вебхук банов")]
        public string discordWebHook_Bans;
        [JsonProperty("Вебхук разбанов")]
        public string discordWebHook_UnBans;
    }

    public class DataBaseSettings
    {
        [JsonProperty("Хост")]
        public string server;
        [JsonProperty("Порт")]
        public string port;
        [JsonProperty("Пользователь")]
        public string username;
        [JsonProperty("Пароль")]
        public string password;
        [JsonProperty("Название бд")]
        public string database;
    }

    public class BanEvadeSetting
    {
        [JsonProperty("Включить блокировку обхода бана")]
        public bool IsEnable;
        [JsonProperty("Время блокировки")]
        public string BanTime;
        [JsonProperty("Причина блокировки")]
        public string Reason;
    }

    public class Language
    {
        [JsonProperty("Сообщение при бане")]
        public string BanMessage;
        [JsonProperty("Сообщение при кике")]
        public string KickMessage;
        [JsonProperty("Сообщение при разбане")]
        public string UnbanMessage;
        [JsonProperty("Сообщение при заходе на сервер")]
        public string BannedMessage;
        [JsonProperty("Сообщение игроку при бане")]
        public string KickbannedMessage;
        public string ReplaceArgs(string msg, object[] args);
    }

    public class SoundSetting
    {
        [JsonProperty("Проигрывать звук всем игрокам при блокировке")]
        public bool Play;
        [JsonProperty("Путь до префаба звука")]
        public string Prefab;
    }

}

public class MainSettings
{
    [JsonProperty("Дефолтная причина блокировки (если не будет указана)")]
    public string defaultBanReason;
    [JsonProperty("Сообщение при бане в чат [true/да | false/нет]")]
    public bool alertOnBan;
    [JsonProperty("Сообщение при кике в чат [true/да | false/нет]")]
    public bool alertOnKick;
    [JsonProperty("Сообщение при разбане в чат [true/да | false/нет]")]
    public bool alertOnUnban;
}

public class DiscordSettings
{
    [JsonProperty("Вебхук банов")]
    public string discordWebHook_Bans;
    [JsonProperty("Вебхук разбанов")]
    public string discordWebHook_UnBans;
}

public class DataBaseSettings
{
    [JsonProperty("Хост")]
    public string server;
    [JsonProperty("Порт")]
    public string port;
    [JsonProperty("Пользователь")]
    public string username;
    [JsonProperty("Пароль")]
    public string password;
    [JsonProperty("Название бд")]
    public string database;
}

public class BanEvadeSetting
{
    [JsonProperty("Включить блокировку обхода бана")]
    public bool IsEnable;
    [JsonProperty("Время блокировки")]
    public string BanTime;
    [JsonProperty("Причина блокировки")]
    public string Reason;
}

public class Language
{
    [JsonProperty("Сообщение при бане")]
    public string BanMessage;
    [JsonProperty("Сообщение при кике")]
    public string KickMessage;
    [JsonProperty("Сообщение при разбане")]
    public string UnbanMessage;
    [JsonProperty("Сообщение при заходе на сервер")]
    public string BannedMessage;
    [JsonProperty("Сообщение игроку при бане")]
    public string KickbannedMessage;
    public string ReplaceArgs(string msg, object[] args);
}

public class SoundSetting
{
    [JsonProperty("Проигрывать звук всем игрокам при блокировке")]
    public bool Play;
    [JsonProperty("Путь до префаба звука")]
    public string Prefab;
}

public class Ban
{
    public ulong SteamID;
    public string Name;
    public string Ip;
    public string Actor;
    public string Reason;
    public DateTime BanTime;
    public long ExpiredTime;
    public Ban(ulong steamId, string name, string ip, string actor, string reason, DateTime banTime, long expiredTime);
}

public class PlayerData
{
    public string Name;
    public string Ip;
}


```

---

## BaseRepair

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

Oxide.Plugins
[Info("Base Repair", "MJSU", "1.0.24")]
[Description("Allows player to repair their entire base")]
internal class BaseRepair : RustPlugin
{
    [PluginReference]
    private Plugin NoEscape;
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
    private IEnumerator DoBuildingRepair(BasePlayer player, BuildingManager.Building building, PlayerRepairStats stats);
    private void DoRepair(BasePlayer player, BaseCombatEntity entity, PlayerRepairStats stats, bool noCost);
    public bool CanAffordRepair(BasePlayer player, List<ItemAmount> amounts);
    public void GetEntityRepairCost(BaseCombatEntity entity, List<ItemAmount> repairAmounts, float missingHealthFraction);
    public bool HasRepairCost(List<ItemAmount> amounts);
    public void FreeItemAmounts(List<ItemAmount> amounts);
    public void SendMissingItemAmounts(BasePlayer player, List<ItemAmount> itemAmounts);
    public void SubscribeAll();
    public void UnsubscribeAll();
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

## Battlefield

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

## Battlepass

```csharp
using System;
using System.Collections.Generic;
using System.Globalization;
using System.Linq;
using Newtonsoft.Json;
using Newtonsoft.Json.Converters;
using Oxide.Core;
using Oxide.Core.Libraries.Covalence;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using UnityEngine;
using WebSocketSharp;
using Random = UnityEngine.Random;

Oxide.Plugins
[Info("Battlepass", "https://topplugin.ru/", "1.19.0⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠")]
public class Battlepass : RustPlugin
{
    [PluginReference]
    private Plugin ImageLibrary;
    private const string Layer;
    private const string ModalLayer;
    private const string OrangeColor;
    private const string ProgressBarColor;
    private static Battlepass _instance;
    private bool needUpdate;
    private readonly Dictionary<BasePlayer, List<ItemCase>> openedCaseItems;
    private readonly List<GeneralMission> _generalMissions;
    private List<int> _privateMissions;
    private readonly List<BasePlayer> _missionPlayers;
    private List<uint> LootedItems;
    private DateTime nextTime;
    private class GeneralMission
    {
        public int ID;
        public MissionConfig Mission;
    }

    private static Configuration _config;
    private class Configuration
    {
        [JsonProperty(PropertyName = "Команда | Command")]
        public string Command;
        [JsonProperty(PropertyName = "Право | Permission")]
        public string Permission;
        [JsonProperty(PropertyName = "Фон | Background")]
        public string Background;
        [JsonProperty(PropertyName = "Логотип | Logo")]
        public string Logo;
        [JsonProperty(PropertyName =
				"Сбрасывать квест после его завершения? | Reset the quest after completing it?")]
        public bool ResetQuestAfterComplete;
        [JsonProperty(PropertyName = "Валюта 1 | Currency 1")]
        public FirstCurrencyClass FirstCurrency;
        [JsonProperty(PropertyName = "Использовать 2 валюту? | Use 2nd currency?")]
        public bool useSecondCur;
        [JsonProperty(PropertyName = "Валюта 2 | Currency 2")]
        public SecondCurrencyClass SecondCurrency;
        [JsonProperty(PropertyName = "Изображение сезона | Season image")]
        public string Battlepass;
        [JsonProperty(PropertyName = "Изображение кейсов | Case Image")]
        public string CaseImg;
        [JsonProperty(PropertyName = "Изображение инвентаря | Inventory Image")]
        public string InventoryImg;
        [JsonProperty(PropertyName = "Изображение топ награды | Image Top Awards")]
        public string AdvanceAward;
        [JsonProperty(PropertyName = "Фон для кейсов | Background for cases")]
        public string CaseBG;
        [JsonProperty(PropertyName = "Кейсы | Cases", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<CaseClass> Cases;
        [JsonProperty(PropertyName = "Количество общих миссий в день | Total missions per day")]
        public int MissionsCount;
        [JsonProperty(PropertyName =
				"Раз во сколько часов обновлять миссии? | How many hours are missions updated?")]
        public int MissionHours;
        [JsonProperty(PropertyName = "Настройка общих миссий | Settings shared missions",
				ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public Dictionary<int, MissionConfig> Missions;
        [JsonProperty(PropertyName = "Настройка вызова дня | Settings challenge of the day",
				ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public Dictionary<int, MissionConfig> PrivateMissions;
        [JsonProperty(PropertyName = "Включить логирование в консоль? | Enable logging to the console?")]
        public bool LogToConsole;
        [JsonProperty(PropertyName = "Включить логирование в файл? | Enable logging to the file?")]
        public bool LogToFile;
    }

    private class CurrencyClass
    {
        [JsonProperty(PropertyName = "Картинка | Image")]
        public string Image;
        [JsonProperty(PropertyName = "Использовать встроенную систему? | Use embedded system?")]
        public bool useDefaultCur;
        [JsonProperty(PropertyName = "Название плагина | Plugin name")]
        public string Plug;
        [JsonProperty(PropertyName = "Функция пополнения баланса | Balance add hook")]
        public string AddHook;
        [JsonProperty(PropertyName = "Функция снятия баланса | Balance remove hook")]
        public string RemoveHook;
        [JsonProperty(PropertyName = "Функция показа баланса | Balance show hook")]
        public string BalanceHook;
    }

    private class FirstCurrencyClass : CurrencyClass
    {
        [JsonProperty(PropertyName = "Rates for permissions",
				ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public Dictionary<string, float> Rates;
        public double ShowBalance(BasePlayer player);
        public void AddBalance(BasePlayer player, int amount);
        public bool RemoveBalance(BasePlayer player, int amount);
    }

    private class SecondCurrencyClass : CurrencyClass
    {
        [JsonProperty(PropertyName = "Право | Permission")]
        public string Permission;
        public double ShowBalance(BasePlayer player);
        public void AddBalance(BasePlayer player, int amount);
        public bool RemoveBalance(BasePlayer player, int amount);
    }

    private class MissionConfig
    {
        [JsonProperty(PropertyName = "Описание миссии | Mission description")]
        public string Description;
        [JsonProperty(PropertyName = "Тип миссии | Mission type")]
        [JsonConverter(typeof(StringEnumConverter))]
        public MissionType Type;
        [JsonProperty(PropertyName = "Шортнейм/префаб | Shortname/prefab")]
        public string Shortname;
        [JsonProperty(PropertyName = "Скин (0 - любой предмет) | Skin (0 - any item)")]
        public ulong Skin;
        [JsonProperty(PropertyName =
				"Уровень апгрейда (для миссий 'Улучшить') | Upgrade Level (for 'Улучшить' missions)")]
        public int Grade;
        [JsonProperty(PropertyName = "Количество | Amount")]
        public int Amount;
        [JsonProperty(PropertyName = "Количество основной награды | Amounr of main reward")]
        public int MainAward;
        [JsonProperty(PropertyName = "Давать доп.награду? | Give extra reward?")]
        public bool UseAdvanceAward;
        [JsonProperty(PropertyName = "Настройка доп.награды | Settings extra reward")]
        public AdvanceAward AdvanceAward;
        [JsonProperty(PropertyName = "(Вызов) Давать вторую валюту | (Challenge) Give second currency?")]
        public bool UseSecondAward;
        [JsonProperty(PropertyName = "(Вызов) Количество основной валюты | (Challenge) Amount of second currency")]
        public int SecondAward;
        private void GiveMainAward(BasePlayer player);
        public void GiveAwards(BasePlayer player);
        public void GivePrivateAwards(BasePlayer player);
    }

    private class AdvanceAward
    {
        [JsonProperty(PropertyName =
				"Картинка (если пусто - берётся иконка по shortname) | Image (if empty - the icon is taken by shortname)")]
        public string Image;
        [JsonProperty(PropertyName =
				"Отображаемое имя (если пусто - стандартное) | Display Name (if empty - standard)")]
        public string DisplayName;
        [JsonProperty(PropertyName = "Отображаемое имя (для интерфейса) | Display Name (for interface)")]
        public string Title;
        [JsonProperty(PropertyName = "Shortname")]
        public string Shortname;
        [JsonProperty(PropertyName = "Скин | Skin")]
        public ulong Skin;
        [JsonProperty(PropertyName = "Количество (для предмета) | Amount (for item)")]
        public int Amount;
        public void GiveItem(BasePlayer player);
    }

    private class CaseClass
    {
        [JsonProperty(PropertyName = "Отображаемое имя кейса | Case Display Name")]
        public string DisplayName;
        [JsonProperty(PropertyName = "Картинка | Image")]
        public string Image;
        [JsonProperty(PropertyName = "Право | Permission")]
        public string Permission;
        [JsonProperty(PropertyName = "Стоимость в валюте 1 | Cost in currency 1")]
        public int FCost;
        [JsonProperty(PropertyName = "Стоимость в валюте 2 | Cost in currency 2")]
        public int PCost;
        [JsonProperty(PropertyName = "Предметы | Items", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<ItemCase> Items;
    }

    private class ItemCase
    {
        [JsonProperty(PropertyName =
				"Отображаемое имя (для вывода в интерфейсе) | Display Name (for display in the interface)")]
        public string Title;
        [JsonProperty(PropertyName = "Шанс | Chance")]
        public float Chance;
        [JsonProperty(PropertyName = "Тип предмета | Item type")]
        [JsonConverter(typeof(StringEnumConverter))]
        public ItemType Type;
        [JsonProperty(PropertyName =
				"Картинка (если пусто - берётся икнока по shortname) | Image (if empty - the icon is taken by shortname) ")]
        public string Image;
        [JsonProperty(PropertyName =
				"Отображаемое имя (для предмета) (если пусто - стандартное) | Display name (for the item) (if empty - standard)")]
        public string DisplayName;
        [JsonProperty(PropertyName = "Shortname")]
        public string Shortname;
        [JsonProperty(PropertyName = "Скин | Skin")]
        public ulong Skin;
        [JsonProperty(PropertyName = "Количество (для предмета) | Amount (for item)")]
        public int Amount;
        [JsonProperty(PropertyName = "Команда | Command")]
        public string Command;
        [JsonProperty(PropertyName = "Плагин | Plugin")]
        public PluginAward PluginAward;
        private void ToItem(BasePlayer player);
        private void ToCommand(BasePlayer player);
        public void GetItem(BasePlayer player);
    }

    private class PluginAward
    {
        [JsonProperty("Название функции для вызова | Hook to call")]
        public string hook;
        [JsonProperty("Название плагина | Plugin name")]
        public string plugin;
        [JsonProperty("Количество | Amount")]
        public int amount;
        [JsonProperty("(GameStores) ИД магазина в сервисе")]
        public string ShopID;
        [JsonProperty("(GameStores) ИД сервера в сервисе")]
        public string ServerID;
        [JsonProperty("(GameStores) Секретный ключ")]
        public string SecretKey;
        public void ToPluginAward(BasePlayer player);
    }

    protected override void LoadConfig();
    protected override void SaveConfig();
    protected override void LoadDefaultConfig();
    private static PluginData _data;
    private void SaveData();
    private void LoadData();
    private class PluginData
    {
        [JsonProperty(PropertyName = "Date of last mission update")]
        public DateTime MissionsDate;
        [JsonProperty(PropertyName = "Missions", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<int> Missions;
        [JsonProperty(PropertyName = "Players Data", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public Dictionary<ulong, PlayerData> PlayerDatas;
    }

    private class PlayerData
    {
        [JsonProperty(PropertyName = "MissionDate")]
        public DateTime MissionDate;
        [JsonProperty(PropertyName = "Mission ID")]
        public int MissionId;
        [JsonProperty(PropertyName = "Mission Progress")]
        public int MissionProgress;
        [JsonProperty(PropertyName = "Currency 1")]
        public int FirstCurrency;
        [JsonProperty(PropertyName = "Currency 2")]
        public int SecondCurrency;
        [JsonProperty(PropertyName = "General Mission Progress",
				ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public Dictionary<int, int> Missions;
        [JsonProperty(PropertyName = "Items", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public Dictionary<int, ItemCase> Items;
    }

    private void Init();
    private void OnServerInitialized();
    private void OnServerSave();
    private void Unload();
    private void OnNewSave();
    private void OnPlayerConnected(BasePlayer player);
    private void OnPlayerDisconnected(BasePlayer player, string reason);
    private void OnCollectiblePickup(Item item, BasePlayer player);
    private void OnDispenserGather(ResourceDispenser dispenser, BasePlayer player, Item item);
    private void OnDispenserBonus(ResourceDispenser dispenser, BasePlayer player, Item item);
    private void OnGrowableGathered(GrowableEntity growable, Item item, BasePlayer player);
    private void OnEntityDeath(BaseEntity entity, HitInfo info);
    private readonly Dictionary<uint, Dictionary<ulong, int>> HeliAttackers;
    private void OnEntityTakeDamage(BaseHelicopter heli, HitInfo info);
    private ulong GetLastAttacker(uint id);
    private void OnItemCraftFinished(ItemCraftTask task, Item item);
    private object CanMoveItem(Item item, PlayerInventory playerLoot, uint id2, int iTargetPos, int num);
    private void OnItemAddedToContainer(ItemContainer container, Item item);
    private void OnItemPickup(Item item, BasePlayer player);
    private void AddedToContainer(BasePlayer player, Item item);
    private bool CanMoveItemsFrom(PlayerInventory playerLoot, BaseEntity entity, Item item);
    private void OnEntitySpawned(BaseEntity entity);
    private void OnPayForUpgrade(BasePlayer player, BuildingBlock block, ConstructionGrade gradeTarget);
    private void OnBuildingUpgrade(BuildingBlock block, BuildingGrade.Enum grade, BasePlayer player);
    private void CmdChatOpenBattlepass(BasePlayer player, string cmd, string[] args);
    [ConsoleCommand("battlepass.wipedata")]
    private void CmdConsoleWipeData(ConsoleSystem.Arg arg);
    private void CmdAddBalance(IPlayer player, string cmd, string[] args);
    [ConsoleCommand("UI_Battlepass")]
    private void CmdConsoleBattlepass(ConsoleSystem.Arg arg);
    private void MainUI(BasePlayer player, bool isFirst);
    private void CasesUI(BasePlayer player);
    private void CaseUI(BasePlayer player, int caseId, bool isFirstCurrent, int count);
    private void CaseModalUI(BasePlayer player, int caseId, bool isFreeCoin, int count);
    private void RefreshBalance(BasePlayer player);
    private void OpenCasesUI(BasePlayer player, int page);
    private void InventoryUI(BasePlayer player, int page);
    private void GiveUI(BasePlayer player, int itemId);
    private void MissionsUI(BasePlayer player, int page);
    private void ShowAward(BasePlayer player, MissionConfig mission, int ySwitch);
    private void RefreshCooldown();
    private void Notification(BasePlayer player, string key, object[] obj);
    private void OnMissionsProgress(BasePlayer player, MissionType type, Item item, BaseEntity entity, int grade);
    private static int CheckMission(MissionConfig mission, Item item, BaseEntity entity, int grade);
    private static void CompleteMission(BasePlayer player, MissionConfig mission, bool isPrivate);
    private void CheckPlayersMission();
    private static string FormatShortTime(TimeSpan time);
    private static MissionConfig GetPrivateMission(PlayerData data);
    private void UpdateMissions();
    private void UpdatePlayerMission(BasePlayer player, DateTime now);
    private List<GeneralMission> GetRandomMissions();
    private static PlayerData GetPlayerData(BasePlayer player);
    private static PlayerData GetPlayerData(ulong userId);
    private int GetFirstCurrency(ulong userId);
    private int GetFirstCurrency(PlayerData data);
    private int GetSecondCurrency(ulong userId);
    private int GetSecondCurrency(PlayerData data);
    private bool RemoveFirstCurrency(ulong player, int amount);
    private bool RemoveFirstCurrency(BasePlayer player, int amount);
    private bool RemoveFirstCurrency(PlayerData data, int amount);
    private bool RemoveSecondCurrency(ulong player, int amount);
    private bool RemoveSecondCurrency(BasePlayer player, int amount);
    private bool RemoveSecondCurrency(PlayerData data, int amount);
    private bool AddFirstCurrency(ulong player, int amount);
    private bool AddFirstCurrency(BasePlayer player, int amount);
    private bool AddFirstCurrency(PlayerData data, int amount);
    private bool AddSecondCurrency(ulong player, int amount);
    private bool AddSecondCurrency(BasePlayer player, int amount);
    private bool AddSecondCurrency(PlayerData data, int amount);
    private static List<ItemCase> GetRandom(CaseClass Case, int count);
    private static void Outline(CuiElementContainer container, string parent, string color, string size);
    private void Arrow(CuiElementContainer container, string parent);
    private Dictionary<int, ItemCase> GetItemsByPage(PlayerData data, int page, int count);
    private List<GeneralMission> GetMessingByPage(int page, int count);
    private static int GetMissionProgress(BasePlayer player, int missionId);
    private static float GetPlayerRates(string userId);
    private static string HexToCuiColor(string hex);
    private void Log(string filename, string key, object[] obj);
    protected override void LoadDefaultMessages();
    private string Msg(string key, string userid, object[] obj);
    private void Reply(BasePlayer player, string key, object[] obj);
}

private class GeneralMission
{
    public int ID;
    public MissionConfig Mission;
}

private class Configuration
{
    [JsonProperty(PropertyName = "Команда | Command")]
    public string Command;
    [JsonProperty(PropertyName = "Право | Permission")]
    public string Permission;
    [JsonProperty(PropertyName = "Фон | Background")]
    public string Background;
    [JsonProperty(PropertyName = "Логотип | Logo")]
    public string Logo;
    [JsonProperty(PropertyName =
				"Сбрасывать квест после его завершения? | Reset the quest after completing it?")]
    public bool ResetQuestAfterComplete;
    [JsonProperty(PropertyName = "Валюта 1 | Currency 1")]
    public FirstCurrencyClass FirstCurrency;
    [JsonProperty(PropertyName = "Использовать 2 валюту? | Use 2nd currency?")]
    public bool useSecondCur;
    [JsonProperty(PropertyName = "Валюта 2 | Currency 2")]
    public SecondCurrencyClass SecondCurrency;
    [JsonProperty(PropertyName = "Изображение сезона | Season image")]
    public string Battlepass;
    [JsonProperty(PropertyName = "Изображение кейсов | Case Image")]
    public string CaseImg;
    [JsonProperty(PropertyName = "Изображение инвентаря | Inventory Image")]
    public string InventoryImg;
    [JsonProperty(PropertyName = "Изображение топ награды | Image Top Awards")]
    public string AdvanceAward;
    [JsonProperty(PropertyName = "Фон для кейсов | Background for cases")]
    public string CaseBG;
    [JsonProperty(PropertyName = "Кейсы | Cases", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<CaseClass> Cases;
    [JsonProperty(PropertyName = "Количество общих миссий в день | Total missions per day")]
    public int MissionsCount;
    [JsonProperty(PropertyName =
				"Раз во сколько часов обновлять миссии? | How many hours are missions updated?")]
    public int MissionHours;
    [JsonProperty(PropertyName = "Настройка общих миссий | Settings shared missions",
				ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public Dictionary<int, MissionConfig> Missions;
    [JsonProperty(PropertyName = "Настройка вызова дня | Settings challenge of the day",
				ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public Dictionary<int, MissionConfig> PrivateMissions;
    [JsonProperty(PropertyName = "Включить логирование в консоль? | Enable logging to the console?")]
    public bool LogToConsole;
    [JsonProperty(PropertyName = "Включить логирование в файл? | Enable logging to the file?")]
    public bool LogToFile;
}

private class CurrencyClass
{
    [JsonProperty(PropertyName = "Картинка | Image")]
    public string Image;
    [JsonProperty(PropertyName = "Использовать встроенную систему? | Use embedded system?")]
    public bool useDefaultCur;
    [JsonProperty(PropertyName = "Название плагина | Plugin name")]
    public string Plug;
    [JsonProperty(PropertyName = "Функция пополнения баланса | Balance add hook")]
    public string AddHook;
    [JsonProperty(PropertyName = "Функция снятия баланса | Balance remove hook")]
    public string RemoveHook;
    [JsonProperty(PropertyName = "Функция показа баланса | Balance show hook")]
    public string BalanceHook;
}

private class FirstCurrencyClass : CurrencyClass
{
    [JsonProperty(PropertyName = "Rates for permissions",
				ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public Dictionary<string, float> Rates;
    public double ShowBalance(BasePlayer player);
    public void AddBalance(BasePlayer player, int amount);
    public bool RemoveBalance(BasePlayer player, int amount);
}

private class SecondCurrencyClass : CurrencyClass
{
    [JsonProperty(PropertyName = "Право | Permission")]
    public string Permission;
    public double ShowBalance(BasePlayer player);
    public void AddBalance(BasePlayer player, int amount);
    public bool RemoveBalance(BasePlayer player, int amount);
}

private class MissionConfig
{
    [JsonProperty(PropertyName = "Описание миссии | Mission description")]
    public string Description;
    [JsonProperty(PropertyName = "Тип миссии | Mission type")]
    [JsonConverter(typeof(StringEnumConverter))]
    public MissionType Type;
    [JsonProperty(PropertyName = "Шортнейм/префаб | Shortname/prefab")]
    public string Shortname;
    [JsonProperty(PropertyName = "Скин (0 - любой предмет) | Skin (0 - any item)")]
    public ulong Skin;
    [JsonProperty(PropertyName =
				"Уровень апгрейда (для миссий 'Улучшить') | Upgrade Level (for 'Улучшить' missions)")]
    public int Grade;
    [JsonProperty(PropertyName = "Количество | Amount")]
    public int Amount;
    [JsonProperty(PropertyName = "Количество основной награды | Amounr of main reward")]
    public int MainAward;
    [JsonProperty(PropertyName = "Давать доп.награду? | Give extra reward?")]
    public bool UseAdvanceAward;
    [JsonProperty(PropertyName = "Настройка доп.награды | Settings extra reward")]
    public AdvanceAward AdvanceAward;
    [JsonProperty(PropertyName = "(Вызов) Давать вторую валюту | (Challenge) Give second currency?")]
    public bool UseSecondAward;
    [JsonProperty(PropertyName = "(Вызов) Количество основной валюты | (Challenge) Amount of second currency")]
    public int SecondAward;
    private void GiveMainAward(BasePlayer player);
    public void GiveAwards(BasePlayer player);
    public void GivePrivateAwards(BasePlayer player);
}

private class AdvanceAward
{
    [JsonProperty(PropertyName =
				"Картинка (если пусто - берётся иконка по shortname) | Image (if empty - the icon is taken by shortname)")]
    public string Image;
    [JsonProperty(PropertyName =
				"Отображаемое имя (если пусто - стандартное) | Display Name (if empty - standard)")]
    public string DisplayName;
    [JsonProperty(PropertyName = "Отображаемое имя (для интерфейса) | Display Name (for interface)")]
    public string Title;
    [JsonProperty(PropertyName = "Shortname")]
    public string Shortname;
    [JsonProperty(PropertyName = "Скин | Skin")]
    public ulong Skin;
    [JsonProperty(PropertyName = "Количество (для предмета) | Amount (for item)")]
    public int Amount;
    public void GiveItem(BasePlayer player);
}

private class CaseClass
{
    [JsonProperty(PropertyName = "Отображаемое имя кейса | Case Display Name")]
    public string DisplayName;
    [JsonProperty(PropertyName = "Картинка | Image")]
    public string Image;
    [JsonProperty(PropertyName = "Право | Permission")]
    public string Permission;
    [JsonProperty(PropertyName = "Стоимость в валюте 1 | Cost in currency 1")]
    public int FCost;
    [JsonProperty(PropertyName = "Стоимость в валюте 2 | Cost in currency 2")]
    public int PCost;
    [JsonProperty(PropertyName = "Предметы | Items", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<ItemCase> Items;
}

private class ItemCase
{
    [JsonProperty(PropertyName =
				"Отображаемое имя (для вывода в интерфейсе) | Display Name (for display in the interface)")]
    public string Title;
    [JsonProperty(PropertyName = "Шанс | Chance")]
    public float Chance;
    [JsonProperty(PropertyName = "Тип предмета | Item type")]
    [JsonConverter(typeof(StringEnumConverter))]
    public ItemType Type;
    [JsonProperty(PropertyName =
				"Картинка (если пусто - берётся икнока по shortname) | Image (if empty - the icon is taken by shortname) ")]
    public string Image;
    [JsonProperty(PropertyName =
				"Отображаемое имя (для предмета) (если пусто - стандартное) | Display name (for the item) (if empty - standard)")]
    public string DisplayName;
    [JsonProperty(PropertyName = "Shortname")]
    public string Shortname;
    [JsonProperty(PropertyName = "Скин | Skin")]
    public ulong Skin;
    [JsonProperty(PropertyName = "Количество (для предмета) | Amount (for item)")]
    public int Amount;
    [JsonProperty(PropertyName = "Команда | Command")]
    public string Command;
    [JsonProperty(PropertyName = "Плагин | Plugin")]
    public PluginAward PluginAward;
    private void ToItem(BasePlayer player);
    private void ToCommand(BasePlayer player);
    public void GetItem(BasePlayer player);
}

private class PluginAward
{
    [JsonProperty("Название функции для вызова | Hook to call")]
    public string hook;
    [JsonProperty("Название плагина | Plugin name")]
    public string plugin;
    [JsonProperty("Количество | Amount")]
    public int amount;
    [JsonProperty("(GameStores) ИД магазина в сервисе")]
    public string ShopID;
    [JsonProperty("(GameStores) ИД сервера в сервисе")]
    public string ServerID;
    [JsonProperty("(GameStores) Секретный ключ")]
    public string SecretKey;
    public void ToPluginAward(BasePlayer player);
}

private class PluginData
{
    [JsonProperty(PropertyName = "Date of last mission update")]
    public DateTime MissionsDate;
    [JsonProperty(PropertyName = "Missions", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<int> Missions;
    [JsonProperty(PropertyName = "Players Data", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public Dictionary<ulong, PlayerData> PlayerDatas;
}

private class PlayerData
{
    [JsonProperty(PropertyName = "MissionDate")]
    public DateTime MissionDate;
    [JsonProperty(PropertyName = "Mission ID")]
    public int MissionId;
    [JsonProperty(PropertyName = "Mission Progress")]
    public int MissionProgress;
    [JsonProperty(PropertyName = "Currency 1")]
    public int FirstCurrency;
    [JsonProperty(PropertyName = "Currency 2")]
    public int SecondCurrency;
    [JsonProperty(PropertyName = "General Mission Progress",
				ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public Dictionary<int, int> Missions;
    [JsonProperty(PropertyName = "Items", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public Dictionary<int, ItemCase> Items;
}


```

---

## BattlePass

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
[Info("Battle Pass", "CASHR#6906", "1.0.0")]
internal class BattlePass : RustPlugin
{
    [PluginReference]
    private Plugin ImageLibrary;
    private Configuration _config;
    private const string Layer;
    private const string perm;
    private const string perm1;
    private class Configuration
    {
        [JsonProperty(PropertyName = "Ссылка на логотип сервера(64x64 px)")]
        public string serverLogoURL;
        [JsonProperty(PropertyName = "Название сервера")]
        public string serverName;
        [JsonProperty("Настройка очков")]
        public readonly PointSettings Point;
        [JsonProperty("Настройки уровней обычных уровней")]
        public readonly LevelSettings LevelDefault;
        [JsonProperty("Настройки уровней донатных уровней")]
        public readonly LevelSettings LevelDonate;
        internal class LevelSettings
        {
            [JsonProperty(PropertyName = "Список уровней", ObjectCreationHandling = ObjectCreationHandling.Replace)]
            public readonly List<Settings> LevelList;
            internal class Settings
            {
                [JsonProperty("Номер уровня")]
                public int Level;
                [JsonProperty("Количество Exp для получения этого уровня")]
                public int Exp;
                [JsonProperty("Картинка отображения награды")]
                public string Image;
                [JsonProperty("Награда за уровень")]
                public RewardSettings Reward;
                internal class RewardSettings
                {
                    [JsonProperty("ShortName")]
                    public string ShortName;
                    [JsonProperty("Amount")]
                    public int Amount;
                    [JsonProperty("SkinID")]
                    public ulong SkinID;
                    [JsonProperty("Команды, которые должны выполниться")]
                    public List<string> command;
                }

            }

        }

        internal class PointSettings
        {
            [JsonProperty("Множитель очков у донатера")]
            public readonly int DonateAmount;
            [JsonProperty("Количество очков за убийство игрока")]
            public readonly int killPlayer;
            [JsonProperty("Количество очков за убийство животных")]
            public readonly int killHuman;
            [JsonProperty("Количество очков за убийство вертолета")]
            public readonly int killHeli;
            [JsonProperty("Количество очков за убийство НПС")]
            public readonly int killNPC;
            [JsonProperty("Количество очков за убийство танка")]
            public readonly int killBredly;
            [JsonProperty("Количество отнимаемых очков за смерть")]
            public readonly int deathPlayer;
            [JsonProperty("Настройки добычи")]
            public readonly GatherSettings Gather;
        }

        internal class GatherSettings
        {
            [JsonProperty(PropertyName = "Настройка добычи(shortname/количество очков", ObjectCreationHandling = ObjectCreationHandling.Replace)]
            public readonly Dictionary<string, int> GatherList;
        }

    }

    protected override void LoadConfig();
    protected override void SaveConfig();
    protected override void LoadDefaultConfig();
    private void OnServerInitialized();
    private void OnPlayerConnected(BasePlayer player);
    private void Unload();
    private object OnDispenserBonus(ResourceDispenser dispenser, BasePlayer player, Item item);
    private void OnCollectiblePickup(Item item, BasePlayer player, CollectibleEntity entity);
    private readonly Dictionary<uint, ulong> LastHeliHit;
    private object OnEntityTakeDamage(BaseCombatEntity entity, HitInfo info);
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
    private void ShowUIMain(BasePlayer player, int page);
}

private class Configuration
{
    [JsonProperty(PropertyName = "Ссылка на логотип сервера(64x64 px)")]
    public string serverLogoURL;
    [JsonProperty(PropertyName = "Название сервера")]
    public string serverName;
    [JsonProperty("Настройка очков")]
    public readonly PointSettings Point;
    [JsonProperty("Настройки уровней обычных уровней")]
    public readonly LevelSettings LevelDefault;
    [JsonProperty("Настройки уровней донатных уровней")]
    public readonly LevelSettings LevelDonate;
    internal class LevelSettings
    {
        [JsonProperty(PropertyName = "Список уровней", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public readonly List<Settings> LevelList;
        internal class Settings
        {
            [JsonProperty("Номер уровня")]
            public int Level;
            [JsonProperty("Количество Exp для получения этого уровня")]
            public int Exp;
            [JsonProperty("Картинка отображения награды")]
            public string Image;
            [JsonProperty("Награда за уровень")]
            public RewardSettings Reward;
            internal class RewardSettings
            {
                [JsonProperty("ShortName")]
                public string ShortName;
                [JsonProperty("Amount")]
                public int Amount;
                [JsonProperty("SkinID")]
                public ulong SkinID;
                [JsonProperty("Команды, которые должны выполниться")]
                public List<string> command;
            }

        }

    }

    internal class PointSettings
    {
        [JsonProperty("Множитель очков у донатера")]
        public readonly int DonateAmount;
        [JsonProperty("Количество очков за убийство игрока")]
        public readonly int killPlayer;
        [JsonProperty("Количество очков за убийство животных")]
        public readonly int killHuman;
        [JsonProperty("Количество очков за убийство вертолета")]
        public readonly int killHeli;
        [JsonProperty("Количество очков за убийство НПС")]
        public readonly int killNPC;
        [JsonProperty("Количество очков за убийство танка")]
        public readonly int killBredly;
        [JsonProperty("Количество отнимаемых очков за смерть")]
        public readonly int deathPlayer;
        [JsonProperty("Настройки добычи")]
        public readonly GatherSettings Gather;
    }

    internal class GatherSettings
    {
        [JsonProperty(PropertyName = "Настройка добычи(shortname/количество очков", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public readonly Dictionary<string, int> GatherList;
    }

}

internal class LevelSettings
{
    [JsonProperty(PropertyName = "Список уровней", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public readonly List<Settings> LevelList;
    internal class Settings
    {
        [JsonProperty("Номер уровня")]
        public int Level;
        [JsonProperty("Количество Exp для получения этого уровня")]
        public int Exp;
        [JsonProperty("Картинка отображения награды")]
        public string Image;
        [JsonProperty("Награда за уровень")]
        public RewardSettings Reward;
        internal class RewardSettings
        {
            [JsonProperty("ShortName")]
            public string ShortName;
            [JsonProperty("Amount")]
            public int Amount;
            [JsonProperty("SkinID")]
            public ulong SkinID;
            [JsonProperty("Команды, которые должны выполниться")]
            public List<string> command;
        }

    }

}

internal class Settings
{
    [JsonProperty("Номер уровня")]
    public int Level;
    [JsonProperty("Количество Exp для получения этого уровня")]
    public int Exp;
    [JsonProperty("Картинка отображения награды")]
    public string Image;
    [JsonProperty("Награда за уровень")]
    public RewardSettings Reward;
    internal class RewardSettings
    {
        [JsonProperty("ShortName")]
        public string ShortName;
        [JsonProperty("Amount")]
        public int Amount;
        [JsonProperty("SkinID")]
        public ulong SkinID;
        [JsonProperty("Команды, которые должны выполниться")]
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
    [JsonProperty("Команды, которые должны выполниться")]
    public List<string> command;
}

internal class PointSettings
{
    [JsonProperty("Множитель очков у донатера")]
    public readonly int DonateAmount;
    [JsonProperty("Количество очков за убийство игрока")]
    public readonly int killPlayer;
    [JsonProperty("Количество очков за убийство животных")]
    public readonly int killHuman;
    [JsonProperty("Количество очков за убийство вертолета")]
    public readonly int killHeli;
    [JsonProperty("Количество очков за убийство НПС")]
    public readonly int killNPC;
    [JsonProperty("Количество очков за убийство танка")]
    public readonly int killBredly;
    [JsonProperty("Количество отнимаемых очков за смерть")]
    public readonly int deathPlayer;
    [JsonProperty("Настройки добычи")]
    public readonly GatherSettings Gather;
}

internal class GatherSettings
{
    [JsonProperty(PropertyName = "Настройка добычи(shortname/количество очков", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public readonly Dictionary<string, int> GatherList;
}

private class Data
{
    public int Level;
    public int Score;
    public readonly List<int> DefaultRewardID;
    public readonly List<int> DonateRewardID;
}


```

---

## BBank

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using Oxide.Core;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using UnityEngine;
using SteamID = System.UInt64;

Oxide.Plugins
[Info("Fucking Bank (BBank)", "bazuka5801", "1.0.0")]
public class BBank : RustPlugin
{
    private float NPC_Radius;
    private const int Hour;
    private const int CardCount;
     class CardConfig
    {
        public string Name;
        public int SecondsToUpgrade;
        public string CostItemShortname;
        public int CostItemAmount;
        public float AmountPercent;
        public float TimePercent;
        public int MaxItemAmount;
    }

    static Dictionary<CardType, CardConfig> Cards;
    static CardConfig GetCardConfig(CardType type);
     void OnServerInitialized();
     void Unload();
     void OnServerSave();
     object OnEntityTakeDamage(BaseCombatEntity entity, HitInfo info);
     void OnPlayerInit(BasePlayer player);
     void TimeTracker_Tick();
    [ChatCommand("spawnnpc")]
     void SpawnNPC(BasePlayer player);
     Dictionary<SteamID, BBankBox> boxesDB;
     void OpenLoot(BasePlayer player);
     void OnSaveLoot(BBankBox box, BasePlayer player);
     bool IsNPCNear(BasePlayer player);
    [ConsoleCommand("bbank.menu")]
     void cmdMenu(ConsoleSystem.Arg arg);
    [ConsoleCommand("bbank.open")]
     void cmdOpen(ConsoleSystem.Arg arg);
    [ConsoleCommand("bbank.upgrade")]
     void cmdUpgrade(ConsoleSystem.Arg arg);
    [ConsoleCommand("bbank.addtime")]
     void cmdAddTime(ConsoleSystem.Arg arg);
     BBankData Data;
     class BBankData
    {
        public Dictionary<SteamID, BankAccountData> Accounts;
        public HashSet<string> CardNumberList;
        public Vector3 NPCPosition;
        public uint NPCID;
        public BBankData();
    }

     class BankAccountData
    {
        public SteamID UserID;
        public string CardNumber;
        public string Shortname;
        public CardType CardType;
        public int Amount;
        public int PlayingSeconds;
        public int DepositSeconds;
        public bool NewCardAvailable;
        public CardConfig CardConfig { get; set; }
        public bool IsLastCard { get; set; }
        public BankAccountData(SteamID steamid, string cardNumber);
        public int GetAmountWithBonus();
        public int GetNextBonus();
        public int GetNextBonusSeconds();
        public void TimeTracker_Tick(BasePlayer player);
        public bool CardUpgrade(BasePlayer player);
        public void OnItemChanged(BasePlayer player, Item item);
    }

     void LoadData();
     void SaveData();
     BankAccountData GetData(BasePlayer player);
    public class BBankBox : MonoBehaviour
    {
         LootableCorpse corpse;
         BasePlayer owner;
        public void Init(LootableCorpse storage, BasePlayer owner);
        public static BBankBox Spawn(BasePlayer player, string name, int size);
        static int rayColl;
        public static LootableCorpse SpawnContainer(BasePlayer player, int size, string name);
        private void PlayerStoppedLooting(BasePlayer player);
        public void Close();
        public void StartLoot();
        public void Push(List<Item> items);
        public void ClearItems();
        public List<Item> GetItems { get; set; }
    }

    public static string CardNumber();
    public static string RandomNum(int length, bool withZero);
    public static string FormatTime(TimeSpan time, int maxSubstr, string language, bool @short);
    private static string Format(int units, string form1, string form2, string form3);
    private const string GUI_MENU;
    private const string GUI_ICON;
     void ShowGUI_Icon(BasePlayer player);
     void ShowGUI_Menu(BasePlayer player, BankAccountData accountData);
     void DestroyGUI_Menu(BasePlayer player);
    [PluginReference]
    private Plugin ImageLibrary;
    private static Dictionary<string, string> ImageData;
    private int LoadedImages;
    private bool IsLoadedImages();
     void UploadImages();
}

 class CardConfig
{
    public string Name;
    public int SecondsToUpgrade;
    public string CostItemShortname;
    public int CostItemAmount;
    public float AmountPercent;
    public float TimePercent;
    public int MaxItemAmount;
}

 class BBankData
{
    public Dictionary<SteamID, BankAccountData> Accounts;
    public HashSet<string> CardNumberList;
    public Vector3 NPCPosition;
    public uint NPCID;
    public BBankData();
}

 class BankAccountData
{
    public SteamID UserID;
    public string CardNumber;
    public string Shortname;
    public CardType CardType;
    public int Amount;
    public int PlayingSeconds;
    public int DepositSeconds;
    public bool NewCardAvailable;
    public CardConfig CardConfig { get; set; }
    public bool IsLastCard { get; set; }
    public BankAccountData(SteamID steamid, string cardNumber);
    public int GetAmountWithBonus();
    public int GetNextBonus();
    public int GetNextBonusSeconds();
    public void TimeTracker_Tick(BasePlayer player);
    public bool CardUpgrade(BasePlayer player);
    public void OnItemChanged(BasePlayer player, Item item);
}

public class BBankBox : MonoBehaviour
{
     LootableCorpse corpse;
     BasePlayer owner;
    public void Init(LootableCorpse storage, BasePlayer owner);
    public static BBankBox Spawn(BasePlayer player, string name, int size);
    static int rayColl;
    public static LootableCorpse SpawnContainer(BasePlayer player, int size, string name);
    private void PlayerStoppedLooting(BasePlayer player);
    public void Close();
    public void StartLoot();
    public void Push(List<Item> items);
    public void ClearItems();
    public List<Item> GetItems { get; set; }
}


```

---

## BeautyRestart

```csharp
using Oxide.Core;
using System;
using System.Reflection;
using System.Collections.Generic;
using UnityEngine;
using System.Linq;
using ConVar;
using Oxide.Game.Rust.Cui;

Oxide.Plugins
[Info("BeautyRestart", "Reynostrum", "1.0.0")]
[Description("Restart the server with a cooldown GUI timer.")]
 class BeautyRestart : RustPlugin
{
     int TimerSeconds;
     int Minutes;
     int Hours;
     int Seconds;
     string HoursF;
     string MinutesF;
     string SecondsF;
     string TimerMessage;
     bool IsTiming;
    static Timer TimerRefresh;
     string GUIAnchorMin { get; set; }
     string GUIAnchorMax { get; set; }
     string GUITextColor { get; set; }
     string GUIBackgroundColor { get; set; }
    protected override void LoadDefaultConfig();
    [ConsoleCommand("brestart")]
     void CMDConsoleRestart(ConsoleSystem.Arg arg);
    [ChatCommand("brestart")]
     void CMDCommandRestart(BasePlayer player, string cmd, string[] args);
    [ConsoleCommand("bcancelrestart")]
     void CMDConsoleCancelRestart(ConsoleSystem.Arg arg);
    [ChatCommand("bcancelrestart")]
     void CMDCommandCancelRestart(BasePlayer player, string cmd, string[] args);
     void Loaded();
     void Unload();
     void Checkcommand(ConsoleSystem.Arg arg, BasePlayer player, string minutes, string name, bool console);
     void Abort();
     string StyleSeconds(int SecondsT);
     void DoRestart();
     void TimeIn();
     void CreatingTimer(int CreatingTimerMinutes, string TimerName);
     void FULLGUIHandler(BasePlayer player, string Command);
     void GUITexto(BasePlayer player);
     void GUIBackground(BasePlayer player);
     void OnPlayerInit(BasePlayer player);
     T GetConfig(string name, T defaultValue);
     string Lang(string key, string id, object[] args);
     bool HasPermission(BasePlayer player, string perm);
     Dictionary<string, string> Messages;
}


```

---

## BedsCooldowns

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

## BeginnerProtection

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using Oxide.Rust.Libraries;
using Oxide.Core.Plugins;

Oxide.Plugins
[Info("Beginner Protection", "Kation", "0.1.2", ResourceId = 910)]
public class BeginnerProtection : RustPlugin
{
    private Dictionary<string, string> message;
    private Dictionary<string, object> setting;
    private Dictionary<string, int> gift;
    private Dictionary<string, string> nameTable;
    [ChatCommand("bp")]
    private void bpCommand(BasePlayer player, string cmd, string[] args);
    [ChatCommand("bp.gift.reload")]
    private void giftReloadCommand(BasePlayer player, string cmd, string[] args);
    [ChatCommand("bp.gift.set")]
    private void giftAddCommand(BasePlayer player, string cmd, string[] args);
    [HookMethod("OnServerInitialized")]
    private void OnServerInitialized();
    protected override void LoadDefaultConfig();
    [HookMethod("OnPlayerAttack")]
    private object OnPlayerAttack(BasePlayer attacker, HitInfo hitInfo);
    [HookMethod("OnPlayerRespawned")]
    private void OnPlayerRespawned(BasePlayer player);
    [HookMethod("OnPlayerDisconnected")]
    private void OnPlayerDisconnected(BasePlayer player);
    [HookMethod("OnPlayerSleepEnded")]
    private void OnPlayerSleepEnded(BasePlayer player);
    [HookMethod("OnPlayerLoot")]
    private void OnPlayerLoot(PlayerLoot lootInventory, BaseEntity targetEntity);
}


```

---

## BerrySpawn

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
using Rust;

Oxide.Plugins
[Info("BerrySpawn", "EcoSmile", "1.0.3")]
 class BerrySpawn : RustPlugin
{
    static BerrySpawn ins;
     PluginConfig config;
    public class PluginConfig
    {
        [JsonProperty("Время респавна кустов (в секундах)")]
        public float RespawnTime;
        [JsonProperty("Минимальное расстояние между кустами (Рекомендуется не менее 20)")]
        public float Distance;
    }

    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    protected override void SaveConfig();
     Coroutine coroutine;
     List<Vector3> Spawnpoint;
     void LoadData();
    private void OnServerInitialized();
     void Unload();
     void OnNewSave();
     Vector3 RandomCircle(Vector3 center, float radius);
     float GetGroundPosition(Vector3 pos);
    private bool TestPosIsValid(Vector3 position);
     List<string> berryPrefab;
     IEnumerator spawnBerry();
}

public class PluginConfig
{
    [JsonProperty("Время респавна кустов (в секундах)")]
    public float RespawnTime;
    [JsonProperty("Минимальное расстояние между кустами (Рекомендуется не менее 20)")]
    public float Distance;
}


```

---

## BestLoot

```csharp
using Rust;
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Text.RegularExpressions;
using System.Reflection;
using UnityEngine;
using Oxide.Core.Plugins;
using Random = System.Random;
using Oxide.Core;
using Newtonsoft.Json;

Oxide.Plugins
[Info("BestLoot", "Admin", "1.0", ResourceId = 0)]
[Description("Настройка лута в различных бочках и ящиках.")]
public class BestLoot : RustPlugin
{
    public const string LOOTTABLES_DATA;
    public const string permReloadLoot;
    public const string permResetConfig;
    public Random rnd;
     bool initialized;
    private ConfigData configData;
    private Dictionary<string, LootTableEntry> lootTables;
     class ConfigData
    {
        public Dictionary<string, LootTableMap> tables { get; set; }
        public List<string> includeammo_items { get; set; }
    }

     class LootTuple
    {
        public string item { get; set; }
        public int amount { get; set; }
        public string subitem { get; set; }
        public int subamount { get; set; }
        public bool nostack { get; set; }
        public float condition { get; set; }
        public LootTuple(string i, int a, string si, int sa, bool ns, float c);
    }

     class LootTableMap
    {
        public List<string> containers { get; set; }
        [JsonProperty(DefaultValueHandling = DefaultValueHandling.IgnoreAndPopulate, NullValueHandling = NullValueHandling.Ignore)]
        public string table { get; set; }
        [JsonProperty(DefaultValueHandling = DefaultValueHandling.IgnoreAndPopulate, NullValueHandling = NullValueHandling.Ignore)]
        public int amount { get; set; }
        [JsonProperty(DefaultValueHandling = DefaultValueHandling.IgnoreAndPopulate, NullValueHandling = NullValueHandling.Ignore)]
        public Dictionary<string, TableEntry> tables { get; set; }
    }

     class LootTableEntry
    {
        [JsonProperty(DefaultValueHandling = DefaultValueHandling.IgnoreAndPopulate, NullValueHandling = NullValueHandling.Ignore)]
        public bool includeammo { get; set; }
        public Dictionary<string, ItemEntry> items { get; set; }
    }

     class TableEntry
    {
        public string table { get; set; }
        public int chance { get; set; }
        public int amount { get; set; }
    }

     class ItemEntry
    {
        public int amount { get; set; }
        public int chance { get; set; }
        [JsonProperty(DefaultValueHandling = DefaultValueHandling.IgnoreAndPopulate, NullValueHandling = NullValueHandling.Ignore)]
        public bool nostack { get; set; }
        [JsonProperty(DefaultValueHandling = DefaultValueHandling.IgnoreAndPopulate, NullValueHandling = NullValueHandling.Ignore)]
        public int min { get; set; }
        [JsonProperty(DefaultValueHandling = DefaultValueHandling.IgnoreAndPopulate, NullValueHandling = NullValueHandling.Ignore)]
        public int max { get; set; }
        [JsonProperty(DefaultValueHandling = DefaultValueHandling.IgnoreAndPopulate, NullValueHandling = NullValueHandling.Ignore)]
        public float? condition { get; set; }
        [JsonProperty(DefaultValueHandling = DefaultValueHandling.IgnoreAndPopulate, NullValueHandling = NullValueHandling.Ignore)]
        public float? conditionmin { get; set; }
        [JsonProperty(DefaultValueHandling = DefaultValueHandling.IgnoreAndPopulate, NullValueHandling = NullValueHandling.Ignore)]
        public float? conditionmax { get; set; }
        [JsonProperty(DefaultValueHandling = DefaultValueHandling.IgnoreAndPopulate, NullValueHandling = NullValueHandling.Ignore)]
        public string subitem { get; set; }
        [JsonProperty(DefaultValueHandling = DefaultValueHandling.IgnoreAndPopulate, NullValueHandling = NullValueHandling.Ignore)]
        public int subamount { get; set; }
    }

     void Init();
     void OnServerInitialized();
    private void LoadVariables();
    protected override void LoadDefaultConfig();
     void LoadLootTables();
     void CreateDefaultLootTables();
     void SaveLootTables();
     void SaveConfig(ConfigData config);
    private void LoadConfigVariables();
     bool HasPerm(string id, string perm);
     void OnEntitySpawned(BaseNetworkable entity);
     void OnLoseCondition(Item item, float amount);
     void Unload();
    [ConsoleCommand("reloadloot")]
     void ReloadLootConsoleCmd(ConsoleSystem.Arg arg);
    [ChatCommand("reloadloot")]
     void ReloadLootCmd(BasePlayer player);
    [ChatCommand("resetconfig")]
     void ResetConfigCmd(ConsoleSystem.Arg arg);
     void ResetConfigCmd(BasePlayer player);
    private void ResetConfig();
    private void ReloadLoot();
     string PickLookTable(LootTableMap lootmap);
     List<LootTuple> PickItems(LootTableMap lootmap, bool ammolookup);
     void DisableSpawnLoot(LootContainer container);
     void FillContainer(LootContainer container);
     void EmptyContainer(LootContainer container);
}

 class ConfigData
{
    public Dictionary<string, LootTableMap> tables { get; set; }
    public List<string> includeammo_items { get; set; }
}

 class LootTuple
{
    public string item { get; set; }
    public int amount { get; set; }
    public string subitem { get; set; }
    public int subamount { get; set; }
    public bool nostack { get; set; }
    public float condition { get; set; }
    public LootTuple(string i, int a, string si, int sa, bool ns, float c);
}

 class LootTableMap
{
    public List<string> containers { get; set; }
    [JsonProperty(DefaultValueHandling = DefaultValueHandling.IgnoreAndPopulate, NullValueHandling = NullValueHandling.Ignore)]
    public string table { get; set; }
    [JsonProperty(DefaultValueHandling = DefaultValueHandling.IgnoreAndPopulate, NullValueHandling = NullValueHandling.Ignore)]
    public int amount { get; set; }
    [JsonProperty(DefaultValueHandling = DefaultValueHandling.IgnoreAndPopulate, NullValueHandling = NullValueHandling.Ignore)]
    public Dictionary<string, TableEntry> tables { get; set; }
}

 class LootTableEntry
{
    [JsonProperty(DefaultValueHandling = DefaultValueHandling.IgnoreAndPopulate, NullValueHandling = NullValueHandling.Ignore)]
    public bool includeammo { get; set; }
    public Dictionary<string, ItemEntry> items { get; set; }
}

 class TableEntry
{
    public string table { get; set; }
    public int chance { get; set; }
    public int amount { get; set; }
}

 class ItemEntry
{
    public int amount { get; set; }
    public int chance { get; set; }
    [JsonProperty(DefaultValueHandling = DefaultValueHandling.IgnoreAndPopulate, NullValueHandling = NullValueHandling.Ignore)]
    public bool nostack { get; set; }
    [JsonProperty(DefaultValueHandling = DefaultValueHandling.IgnoreAndPopulate, NullValueHandling = NullValueHandling.Ignore)]
    public int min { get; set; }
    [JsonProperty(DefaultValueHandling = DefaultValueHandling.IgnoreAndPopulate, NullValueHandling = NullValueHandling.Ignore)]
    public int max { get; set; }
    [JsonProperty(DefaultValueHandling = DefaultValueHandling.IgnoreAndPopulate, NullValueHandling = NullValueHandling.Ignore)]
    public float? condition { get; set; }
    [JsonProperty(DefaultValueHandling = DefaultValueHandling.IgnoreAndPopulate, NullValueHandling = NullValueHandling.Ignore)]
    public float? conditionmin { get; set; }
    [JsonProperty(DefaultValueHandling = DefaultValueHandling.IgnoreAndPopulate, NullValueHandling = NullValueHandling.Ignore)]
    public float? conditionmax { get; set; }
    [JsonProperty(DefaultValueHandling = DefaultValueHandling.IgnoreAndPopulate, NullValueHandling = NullValueHandling.Ignore)]
    public string subitem { get; set; }
    [JsonProperty(DefaultValueHandling = DefaultValueHandling.IgnoreAndPopulate, NullValueHandling = NullValueHandling.Ignore)]
    public int subamount { get; set; }
}


```

---

## BetterChat

```csharp
using System.Text.RegularExpressions;
using System.Collections.Generic;
using UnityEngine;
using System.Linq;
using System;

Oxide.Plugins
[Info("Better Chat", "LaserHydra", "4.0.2", ResourceId = 979)]
[Description("Better Chat")]
 class BetterChat : RustPlugin
{
     class Player
    {
        public ulong steamID;
        public string name;
        public MuteInfo mute;
        public List<ulong> ignoring;
        internal bool Ignored(BasePlayer player);
        internal bool Ignored(ulong steamID);
        internal void Update(BasePlayer player);
        static internal Player Find(BasePlayer player);
        static internal Player Find(ulong steamID);
        static internal void Create(BasePlayer player);
        static internal Player FindOrCreate(BasePlayer player);
        public Player();
        static internal void Updated();
        public override int GetHashCode();
    }

     class MuteInfo
    {
        internal bool Muted { get; set; }
         bool Expired { get; set; }
        public MutedState state;
        public Date date;
        internal Timer timer;
        internal ulong steamID;
        internal Player player { get; set; }
        internal BasePlayer BasePlayer { get; set; }
        internal void Updated();
        internal void Unmute(bool broadcast);
        internal void Mute(bool timed, DateTime time);
        internal void Update();
    }

     class Date
    {
        public string _value;
        internal DateTime value { get; set; }
        internal bool Expired { get; set; }
    }

     class Group
    {
        public Group();
        public string GroupName;
        public int Priority;
        public TitleSettings Title;
        public NameSettings PlayerName;
        public MessageSettings Message;
        public Formatting Formatting;
        internal Dictionary<string, object> Dictionary { get; set; }
        internal object Set(string key, string value);
        internal void Init();
        internal static void Updated();
        internal string Permission { get; set; }
        internal bool HasGroup(BasePlayer player);
        internal void AddToGroup(BasePlayer player);
        internal void RemoveFromGroup(BasePlayer player);
        internal void AddToGroup(ulong userID);
        internal void RemoveFromGroup(ulong userID);
        internal static Group Find(string GroupName);
        internal static void Remove(string GroupName);
        internal static List<Group> GetGroups(BasePlayer player, Sorting sorting);
        internal static List<Group> GetAllGroups(Sorting sorting);
        internal static Group GetPrimaryGroup(BasePlayer player);
        internal static string Format(BasePlayer player, string message, bool console);
        internal static string StripTags(string source);
        public override int GetHashCode();
        public override string ToString();
    }

     class TitleSettings
    {
        public bool Hidden;
        public bool HideIfNotHighestPriority;
        public int Size;
        public string Color;
        public string Text;
        internal string Formatted { get; set; }
    }

     class MessageSettings
    {
        public int Size;
        public string Color;
        internal string Formatted { get; set; }
        internal string Replace(string source, string message);
    }

     class Formatting
    {
        public string Console;
        public string Chat;
    }

     class NameSettings
    {
        public int Size;
        public string Color;
        internal string Formatted { get; set; }
        internal string Replace(string source, string name);
    }

    static BetterChat Plugin;
     List<Group> Groups;
     List<Player> Players;
     bool globalMute;
     bool General_ReverseTitleOrder;
     bool AntiFlood_Enabled;
     float AntiFlood_Seconds;
     bool WordFilter_Enabled;
     string WordFilter_Replacement;
     bool WordFilter_UseCustomReplacement;
     string WordFilter_CustomReplacement;
     List<object> WordFilter_Phrases;
     void Loaded();
     void OnPlayerInit(BasePlayer player);
     void LoadConfig();
     void LoadMessages();
    protected override void LoadDefaultConfig();
     string FormatTime(TimeSpan time);
     bool TryGetDateTime(string source, DateTime date);
    [ChatCommand("ignore")]
     void cmdIgnore(BasePlayer player, string cmd, string[] args);
    [ChatCommand("unignore")]
     void cmdUnignore(BasePlayer player, string cmd, string[] args);
    [ConsoleCommand("muteglobal")]
     void ccmdMuteGlobal(ConsoleSystem.Arg arg);
    [ChatCommand("muteglobal")]
     void cmdMuteGlobal(BasePlayer player, string cmd, string[] args);
    [ConsoleCommand("unmuteglobal")]
     void ccmdUnmuteGlobal(ConsoleSystem.Arg arg);
    [ChatCommand("unmuteglobal")]
     void cmdUnmuteGlobal(BasePlayer player, string cmd, string[] args);
    [ConsoleCommand("mute")]
     void ccmdMute(ConsoleSystem.Arg arg);
    [ChatCommand("mute")]
     void cmdMute(BasePlayer player, string cmd, string[] args);
    [ConsoleCommand("unmute")]
     void ccmdUnmute(ConsoleSystem.Arg arg);
    [ChatCommand("unmute")]
     void cmdUnmute(BasePlayer player, string cmd, string[] args);
    [ConsoleCommand("chat")]
     void ccmdBetterChat(ConsoleSystem.Arg arg);
    [ChatCommand("chat")]
     void cmdBetterChat(BasePlayer player, string cmd, string[] args);
     void OnUnmuted(Player player);
     string FilterText(string original);
     string Replace(string original);
     string TranslateLeet(string original);
     Dictionary<string, object> API_FindGroup(string name);
     List<Dictionary<string, object>> API_GetAllGroups();
     Dictionary<string, object> API_FindPlayerPrimaryGroup(BasePlayer player);
     List<Dictionary<string, object>> API_FindPlayerGroups(BasePlayer player);
     bool API_GroupExists(string name);
     bool API_AddGroup(string name);
     bool API_RemoveGroup(string name);
     bool API_IsUserInGroup(BasePlayer player, string groupName);
     bool API_RemoveUserFromGroup(BasePlayer player, string groupName);
     bool API_AddUserToGroup(BasePlayer player, string groupName);
     object API_SetGroupSetting(string groupName, string key, string value);
     bool API_IsPlayerMuted(BasePlayer player);
     bool API_PlayerIgnores(BasePlayer player1, BasePlayer player2);
     object OnPlayerChat(ConsoleSystem.Arg arg);
     void RunChatCommandFromConsole(ConsoleSystem.Arg arg, Action<BasePlayer, string, string[]> chatCommand);
     BasePlayer GetPlayer(string searchedPlayer, BasePlayer player);
     string ListToString(List<T> list, int first, string seperator);
     bool TryConvert(Source s, Converted c);
     void SetConfig(object[] args);
     T GetConfig(T defaultVal, object[] args);
     void LoadData(T data, string filename);
     void SaveData(T data, string filename);
     string Name { get; set; }
     string GetMsg(string key, object userID);
     void RegisterPerm(string[] permArray);
     bool HasPerm(object uid, string[] permArray);
     string PermissionPrefix { get; set; }
     void BroadcastChat(string prefix, string msg);
     void SendChatMessage(BasePlayer player, string prefix, string msg);
}

 class Player
{
    public ulong steamID;
    public string name;
    public MuteInfo mute;
    public List<ulong> ignoring;
    internal bool Ignored(BasePlayer player);
    internal bool Ignored(ulong steamID);
    internal void Update(BasePlayer player);
    static internal Player Find(BasePlayer player);
    static internal Player Find(ulong steamID);
    static internal void Create(BasePlayer player);
    static internal Player FindOrCreate(BasePlayer player);
    public Player();
    static internal void Updated();
    public override int GetHashCode();
}

 class MuteInfo
{
    internal bool Muted { get; set; }
     bool Expired { get; set; }
    public MutedState state;
    public Date date;
    internal Timer timer;
    internal ulong steamID;
    internal Player player { get; set; }
    internal BasePlayer BasePlayer { get; set; }
    internal void Updated();
    internal void Unmute(bool broadcast);
    internal void Mute(bool timed, DateTime time);
    internal void Update();
}

 class Date
{
    public string _value;
    internal DateTime value { get; set; }
    internal bool Expired { get; set; }
}

 class Group
{
    public Group();
    public string GroupName;
    public int Priority;
    public TitleSettings Title;
    public NameSettings PlayerName;
    public MessageSettings Message;
    public Formatting Formatting;
    internal Dictionary<string, object> Dictionary { get; set; }
    internal object Set(string key, string value);
    internal void Init();
    internal static void Updated();
    internal string Permission { get; set; }
    internal bool HasGroup(BasePlayer player);
    internal void AddToGroup(BasePlayer player);
    internal void RemoveFromGroup(BasePlayer player);
    internal void AddToGroup(ulong userID);
    internal void RemoveFromGroup(ulong userID);
    internal static Group Find(string GroupName);
    internal static void Remove(string GroupName);
    internal static List<Group> GetGroups(BasePlayer player, Sorting sorting);
    internal static List<Group> GetAllGroups(Sorting sorting);
    internal static Group GetPrimaryGroup(BasePlayer player);
    internal static string Format(BasePlayer player, string message, bool console);
    internal static string StripTags(string source);
    public override int GetHashCode();
    public override string ToString();
}

 class TitleSettings
{
    public bool Hidden;
    public bool HideIfNotHighestPriority;
    public int Size;
    public string Color;
    public string Text;
    internal string Formatted { get; set; }
}

 class MessageSettings
{
    public int Size;
    public string Color;
    internal string Formatted { get; set; }
    internal string Replace(string source, string message);
}

 class Formatting
{
    public string Console;
    public string Chat;
}

 class NameSettings
{
    public int Size;
    public string Color;
    internal string Formatted { get; set; }
    internal string Replace(string source, string name);
}


```

---

## BetterLoot

```csharp
using Rust;
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Text.RegularExpressions;
using System.Reflection;
using UnityEngine;
using Oxide.Core.Plugins;
using Random = System.Random;
using Oxide.Core;

Oxide.Plugins
[Info("BetterLoot", "Fujikura/dcode", "2.10.0", ResourceId = 828)]
[Description("A complete re-implementation of the drop system")]
public class BetterLoot : RustPlugin
{
     bool Changed;
     StoredSupplyDrop storedSupplyDrop;
     StoredHeliCrate storedHeliCrate;
     StoredExportNames storedExportNames;
     StoredLootTable storedLootTable;
     SeparateLootTable separateLootTable;
     StoredBlacklist storedBlacklist;
     Dictionary<string, string> messages;
     Regex barrelEx;
     Regex crateEx;
     Regex heliEx;
     List<string>[] items;
     List<string>[] itemsB;
     List<string>[] itemsC;
     List<string>[] itemsHeli;
     List<string>[] itemsSupply;
     int totalItems;
     int totalItemsB;
     int totalItemsC;
     int totalItemsHeli;
     int totalItemsSupply;
     int[] itemWeights;
     int[] itemWeightsB;
     int[] itemWeightsC;
     int[] itemWeightsHeli;
     int[] itemWeightsSupply;
     int totalItemWeight;
     int totalItemWeightB;
     int totalItemWeightC;
     int totalItemWeightHeli;
     int totalItemWeightSupply;
     List<ItemDefinition> originalItems;
     List<ItemDefinition> originalItemsB;
     List<ItemDefinition> originalItemsC;
     List<ItemDefinition> originalItemsHeli;
     List<ItemDefinition> originalItemsSupply;
     Random rng;
     bool initialized;
     int lastMinute;
     List<ContainerToRefresh> refreshList;
     DateTime lastRefresh;
    static Dictionary<string,object> defaultItemOverride();
    static List<string> itemListExcludes;
     bool pluginEnabled;
     int delayPluginInit;
     bool seperateLootTables;
     string barrelTypes;
     string crateTypes;
     bool enableBarrels;
     int minItemsPerBarrel;
     int maxItemsPerBarrel;
     bool enableCrates;
     int minItemsPerCrate;
     int maxItemsPerCrate;
     int minItemsPerSupplyDrop;
     int maxItemsPerSupplyDrop;
     int minItemsPerHeliCrate;
     int maxItemsPerHeliCrate;
     double baseItemRarity;
     int refreshMinutes;
     bool removeStackedContainers;
     bool enforceBlacklist;
     bool dropWeaponsWithAmmo;
     bool includeSupplyDrop;
     bool excludeHeliCrate;
     bool listUpdatesOnLoaded;
     bool listUpdatesOnRefresh;
     bool useCustomTableHeli;
     bool useCustomTableSupply;
     bool refreshBarrels;
     bool refreshCrates;
     Dictionary<string,object> rarityItemOverride;
     object GetConfig(string menu, string datavalue, object defaultValue);
     void LoadDefaultMessages();
     void LoadVariables();
    protected override void LoadDefaultConfig();
     Dictionary<string, string> weaponAmmunition;
     void Init();
     void OnServerInitialized();
     void OnTick();
     void UpdateInternals(bool doLog);
     void FixLoot();
     Item MightyRNG(string type);
     void ClearContainer(LootContainer container);
     void SuppressRefresh(LootContainer container);
     void PopulateContainer(LootContainer container);
     void OnEntitySpawned(BaseNetworkable entity);
    [ChatCommand("blacklist")]
     void cmdChatBlacklist(BasePlayer player, string command, string[] args);
     void OnItemAddedToContainer(ItemContainer container, Item item);
    static string ContainerName(LootContainer container);
    static int RarityIndex(Rarity rarity);
    static string RarityName(int index);
     bool ItemExists(string name);
     bool IsWeapon(string name);
     int ItemWeight(double baseRarity, int index);
     string _(string text, Dictionary<string, string> replacements);
     bool isSupplyDropActive();
     class ContainerToRefresh
    {
        public LootContainer container;
        public DateTime time;
    }

     class StoredLootTable
    {
        public Dictionary<string, int> ItemList;
        public StoredLootTable();
    }

     void LoadLootTable();
     void SaveLootTable();
     class SeparateLootTable
    {
        public Dictionary<string, int> ItemListBarrels;
        public Dictionary<string, int> ItemListCrates;
        public SeparateLootTable();
    }

     void LoadSeparateLootTable();
     void SaveSeparateLootTable();
     class StoredExportNames
    {
        public int version;
        public List<string> AllItemsAvailable;
        public Dictionary<string, int> ItemListStackable;
        public StoredExportNames();
    }

     void SaveExportNames();
     class StoredSupplyDrop
    {
        public Dictionary<string, int> ItemList;
        public StoredSupplyDrop();
    }

     void LoadSupplyDrop();
     void SaveSupplyDrop();
     class StoredHeliCrate
    {
        public Dictionary<string, int> ItemList;
        public StoredHeliCrate();
    }

     void LoadHeliCrate();
     void SaveHeliCrate();
     class StoredBlacklist
    {
        public List<string> ItemList;
        public StoredBlacklist();
    }

     void LoadBlacklist();
     void SaveBlacklist();
    [ConsoleCommand("betterloot.reload")]
     void consoleReload(ConsoleSystem.Arg arg);
}

 class ContainerToRefresh
{
    public LootContainer container;
    public DateTime time;
}

 class StoredLootTable
{
    public Dictionary<string, int> ItemList;
    public StoredLootTable();
}

 class SeparateLootTable
{
    public Dictionary<string, int> ItemListBarrels;
    public Dictionary<string, int> ItemListCrates;
    public SeparateLootTable();
}

 class StoredExportNames
{
    public int version;
    public List<string> AllItemsAvailable;
    public Dictionary<string, int> ItemListStackable;
    public StoredExportNames();
}

 class StoredSupplyDrop
{
    public Dictionary<string, int> ItemList;
    public StoredSupplyDrop();
}

 class StoredHeliCrate
{
    public Dictionary<string, int> ItemList;
    public StoredHeliCrate();
}

 class StoredBlacklist
{
    public List<string> ItemList;
    public StoredBlacklist();
}


```

---

## BetterResearching

```csharp

Oxide.Plugins
[Info("Better Researching", "Waizujin", 1.3)]
[Description("Allows instant researching and adjustable research chance.")]
public class BetterResearching : RustPlugin
{
    public float ResearchChance { get; set; }
    public float ResearchCostFraction { get; set; }
    public int PaperRequired { get; set; }
    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
     void OnServerInitialized();
     void OnItemDeployed(Deployer deployer, BaseEntity deployedEntity);
     void OnItemResearchStart(ResearchTable table);
    private float OnItemResearchEnd(ResearchTable table, float chance);
    public void updateResearchTables();
}


```

---

## BetterSay

```csharp
using System.Text.RegularExpressions;
using System.Collections.Generic;
using System.Linq;
using System;

Oxide.Plugins
[Info("Better Say", "LaserHydra", "2.0.2", ResourceId = 998)]
[Description("Customize the say console command output as you want")]
 class BetterSay : RustPlugin
{
     void Loaded();
     void LoadConfig();
     void LoadDefaultConfig();
     string RemoveFormatting(string old);
     object OnServerCommand(ConsoleSystem.Arg arg);
     BasePlayer GetPlayer(string searchedPlayer, BasePlayer executer, string prefix);
     string ListToString(List<string> list, int first, string seperator);
     void SetConfig(object[] args);
     void BroadcastChat(string prefix, string msg);
     void SendChatMessage(BasePlayer player, string prefix, string msg);
}


```

---

## BetterVanish-1.7.4

```csharp
using System;
using System.Collections.Generic;
using System.Globalization;
using System.Linq;
using Facepunch;
using Network;
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Libraries.Covalence;
using Oxide.Game.Rust.Cui;
using Rust;
using Rust.Ai;
using Rust.Registry;
using UnityEngine;
using QueryGrid = BaseEntity.Query;
using Human_Npc = global::HumanNPC;

Oxide.Plugins
[Info("BetterVanish", "Def", "1.7.4")]
[Description("Customized & Optimized \"Vanish\"")]
public class BetterVanish : RustPlugin
{
    private const string PermUse;
    private const string PermUseOther;
    private const string PermPermanent;
    private const string PermUnvanish;
    private const string PermInvSpy;
    private const string PermSkipLocks;
    private const string CuiName;
    private const string PlayerPrefab;
    private const string PlayerCorpsePrefab;
    private const string EffectDisappear;
    private const string EffectAppear;
    private static readonly Effect EffectInstance;
    private static readonly DamageTypeList EmptyDmgList;
    private static readonly GameObjectRef EmptyObjRef;
    private static readonly object FalseObj;
    private static readonly object TrueObj;
    private static readonly List<string> HooksLst;
    private static BetterVanish _instance;
    private static CuiElementContainer _cui;
    private GameObjectRef _fallDmgEff;
    private GameObjectRef _drownEff;
    private bool _isShutdown;
    private bool _lastHooksState;
    private const int CfgRev;
    private static Configuration _config;
    public class Configuration
    {
        [JsonProperty(PropertyName = "Show visual indicator (true/false)")]
        public bool ShowIndicator;
        [JsonProperty(PropertyName = "Visual indicator image address")]
        public string IndicatorAddress;
        [JsonProperty(PropertyName = "Visual indicator anchor min")]
        public string IndicatorAnchorMin;
        [JsonProperty(PropertyName = "Visual indicator anchor max")]
        public string IndicatorAnchorMax;
        [JsonProperty(PropertyName = "Visual indicator color")]
        public string IndicatorColor;
        [JsonProperty("Depth of an underground teleport (upon disconnection)")]
        public float UndergroundTeleportDepth;
        [JsonProperty(PropertyName = "Block all incoming damage while vanished (true/false)")]
        public bool BlockAllIncomingDamage;
        [JsonProperty(PropertyName = "Block all outgoing damage while vanished (true/false)")]
        public bool BlockAllOutgoingDamage;
        [JsonProperty(PropertyName = "Auto vanish on connect (true/false)")]
        public bool AutoVanish;
        [JsonProperty(PropertyName = "Auto noclip on connect (true/false)")]
        public bool AutoNoclip;
        [JsonProperty(PropertyName = "Auto noclip on vanish (true/false)")]
        public bool AutoNoclipOnVanish;
        [JsonProperty(PropertyName = "Turn off noclip on reappear (true/false)")]
        public bool TurnOffNoclipOnReappear;
        [JsonProperty(PropertyName = "Persist vanish (don't unhide upon leave & restore after restart)")]
        public bool PersistVanish;
        [JsonProperty(PropertyName = "Use sound effects (true/false)")]
        public bool SoundEffects;
        [JsonProperty(PropertyName = "Enable safepoints (true/false)")]
        public bool SafePoints;
        [JsonProperty(PropertyName = "Remove all safepoints after wipe (true/false)")]
        public bool SafePointsRemoval;
        [JsonProperty(PropertyName = "Config revision (do not edit)")]
        public int ConfigRev;
        public static Configuration DefaultConfig();
    }

    private void MigrateConfig();
    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private readonly ListHashSet<ulong> _hiddenPlayers;
    private readonly ListHashSet<ulong> _hiddenPlayersPersist;
    private Dictionary<ulong, string> _safePoints;
    private void LoadSafePoints();
    private void SaveSafePoints();
    private void LoadPersistPlayers();
    private void SavePersistPlayers();
    private void LoadData();
    private void SaveData();
    private class VanishComponent : MonoBehaviour, IEntity
    {
        private BasePlayer _owner;
        private GameObject _dummyObj;
        private static readonly ListHashSet<Type> InterestedTriggers;
        private void Awake();
        private void UpdateNetworkGroups();
        public void StopNetworkGroupsUpdate();
        public void StartNetworkGroupsUpdate();
        private void OnDestroy();
        public bool IsDestroyed { get; set; }
        private void SetupDummyCollider();
        private IEnumerable<TriggerBase> GetValidTriggers(Collider collider);
        private void OnTriggerEnter(Collider collider);
        private void OnTriggerExit(Collider collider);
    }

    private class LootProxyController : FacepunchBehaviour
    {
        public BasePlayer lootsrc;
        public BasePlayer looter;
        public PlayerCorpse proxy;
        public void Init(BasePlayer lootSource, BasePlayer looterPlayer);
        private void Awake();
        private void PlayerStoppedLooting(BasePlayer player);
        private void LifeCheck();
        private void OnDestroy();
    }

    protected override void LoadDefaultMessages();
    private void Init();
    private void OnServerInitialized();
    private void OnNewSave(string _);
    private void OnServerShutdown();
    private void Unload();
    private void VanishCommand(IPlayer iplayer, string command, string[] args);
    private void SetVanishCommand(IPlayer iplayer, string command, string[] args);
    private void UnvanishAllCommand(IPlayer iplayer, string command, string[] args);
    private void InvSpyCommand(IPlayer iplayer, string command, string[] args);
    private void Disappear(BasePlayer player, bool showGui);
    private void Reappear(BasePlayer player);
    private object OnEntityTakeDamage(BaseEntity victim, HitInfo info);
    private object OnEntityMarkHostile(BasePlayer player);
    private object OnPlayerColliderEnable(BasePlayer player, CapsuleCollider _);
    private object CanUseLockedEntity(BasePlayer player, BaseLock baseLock);
    private object OnPlayerViolation(BasePlayer player, AntiHackType type, float amount);
    private object OnTeamInvite(BasePlayer inviter, BasePlayer target);
    private void OnPlayerConnected(BasePlayer player);
    private void OnPlayerDisconnected(BasePlayer player);
    private static void VanishGui(BasePlayer player);
    private static void DestroyVanishGui(BasePlayer player);
    private void InvSpyShowLoot(BasePlayer player, string[] args);
    private static PlayerCorpse InvSpyCreateProxy(BasePlayer receiver, BasePlayer target);
    private void HandleEvent(BasePlayer player, VanishEvent evt);
    private static void RemoveFromTargets(BasePlayer player);
    private static void RemoveFromTargets0(BasePlayer player, BaseAIBrain brain);
    private void ReappearAll(Action<BasePlayer> onReappear);
    private void DisappearAll(Action<BasePlayer> onDisappear);
    private void AddPersistPlayer(BasePlayer player);
    private void AddCommandAliases(string key, string command);
    private void SubscribeToHooks();
    private void UnSubscribeFromHooks();
    private static void SendEffectTo(string effect, BasePlayer player);
    private bool HasSafePoint(BasePlayer player);
    private Vector3 GetSafePoint(BasePlayer player);
    private string Lang(string key, string id, object[] args);
    private bool HasPerm(string id, string perm);
    private bool IsPlayerVanished(BasePlayer player);
    private bool IsPlayerVanished(ulong playerId);
    private bool IsPlayerPersisted(BasePlayer player);
    private bool IsPlayerPermanent(BasePlayer player);
    private bool CanPersistPlayer(BasePlayer player);
    private static object FindPlayer(string name, bool sleeping);
    private static BasePlayer FindPlayerById(ulong uid);
    private static void SendEntitySnapshotEx(BaseNetworkable receiver, BaseNetworkable ent);
    private static string GetPrettyPlayerName(BasePlayer player);
    private void ManageHooks();
    private bool _IsInvisible(BasePlayer player);
    private bool _IsInvisible(IPlayer player);
    private void _Disappear(BasePlayer player);
    private void _Reappear(BasePlayer player);
}

public class Configuration
{
    [JsonProperty(PropertyName = "Show visual indicator (true/false)")]
    public bool ShowIndicator;
    [JsonProperty(PropertyName = "Visual indicator image address")]
    public string IndicatorAddress;
    [JsonProperty(PropertyName = "Visual indicator anchor min")]
    public string IndicatorAnchorMin;
    [JsonProperty(PropertyName = "Visual indicator anchor max")]
    public string IndicatorAnchorMax;
    [JsonProperty(PropertyName = "Visual indicator color")]
    public string IndicatorColor;
    [JsonProperty("Depth of an underground teleport (upon disconnection)")]
    public float UndergroundTeleportDepth;
    [JsonProperty(PropertyName = "Block all incoming damage while vanished (true/false)")]
    public bool BlockAllIncomingDamage;
    [JsonProperty(PropertyName = "Block all outgoing damage while vanished (true/false)")]
    public bool BlockAllOutgoingDamage;
    [JsonProperty(PropertyName = "Auto vanish on connect (true/false)")]
    public bool AutoVanish;
    [JsonProperty(PropertyName = "Auto noclip on connect (true/false)")]
    public bool AutoNoclip;
    [JsonProperty(PropertyName = "Auto noclip on vanish (true/false)")]
    public bool AutoNoclipOnVanish;
    [JsonProperty(PropertyName = "Turn off noclip on reappear (true/false)")]
    public bool TurnOffNoclipOnReappear;
    [JsonProperty(PropertyName = "Persist vanish (don't unhide upon leave & restore after restart)")]
    public bool PersistVanish;
    [JsonProperty(PropertyName = "Use sound effects (true/false)")]
    public bool SoundEffects;
    [JsonProperty(PropertyName = "Enable safepoints (true/false)")]
    public bool SafePoints;
    [JsonProperty(PropertyName = "Remove all safepoints after wipe (true/false)")]
    public bool SafePointsRemoval;
    [JsonProperty(PropertyName = "Config revision (do not edit)")]
    public int ConfigRev;
    public static Configuration DefaultConfig();
}

private class VanishComponent : MonoBehaviour, IEntity
{
    private BasePlayer _owner;
    private GameObject _dummyObj;
    private static readonly ListHashSet<Type> InterestedTriggers;
    private void Awake();
    private void UpdateNetworkGroups();
    public void StopNetworkGroupsUpdate();
    public void StartNetworkGroupsUpdate();
    private void OnDestroy();
    public bool IsDestroyed { get; set; }
    private void SetupDummyCollider();
    private IEnumerable<TriggerBase> GetValidTriggers(Collider collider);
    private void OnTriggerEnter(Collider collider);
    private void OnTriggerExit(Collider collider);
}

private class LootProxyController : FacepunchBehaviour
{
    public BasePlayer lootsrc;
    public BasePlayer looter;
    public PlayerCorpse proxy;
    public void Init(BasePlayer lootSource, BasePlayer looterPlayer);
    private void Awake();
    private void PlayerStoppedLooting(BasePlayer player);
    private void LifeCheck();
    private void OnDestroy();
}


```

---

## BGrade

```csharp
using System;
using System.Linq;
using System.Reflection;
using System.Collections.Generic;
using Rust;
using UnityEngine;
using Oxide.Core;
using Oxide.Core.Plugins;
using Oxide.Core.Libraries;
using Oxide.Plugins.BGradeExt;

Oxide.Plugins
[Info("BGrade", "Ryan", "1.0.49")]
[Description("Auto update building blocks when placed")]
public class BGrade : RustPlugin
{
    public static BGrade Instance;
    private List<string> RegisteredPerms;
    private static BindingFlags Flags;
    private readonly FieldInfo _permset;
    private DateTime HookExpire;
    private Dictionary<Vector3, int> LastAttacked;
    private bool ConfigChanged;
    private bool AllowTimer;
    private int MaxTimer;
    private int DefaultTimer;
    private bool CheckLastAttack;
    private int UpgradeCooldown;
    private List<string> ChatCommands;
    private List<string> ConsoleCommands;
    private bool RefundOnBlock;
    private bool DestroyOnDisconnect;
    protected override void LoadDefaultConfig();
    private void InitConfig();
    private T GetConfig(T defaultVal, string[] path);
    protected override void LoadDefaultMessages();
    private void RegisterPermissions();
    private void RegisterCommands();
    private void DestroyAll();
    private List<string> GetPluginPerms();
    private void DealWithHookResult(BasePlayer player, BuildingBlock buildingBlock, int hookResult, GameObject gameObject);
    private string TakeResources(BasePlayer player, int playerGrade, BuildingBlock buildingBlock, Dictionary<int, int> items);
    private void CheckLastAttacked();
    private bool WasAttackedRecently(Vector3 position);
    private class BGradePlayer : MonoBehaviour
    {
        private BasePlayer _player;
        private Timer _timer;
        private int _grade;
        private int _time;
        public void Awake();
        public int GetTime(bool updateTime);
        public void UpdateTime();
        public int GetGrade();
        public bool IsTimerValid { get; set; }
        private void SetTimer(Timer timer);
        public void SetGrade(int newGrade);
        public void SetTime(int newTime);
        public void DestroyTimer();
        public void OnDestroy();
    }

    private void Init();
    private void OnServerSave();
    private void Unload();
    private void OnEntityBuilt(Planner plan, GameObject gameObject);
    private void OnEntityDeath(BaseCombatEntity combatEntity, HitInfo info);
    private void OnPlayerDisconnected(BasePlayer player);
    private void BGradeCommand(BasePlayer player, string command, string[] args);
    private void BGradeUpCommand(ConsoleSystem.Arg arg);
}

private class BGradePlayer : MonoBehaviour
{
    private BasePlayer _player;
    private Timer _timer;
    private int _grade;
    private int _time;
    public void Awake();
    public int GetTime(bool updateTime);
    public void UpdateTime();
    public int GetGrade();
    public bool IsTimerValid { get; set; }
    private void SetTimer(Timer timer);
    public void SetGrade(int newGrade);
    public void SetTime(int newTime);
    public void DestroyTimer();
    public void OnDestroy();
}

Oxide.Plugins.BGradeExt
public static class BGradeExtensions
{
    private static readonly Permission permission;
    private static readonly Lang lang;
    public static bool HasAnyPermission(BasePlayer player, List<string> perms);
    public static bool HasPermission(BasePlayer player, string perm);
    public static bool HasPluginPerm(BasePlayer player, string perm);
    public static string Lang(string key, string id, object[] args);
    public static bool HasItemAmount(BasePlayer player, int itemId, int itemAmount);
    public static bool HasItemAmount(BasePlayer player, int itemId, int itemAmount, int amountGot);
    public static void TakeItem(BasePlayer player, int itemId, int itemAmount);
}


```

---

## BHair

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
[Info("BHair", "King", "1.0.0")]
public class BHair : RustPlugin
{
    [PluginReference]
     Plugin ImageLibrary;
    private static BHair plugin;
    public string Layer;
    public string NewLayer;
    public string NewLineLayer;
    public string HairLayer;
    private PluginConfig config;
    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    private void UpdateConfigValues();
    protected override void SaveConfig();
    public class MainSettings
    {
        [JsonProperty("Команда для меню прицелов")]
        public string openMenu;
        [JsonProperty(PropertyName = "Минимальный размер прицелов")]
        public int MinHairSize;
        [JsonProperty(PropertyName = "Максимальный размер прицелов")]
        public int MaxHairSize;
    }

    public class SettingsDefaultHair
    {
        [JsonProperty("Айди стандартного прицела ( Первоначальный айди прицела )")]
        public int HairID;
        [JsonProperty("Размеры стандратного прицела ( Первоначальный размер прицела )")]
        public int SizeHair;
    }

    public class SettingsHair
    {
        [JsonProperty("Картинка - ( Прицел )")]
        public string Image;
        [JsonProperty("Айди прицела ( При создании новых прибавлять )")]
        public int HairID;
    }

    private class PluginConfig
    {
        [JsonProperty("Основные настройки")]
        public MainSettings _MainSettings;
        [JsonProperty("Стандартные настройки игрока")]
        public SettingsDefaultHair _SettingsDefaultHair;
        [JsonProperty("Настройка выбора прицелов")]
        public List<SettingsHair> _SettingsHair;
        [JsonProperty("Config version")]
        public VersionNumber PluginVersion;
        public static PluginConfig DefaultConfig();
    }

    private void LoadData();
    private void SaveData();
    private class PluginData
    {
        public Dictionary<ulong, PlayerData> PlayerData;
    }

    private static PluginData data;
    private class PlayerData
    {
        public int HairID;
        public int HairSize;
        public static PlayerData GetOrAdd(BasePlayer player);
        public static PlayerData GetOrAdd(ulong userId);
    }

    private bool HasImage(string imageName, ulong imageId);
    private bool AddImage(string url, string shortname, ulong skin);
    private string GetImage(string shortname, ulong skin);
    private void OnServerInitialized();
    private void Unload();
    private void OnPlayerConnected(BasePlayer player);
    private void MenuOpen(BasePlayer player);
    private void MainUi(BasePlayer player);
    private void HairUI(BasePlayer player, string hair);
    [ConsoleCommand("UI_BHAIR")]
    private void CmdConsoleMarkers(ConsoleSystem.Arg arg);
}

public class MainSettings
{
    [JsonProperty("Команда для меню прицелов")]
    public string openMenu;
    [JsonProperty(PropertyName = "Минимальный размер прицелов")]
    public int MinHairSize;
    [JsonProperty(PropertyName = "Максимальный размер прицелов")]
    public int MaxHairSize;
}

public class SettingsDefaultHair
{
    [JsonProperty("Айди стандартного прицела ( Первоначальный айди прицела )")]
    public int HairID;
    [JsonProperty("Размеры стандратного прицела ( Первоначальный размер прицела )")]
    public int SizeHair;
}

public class SettingsHair
{
    [JsonProperty("Картинка - ( Прицел )")]
    public string Image;
    [JsonProperty("Айди прицела ( При создании новых прибавлять )")]
    public int HairID;
}

private class PluginConfig
{
    [JsonProperty("Основные настройки")]
    public MainSettings _MainSettings;
    [JsonProperty("Стандартные настройки игрока")]
    public SettingsDefaultHair _SettingsDefaultHair;
    [JsonProperty("Настройка выбора прицелов")]
    public List<SettingsHair> _SettingsHair;
    [JsonProperty("Config version")]
    public VersionNumber PluginVersion;
    public static PluginConfig DefaultConfig();
}

private class PluginData
{
    public Dictionary<ulong, PlayerData> PlayerData;
}

private class PlayerData
{
    public int HairID;
    public int HairSize;
    public static PlayerData GetOrAdd(BasePlayer player);
    public static PlayerData GetOrAdd(ulong userId);
}


```

---

## BHelp

```csharp
using System.Security;
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
[Info("BHelp", "https://devplugins.ru/", "1.0.0")]
public class BHelp : RustPlugin
{
     Dictionary<string, string> Buttons;
    private string Layer;
    private void OnServerInitialized();
    private void Unload();
    private PluginConfig config;
    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    private void UpdateConfigValues();
    protected override void SaveConfig();
    public class SettingsHelp
    {
        [JsonProperty("Название страницы")]
        public string _Name;
        [JsonProperty("Титл страницы")]
        public string _Title;
        [JsonProperty("Текст страницы")]
        public string _Text;
    }

    private class PluginConfig
    {
        [JsonProperty("Настройка кнопок")]
        public Dictionary<string, SettingsHelp> _SettingsHelp;
        [JsonProperty("Config version")]
        public VersionNumber PluginVersion;
        public static PluginConfig DefaultConfig();
    }

    [ConsoleCommand("UI_HELP")]
    private void cmdBHelp(ConsoleSystem.Arg args);
    private void MainUI(BasePlayer player);
    private void MenuButtons(BasePlayer player, string Button);
    private void TextUI(BasePlayer player, SettingsHelp _config);
}

public class SettingsHelp
{
    [JsonProperty("Название страницы")]
    public string _Name;
    [JsonProperty("Титл страницы")]
    public string _Title;
    [JsonProperty("Текст страницы")]
    public string _Text;
}

private class PluginConfig
{
    [JsonProperty("Настройка кнопок")]
    public Dictionary<string, SettingsHelp> _SettingsHelp;
    [JsonProperty("Config version")]
    public VersionNumber PluginVersion;
    public static PluginConfig DefaultConfig();
}


```

---

## BigBox

```csharp

Oxide.Plugins
[Info("BigBox", "Frizen", "1.0.0")]
internal class BigBox : RustPlugin
{
    private void OnServerInitialized();
     void OnEntitySpawned(BaseNetworkable entity);
}


```

---

## BlackJack

```csharp
using Newtonsoft.Json;
using Oxide.Game.Rust.Cui;
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

Oxide.Plugins
[Info("BlackJack", "k1lly0u", "0.1.2")]
[Description("A Black Jack card game used with the Casino plugin")]
 class BlackJack : RustPlugin
{
    public static BlackJack Instance { get; set; }
    private void Loaded();
    private void OnServerInitialized();
    private void Unload();
    private void InitializeGame(BaseEntity table, Casino.StoredData.GameData gameData);
    private const string GAME_BG;
    private const string PLAYER_OVERLAY;
    private const string BALANCE_OVERLAY;
    private const string BETTING_OVERLAY;
    private const string MESSAGE_OVERLAY;
    private const string NOTIFICATION_OVERLAY;
    private const string PLAY_OVERLAY;
    private const string STATUS_OVERLAY;
    private static string BLACK_COLOR;
    public class BlackJackGame : Casino.CardGame
    {
        private BlackJackAI DealerAI;
        private Casino.Deck deck;
        private CuiElementContainer backgroundContainer;
        private Dictionary<string, CuiElementContainer> containers;
        private int playerIndex;
        internal readonly Casino.UI4[] cardPositions;
        public override void OnPlayerEnter(BasePlayer player, int position);
        public override void OnPlayerExit(BasePlayer player);
        private void OnPlayerExit(BlackJackPlayer blackjackPlayer);
        public override void OnGameInitialized();
        private void ResetGame();
        private void CreatePlayerUI(bool betsLocked);
        private void CreatePlayerUI(Casino.CardPlayer targetPlayer, bool betsLocked);
        private CuiElementContainer CreatePlayerUIElement(bool betsLocked);
        private void PlaceBets();
        internal void PlaceBets(BlackJackPlayer blackjackPlayer);
        internal void CreateUIMessage(string message);
        internal void CreateUIMessage(BlackJackPlayer blackjackPlayer, string message);
        internal void CreateUIMessageOffset(string message, float time);
        private void PreStartRound();
        private void StartRound();
        private IEnumerator DealCards();
        internal void DealCard(BlackJackPlayer blackjackPlayer, bool hidden);
        private void NextPlayerTurn();
        private void UserPlayTurn();
        private void CreatePlayOverlay(BlackJackPlayer blackjackPlayer);
        internal void Hit(BlackJackPlayer blackjackPlayer);
        internal void Stay(BlackJackPlayer blackjackPlayer);
        internal void FinalizeGame();
        private void CalculateScores();
        internal void DestroyUIElement(string str);
        internal void DestroyUIElements();
        internal void AddUIElement(string panel, CuiElementContainer container, bool addToList);
    }

    internal class BlackJackPlayer : Casino.CardPlayer
    {
        public bool IsBusted { get; set; }
        internal void Hit(Casino.Card dealtCard);
        internal List<Casino.Card> Show();
        internal Casino.Card LastCard();
        internal int Count { get; set; }
        internal override void OnDestroy();
        internal override void ResetHand();
        internal void DestroyCards();
        internal void DestroyUIElements();
        internal bool HasBlackJack();
        internal int GetScore();
    }

    private class BlackJackAI : BlackJackPlayer
    {
        private Coroutine playRoutine;
        internal override void Awake();
        internal override void OnDestroy();
        internal override void ResetHand();
        internal void RegisterCardGame(Casino.CardGame cardGame);
        public void PlayTurn();
        public IEnumerator RunAI();
        private void RevealHiddenCard();
    }

    [ConsoleCommand("blackjack.leavetable")]
    private void ccmdLeaveTable(ConsoleSystem.Arg arg);
    [ConsoleCommand("blackjack.hit")]
    private void ccmdHit(ConsoleSystem.Arg arg);
    [ConsoleCommand("blackjack.stay")]
    private void ccmdStay(ConsoleSystem.Arg arg);
    [ConsoleCommand("blackjack.bet")]
    private void ccmdBet(ConsoleSystem.Arg arg);
    [ConsoleCommand("blackjack.deductbet")]
    private void ccmdDeductBet(ConsoleSystem.Arg arg);
    [ConsoleCommand("blackjack.setbet")]
    private void ccmdSetBet(ConsoleSystem.Arg arg);
    private ConfigData configData;
    private class ConfigData
    {
        [JsonProperty("Card Color (blue, gray, green, purple, red, yellow)")]
        public string CardColor { get; set; }
        [JsonProperty("Show Winnings")]
        public bool ShowWinnings { get; set; }
        [JsonProperty("Show Balance")]
        public bool ShowBalance { get; set; }
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

public class BlackJackGame : Casino.CardGame
{
    private BlackJackAI DealerAI;
    private Casino.Deck deck;
    private CuiElementContainer backgroundContainer;
    private Dictionary<string, CuiElementContainer> containers;
    private int playerIndex;
    internal readonly Casino.UI4[] cardPositions;
    public override void OnPlayerEnter(BasePlayer player, int position);
    public override void OnPlayerExit(BasePlayer player);
    private void OnPlayerExit(BlackJackPlayer blackjackPlayer);
    public override void OnGameInitialized();
    private void ResetGame();
    private void CreatePlayerUI(bool betsLocked);
    private void CreatePlayerUI(Casino.CardPlayer targetPlayer, bool betsLocked);
    private CuiElementContainer CreatePlayerUIElement(bool betsLocked);
    private void PlaceBets();
    internal void PlaceBets(BlackJackPlayer blackjackPlayer);
    internal void CreateUIMessage(string message);
    internal void CreateUIMessage(BlackJackPlayer blackjackPlayer, string message);
    internal void CreateUIMessageOffset(string message, float time);
    private void PreStartRound();
    private void StartRound();
    private IEnumerator DealCards();
    internal void DealCard(BlackJackPlayer blackjackPlayer, bool hidden);
    private void NextPlayerTurn();
    private void UserPlayTurn();
    private void CreatePlayOverlay(BlackJackPlayer blackjackPlayer);
    internal void Hit(BlackJackPlayer blackjackPlayer);
    internal void Stay(BlackJackPlayer blackjackPlayer);
    internal void FinalizeGame();
    private void CalculateScores();
    internal void DestroyUIElement(string str);
    internal void DestroyUIElements();
    internal void AddUIElement(string panel, CuiElementContainer container, bool addToList);
}

internal class BlackJackPlayer : Casino.CardPlayer
{
    public bool IsBusted { get; set; }
    internal void Hit(Casino.Card dealtCard);
    internal List<Casino.Card> Show();
    internal Casino.Card LastCard();
    internal int Count { get; set; }
    internal override void OnDestroy();
    internal override void ResetHand();
    internal void DestroyCards();
    internal void DestroyUIElements();
    internal bool HasBlackJack();
    internal int GetScore();
}

private class BlackJackAI : BlackJackPlayer
{
    private Coroutine playRoutine;
    internal override void Awake();
    internal override void OnDestroy();
    internal override void ResetHand();
    internal void RegisterCardGame(Casino.CardGame cardGame);
    public void PlayTurn();
    public IEnumerator RunAI();
    private void RevealHiddenCard();
}

private class ConfigData
{
    [JsonProperty("Card Color (blue, gray, green, purple, red, yellow)")]
    public string CardColor { get; set; }
    [JsonProperty("Show Winnings")]
    public bool ShowWinnings { get; set; }
    [JsonProperty("Show Balance")]
    public bool ShowBalance { get; set; }
    public Oxide.Core.VersionNumber Version { get; set; }
}


```

---

## BlessOfTheGods

```csharp
using System;
using UnityEngine;
using Random=System.Random;
using Rust;
using Rust.Xp;
using System.Collections.Generic;

Oxide.Plugins
[Info("Blessing of the Gods", "Dora", "1.0.4", ResourceId = 2022)]
[Description("Player get blessed by the gods.")]
 class BlessOfTheGods : RustPlugin
{
     Random rng;
     int number;
     void LoadDefaultMessages();
    private ConfigData configData;
     class ConfigData
    {
        public int repeatInterval { get; set; }
        public bool autoBless { get; set; }
        public int godOfGluttonyFood { get; set; }
        public int godOfGluttonyWater { get; set; }
        public float godOfDamnHPDeduction { get; set; }
        public int godOfPlaguePoison { get; set; }
        public bool godOfWarPercentEnabler { get; set; }
        public float godOfWarPercentXP { get; set; }
        public int godOfWarFixedXP { get; set; }
        public bool enableGodOfLife { get; set; }
        public bool enableGodOfDamn { get; set; }
        public bool enableGodOfGluttony { get; set; }
        public bool enableGodOfPlague { get; set; }
        public bool enableGodOfWar { get; set; }
        public bool enableGodOfDeath { get; set; }
    }

    private void LoadVariables();
    protected override void LoadDefaultConfig();
    private void LoadConfigVariables();
    private void SaveConfig(ConfigData config);
    private void OnServerInitialized();
    private void repeatBless();
    private void blessPlayers();
    private void godOfLife(BasePlayer player);
    private void godOfDamn(BasePlayer player);
    private void godOfGluttony(BasePlayer player);
    private void godOfPoverty(BasePlayer player);
    private void godOfDeath(BasePlayer player);
    private void godOfPlague(BasePlayer player);
    private void godOfWar(BasePlayer player);
    [ChatCommand("rebless")]
    private void blessPlayer(BasePlayer player);
    [ChatCommand("blesshelp")]
    private void blessHelp(BasePlayer player);
    [ChatCommand("bless")]
    private void blessing(BasePlayer player, string command, string[] args);
    private void sendChatMessage(BasePlayer player, string prefix, string msg);
    private void broadcastChat(string prefix, string msg);
    private void broadcastSuccess(BasePlayer player, String godName, String action, String msg, string color);
    private void sendToPlayer(BasePlayer player, String godName, String action, String msg, string color);
     string Lang(string key, string id, object[] args);
    private BasePlayer getPlayerName(string name);
    private bool hasPermission(BasePlayer player, string perm);
}

 class ConfigData
{
    public int repeatInterval { get; set; }
    public bool autoBless { get; set; }
    public int godOfGluttonyFood { get; set; }
    public int godOfGluttonyWater { get; set; }
    public float godOfDamnHPDeduction { get; set; }
    public int godOfPlaguePoison { get; set; }
    public bool godOfWarPercentEnabler { get; set; }
    public float godOfWarPercentXP { get; set; }
    public int godOfWarFixedXP { get; set; }
    public bool enableGodOfLife { get; set; }
    public bool enableGodOfDamn { get; set; }
    public bool enableGodOfGluttony { get; set; }
    public bool enableGodOfPlague { get; set; }
    public bool enableGodOfWar { get; set; }
    public bool enableGodOfDeath { get; set; }
}


```

---

## BlockBlockElevators

```csharp
using System.Collections.Generic;
using System;
using System.Reflection;
using System.Data;
using UnityEngine;
using Oxide.Core;

Oxide.Plugins
[Info("BlockBlockElevators", "Reneb", "1.0.1")]
 class BlockBlockElevators : RustPlugin
{
     BuildingBlock cachedBlock;
     TriggerBase cachedTrigger;
     BasePlayer cachedPlayer;
     int playerMask;
     int blockLayer;
     int triggerLayer;
     bool hasStarted;
     void OnServerInitialized();
     void OnEntityBuilt(Planner planner, GameObject gameObject);
     void ResetBlock(BuildingBlock block);
     void OnEntityEnter(TriggerBase triggerbase, BaseEntity entity);
}


```

---

## BlockBugPrevent

```csharp
using System.Collections.Generic;
using UnityEngine;

Oxide.Plugins
[Info("BlockBugPrevent", "sami37", "1.0.0", ResourceId = 2166)]
[Description("Prevent foundation block build on another foundation.")]
public class BlockBugPrevent : RustPlugin
{
     void Loaded();
    static int colisionentity;
     void OnEntityBuilt(Planner planner, UnityEngine.GameObject gameObject);
}


```

---

## BlockRemover

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Reflection;
using Oxide.Core;
using UnityEngine;

Oxide.Plugins
[Info("Block Remover", "bawNg / Nogrod", "0.4.1")]
 class BlockRemover : RustPlugin
{
    private ConfigData configData;
    private readonly FieldInfo entityListField;
    private readonly FieldInfo instancesField;
    private readonly Collider[] colBuffer;
    private const string PermCount;
    private const string PermRemove;
     class ConfigData
    {
        public float CupboardDistance { get; set; }
        public VersionNumber Version { get; set; }
    }

    protected override void LoadDefaultConfig();
     void Loaded();
    [ConsoleCommand("block.countall")]
     void cmdCountBlockAll(ConsoleSystem.Arg arg);
    [ConsoleCommand("block.count")]
     void cmdCountBlock(ConsoleSystem.Arg arg);
    [ConsoleCommand("block.remove")]
     void cmdRemoveBlock(ConsoleSystem.Arg arg);
    [ConsoleCommand("block.removeall")]
     void cmdRemoveBlockAll(ConsoleSystem.Arg arg);
     HashSet<BuildingBlock> FindAllCupboardlessBlocks(BuildingGrade.Enum grade);
     HashSet<StabilityEntity> FindAllCupboardlessStabilityEntities();
     void FilterAllCupboardless(HashSet<T> blocks);
     HashSet<BuildingBlock> FindAllBuildingBlocks(BuildingGrade.Enum grade);
     HashSet<StabilityEntity> FindAllStabilityEntities();
     BuildingPrivlidge[] FindAllToolCupboards();
     bool CheckAccess(ConsoleSystem.Arg arg, string perm);
     bool ParseGrade(ConsoleSystem.Arg arg, BuildingGrade.Enum grade);
}

 class ConfigData
{
    public float CupboardDistance { get; set; }
    public VersionNumber Version { get; set; }
}


```

---

## BlockStashPlacement

```csharp
using System.Collections.Generic;
using UnityEngine;

Oxide.Plugins
[Info("BlockStashPlacement", "Flaymar", "1.0.2", ResourceId = 2060)]
[Description("Blocks small stash placement!")]
public class BlockStashPlacement : RustPlugin
{
     void Loaded();
     Dictionary<string, string> messages;
     void OnEntitySpawned(BaseEntity entity, GameObject gameObject);
}


```

---

## BlockStructure

```csharp
using System.Collections.Generic;
using UnityEngine;
using System;

Oxide.Plugins
[Info("BlockStructure", "Marat", "1.0.1, ResourceId = 2092")]
[Description("Sets a limit build in height and depth in water")]
 class BlockStructure : RustPlugin
{
     void Loaded();
     int HeightBlock;
     int WaterBlock;
     bool ConfigChanged;
     bool usePermissions;
     bool BlockInHeight;
     bool BlockInWater;
     bool BlockInRock;
     string permBS;
    protected override void LoadDefaultConfig();
     void LoadConfiguration();
     T GetConfigValue(string category, string setting, T defaultValue);
     void LoadDefaultMessages();
     void Block(BaseNetworkable block, BasePlayer player, bool Height, bool Water);
     string Lang(string key, string id, object[] args);
     void Reply(BasePlayer player, string message, string args);
     void OnEntityBuilt(Planner plan, GameObject obj);
     bool IsAllowed(string id, string perm);
}


```

---

## BlockSystem

```csharp
using System.Linq;
using System;
using System.Collections.Generic;
using Oxide.Core;
using UnityEngine;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;

Oxide.Plugins
[Info("BlockSystem", "Chibubrik", "1.0.0")]
 class BlockSystem : RustPlugin
{
    [PluginReference]
     Plugin ImageLibrary;
     Dictionary<int, List<string>> settings;
     void OnServerInitialized();
    private object CanWearItem(PlayerInventory inventory, Item item);
    private object CanEquipItem(PlayerInventory inventory, Item item);
    private object CanMoveItem(Item item, PlayerInventory inventory, uint targetContainer);
    private object OnReloadWeapon(BasePlayer player, BaseProjectile projectile);
    private object OnReloadMagazine(BasePlayer player, BaseProjectile projectile);
    private object CanAcceptItem(ItemContainer container, Item item);
    private double IsBlocked(ItemDefinition itemDefinition);
    private double IsBlocked(string shortname);
    private double UnBlockTime(int amount);
    static readonly DateTime epoch;
    static double CurrentTime();
    public static string FormatShortTime(TimeSpan time);
    [ChatCommand("block")]
     void ChatBlock(BasePlayer player);
     void BlockUI(BasePlayer player);
}


```

---

## BlueprintBlocker

```csharp
using Oxide.Core;
using Oxide.Core.Plugins;
using Oxide.Core;
using System.Data;
using System;
using System.Collections.Generic;
using System.Linq;
using UnityEngine;

Oxide.Plugins
[Info("BlueprintBlocker", "DylanSMR", "1.0.5")]
[Description("Blocks certain blueprint items.")]
 class BlueprintBlocker : RustPlugin
{
    private List<string> blueprintBlacklist;
    private uint FragAmount;
    private uint EFragAmount;
    private uint PlayerTotalFrags;
    private uint EntityTotalFrags;
     void LoadDefaultConfig();
     T GetConfig(string key, T defaultValue);
     void Loaded();
     void LoadLangaugeAPI();
    private string GetMessage(string name, string sid);
     void OnConsumableUse(Item item);
    private void OnEntitySpawned(BaseNetworkable entity);
    [ConsoleCommand("deletefrags")]
     void DeleteAllFrags(ConsoleSystem.Arg arg);
    [ConsoleCommand("AddConfig")]
     void AddConfig(ConsoleSystem.Arg arg);
    [ConsoleCommand("RemoveConfig")]
     void RemoveConfig(ConsoleSystem.Arg arg);
}


```

---

## BlueprintChecker

```csharp
using System;
using System.Collections.Generic;
using Oxide.Core;

Oxide.Plugins
[ Info(        "Blueprint Checker", "Dablin", "1.0.6"                                     )]
[ Description( "Check how many blueprints an online player has learnt and what they are." )]
 class BlueprintChecker : RustPlugin
{
    private const string CREDIT_TITLE;
    private const string CREDIT_AUTHOR;
    private const string CREDIT_VERSION;
    private const string CREDIT_EMAIL;
    private const string CREDIT_WEBSITE;
    private const string CREDIT_BY;
    private const string CREDIT_V;
    private const string CREDIT_OPEN;
    private const string CREDIT_CLOSE;
    private string ARG_ListInScon;
    private string ARG_ListInChat;
    private string ARG_Help;
    private string ARG_Knows;
    private string CDV;
    private int CFG_AuthorityLevelRequired;
    private bool CFG_UpdatePlayerOnConnect;
    private bool CFG_UpdatePlayerOnStudy;
    private bool CFG_UpdatePlayerVerbose;
    private string MSG_AccessDenied;
    private string MSG_CommandLine;
    private string MSG_Help;
    private string MSG_Knows;
    private string MSG_KnowsThis;
    private string MSG_KnowsCount;
    private string MSG_KnowsNot;
    private string MSG_LogWithArg;
    private string MSG_LogNoArg;
    private string MSG_MultiplePlayersFound;
    private string MSG_PlayerNotFound;
    private string MSG_What;
    private List<int> Player_BPKeys;
    private string Player_ID;
    private int Player_KnownBP;
    private string Player_Name;
    private int BPMax;
    private bool ConfigMismatch;
    private bool ConfigMissing;
    private string CRNL;
    private bool PlayerOfflineData;
    private BasePlayer PlayerChecker;
    private BasePlayer PlayerToCheck;
    private Data PlayerBPData;
    private string SPC;
    private int SteamIDLength;
    private const string DBG_PLAYERBPKEYS;
    private const string DBG_MAXBP;
    private bool DEBUG;
     class Data
    {
        public string Version;
        public List< PlayerData > PlayerBPData;
        public Data();
    }

     class PlayerData
    {
        public string UserID;
        public string UserName;
        public int KnownBP;
        public List<int> BlueprintKeys;
        public PlayerData();
    }

    private bool AuthenticatedPlayer(BasePlayer PLAYER, int AUTHORITYLEVEL);
    [ ChatCommand( "bpcheck" )]
    private void BPCheck(BasePlayer PLAYER, string COMMAND, string[] ARGS);
    private void Loaded();
    private int CountBP();
    private string Credit();
    private string CrNl(int LINENUMBER);
    private void Debug(string MESSAGE);
    private string GetBPKeyState(BasePlayer PLAYER, List<int> STATEKEYRING);
    private object GetConfig(string KEY, object DEFAULTVALUE);
    private BasePlayer GetPlayer(string PLAYERTOCHECK, BasePlayer PLAYERCHECKER);
    private void OnConsumableUse(Item ITEM);
    private void OnPlayerInit(BasePlayer PLAYER);
    private int PlayerBPCheck(string PLAYERID, BasePlayer PLAYERCHECKER, string BLUEPRINTID, string BPLONGNAME);
    private int PlayerBPCount(string PLAYERID, BasePlayer PLAYERCHECKER);
    private string PlayerBPList(string PLAYERID, BasePlayer PLAYERCHECKER);
    private void PrintToChat(BasePlayer PLAYERCHECKER, string MESSAGE);
    private void OnServerInitialized();
    private void UpdateDatabase(string PLAYERID, string PLAYERNAME);
    private void UpdateKeyring(BasePlayer PLAYER, List<int> STATEKEYRING);
    private void UpdatePlayer(BasePlayer PLAYER);
    protected override void LoadDefaultConfig();
}

 class Data
{
    public string Version;
    public List< PlayerData > PlayerBPData;
    public Data();
}

 class PlayerData
{
    public string UserID;
    public string UserName;
    public int KnownBP;
    public List<int> BlueprintKeys;
    public PlayerData();
}


```

---

## BluePrinter

```csharp
using System;
using System.Collections.Generic;
using System.Globalization;
using System.Text.RegularExpressions;
using Oxide.Core;
using Oxide.Core.Plugins;
using Oxide.Core.Libraries;

Oxide.Plugins
[Info("BluePrinter", "mk_sky", "1.0.7", ResourceId = 1343)]
[Description("Allows producing blueprints your char knows, needs paper or blueprintparts.")]
 class BluePrinter : RustPlugin
{
     ListDictionary<Rust.Rarity, int> paperNeeded;
     ListDictionary<Rust.Rarity, int> blueprintPartsNeeded;
     ListDictionary<Rust.Rarity, int> drawTimes;
     ListDictionary<Rust.Rarity, int> drawTimeModifier;
     ListDictionary<string, string> localization;
     ListDictionary<string, string> itemAlias;
     bool cancelBPWhenDead;
     bool paperUsageAllowed;
     bool blueprintPartsUsageAllowed;
     bool drawTimeModifierEnabled;
     bool mulitUseBPsAllowed;
     bool popupsEnabled;
    [PluginReference]
     Plugin PopupNotifications;
     void OnServerInitialized();
    protected override void LoadDefaultConfig();
     void ConfigLoader();
     void ConfigUpdater();
    [ConsoleCommand("blueconf.recreate")]
     void ConsoleCommandConfigRecreate();
    [ConsoleCommand("blueconf.load")]
     void ConsoleCommandConfigLoad();
    [ConsoleCommand("blueconf.set")]
     void ConsoleCommandConfigSet(ConsoleSystem.Arg arg);
    [ChatCommand("bluehelp")]
     void ChatCommandHelp(BasePlayer player);
    [ChatCommand("blueprinter")]
     void ChatCommandBluePrinter(BasePlayer player, string command, string[] args);
    [ChatCommand("debug")]
     void ChatCommandDebug(BasePlayer player);
     void OnEntityDeath(BaseCombatEntity entity, HitInfo hitInfo);
     void TimedBluePrint(string playerID);
    static bool IsUInt(string s);
}


```

---

## BlueprintManager

```csharp
using System;
using System.Collections.Generic;
using System.Globalization;
using System.Linq;
using System.Reflection;
using Oxide.Core;
using Rust;
using RustNative;
using UnityEngine;

Oxide.Plugins
[Info("BlueprintManager", "Nogrod", "1.2.0", ResourceId = 833)]
 class BlueprintManager : RustPlugin
{
    private Dictionary<string, string> _itemShortname;
    private int _authLevel;
    private int _authLevelOther;
    private bool _giveOnConnect;
    private bool _configChanged;
    private List<ItemDefinition> _giveBps;
    private Dictionary<ulong, HashSet<string>> playerLearned;
     void OnServerInitialized();
    new void LoadDefaultConfig();
     void OnPlayerInit(BasePlayer player);
     bool CheckAccess(BasePlayer player, int authLevel, ConsoleSystem.Arg arg);
    private T GetConfig(string key, T defaultValue);
    [ConsoleCommand("bp.print")]
     void cmdConsoleBpPrint(ConsoleSystem.Arg arg);
    [ConsoleCommand("bp.add")]
     void cmdConsoleBpAdd(ConsoleSystem.Arg arg);
    [ChatCommand("bpadd")]
     void cmdChatBpAdd(BasePlayer player, string command, string[] args);
     void BpAdd(BasePlayer player, string[] args, ConsoleSystem.Arg arg);
    [ConsoleCommand("bp.all")]
     void cmdConsoleBpAll(ConsoleSystem.Arg arg);
    [ChatCommand("bpall")]
     void cmdChatBpAll(BasePlayer player, string command, string[] args);
     void BpAll(BasePlayer player, string[] args, ConsoleSystem.Arg arg);
    [ConsoleCommand("bp.reset")]
     void cmdConsoleBpReset(ConsoleSystem.Arg arg);
    [ChatCommand("bpreset")]
     void cmdChatBpReset(BasePlayer player, string command, string[] args);
     void BpReset(BasePlayer player, string[] args, ConsoleSystem.Arg arg);
    [ConsoleCommand("bp.addall")]
     void cmdConsoleBpAddAll(ConsoleSystem.Arg arg);
    [ChatCommand("bpaddall")]
     void cmdChatBpAddAll(BasePlayer player, string command, string[] args);
     void BpAddAll(BasePlayer player, string[] args, ConsoleSystem.Arg arg);
    [ConsoleCommand("bp.remove")]
     void cmdConsoleBpRemove(ConsoleSystem.Arg arg);
    [ChatCommand("bpremove")]
     void cmdChatBpRemove(BasePlayer player, string command, string[] args);
     void BpRemove(BasePlayer player, string[] args, ConsoleSystem.Arg arg);
    [ConsoleCommand("bp.removeall")]
     void cmdConsoleBpRemoveAll(ConsoleSystem.Arg arg);
    [ChatCommand("bpremoveall")]
     void cmdChatBpRemoveAll(BasePlayer player, string command, string[] args);
     void BpRemoveAll(BasePlayer player, string[] args, ConsoleSystem.Arg arg);
    [ConsoleCommand("bp.clean")]
     void cmdConsoleBpClean(ConsoleSystem.Arg arg);
    [ChatCommand("bpclean")]
     void cmdChatBpClean(BasePlayer player, string command, string[] args);
     void BpClean(BasePlayer player, string[] args, ConsoleSystem.Arg arg);
    [ConsoleCommand("bp.save")]
     void cmdConsoleBpSave(ConsoleSystem.Arg arg);
    [ChatCommand("bpsave")]
     void cmdChatBpSave(BasePlayer player, string command, string[] args);
     void BpSave(BasePlayer player, string[] args, ConsoleSystem.Arg arg);
    [ConsoleCommand("bp.load")]
     void cmdConsoleBpLoad(ConsoleSystem.Arg arg);
    [ChatCommand("bpload")]
     void cmdChatBpLoad(BasePlayer player, string command, string[] args);
     void BpLoad(BasePlayer player, string[] args, ConsoleSystem.Arg arg);
    [ChatCommand("remember")]
     void cmdChatRemember(BasePlayer player, string command, string[] args);
     void Reply(BasePlayer player, ConsoleSystem.Arg arg, string format, object[] args);
    private static void Learn(ulong persistentPlayerId, IEnumerable<ItemDefinition> itemDefs);
    private static HashSet<string> GetLearned(ulong persistentPlayerId);
    private void DeletePersistentPlayersExcept(List<ulong> players);
    private ulong[] GetAllPersistentPlayerId();
    private static BasePlayer FindPlayer(string nameOrIdOrIp);
}


```

---

## BlueprintShare

```csharp
using System;
using System.Collections.Generic;
using System.Globalization;
using System.IO;
using System.Linq;
using Network;
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;
using Oxide.Core;
using Oxide.Core.Libraries;
using Oxide.Core.Libraries.Covalence;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using Rust;
using UnityEngine;
using VLB;

Oxide.Plugins
[Info("Blueprint Share", "Sempai#3239", "1.0.0")]
[Description("https://rustworkshop.space/resources/blueprint-share.245/")]
public class BlueprintShare : RustPlugin
{
    private void Init();
    private void OnTechTreeNodeUnlocked(Workbench workbench, TechTreeData.NodeInstance node, BasePlayer player);
    private void OnPlayerStudyBlueprint(BasePlayer player, Item item);
    private void OnLearned(BasePlayer player, ItemDefinition definition);
    private void Unlock(ulong idLong, ItemDefinition targetItem, string referrer);
    private List<ulong> GetAllMates(BasePlayer player);
    private static ConfigDefinition config;
    private class ConfigDefinition
    {
        [JsonProperty("Blocked shortnames")]
        public string[] blockedShortnames;
        [JsonProperty("Share with Team")]
        public bool shareTeam;
        [JsonProperty("Share with Clan")]
        public bool shareClan;
        [JsonProperty("Share with Friends")]
        public bool shareFriends;
        [JsonProperty("Share physical blueprints research")]
        public bool shareBlueprints;
        [JsonProperty("Share tech tree research")]
        public bool shareTechTree;
        [JsonProperty("Sender ID")]
        public ulong senderId;
    }

    private partial class Message
    {
        private static Dictionary<Key, object> messages;
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

    [PluginReference]
    private Plugin Friends;
    private Plugin RustIOFriendListAPI;
    private bool IsFriends(BasePlayer player1, BasePlayer player2);
    private bool IsFriends(BasePlayer player1, ulong player2);
    private bool IsFriends(ulong player1, BasePlayer player2);
    private bool IsFriends(ulong id1, ulong id2);
    private string[] GetFriends(string playerID);
    private static bool InSameTeam(BasePlayer player1, BasePlayer player2);
    private static bool InSameTeam(BasePlayer player1, ulong player2);
    private static bool InSameTeam(ulong player1, BasePlayer player2);
    [PluginReference]
    private Plugin Clans;
    private string GetPlayerClan(BasePlayer player);
    private string GetPlayerClan(ulong playerID);
    private string GetPlayerClan(string playerID);
    private bool InSameClan(BasePlayer player1, BasePlayer player2);
    private bool InSameClan(ulong player1, ulong player2);
    private bool InSameClan(BasePlayer player1, ulong player2);
    private bool InSameClan(ulong player1, BasePlayer player2);
    private bool IsClanOwner(string name, string playerID);
    private bool IsClanModerator(string name, string playerID);
    private ulong[] GetClanPlayers(string name);
}

private class ConfigDefinition
{
    [JsonProperty("Blocked shortnames")]
    public string[] blockedShortnames;
    [JsonProperty("Share with Team")]
    public bool shareTeam;
    [JsonProperty("Share with Clan")]
    public bool shareClan;
    [JsonProperty("Share with Friends")]
    public bool shareFriends;
    [JsonProperty("Share physical blueprints research")]
    public bool shareBlueprints;
    [JsonProperty("Share tech tree research")]
    public bool shareTechTree;
    [JsonProperty("Sender ID")]
    public ulong senderId;
}

private partial class Message
{
    private static Dictionary<Key, object> messages;
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

## BoostSystem

```csharp
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Configuration;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using System;
using System.Collections.Generic;
using System.Text;
using UnityEngine;

Oxide.Plugins
[Info("BoostSystem", "Fanatey", "0.0.4")]
 class BoostSystem : RustPlugin
{
    [PluginReference]
     Plugin IQEconomic;
     Plugin ImageLibrary;
    public void apiremovebalance(ulong userID, int Balance);
    public bool apiisremoved(ulong userID, int Amount);
    private List<string> ListGold;
     int ammounttoupgrade1;
     int ammounttoupgrade2;
     int ammounttoupgrade3;
     float healbost1;
     float healbost2;
     float healbost3;
     float damagebost1;
     float damagebost2;
     float damagebost3;
     float protectdamage1;
     float protectdamage2;
     float protectdamage3;
     bool wipedData;
     bool Economic;
     int itemid;
     int dropchance;
     string projectname;
     Timer heall;
     Timer damagee;
     Timer takedamagee;
    private string NameGold;
     int skinid;
     string shortname;
    protected override void LoadDefaultConfig();
     Dictionary<ulong, Dictionary<string, string>> boost;
    private List<LootContainer> handledContainers;
     void OnLootEntity(BasePlayer player, BaseEntity entity, Item item);
     void OnEntityDeath(BaseCombatEntity entity, HitInfo info, Item item);
     void OnItemAddedToContainer(ItemContainer container, Item item);
     void cuimenu(BasePlayer player);
     void healbar(BasePlayer player);
     void damagebar(BasePlayer player);
     void takedamagebar(BasePlayer player);
     void DestroyTimer(Timer timer);
    [JsonProperty("Системный слой")]
    private string Layer;
    [ConsoleCommand("wp")]
     void cmdvp(ConsoleSystem.Arg arg);
     object OnRunPlayerMetabolism(PlayerMetabolism metabolism, BaseCombatEntity entity);
    [ChatCommand("boost")]
     void Bosst(BasePlayer player);
    [ConsoleCommand("cmdbostheal")]
     void cmdboostheal(ConsoleSystem.Arg arg);
    [ConsoleCommand("cmdbostdamage")]
     void cmdbostdamage(ConsoleSystem.Arg arg);
    [ConsoleCommand("cmdbosttakedamage")]
     void cmdbosttakedamage(ConsoleSystem.Arg arg);
     void OnEntityTakeDamage(BaseCombatEntity entity, HitInfo hitInfo);
     T GetConfig(string name, T defaultValue);
    public static void GetVariable(DynamicConfigFile config, string name, T value, T defaultValue);
     DynamicConfigFile boostfile;
     void LoadData();
     void SaveData();
     void OnServerInitialized();
     void Unload();
    private void GetNewSave();
     void WipeData();
}


```

---

## BotSpawn

```csharp
using System;
using System.IO;
using System.Collections.Generic;
using System.Linq;
using Oxide.Core;
using Oxide.Core.Plugins;
using UnityEngine;
using Oxide.Game.Rust;
using System.Globalization;
using Newtonsoft.Json.Linq;
using Newtonsoft.Json;
using UnityEngine.SceneManagement;
using Facepunch;
using Rust;

Oxide.Plugins
[Info("BotSpawn", "Steenamaroo", "1.6.8", ResourceId = 2580)]
[Description("Spawn tailored AI with kits at monuments, custom locations, or randomly.")]
 class BotSpawn : RustPlugin
{
    [PluginReference]
     Plugin Vanish;
     Plugin Kits;
    const string permAllowed;
     bool HasPermission(string id, string perm);
     int no_of_AI;
    static System.Random random;
    public Dictionary<string, List<Vector3>> spawnLists;
    public Timer aridTimer;
    public Timer temperateTimer;
    public Timer tundraTimer;
    public Timer arcticTimer;
     bool isBiome(string name);
     class StoredData
    {
        public Dictionary<string, DataProfile> DataProfiles;
        public Dictionary<string, ProfileRelocation> MigrationDataDoNotEdit;
        public StoredData();
    }

     class StoredDataOld
    {
        public Dictionary<string, MonumentSettings> CustomProfiles;
        public StoredDataOld();
    }

    public class MonumentSettings
    {
        public bool AutoSpawn;
        public bool Murderer;
        public int Bots;
        public int BotHealth;
        public int Radius;
        public List<string> Kit;
        public string BotNamePrefix;
        public List<string> BotNames;
        public int Bot_Accuracy;
        public float Bot_Damage;
        public int Respawn_Timer;
        public bool Disable_Radio;
        public float LocationX;
        public float LocationY;
        public float LocationZ;
        public int Roam_Range;
        public bool Peace_Keeper;
        public int Peace_Keeper_Cool_Down;
        public bool Weapon_Drop;
        public bool Keep_Default_Loadout;
        public bool Wipe_Belt;
        public bool Wipe_Clothing;
        public bool Allow_Rust_Loot;
        public int Suicide_Timer;
        public bool Chute;
        public int Long_Attack_Distance;
    }

     StoredDataOld storedDataOld;
     StoredData storedData;
     void Init();
     void OnServerInitialized();
     void Loaded();
     void Unload();
     bool isAuth(BasePlayer player);
     void UpdateRecords(NPCPlayerApex player);
     void Wipe();
    public static string Get(ulong v);
     void GenerateSpawnPoints(List<Vector3> spawnlist, string name, int number, Timer myTimer, int biomeNo);
    public static Vector3 CalculateGroundPos(Vector3 sourcePos, bool Biome);
     Vector3 TryGetSpawn(Vector3 centerPoint, int radius);
     void AttackPlayer(Vector3 location, string name, DataProfile profile, string group);
     void SpawnBots(string name, DataProfile zone, string type, string group, Vector3 location);
     BaseEntity InstantiateSci(Vector3 position, Quaternion rotation, bool murd);
     void addChute(NPCPlayerApex botapex, Vector3 newPos);
     void setName(DataProfile zone, NPCPlayerApex botapex, int number);
     void giveKit(NPCPlayerApex botapex, DataProfile zone, int kitRnd);
     void sortWeapons(NPCPlayerApex botapex);
     void runSuicide(NPCPlayerApex botapex, int suicInt);
    static BasePlayer FindPlayerByName(string name);
    private void OnEntityKill(BaseNetworkable entity);
     object OnEntityTakeDamage(BaseCombatEntity entity, HitInfo info);
     void OnPlayerDie(BasePlayer player);
     void OnEntitySpawned(BaseEntity entity);
     void SelectWeapon(NPCPlayerApex npcPlayer, BasePlayer victim, bool hasAttacker);
     void ChangeWeapon(NPCPlayer npcPlayer, HeldEntity held, uint UID);
     object OnNpcPlayerResume(NPCPlayerApex player);
     void OnNpcDestinationSet(NPCPlayerApex player);
     object OnNpcPlayerTarget(NPCPlayerApex npcPlayer, BaseEntity entity);
    private object RaycastAll(Ray ray, float distance);
     object CanBradleyApcTarget(BradleyAPC bradley, BaseEntity target);
     object OnNpcTarget(BaseNpc npc, BaseEntity entity);
     object CanBeTargeted(BaseCombatEntity player, MonoBehaviour turret);
    private void FindMonuments();
     void AddProfile(string name, ConfigProfile monument, Vector3 pos, float rotation);
    private Vector3 RotateVector2D(Vector3 oldDirection, float angle);
     void SaveData();
    [ConsoleCommand("bot.respawn")]
     void cmdBotRespawn();
    [ConsoleCommand("bot.count")]
     void cmdBotCount();
    [ChatCommand("botspawn")]
     void botspawn(BasePlayer player, string command, string[] args);
    public static List<ulong> DeadNPCPlayerIds;
    public static Dictionary<ulong, kitData> kitList;
    public static Dictionary<ulong, string> kitRemoveList;
    public static List<Vector3> smokeGrenades;
    public static List<ulong> aggroPlayers;
    public class kitData
    {
        public string Kit;
        public bool Wipe_Belt;
        public bool Wipe_Clothing;
        public bool Allow_Rust_Loot;
    }

    public class botData : MonoBehaviour
    {
        public Vector3 spawnPoint;
        public int canChangeWeapon;
        public float enemyDistance;
        public int currentWeaponRange;
        public List<Item> AllProjectiles;
        public List<Item> MeleeWeapons;
        public List<Item> CloseRangeWeapons;
        public List<Item> MediumRangeWeapons;
        public List<Item> LongRangeWeapons;
        public int accuracy;
        public float damage;
        public float range;
        public int health;
        public string monumentName;
        public bool dropweapon;
        public bool respawn;
        public bool biome;
        public int roamRange;
        public bool goingHome;
        public bool keepAttire;
        public bool peaceKeeper;
        public string group;
        public bool chute;
        public bool inAir;
        public int LongRangeAttack;
        public int peaceKeeper_CoolDown;
         int landingAttempts;
         Vector3 landingDirection;
         int updateCounter;
         NPCPlayerApex botapex;
         void Start();
        private void OnCollisionEnter(Collision collision);
         void Update();
    }

    private ConfigData configData;
     class TempRecord
    {
        public static List<NPCPlayerApex> NPCPlayers;
        public static Dictionary<string, DataProfile> AllProfiles;
    }

     class Global
    {
        public bool NPCs_Attack_BotSpawn;
        public bool BotSpawn_Attacks_NPCs;
        public bool BotSpawn_Attacks_BotSpawn;
        public bool Ignore_Animals;
        public bool APC_Safe;
        public bool Turret_Safe;
        public bool Animal_Safe;
        public bool Supply_Enabled;
        public bool Remove_BackPacks;
        public bool Ignore_HumanNPC;
        public bool Pve_Safe;
        public int Corpse_Duration;
    }

    public class Monuments
    {
        public AirDropProfile AirDrop;
        public ConfigProfile Airfield;
        public ConfigProfile Dome;
        public ConfigProfile Compound;
        public ConfigProfile Compound1;
        public ConfigProfile Compound2;
        public ConfigProfile GasStation;
        public ConfigProfile GasStation1;
        public ConfigProfile Harbor1;
        public ConfigProfile Harbor2;
        public ConfigProfile Junkyard;
        public ConfigProfile Launchsite;
        public ConfigProfile Lighthouse;
        public ConfigProfile Lighthouse1;
        public ConfigProfile Lighthouse2;
        public ConfigProfile MilitaryTunnel;
        public ConfigProfile PowerPlant;
        public ConfigProfile QuarrySulphur;
        public ConfigProfile QuarryStone;
        public ConfigProfile QuarryHQM;
        public ConfigProfile SuperMarket;
        public ConfigProfile SuperMarket1;
        public ConfigProfile Radtown;
        public ConfigProfile Satellite;
        public ConfigProfile Trainyard;
        public ConfigProfile Warehouse;
        public ConfigProfile Warehouse1;
        public ConfigProfile Warehouse2;
        public ConfigProfile Watertreatment;
    }

    public class Biomes
    {
        public ConfigProfile BiomeArid;
        public ConfigProfile BiomeTemperate;
        public ConfigProfile BiomeTundra;
        public ConfigProfile BiomeArctic;
    }

    public class AirDropProfile
    {
        public bool AutoSpawn;
        public bool Murderer;
        public int Bots;
        public int BotHealth;
        public int Radius;
        public List<string> Kit;
        public string BotNamePrefix;
        public List<string> BotNames;
        public int Bot_Accuracy;
        public float Bot_Damage;
        public bool Disable_Radio;
        public int Roam_Range;
        public bool Peace_Keeper;
        public int Peace_Keeper_Cool_Down;
        public bool Weapon_Drop;
        public bool Keep_Default_Loadout;
        public bool Wipe_Belt;
        public bool Wipe_Clothing;
        public bool Allow_Rust_Loot;
        public int Suicide_Timer;
        public bool Chute;
        public int Long_Attack_Distance;
    }

    public class ConfigProfile
    {
        public bool AutoSpawn;
        public bool Murderer;
        public int Bots;
        public int BotHealth;
        public int Radius;
        public List<string> Kit;
        public string BotNamePrefix;
        public List<string> BotNames;
        public int Bot_Accuracy;
        public float Bot_Damage;
        public bool Disable_Radio;
        public int Roam_Range;
        public bool Peace_Keeper;
        public int Peace_Keeper_Cool_Down;
        public bool Weapon_Drop;
        public bool Keep_Default_Loadout;
        public bool Wipe_Belt;
        public bool Wipe_Clothing;
        public bool Allow_Rust_Loot;
        public int Suicide_Timer;
        public bool Chute;
        public int Long_Attack_Distance;
        public int Respawn_Timer;
    }

    public class DataProfile
    {
        public bool AutoSpawn;
        public bool Murderer;
        public int Bots;
        public int BotHealth;
        public int Radius;
        public List<string> Kit;
        public string BotNamePrefix;
        public List<string> BotNames;
        public int Bot_Accuracy;
        public float Bot_Damage;
        public bool Disable_Radio;
        public int Roam_Range;
        public bool Peace_Keeper;
        public int Peace_Keeper_Cool_Down;
        public bool Weapon_Drop;
        public bool Keep_Default_Loadout;
        public bool Wipe_Belt;
        public bool Wipe_Clothing;
        public bool Allow_Rust_Loot;
        public int Suicide_Timer;
        public bool Chute;
        public int Long_Attack_Distance;
        public int Respawn_Timer;
        public float LocationX;
        public float LocationY;
        public float LocationZ;
        public string Parent_Monument;
    }

    public class ProfileRelocation
    {
        public float OldParentMonumentX;
        public float OldParentMonumentY;
        public float OldParentMonumentZ;
        public float ParentMonumentX;
        public float ParentMonumentY;
        public float ParentMonumentZ;
        public float oldRotation;
        public float worldRotation;
    }

     class ConfigData
    {
        public Global Global;
        public Monuments Monuments;
        public Biomes Biomes;
    }

     class OldConfigData
    {
        public Options Options;
        public Monuments Zones;
    }

     class Options
    {
        public bool NPCs_Attack_BotSpawn;
        public bool BotSpawn_Attacks_NPCs;
        public bool BotSpawn_Attacks_BotSpawn;
        public bool Ignore_Animals;
        public bool APC_Safe;
        public bool Turret_Safe;
        public bool Animal_Safe;
        public bool Supply_Enabled;
        public bool Remove_BackPacks;
        public bool Ignore_HumanNPC;
        public bool Pve_Safe;
        public int Corpse_Duration;
    }

    private void LoadConfigVariables();
    protected override void LoadDefaultConfig();
     void SaveConfig(ConfigData config);
     Dictionary<string, string> messages;
    [HookMethod("AddGroupSpawn")]
    public string[] AddGroupSpawn(Vector3 location, string profileName, string group);
    [HookMethod("RemoveGroupSpawn")]
    public string[] RemoveGroupSpawn(string group);
    [HookMethod("CreateNewProfile")]
    public string[] CreateNewProfile(string name, string profile);
    [HookMethod("ProfileExists")]
    public string[] ProfileExists(string name);
    [HookMethod("RemoveProfile")]
    public string[] RemoveProfile(string name);
}

 class StoredData
{
    public Dictionary<string, DataProfile> DataProfiles;
    public Dictionary<string, ProfileRelocation> MigrationDataDoNotEdit;
    public StoredData();
}

 class StoredDataOld
{
    public Dictionary<string, MonumentSettings> CustomProfiles;
    public StoredDataOld();
}

public class MonumentSettings
{
    public bool AutoSpawn;
    public bool Murderer;
    public int Bots;
    public int BotHealth;
    public int Radius;
    public List<string> Kit;
    public string BotNamePrefix;
    public List<string> BotNames;
    public int Bot_Accuracy;
    public float Bot_Damage;
    public int Respawn_Timer;
    public bool Disable_Radio;
    public float LocationX;
    public float LocationY;
    public float LocationZ;
    public int Roam_Range;
    public bool Peace_Keeper;
    public int Peace_Keeper_Cool_Down;
    public bool Weapon_Drop;
    public bool Keep_Default_Loadout;
    public bool Wipe_Belt;
    public bool Wipe_Clothing;
    public bool Allow_Rust_Loot;
    public int Suicide_Timer;
    public bool Chute;
    public int Long_Attack_Distance;
}

public class kitData
{
    public string Kit;
    public bool Wipe_Belt;
    public bool Wipe_Clothing;
    public bool Allow_Rust_Loot;
}

public class botData : MonoBehaviour
{
    public Vector3 spawnPoint;
    public int canChangeWeapon;
    public float enemyDistance;
    public int currentWeaponRange;
    public List<Item> AllProjectiles;
    public List<Item> MeleeWeapons;
    public List<Item> CloseRangeWeapons;
    public List<Item> MediumRangeWeapons;
    public List<Item> LongRangeWeapons;
    public int accuracy;
    public float damage;
    public float range;
    public int health;
    public string monumentName;
    public bool dropweapon;
    public bool respawn;
    public bool biome;
    public int roamRange;
    public bool goingHome;
    public bool keepAttire;
    public bool peaceKeeper;
    public string group;
    public bool chute;
    public bool inAir;
    public int LongRangeAttack;
    public int peaceKeeper_CoolDown;
     int landingAttempts;
     Vector3 landingDirection;
     int updateCounter;
     NPCPlayerApex botapex;
     void Start();
    private void OnCollisionEnter(Collision collision);
     void Update();
}

 class TempRecord
{
    public static List<NPCPlayerApex> NPCPlayers;
    public static Dictionary<string, DataProfile> AllProfiles;
}

 class Global
{
    public bool NPCs_Attack_BotSpawn;
    public bool BotSpawn_Attacks_NPCs;
    public bool BotSpawn_Attacks_BotSpawn;
    public bool Ignore_Animals;
    public bool APC_Safe;
    public bool Turret_Safe;
    public bool Animal_Safe;
    public bool Supply_Enabled;
    public bool Remove_BackPacks;
    public bool Ignore_HumanNPC;
    public bool Pve_Safe;
    public int Corpse_Duration;
}

public class Monuments
{
    public AirDropProfile AirDrop;
    public ConfigProfile Airfield;
    public ConfigProfile Dome;
    public ConfigProfile Compound;
    public ConfigProfile Compound1;
    public ConfigProfile Compound2;
    public ConfigProfile GasStation;
    public ConfigProfile GasStation1;
    public ConfigProfile Harbor1;
    public ConfigProfile Harbor2;
    public ConfigProfile Junkyard;
    public ConfigProfile Launchsite;
    public ConfigProfile Lighthouse;
    public ConfigProfile Lighthouse1;
    public ConfigProfile Lighthouse2;
    public ConfigProfile MilitaryTunnel;
    public ConfigProfile PowerPlant;
    public ConfigProfile QuarrySulphur;
    public ConfigProfile QuarryStone;
    public ConfigProfile QuarryHQM;
    public ConfigProfile SuperMarket;
    public ConfigProfile SuperMarket1;
    public ConfigProfile Radtown;
    public ConfigProfile Satellite;
    public ConfigProfile Trainyard;
    public ConfigProfile Warehouse;
    public ConfigProfile Warehouse1;
    public ConfigProfile Warehouse2;
    public ConfigProfile Watertreatment;
}

public class Biomes
{
    public ConfigProfile BiomeArid;
    public ConfigProfile BiomeTemperate;
    public ConfigProfile BiomeTundra;
    public ConfigProfile BiomeArctic;
}

public class AirDropProfile
{
    public bool AutoSpawn;
    public bool Murderer;
    public int Bots;
    public int BotHealth;
    public int Radius;
    public List<string> Kit;
    public string BotNamePrefix;
    public List<string> BotNames;
    public int Bot_Accuracy;
    public float Bot_Damage;
    public bool Disable_Radio;
    public int Roam_Range;
    public bool Peace_Keeper;
    public int Peace_Keeper_Cool_Down;
    public bool Weapon_Drop;
    public bool Keep_Default_Loadout;
    public bool Wipe_Belt;
    public bool Wipe_Clothing;
    public bool Allow_Rust_Loot;
    public int Suicide_Timer;
    public bool Chute;
    public int Long_Attack_Distance;
}

public class ConfigProfile
{
    public bool AutoSpawn;
    public bool Murderer;
    public int Bots;
    public int BotHealth;
    public int Radius;
    public List<string> Kit;
    public string BotNamePrefix;
    public List<string> BotNames;
    public int Bot_Accuracy;
    public float Bot_Damage;
    public bool Disable_Radio;
    public int Roam_Range;
    public bool Peace_Keeper;
    public int Peace_Keeper_Cool_Down;
    public bool Weapon_Drop;
    public bool Keep_Default_Loadout;
    public bool Wipe_Belt;
    public bool Wipe_Clothing;
    public bool Allow_Rust_Loot;
    public int Suicide_Timer;
    public bool Chute;
    public int Long_Attack_Distance;
    public int Respawn_Timer;
}

public class DataProfile
{
    public bool AutoSpawn;
    public bool Murderer;
    public int Bots;
    public int BotHealth;
    public int Radius;
    public List<string> Kit;
    public string BotNamePrefix;
    public List<string> BotNames;
    public int Bot_Accuracy;
    public float Bot_Damage;
    public bool Disable_Radio;
    public int Roam_Range;
    public bool Peace_Keeper;
    public int Peace_Keeper_Cool_Down;
    public bool Weapon_Drop;
    public bool Keep_Default_Loadout;
    public bool Wipe_Belt;
    public bool Wipe_Clothing;
    public bool Allow_Rust_Loot;
    public int Suicide_Timer;
    public bool Chute;
    public int Long_Attack_Distance;
    public int Respawn_Timer;
    public float LocationX;
    public float LocationY;
    public float LocationZ;
    public string Parent_Monument;
}

public class ProfileRelocation
{
    public float OldParentMonumentX;
    public float OldParentMonumentY;
    public float OldParentMonumentZ;
    public float ParentMonumentX;
    public float ParentMonumentY;
    public float ParentMonumentZ;
    public float oldRotation;
    public float worldRotation;
}

 class ConfigData
{
    public Global Global;
    public Monuments Monuments;
    public Biomes Biomes;
}

 class OldConfigData
{
    public Options Options;
    public Monuments Zones;
}

 class Options
{
    public bool NPCs_Attack_BotSpawn;
    public bool BotSpawn_Attacks_NPCs;
    public bool BotSpawn_Attacks_BotSpawn;
    public bool Ignore_Animals;
    public bool APC_Safe;
    public bool Turret_Safe;
    public bool Animal_Safe;
    public bool Supply_Enabled;
    public bool Remove_BackPacks;
    public bool Ignore_HumanNPC;
    public bool Pve_Safe;
    public int Corpse_Duration;
}


```

---

## Bounty

```csharp
using System.Collections.Generic;
using System;
using Oxide.Core;
using Oxide.Core.Plugins;
using System.Linq;
using Oxide.Core.Configuration;
using UnityEngine;

Oxide.Plugins
[Info("Bounty", "k1lly0u", "0.1.73", ResourceId = 1649)]
 class Bounty : RustPlugin
{
    [PluginReference]
     Plugin Clans;
    [PluginReference]
     Plugin Friends;
    [PluginReference]
     Plugin PopupNotifications;
    [PluginReference]
     Plugin Economics;
    private bool Changed;
     BountyDataStorage bountyData;
    private DynamicConfigFile BountyData;
     RewardDataStorage rewardData;
    private DynamicConfigFile RewardData;
     PlayerTimeStamp playerTimeData;
    private Dictionary<string, string> itemInfo;
    private Dictionary<ulong, OpenBox> rewardBoxs;
     string rBox;
     void Loaded();
     void OnServerInitialized();
     void CheckDependencies();
     void OnPlayerInit(BasePlayer player);
    protected override void LoadDefaultConfig();
     void Unload();
     void OnEntityDeath(BaseCombatEntity entity, HitInfo info);
     void OnPlayerLootEnd(PlayerLoot inventory);
     void addBounty(BasePlayer player, BasePlayer target, string item, int amount);
     void StoreRewardData(BasePlayer player);
    private void notifyBounty(BasePlayer player, BasePlayer target);
    private void SendMSG(BasePlayer player, string msg);
    private bool checkExisting(BasePlayer player, BasePlayer target);
    private bool itemCosts(BasePlayer player, string item, int amount);
    private void recordEarnings(BasePlayer player, BasePlayer victim);
    private bool claimBounty(BasePlayer player, int ID);
    private void InitializeTable();
    private object GiveItem(BasePlayer player, string itemname, int amount, ItemContainer pref);
     List<BasePlayer> FindPlayer(string arg);
     List<string> FindItem(string arg);
    private bool IsClanmate(ulong playerId, ulong friendId);
    private bool IsFriend(BasePlayer player, ulong friendID);
    private void RemindPlayers();
    private void SendPopup(BasePlayer player, string msg);
    private bool CheckPlayerMoney(BasePlayer player, int amount);
    private bool RewardMoney(BasePlayer player, double amount);
    private void initTimestamp(BasePlayer player);
    private void calculateTimestamp(BasePlayer player);
    private long getCurrentTime();
    [ChatCommand("bounty")]
    private void cmdBounty(BasePlayer player, string command, string[] args);
    [ConsoleCommand("bounty.wipe")]
     void ccmdbWipe(ConsoleSystem.Arg arg);
    [ConsoleCommand("bounty.list")]
     void ccmdbList(ConsoleSystem.Arg arg);
     bool canBountyPlayer(BasePlayer player);
     bool canBountyAdmin(BasePlayer player);
     bool isBanned(BasePlayer player);
     bool isAuth(BasePlayer player);
     bool isAuthCon(ConsoleSystem.Arg arg);
    static int auth;
    static bool useClans;
    static bool useFriendsAPI;
    static bool usePopup;
    static bool useReminders;
    static bool useEconomics;
    static bool globalBroadcast;
    static bool useEconomyOnly;
    static int saveLoop;
    static int popupTime;
    static int remindTime;
    static string mainColor;
    static string msgColor;
    private void LoadVariables();
    private void LoadConfigVariables();
    private void CheckCfg(string Key, T var);
    private void CheckCfgFloat(string Key, float var);
     object GetConfig(string menu, string datavalue, object defaultValue);
     void SaveData();
     void SaveDataLoop();
     void LoadData();
     class BountyDataStorage
    {
        public Dictionary<ulong, PlayerData> players;
        public BountyDataStorage();
    }

     class PlayerData
    {
        public ulong PlayerID;
        public string PlayerName;
        public int TotalBountys;
        public long TotalWantedSec;
        public string TotalWantedClock;
        public Dictionary<ulong, BountyInfo> Bountys;
        public PlayerData();
        public PlayerData(string name, ulong id);
    }

     class BountyInfo
    {
        public string InitiatorName;
        public List<ItemStorage> BountyItems;
        public long WantedTime;
        public string WantedTimeClock;
        public BountyInfo();
        public BountyInfo(ulong playerid, string playername, List<ItemStorage> items);
    }

     class RewardDataStorage
    {
        public Dictionary<ulong, RewardInfo> rewards;
        public RewardDataStorage();
    }

     class RewardInfo
    {
        public ulong PlayerID;
        public string PlayerName;
        public int TotalCount;
        public Dictionary<int, UnclaimedData> Rewards;
        public RewardInfo();
        public RewardInfo(ulong aid, string attackname);
    }

     class UnclaimedData
    {
        public ulong VictimID;
        public string VictimName;
        public List<ItemStorage> Rewards;
        public UnclaimedData();
    }

     class ItemStorage
    {
        public bool money;
        public int amount;
        public string itemname;
    }

     class OpenBox
    {
        public BasePlayer target;
        public BaseEntity entity;
    }

     class PlayerTimeStamp
    {
        public Dictionary<ulong, PlayerStateInfo> playerTime;
        public PlayerTimeStamp();
    }

     class PlayerStateInfo
    {
        public long InitTimeStamp;
        public PlayerStateInfo();
        public PlayerStateInfo(BasePlayer player);
    }

     Dictionary<string, string> messages;
}

 class BountyDataStorage
{
    public Dictionary<ulong, PlayerData> players;
    public BountyDataStorage();
}

 class PlayerData
{
    public ulong PlayerID;
    public string PlayerName;
    public int TotalBountys;
    public long TotalWantedSec;
    public string TotalWantedClock;
    public Dictionary<ulong, BountyInfo> Bountys;
    public PlayerData();
    public PlayerData(string name, ulong id);
}

 class BountyInfo
{
    public string InitiatorName;
    public List<ItemStorage> BountyItems;
    public long WantedTime;
    public string WantedTimeClock;
    public BountyInfo();
    public BountyInfo(ulong playerid, string playername, List<ItemStorage> items);
}

 class RewardDataStorage
{
    public Dictionary<ulong, RewardInfo> rewards;
    public RewardDataStorage();
}

 class RewardInfo
{
    public ulong PlayerID;
    public string PlayerName;
    public int TotalCount;
    public Dictionary<int, UnclaimedData> Rewards;
    public RewardInfo();
    public RewardInfo(ulong aid, string attackname);
}

 class UnclaimedData
{
    public ulong VictimID;
    public string VictimName;
    public List<ItemStorage> Rewards;
    public UnclaimedData();
}

 class ItemStorage
{
    public bool money;
    public int amount;
    public string itemname;
}

 class OpenBox
{
    public BasePlayer target;
    public BaseEntity entity;
}

 class PlayerTimeStamp
{
    public Dictionary<ulong, PlayerStateInfo> playerTime;
    public PlayerTimeStamp();
}

 class PlayerStateInfo
{
    public long InitTimeStamp;
    public PlayerStateInfo();
    public PlayerStateInfo(BasePlayer player);
}


```

---

## BountyNET

```csharp
using System;
using System.Collections.Generic;
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Configuration;
using Oxide.Game.Rust.Cui;
using Oxide.Core.Plugins;
using UnityEngine;
using System.Linq;
using Oxide.Core.Libraries.Covalence;

Oxide.Plugins
[Info("BountyNET", "k1lly0u", "0.1.8")]
[Description("A bounty system that can only be access via a RustNET terminal")]
 class BountyNET : RustPlugin
{
    [PluginReference]
     Plugin Clans;
     Plugin Friends;
     Plugin PopupNotifications;
     Plugin Economics;
     Plugin ServerRewards;
    private static BountyNET ins;
    private StoredData storedData;
    private OfflinePlayers offlinePlayers;
    private DynamicConfigFile data;
    private DynamicConfigFile offline;
    private Dictionary<ulong, ulong> bountyCreator;
    private Dictionary<StorageContainer, ulong> openContainers;
    private Dictionary<int, string> idToDisplayName;
    private string boxPrefab;
    private FriendManager friendManager;
    private void Loaded();
    private void OnServerInitialized();
    private void OnPlayerInit(BasePlayer player);
    private void OnPlayerDisconnected(BasePlayer player);
    private object OnEntityTakeDamage(BaseCombatEntity entity, HitInfo info);
    private void OnEntityDeath(BaseCombatEntity entity, HitInfo info);
    private object CanLootEntity(BasePlayer player, StorageContainer container);
    private void OnPlayerLootEnd(PlayerLoot inventory);
    private void OnServerSave();
    private void Unload();
    private void BroadcastToPlayer(BasePlayer player, string message);
    private void PopupToPlayer(BasePlayer player, string message, float duration);
    private void CreateNewBounty(BasePlayer initiator, ulong targetId, int rpAmount, int ecoAmount, ItemContainer container, bool notify);
    private void GivePlayerRewards(BasePlayer player, RewardInfo rewardInfo, bool notify);
    private Item CreateItem(RewardInfo.ItemData itemData);
    private void SpawnItemContainer(BasePlayer player);
    private void OpenInventory(BasePlayer player, StorageContainer container);
    private void ClearContainer(ItemContainer itemContainer);
    private int GetUniqueId();
    private double CurrentTime();
    private List<BasePlayer> FindPlayer(string partialNameOrId);
    private string FormatTime(double time);
    private string RemoveTag(string str);
    private string TrimToSize(string str);
    private string GetHelpString(ulong playerId, bool title);
    private bool AllowPublicAccess();
    private class FriendManager : MonoBehaviour
    {
        private List<FriendEntry> friends;
        private void Awake();
        public void OnFriendshipEnded(string playerId, string friendId);
        public bool WereFriends(string playerId, string friendId);
        public bool WereFriends(ulong playerId, ulong friendId);
        private void RemoveOldData();
    }

    public bool IsFriendlyPlayer(ulong playerId, ulong friendId);
    private bool IsClanmate(ulong playerId, ulong friendId);
    private bool IsFriend(ulong playerID, ulong friendID);
    private bool IsTeamMate(ulong playerId, ulong friendId);
    private void OnFriendRemoved(string playerId, string friendId);
    private void OnServerCommand(ConsoleSystem.Arg arg);
    private CuiElementContainer CreateBountyContainer(BasePlayer player, int terminalId, string title, string returnCommand);
    private void CreateConsoleWindow(BasePlayer player, int terminalId, int page);
    private void CreatePlayerSelectionMenu(BasePlayer player, int terminalId, int page, bool isOffline, string type);
    private void CreateNewAmountMenu(BasePlayer player, int terminalId, ulong targetId, int amount, bool isRp);
    private void CreateCancelBountyMenu(BasePlayer player, int terminalId, int page);
    private void CreateClaimMenu(BasePlayer player, int terminalId, int page);
    private void CreateBountyViewMenu(BasePlayer player, int terminalId, int page);
    private void CreateIndividualBountyView(BasePlayer player, int terminalId, ulong targetId, int page);
    private void CreateWantedMenu(BasePlayer player, int terminalId, int page);
    private void CreateHuntersMenu(BasePlayer player, int terminalId, int page);
    [ConsoleCommand("bounty.changepage")]
    private void ccmdChangePage(ConsoleSystem.Arg arg);
    [ConsoleCommand("bounty.claimreward")]
    private void ccmdClaimReward(ConsoleSystem.Arg arg);
    [ConsoleCommand("bounty.addbounty")]
    private void ccmdAddNewBounty(ConsoleSystem.Arg arg);
    [ConsoleCommand("bounty.cancelbounty")]
    private void ccmdClearBounty(ConsoleSystem.Arg arg);
    [ConsoleCommand("bounty.viewbounties")]
    private void ccmdViewBounties(ConsoleSystem.Arg arg);
    private float[] GetButtonPosition(int i);
    private int RowNumber(int max, int count);
    private void ClearBounties(BasePlayer player, int terminalId, int page, ulong targetId);
    [ConsoleCommand("bounty")]
    private void ccmdBounty(ConsoleSystem.Arg arg);
    private ConfigData configData;
    private class ConfigData
    {
        [JsonProperty(PropertyName = "Ignore kills by clan members")]
        public bool IgnoreClans { get; set; }
        [JsonProperty(PropertyName = "Ignore kills by friends")]
        public bool IgnoreFriends { get; set; }
        [JsonProperty(PropertyName = "Set a limit of how many active bounties a player can have at any time (set to 0 to disable the limit)")]
        public int SetLimit { get; set; }
        [JsonProperty(PropertyName = "Notification Options")]
        public NotificationOptions Notifications { get; set; }
        [JsonProperty(PropertyName = "Reward Options")]
        public RewardOptions Rewards { get; set; }
        public class NotificationOptions
        {
            [JsonProperty(PropertyName = "PopupNotifications - Broadcast using PopupNotifications")]
            public bool UsePopupNotifications { get; set; }
            [JsonProperty(PropertyName = "PopupNotifications - Duration of notification")]
            public float PopupDuration { get; set; }
            [JsonProperty(PropertyName = "Broadcast new bounties globally")]
            public bool BroadcastNewBounties { get; set; }
            [JsonProperty(PropertyName = "Reminders - Remind targets they have a bounty on them")]
            public bool ShowReminders { get; set; }
            [JsonProperty(PropertyName = "Reminders - Amount of time between reminders (in minutes)")]
            public int ReminderTime { get; set; }
        }

        public class RewardOptions
        {
            [JsonProperty(PropertyName = "Allow bounties to be placed using Economics")]
            public bool AllowEconomics { get; set; }
            [JsonProperty(PropertyName = "Allow bounties to be placed using RP")]
            public bool AllowServerRewards { get; set; }
            [JsonProperty(PropertyName = "Allow bounties to be placed using items")]
            public bool AllowItems { get; set; }
        }

        [JsonProperty(PropertyName = "Bounty icon URL for RustNET menu")]
        public string RustNETIcon { get; set; }
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
        public Dictionary<ulong, PlayerData> players;
        public Dictionary<int, RewardInfo> rewards;
    }

    private class PlayerData
    {
        public string displayName;
        public int totalBounties;
        public int bountiesClaimed;
        public double totalWantedTime;
        public List<BountyInfo> activeBounties;
        public List<int> unclaimedRewards;
        public PlayerData();
        public PlayerData(string displayName);
        public void ClaimRewards(List<int> rewards);
        public BountyInfo GetBountyOf(ulong initiatorId);
        public int GetTotalBounties();
        public void UpdateWantedTime();
        public double GetCurrentWantedTime();
        public class BountyInfo
        {
            public ulong initiatorId;
            public string initiatorName;
            public double initiatedTime;
            public int rewardId;
            public BountyInfo();
            public BountyInfo(ulong initiatorId, string initiatorName, int rewardId);
        }

    }

    private class RewardInfo
    {
        public int rpAmount;
        public int econAmount;
        public List<ItemData> rewardItems;
        public RewardInfo();
        public RewardInfo(int rpAmount, int econAmount, ItemContainer container);
        public string GetRewardString(ulong playerId);
        private IEnumerable<ItemData> GetItems(ItemContainer container);
        public class ItemData
        {
            public int itemid;
            public ulong skin;
            public int amount;
            public float condition;
            public int ammo;
            public string ammotype;
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

    }

    private class OfflinePlayers
    {
        public Hash<string, double> offlinePlayers;
        public void AddOfflinePlayer(string userId);
        public void OnPlayerInit(string userId);
        public void RemoveOldPlayers();
        public IPlayer[] GetOfflineList();
        public double CurrentTime();
    }

    private string msg(string key, ulong playerId);
     Dictionary<string, string> messages;
}

private class FriendManager : MonoBehaviour
{
    private List<FriendEntry> friends;
    private void Awake();
    public void OnFriendshipEnded(string playerId, string friendId);
    public bool WereFriends(string playerId, string friendId);
    public bool WereFriends(ulong playerId, ulong friendId);
    private void RemoveOldData();
}

private class ConfigData
{
    [JsonProperty(PropertyName = "Ignore kills by clan members")]
    public bool IgnoreClans { get; set; }
    [JsonProperty(PropertyName = "Ignore kills by friends")]
    public bool IgnoreFriends { get; set; }
    [JsonProperty(PropertyName = "Set a limit of how many active bounties a player can have at any time (set to 0 to disable the limit)")]
    public int SetLimit { get; set; }
    [JsonProperty(PropertyName = "Notification Options")]
    public NotificationOptions Notifications { get; set; }
    [JsonProperty(PropertyName = "Reward Options")]
    public RewardOptions Rewards { get; set; }
    public class NotificationOptions
    {
        [JsonProperty(PropertyName = "PopupNotifications - Broadcast using PopupNotifications")]
        public bool UsePopupNotifications { get; set; }
        [JsonProperty(PropertyName = "PopupNotifications - Duration of notification")]
        public float PopupDuration { get; set; }
        [JsonProperty(PropertyName = "Broadcast new bounties globally")]
        public bool BroadcastNewBounties { get; set; }
        [JsonProperty(PropertyName = "Reminders - Remind targets they have a bounty on them")]
        public bool ShowReminders { get; set; }
        [JsonProperty(PropertyName = "Reminders - Amount of time between reminders (in minutes)")]
        public int ReminderTime { get; set; }
    }

    public class RewardOptions
    {
        [JsonProperty(PropertyName = "Allow bounties to be placed using Economics")]
        public bool AllowEconomics { get; set; }
        [JsonProperty(PropertyName = "Allow bounties to be placed using RP")]
        public bool AllowServerRewards { get; set; }
        [JsonProperty(PropertyName = "Allow bounties to be placed using items")]
        public bool AllowItems { get; set; }
    }

    [JsonProperty(PropertyName = "Bounty icon URL for RustNET menu")]
    public string RustNETIcon { get; set; }
    public Oxide.Core.VersionNumber Version { get; set; }
}

public class NotificationOptions
{
    [JsonProperty(PropertyName = "PopupNotifications - Broadcast using PopupNotifications")]
    public bool UsePopupNotifications { get; set; }
    [JsonProperty(PropertyName = "PopupNotifications - Duration of notification")]
    public float PopupDuration { get; set; }
    [JsonProperty(PropertyName = "Broadcast new bounties globally")]
    public bool BroadcastNewBounties { get; set; }
    [JsonProperty(PropertyName = "Reminders - Remind targets they have a bounty on them")]
    public bool ShowReminders { get; set; }
    [JsonProperty(PropertyName = "Reminders - Amount of time between reminders (in minutes)")]
    public int ReminderTime { get; set; }
}

public class RewardOptions
{
    [JsonProperty(PropertyName = "Allow bounties to be placed using Economics")]
    public bool AllowEconomics { get; set; }
    [JsonProperty(PropertyName = "Allow bounties to be placed using RP")]
    public bool AllowServerRewards { get; set; }
    [JsonProperty(PropertyName = "Allow bounties to be placed using items")]
    public bool AllowItems { get; set; }
}

private class StoredData
{
    public Dictionary<ulong, PlayerData> players;
    public Dictionary<int, RewardInfo> rewards;
}

private class PlayerData
{
    public string displayName;
    public int totalBounties;
    public int bountiesClaimed;
    public double totalWantedTime;
    public List<BountyInfo> activeBounties;
    public List<int> unclaimedRewards;
    public PlayerData();
    public PlayerData(string displayName);
    public void ClaimRewards(List<int> rewards);
    public BountyInfo GetBountyOf(ulong initiatorId);
    public int GetTotalBounties();
    public void UpdateWantedTime();
    public double GetCurrentWantedTime();
    public class BountyInfo
    {
        public ulong initiatorId;
        public string initiatorName;
        public double initiatedTime;
        public int rewardId;
        public BountyInfo();
        public BountyInfo(ulong initiatorId, string initiatorName, int rewardId);
    }

}

public class BountyInfo
{
    public ulong initiatorId;
    public string initiatorName;
    public double initiatedTime;
    public int rewardId;
    public BountyInfo();
    public BountyInfo(ulong initiatorId, string initiatorName, int rewardId);
}

private class RewardInfo
{
    public int rpAmount;
    public int econAmount;
    public List<ItemData> rewardItems;
    public RewardInfo();
    public RewardInfo(int rpAmount, int econAmount, ItemContainer container);
    public string GetRewardString(ulong playerId);
    private IEnumerable<ItemData> GetItems(ItemContainer container);
    public class ItemData
    {
        public int itemid;
        public ulong skin;
        public int amount;
        public float condition;
        public int ammo;
        public string ammotype;
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

}

public class ItemData
{
    public int itemid;
    public ulong skin;
    public int amount;
    public float condition;
    public int ammo;
    public string ammotype;
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

private class OfflinePlayers
{
    public Hash<string, double> offlinePlayers;
    public void AddOfflinePlayer(string userId);
    public void OnPlayerInit(string userId);
    public void RemoveOldPlayers();
    public IPlayer[] GetOfflineList();
    public double CurrentTime();
}


```

---

## BoxLooters

```csharp
using System;
using System.Collections.Generic;
using Oxide.Core;
using Oxide.Core.Configuration;
using UnityEngine;
using System.Linq;
using System.Reflection;

Oxide.Plugins
[Info("BoxLooters", "4seti / k1lly0u", "0.3.1", ResourceId = 989)]
 class BoxLooters : RustPlugin
{
     BoxDS boxData;
     PlayerDS playerData;
    private DynamicConfigFile bdata;
    private DynamicConfigFile pdata;
    private Vector3 eyesAdjust;
    private FieldInfo serverinput;
    private bool eraseData;
    private Dictionary<uint, BoxData> boxCache;
    private Dictionary<ulong, PlayerData> playerCache;
     void Loaded();
     void OnServerInitialized();
     void OnNewSave(string filename);
     void OnServerSave();
     void Unload();
     void OnLootEntity(BasePlayer looter, BaseEntity entity);
     void OnEntityDeath(BaseCombatEntity entity, HitInfo hitInfo);
     void ClearAllData();
     void RemoveOldData();
     object FindBoxFromRay(BasePlayer player);
     void ReplyInfo(BasePlayer player, string Id, int replies, bool isPlayer, string additional);
     double GrabCurrentTime();
     bool HasPermission(BasePlayer player);
     float GetDistance(Vector3 init, Vector3 target);
     bool IsValidType(BaseEntity entity);
    [ChatCommand("box")]
     void cmdBox(BasePlayer player, string command, string[] args);
    private ConfigData configData;
     class ConfigData
    {
        public int RemoveHours { get; set; }
    }

    private void LoadVariables();
    protected override void LoadDefaultConfig();
    private void LoadConfigVariables();
     void SaveConfig(ConfigData config);
     class BoxData
    {
        public double lastInit;
        public float x;
        public float y;
        public float z;
        public ulong destroyId;
        public string destroyName;
        public Dictionary<ulong, LootEntry> Looters;
        public BoxData();
        public BoxData(double time, BasePlayer player, Vector3 pos);
        public void AddLoot(BasePlayer looter, double time, string date);
        public void SetKiller(ulong Id, string name);
        public Vector3 GetPosition();
    }

     class PlayerData
    {
        public double lastInit;
        public Dictionary<ulong, LootEntry> Looters;
        public PlayerData();
        public PlayerData(double time, BasePlayer player);
        public void AddLoot(BasePlayer looter, double time, string date);
    }

    public class LootEntry
    {
        public string Name;
        public double LastInit;
        public string FirstLoot;
        public string LastLoot;
        public LootEntry();
        public LootEntry(string name, string firstLoot, double lastInit);
    }

     void SaveData();
     void LoadData();
     class BoxDS
    {
        public Dictionary<uint, BoxData> boxes;
    }

     class PlayerDS
    {
        public Dictionary<ulong, PlayerData> players;
    }

     Dictionary<string, string> messages;
}

 class ConfigData
{
    public int RemoveHours { get; set; }
}

 class BoxData
{
    public double lastInit;
    public float x;
    public float y;
    public float z;
    public ulong destroyId;
    public string destroyName;
    public Dictionary<ulong, LootEntry> Looters;
    public BoxData();
    public BoxData(double time, BasePlayer player, Vector3 pos);
    public void AddLoot(BasePlayer looter, double time, string date);
    public void SetKiller(ulong Id, string name);
    public Vector3 GetPosition();
}

 class PlayerData
{
    public double lastInit;
    public Dictionary<ulong, LootEntry> Looters;
    public PlayerData();
    public PlayerData(double time, BasePlayer player);
    public void AddLoot(BasePlayer looter, double time, string date);
}

public class LootEntry
{
    public string Name;
    public double LastInit;
    public string FirstLoot;
    public string LastLoot;
    public LootEntry();
    public LootEntry(string name, string firstLoot, double lastInit);
}

 class BoxDS
{
    public Dictionary<uint, BoxData> boxes;
}

 class PlayerDS
{
    public Dictionary<ulong, PlayerData> players;
}


```

---

## BPUnlockerVip

```csharp
using Newtonsoft.Json;
using Oxide.Core.Libraries.Covalence;
using ProtoBuf;
using System.Collections;
using System.Collections.Generic;
using System.Linq;
using UnityEngine;

Oxide.Plugins
[Info("Blueprints Unlocker", "Vlad-00003", "1.2.1")]
[Description("Unlock all blueprints to the players")]
 class BPUnlockerVip : RustPlugin
{
    private PluginConfig _config;
    private readonly List<ItemDefinition> _available;
    private Timer _updater;
    private readonly Queue<ulong> _order;
    private bool _inProgress;
    private class PluginConfig
    {
        [JsonProperty("Permission to automaticly unlock ALL blueprints")]
        public string All;
        [JsonProperty("Permission to remove workbench requirements")]
        public string NoWorkbench;
        [JsonProperty("List of cutom permissions")]
        public Dictionary<string, List<string>> CustomPermissions;
        [JsonProperty("List of all available blueprints to unlock(editing does nothing)")]
        public Dictionary<string, List<string>> Available;
        [JsonProperty("Command to unlock/lock blueprints for the player")]
        public string Command;
        [JsonProperty("Permission to use command")]
        public string CommandPermission;
        [JsonIgnore]
        public readonly Dictionary<string, List<ItemDefinition>> Custom;
    }

    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    protected override void SaveConfig();
     void Init();
     void Unload();
     void OnServerInitialized();
    private string GetMsg(string langline, object userId, object[] args);
    protected override void LoadDefaultMessages();
    private void OnPlayerInit(BasePlayer player);
     object CanCraft(PlayerBlueprints bps, ItemDefinition itemDef, int skinId);
    private void UpdatePermissions();
     IEnumerator Unlock(ulong userId, List<ItemDefinition> list);
    private void CmdBp(IPlayer player, string cmd, string[] args);
    private List<ItemDefinition> GetBpist(object player);
}

private class PluginConfig
{
    [JsonProperty("Permission to automaticly unlock ALL blueprints")]
    public string All;
    [JsonProperty("Permission to remove workbench requirements")]
    public string NoWorkbench;
    [JsonProperty("List of cutom permissions")]
    public Dictionary<string, List<string>> CustomPermissions;
    [JsonProperty("List of all available blueprints to unlock(editing does nothing)")]
    public Dictionary<string, List<string>> Available;
    [JsonProperty("Command to unlock/lock blueprints for the player")]
    public string Command;
    [JsonProperty("Permission to use command")]
    public string CommandPermission;
    [JsonIgnore]
    public readonly Dictionary<string, List<ItemDefinition>> Custom;
}


```

---

## BradleyTiers

```csharp
using System;
using System.Linq;
using System.Reflection;
using System.Collections.Generic;
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Plugins;
using Rust;
using UnityEngine;

Oxide.Plugins
[Info("BradleyTiers", "Krungh Crow", "1.2.2")]
[Description("Bradley with difficulties and events")]
 class BradleyTiers : RustPlugin
{
    [PluginReference]
     Plugin ArmoredTrain;
     Plugin Convoy;
     Plugin Economics;
     Plugin GUIAnnouncements;
     Plugin Notify;
     Plugin UINotify;
     Plugin ServerRewards;
    const string Use_Perm;
    private System.Random random;
    private Dictionary<string, List<ulong>> Skins { get; set; }
     ulong chaticon;
     string prefix;
     float accuracy;
     string _announce;
     int CaseCount;
     string color1;
     string color2;
     float damagescale;
     string difficulty;
     string _vanilla;
     string _easy;
     string _medium;
     string _hard;
     string _nightmare;
     string lootprofile;
     int reward;
     int total;
     int Pid;
     bool Debug;
     bool showchat;
     bool showchatannouncement;
     bool showchatreward;
     bool UseNotify;
     bool IsConvoyAPC;
     Dictionary<string, CooldownTimer> CoolDowns;
    public class CooldownTimer
    {
        public Timer timer;
        public float start;
        public float countdown;
    }

     void Init();
    private ConfigData configData;
     class ConfigData
    {
        [JsonProperty(PropertyName = "Main config")]
        public SettingsPlugin PlugCFG;
        [JsonProperty(PropertyName = "Tier Names")]
        public SettingsNames Naming;
        [JsonProperty(PropertyName = "Kill Rewards")]
        public SettingsReward Rewards;
        [JsonProperty(PropertyName = "Loot Tables")]
        public SettingsLoot Loot;
        [JsonProperty(PropertyName = "Easy Bradley")]
        public SettingsBradEasy Easy;
        [JsonProperty(PropertyName = "Medium Bradley")]
        public SettingsBradMedium Medium;
        [JsonProperty(PropertyName = "Hard Bradley")]
        public SettingsBradHard Hard;
        [JsonProperty(PropertyName = "Nightmare Bradley")]
        public SettingsBradNightmare Nightmare;
    }

     class SettingsPlugin
    {
        [JsonProperty(PropertyName = "Debug")]
        public bool Debug;
        [JsonProperty(PropertyName = "Chat Prefix")]
        public string Prefix;
        [JsonProperty(PropertyName = "Use GUIAnnouncement")]
        public bool GUIAUse;
        [JsonProperty(PropertyName = "Reply to player in chat on attack")]
        public bool ChatUse;
        [JsonProperty(PropertyName = "Reply to player in chat on reward")]
        public bool ChatRewardUse;
        [JsonProperty(PropertyName = "Show kills/spawns in Global chat")]
        public bool ChatAnnounceUse;
        [JsonProperty(PropertyName = "Use Notify")]
        public bool UseNotify;
        [JsonProperty(PropertyName = "Notify profile ID")]
        public int PiD;
        [JsonProperty(PropertyName = "Include Vanilla Bradley")]
        public bool VanillaUse;
        [JsonProperty(PropertyName = "Include ArmoredTrain Bradley")]
        public bool TrainUse;
        [JsonProperty(PropertyName = "Include Convoy Bradley")]
        public bool ConvoyUse;
        [JsonProperty(PropertyName = "Include SatDish/Harbor Event Bradley")]
        public bool SatdishUse;
        [JsonProperty(PropertyName = "Bradley Tiers can interact with NPC")]
        public bool DmgNPCUse;
    }

     class SettingsNames
    {
        [JsonProperty(PropertyName = "Vanilla")]
        public string Vanilla;
        [JsonProperty(PropertyName = "Easy")]
        public string Easy;
        [JsonProperty(PropertyName = "Medium")]
        public string Medium;
        [JsonProperty(PropertyName = "Hard")]
        public string Hard;
        [JsonProperty(PropertyName = "Nightmare")]
        public string Nightmare;
    }

     class SettingsReward
    {
        [JsonProperty(PropertyName = "Use Economics?")]
        public bool UseEco;
        [JsonProperty(PropertyName = "Use ServerRewards?")]
        public bool UseSR;
        [JsonProperty(PropertyName = "Vanilla amount")]
        public int Vanilla;
        [JsonProperty(PropertyName = "Easy amount")]
        public int Easy;
        [JsonProperty(PropertyName = "Medium amount")]
        public int Medium;
        [JsonProperty(PropertyName = "Hard amount")]
        public int Hard;
        [JsonProperty(PropertyName = "Nightmare amount")]
        public int Nightmare;
    }

     class SettingsBradEasy
    {
        [JsonProperty(PropertyName = "Bradley Health")]
        public int BradleyHealth;
        [JsonProperty(PropertyName = "Bradley Max Fire Range")]
        public int BradleyMaxFireRange;
        [JsonProperty(PropertyName = "Bradley Bulletdamage (15 is vanilla)")]
        public float BradleyBDamage;
        [JsonProperty(PropertyName = "Bradley Throttle Responce")]
        public float BradleySpeed;
        [JsonProperty(PropertyName = "Bradley Accuracy (0-1)")]
        public float BradleyAccuracy;
        [JsonProperty(PropertyName = "Bradley Damage scale (0-1)")]
        public float BradleyDamageScale;
        [JsonProperty(PropertyName = "Add Custom Loot")]
        public bool UpdateCratesLoot;
        [JsonProperty(PropertyName = "Bradley Max crates after kill")]
        public int BradleyCratesAmount;
        [JsonProperty(PropertyName = "Spawn Min Amount Items")]
        public int MinAmount { get; set; }
        [JsonProperty(PropertyName = "Spawn Max Amount Items")]
        public int MaxAmount { get; set; }
        [JsonProperty(PropertyName = "Loot Table", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<BradleyItem> Loot { get; set; }
    }

     class SettingsBradMedium
    {
        [JsonProperty(PropertyName = "Bradley Health")]
        public int BradleyHealth;
        [JsonProperty(PropertyName = "Bradley Max Fire Range")]
        public int BradleyMaxFireRange;
        [JsonProperty(PropertyName = "Bradley Bulletdamage (15 is vanilla)")]
        public float BradleyBDamage;
        [JsonProperty(PropertyName = "Bradley Throttle Responce")]
        public float BradleySpeed;
        [JsonProperty(PropertyName = "Bradley Accuracy (0-1)")]
        public float BradleyAccuracy;
        [JsonProperty(PropertyName = "Bradley Damage scale (0-1)")]
        public float BradleyDamageScale;
        [JsonProperty(PropertyName = "Add Custom Loot")]
        public bool UpdateCratesLoot;
        [JsonProperty(PropertyName = "Bradley Max crates after kill")]
        public int BradleyCratesAmount;
        [JsonProperty(PropertyName = "Spawn Min Amount Items")]
        public int MinAmount { get; set; }
        [JsonProperty(PropertyName = "Spawn Max Amount Items")]
        public int MaxAmount { get; set; }
        [JsonProperty(PropertyName = "Loot Table", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<BradleyItem> Loot { get; set; }
    }

     class SettingsBradHard
    {
        [JsonProperty(PropertyName = "Bradley Health")]
        public int BradleyHealth;
        [JsonProperty(PropertyName = "Bradley Max Fire Range")]
        public int BradleyMaxFireRange;
        [JsonProperty(PropertyName = "Bradley Bulletdamage (15 is vanilla)")]
        public float BradleyBDamage;
        [JsonProperty(PropertyName = "Bradley Throttle Responce")]
        public float BradleySpeed;
        [JsonProperty(PropertyName = "Bradley Accuracy (0-1)")]
        public float BradleyAccuracy;
        [JsonProperty(PropertyName = "Bradley Damage scale (0-1)")]
        public float BradleyDamageScale;
        [JsonProperty(PropertyName = "Add Custom Loot")]
        public bool UpdateCratesLoot;
        [JsonProperty(PropertyName = "Bradley Max crates after kill")]
        public int BradleyCratesAmount;
        [JsonProperty(PropertyName = "Spawn Min Amount Items")]
        public int MinAmount { get; set; }
        [JsonProperty(PropertyName = "Spawn Max Amount Items")]
        public int MaxAmount { get; set; }
        [JsonProperty(PropertyName = "Loot Table", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<BradleyItem> Loot { get; set; }
    }

     class SettingsBradNightmare
    {
        [JsonProperty(PropertyName = "Bradley Health")]
        public int BradleyHealth;
        [JsonProperty(PropertyName = "Bradley Max Fire Range")]
        public int BradleyMaxFireRange;
        [JsonProperty(PropertyName = "Bradley Bulletdamage (15 is vanilla)")]
        public float BradleyBDamage;
        [JsonProperty(PropertyName = "Bradley Throttle Responce")]
        public float BradleySpeed;
        [JsonProperty(PropertyName = "Bradley Accuracy (0-1)")]
        public float BradleyAccuracy;
        [JsonProperty(PropertyName = "Bradley Damage scale (0-1)")]
        public float BradleyDamageScale;
        [JsonProperty(PropertyName = "Add Custom Loot")]
        public bool UpdateCratesLoot;
        [JsonProperty(PropertyName = "Bradley Max crates after kill")]
        public int BradleyCratesAmount;
        [JsonProperty(PropertyName = "Spawn Min Amount Items")]
        public int MinAmount { get; set; }
        [JsonProperty(PropertyName = "Spawn Max Amount Items")]
        public int MaxAmount { get; set; }
        [JsonProperty(PropertyName = "Loot Table", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<BradleyItem> Loot { get; set; }
    }

     class SettingsLoot
    {
        [JsonProperty(PropertyName = "Use lootsystem")]
        public bool UseLoot;
        [JsonProperty(PropertyName = "Use Random Skins")]
        public bool RandomSkins { get; set; }
    }

    private bool LoadConfigVariables();
    protected override void LoadDefaultConfig();
     void SaveConf();
    protected override void LoadDefaultMessages();
    [ChatCommand("bt")]
    private void cmdPrimary(BasePlayer player, string command, string[] args);
    private string msg(string key, string id);
    private void OnBradleyApcInitialize(BradleyAPC bradley);
     void OnEntityDeath(BradleyAPC apc, HitInfo info);
     void OnEntityTakeDamage(BradleyAPC bradley, HitInfo info);
    private object OnEntityTakeDamage(BaseCombatEntity entity, HitInfo info);
    private object CanBradleyApcTarget(BradleyAPC bradley, NPCPlayer npc);
    private static string GetGrid(Vector3 position);
    private static List<BradleyItem> DefaultLoot { get; set; }
    public class BradleyItem
    {
        public float probability { get; set; }
        public string shortname { get; set; }
        public string name { get; set; }
        public ulong skin { get; set; }
        public int amountMin { get; set; }
        public int amount { get; set; }
    }

    private void SpawnLoot(ItemContainer container, List<BradleyItem> loot);
    private List<ulong> GetItemSkins(ItemDefinition def);
    private List<ulong> ExtractItemSkins(ItemDefinition def, List<ulong> skins);
}

public class CooldownTimer
{
    public Timer timer;
    public float start;
    public float countdown;
}

 class ConfigData
{
    [JsonProperty(PropertyName = "Main config")]
    public SettingsPlugin PlugCFG;
    [JsonProperty(PropertyName = "Tier Names")]
    public SettingsNames Naming;
    [JsonProperty(PropertyName = "Kill Rewards")]
    public SettingsReward Rewards;
    [JsonProperty(PropertyName = "Loot Tables")]
    public SettingsLoot Loot;
    [JsonProperty(PropertyName = "Easy Bradley")]
    public SettingsBradEasy Easy;
    [JsonProperty(PropertyName = "Medium Bradley")]
    public SettingsBradMedium Medium;
    [JsonProperty(PropertyName = "Hard Bradley")]
    public SettingsBradHard Hard;
    [JsonProperty(PropertyName = "Nightmare Bradley")]
    public SettingsBradNightmare Nightmare;
}

 class SettingsPlugin
{
    [JsonProperty(PropertyName = "Debug")]
    public bool Debug;
    [JsonProperty(PropertyName = "Chat Prefix")]
    public string Prefix;
    [JsonProperty(PropertyName = "Use GUIAnnouncement")]
    public bool GUIAUse;
    [JsonProperty(PropertyName = "Reply to player in chat on attack")]
    public bool ChatUse;
    [JsonProperty(PropertyName = "Reply to player in chat on reward")]
    public bool ChatRewardUse;
    [JsonProperty(PropertyName = "Show kills/spawns in Global chat")]
    public bool ChatAnnounceUse;
    [JsonProperty(PropertyName = "Use Notify")]
    public bool UseNotify;
    [JsonProperty(PropertyName = "Notify profile ID")]
    public int PiD;
    [JsonProperty(PropertyName = "Include Vanilla Bradley")]
    public bool VanillaUse;
    [JsonProperty(PropertyName = "Include ArmoredTrain Bradley")]
    public bool TrainUse;
    [JsonProperty(PropertyName = "Include Convoy Bradley")]
    public bool ConvoyUse;
    [JsonProperty(PropertyName = "Include SatDish/Harbor Event Bradley")]
    public bool SatdishUse;
    [JsonProperty(PropertyName = "Bradley Tiers can interact with NPC")]
    public bool DmgNPCUse;
}

 class SettingsNames
{
    [JsonProperty(PropertyName = "Vanilla")]
    public string Vanilla;
    [JsonProperty(PropertyName = "Easy")]
    public string Easy;
    [JsonProperty(PropertyName = "Medium")]
    public string Medium;
    [JsonProperty(PropertyName = "Hard")]
    public string Hard;
    [JsonProperty(PropertyName = "Nightmare")]
    public string Nightmare;
}

 class SettingsReward
{
    [JsonProperty(PropertyName = "Use Economics?")]
    public bool UseEco;
    [JsonProperty(PropertyName = "Use ServerRewards?")]
    public bool UseSR;
    [JsonProperty(PropertyName = "Vanilla amount")]
    public int Vanilla;
    [JsonProperty(PropertyName = "Easy amount")]
    public int Easy;
    [JsonProperty(PropertyName = "Medium amount")]
    public int Medium;
    [JsonProperty(PropertyName = "Hard amount")]
    public int Hard;
    [JsonProperty(PropertyName = "Nightmare amount")]
    public int Nightmare;
}

 class SettingsBradEasy
{
    [JsonProperty(PropertyName = "Bradley Health")]
    public int BradleyHealth;
    [JsonProperty(PropertyName = "Bradley Max Fire Range")]
    public int BradleyMaxFireRange;
    [JsonProperty(PropertyName = "Bradley Bulletdamage (15 is vanilla)")]
    public float BradleyBDamage;
    [JsonProperty(PropertyName = "Bradley Throttle Responce")]
    public float BradleySpeed;
    [JsonProperty(PropertyName = "Bradley Accuracy (0-1)")]
    public float BradleyAccuracy;
    [JsonProperty(PropertyName = "Bradley Damage scale (0-1)")]
    public float BradleyDamageScale;
    [JsonProperty(PropertyName = "Add Custom Loot")]
    public bool UpdateCratesLoot;
    [JsonProperty(PropertyName = "Bradley Max crates after kill")]
    public int BradleyCratesAmount;
    [JsonProperty(PropertyName = "Spawn Min Amount Items")]
    public int MinAmount { get; set; }
    [JsonProperty(PropertyName = "Spawn Max Amount Items")]
    public int MaxAmount { get; set; }
    [JsonProperty(PropertyName = "Loot Table", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<BradleyItem> Loot { get; set; }
}

 class SettingsBradMedium
{
    [JsonProperty(PropertyName = "Bradley Health")]
    public int BradleyHealth;
    [JsonProperty(PropertyName = "Bradley Max Fire Range")]
    public int BradleyMaxFireRange;
    [JsonProperty(PropertyName = "Bradley Bulletdamage (15 is vanilla)")]
    public float BradleyBDamage;
    [JsonProperty(PropertyName = "Bradley Throttle Responce")]
    public float BradleySpeed;
    [JsonProperty(PropertyName = "Bradley Accuracy (0-1)")]
    public float BradleyAccuracy;
    [JsonProperty(PropertyName = "Bradley Damage scale (0-1)")]
    public float BradleyDamageScale;
    [JsonProperty(PropertyName = "Add Custom Loot")]
    public bool UpdateCratesLoot;
    [JsonProperty(PropertyName = "Bradley Max crates after kill")]
    public int BradleyCratesAmount;
    [JsonProperty(PropertyName = "Spawn Min Amount Items")]
    public int MinAmount { get; set; }
    [JsonProperty(PropertyName = "Spawn Max Amount Items")]
    public int MaxAmount { get; set; }
    [JsonProperty(PropertyName = "Loot Table", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<BradleyItem> Loot { get; set; }
}

 class SettingsBradHard
{
    [JsonProperty(PropertyName = "Bradley Health")]
    public int BradleyHealth;
    [JsonProperty(PropertyName = "Bradley Max Fire Range")]
    public int BradleyMaxFireRange;
    [JsonProperty(PropertyName = "Bradley Bulletdamage (15 is vanilla)")]
    public float BradleyBDamage;
    [JsonProperty(PropertyName = "Bradley Throttle Responce")]
    public float BradleySpeed;
    [JsonProperty(PropertyName = "Bradley Accuracy (0-1)")]
    public float BradleyAccuracy;
    [JsonProperty(PropertyName = "Bradley Damage scale (0-1)")]
    public float BradleyDamageScale;
    [JsonProperty(PropertyName = "Add Custom Loot")]
    public bool UpdateCratesLoot;
    [JsonProperty(PropertyName = "Bradley Max crates after kill")]
    public int BradleyCratesAmount;
    [JsonProperty(PropertyName = "Spawn Min Amount Items")]
    public int MinAmount { get; set; }
    [JsonProperty(PropertyName = "Spawn Max Amount Items")]
    public int MaxAmount { get; set; }
    [JsonProperty(PropertyName = "Loot Table", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<BradleyItem> Loot { get; set; }
}

 class SettingsBradNightmare
{
    [JsonProperty(PropertyName = "Bradley Health")]
    public int BradleyHealth;
    [JsonProperty(PropertyName = "Bradley Max Fire Range")]
    public int BradleyMaxFireRange;
    [JsonProperty(PropertyName = "Bradley Bulletdamage (15 is vanilla)")]
    public float BradleyBDamage;
    [JsonProperty(PropertyName = "Bradley Throttle Responce")]
    public float BradleySpeed;
    [JsonProperty(PropertyName = "Bradley Accuracy (0-1)")]
    public float BradleyAccuracy;
    [JsonProperty(PropertyName = "Bradley Damage scale (0-1)")]
    public float BradleyDamageScale;
    [JsonProperty(PropertyName = "Add Custom Loot")]
    public bool UpdateCratesLoot;
    [JsonProperty(PropertyName = "Bradley Max crates after kill")]
    public int BradleyCratesAmount;
    [JsonProperty(PropertyName = "Spawn Min Amount Items")]
    public int MinAmount { get; set; }
    [JsonProperty(PropertyName = "Spawn Max Amount Items")]
    public int MaxAmount { get; set; }
    [JsonProperty(PropertyName = "Loot Table", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<BradleyItem> Loot { get; set; }
}

 class SettingsLoot
{
    [JsonProperty(PropertyName = "Use lootsystem")]
    public bool UseLoot;
    [JsonProperty(PropertyName = "Use Random Skins")]
    public bool RandomSkins { get; set; }
}

public class BradleyItem
{
    public float probability { get; set; }
    public string shortname { get; set; }
    public string name { get; set; }
    public ulong skin { get; set; }
    public int amountMin { get; set; }
    public int amount { get; set; }
}


```

---

## Broadcaster

```csharp
using System.Collections.Generic;
using Oxide.Core.Configuration;
using Oxide.Core;
using System;

Oxide.Plugins
[Info("Broadcaster", "Oxide Россия - oxide-russia.ru", "1.1.15")]
 class Broadcaster : RustPlugin
{
     string usagePerm;
     string joinIcon;
     string leaveIcon;
     string firstMessageFormat;
     string joinMessageFormat;
     string leaveMessageFormat;
     string motdMessage;
     string motdColour;
     bool motdEnabled;
     bool onlineCommand;
     bool joinLeaveIcons;
     string joinColour;
     string firstJoinColour;
     string leaveColour;
     string nameColour;
     string broadcastPrefix;
     bool broadcastPrefixEnabled;
     string broadcastPrefixColour;
     bool broadcastRandomMessages;
     int broadcastInterval;
     Timer autoBroadcastTimer;
     List<object> broadcastMessages;
     StoredData storedData;
     string DataFile;
     string errorColour;
     string mainColour;
     string mainPrefix;
     void Init();
     void Loaded();
    [ChatCommand("who")]
     void OnCommandWho(BasePlayer sender, string command, string[] args);
    [ChatCommand("online")]
     void OnCommandOnline(BasePlayer sender, string command, string[] args);
    [ChatCommand("players")]
     void OnCommandPlayers(BasePlayer sender, string command, string[] args);
    [ChatCommand("whois")]
     void OnCommandWhois(BasePlayer sender, string command, string[] args);
    [ChatCommand("forcebroadcast")]
     void OnCommandForce(BasePlayer sender, string command, string[] args);
    [ChatCommand("bc")]
     void OnCommandAp(BasePlayer sender, string command, string[] args);
     void startTimer(int interval);
     void broadcastRandomMessage();
     int CurrentNum;
     string getRandomBroadcast();
     void OnPlayerInit(BasePlayer player);
     void updatePlayerInfo(BasePlayer player);
     void OnPlayerDisconnected(BasePlayer player, string reason);
     void sendJoinMessage(BasePlayer player);
     void sendLeaveMessage(BasePlayer player);
     void sendMOTD(BasePlayer player);
     void showPlayerInfo(BasePlayer player, BasePlayer sender);
     void addNewPlayer(BasePlayer player, bool firstTime);
     void RegisterPermissions();
     void initGlobals();
    protected override void LoadDefaultConfig();
     List<string> GenerateDefaultBroadcastMessages();
     void LoadDefaultMessages();
     string Lang(string key, string id, object[] args);
     void writeFile();
     void openDataFile();
     string color(string color, string text);
     string getMessageFormat(BasePlayer player, string type);
     void sendError(BasePlayer player, string type, string extra);
     void sendMessage(BasePlayer player, string type, string extra);
     PlayerInfo getPlayerInfo(BasePlayer player);
     BasePlayer getPlayer(PlayerInfo player);
     BasePlayer getPlayer(string partialName);
     class PlayerInfo
    {
        public string displayName;
        public ulong userID;
        public bool newPlayer;
        public string ipAddress;
        public List<string> Aliases;
        public PlayerInfo(string name, ulong id);
        public void addAlias(string name);
    }

     class StoredData
    {
        public List<PlayerInfo> Players;
    }

}

 class PlayerInfo
{
    public string displayName;
    public ulong userID;
    public bool newPlayer;
    public string ipAddress;
    public List<string> Aliases;
    public PlayerInfo(string name, ulong id);
    public void addAlias(string name);
}

 class StoredData
{
    public List<PlayerInfo> Players;
}


```

---

## BrokenItemsCleaner

```csharp
using System;
using System.Collections;
using System.Collections.Generic;
using System.Diagnostics;
using System.Linq;
using JetBrains.Annotations;
using UnityEngine;
using Debug = UnityEngine.Debug;

Oxide.Plugins
[Info("BrokenItemsCleaner", "EcoSmile/Vlad-00003", "1.3.5")]
public class BrokenItemsCleaner : RustPlugin
{
    [PluginReference("Backpacks")]
    private RustPlugin Backpacks;
    private Coroutine _cleanup;
    private CleanupData _cleanupData;
    private void Loaded();
    private void OnServerSave();
    private void Unload();
    private class CleanupData
    {
        private readonly Stopwatch _stopwatch;
        public readonly HashSet<ulong> HeldEntities;
        public readonly Dictionary<ulong, Item[]> Items;
        public int Removed;
        public int Repaired;
        public void Add(IItemGetter selector, BaseNetworkable[] array);
        public void Add(ulong userId, ItemContainer container);
        public bool InvalidOrListed(HeldEntity entity);
        public void Clear();
        public void StartNew();
        public void Stop();
        public override string ToString();
    }

    private class ItemGetter : IItemGetter
    {
        private readonly Func<T, IEnumerable<Item>> _getter;
        public ItemGetter(Func<T, IEnumerable<Item>> getter);
        [CanBeNull]
        public IEnumerable<Item> GetItems(BaseNetworkable obj);
        public IEnumerable<BaseNetworkable> Entities(BaseNetworkable[] array);
        public string TypeName { get; set; }
    }

    private static readonly List<IItemGetter> ItemGetters;
    private void CheckLost(IEnumerable<Item> items);
    private IEnumerator PreformCleanup();
    private void OnCleanupComplete(Exception ex);
    private IEnumerator ActualCleanup();
}

private class CleanupData
{
    private readonly Stopwatch _stopwatch;
    public readonly HashSet<ulong> HeldEntities;
    public readonly Dictionary<ulong, Item[]> Items;
    public int Removed;
    public int Repaired;
    public void Add(IItemGetter selector, BaseNetworkable[] array);
    public void Add(ulong userId, ItemContainer container);
    public bool InvalidOrListed(HeldEntity entity);
    public void Clear();
    public void StartNew();
    public void Stop();
    public override string ToString();
}

private class ItemGetter : IItemGetter
{
    private readonly Func<T, IEnumerable<Item>> _getter;
    public ItemGetter(Func<T, IEnumerable<Item>> getter);
    [CanBeNull]
    public IEnumerable<Item> GetItems(BaseNetworkable obj);
    public IEnumerable<BaseNetworkable> Entities(BaseNetworkable[] array);
    public string TypeName { get; set; }
}


```

---

## BStats

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
[Info("BStats", "L&W", "2.0.0")]
public class BStats : RustPlugin
{
    [PluginReference]
    private Plugin ImageLibrary;
    private string[] _gatherHooks;
    private static BStats plugin;
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
    private void PlayerTop(BasePlayer player, ulong playerID, int page);
    private void TopPlayerList(BasePlayer player, int page);
    private void PlayerTopInfo(BasePlayer player, ulong playerID, int page);
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

## BuildBlocker

```csharp
using UnityEngine;
using System;

Oxide.Plugins
[Info("BuildBlocker", "Bombardir", "1.3.4", ResourceId = 834)]
 class BuildBlocker : RustPlugin
{
    static bool OnRoad;
    static bool OnRiver;
    static bool OnRock;
    static bool InTunnel;
    static bool InRock;
    static bool InCave;
    static bool InWarehouse;
    static bool InMetalBuilding;
    static bool InHangar;
    static bool InTank;
    static bool InBase;
    static bool InStormDrain;
    static bool UnTerrain;
    static bool UnBridge;
    static bool UnRadio;
    static bool BlockStructuresHeight;
    static bool BlockDeployablesHeight;
    static int MaxHeight;
    static bool BlockStructuresWater;
    static bool BlockDeployablesWater;
    static int MaxWater;
    static int AuthLVL;
    static string Msg;
    static string MsgHeight;
    static string MsgWater;
     void LoadDefaultConfig();
     void CheckCfg(string Key, T var);
     void Init();
     void CheckBlock(BaseNetworkable StartBlock, BasePlayer sender, bool CheckHeight, bool CheckWater);
     void OnEntityBuilt(Planner plan, GameObject obj);
     void OnItemDeployed(Deployer deployer, BaseEntity deployedentity);
}


```

---

## BuildingBlocked

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using UnityEngine;

Oxide.Plugins
[Info("BuildingBlocked", "Sempai#3239", "1.1.0")]
 class BuildingBlocked : RustPlugin
{
     int terrainMask;
     int constructionMask;
     int waterLevel;
     int maxHeight;
     bool caveBuild;
     bool roadRestrict;
     bool iceBuild;
     bool adminIgnore;
     float treeRadius;
     bool buildBlock;
    private void LoadDefaultConfig();
    private void GetConfig(string MainMenu, string Key, T var);
     WaterCollision collision;
     void OnServerInitialized();
    static void DrawBox(BasePlayer player, Vector3 center, Quaternion rotation, Vector3 size);
    static Vector3 RotatePointAroundPivot(Vector3 point, Vector3 pivot, Quaternion rotation);
     void OnEntityBuilt(Planner planner, GameObject gameobject, Vector3 Pos);
     bool CompareFoundationStacking(Vector3 vec1, Vector3 vec2);
     void SendReply(BasePlayer player, string msg);
     bool InCave(Vector3 vec);
     void Refund(BasePlayer player, BaseEntity entity);
    public static class RefundHelper
    {
        private static Dictionary<uint, Dictionary<ItemDefinition, int>> refundItems;
        public static void Refund(BasePlayer player, BaseEntity entity, float percent);
        private static void InitRefundItems();
    }

     Dictionary<string, string> Messages;
}

public static class RefundHelper
{
    private static Dictionary<uint, Dictionary<ItemDefinition, int>> refundItems;
    public static void Refund(BasePlayer player, BaseEntity entity, float percent);
    private static void InitRefundItems();
}


```

---

## BuildingBlockGUI

```csharp
using System;
using System.Linq;
using System.Collections.Generic;
using UnityEngine;
using Oxide.Game.Rust.Cui;

Oxide.Plugins
[Info("BuildingBlockGUI", "wazzzup", "1.1.0")]
[Description("Displays GUI to player when he enters or leaves building block without need of Planner")]
public class BuildingBlockGUI : RustPlugin
{
     List<ulong> activeUI;
    private bool configChanged;
    private float configTimerSeconds;
    private bool configUseTimer;
    private bool configUseImage;
    private string configImageURL;
    private bool configUseGameTips;
    private string configAnchorMin;
    private string configAnchorMax;
    private string configUIColor;
    private string configUITextColor;
     Timer _timer;
    protected override void LoadDefaultConfig();
     void LoadVariables();
    private object GetConfig(string dataValue, object defaultValue);
     void LoadDefaultMessages();
    private string msg(string key, BasePlayer player);
     void Init();
     void OnServerInitialized();
     void Unload();
     void OnPlayerDisconnected(BasePlayer player);
     void DestroyUI(BasePlayer player);
     void CreateUI(BasePlayer player);
     void PluginTimerTick();
}


```

---

## BuildingGrades

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Reflection;
using Facepunch;
using UnityEngine;

Oxide.Plugins
[Info("Building Grades", "bawNg / Nogrod", "0.3.6", ResourceId = 865)]
 class BuildingGrades : RustPlugin
{
    private readonly FieldInfo serverInputField;
    private readonly FieldInfo instancesField;
    private const string Perm;
    private const string PermNoCost;
    private const string PermOwner;
    private const float Distance;
    private readonly HashSet<ulong> runningPlayers;
    private ConfigData configData;
    private readonly Dictionary<string, HashSet<uint>> categories;
     class ConfigData
    {
        public int BatchSize { get; set; }
        public Dictionary<string, HashSet<string>> Categories { get; set; }
    }

    protected override void LoadDefaultConfig();
    private void Init();
     void OnServerInitialized();
    [ChatCommand("up4")]
     void UpCommand4(BasePlayer player, string command, string[] args);
    [ChatCommand("up3")]
     void UpCommand3(BasePlayer player, string command, string[] args);
    [ChatCommand("up2")]
     void UpCommand2(BasePlayer player, string command, string[] args);
    [ChatCommand("up1")]
     void UpCommand1(BasePlayer player, string command, string[] args);
    [ChatCommand("up")]
     void UpCommand(BasePlayer player, string command, string[] args);
    [ChatCommand("down3")]
     void DownCommand3(BasePlayer player, string command, string[] args);
    [ChatCommand("down2")]
     void DownCommand2(BasePlayer player, string command, string[] args);
    [ChatCommand("down1")]
     void DownCommand1(BasePlayer player, string command, string[] args);
    [ChatCommand("down0")]
     void DownCommand0(BasePlayer player, string command, string[] args);
    [ChatCommand("down")]
     void DownCommand(BasePlayer player, string command, string[] args);
     void ChangeBuildingGrade(BasePlayer player, string[] args, bool increment);
    private void DoUpgrade(HashSet<BuildingBlock> all_blocks, int targetGrade, bool increment, BasePlayer player);
    private Dictionary<int, float> GetCosts(HashSet<BuildingBlock> blocks, int targetGrade);
    private bool CanAffordUpgrade(Dictionary<int, float> costs, BasePlayer player);
    private void PayForUpgrade(Dictionary<int, float> costs, BasePlayer player);
    static bool CanUpgrade(BuildingBlock block, BuildingGrade.Enum grade);
    static int NextBlockGrade(BuildingBlock building_block, int targetGrade, int offset);
     Stack<BuildingBlock> GetTargetBuildingBlock(BasePlayer player);
     bool IsAllowed(BasePlayer player);
    private void PrintMessage(BasePlayer player, string msgId, object[] args);
}

 class ConfigData
{
    public int BatchSize { get; set; }
    public Dictionary<string, HashSet<string>> Categories { get; set; }
}


```

---

## BuildingInfo

```csharp
using System;
using System.Collections.Generic;
using System.Text;
using Newtonsoft.Json;
using UnityEngine;

Oxide.Plugins
[Info("Building Info", "misticos", "1.0.7")]
[Description("Scan buildings and get their owners")]
 class BuildingInfo : RustPlugin
{
    private const string PermScan;
    private const string PermOwner;
    private const string PermAuthed;
    private const string PermBypass;
    private Configuration _config;
    public class Configuration
    {
        [JsonProperty(PropertyName = "Command Scan")]
        public string CommandScan;
        [JsonProperty(PropertyName = "Command Scan Owner")]
        public string CommandOwner;
        [JsonProperty(PropertyName = "Command Scan Authorized Players")]
        public string CommandAuthed;
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private void CommandChatScan(BasePlayer player, string command, string[] args);
    private void CommandChatOwner(BasePlayer player, string command, string[] args);
    private void CommandChatAuthed(BasePlayer player, string command, string[] args);
    protected override void LoadDefaultMessages();
    private void Init();
    private string GetMsg(string key, string userId);
    private BaseEntity GetBuilding(BasePlayer player);
}

public class Configuration
{
    [JsonProperty(PropertyName = "Command Scan")]
    public string CommandScan;
    [JsonProperty(PropertyName = "Command Scan Owner")]
    public string CommandOwner;
    [JsonProperty(PropertyName = "Command Scan Authorized Players")]
    public string CommandAuthed;
}


```

---

## BuildingOwners

```csharp
using System.Collections.Generic;
using System;
using System.Reflection;
using System.Data;
using UnityEngine;
using Facepunch;
using Oxide.Core;

Oxide.Plugins
[Info("Building Owners", "Reneb", "3.0.2")]
 class BuildingOwners : RustPlugin
{
    private Core.Configuration.DynamicConfigFile BuildingOwnersData;
    private static bool serverInitialized;
     int constructionLayer;
     string changeownerPermissions;
     void Loaded();
     void OnServerInitialized();
     void OnEntityBuilt(HeldEntity heldentity, GameObject gameobject);
     void OnServerSave();
     void OnServerQuit();
     void OnNewSave(string name);
     void SaveData();
     void SetBlockData(BuildingBlock block, string steamid);
     object FindBlockData(BuildingBlock block);
     string GetMsg(string key, object steamid);
     string GetMsg(string key, BasePlayer player);
     bool hasAccess(BasePlayer player, string permissionName);
     object FindBuilding(BasePlayer player, Vector3 sourcePos, Vector3 sourceDirection);
    [ChatCommand("changeowner")]
     void cmdChatchangeowner(BasePlayer player, string command, string[] args);
}


```

---

## BuildingPrivileges

```csharp
using System.Collections.Generic;
using System.Reflection;
using System;
using System.Data;
using UnityEngine;
using Oxide.Core;

Oxide.Plugins
[Info("BuildingPrivileges", "Reneb", 1.0)]
 class BuildingPrivileges : RustPlugin
{
    private FieldInfo localbuildingPrivileges;
     void Loaded();
    [ChatCommand("bdp")]
     void cmdChatBuildingPrivileges(BasePlayer player, string command, string[] args);
}


```

---

## BuildingProtection

```csharp
using System;
using System.Linq;
using System.Collections;
using System.Collections.Generic;
using Oxide.Core;
using Oxide.Game.Rust.Cui;
using Oxide.Core.Plugins;
using UnityEngine;
using Rust;
using Newtonsoft.Json;

Oxide.Plugins
[Info("BuildingProtection", "Nimant", "1.0.4")]
 class BuildingProtection : RustPlugin
{
    [PluginReference]
    private Plugin NoEscape;
    private const string cdhAlpanRBpDIza;
    private const string vBvxkHaTFkygGpYQwuzSUq;
    private const string nwVBCplpXvAaIKSWWs;
    private static Dictionary<ulong, yiMnwiLrszNVuaAEN> LvQVdOcRKczi;
    private class yiMnwiLrszNVuaAEN
    {
        public float IFKqjvXybmID;
        public int DMhmrrnVKvNem;
        public long akJrcdwqcuUZ;
        public BuildingPrivlidge hmtpabplIUjQXuPIlBFjNVPMJLFVo;
        public List<uint> sXIuBpDbLuyUxjuSqqpnUl;
        public Dictionary<string, float> BqxeHdPCSjIaYcCvbVTWnINaL;
        public Dictionary<string, int> cgbyuoVaBDvaUCsmdsuuUCzMMyHGBr;
    }

    private static bool tqIKYYnczXoFtEtnl;
    private static Dictionary<ulong, IDEEeyYqtzfzZAaNgzawGG> ghIxMrSwhDBlkgYnCv;
    private class IDEEeyYqtzfzZAaNgzawGG
    {
        public int aFJePcIPSmPPltmGcZUQiqXRhOTS;
        public Dictionary<uint, SbfmlOGavlfz> zcAwbVZDcDRxKRifPv;
    }

    private static List<BuildingPrivlidge> VcQBQxmGKPMeQ;
    private static string HdBWieoSOUQFlqrjstNvpyGBTqWeu;
    private static Dictionary<ulong, Dictionary<uint, long>> jiFWAzvYGB;
    private void Init();
    private void OnServerInitialized();
    private void OnPlayerInit(BasePlayer rsQUZEouKGeITBPkGbMXD);
    private void Unload();
    private void OnNewSave();
    private void OnHammerHit(BasePlayer rsQUZEouKGeITBPkGbMXD, HitInfo info);
    private void OnStructureUpgrade(BaseCombatEntity rRhofpSkipGxaeXiPA);
    private void OnEntityTakeDamage(BaseCombatEntity rRhofpSkipGxaeXiPA, HitInfo ucCWTnYiNonwsvvBunNDbG);
    private void OnEntityKill(BaseEntity rRhofpSkipGxaeXiPA);
    private void cmdBP(BasePlayer rsQUZEouKGeITBPkGbMXD, string ZlEandkYxKDZZZIovjiteHiSq, string[] args);
    private void tOWewrFHkzfVeuwaePTjsDHPRwC();
    private void eKkZkmksdVryKYmLmLJYStVf(ulong zgLClBAOijWQCyL);
    private static void BUfsOpNUJSZIHOKcCKQHsUJEzMn(BaseCombatEntity rRhofpSkipGxaeXiPA, HitInfo ucCWTnYiNonwsvvBunNDbG);
    private yiMnwiLrszNVuaAEN BzKAnZYXmLCnkMT(BasePlayer rsQUZEouKGeITBPkGbMXD, BuildingPrivlidge otFWnSlOJn);
    private static bool COmXKLxhnkQjVNVhZwKlO(BasePlayer rsQUZEouKGeITBPkGbMXD);
    private static void eYBjRpbrPjzttOHVuFwllpjVtVf(BasePlayer rsQUZEouKGeITBPkGbMXD);
    private static string tRONBEbkJWOJUIpKaLfxXOFTIl(string qbIhAMtgvDRcLgBkKgBBWwCGVHZ);
    private static BuildingPrivlidge eYUvlfadCaYToFINsWmKbMIE(uint HhzrRtMSZEffWiLtOZtcFEIUdHUN);
    private static List<uint> EqwlSCWRVetitFa(BuildingPrivlidge otFWnSlOJn);
    private static int jTlQhHaDAKWrPKUxCqoPYwcvcHMG(BuildingPrivlidge otFWnSlOJn);
    private static Dictionary<string, float> NJoZKpUkwFhUVQBfVoNCWTeNIIuo(BuildingPrivlidge otFWnSlOJn);
    private static Dictionary<string, float> nqiFvQMDORsHzmUKBAlo(BuildingPrivlidge otFWnSlOJn, float wJLtiFZeqrdzfhizmsJtC);
    private static Dictionary<string, int> WqXXsuHvgzivATTcwWqCOCQTefp(BasePlayer rsQUZEouKGeITBPkGbMXD, List<string> yNBOgvVauStjFZTQHp);
    private static SbfmlOGavlfz mDEgmsqMtmPdHMkyijNXh(BuildingPrivlidge otFWnSlOJn);
    private static string OSrYzuFgFTkfFqBa(BasePlayer rsQUZEouKGeITBPkGbMXD, uint HhzrRtMSZEffWiLtOZtcFEIUdHUN);
    private static string rYVUpJTmdVGiBKbsinI(string QFQHXJdKeKegjOOZOk);
    private static string uOHCcqPxmfWsehkREbRbYiLcLbDogq(int gkIssEuBsqKimzuwjfPmPlBO, bool VgTBBdpHZiLS);
    private string WlRJiciKvTk(long ItHQJBPTwYqtsxqaLKHtmgFjppu, bool ipfLaKxcVLKDNMGVYcsN, ulong zgLClBAOijWQCyL);
    private bool pvSHSIlKRTxJVheYbWvraKzZlb(BasePlayer rsQUZEouKGeITBPkGbMXD);
    private static float SqAOwudNaQicEIczNJ(float rczTOzbQCSssCKOtewRdMniA);
    private static float qDanDNfhirdFKwyOVJnIOBVW(int zsAfnJpOqMuHnmsxqLKnhtLJOb);
    private const string wWtOepewkLdlEswOdnRSBkTwijOZ;
    private void LhObORdPwKeIUEGRhNhGIZ(BasePlayer rsQUZEouKGeITBPkGbMXD, BuildingPrivlidge otFWnSlOJn);
    private void OhqCnOHANvflgxsQ(BasePlayer rsQUZEouKGeITBPkGbMXD, SbfmlOGavlfz cHGqPsCQncUbNPzmtPLVUEDFSA, BuildingPrivlidge otFWnSlOJn);
    private void icBUZELCuDhodqtVQZXlAgguvyMSR(BasePlayer rsQUZEouKGeITBPkGbMXD);
    private void VTFHpkukwgY(BasePlayer rsQUZEouKGeITBPkGbMXD);
    private static void GVJDzFVqjVzvnqTiKnj(BasePlayer rsQUZEouKGeITBPkGbMXD, CuiElementContainer DbSgKCHPUQoVDyWKaQo);
    private static void NymUWdVgVR(CuiElementContainer DbSgKCHPUQoVDyWKaQo, string QFQHXJdKeKegjOOZOk, string MtqMaeVWBs, string GBVMOqUTvNOaEwMFCGrVzLobdGeDU);
    private static void fiNfTratapNQOTqKQ(CuiElementContainer DbSgKCHPUQoVDyWKaQo, string QFQHXJdKeKegjOOZOk, string MtqMaeVWBs, string GBVMOqUTvNOaEwMFCGrVzLobdGeDU, string JOzuVLFiUYauUsE, string mamobRnEmYeBZTqdzzfWGxihqgG);
    private static void dVEBunTpLpLSAABaIpelmVbA(CuiElementContainer DbSgKCHPUQoVDyWKaQo, string LQfeSVZcfE, int qkPDTIfAYJwPvQIhniVh, string MtqMaeVWBs, string GBVMOqUTvNOaEwMFCGrVzLobdGeDU, TextAnchor muxkNvKsDZiSfZhXOJUxFvLdpRh, string JOzuVLFiUYauUsE, string mamobRnEmYeBZTqdzzfWGxihqgG);
    private static void xPyLVNEuusFCKuNIJElKeixH(CuiElementContainer DbSgKCHPUQoVDyWKaQo, string LQfeSVZcfE, int qkPDTIfAYJwPvQIhniVh, string MtqMaeVWBs, string GBVMOqUTvNOaEwMFCGrVzLobdGeDU, TextAnchor muxkNvKsDZiSfZhXOJUxFvLdpRh, string JOzuVLFiUYauUsE, string mamobRnEmYeBZTqdzzfWGxihqgG);
    private static void tguGYRpxXjTCnTsTDTBoyms(CuiElementContainer DbSgKCHPUQoVDyWKaQo, string QFQHXJdKeKegjOOZOk, string LQfeSVZcfE, int qkPDTIfAYJwPvQIhniVh, string MtqMaeVWBs, string GBVMOqUTvNOaEwMFCGrVzLobdGeDU, string ZlEandkYxKDZZZIovjiteHiSq, string font, TextAnchor muxkNvKsDZiSfZhXOJUxFvLdpRh, string JOzuVLFiUYauUsE, string mamobRnEmYeBZTqdzzfWGxihqgG);
    private static void BsmUteByfSSG(CuiElementContainer DbSgKCHPUQoVDyWKaQo, string png, string MtqMaeVWBs, string GBVMOqUTvNOaEwMFCGrVzLobdGeDU, string JOzuVLFiUYauUsE);
    private void QxNidWRapMOxAeEJdwWGvy(BasePlayer rsQUZEouKGeITBPkGbMXD);
    private void ePOrSDyGJhsFlWJeNW(BasePlayer rsQUZEouKGeITBPkGbMXD, long xZQcQFQwWOHfQ, float percent, int OWHOOgESfqMyGqCVjAZCUEyDpnZhY);
    private static Dictionary<ulong, Timer> iSgCIwTPNAWFbhVaWhF;
    private void HSKmDEFdCiebyrOAObPIsISD(BasePlayer rsQUZEouKGeITBPkGbMXD, string gmyDBiESlhtGaypwBUj);
    private static void ynJbMZRfNxnmxMMEcQNeA(BasePlayer rsQUZEouKGeITBPkGbMXD);
    private void wqnridXLTCy();
    private IEnumerator wqnridXLTCy(string wPmPArIBqkoBuVHgwleuLY);
    [ConsoleCommand("bp_12345.close")]
    private void ROKglyToVJBFPoyAhZzhBs(ConsoleSystem.Arg ssIlsReTfpcHcXuN);
    [ConsoleCommand("bp_12345.ok")]
    private void IIahSRUiWnud(ConsoleSystem.Arg ssIlsReTfpcHcXuN);
    [ConsoleCommand("bp_12345.toggleicon")]
    private void zmvHujfPPYnkCbsz(ConsoleSystem.Arg ssIlsReTfpcHcXuN);
    [ConsoleCommand("bp_12345.apply")]
    private void vyCUouNFRtyUNpDqukRMDY(ConsoleSystem.Arg ssIlsReTfpcHcXuN);
    [ConsoleCommand("bp_12345.percent_shift")]
    private void ZOGzkyzszemhi(ConsoleSystem.Arg ssIlsReTfpcHcXuN);
    [ConsoleCommand("bp_12345.hour_shift")]
    private void RbYIUzIAeqRKsU(ConsoleSystem.Arg ssIlsReTfpcHcXuN);
    private static long InhebQTFJTvv(DateTime BlgKFooShdvuNRGuaDIyEXS);
    private static string CRHvtuviAlBWexqFvfnuVzlN(long yTDlDKqBhKxcMlYxJIHkIPR, List<string> hFLHNvcLQWTdYrbMwDsLCv);
    private string qAtenYuUiJYoqqKQCAecaaWAFdE(string xKrSrgXmMgQOeU, ulong ZPgIlsvwveqaNZyUwSQQeBNuSiqF);
    private static Dictionary<string, string> BSGpNGGzTyiGKoKEeOyX;
    private static ConfigData arYyoRcCywByxnRDoH;
    private class ConfigData
    {
        [JsonProperty(PropertyName = "Процент ресурсов от стоимости дома, который будет требоватся на защиту дома в 1% и длительностью 1 час")]
        public float FyFSbfbFsugNCQPEcTYwgViqXvNJYM;
        [JsonProperty(PropertyName = "Коррекционный коэффициент для стоимости защиты в зависимости от количества строительных блоков")]
        public Dictionary<int, float> udJYCpbEzuJHHiaBgHiIjGVDRW;
        [JsonProperty(PropertyName = "Коррекционный коэффициент для стоимости защиты с вип привилегиями")]
        public Dictionary<string, float> byNFtPigVDNuPTqiNFIV;
        [JsonProperty(PropertyName = "Строительные объекты которые требуется защищать (помимо строительных блоков)")]
        public List<string> zJYkgpJHDSsNREfyHhmjf;
        [JsonProperty(PropertyName = "Минимальный разрешенный процент защиты")]
        public int YxdtJNemecgyegrsApTAmpUD;
        [JsonProperty(PropertyName = "Максимальный разрешенный процент защиты")]
        public int huruqQKdraVovZBmpkxJVdJb;
        [JsonProperty(PropertyName = "Шаг изменения процентов")]
        public int vgxvQdfehcBa;
        [JsonProperty(PropertyName = "Коэффициент увеличения стоимости защиты на каждый шаг процента")]
        public float yIUmCjgDLih;
        [JsonProperty(PropertyName = "Минимальное количество часов защиты")]
        public int yczkCuQTZObFoEWVhjRmSgl;
        [JsonProperty(PropertyName = "Максимальное количество часов защиты")]
        public int bKqOdBBuLuEbwaoDWdLftgQl;
        [JsonProperty(PropertyName = "Коэффициент увеличения стоимости защиты на каждый дополнительный час")]
        public float PTAxFMEoLPvdmSJKR;
        [JsonProperty(PropertyName = "Замена стандартных ресурсов на свои ресурсы или предметы")]
        public Dictionary<string, YaeTHASHmpx> UyCiuPAVpIePcZqusTn;
        [JsonProperty(PropertyName = "Названия ресурсов в меню")]
        public Dictionary<string, string> uekIFKWiDT;
        [JsonProperty(PropertyName = "Цвет панели 1")]
        public string gqYWyqIPghYShSuRHooLlhtnT;
        [JsonProperty(PropertyName = "Цвет панели 2")]
        public string PzLkaNIXiEoXsobwBBhcelYZw;
        [JsonProperty(PropertyName = "Цвет кнопки закрытия панели")]
        public string wGvRZXoWPQvBgkfDEzugQDgXBJFJ;
        [JsonProperty(PropertyName = "Цвет панели информационного сообщения")]
        public string yniMHiVFyd;
        [JsonProperty(PropertyName = "Цвет кнопок и акцентированного текста")]
        public string BkHejxOINcSUOzjmiWDahxLg;
        [JsonProperty(PropertyName = "Цвет акцентированного текста для предметов")]
        public string SbbdVbbCRU;
        [JsonProperty(PropertyName = "Ссылка на картинку информирующей о защите дома")]
        public string PiJaDAGVeuFyWiDKactrDRAuhnajhz;
        [JsonProperty(PropertyName = "Позиция иконки (MinX MinY)")]
        public string AnchorMin;
        [JsonProperty(PropertyName = "Позиция иконки (MaxX MaxY)")]
        public string AnchorMax;
        [JsonProperty(PropertyName = "Включать отображение иконки сразу для всех авторизованных в шкафу")]
        public bool AutoEnableIcon;
        [JsonProperty(PropertyName = "Чат команда для активации меню шкафа в зоне действия шкафа (если пусто, - не используется)")]
        public string CommandChatBP;
        [JsonProperty(PropertyName = "Разрешать взрывом по дому проверять наличие у него защиты")]
        public bool aYQnBANSjsFGzKOf;
        [JsonProperty(PropertyName = "Задержка вывода повторного сообщения о наличии защиты у дома (секунды)")]
        public int XsYIeQxdlMoJqCssCQzljApcGdtG;
    }

    private class YaeTHASHmpx
    {
        [JsonProperty(PropertyName = "Название нового ресурса или предмета")]
        public string JDcZYhDHxYvDE;
        [JsonProperty(PropertyName = "Рейт нового ресурса по отношению к одной еденице старого (0 - не использовать вообще)")]
        public float XbTLPuZbhqqvJWRfrC;
    }

    private void htPGTdfQfJPtdtGTDsLmtGTvpxvxMp();
    protected override void LoadDefaultConfig();
    private void oemcSUfxaTkeLYsdBfc(ConfigData RGCFhAPNHsKuPEU);
    private static JMtcnixYfDPaICxDq fFXlNOaDpM;
    private class JMtcnixYfDPaICxDq
    {
        [JsonProperty(PropertyName = "_blocks_")]
        public Dictionary<uint, SbfmlOGavlfz> FJblgubrcofQAqy;
        [JsonProperty(PropertyName = "_players_")]
        public Dictionary<ulong, List<uint>> qYRLmeQQhezbQk;
    }

    private class SbfmlOGavlfz
    {
        [JsonProperty(PropertyName = "_perc_")]
        public float aOcKgYsiyGLZqCTJhFXIezcDCh;
        [JsonProperty(PropertyName = "_time_")]
        public long EGNIlRsVsdVdQSMNvcyj;
        [JsonProperty(PropertyName = "_flag_")]
        public bool YTyclzzTMPRn;
    }

    private void hMKgDXpDLmRwgZeQomY();
    private void OfGZZIBPEJVfix();
}

private class yiMnwiLrszNVuaAEN
{
    public float IFKqjvXybmID;
    public int DMhmrrnVKvNem;
    public long akJrcdwqcuUZ;
    public BuildingPrivlidge hmtpabplIUjQXuPIlBFjNVPMJLFVo;
    public List<uint> sXIuBpDbLuyUxjuSqqpnUl;
    public Dictionary<string, float> BqxeHdPCSjIaYcCvbVTWnINaL;
    public Dictionary<string, int> cgbyuoVaBDvaUCsmdsuuUCzMMyHGBr;
}

private class IDEEeyYqtzfzZAaNgzawGG
{
    public int aFJePcIPSmPPltmGcZUQiqXRhOTS;
    public Dictionary<uint, SbfmlOGavlfz> zcAwbVZDcDRxKRifPv;
}

private class ConfigData
{
    [JsonProperty(PropertyName = "Процент ресурсов от стоимости дома, который будет требоватся на защиту дома в 1% и длительностью 1 час")]
    public float FyFSbfbFsugNCQPEcTYwgViqXvNJYM;
    [JsonProperty(PropertyName = "Коррекционный коэффициент для стоимости защиты в зависимости от количества строительных блоков")]
    public Dictionary<int, float> udJYCpbEzuJHHiaBgHiIjGVDRW;
    [JsonProperty(PropertyName = "Коррекционный коэффициент для стоимости защиты с вип привилегиями")]
    public Dictionary<string, float> byNFtPigVDNuPTqiNFIV;
    [JsonProperty(PropertyName = "Строительные объекты которые требуется защищать (помимо строительных блоков)")]
    public List<string> zJYkgpJHDSsNREfyHhmjf;
    [JsonProperty(PropertyName = "Минимальный разрешенный процент защиты")]
    public int YxdtJNemecgyegrsApTAmpUD;
    [JsonProperty(PropertyName = "Максимальный разрешенный процент защиты")]
    public int huruqQKdraVovZBmpkxJVdJb;
    [JsonProperty(PropertyName = "Шаг изменения процентов")]
    public int vgxvQdfehcBa;
    [JsonProperty(PropertyName = "Коэффициент увеличения стоимости защиты на каждый шаг процента")]
    public float yIUmCjgDLih;
    [JsonProperty(PropertyName = "Минимальное количество часов защиты")]
    public int yczkCuQTZObFoEWVhjRmSgl;
    [JsonProperty(PropertyName = "Максимальное количество часов защиты")]
    public int bKqOdBBuLuEbwaoDWdLftgQl;
    [JsonProperty(PropertyName = "Коэффициент увеличения стоимости защиты на каждый дополнительный час")]
    public float PTAxFMEoLPvdmSJKR;
    [JsonProperty(PropertyName = "Замена стандартных ресурсов на свои ресурсы или предметы")]
    public Dictionary<string, YaeTHASHmpx> UyCiuPAVpIePcZqusTn;
    [JsonProperty(PropertyName = "Названия ресурсов в меню")]
    public Dictionary<string, string> uekIFKWiDT;
    [JsonProperty(PropertyName = "Цвет панели 1")]
    public string gqYWyqIPghYShSuRHooLlhtnT;
    [JsonProperty(PropertyName = "Цвет панели 2")]
    public string PzLkaNIXiEoXsobwBBhcelYZw;
    [JsonProperty(PropertyName = "Цвет кнопки закрытия панели")]
    public string wGvRZXoWPQvBgkfDEzugQDgXBJFJ;
    [JsonProperty(PropertyName = "Цвет панели информационного сообщения")]
    public string yniMHiVFyd;
    [JsonProperty(PropertyName = "Цвет кнопок и акцентированного текста")]
    public string BkHejxOINcSUOzjmiWDahxLg;
    [JsonProperty(PropertyName = "Цвет акцентированного текста для предметов")]
    public string SbbdVbbCRU;
    [JsonProperty(PropertyName = "Ссылка на картинку информирующей о защите дома")]
    public string PiJaDAGVeuFyWiDKactrDRAuhnajhz;
    [JsonProperty(PropertyName = "Позиция иконки (MinX MinY)")]
    public string AnchorMin;
    [JsonProperty(PropertyName = "Позиция иконки (MaxX MaxY)")]
    public string AnchorMax;
    [JsonProperty(PropertyName = "Включать отображение иконки сразу для всех авторизованных в шкафу")]
    public bool AutoEnableIcon;
    [JsonProperty(PropertyName = "Чат команда для активации меню шкафа в зоне действия шкафа (если пусто, - не используется)")]
    public string CommandChatBP;
    [JsonProperty(PropertyName = "Разрешать взрывом по дому проверять наличие у него защиты")]
    public bool aYQnBANSjsFGzKOf;
    [JsonProperty(PropertyName = "Задержка вывода повторного сообщения о наличии защиты у дома (секунды)")]
    public int XsYIeQxdlMoJqCssCQzljApcGdtG;
}

private class YaeTHASHmpx
{
    [JsonProperty(PropertyName = "Название нового ресурса или предмета")]
    public string JDcZYhDHxYvDE;
    [JsonProperty(PropertyName = "Рейт нового ресурса по отношению к одной еденице старого (0 - не использовать вообще)")]
    public float XbTLPuZbhqqvJWRfrC;
}

private class JMtcnixYfDPaICxDq
{
    [JsonProperty(PropertyName = "_blocks_")]
    public Dictionary<uint, SbfmlOGavlfz> FJblgubrcofQAqy;
    [JsonProperty(PropertyName = "_players_")]
    public Dictionary<ulong, List<uint>> qYRLmeQQhezbQk;
}

private class SbfmlOGavlfz
{
    [JsonProperty(PropertyName = "_perc_")]
    public float aOcKgYsiyGLZqCTJhFXIezcDCh;
    [JsonProperty(PropertyName = "_time_")]
    public long EGNIlRsVsdVdQSMNvcyj;
    [JsonProperty(PropertyName = "_flag_")]
    public bool YTyclzzTMPRn;
}


```

---

## BuildingProtection (1)

```csharp
using System;
using System.Linq;
using System.Collections;
using System.Collections.Generic;
using Oxide.Core;
using Oxide.Game.Rust.Cui;
using Oxide.Core.Plugins;
using UnityEngine;
using Rust;
using Newtonsoft.Json;

Oxide.Plugins
[Info("BuildingProtection", "Nimant", "1.0.3")]
 class BuildingProtection : RustPlugin
{
    [PluginReference]
    private Plugin NoEscape;
    private const string OWOVpgnVlfVxxsMuOrNrJ;
    private const string PGMMtZJJBYEHHcAHhhRG;
    private const string ZeNttCwYMfQavz;
    private static Dictionary<ulong, DHyiUPhOmQlUYi> ZpRpfYtdstxkqnrDctmFjHG;
    private class DHyiUPhOmQlUYi
    {
        public float xTmepcJMRSBCYpotIWvVjxncHGpirm;
        public int CKfukblzRm;
        public long UYhfLDMgybnUfsfAr;
        public BuildingPrivlidge daGIJHDKUbwTcWLDqcyvEKyoUgP;
        public List<uint> fUSUsDdJKJxLwFEoG;
        public Dictionary<string, int> InzmNYamAcRouAhooBXjK;
        public Dictionary<string, int> LOiPIabNmehxGwNGwcfrSFOAkxg;
    }

    private static bool DSZRmXskeNPbtuIULZ;
    private static Dictionary<ulong, rcxnpARIqA> jVrZEpCTSbLq;
    private class rcxnpARIqA
    {
        public int GbtXbbXGQhryAd;
        public Dictionary<uint, THKYObKFdbXpSTzeXvejA> VPOgwjDRXYfHDUisEgLjZUynG;
    }

    private static List<BuildingPrivlidge> hqVwQYSwveXkjeRwploLosAbB;
    private static string PfwNpbGXroE;
    private void Init();
    private void OnServerInitialized();
    private void OnPlayerInit(BasePlayer eztFfWeeIhyNfEiSrzLS);
    private void Unload();
    private void OnNewSave();
    private void OnHammerHit(BasePlayer eztFfWeeIhyNfEiSrzLS, HitInfo info);
    private void OnStructureUpgrade(BaseCombatEntity CyGbREYipufEhLlJYkZFPkw);
    private void OnEntityTakeDamage(BaseCombatEntity CyGbREYipufEhLlJYkZFPkw, HitInfo dZmGnyMWbrVRuaEc);
    private void cmdBP(BasePlayer eztFfWeeIhyNfEiSrzLS, string hXITxPqoZrEvoXys, string[] args);
    private void xQWAhngVweKRUulszOTiHptfrUS();
    private void FvqvCLgzoHekniMb(ulong pDktUvdrNoKvwbLOHcCvkdviel);
    private static void CuFTmOvASzbETPIsC(BaseCombatEntity CyGbREYipufEhLlJYkZFPkw, HitInfo dZmGnyMWbrVRuaEc);
    private DHyiUPhOmQlUYi ciaTOTCuDwKfpyfWbXJYIpZMnp(BasePlayer eztFfWeeIhyNfEiSrzLS, BuildingPrivlidge UElTEUsksLPPEdfWq);
    private static bool dYDXPtOHhfykMrMThXyGbs(BasePlayer eztFfWeeIhyNfEiSrzLS);
    private static void ThablvjYgzPPFtapXSKl(BasePlayer eztFfWeeIhyNfEiSrzLS);
    private static string USsAdFTaOOwkGrEVPu(string hSamcTjjkLhpSCRRSZx);
    private static BuildingPrivlidge RyWHwlZMXKbHFnNseXpZCjffJRkv(uint JmSlRHEiXsEwgvVxzC);
    private static List<uint> aThTzBZfWtzCTSmtbpfvpFEJMnb(BuildingPrivlidge UElTEUsksLPPEdfWq);
    private static int KcBlKeRmnM(BuildingPrivlidge UElTEUsksLPPEdfWq);
    private static Dictionary<string, float> hwRhYZSATICNXfBRs(BuildingPrivlidge UElTEUsksLPPEdfWq);
    private static Dictionary<string, int> tmiyvxVvON(BuildingPrivlidge UElTEUsksLPPEdfWq, float LjHZPpcPPRcNDLf);
    private static Dictionary<string, int> WsxxcFKfjohNq(BasePlayer eztFfWeeIhyNfEiSrzLS, List<string> gQyKJTfugWyZSiUzJisDeslf);
    private static THKYObKFdbXpSTzeXvejA GDfNGTzuGYfMtZESO(BuildingPrivlidge UElTEUsksLPPEdfWq);
    private static string SSvJOPDahXrxGAQaDcOCVYDwelq(BasePlayer eztFfWeeIhyNfEiSrzLS, uint JmSlRHEiXsEwgvVxzC);
    private static string MeZRnTsgPJchwzIYpqJDE(string RKEbMvykjk);
    private static string ayqPLbEUYCxOGOq(int mBnsIqPKgEnaOcqESyHZfV, bool twbQkCJGfoUHqHCPQ);
    private string HWaWrTxPyBtfJCWfzUhu(long CfYJiWHBvlwWIRlpuDZQl, bool EUfzGfHpjZYqkHhCHRNLmI, ulong pDktUvdrNoKvwbLOHcCvkdviel);
    private bool oheHITHFJFYiOHPmCfYfunMROyOedP(BasePlayer eztFfWeeIhyNfEiSrzLS);
    private static float neygPZBqOxPeyyCfNHjNlYY(float xQDkpXxGStqJpwaOjmgUSmikWB);
    private static float CDSVHYsJCghKZTqkdWzNszkvBv(int XJfmsxqQYoh);
    private const string ayYSMEgmsLvDAgljdcPIml;
    private void PjoEKarzAEFhBoEh(BasePlayer eztFfWeeIhyNfEiSrzLS, BuildingPrivlidge UElTEUsksLPPEdfWq);
    private void oLFicwVIXrJDOklAOcO(BasePlayer eztFfWeeIhyNfEiSrzLS, THKYObKFdbXpSTzeXvejA kEzXjiiXPmrzNAIOpqcMm, BuildingPrivlidge UElTEUsksLPPEdfWq);
    private void GPdvkxiUBmDiI(BasePlayer eztFfWeeIhyNfEiSrzLS);
    private void jqHEvKPWUOFScgGR(BasePlayer eztFfWeeIhyNfEiSrzLS);
    private static void KuKMfYLEaKPPPFmiqfbkBAUrbf(BasePlayer eztFfWeeIhyNfEiSrzLS, CuiElementContainer CTanCGEFHH);
    private static void apCsaAWiXv(CuiElementContainer CTanCGEFHH, string RKEbMvykjk, string BrOBDQWVVpocLVedYIaYqgtSwEQGJ, string yACafJEmfctMoJBsdaqETMMAeAgKi);
    private static void SuXbKyvVKZKv(CuiElementContainer CTanCGEFHH, string RKEbMvykjk, string BrOBDQWVVpocLVedYIaYqgtSwEQGJ, string yACafJEmfctMoJBsdaqETMMAeAgKi, string gaUGSeRmdtVANhGzTxSRFIZJtHP, string kelJPfHxMZvDS);
    private static void yiDkSeDzLRhkbHKquZShOcqGAyeXZL(CuiElementContainer CTanCGEFHH, string QqbMTvNEKFVg, int DVjZOEcMKV, string BrOBDQWVVpocLVedYIaYqgtSwEQGJ, string yACafJEmfctMoJBsdaqETMMAeAgKi, TextAnchor wuPPWtuwCJp, string gaUGSeRmdtVANhGzTxSRFIZJtHP, string kelJPfHxMZvDS);
    private static void OoYQdZvWCGnJexBWqAOSQWz(CuiElementContainer CTanCGEFHH, string QqbMTvNEKFVg, int DVjZOEcMKV, string BrOBDQWVVpocLVedYIaYqgtSwEQGJ, string yACafJEmfctMoJBsdaqETMMAeAgKi, TextAnchor wuPPWtuwCJp, string gaUGSeRmdtVANhGzTxSRFIZJtHP, string kelJPfHxMZvDS);
    private static void jzZksiWahxYYl(CuiElementContainer CTanCGEFHH, string RKEbMvykjk, string QqbMTvNEKFVg, int DVjZOEcMKV, string BrOBDQWVVpocLVedYIaYqgtSwEQGJ, string yACafJEmfctMoJBsdaqETMMAeAgKi, string hXITxPqoZrEvoXys, string font, TextAnchor wuPPWtuwCJp, string gaUGSeRmdtVANhGzTxSRFIZJtHP, string kelJPfHxMZvDS);
    private static void qYftOzWmucUJ(CuiElementContainer CTanCGEFHH, string png, string BrOBDQWVVpocLVedYIaYqgtSwEQGJ, string yACafJEmfctMoJBsdaqETMMAeAgKi, string gaUGSeRmdtVANhGzTxSRFIZJtHP);
    private void bkCLUKXnmbelgdhAIeIMoc(BasePlayer eztFfWeeIhyNfEiSrzLS);
    private void nDSdiAzYcDUrZTOEZsa(BasePlayer eztFfWeeIhyNfEiSrzLS, long LtBSCPxDGghGy, float percent, int ZIZaiMatAyPWPrLlyRFzqVJ);
    private static Dictionary<ulong, Timer> jriazZYsHvlgHweLXNiFwgzYRjOP;
    private void JHjmeAlSceXEOAqRiuNhMpnsg(BasePlayer eztFfWeeIhyNfEiSrzLS, string ezSHcKJNszZPiCrf);
    private static void ZDVNslHPlv(BasePlayer eztFfWeeIhyNfEiSrzLS);
    private void YdyGyUGPjDyQuoEwgBawGtqbaB();
    private IEnumerator YdyGyUGPjDyQuoEwgBawGtqbaB(string itpoWCRnTJMpocYTkAuDkDNQpyVsor);
    [ConsoleCommand("bp_12345.close")]
    private void UJBQjrkhDdeSMs(ConsoleSystem.Arg caFpWMeDPIgyNMLe);
    [ConsoleCommand("bp_12345.ok")]
    private void xfSgROuDbrNTDAm(ConsoleSystem.Arg caFpWMeDPIgyNMLe);
    [ConsoleCommand("bp_12345.toggleicon")]
    private void YvwlYygWwUfmBhF(ConsoleSystem.Arg caFpWMeDPIgyNMLe);
    [ConsoleCommand("bp_12345.apply")]
    private void fiVLqOSKIemVwZjynS(ConsoleSystem.Arg caFpWMeDPIgyNMLe);
    [ConsoleCommand("bp_12345.percent_shift")]
    private void YkgvHGZGgN(ConsoleSystem.Arg caFpWMeDPIgyNMLe);
    [ConsoleCommand("bp_12345.hour_shift")]
    private void TZyrfWqhSnUDTeLclFkiEC(ConsoleSystem.Arg caFpWMeDPIgyNMLe);
    private static long hpGNPeGnzYRReiWGgloEPvAeufDr(DateTime dsDlrjGGMgUUfoB);
    private static string LXquRVLGGmwxgmKcB(long CoQhYAEfDGx, List<string> glStxPuEyKPmxaXBmhEOEd);
    private string zlBwsfqIZhISnFHdBH(string QHGbRfmUAlPNrWuvKpWfjB, ulong KEFZYWxMzGHkc);
    private static Dictionary<string, string> HhSsvHSFdMIRcQjDGsqeEQRnBGpjRP;
    private static ConfigData HIRsvYQPzWyuvfESFxHaUFIZurtTC;
    private class ConfigData
    {
        [JsonProperty(PropertyName = "Процент ресурсов от стоимости дома, который будет требоватся на защиту дома в 1% и длительностью 1 час")]
        public float QcDimTVYjcB;
        [JsonProperty(PropertyName = "Коррекционный коэффициент для стоимости защиты в зависимости от количества строительных блоков")]
        public Dictionary<int, float> PcLEgwhWxwYXWLBUHMxBSBbSYVv;
        [JsonProperty(PropertyName = "Коррекционный коэффициент для стоимости защиты с вип привилегиями")]
        public Dictionary<string, float> RmrosELnidzVJnFLKUlKeIxGLnRX;
        [JsonProperty(PropertyName = "Строительные объекты которые требуется защищать (помимо строительных блоков)")]
        public List<string> WAwJTluQpKj;
        [JsonProperty(PropertyName = "Минимальный разрешенный процент защиты")]
        public int WUkZrDfquy;
        [JsonProperty(PropertyName = "Максимальный разрешенный процент защиты")]
        public int JdTmRPDwkC;
        [JsonProperty(PropertyName = "Шаг изменения процентов")]
        public int uRkyQkziKUEaKyLdGPminbfEN;
        [JsonProperty(PropertyName = "Коэффициент увеличения стоимости защиты на каждый шаг процента")]
        public float hgTDiegoLXbuTVpHav;
        [JsonProperty(PropertyName = "Минимальное количество часов защиты")]
        public int qQiYjYCTFYGvVlYwaVgSv;
        [JsonProperty(PropertyName = "Максимальное количество часов защиты")]
        public int dQFQKANrXEiOISjoPReFilZ;
        [JsonProperty(PropertyName = "Коэффициент увеличения стоимости защиты на каждый дополнительный час")]
        public float aIVGeziGeitoyycbJRBtiMEXbHqOBW;
        [JsonProperty(PropertyName = "Замена стандартных ресурсов на свои ресурсы или предметы")]
        public Dictionary<string, TxkvFQxiOYonNxThqkY> ESiTmtHBTeohwpzBGeUW;
        [JsonProperty(PropertyName = "Названия ресурсов в меню")]
        public Dictionary<string, string> gKOyVvBdMhstfyHffixIQDfEbhpLvj;
        [JsonProperty(PropertyName = "Цвет панели 1")]
        public string huotRVpEiisGCL;
        [JsonProperty(PropertyName = "Цвет панели 2")]
        public string pHgijKkhafaWaQAWyCNIwZXHzcG;
        [JsonProperty(PropertyName = "Цвет кнопки закрытия панели")]
        public string chnJkKVjBAkIgjSFJaGUm;
        [JsonProperty(PropertyName = "Цвет панели информационного сообщения")]
        public string TGaJzdOECkNQlmE;
        [JsonProperty(PropertyName = "Цвет кнопок и акцентированного текста")]
        public string YyPzjwnofbDqeDtrAFPpCAqcXGDkM;
        [JsonProperty(PropertyName = "Цвет акцентированного текста для предметов")]
        public string rZGBuvEfDiwFzCUjsXDuPhBCyGEo;
        [JsonProperty(PropertyName = "Ссылка на картинку информирующей о защите дома")]
        public string WJJmQmItrSjbZCphagKfVlPQnMVi;
        [JsonProperty(PropertyName = "Позиция иконки (MinX MinY)")]
        public string AnchorMin;
        [JsonProperty(PropertyName = "Позиция иконки (MaxX MaxY)")]
        public string AnchorMax;
        [JsonProperty(PropertyName = "Включать отображение иконки сразу для всех авторизованных в шкафу")]
        public bool AutoEnableIcon;
        [JsonProperty(PropertyName = "Чат команда для активации меню шкафа в зоне действия шкафа (если пусто, - не используется)")]
        public string CommandChatBP;
    }

    private class TxkvFQxiOYonNxThqkY
    {
        [JsonProperty(PropertyName = "Название нового ресурса или предмета")]
        public string XPWVFkAIoCXaSFsTVAzqSSiGI;
        [JsonProperty(PropertyName = "Рейт нового ресурса по отношению к одной еденице старого (0 - не использовать вообще)")]
        public float YYRShJFRcBfcbPCquuXjgjjMQDHQT;
    }

    private void FbUwtQnmPCLjJgk();
    protected override void LoadDefaultConfig();
    private void RaUuSZVKIAvrtfTR(ConfigData DeyFbVbpxSFvNvCWafbkdKUcsSbp);
    private static ceuARwUFPXh htjoTujQiNY;
    private class ceuARwUFPXh
    {
        [JsonProperty(PropertyName = "_blocks_")]
        public Dictionary<uint, THKYObKFdbXpSTzeXvejA> iCsBlkTpLGdFOvVb;
        [JsonProperty(PropertyName = "_players_")]
        public Dictionary<ulong, List<uint>> HVMhRoaoCievfPDLpLtyzLrnHS;
    }

    private class THKYObKFdbXpSTzeXvejA
    {
        [JsonProperty(PropertyName = "_perc_")]
        public float cdspsASrrXBdzATOFNpbJmxD;
        [JsonProperty(PropertyName = "_time_")]
        public long dTyvbttwmBMF;
        [JsonProperty(PropertyName = "_flag_")]
        public bool jUEuKEKgYDmZEzqLbOAwZjRc;
    }

    private void wZjNGtqHGNwmqtfUizKyMOX();
    private void mQzhRoJNMQhRYNGRRGouBHwpzchVGn();
}

private class DHyiUPhOmQlUYi
{
    public float xTmepcJMRSBCYpotIWvVjxncHGpirm;
    public int CKfukblzRm;
    public long UYhfLDMgybnUfsfAr;
    public BuildingPrivlidge daGIJHDKUbwTcWLDqcyvEKyoUgP;
    public List<uint> fUSUsDdJKJxLwFEoG;
    public Dictionary<string, int> InzmNYamAcRouAhooBXjK;
    public Dictionary<string, int> LOiPIabNmehxGwNGwcfrSFOAkxg;
}

private class rcxnpARIqA
{
    public int GbtXbbXGQhryAd;
    public Dictionary<uint, THKYObKFdbXpSTzeXvejA> VPOgwjDRXYfHDUisEgLjZUynG;
}

private class ConfigData
{
    [JsonProperty(PropertyName = "Процент ресурсов от стоимости дома, который будет требоватся на защиту дома в 1% и длительностью 1 час")]
    public float QcDimTVYjcB;
    [JsonProperty(PropertyName = "Коррекционный коэффициент для стоимости защиты в зависимости от количества строительных блоков")]
    public Dictionary<int, float> PcLEgwhWxwYXWLBUHMxBSBbSYVv;
    [JsonProperty(PropertyName = "Коррекционный коэффициент для стоимости защиты с вип привилегиями")]
    public Dictionary<string, float> RmrosELnidzVJnFLKUlKeIxGLnRX;
    [JsonProperty(PropertyName = "Строительные объекты которые требуется защищать (помимо строительных блоков)")]
    public List<string> WAwJTluQpKj;
    [JsonProperty(PropertyName = "Минимальный разрешенный процент защиты")]
    public int WUkZrDfquy;
    [JsonProperty(PropertyName = "Максимальный разрешенный процент защиты")]
    public int JdTmRPDwkC;
    [JsonProperty(PropertyName = "Шаг изменения процентов")]
    public int uRkyQkziKUEaKyLdGPminbfEN;
    [JsonProperty(PropertyName = "Коэффициент увеличения стоимости защиты на каждый шаг процента")]
    public float hgTDiegoLXbuTVpHav;
    [JsonProperty(PropertyName = "Минимальное количество часов защиты")]
    public int qQiYjYCTFYGvVlYwaVgSv;
    [JsonProperty(PropertyName = "Максимальное количество часов защиты")]
    public int dQFQKANrXEiOISjoPReFilZ;
    [JsonProperty(PropertyName = "Коэффициент увеличения стоимости защиты на каждый дополнительный час")]
    public float aIVGeziGeitoyycbJRBtiMEXbHqOBW;
    [JsonProperty(PropertyName = "Замена стандартных ресурсов на свои ресурсы или предметы")]
    public Dictionary<string, TxkvFQxiOYonNxThqkY> ESiTmtHBTeohwpzBGeUW;
    [JsonProperty(PropertyName = "Названия ресурсов в меню")]
    public Dictionary<string, string> gKOyVvBdMhstfyHffixIQDfEbhpLvj;
    [JsonProperty(PropertyName = "Цвет панели 1")]
    public string huotRVpEiisGCL;
    [JsonProperty(PropertyName = "Цвет панели 2")]
    public string pHgijKkhafaWaQAWyCNIwZXHzcG;
    [JsonProperty(PropertyName = "Цвет кнопки закрытия панели")]
    public string chnJkKVjBAkIgjSFJaGUm;
    [JsonProperty(PropertyName = "Цвет панели информационного сообщения")]
    public string TGaJzdOECkNQlmE;
    [JsonProperty(PropertyName = "Цвет кнопок и акцентированного текста")]
    public string YyPzjwnofbDqeDtrAFPpCAqcXGDkM;
    [JsonProperty(PropertyName = "Цвет акцентированного текста для предметов")]
    public string rZGBuvEfDiwFzCUjsXDuPhBCyGEo;
    [JsonProperty(PropertyName = "Ссылка на картинку информирующей о защите дома")]
    public string WJJmQmItrSjbZCphagKfVlPQnMVi;
    [JsonProperty(PropertyName = "Позиция иконки (MinX MinY)")]
    public string AnchorMin;
    [JsonProperty(PropertyName = "Позиция иконки (MaxX MaxY)")]
    public string AnchorMax;
    [JsonProperty(PropertyName = "Включать отображение иконки сразу для всех авторизованных в шкафу")]
    public bool AutoEnableIcon;
    [JsonProperty(PropertyName = "Чат команда для активации меню шкафа в зоне действия шкафа (если пусто, - не используется)")]
    public string CommandChatBP;
}

private class TxkvFQxiOYonNxThqkY
{
    [JsonProperty(PropertyName = "Название нового ресурса или предмета")]
    public string XPWVFkAIoCXaSFsTVAzqSSiGI;
    [JsonProperty(PropertyName = "Рейт нового ресурса по отношению к одной еденице старого (0 - не использовать вообще)")]
    public float YYRShJFRcBfcbPCquuXjgjjMQDHQT;
}

private class ceuARwUFPXh
{
    [JsonProperty(PropertyName = "_blocks_")]
    public Dictionary<uint, THKYObKFdbXpSTzeXvejA> iCsBlkTpLGdFOvVb;
    [JsonProperty(PropertyName = "_players_")]
    public Dictionary<ulong, List<uint>> HVMhRoaoCievfPDLpLtyzLrnHS;
}

private class THKYObKFdbXpSTzeXvejA
{
    [JsonProperty(PropertyName = "_perc_")]
    public float cdspsASrrXBdzATOFNpbJmxD;
    [JsonProperty(PropertyName = "_time_")]
    public long dTyvbttwmBMF;
    [JsonProperty(PropertyName = "_flag_")]
    public bool jUEuKEKgYDmZEzqLbOAwZjRc;
}


```

---

## BuildingProtector

```csharp
using System;
using System.Collections.Generic;
using Rust;

Oxide.Plugins
[Info("Building Protector", "Onyx", "1.2.1", ResourceId=1200)]
 class BuildingProtector : RustPlugin
{
     bool configChanged;
     string chatPrefix;
     string chatPrefixColor;
     string defaultDaysProtected;
     int defaultStartHours;
     int defaultStartMinutes;
     int defaultStartSeconds;
     int defaultEndHours;
     int defaultEndMinutes;
     int defaultEndSeconds;
     string daysProtected;
     int startHours;
     int startMinutes;
     int startSeconds;
     int endHours;
     int endMinutes;
     int endSeconds;
     bool defaultProtectAllBuildingBlocks;
     bool defaultInformPlayer;
     float defaultInformInterval;
     bool protectAllBuildingBlocks;
     bool informPlayer;
     float informInterval;
     string defaultInformMessage;
     string informMessage;
     class OnlinePlayer
    {
        public BasePlayer Player;
        public float LastInformTime;
        public OnlinePlayer(BasePlayer player);
    }

    [OnlinePlayers]
     Hash<BasePlayer, OnlinePlayer> onlinePlayers;
     DateTime epoch;
     Protector protector;
    protected override void LoadDefaultConfig();
     void Loaded();
     void dailyLoop(float timeToWait);
     class Protector : RustPlugin
    {
        readonly BuildingProtector parent;
        public DateTime startRaid;
        public DateTime endRaid;
        public int toStart;
        public int toEnd;
        public Protector(BuildingProtector parent);
         void loop();
    }

     void LoadVariables();
     void OnEntityTakeDamage(BaseCombatEntity entity, HitInfo info);
     void OnPlayerInit(BasePlayer player);
     void Log(string message);
     void SendChatMessage(BasePlayer player, string message, object[] arguments);
     object GetConfigValue(string category, string setting, object defaultValue);
    private long GetTimestamp();
    [ChatCommand("raid")]
    private void SaveCommand(BasePlayer player, string command, string[] args);
}

 class OnlinePlayer
{
    public BasePlayer Player;
    public float LastInformTime;
    public OnlinePlayer(BasePlayer player);
}

 class Protector : RustPlugin
{
    readonly BuildingProtector parent;
    public DateTime startRaid;
    public DateTime endRaid;
    public int toStart;
    public int toEnd;
    public Protector(BuildingProtector parent);
     void loop();
}


```

---

## BuildingRestriction

```csharp
using System;
using System.Collections;
using System.Collections.Generic;
using System.Linq;
using UnityEngine;

Oxide.Plugins
[Info("BuildingRestriction", "Jakkee", "1.1.3", ResourceId = 2124)]
 class BuildingRestriction : RustPlugin
{
    private List<string> AllowedBuildingBlocks;
    private float MaxHeight;
    private int MaxTFoundations;
    private int MaxFoundations;
    private string PermBypass;
    private string TriangleFoundation;
    private string Foundation;
     Dictionary<uint, List<BuildingBlock>> buildingids;
    protected override void LoadDefaultConfig();
     void Loaded();
     Dictionary<string, string> messages;
     void CheckConfig();
     void OnServerInitialized();
    protected void ReloadConfig();
     void UpdateDictionary();
     void OnEntityBuilt(Planner planner, GameObject gameobject);
     void OnEntityDeath(BaseCombatEntity entity, HitInfo info);
     void OnStructureDemolish(BaseCombatEntity entity, BasePlayer player);
    private int GetCountOf(List<BuildingBlock> ConnectingStructure, string buildingobject);
    private T GetConfig(string name, T defaultValue);
     string Lang(string key, string id, object[] args);
     bool HasPermission(string id, string perm);
}


```

---

## BuildingSkins

```csharp
using Oxide.Core;
using Oxide.Core.Libraries.Covalence;
using Oxide.Game.Rust.Cui;
using Oxide.Core.Plugins;
using System.Collections;
using System.Collections.Generic;
using System.Linq;
using UnityEngine;
using Newtonsoft.Json;

Oxide.Plugins
[Info("Building Skins", "Marat", "2.0.3")]
[Description("Automatic application of DLC skins for building blocks")]
 class BuildingSkins : RustPlugin
{
    [PluginReference]
    private Plugin ImageLibrary;
    private const string InitialLayer;
    private const string permissionUse;
    private const string permissionAll;
    private const string permissionBuild;
    private const string permissionAdmin;
    private readonly Dictionary<ulong, Coroutine> runningCoroutines;
    private readonly Dictionary<BuildingGrade.Enum, List<ulong>> gradesSkin;
    private readonly Dictionary<uint, string> colors;
    private void OnServerInitialized();
    private void Unload();
    private void OnServerSave();
    private void OnPlayerConnected(BasePlayer player);
    private void OnPlayerDisconnected(BasePlayer player);
    private void OnPlayerRespawned(BasePlayer player);
    private void OnHammerHit(BasePlayer player, HitInfo info);
    private object OnStructureUpgrade(BuildingBlock block, BasePlayer player, BuildingGrade.Enum grade, ulong skin);
    private void OnStructureGradeUpdated(BuildingBlock block, BasePlayer player, BuildingGrade.Enum oldGrade, BuildingGrade.Enum newGrade);
    private static PluginConfig config;
    private class PluginConfig
    {
        [JsonProperty("Building skin change commands")]
        public string[] Commands;
        [JsonProperty("Block building skin in building blocked")]
        public bool BuildingBlocked;
        [JsonProperty("Number of blocks updated per tick")]
        public int UpdatesPerTick;
        [JsonProperty("Use separate permissions for skins")]
        public bool SeparatePermissions;
        [JsonProperty("Image and description settings")]
        public Dictionary<int, List<BlockInfo>> BuildingImages;
        public Oxide.Core.VersionNumber Version;
    }

    protected override void LoadDefaultConfig();
    private class BlockInfo
    {
        public string Title;
        public string Url;
        public ulong SkinId;
        public BlockInfo(string title, string url, ulong skinId);
    }

    protected override void SaveConfig();
    protected override void LoadConfig();
    [ConsoleCommand("UI_BuildingController")]
    private void CmdConsoleHandler(ConsoleSystem.Arg arg);
    private void CmdChangeSkin(IPlayer ipPlayer, string command, string[] arg);
    private void StartCoroutine(BasePlayer player, IEnumerator routine);
    private void StopCoroutine(BasePlayer player);
    private IEnumerator UpgradeSkin(BasePlayer player, BuildingBlock[] blocks);
    private bool HasPermission(BasePlayer player, string name);
    private ulong GetPlayerSkinID(BasePlayer player, BuildingGrade.Enum grade);
    private BuildingBlock GetLookEntity(BasePlayer player);
    private static void SoundEffect(BasePlayer player, string effect);
    private string GetMessage(string key, BasePlayer player);
    private void InitializeLayers(BasePlayer player, bool update);
    private void ImageLayers(BasePlayer player, int index, int skinIndex);
    private void SettingsLayer(BasePlayer player);
    private void ColorLayer(BasePlayer player, int index);
    private IEnumerator PreloadImages(BasePlayer player);
    protected override void LoadDefaultMessages();
    private StoredData storedData;
    private class StoredData
    {
        public Dictionary<ulong, Data> PlayerData;
    }

    private class Data
    {
        [JsonProperty(DefaultValueHandling = DefaultValueHandling.Ignore)]
        public bool ChangeHammer;
        [JsonProperty(DefaultValueHandling = DefaultValueHandling.Ignore)]
        public bool NeedsRepair;
        [JsonProperty(DefaultValueHandling = DefaultValueHandling.Ignore)]
        public bool EnableAnimation;
        [JsonProperty(DefaultValueHandling = DefaultValueHandling.Ignore)]
        public bool RandomColor;
        [JsonProperty(DefaultValueHandling = DefaultValueHandling.Ignore)]
        public uint Color;
        [JsonProperty(DefaultValueHandling = DefaultValueHandling.Ignore)]
        public ulong Wood;
        [JsonProperty(DefaultValueHandling = DefaultValueHandling.Ignore)]
        public ulong Stone;
        [JsonProperty(DefaultValueHandling = DefaultValueHandling.Ignore)]
        public ulong Metal;
        [JsonProperty(DefaultValueHandling = DefaultValueHandling.Ignore)]
        public ulong TopTier;
    }

    private void SaveData();
    private void LoadData();
}

private class PluginConfig
{
    [JsonProperty("Building skin change commands")]
    public string[] Commands;
    [JsonProperty("Block building skin in building blocked")]
    public bool BuildingBlocked;
    [JsonProperty("Number of blocks updated per tick")]
    public int UpdatesPerTick;
    [JsonProperty("Use separate permissions for skins")]
    public bool SeparatePermissions;
    [JsonProperty("Image and description settings")]
    public Dictionary<int, List<BlockInfo>> BuildingImages;
    public Oxide.Core.VersionNumber Version;
}

private class BlockInfo
{
    public string Title;
    public string Url;
    public ulong SkinId;
    public BlockInfo(string title, string url, ulong skinId);
}

private class StoredData
{
    public Dictionary<ulong, Data> PlayerData;
}

private class Data
{
    [JsonProperty(DefaultValueHandling = DefaultValueHandling.Ignore)]
    public bool ChangeHammer;
    [JsonProperty(DefaultValueHandling = DefaultValueHandling.Ignore)]
    public bool NeedsRepair;
    [JsonProperty(DefaultValueHandling = DefaultValueHandling.Ignore)]
    public bool EnableAnimation;
    [JsonProperty(DefaultValueHandling = DefaultValueHandling.Ignore)]
    public bool RandomColor;
    [JsonProperty(DefaultValueHandling = DefaultValueHandling.Ignore)]
    public uint Color;
    [JsonProperty(DefaultValueHandling = DefaultValueHandling.Ignore)]
    public ulong Wood;
    [JsonProperty(DefaultValueHandling = DefaultValueHandling.Ignore)]
    public ulong Stone;
    [JsonProperty(DefaultValueHandling = DefaultValueHandling.Ignore)]
    public ulong Metal;
    [JsonProperty(DefaultValueHandling = DefaultValueHandling.Ignore)]
    public ulong TopTier;
}


```

---

## BuildingUpgrade

```csharp
using Oxide.Core;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using System;
using System.Collections.Generic;
using System.Linq;
using UnityEngine;

Oxide.Plugins
[Info("Building Upgrade", "OxideBro", "1.1.22")]
 class BuildingUpgrade : RustPlugin
{
    [PluginReference]
    private Plugin NoEscape;
    [PluginReference]
     Plugin Remove;
    private void PayForUpgrade(ConstructionGrade g, BasePlayer player);
    private ConstructionGrade GetGrade(BuildingBlock block, BuildingGrade.Enum iGrade);
    private bool CanAffordUpgrade(BuildingBlock block, BuildingGrade.Enum iGrade, BasePlayer player);
     Dictionary<BuildingGrade.Enum, string> gradesString;
     Dictionary<BasePlayer, BuildingGrade.Enum> grades;
     Dictionary<BasePlayer, int> timers;
    public Timer mytimer;
    private int resetTime;
    private string permissionAutoGrade;
    private string permissionAutoGradeFree;
    private string permissionAutoGradeHammer;
    private bool permissionAutoGradeAdmin;
    private bool getBuild;
    private bool permissionOn;
    private bool useNoEscape;
    private bool InfoNotice;
    private int InfoNoticeSize;
    private string InfoNoticeText;
    private int InfoNoticeTextTime;
    private bool CanUpgradeDamaged;
    private string PanelAnchorMin;
    private string PanelAnchorMax;
    private string PanelColor;
    private int TextFontSize;
    private string TextСolor;
    private string TextAnchorMin;
    private string TextAnchorMax;
    private string MessageAutoGradePremHammer;
    private string MessageAutoGradePrem;
    private string MessageAutoGradeNo;
    private string MessageAutoGradeOn;
    private string MessageAutoGradeOff;
    private string ChatCMD;
    private string ConsoleCMD;
    private bool EnabledRemove;
    private void LoadDefaultConfig();
    private void GetConfig(string MainMenu, string Key, T var);
     void cmdAutoGrade(BasePlayer player, string command, string[] args);
     void consoleAutoGrade(ConsoleSystem.Arg arg, string[] args);
    private void Init();
     void OnServerInitialized();
    private void OnActiveItemChanged(BasePlayer player, Item newItem);
    private void OnPlayerInput(BasePlayer player, InputState input);
     void Unload();
     void ShowUIInfo(BasePlayer player);
     void OnHammerHit(BasePlayer player, HitInfo info);
     void OnEntitySpawned(BaseNetworkable entity);
     void Grade(BuildingBlock block, BasePlayer player);
     int NextGrade(int grade);
     void GradeTimerHandler();
     void DrawUI(BasePlayer player, BuildingGrade.Enum grade, int seconds);
     void DestroyUI(BasePlayer player);
    private string GUI;
     void UpdateTimer(BasePlayer player, ulong playerid);
     object BuildingUpgradeActivate(ulong id);
     void BuildingUpgradeDeactivate(ulong id);
}


```

---

## BuildingWorkbench

```csharp
using System.Collections;
using System.Collections.Generic;
using System.ComponentModel;
using System.Linq;
using Newtonsoft.Json;
using Oxide.Core.Plugins;
using UnityEngine;

Oxide.Plugins
[Info("Building Workbench", "", "1.1.2")]
[Description("Расширяет диапазон верстака для работы внутри всего здания")]
public class BuildingWorkbench : RustPlugin
{
    [PluginReference]
    private readonly Plugin GameTipAPI;
    private PluginConfig _pluginConfig;
    private TriggerBase _triggerBase;
    private GameObject _object;
    private const string UsePermission;
    private const string CancelCraftIgnorePermission;
    private const string AccentColor;
    private readonly List<ulong> _notifiedPlayer;
    private readonly Hash<ulong, int> _playerLevel;
    private Coroutine _routine;
    private bool _init;
    private void Init();
    protected override void LoadDefaultMessages();
    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    private void OnServerInitialized();
    private void OnPlayerConnected(BasePlayer player);
    private void OnPlayerDisconnected(BasePlayer player, string reason);
    private void Unload();
    private void StartUpdatingWorkbench();
    private IEnumerator HandleWorkbenchUpdate();
    private void UpdatePlayerPriv(BasePlayer player, Hash<uint, int> cache);
    private void UpdatePlayerBench(BasePlayer player, int level, float checkOffset);
    private void UpdateNearbyPlayers(Vector3 pos, uint buildingId, BasePlayer player);
    private void OnPlayerLeftBuilding(BasePlayer player);
    private void OnEntityLeave(TriggerBase trigger, BaseEntity entity);
    private void OnEntitySpawned(Workbench bench);
    private void OnEntityKill(Workbench bench);
    private void OnCupboardAuthorize(BuildingPrivlidge privilege, BasePlayer player);
    private void OnCupboardDeauthorize(BuildingPrivlidge privilege, BasePlayer player);
    private void OnAuthChanged(BasePlayer player);
    private void OnCupboardClearList(BuildingPrivlidge privilege, BasePlayer player);
    private int GetBuildingWorkbenchLevel(uint buildingId);
    private void Chat(BasePlayer player, string format, object[] args);
    private bool HasPermission(BasePlayer player, string perm);
    private string Lang(string key, BasePlayer player, object[] args);
    private class PluginConfig
    {
        [DefaultValue(true)]
        [JsonProperty(PropertyName = "Enable Notifications")]
        public bool EnableNotifications { get; set; }
        [DefaultValue(false)]
        [JsonProperty(PropertyName = "Cancel craft when leaving building")]
        public bool CancelCraft { get; set; }
        [DefaultValue(true)]
        [JsonProperty(PropertyName = "Cancel craft notification")]
        public bool CancelCraftNotification { get; set; }
        [DefaultValue(3f)]
        [JsonProperty(PropertyName = "Update Rate (Seconds)")]
        public float UpdateRate { get; set; }
    }

    private class LangKeys
    {
        public const string Chat;
        public const string Notification;
        public const string CraftCanceled;
    }

}

private class PluginConfig
{
    [DefaultValue(true)]
    [JsonProperty(PropertyName = "Enable Notifications")]
    public bool EnableNotifications { get; set; }
    [DefaultValue(false)]
    [JsonProperty(PropertyName = "Cancel craft when leaving building")]
    public bool CancelCraft { get; set; }
    [DefaultValue(true)]
    [JsonProperty(PropertyName = "Cancel craft notification")]
    public bool CancelCraftNotification { get; set; }
    [DefaultValue(3f)]
    [JsonProperty(PropertyName = "Update Rate (Seconds)")]
    public float UpdateRate { get; set; }
}

private class LangKeys
{
    public const string Chat;
    public const string Notification;
    public const string CraftCanceled;
}


```

---

## BuildingWrapper

```csharp
using System.Collections.Generic;
using System;
using System.Reflection;
using System.Linq;
using UnityEngine;
using Oxide.Core.Plugins;
using Facepunch;

Oxide.Plugins
[Info("BuildingWrapper", "ignignokt84", "0.1.3", ResourceId = 1798)]
 class BuildingWrapper : RustPlugin
{
     void LoadDefaultMessages();
    private const string ZoneManagerPermZone;
    [PluginReference]
     Plugin ZoneManager;
    static FieldInfo serverinput;
    private readonly FieldInfo instancesField;
    private int layerMasks;
    public string usageString;
    private const float yAdjust;
    private Collider[] colBuffer;
     void Loaded();
    private void OnServerInitialized();
    private void OnPluginLoaded(Plugin plugin);
    private void OnPluginUnloaded(Plugin plugin);
     string GetMessage(string key, string userId);
     void showUsage(BasePlayer player);
     void cmdChatDelegator(BasePlayer player, string command, string[] args);
     void WrapBuilding(BasePlayer player, string zoneId, Option shape, float buffer, Command cmd);
     HashSet<BuildingBlock> getZoneEntities(BasePlayer player, string zoneId, Option zoneShape);
     Vector3 parseVector3(string str);
     bool GetRaycastTarget(BasePlayer player, object closestEntity);
     bool GetStructure(HashSet<BuildingBlock> initialBlocks, HashSet<BuildingBlock> structure);
     bool WrapBox(BasePlayer player, string zoneId, HashSet<BuildingBlock> blocks, float buffer);
     bool WrapSphere(string zoneId, HashSet<BuildingBlock> blocks, float buffer);
    private Vector2[] rotateAll(Vector2[] v, float angle, Vector2 center);
    private Vector2 rotate(Vector2 v, float angle, Vector2 center);
     float getAngle(Vector2 p0, Vector2 p1);
     Vector2[] constructHull(Vector2[] points);
     float isLeft(Vector2 p0, Vector2 p1, Vector2 p2);
     Vector2 getCenterFromHull(Vector2[] hull);
    static string wrapSize(int size, string input);
    static string wrapColor(string color, string input);
     void drawHull(BasePlayer player, Vector2[] hull, Vector3 center);
     void drawCenter(BasePlayer player, Vector3 center);
    private static bool isAdmin(BasePlayer player);
    private bool hasPermission(BasePlayer player, string permname);
    public void BoxColliders(Vector3 position, Vector3 halfExtents, Quaternion orientation, List<T> list, int layerMask, QueryTriggerInteraction triggerInteraction);
    public void BoxEntities(Vector3 position, Vector3 halfExtents, Quaternion orientation, List<T> list, int layerMask, QueryTriggerInteraction triggerInteraction);
}


```

---

## BuildProtection

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using Facepunch;
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using UnityEngine;

Oxide.Plugins
[Info("BuildProtection", "CASHR#6906", "1.0.1")]
internal class BuildProtection : RustPlugin
{
    [PluginReference]
    private Plugin ImageLibrary;
    private readonly Dictionary<BasePlayer, BuildingPrivlidge> TCList;
    private Configuration _config;
    private class Configuration
    {
        [JsonProperty(PropertyName = "The list of privileges (The name of the permission and the maximum percentage of protection)", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public readonly Dictionary<string, int> PermList;
        [JsonProperty(PropertyName = "The cost of 100% home protection", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public readonly List<ItemToProtect> PriceList;
        [JsonProperty(PropertyName = "Setting the time (in hours) and multiplying the coefficients affecting the cost", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public readonly Dictionary<int, float> TimeList;
        internal class ItemToProtect
        {
            [JsonProperty("Amount")]
            public int Amount;
            [JsonProperty("displayName in UI")]
            public string displayName;
            [JsonProperty("Picture to display in the UI (leave blank if the standard item)")]
            public string Image;
            [JsonProperty("SHORTNAME")]
            public string ShortName;
            [JsonProperty("SkinID")]
            public ulong skinID;
        }

    }

    protected override void LoadConfig();
    protected override void SaveConfig();
    protected override void LoadDefaultConfig();
    private int GetMaxProtection(string userID);
    private void OnServerInitialized();
    private void Unload();
    private void OnEntityTakeDamage(BuildingBlock entity, HitInfo info);
    private void OnEntityTakeDamage(DecayEntity entity, HitInfo info);
    private void OnHammerHit(BasePlayer player, HitInfo info);
    private void Take(IEnumerable<Item> itemList, string shortname, ulong skinId, int iAmount);
    [ConsoleCommand("UI_BUILDPROTECTION")]
    private void cmdConsoleUI_BUILDPROTECTION(ConsoleSystem.Arg arg);
    private void ShowProtectUI(BasePlayer player, Data data);
    private void ShowUI(BasePlayer player, int procent, int time);
    private static int ItemCount(IReadOnlyList<Item> items, string shortname, ulong skin);
    private void Outline(CuiElementContainer container, string parent, string color, string size);
    protected override void LoadDefaultMessages();
    private string GetMessage(string langKey, string steamID);
    private string GetMessage(string langKey, string steamID, object[] args);
    private Dictionary<ulong, Data> _data;
    private class Data
    {
        public DateTime FinishProtection;
        public bool IsProtect;
        public int Protection;
    }

    private void LoadData();
    private void OnServerSave();
    private void SaveData();
}

private class Configuration
{
    [JsonProperty(PropertyName = "The list of privileges (The name of the permission and the maximum percentage of protection)", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public readonly Dictionary<string, int> PermList;
    [JsonProperty(PropertyName = "The cost of 100% home protection", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public readonly List<ItemToProtect> PriceList;
    [JsonProperty(PropertyName = "Setting the time (in hours) and multiplying the coefficients affecting the cost", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public readonly Dictionary<int, float> TimeList;
    internal class ItemToProtect
    {
        [JsonProperty("Amount")]
        public int Amount;
        [JsonProperty("displayName in UI")]
        public string displayName;
        [JsonProperty("Picture to display in the UI (leave blank if the standard item)")]
        public string Image;
        [JsonProperty("SHORTNAME")]
        public string ShortName;
        [JsonProperty("SkinID")]
        public ulong skinID;
    }

}

internal class ItemToProtect
{
    [JsonProperty("Amount")]
    public int Amount;
    [JsonProperty("displayName in UI")]
    public string displayName;
    [JsonProperty("Picture to display in the UI (leave blank if the standard item)")]
    public string Image;
    [JsonProperty("SHORTNAME")]
    public string ShortName;
    [JsonProperty("SkinID")]
    public ulong skinID;
}

private class Data
{
    public DateTime FinishProtection;
    public bool IsProtect;
    public int Protection;
}


```

---

## BuildTools

```csharp
using System;
using System.Collections;
using System.Collections.Generic;
using System.Globalization;
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
[Info("Build Tools", "Mevent", "1.3.2")]
public class BuildTools : RustPlugin
{
    [PluginReference]
    private Plugin ImageLibrary;
    private Plugin NoEscape;
    private Plugin Clans;
    private Plugin Friends;
    private Plugin Notify;
    private Plugin UINotify;
    private const string Layer;
    private static BuildTools _instance;
    private const string PermAll;
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
        public static void Get(BasePlayer player);
        public static void Remove(BasePlayer player);
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
        [JsonProperty(PropertyName = "Permission (ex: buildtools.1)")]
        public string Permission;
    }

    protected override void LoadConfig();
    protected override void SaveConfig();
    protected override void LoadDefaultConfig();
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
    public static void Get(BasePlayer player);
    public static void Remove(BasePlayer player);
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
    [JsonProperty(PropertyName = "Permission (ex: buildtools.1)")]
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


```

---

## BulletProjectile

```csharp
using System;
using System.Collections.Generic;
using UnityEngine;
using Rust;
using ProtoBuf;
using Oxide.Core.Plugins;
using System.Linq;

Oxide.Plugins
[Info("BulletProjectile", "Karuza", "01.0.00")]
public class BulletProjectile : RustPlugin
{
    [HookMethod("ShootProjectile")]
    public void ShootProjectile(BasePlayer owner, Vector3 muzzlePos, Vector3 velocity, List<DamageTypeEntry> damageTypeOverrides, string ammoPrefabPathOverride);
    public class ProjectileSystem : MonoBehaviour
    {
        public float gravityModifier;
        public float penetrationPower;
        public MinMax damageDistances;
        public MinMax damageMultipliers;
        public List<DamageTypeEntry> damageTypes;
        [NonSerialized]
        public float integrity;
        [NonSerialized]
        public float maxDistance;
        [NonSerialized]
        public Projectile.Modifier modifier;
        [Header("Attributes")]
        public Vector3 initialVelocity;
        [Tooltip("This projectile will raycast for this many units, and then become a projectile. This is typically done for bullets.")]
        public float initialDistance;
        [Header("Impact Rules")]
        [Range(0.0f, 1f)]
        public float ricochetChance;
        [Header("Rendering")]
        public ScaleRenderer rendererToScale;
        public ScaleRenderer firstPersonRenderer;
        [Header("Tumble")]
        public float tumbleSpeed;
        [NonSerialized]
        public BasePlayer owner;
        public string projectilePrefabPath { get; set; }
        [NonSerialized]
        public uint seed;
        [NonSerialized]
        public bool clientsideEffect;
        [NonSerialized]
        public bool clientsideAttack;
        [NonSerialized]
        public bool invisible;
        private Vector3 currentVelocity;
        private Vector3 currentPosition;
        private float traveledDistance;
        private float traveledTime;
        private Vector3 sentPosition;
        private Vector3 previousPosition;
        private float previousTraveledTime;
        private bool isRetiring;
        private HitTest hitTest;
        protected static Effect reusableInstance;
        public void InitializeBullet(Vector3 location);
        public void CalculateDamage(HitInfo info, Projectile.Modifier mod, float scale);
        public bool isAuthoritative { get; set; }
        private bool isAlive { get; set; }
        private void Retire();
        private void Cleanup();
        public void AdjustVelocity(Vector3 delta);
        public void InitializeVelocity(Vector3 overrideVel);
        protected void OnDisable();
        protected void FixedUpdate();
        protected void Update();
        private void UpdateVelocity(float deltaTime);
        private void DoVelocityUpdate(float deltaTime);
        private void DoMovement(float deltaTime);
        private bool DoWaterHit(HitTest test, Vector3 targetPosition);
        private bool DoRicochet(HitTest test, Vector3 point, Vector3 normal);
        public static Attack BuildAttackMessage(HitTest test);
        private bool DoHit(HitTest test, Vector3 point, Vector3 normal);
        public static void LoadFromAttack(HitInfo info, Attack attack, bool serverSide);
        private bool Reflect(uint seed, Vector3 point, Vector3 normal);
        private bool Refract(uint seed, Vector3 point, Vector3 normal, float resistance);
        private Vector3 Refract(Vector3 v, Vector3 n, float f);
        private Quaternion RandomRotation(uint seed, float range);
        public static uint Xorshift(uint x);
        internal void Launch();
    }

    public static class GameTrace
    {
        public static void TraceAll(HitTest test, List<TraceInfo> traces, int layerMask);
        public static Transform GetTransform(Transform bone, Vector3 position, BaseEntity entity);
    }

}

public class ProjectileSystem : MonoBehaviour
{
    public float gravityModifier;
    public float penetrationPower;
    public MinMax damageDistances;
    public MinMax damageMultipliers;
    public List<DamageTypeEntry> damageTypes;
    [NonSerialized]
    public float integrity;
    [NonSerialized]
    public float maxDistance;
    [NonSerialized]
    public Projectile.Modifier modifier;
    [Header("Attributes")]
    public Vector3 initialVelocity;
    [Tooltip("This projectile will raycast for this many units, and then become a projectile. This is typically done for bullets.")]
    public float initialDistance;
    [Header("Impact Rules")]
    [Range(0.0f, 1f)]
    public float ricochetChance;
    [Header("Rendering")]
    public ScaleRenderer rendererToScale;
    public ScaleRenderer firstPersonRenderer;
    [Header("Tumble")]
    public float tumbleSpeed;
    [NonSerialized]
    public BasePlayer owner;
    public string projectilePrefabPath { get; set; }
    [NonSerialized]
    public uint seed;
    [NonSerialized]
    public bool clientsideEffect;
    [NonSerialized]
    public bool clientsideAttack;
    [NonSerialized]
    public bool invisible;
    private Vector3 currentVelocity;
    private Vector3 currentPosition;
    private float traveledDistance;
    private float traveledTime;
    private Vector3 sentPosition;
    private Vector3 previousPosition;
    private float previousTraveledTime;
    private bool isRetiring;
    private HitTest hitTest;
    protected static Effect reusableInstance;
    public void InitializeBullet(Vector3 location);
    public void CalculateDamage(HitInfo info, Projectile.Modifier mod, float scale);
    public bool isAuthoritative { get; set; }
    private bool isAlive { get; set; }
    private void Retire();
    private void Cleanup();
    public void AdjustVelocity(Vector3 delta);
    public void InitializeVelocity(Vector3 overrideVel);
    protected void OnDisable();
    protected void FixedUpdate();
    protected void Update();
    private void UpdateVelocity(float deltaTime);
    private void DoVelocityUpdate(float deltaTime);
    private void DoMovement(float deltaTime);
    private bool DoWaterHit(HitTest test, Vector3 targetPosition);
    private bool DoRicochet(HitTest test, Vector3 point, Vector3 normal);
    public static Attack BuildAttackMessage(HitTest test);
    private bool DoHit(HitTest test, Vector3 point, Vector3 normal);
    public static void LoadFromAttack(HitInfo info, Attack attack, bool serverSide);
    private bool Reflect(uint seed, Vector3 point, Vector3 normal);
    private bool Refract(uint seed, Vector3 point, Vector3 normal, float resistance);
    private Vector3 Refract(Vector3 v, Vector3 n, float f);
    private Quaternion RandomRotation(uint seed, float range);
    public static uint Xorshift(uint x);
    internal void Launch();
}

public static class GameTrace
{
    public static void TraceAll(HitTest test, List<TraceInfo> traces, int layerMask);
    public static Transform GetTransform(Transform bone, Vector3 position, BaseEntity entity);
}


```

---

## BuriedTreasure

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
[Info("BuriedTreasure", "Colon Blow", "1.0.11")]
[Description("Treasure on your server")]
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
            [JsonProperty(PropertyName = "Loot Table - Basic Treasure Chest (shortname, max qty) ")]
            public Dictionary<string, int> BasicLootTable { get; set; }
            [JsonProperty(PropertyName = "Loot Table - UnCommon Treasure Chest (shortname, max qty) ")]
            public Dictionary<string, int> UnCommonLootTable { get; set; }
            [JsonProperty(PropertyName = "Loot Table - Rare Treasure Chest (shortname, max qty) ")]
            public Dictionary<string, int> RareLootTable { get; set; }
            [JsonProperty(PropertyName = "Loot Table - Elite Treasure Chest (shortname, max qty) ")]
            public Dictionary<string, int> EliteLootTable { get; set; }
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
         Dictionary<string, int> loottable;
         bool isvisible;
         bool didspawnchest;
         float despawncounter;
         float detectionradius;
         void Awake();
        private void OnTriggerEnter(Collider col);
         void SpawnTreasureChest();
         void AddLootTableItems(BaseEntity treasurebox);
         void AddCustomLoot(BaseEntity treasurebox, int qauntity, int itemid);
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
        [JsonProperty(PropertyName = "Loot Table - Basic Treasure Chest (shortname, max qty) ")]
        public Dictionary<string, int> BasicLootTable { get; set; }
        [JsonProperty(PropertyName = "Loot Table - UnCommon Treasure Chest (shortname, max qty) ")]
        public Dictionary<string, int> UnCommonLootTable { get; set; }
        [JsonProperty(PropertyName = "Loot Table - Rare Treasure Chest (shortname, max qty) ")]
        public Dictionary<string, int> RareLootTable { get; set; }
        [JsonProperty(PropertyName = "Loot Table - Elite Treasure Chest (shortname, max qty) ")]
        public Dictionary<string, int> EliteLootTable { get; set; }
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
    [JsonProperty(PropertyName = "Loot Table - Basic Treasure Chest (shortname, max qty) ")]
    public Dictionary<string, int> BasicLootTable { get; set; }
    [JsonProperty(PropertyName = "Loot Table - UnCommon Treasure Chest (shortname, max qty) ")]
    public Dictionary<string, int> UnCommonLootTable { get; set; }
    [JsonProperty(PropertyName = "Loot Table - Rare Treasure Chest (shortname, max qty) ")]
    public Dictionary<string, int> RareLootTable { get; set; }
    [JsonProperty(PropertyName = "Loot Table - Elite Treasure Chest (shortname, max qty) ")]
    public Dictionary<string, int> EliteLootTable { get; set; }
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
     Dictionary<string, int> loottable;
     bool isvisible;
     bool didspawnchest;
     float despawncounter;
     float detectionradius;
     void Awake();
    private void OnTriggerEnter(Collider col);
     void SpawnTreasureChest();
     void AddLootTableItems(BaseEntity treasurebox);
     void AddCustomLoot(BaseEntity treasurebox, int qauntity, int itemid);
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

