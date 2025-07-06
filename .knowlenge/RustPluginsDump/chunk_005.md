# uMod Plugins Dataset - Code Abstractions (Continued)

Chunk 5 - Generated: 2025-07-06 20:39:06

## IQReportSystem-2.23.90

```csharp
using Oxide.Game.Rust.Cui;
using System.Text;
using System.Collections.Generic;
using Oxide.Core.Libraries;
using Object = System.Object;
using ConVar;
using Newtonsoft.Json;
using Facepunch.Utility;
using System.Linq;
using Network;
using Oxide.Core;
using Net = Network.Net;
using System.Text.RegularExpressions;
using UnityEngine;
using UnityEngine.UI;
using Oxide.Core.Plugins;
using UnityEngine.Networking;
using System.Collections;
using Newtonsoft.Json.Linq;
using Oxide.Core.Libraries.Covalence;
using System;
using Time = UnityEngine.Time;

Oxide.Plugins
[Info("IQReportSystem", "Mercury", "2.23.90")]
[Description("One love IQReportSystem")]
internal class IQReportSystem : RustPlugin
{
    private Boolean IsClans(String userID, String targetID);
    private class ImageUi
    {
        private static Coroutine coroutineImg;
        private static Dictionary<String, String> Images;
        private static List<String> KeyImages;
        public static void DownloadImages();
        private static IEnumerator AddImage();
        public static string GetImage(String ImgKey);
        public static void Initialize();
        public static void Unload();
    }

    private Boolean CanStartedChecked(BasePlayer Target);
    [ConsoleCommand("iqrs")]
    private void ConsoleCommandReport(ConsoleSystem.Arg arg);
    private void DrawUI_ModeratorStitistics_Banner(BasePlayer Moderator, String OffsetMin, String OffsetMax, String TitleBanner, String ArgBanner, String AdditionalText, Int32 CountRaiting);
    private String GetImage(String fileName, UInt64 skin);
    private void OnPlayerConnected(BasePlayer player);
    private void DrawUI_ModeratorStitistics_Banner_AdditionalText(BasePlayer Moderator, String AdditionalText);
    private void DrawUI_ShowPoopUP(BasePlayer player, String displayName, String userID);
    private void WriteData();
    private void DrawUI_PoopUp_Moderator_Panel_Info(BasePlayer player, BasePlayer Target, String TitlePanel, List<Configuration.LangText> InfoList, String OffsetMin, String OffsetMax, List<String> AlternativeInfoList);
    private List<Fields> DT_StopCheck(UInt64 TargetID, BasePlayer Moderator, Boolean AutoStop, Boolean IsConsole, Configuration.ReasonReport Verdict);
    private void StartPluginLoad();
    private void CheckStatusPlayer(BasePlayer player, String ReasonDisconnected);
    private static Configuration config;
    private String CorrectedClanName(BasePlayer player);
    private void DrawUI_TemplatePlayer_Moderator_IsSteam(BasePlayer player, String UserID);
    private void AddCooldown(UInt64 SenderID, UInt64 TargetID);
    private void DrawUI_Player_Alert(BasePlayer player);
    private void SendRaitingModerator(UInt64 ModeratorID, Int32 IndexAchive, Int32 StarAmount);
    private void ReadData();
    private Dictionary<UInt64, Timer> TimerWaitPlayer;
    private List<String> GetServersBansRCC(UInt64 TargetID);
    private void OnPlayerDisconnected(BasePlayer player, String reason);
    private List<Fields> DT_PlayerSendContact(BasePlayer Sender, String Contact);
    public Dictionary<BasePlayer, Coroutine> RoutineSounds;
    private Dictionary<UInt64, LocalRepositoryOzProtect> OzProtect_LocalRepository;
    protected override void SaveConfig();
    public class OzResponse
    {
        public int unixtime { get; set; }
        public string reason { get; set; }
        public string proofid { get; set; }
        public bool pirate { get; set; }
        public bool active { get; set; }
        public bool reliable { get; set; }
        public bool unnecessary { get; set; }
        public OzServer server { get; set; }
        public string admin { get; set; }
        public int game { get; set; }
        public string date { get; set; }
        public int bantime { get; set; }
    }

    private void DrawUI_Moderator_Checked_Menu_Discord(BasePlayer moderator, String Discord);
    private String GetClanTag(String userID);
    private void DrawUI_Raiting_Menu_Stars(BasePlayer player, Int32 SelectedAmount, UInt64 ModeratorID);
    public Boolean IsValidStartChecked(BasePlayer Target, BasePlayer Moderator, Boolean IsConsole);
    protected override void LoadDefaultConfig();
    private void SendChat(String Message, BasePlayer player, ConVar.Chat.ChatChannel channel);
    private class InterfaceBuilder
    {
        public static InterfaceBuilder Instance;
        public const String UI_LAYER;
        public const String UI_REPORT_PANEL;
        public const String UI_REPORT_LEFT_PANEL;
        public const String UI_REPORT_PLAYER_PANEL;
        public const String UI_REPORT_POOPUP_PLAYER;
        public const String UI_REPORT_POOPUP_MODERATOR;
        public const String UI_REPORT_MODERATOR_STATISTICS;
        public const String UI_REPORT_MODERATOR_MENU_CHECKED;
        public const String UI_REPORT_RAITING_PLAYER_PANEL;
        public const String UI_REPORT_PLAYER_ALERT;
        public Dictionary<String, String> Interfaces;
        public InterfaceBuilder();
        public static void AddInterface(String name, String json);
        public static String GetInterface(String name);
        public static void DestroyAll();
        private void Building_Bacgkround();
        private void Building_HeaderPanel_Search();
        private void Building_Profile_Moderator_Stats();
        private void Building_Profile_Template_Banner();
        private void Building_Profile_Template_Banner_AdditionalText();
        private void Building_Profile_Template_Banner_AdditionalImg();
        private void Building_Panel_Players();
        private void Building_PageController();
        private void Building_PlayerTemplate();
        private void Building_PoopUp_Player();
        private void Building_PoopUp_Reason();
        private void Building_PlayerTemplate_Moderator();
        private void Building_PlayerTemplate_Moderator_IsSteam();
        private void Building_Left_Menu();
        private void Building_Button_Template();
        private void Building_PoopUP_Moderator();
        private void Building_PoopUP_Moderator_InfoBlock();
        private void Building_Text_Template_Moderator_Block_Info();
        private void Building_Moderator_Menu();
        private void Building_ModeratorMenu_Verdict_Button();
        private void Building_ModeratorMenuChecked_InfoOnline();
        private void Building_ModeratorMenuChecked_InfoDiscord();
        private void Building_DropList_Reasons();
        private void Building_Raiting_Menu();
        private void Building_Raiting_Select_Button();
        private void Building_Player_Alert();
    }

    public Boolean HasImage(String imageName);
     void OnPlayerBanned(string name, ulong id, string address, string reason);
    private void DrawUI_ModeratorStitistics_Banner_RaitingImage(BasePlayer Moderator, Int32 X);
    private List<String> GetServersCheckRCC(UInt64 TargetID);
    private void DrawUI_PoopUp_Moderator_InfoText(BasePlayer player, Int32 Y, String Text);
    private void DrawUI_PageController(BasePlayer player, Int32 Page, Boolean IsModerator, String SearchName);
    private void ChatCommandDiscord(BasePlayer player, String cmd, String[] args);
    private void SendVK(String Message);
    private Boolean IsClansStartChecked(String userID, String targetID);
    internal class LocalRepositoryRCC
    {
        public List<String> LastChecksServers;
        public List<String> LastBansServers;
    }

    private new void LoadDefaultMessages();
    private void RequestVK(String Message);
    private static Double CurrentTime { get; set; }
    private void OnServerInitialized();
    internal class LocalRepositoryOzProtect
    {
        public List<String> LastBansServers;
    }

    private void SendReportPlayer(BasePlayer Sender, UInt64 TargetID, Int32 ReasonIndex);
    private void DrawUI_LeftMenu(BasePlayer player, Boolean IsModerator);
    public Boolean IsFake(UInt64 userID);
    public class NpcSound
    {
        [JsonConverter(typeof(SoundFileConverter))]
        public List<byte[]> Data;
    }

    private List<byte[]> FromSaveData(byte[] bytes);
    private void Request(String url, String payload, Action<Int32> callback);
    private void SendPlayerDiscord(BasePlayer player, String Discord);
    private void DrawUI_PlayerPanel(BasePlayer player, Int32 Page, Boolean IsModerator, String SearchName);
    protected override void LoadConfig();
    private readonly Dictionary<UInt64, ProcessCheckRepository> PlayerChecks;
    private Dictionary<UInt64, LocalRepositoryRCC> RCC_LocalRepository;
    private List<String> GetServersBansOzProtect(UInt64 TargetID);
    private void DrawUI_TemplatePlayer(BasePlayer player, Int32 X, Int32 Y, String NickName, String UserID);
    private const String HideMenuPermissions;
    private class SoundFileConverter : JsonConverter
    {
        public override void WriteJson(JsonWriter writer, object value, JsonSerializer serializer);
        public override object ReadJson(JsonReader reader, Type objectType, object existingValue, JsonSerializer serializer);
        public override bool CanConvert(Type objectType);
    }

    private IEnumerator StartAfkCheck(BasePlayer Target, BasePlayer Moderator, Boolean IsConsole, Boolean SkipAFK);
    private void SendDiscord(String Webhook, List<Fields> fields, Authors Authors, Int32 Color);
    private void ShowPlayersList(BasePlayer player, List<BasePlayer> playersList, List<FakePlayer> fakePlayers, Boolean IsModerator);
    private String VKT_ChangeStatus(Boolean IsModerator, String PlayerName, String UserID, String StatusConnection);
    private void DrawUI_ModeratorStatistics(BasePlayer Moderator);
    private void DrawUI_Moderator_Checked_Menu(BasePlayer moderator, UInt64 TargetID);
    private void DrawUI_Reason_Raiting_Or_Moderator_Menu(BasePlayer moderator, String Text, String ParentUI, String NameLayer, String OffsetMin, String OffsetMax, String Command);
    private void DrawUI_HeaderUI_Search(BasePlayer player, Boolean IsModerator);
    private void StartCheckedPlayer(BasePlayer Target, BasePlayer Moderator, Boolean IsConsole);
    public class Authors
    {
        public String name { get; set; }
        public String url { get; set; }
        public String icon_url { get; set; }
        public String proxy_icon_url { get; set; }
        public Authors(String name, String url, String icon_url, String proxy_icon_url);
    }

    private void Unload();
     object OnPlayerChat(BasePlayer player, String message, Chat.ChatChannel channel);
    public class OzResult
    {
        public string status { get; set; }
        public List<OzResponse> response { get; set; }
    }

    private void DrawUI_PoopUp_Moderator(BasePlayer player, BasePlayer Target);
    private class PlayerRepository
    {
        public Int32 GetCooldownLeft(UInt64 TargetID);
        private Boolean IsReportedPlayer(UInt64 TargetID);
        public Boolean IsCooldown(UInt64 TargetID);
        public Double Cooldown;
        public void AddCooldown(UInt64 TargetID);
        public Boolean IsRepeatReported(UInt64 TargetID);
        public Dictionary<UInt64, Double> ReportedList;
    }

    [PluginReference]
     Plugin IQChat;
     Plugin ImageLibrary;
     Plugin IQFakeActive;
     Plugin NoEscape;
     Plugin EventHelper;
     Plugin Battles;
     Plugin Duel;
     Plugin Duelist;
     Plugin ArenaTournament;
     Plugin Friends;
     Plugin Clans;
     Plugin MultiFighting;
     Plugin StopDamageMan;
    private void GetPlayerCheckServerOzProtect(UInt64 TargetID);
    private void GetPlayerCheckServerRCC(UInt64 TargetID);
    private void BanPlayerOzProtect(UInt64 TargetID, UInt64 ModerID, String ReasonBan);
    private void StartCheckOzProtect(UInt64 TargetID, UInt64 ModerID);
    private class ProcessCheckRepository
    {
        public String DiscordTarget;
        public String DisplayName;
        public UInt64 ModeratorID;
    }

    private String GetUrlVK(String Message);
    private void StopCheckedPlayer(UInt64 TargetID, BasePlayer Moderator, Boolean AutoStop, Boolean IsConsole);
    private void SoundPlay(BasePlayer player);
    private Boolean IsCombatBlock(BasePlayer Target);
    private String VKT_PlayerSendReport(BasePlayer Sender, UInt64 TargetID, String Reason);
    private Boolean IsFriendSendReport(UInt64 userID, UInt64 targetID);
    private void CheckStatusModerator(BasePlayer Moderator, String ReasonDisconnected);
    private String VKT_StopCheck(UInt64 TargetID, BasePlayer Moderator, Boolean AutoStop, Boolean IsConsole, Configuration.ReasonReport Verdict);
    public class OzServer
    {
        public string name { get; set; }
        public string ico { get; set; }
        public string overlay { get; set; }
        public string desc { get; set; }
        public string site { get; set; }
        public bool pirate { get; set; }
        public int game { get; set; }
        public string ip { get; set; }
    }

    public String FindFakeName(ulong userID);
    private Authors GetAuthorDiscord(Configuration.NotifyDiscord.Webhooks.TemplatesNotify templatesNotify);
    private Boolean IsFriendStartChecked(UInt64 userID, UInt64 targetID);
    private Boolean IsClansSendReport(String userID, String targetID);
    [ConsoleCommand("report.panel")]
    private void FuncionalCommandReport(ConsoleSystem.Arg arg);
    private class ModeratorInformation
    {
        public Int32 AmountChecked;
        public Int32 AmountBans;
        public Int32 AverageRaiting;
        public List<Int32> OneScore;
        public List<Int32> TwoScore;
        public List<Int32> ThreeScore;
        public Int32 GetAverageRaiting();
        public Int32 GetAverageRaitingAchive(List<Int32> Score);
    }

    private Dictionary<UInt64, PlayerRepository> PlayerRepositories;
    private class Response
    {
        public List<String> last_ip;
        public String last_nick;
        public List<UInt64> another_accs;
        public List<last_checks> last_check;
        public class last_checks
        {
            public UInt64 moderSteamID;
            public String serverName;
            public Int32 time;
        }

        public List<RustCCBans> bans;
        public class RustCCBans
        {
            public Int32 banID;
            public String reason;
            public String serverName;
            public Int32 OVHserverID;
            public Int32 banDate;
        }

    }

    private String VKT_StartCheck(BasePlayer Target, BasePlayer Moderator, Boolean IsConsole);
    private void BanPlayerRCC(UInt64 TargetID, String ReasonBan);
    private Boolean IsRaidBlock(BasePlayer Target);
    private Dictionary<BasePlayer, Coroutine> AfkCheckRoutine;
    private static IQReportSystem _;
    private void DrawUI_LeftMenu_Button(BasePlayer player, String TitleButton, String IconButton, String ColorButton, String OffsetMin, String OffsetMax, String Command);
    private List<Fields> DT_PlayerMaxReport(String PlayerName, Int32 Reports, UInt64 TargetID);
    private void RegisteredPlayer(BasePlayer player);
    private void DrawUI_TemplatePlayer_Moderator(BasePlayer player, Int32 Y, String UserID, String NickName);
    private void SendSound(UInt64 netId, byte[] data);
    public class Fields
    {
        public String name { get; set; }
        public String value { get; set; }
        public bool inline { get; set; }
        public Fields(String name, String value, bool inline);
    }

    private Dictionary<UInt64, Timer> TimerWaitChecked;
    private String VKT_PlayerMaxReport(String PlayerName, Int32 Reports, UInt64 TargetID);
    private List<String> GetTeamsNames(BasePlayer Target);
    private void Init();
    private static InterfaceBuilder _interface;
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

    private void StopDamageAdd(BasePlayer player);
    private class Configuration
    {
        [JsonProperty(LanguageEn ? "Setting up compatibility with IQReportSystemr" : "Настройка совместимостей с IQReportSystem")]
        public References ReferenceSettings;
        [JsonProperty(LanguageEn ? "Color setting" : "Настройка цветов")]
        public Colors ColorsSettings;
        [JsonProperty(LanguageEn ? "Setting images" : "Настройка изображений")]
        public Images ImagesSettings;
        [JsonProperty(LanguageEn ? "List of reports and reasons for blocking" : "Список жалоб и причин блокировки")]
        public List<ReasonReport> ReasonList;
        [JsonProperty(LanguageEn ? "Setting up sending complaints via F7 or the RUST game menu" : "Настройка отправки жалоб через F7 или игровое меню RUST")]
        public ReportF7AndGameMenu ReportF7AndGameMenuSettings;
        [JsonProperty(LanguageEn ? "Setting up the Player check process" : "Настройка процесса проверки игрока")]
        public CheckController CheckControllerSettings;
        [JsonProperty(LanguageEn ? "Setting up moderator notifications and the maximum number of reports" : "Настройка уведомления модераторов и максимального количества репортов")]
        public ReportContollerModeration ReportContollerModerationSettings;
        [JsonProperty(LanguageEn ? "Setting up sending complaints by players" : "Настройка отправки жалоб игроками")]
        public ReportSendController ReportSendControllerSettings;
        [JsonProperty(LanguageEn ? "Additional verdict setting" : "Дополнительная настройка вынесения вердикта")]
        public VerdictController VerdictControllerSettings;
        [JsonProperty(LanguageEn ? "Configuring notifications for all players about actions in the plugin" : "Настройка уведомлений для всех игроков о действиях в плагине")]
        public NotifyChat NotifyChatSettings;
        [JsonProperty(LanguageEn ? "Setting up notifications in Discord" : "Настройка уведомлений в Discord")]
        public NotifyDiscord NotifyDiscordSettings;
        [JsonProperty(LanguageEn ? "Setting up notifications in VK" : "Настройка уведомлений в VK")]
        public NotifyVK NotifyVKSettings;
        [JsonProperty(LanguageEn ? "Command to send data when calling for verification (console and chat)" : "Команда для отправки данных при вызове на проверку (консольная и чатовая)")]
        public String CommandForContact;
        internal class VerdictController
        {
            [JsonProperty(LanguageEn ? "Banned all the `Friends` of a player who has been given a verdict by a moderator (true - yes/false - no)" : "Блокировать всех `Друзей` игрока, которому вынес вердикт модератор (true - да/false - нет)")]
            public Boolean UseBanAllTeam;
            [JsonProperty(LanguageEn ? "Index from the list of complaints when blocking a player's `Friends` (From your list - starts from 0)" : "Индекс из списка жалоб при блокировки `Друзей` игрока  (Из вашего списка - начинается от 0)")]
            public Int32 IndexBanReason;
        }

        internal class CheckController
        {
            [JsonProperty(LanguageEn ? "Use sound notification for players when calling for verification (true - yes / false - no) [You must upload sound files to the IQSystem/IQReportSystem/Sounds folder]" : "Использовать звуковое оповещение для игроков при вызове на проверку (true - да/false - нет) [Вы должны загрузить файлы со звуком в папку IQSystem/IQReportSystem/Sounds]")]
            public Boolean UseSoundAlert;
            [JsonProperty(LanguageEn ? "Record a demos of the player during his check" : "Записывать демо игрока во время его проверки")]
            public Boolean UseDemo;
            [JsonProperty(LanguageEn ? "Use AFK validation before calling for check (true - yes / false - no)" : "Использовать проверку на AFK перед вызовом на проверку (true - да/false - нет)")]
            public Boolean UseCheckAFK;
            [JsonProperty(LanguageEn ? "Cancel check for a player automatically with saving reports if he left the server for 15 minutes or more (true - yes/false - no)" : "Отменять проверку игроку автоматически с сохранением репортов если он покинул сервер на 15 минут и более (true - да/false - нет)")]
            public Boolean StopCheckLeavePlayer;
            [JsonProperty(LanguageEn ? "Use tracking of crafting of invaders by the player (will notify the moderator about it) (true - yes / false - no)" : "Использовать отслеживание крафта предметов игроком (будет уведомлять модератора об этом) (true - да/false - нет)")]
            public Boolean TrackCrafting;
            [JsonProperty(LanguageEn ? "Use tracking of messages sent to the chat by the player (will notify the moderator about it) (true - yes / false - no)" : "Использовать отслеживание отправки сообщений в чат игроком (будет уведомлять модератора об этом) (true - да/false - нет)")]
            public Boolean TrackChat;
            [JsonProperty(LanguageEn ? "Use player command usage tracking (will notify the moderator about it) (true - yes / false - no)" : "Использовать отслеживание использования команд игроком (будет уведомлять модератора об этом) (true - да/false - нет)")]
            public Boolean TrackCommand;
        }

        internal class ReportContollerModeration
        {
            [JsonProperty(LanguageEn ? "Maximum number of reports to display the player in the moderator menu and moderator notifications" : "Максимальное количество репортов для отображения игрока в меню модератора и уведомления модератора")]
            public Int32 ReportCountTrigger;
            [JsonProperty(LanguageEn ? "Setting up moderator notifications about the maximum number of player reports" : "Настройка уведомления модератора о максимальном количестве репортов игрока")]
            public AlertModeration AlertModerationSettings;
            internal class AlertModeration
            {
                [JsonProperty(LanguageEn ? "Notify the moderator that the player has scored the maximum number of reports" : "Уведомлять модератора о том, что игрок набрал максимальное количество репортов")]
                public Boolean AlertModerator;
                [JsonProperty(LanguageEn ? "Enable an audio notification to the moderator during the notification of the number of reports" : "Включать звуковое уведомление модератору во время уведомления о количестве репортов")]
                public Boolean AlertSound;
                [JsonProperty(LanguageEn ? "The path to the notification sound (this is the path of the prefab of the game - you can see here : https://github.com/OrangeWulf/Rust-Docs/blob/master/Extended/Effects.md)" : "Путь до звука уведомления (это путь префаба игры - посмотреть можно тут : https://github.com/OrangeWulf/Rust-Docs/blob/master/Extended/Effects.md)")]
                public String PathSound;
            }

        }

        internal class ReportSendController
        {
            [JsonProperty(LanguageEn ? "Prohibit a player from sending a complaint against one player several times (true - yes/false - no)" : "Запретить игроку несколько раз отправлять жалобу на одного игрока (true - да/false - нет)")]
            public Boolean NoRepeatReport;
            [JsonProperty(LanguageEn ? "Cooldown before sending a complaint to the players (in seconds) (if you don't need a recharge, leave 0)" : "Перезарядка перед отправкой жалобы на игроков (в секундах) (если вам не нужна перезарядка - оставьте 0)")]
            public Int32 CooldownReport;
            [JsonProperty(LanguageEn ? "Use cooldown before sending a complaint only for a repeated complaint against one player (true) otherwise for all players (false)" : "Использовать перезарядку перед отправкой жалобы только на повторную жалобу на одного игрока (true) иначе на всех игроков (false)")]
            public Boolean CooldownRepeatOrAll;
        }

        internal class ReportF7AndGameMenu
        {
            [JsonProperty(LanguageEn ? "Use sending report via F7 and the RUST game menu (true - yes/false - no)" : "Использовать отправку жалоб через F7 и игровое меню RUST (true - да/false - нет)")]
            public Boolean UseFunction;
            [JsonProperty(LanguageEn ? "Index from the list of complaints when sent via F7 and the RUST game menu (From your list - starts from 0)" : "Индекс из списка жалоб при отправке через F7 и игровое меню RUST (Из вашего списка - начинается от 0)")]
            public Int32 DefaultIndexReason;
        }

        internal class NotifyChat
        {
            [JsonProperty(LanguageEn ? "Notify players when a moderator has started checking a player (configurable in the language file)" : "Уведомлять игроков о том, что модератор начал проверку игрока (настраивается в языковом файле)")]
            public Boolean UseNotifyCheck;
            [JsonProperty(LanguageEn ? "Notify players when a moderator has finished checking a player (configurable in the language file)" : "Уведомлять игроков о том, что модератор завершил проверку игрока (настраивается в языковом файле)")]
            public Boolean UseNotifyStopCheck;
            [JsonProperty(LanguageEn ? "Notify players that the moderator has completed the verification of the player and issued a verdict (banned) (configurable in the language file)" : "Уведомлять игроков о том, что модератор завершил проверку игрока и вынес вердикт (забанил) (настраивается в языковом файле)")]
            public Boolean UseNotifyVerdictCheck;
        }

        internal class NotifyVK
        {
            [JsonProperty(LanguageEn ? "Token from the VK group (you can find it in the community settings)" : "Токен от группы ВК (вы можете найти его в настройках сообщества)")]
            public String VKTokenGroup;
            [JsonProperty(LanguageEn ? "ID of the conversation the bot is invited to (countdown starts from 1 - every new conversation +1)" : "ID беседы в которую приглашен бот (отсчет начинается с 1 - каждую новую беседу +1)")]
            public String VKChatID;
        }

        internal class NotifyDiscord
        {
            [JsonProperty(LanguageEn ? "Set up WebHooks to send to Discord (if you don't need this feature - leave the field blank)" : "Настройка WebHooks для отправки в Discord (если вам не нужна эта функция - оставьте поле пустым)")]
            public Webhooks WebhooksList;
            internal class Webhooks
            {
                internal class TemplatesNotify
                {
                    [JsonProperty("Webhook")]
                    public String WebhookNotify;
                    [JsonProperty(LanguageEn ? "Discord message color (Can be found on the website - https://old.message.style/dashboard in the JSON section)" : "Цвет сообщения в Discord (Можно найти на сайте - https://old.message.style/dashboard в разделе JSON)")]
                    public Int32 Color;
                    [JsonProperty(LanguageEn ? "Title message" : "Заголовок сообщения")]
                    public String AuthorName;
                    [JsonProperty(LanguageEn ? "Link to the icon for the avatar of the message" : "Ссылка на иконку для аватарки сообщения")]
                    public String IconURL;
                }

                [JsonProperty(LanguageEn ? "WebHook : Setting up sending information about the start of a check" : "WebHook : Настройка отправки информации о начале проверки")]
                public TemplatesNotify NotifyStartCheck;
                [JsonProperty(LanguageEn ? "WebHook : Setting up sending information about the stop of a check" : "WebHook : Настройка отправки информации о завершении проверки")]
                public TemplatesNotify NotifyStopCheck;
                [JsonProperty(LanguageEn ? "WebHook : Settings for sending player contact information (when a player sends their Discord)" : "WebHook : Настройка отправки информации о контактах игрока (когда игрок отправляет свой Discord)")]
                public TemplatesNotify NotifyContacts;
                [JsonProperty(LanguageEn ? "WebHook : Setting up sending information about player complaints" : "WebHook : Настройка отправки информации о жалобах игроков")]
                public TemplatesNotify NotifySendReport;
                [JsonProperty(LanguageEn ? "WebHook : Setting up sending information when a player has exceeded the maximum number of complaints" : "WebHook : Настройка отправки информации когда игрок превысил максимальное количество жалоб")]
                public TemplatesNotify NotifyMaxReport;
                [JsonProperty(LanguageEn ? "WebHook : Setting up sending information about changing the status of the player and moderator" : "WebHook : Настройка отправки информации о изменении статуса игрока и модератора")]
                public TemplatesNotify NotifyStatusPlayerOrModerator;
            }

        }

        internal class ReasonReport
        {
            [JsonProperty(LanguageEn ? "Reason" : "Причина")]
            public LangText Title;
            [JsonProperty(LanguageEn ? "The command from your ban system to block the user ({0} - will be replaced by the player's ID)" : "Команда вашей бан-системы на блокировку пользователя ({0} - заменится на ID игрока)")]
            public String BanCommand;
            [JsonProperty(LanguageEn ? "Hide this reason from the player (true) (will only be seen by the moderator when passing a verdict)" : "Скрыть данную причину от игрока (true) (будет видеть только модератор при вынесении вердикта)")]
            public Boolean HideUser;
        }

        internal class Images
        {
            [JsonProperty(LanguageEn ? "Reports section images" : "Изображения раздела для жалоб")]
            public PlayerListBlock PlayerListBlockSettings;
            [JsonProperty(LanguageEn ? "Images of the statistics section" : "Изображения раздела статистики")]
            public StatisticsBlock StatisticsBlockSettings;
            [JsonProperty(LanguageEn ? "Left menu images" : "Изображения левого меню")]
            public LeftBlock LeftBlockSettings;
            [JsonProperty(LanguageEn ? "Moderation section images" : "Изображения раздела модерации")]
            public ModerationBlock ModerationBlockSettings;
            [JsonProperty(LanguageEn ? "Moderator menu images when checking" : "Изображения меню модератора при проверки")]
            public ModeratorMenuChecked ModeratorMenuCheckedSettings;
            [JsonProperty(LanguageEn ? "Images of the player rating menu quality of the moderator's work" : "Изображения меню оценки игроком качество работы модератора")]
            public PlayerMenuRaiting PlayerMenuRaitingSettings;
            [JsonProperty(LanguageEn ? "PNG of the menu background (1382x950)" : "PNG заднего фона меню (1382x950)")]
            public String Background;
            [JsonProperty(LanguageEn ? "PNG down arrows (page flipping) (10x5)" : "PNG стрелки вниз(перелистывание страниц) (10x5)")]
            public String PageDown;
            [JsonProperty(LanguageEn ? "PNG up arrows (page flipping) (10x5)" : "PNG стрелки вверх(перелистывание страниц) (10x5)")]
            public String PageUp;
            [JsonProperty(LanguageEn ? "PNG icon search (16x16)" : "PNG иконки поиска (16x16)")]
            public String Search;
            [JsonProperty(LanguageEn ? "PNG : Icon for adjusting avatar (64x64)" : "PNG : Иконка для корректировки автарки (64x64)")]
            public String AvatarBlur;
            [JsonProperty(LanguageEn ? "PNG : Icon for moderation verdict or rating (307x36)" : "PNG : Иконка для вердикта модерации или рейтинга (307x36)")]
            public String ReasonModeratorAndRaiting;
            [JsonProperty(LanguageEn ? "PNG : Image on the player's screen with text about the start of the checks (1450x559)" : "PNG : Изображение на экране игрока с текстом о начале проверки (1450x559)")]
            public String PlayerAlerts;
            internal class PlayerMenuRaiting
            {
                [JsonProperty(LanguageEn ? "PNG : Menu background when evaluating a reviewer (307x98)" : "PNG : Задний фон меню при оценке проверяющего (307x98)")]
                public String PlayerMenuRaitingBackground;
            }

            internal class ModeratorMenuChecked
            {
                [JsonProperty(LanguageEn ? "PNG : Menu background when checking player (307x148)" : "PNG : Задний фон меню при проверки игрока (307x148)")]
                public String ModeratorCheckedBackground;
                [JsonProperty(LanguageEn ? "PNG : End test button (128x40)" : "PNG : Кнопка завершения проверки (128x40)")]
                public String ModeratorCheckedStopButton;
                [JsonProperty(LanguageEn ? "PNG : Verdict button (128x40)" : "PNG : Кнопка вердикта (128x40)")]
                public String ModeratorVerdictButton;
                [JsonProperty(LanguageEn ? "PNG : `Licenses` icon (if Lumia support is enabled) (16x16)" : "PNG : Иконка `Лицензии` (если включена поддержка Luma) (16x16)")]
                public String SteamIcoPlayer;
                [JsonProperty(LanguageEn ? "PNG : `Pirate` icon (if Lumia support is enabled) (16x16)" : "PNG : Иконка `Пират` (если включена поддержка Luma) (16x16)")]
                public String PirateIcoPlayer;
            }

            internal class ModerationBlock
            {
                [JsonProperty(LanguageEn ? "PNG : Player information popup background (831x599)" : "PNG : Задний фон всплывающего окна с информацией о игроке (831x599)")]
                public String ModeratorPoopUPBackgorund;
                [JsonProperty(LanguageEn ? "PNG : Element background for text (155x22)" : "PNG : Задний фон элемента для текста (155x22)")]
                public String ModeratorPoopUPTextBackgorund;
                [JsonProperty(LanguageEn ? "PNG : Information panel background (196x240)" : "PNG : Задний фон панели с информацией (196x240)")]
                public String ModeratorPoopUPPanelBackgorund;
            }

            internal class LeftBlock
            {
                [JsonProperty(LanguageEn ? "PNG : Icon for sidebar button (192x55)" : "PNG : Иконка для кнопки в боковом меню (192x55)")]
                public String ButtonBackgorund;
                [JsonProperty(LanguageEn ? "PNG : Icon for the button in `reports` (32x32)" : "PNG : Иконка для кнопки в `жалобы`(32x32)")]
                public String ReportIcon;
                [JsonProperty(LanguageEn ? "PNG : Icon for the button in `moderation` (32x32)" : "PNG : Иконка для кнопки в `модерация`(32x32)")]
                public String ModerationIcon;
            }

            internal class PlayerListBlock
            {
                [JsonProperty(LanguageEn ? "PNG : Cause selection popup background (567x599)" : "PNG : Задний фон всплывающего сообщения с выбором причины (567x599)")]
                public String PoopUpBackgorund;
                [JsonProperty(LanguageEn ? "PNG : Reason background in popup (463x87)" : "PNG : Задний фон причины в всплывающем окне (463x87)")]
                public String PoopUpReasonBackgorund;
            }

            internal class StatisticsBlock
            {
                [JsonProperty(LanguageEn ? "PNG Background of the statistics block (283x81)" : "PNG Задний фон блока статистики (283x81)")]
                public String BlockStatsModeration;
                [JsonProperty(LanguageEn ? "PNG Background of the rating block in statistics (65x28)" : "PNG Задний фон блока рейтинга в статистике (65x28)")]
                public String BlockStatsRaitingModeration;
                [JsonProperty(LanguageEn ? "PNG : Rating icon in statistics (16x15)" : "PNG : Иконка рейтинга в статистике (16x15)")]
                public String RaitingImage;
            }

        }

        internal class Colors
        {
            [JsonProperty(LanguageEn ? "RGBA of the main text color" : "RGBA основного цвета текста")]
            public String MainColorText;
            [JsonProperty(LanguageEn ? "RGBA of additional text color" : "RGBA дополнительного цвета текста")]
            public String AdditionalColorText;
            [JsonProperty(LanguageEn ? "RGBA additional color of elements (Buttons, dies)" : "RGBA дополнительный цвет элементов (Кнопки, плашки)")]
            public String AdditionalColorElements;
            [JsonProperty(LanguageEn ? "RGBA additional color of elements (Buttons, dies) #2" : "RGBA дополнительный цвет элементов (Кнопки, плашки) #2")]
            public String AdditionalColorElementsTwo;
            [JsonProperty(LanguageEn ? "RGBA additional color of elements (Buttons, dies) #3" : "RGBA дополнительный цвет элементов (Кнопки, плашки) #3")]
            public String AdditionalColorElementsThree;
            [JsonProperty(LanguageEn ? "RGBA the main color" : "RGBA основной цвет")]
            public String MainColor;
        }

        internal class References
        {
            [JsonProperty(LanguageEn ? "IQFakeActive : Use collaboration (true - yes/false - no)" : "IQFakeActive : Использовать совместную работу (true - да/false - нет)")]
            public Boolean IQFakeActiveUse;
            [JsonProperty(LanguageEn ? "Setting up IQChat" : "Настройка IQChat")]
            public IQChatReference IQChatSetting;
            [JsonProperty(LanguageEn ? "Setting up NoEscape" : "Настройка NoEscape")]
            public NoEscapeReference NoEscapeSetting;
            [JsonProperty(LanguageEn ? "Duels : Reschedule the player's check if he is in a duel (true - yes/false - no)" : "Duels : Перенести проверку игрока если он на дуэли (true - да/false - нет)")]
            public Boolean NoCheckedDuel;
            [JsonProperty(LanguageEn ? "Setting up Friends" : "Настройка Friends")]
            public FriendsReference FriendsSetting;
            [JsonProperty(LanguageEn ? "Setting up Clans" : "Настройка Clans")]
            public ClansReference ClansSetting;
            [JsonProperty(LanguageEn ? "Setting up MultiFighting (Luma)" : "Настройка MultiFighting (Luma)")]
            public MultiFighting MultiFightingSetting;
            [JsonProperty(LanguageEn ? "Setting up StopDamageMan" : "Настройка StopDamageMan")]
            public StopDamageMan StopDamageManSetting;
            [JsonProperty(LanguageEn ? "Setting up RCC support" : "Настройка поддержки RCC")]
            public RustCheatCheck RCCSettings;
            [JsonProperty(LanguageEn ? "Setting up OzProtect support" : "Настройка поддержки OzProtect")]
            public OzProtectCheck OzProtectSettings;
            internal class MultiFighting
            {
                [JsonProperty(LanguageEn ? "MultiFighting (Luma) : Display an icon with the player status - `Steam` / `Pirate`" : "MultiFighting (Luma) : Отображать иконку со статусом игрока - `Steam` / `Пират`")]
                public Boolean UseSteamCheck;
            }

            internal class StopDamageMan
            {
                [JsonProperty(LanguageEn ? "StopDamageMan : Disable player damage during check" : "StopDamageMan : Отключать игроку урон во время проверки")]
                public Boolean UseStopDamage;
            }

            internal class RustCheatCheck
            {
                [JsonProperty(LanguageEn ? "RCC key (if you don't need RCC support, leave the key blank)" : "Ключ RCC (если вам не нужна поддержка RCC - оставьте ключ пустым)")]
                public String RCCKey;
            }

            internal class OzProtectCheck
            {
                [JsonProperty(LanguageEn ? "OzProtect key (if you don't need OzProtect support, leave the key blank)" : "Ключ OzProtect (если вам не нужна поддержка OzProtect - оставьте ключ пустым)")]
                public String OzProtectKey;
            }

            internal class IQChatReference
            {
                [JsonProperty(LanguageEn ? "IQChat : Custom prefix in chat" : "IQChat : Кастомный префикс в чате")]
                public String CustomPrefix;
                [JsonProperty(LanguageEn ? "IQChat : Custom chat avatar (If required)" : "IQChat : Кастомный аватар в чате(Если требуется)")]
                public String CustomAvatar;
                [JsonProperty(LanguageEn ? "IQChat : Use UI notification (true - yes/false - no)" : "IQChat : Использовать UI уведомление (true - да/false - нет)")]
                public Boolean UIAlertUse;
            }

            internal class NoEscapeReference
            {
                [JsonProperty(LanguageEn ? "NoEscape : Reschedule a player's check if he has a `Raid-Block` (true - yes/false - no)" : "NoEscape : Перенести проверку игрока если у него есть `Raid-Блок` (true - да/false - нет)")]
                public Boolean NoCheckedRaidBlock;
                [JsonProperty(LanguageEn ? "NoEscape : Reschedule a player's check if he has a `Combat-Block` (true - yes/false - no)" : "NoEscape : Перенести проверку игрока если у него есть `Комбат-Блок` (true - да/false - нет)")]
                public Boolean NoCheckedCombatBlock;
            }

            internal class FriendsReference
            {
                [JsonProperty(LanguageEn ? "Friends : Prohibit players in the team from sending reports to each other (true - yes/false - no)" : "Friends : Запретить игрокам в команде отправлять репорты друг на друга (true - да/false - нет)")]
                public Boolean SendReportFriend;
                [JsonProperty(LanguageEn ? "Friends : Prohibit the moderator from checking his teammate (true - yes/false - no)" : "Friends : Запретить модератору проверять своего тиммейта (true - да/false - нет)")]
                public Boolean StartCheckedFriend;
            }

            internal class ClansReference
            {
                [JsonProperty(LanguageEn ? "Clans : Prohibit players in the same clan from sending reports to each other (true - yes/false - no)" : "Clans : Запретить игрокам в одном клане отправлять репорты друг на друга (true - да/false - нет)")]
                public Boolean SendReportClan;
                [JsonProperty(LanguageEn ? "Clans : Prohibit the moderator from checking the members of his clan (true - yes/false - no)" : "Clans : Запретить модератору проверять участников своего клана (true - да/false - нет)")]
                public Boolean StartCheckedClan;
            }

        }

        internal class LangText
        {
            [JsonProperty(LanguageEn ? "Reason title russian" : "Причина на русском")]
            public String LanguageRU;
            [JsonProperty(LanguageEn ? "Reason title english" : "Причина на английском")]
            public String LanguageEN;
            public String GetReasonTitle(UInt64 TargetID);
        }

        public static Configuration GetNewConfiguration();
    }

    private void DrawUI_Moderator_Checked_Menu_Status(BasePlayer moderator, String Status);
    private void SteamAvatarAdd(String userid);
    private void PreLoadedPlugin();
    public string GetLang(string LangKey, string userID, object[] args);
     void OnItemCraftCancelled(ItemCraftTask task, ItemCrafter crafter);
    private Boolean IsModerator(BasePlayer moderator);
    [ChatCommand("report")]
    private void ChatCommandReport(BasePlayer player, String cmd, String[] args);
     void OnPlayerCommand(BasePlayer player, string command, string[] args);
    private List<Fields> DT_PlayerSendReport(BasePlayer Sender, UInt64 TargetID, String Reason);
    private Dictionary<UInt64, PlayerInformation> PlayerInformations;
    private void RunEffect(BasePlayer Moderator);
    private void DrawUI_Raiting_Menu_Player(BasePlayer player, UInt64 ModeratorID);
    private const String ModeratorPermissions;
     void SyncReservedFinish(String JSON);
    private NpcSound LoadDataSound(String name);
    public Boolean IsDuel(UInt64 userID);
    private Boolean IsFriends(UInt64 userID, UInt64 targetID);
     object OnItemCraft(ItemCraftTask task, BasePlayer player, Item item);
    private class PlayerInformation
    {
        public Int32 Reports;
        public Int32 SendReports;
        public Int32 AmountChecked;
        public String LastModerator;
        public List<Configuration.LangText> ReasonHistory;
    }

    private void ConsoleCommandDiscord(ConsoleSystem.Arg arg);
    private const Boolean LanguageEn;
    private void StopDamageRemove(UInt64 playerID);
    private void StartCheckRCC(UInt64 TargetID, UInt64 ModerID);
    private void DrawUI_Report_Panel(BasePlayer player);
    private IEnumerator SendSounds(BasePlayer player, String clipName, SpeakerEntityMgr.SpeakerEntity speakerEntity);
    private static StringBuilder sb;
    private List<Fields> DT_ChangeStatus(Boolean IsModerator, String PlayerName, String UserID, String StatusConnection);
    private void AlertModerator(BasePlayer Moderator, String PlayerName, Int32 ReportCount);
    public void StartSysncFakeActive();
    private readonly Regex _avatarRegex;
    public class FakePlayer
    {
        public UInt64 UserID;
        public String DisplayName;
    }

    public class SpeakerEntityMgr
    {
        private static List<SpeakerEntity> _Entities;
        public static SpeakerEntity Create(BasePlayer listeners);
        public static void Shutdown();
        public static void Kill(SpeakerEntity entity);
        public class SpeakerEntity
        {
            public UInt64 UID_SPEAKER;
            private UInt64 UID_CHAIR;
            public BasePlayer Listeners { get; set; }
            public void SetListeners(BasePlayer listeners);
            public void SendEntitities();
            private ProtoBuf.Entity GetEntitySpeaker(BasePlayer player);
            private ProtoBuf.Entity GetEntityChair(BasePlayer player);
            public void Kill();
            private void SendEntity(Func<BasePlayer, ProtoBuf.Entity> entityGetter);
            private void DestroyEntity(UInt64 uid);
        }

    }

    private readonly Hash<String, NpcSound> CachedSound;
    private List<Fields> DT_StartCheck(BasePlayer Target, BasePlayer Moderator, Boolean IsConsole);
    private void OnPlayerReported(BasePlayer reporter, String targetName, String targetId, String subject, String message, String type);
    private void DrawUI_Moderator_Button(BasePlayer moderator, String Command);
    private void DrawUI_ShowPoopUP_Reason(BasePlayer player, String userID);
    private String VKT_PlayerSendContact(BasePlayer Sender, String Contact);
    private void SendVerdictPlayer(UInt64 TargetID, BasePlayer Moderator, Configuration.ReasonReport Verdict);
    private Boolean IsSteam(String id);
    private void AlertMaxReportDiscord(String PlayerName, Int32 ReportCount, UInt64 TargetID);
    private Dictionary<UInt64, ModeratorInformation> ModeratorInformations;
    public List<FakePlayer> PlayerBases;
}

private class ImageUi
{
    private static Coroutine coroutineImg;
    private static Dictionary<String, String> Images;
    private static List<String> KeyImages;
    public static void DownloadImages();
    private static IEnumerator AddImage();
    public static string GetImage(String ImgKey);
    public static void Initialize();
    public static void Unload();
}

public class OzResponse
{
    public int unixtime { get; set; }
    public string reason { get; set; }
    public string proofid { get; set; }
    public bool pirate { get; set; }
    public bool active { get; set; }
    public bool reliable { get; set; }
    public bool unnecessary { get; set; }
    public OzServer server { get; set; }
    public string admin { get; set; }
    public int game { get; set; }
    public string date { get; set; }
    public int bantime { get; set; }
}

private class InterfaceBuilder
{
    public static InterfaceBuilder Instance;
    public const String UI_LAYER;
    public const String UI_REPORT_PANEL;
    public const String UI_REPORT_LEFT_PANEL;
    public const String UI_REPORT_PLAYER_PANEL;
    public const String UI_REPORT_POOPUP_PLAYER;
    public const String UI_REPORT_POOPUP_MODERATOR;
    public const String UI_REPORT_MODERATOR_STATISTICS;
    public const String UI_REPORT_MODERATOR_MENU_CHECKED;
    public const String UI_REPORT_RAITING_PLAYER_PANEL;
    public const String UI_REPORT_PLAYER_ALERT;
    public Dictionary<String, String> Interfaces;
    public InterfaceBuilder();
    public static void AddInterface(String name, String json);
    public static String GetInterface(String name);
    public static void DestroyAll();
    private void Building_Bacgkround();
    private void Building_HeaderPanel_Search();
    private void Building_Profile_Moderator_Stats();
    private void Building_Profile_Template_Banner();
    private void Building_Profile_Template_Banner_AdditionalText();
    private void Building_Profile_Template_Banner_AdditionalImg();
    private void Building_Panel_Players();
    private void Building_PageController();
    private void Building_PlayerTemplate();
    private void Building_PoopUp_Player();
    private void Building_PoopUp_Reason();
    private void Building_PlayerTemplate_Moderator();
    private void Building_PlayerTemplate_Moderator_IsSteam();
    private void Building_Left_Menu();
    private void Building_Button_Template();
    private void Building_PoopUP_Moderator();
    private void Building_PoopUP_Moderator_InfoBlock();
    private void Building_Text_Template_Moderator_Block_Info();
    private void Building_Moderator_Menu();
    private void Building_ModeratorMenu_Verdict_Button();
    private void Building_ModeratorMenuChecked_InfoOnline();
    private void Building_ModeratorMenuChecked_InfoDiscord();
    private void Building_DropList_Reasons();
    private void Building_Raiting_Menu();
    private void Building_Raiting_Select_Button();
    private void Building_Player_Alert();
}

internal class LocalRepositoryRCC
{
    public List<String> LastChecksServers;
    public List<String> LastBansServers;
}

internal class LocalRepositoryOzProtect
{
    public List<String> LastBansServers;
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

public class Authors
{
    public String name { get; set; }
    public String url { get; set; }
    public String icon_url { get; set; }
    public String proxy_icon_url { get; set; }
    public Authors(String name, String url, String icon_url, String proxy_icon_url);
}

public class OzResult
{
    public string status { get; set; }
    public List<OzResponse> response { get; set; }
}

private class PlayerRepository
{
    public Int32 GetCooldownLeft(UInt64 TargetID);
    private Boolean IsReportedPlayer(UInt64 TargetID);
    public Boolean IsCooldown(UInt64 TargetID);
    public Double Cooldown;
    public void AddCooldown(UInt64 TargetID);
    public Boolean IsRepeatReported(UInt64 TargetID);
    public Dictionary<UInt64, Double> ReportedList;
}

private class ProcessCheckRepository
{
    public String DiscordTarget;
    public String DisplayName;
    public UInt64 ModeratorID;
}

public class OzServer
{
    public string name { get; set; }
    public string ico { get; set; }
    public string overlay { get; set; }
    public string desc { get; set; }
    public string site { get; set; }
    public bool pirate { get; set; }
    public int game { get; set; }
    public string ip { get; set; }
}

private class ModeratorInformation
{
    public Int32 AmountChecked;
    public Int32 AmountBans;
    public Int32 AverageRaiting;
    public List<Int32> OneScore;
    public List<Int32> TwoScore;
    public List<Int32> ThreeScore;
    public Int32 GetAverageRaiting();
    public Int32 GetAverageRaitingAchive(List<Int32> Score);
}

private class Response
{
    public List<String> last_ip;
    public String last_nick;
    public List<UInt64> another_accs;
    public List<last_checks> last_check;
    public class last_checks
    {
        public UInt64 moderSteamID;
        public String serverName;
        public Int32 time;
    }

    public List<RustCCBans> bans;
    public class RustCCBans
    {
        public Int32 banID;
        public String reason;
        public String serverName;
        public Int32 OVHserverID;
        public Int32 banDate;
    }

}

public class last_checks
{
    public UInt64 moderSteamID;
    public String serverName;
    public Int32 time;
}

public class RustCCBans
{
    public Int32 banID;
    public String reason;
    public String serverName;
    public Int32 OVHserverID;
    public Int32 banDate;
}

public class Fields
{
    public String name { get; set; }
    public String value { get; set; }
    public bool inline { get; set; }
    public Fields(String name, String value, bool inline);
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

private class Configuration
{
    [JsonProperty(LanguageEn ? "Setting up compatibility with IQReportSystemr" : "Настройка совместимостей с IQReportSystem")]
    public References ReferenceSettings;
    [JsonProperty(LanguageEn ? "Color setting" : "Настройка цветов")]
    public Colors ColorsSettings;
    [JsonProperty(LanguageEn ? "Setting images" : "Настройка изображений")]
    public Images ImagesSettings;
    [JsonProperty(LanguageEn ? "List of reports and reasons for blocking" : "Список жалоб и причин блокировки")]
    public List<ReasonReport> ReasonList;
    [JsonProperty(LanguageEn ? "Setting up sending complaints via F7 or the RUST game menu" : "Настройка отправки жалоб через F7 или игровое меню RUST")]
    public ReportF7AndGameMenu ReportF7AndGameMenuSettings;
    [JsonProperty(LanguageEn ? "Setting up the Player check process" : "Настройка процесса проверки игрока")]
    public CheckController CheckControllerSettings;
    [JsonProperty(LanguageEn ? "Setting up moderator notifications and the maximum number of reports" : "Настройка уведомления модераторов и максимального количества репортов")]
    public ReportContollerModeration ReportContollerModerationSettings;
    [JsonProperty(LanguageEn ? "Setting up sending complaints by players" : "Настройка отправки жалоб игроками")]
    public ReportSendController ReportSendControllerSettings;
    [JsonProperty(LanguageEn ? "Additional verdict setting" : "Дополнительная настройка вынесения вердикта")]
    public VerdictController VerdictControllerSettings;
    [JsonProperty(LanguageEn ? "Configuring notifications for all players about actions in the plugin" : "Настройка уведомлений для всех игроков о действиях в плагине")]
    public NotifyChat NotifyChatSettings;
    [JsonProperty(LanguageEn ? "Setting up notifications in Discord" : "Настройка уведомлений в Discord")]
    public NotifyDiscord NotifyDiscordSettings;
    [JsonProperty(LanguageEn ? "Setting up notifications in VK" : "Настройка уведомлений в VK")]
    public NotifyVK NotifyVKSettings;
    [JsonProperty(LanguageEn ? "Command to send data when calling for verification (console and chat)" : "Команда для отправки данных при вызове на проверку (консольная и чатовая)")]
    public String CommandForContact;
    internal class VerdictController
    {
        [JsonProperty(LanguageEn ? "Banned all the `Friends` of a player who has been given a verdict by a moderator (true - yes/false - no)" : "Блокировать всех `Друзей` игрока, которому вынес вердикт модератор (true - да/false - нет)")]
        public Boolean UseBanAllTeam;
        [JsonProperty(LanguageEn ? "Index from the list of complaints when blocking a player's `Friends` (From your list - starts from 0)" : "Индекс из списка жалоб при блокировки `Друзей` игрока  (Из вашего списка - начинается от 0)")]
        public Int32 IndexBanReason;
    }

    internal class CheckController
    {
        [JsonProperty(LanguageEn ? "Use sound notification for players when calling for verification (true - yes / false - no) [You must upload sound files to the IQSystem/IQReportSystem/Sounds folder]" : "Использовать звуковое оповещение для игроков при вызове на проверку (true - да/false - нет) [Вы должны загрузить файлы со звуком в папку IQSystem/IQReportSystem/Sounds]")]
        public Boolean UseSoundAlert;
        [JsonProperty(LanguageEn ? "Record a demos of the player during his check" : "Записывать демо игрока во время его проверки")]
        public Boolean UseDemo;
        [JsonProperty(LanguageEn ? "Use AFK validation before calling for check (true - yes / false - no)" : "Использовать проверку на AFK перед вызовом на проверку (true - да/false - нет)")]
        public Boolean UseCheckAFK;
        [JsonProperty(LanguageEn ? "Cancel check for a player automatically with saving reports if he left the server for 15 minutes or more (true - yes/false - no)" : "Отменять проверку игроку автоматически с сохранением репортов если он покинул сервер на 15 минут и более (true - да/false - нет)")]
        public Boolean StopCheckLeavePlayer;
        [JsonProperty(LanguageEn ? "Use tracking of crafting of invaders by the player (will notify the moderator about it) (true - yes / false - no)" : "Использовать отслеживание крафта предметов игроком (будет уведомлять модератора об этом) (true - да/false - нет)")]
        public Boolean TrackCrafting;
        [JsonProperty(LanguageEn ? "Use tracking of messages sent to the chat by the player (will notify the moderator about it) (true - yes / false - no)" : "Использовать отслеживание отправки сообщений в чат игроком (будет уведомлять модератора об этом) (true - да/false - нет)")]
        public Boolean TrackChat;
        [JsonProperty(LanguageEn ? "Use player command usage tracking (will notify the moderator about it) (true - yes / false - no)" : "Использовать отслеживание использования команд игроком (будет уведомлять модератора об этом) (true - да/false - нет)")]
        public Boolean TrackCommand;
    }

    internal class ReportContollerModeration
    {
        [JsonProperty(LanguageEn ? "Maximum number of reports to display the player in the moderator menu and moderator notifications" : "Максимальное количество репортов для отображения игрока в меню модератора и уведомления модератора")]
        public Int32 ReportCountTrigger;
        [JsonProperty(LanguageEn ? "Setting up moderator notifications about the maximum number of player reports" : "Настройка уведомления модератора о максимальном количестве репортов игрока")]
        public AlertModeration AlertModerationSettings;
        internal class AlertModeration
        {
            [JsonProperty(LanguageEn ? "Notify the moderator that the player has scored the maximum number of reports" : "Уведомлять модератора о том, что игрок набрал максимальное количество репортов")]
            public Boolean AlertModerator;
            [JsonProperty(LanguageEn ? "Enable an audio notification to the moderator during the notification of the number of reports" : "Включать звуковое уведомление модератору во время уведомления о количестве репортов")]
            public Boolean AlertSound;
            [JsonProperty(LanguageEn ? "The path to the notification sound (this is the path of the prefab of the game - you can see here : https://github.com/OrangeWulf/Rust-Docs/blob/master/Extended/Effects.md)" : "Путь до звука уведомления (это путь префаба игры - посмотреть можно тут : https://github.com/OrangeWulf/Rust-Docs/blob/master/Extended/Effects.md)")]
            public String PathSound;
        }

    }

    internal class ReportSendController
    {
        [JsonProperty(LanguageEn ? "Prohibit a player from sending a complaint against one player several times (true - yes/false - no)" : "Запретить игроку несколько раз отправлять жалобу на одного игрока (true - да/false - нет)")]
        public Boolean NoRepeatReport;
        [JsonProperty(LanguageEn ? "Cooldown before sending a complaint to the players (in seconds) (if you don't need a recharge, leave 0)" : "Перезарядка перед отправкой жалобы на игроков (в секундах) (если вам не нужна перезарядка - оставьте 0)")]
        public Int32 CooldownReport;
        [JsonProperty(LanguageEn ? "Use cooldown before sending a complaint only for a repeated complaint against one player (true) otherwise for all players (false)" : "Использовать перезарядку перед отправкой жалобы только на повторную жалобу на одного игрока (true) иначе на всех игроков (false)")]
        public Boolean CooldownRepeatOrAll;
    }

    internal class ReportF7AndGameMenu
    {
        [JsonProperty(LanguageEn ? "Use sending report via F7 and the RUST game menu (true - yes/false - no)" : "Использовать отправку жалоб через F7 и игровое меню RUST (true - да/false - нет)")]
        public Boolean UseFunction;
        [JsonProperty(LanguageEn ? "Index from the list of complaints when sent via F7 and the RUST game menu (From your list - starts from 0)" : "Индекс из списка жалоб при отправке через F7 и игровое меню RUST (Из вашего списка - начинается от 0)")]
        public Int32 DefaultIndexReason;
    }

    internal class NotifyChat
    {
        [JsonProperty(LanguageEn ? "Notify players when a moderator has started checking a player (configurable in the language file)" : "Уведомлять игроков о том, что модератор начал проверку игрока (настраивается в языковом файле)")]
        public Boolean UseNotifyCheck;
        [JsonProperty(LanguageEn ? "Notify players when a moderator has finished checking a player (configurable in the language file)" : "Уведомлять игроков о том, что модератор завершил проверку игрока (настраивается в языковом файле)")]
        public Boolean UseNotifyStopCheck;
        [JsonProperty(LanguageEn ? "Notify players that the moderator has completed the verification of the player and issued a verdict (banned) (configurable in the language file)" : "Уведомлять игроков о том, что модератор завершил проверку игрока и вынес вердикт (забанил) (настраивается в языковом файле)")]
        public Boolean UseNotifyVerdictCheck;
    }

    internal class NotifyVK
    {
        [JsonProperty(LanguageEn ? "Token from the VK group (you can find it in the community settings)" : "Токен от группы ВК (вы можете найти его в настройках сообщества)")]
        public String VKTokenGroup;
        [JsonProperty(LanguageEn ? "ID of the conversation the bot is invited to (countdown starts from 1 - every new conversation +1)" : "ID беседы в которую приглашен бот (отсчет начинается с 1 - каждую новую беседу +1)")]
        public String VKChatID;
    }

    internal class NotifyDiscord
    {
        [JsonProperty(LanguageEn ? "Set up WebHooks to send to Discord (if you don't need this feature - leave the field blank)" : "Настройка WebHooks для отправки в Discord (если вам не нужна эта функция - оставьте поле пустым)")]
        public Webhooks WebhooksList;
        internal class Webhooks
        {
            internal class TemplatesNotify
            {
                [JsonProperty("Webhook")]
                public String WebhookNotify;
                [JsonProperty(LanguageEn ? "Discord message color (Can be found on the website - https://old.message.style/dashboard in the JSON section)" : "Цвет сообщения в Discord (Можно найти на сайте - https://old.message.style/dashboard в разделе JSON)")]
                public Int32 Color;
                [JsonProperty(LanguageEn ? "Title message" : "Заголовок сообщения")]
                public String AuthorName;
                [JsonProperty(LanguageEn ? "Link to the icon for the avatar of the message" : "Ссылка на иконку для аватарки сообщения")]
                public String IconURL;
            }

            [JsonProperty(LanguageEn ? "WebHook : Setting up sending information about the start of a check" : "WebHook : Настройка отправки информации о начале проверки")]
            public TemplatesNotify NotifyStartCheck;
            [JsonProperty(LanguageEn ? "WebHook : Setting up sending information about the stop of a check" : "WebHook : Настройка отправки информации о завершении проверки")]
            public TemplatesNotify NotifyStopCheck;
            [JsonProperty(LanguageEn ? "WebHook : Settings for sending player contact information (when a player sends their Discord)" : "WebHook : Настройка отправки информации о контактах игрока (когда игрок отправляет свой Discord)")]
            public TemplatesNotify NotifyContacts;
            [JsonProperty(LanguageEn ? "WebHook : Setting up sending information about player complaints" : "WebHook : Настройка отправки информации о жалобах игроков")]
            public TemplatesNotify NotifySendReport;
            [JsonProperty(LanguageEn ? "WebHook : Setting up sending information when a player has exceeded the maximum number of complaints" : "WebHook : Настройка отправки информации когда игрок превысил максимальное количество жалоб")]
            public TemplatesNotify NotifyMaxReport;
            [JsonProperty(LanguageEn ? "WebHook : Setting up sending information about changing the status of the player and moderator" : "WebHook : Настройка отправки информации о изменении статуса игрока и модератора")]
            public TemplatesNotify NotifyStatusPlayerOrModerator;
        }

    }

    internal class ReasonReport
    {
        [JsonProperty(LanguageEn ? "Reason" : "Причина")]
        public LangText Title;
        [JsonProperty(LanguageEn ? "The command from your ban system to block the user ({0} - will be replaced by the player's ID)" : "Команда вашей бан-системы на блокировку пользователя ({0} - заменится на ID игрока)")]
        public String BanCommand;
        [JsonProperty(LanguageEn ? "Hide this reason from the player (true) (will only be seen by the moderator when passing a verdict)" : "Скрыть данную причину от игрока (true) (будет видеть только модератор при вынесении вердикта)")]
        public Boolean HideUser;
    }

    internal class Images
    {
        [JsonProperty(LanguageEn ? "Reports section images" : "Изображения раздела для жалоб")]
        public PlayerListBlock PlayerListBlockSettings;
        [JsonProperty(LanguageEn ? "Images of the statistics section" : "Изображения раздела статистики")]
        public StatisticsBlock StatisticsBlockSettings;
        [JsonProperty(LanguageEn ? "Left menu images" : "Изображения левого меню")]
        public LeftBlock LeftBlockSettings;
        [JsonProperty(LanguageEn ? "Moderation section images" : "Изображения раздела модерации")]
        public ModerationBlock ModerationBlockSettings;
        [JsonProperty(LanguageEn ? "Moderator menu images when checking" : "Изображения меню модератора при проверки")]
        public ModeratorMenuChecked ModeratorMenuCheckedSettings;
        [JsonProperty(LanguageEn ? "Images of the player rating menu quality of the moderator's work" : "Изображения меню оценки игроком качество работы модератора")]
        public PlayerMenuRaiting PlayerMenuRaitingSettings;
        [JsonProperty(LanguageEn ? "PNG of the menu background (1382x950)" : "PNG заднего фона меню (1382x950)")]
        public String Background;
        [JsonProperty(LanguageEn ? "PNG down arrows (page flipping) (10x5)" : "PNG стрелки вниз(перелистывание страниц) (10x5)")]
        public String PageDown;
        [JsonProperty(LanguageEn ? "PNG up arrows (page flipping) (10x5)" : "PNG стрелки вверх(перелистывание страниц) (10x5)")]
        public String PageUp;
        [JsonProperty(LanguageEn ? "PNG icon search (16x16)" : "PNG иконки поиска (16x16)")]
        public String Search;
        [JsonProperty(LanguageEn ? "PNG : Icon for adjusting avatar (64x64)" : "PNG : Иконка для корректировки автарки (64x64)")]
        public String AvatarBlur;
        [JsonProperty(LanguageEn ? "PNG : Icon for moderation verdict or rating (307x36)" : "PNG : Иконка для вердикта модерации или рейтинга (307x36)")]
        public String ReasonModeratorAndRaiting;
        [JsonProperty(LanguageEn ? "PNG : Image on the player's screen with text about the start of the checks (1450x559)" : "PNG : Изображение на экране игрока с текстом о начале проверки (1450x559)")]
        public String PlayerAlerts;
        internal class PlayerMenuRaiting
        {
            [JsonProperty(LanguageEn ? "PNG : Menu background when evaluating a reviewer (307x98)" : "PNG : Задний фон меню при оценке проверяющего (307x98)")]
            public String PlayerMenuRaitingBackground;
        }

        internal class ModeratorMenuChecked
        {
            [JsonProperty(LanguageEn ? "PNG : Menu background when checking player (307x148)" : "PNG : Задний фон меню при проверки игрока (307x148)")]
            public String ModeratorCheckedBackground;
            [JsonProperty(LanguageEn ? "PNG : End test button (128x40)" : "PNG : Кнопка завершения проверки (128x40)")]
            public String ModeratorCheckedStopButton;
            [JsonProperty(LanguageEn ? "PNG : Verdict button (128x40)" : "PNG : Кнопка вердикта (128x40)")]
            public String ModeratorVerdictButton;
            [JsonProperty(LanguageEn ? "PNG : `Licenses` icon (if Lumia support is enabled) (16x16)" : "PNG : Иконка `Лицензии` (если включена поддержка Luma) (16x16)")]
            public String SteamIcoPlayer;
            [JsonProperty(LanguageEn ? "PNG : `Pirate` icon (if Lumia support is enabled) (16x16)" : "PNG : Иконка `Пират` (если включена поддержка Luma) (16x16)")]
            public String PirateIcoPlayer;
        }

        internal class ModerationBlock
        {
            [JsonProperty(LanguageEn ? "PNG : Player information popup background (831x599)" : "PNG : Задний фон всплывающего окна с информацией о игроке (831x599)")]
            public String ModeratorPoopUPBackgorund;
            [JsonProperty(LanguageEn ? "PNG : Element background for text (155x22)" : "PNG : Задний фон элемента для текста (155x22)")]
            public String ModeratorPoopUPTextBackgorund;
            [JsonProperty(LanguageEn ? "PNG : Information panel background (196x240)" : "PNG : Задний фон панели с информацией (196x240)")]
            public String ModeratorPoopUPPanelBackgorund;
        }

        internal class LeftBlock
        {
            [JsonProperty(LanguageEn ? "PNG : Icon for sidebar button (192x55)" : "PNG : Иконка для кнопки в боковом меню (192x55)")]
            public String ButtonBackgorund;
            [JsonProperty(LanguageEn ? "PNG : Icon for the button in `reports` (32x32)" : "PNG : Иконка для кнопки в `жалобы`(32x32)")]
            public String ReportIcon;
            [JsonProperty(LanguageEn ? "PNG : Icon for the button in `moderation` (32x32)" : "PNG : Иконка для кнопки в `модерация`(32x32)")]
            public String ModerationIcon;
        }

        internal class PlayerListBlock
        {
            [JsonProperty(LanguageEn ? "PNG : Cause selection popup background (567x599)" : "PNG : Задний фон всплывающего сообщения с выбором причины (567x599)")]
            public String PoopUpBackgorund;
            [JsonProperty(LanguageEn ? "PNG : Reason background in popup (463x87)" : "PNG : Задний фон причины в всплывающем окне (463x87)")]
            public String PoopUpReasonBackgorund;
        }

        internal class StatisticsBlock
        {
            [JsonProperty(LanguageEn ? "PNG Background of the statistics block (283x81)" : "PNG Задний фон блока статистики (283x81)")]
            public String BlockStatsModeration;
            [JsonProperty(LanguageEn ? "PNG Background of the rating block in statistics (65x28)" : "PNG Задний фон блока рейтинга в статистике (65x28)")]
            public String BlockStatsRaitingModeration;
            [JsonProperty(LanguageEn ? "PNG : Rating icon in statistics (16x15)" : "PNG : Иконка рейтинга в статистике (16x15)")]
            public String RaitingImage;
        }

    }

    internal class Colors
    {
        [JsonProperty(LanguageEn ? "RGBA of the main text color" : "RGBA основного цвета текста")]
        public String MainColorText;
        [JsonProperty(LanguageEn ? "RGBA of additional text color" : "RGBA дополнительного цвета текста")]
        public String AdditionalColorText;
        [JsonProperty(LanguageEn ? "RGBA additional color of elements (Buttons, dies)" : "RGBA дополнительный цвет элементов (Кнопки, плашки)")]
        public String AdditionalColorElements;
        [JsonProperty(LanguageEn ? "RGBA additional color of elements (Buttons, dies) #2" : "RGBA дополнительный цвет элементов (Кнопки, плашки) #2")]
        public String AdditionalColorElementsTwo;
        [JsonProperty(LanguageEn ? "RGBA additional color of elements (Buttons, dies) #3" : "RGBA дополнительный цвет элементов (Кнопки, плашки) #3")]
        public String AdditionalColorElementsThree;
        [JsonProperty(LanguageEn ? "RGBA the main color" : "RGBA основной цвет")]
        public String MainColor;
    }

    internal class References
    {
        [JsonProperty(LanguageEn ? "IQFakeActive : Use collaboration (true - yes/false - no)" : "IQFakeActive : Использовать совместную работу (true - да/false - нет)")]
        public Boolean IQFakeActiveUse;
        [JsonProperty(LanguageEn ? "Setting up IQChat" : "Настройка IQChat")]
        public IQChatReference IQChatSetting;
        [JsonProperty(LanguageEn ? "Setting up NoEscape" : "Настройка NoEscape")]
        public NoEscapeReference NoEscapeSetting;
        [JsonProperty(LanguageEn ? "Duels : Reschedule the player's check if he is in a duel (true - yes/false - no)" : "Duels : Перенести проверку игрока если он на дуэли (true - да/false - нет)")]
        public Boolean NoCheckedDuel;
        [JsonProperty(LanguageEn ? "Setting up Friends" : "Настройка Friends")]
        public FriendsReference FriendsSetting;
        [JsonProperty(LanguageEn ? "Setting up Clans" : "Настройка Clans")]
        public ClansReference ClansSetting;
        [JsonProperty(LanguageEn ? "Setting up MultiFighting (Luma)" : "Настройка MultiFighting (Luma)")]
        public MultiFighting MultiFightingSetting;
        [JsonProperty(LanguageEn ? "Setting up StopDamageMan" : "Настройка StopDamageMan")]
        public StopDamageMan StopDamageManSetting;
        [JsonProperty(LanguageEn ? "Setting up RCC support" : "Настройка поддержки RCC")]
        public RustCheatCheck RCCSettings;
        [JsonProperty(LanguageEn ? "Setting up OzProtect support" : "Настройка поддержки OzProtect")]
        public OzProtectCheck OzProtectSettings;
        internal class MultiFighting
        {
            [JsonProperty(LanguageEn ? "MultiFighting (Luma) : Display an icon with the player status - `Steam` / `Pirate`" : "MultiFighting (Luma) : Отображать иконку со статусом игрока - `Steam` / `Пират`")]
            public Boolean UseSteamCheck;
        }

        internal class StopDamageMan
        {
            [JsonProperty(LanguageEn ? "StopDamageMan : Disable player damage during check" : "StopDamageMan : Отключать игроку урон во время проверки")]
            public Boolean UseStopDamage;
        }

        internal class RustCheatCheck
        {
            [JsonProperty(LanguageEn ? "RCC key (if you don't need RCC support, leave the key blank)" : "Ключ RCC (если вам не нужна поддержка RCC - оставьте ключ пустым)")]
            public String RCCKey;
        }

        internal class OzProtectCheck
        {
            [JsonProperty(LanguageEn ? "OzProtect key (if you don't need OzProtect support, leave the key blank)" : "Ключ OzProtect (если вам не нужна поддержка OzProtect - оставьте ключ пустым)")]
            public String OzProtectKey;
        }

        internal class IQChatReference
        {
            [JsonProperty(LanguageEn ? "IQChat : Custom prefix in chat" : "IQChat : Кастомный префикс в чате")]
            public String CustomPrefix;
            [JsonProperty(LanguageEn ? "IQChat : Custom chat avatar (If required)" : "IQChat : Кастомный аватар в чате(Если требуется)")]
            public String CustomAvatar;
            [JsonProperty(LanguageEn ? "IQChat : Use UI notification (true - yes/false - no)" : "IQChat : Использовать UI уведомление (true - да/false - нет)")]
            public Boolean UIAlertUse;
        }

        internal class NoEscapeReference
        {
            [JsonProperty(LanguageEn ? "NoEscape : Reschedule a player's check if he has a `Raid-Block` (true - yes/false - no)" : "NoEscape : Перенести проверку игрока если у него есть `Raid-Блок` (true - да/false - нет)")]
            public Boolean NoCheckedRaidBlock;
            [JsonProperty(LanguageEn ? "NoEscape : Reschedule a player's check if he has a `Combat-Block` (true - yes/false - no)" : "NoEscape : Перенести проверку игрока если у него есть `Комбат-Блок` (true - да/false - нет)")]
            public Boolean NoCheckedCombatBlock;
        }

        internal class FriendsReference
        {
            [JsonProperty(LanguageEn ? "Friends : Prohibit players in the team from sending reports to each other (true - yes/false - no)" : "Friends : Запретить игрокам в команде отправлять репорты друг на друга (true - да/false - нет)")]
            public Boolean SendReportFriend;
            [JsonProperty(LanguageEn ? "Friends : Prohibit the moderator from checking his teammate (true - yes/false - no)" : "Friends : Запретить модератору проверять своего тиммейта (true - да/false - нет)")]
            public Boolean StartCheckedFriend;
        }

        internal class ClansReference
        {
            [JsonProperty(LanguageEn ? "Clans : Prohibit players in the same clan from sending reports to each other (true - yes/false - no)" : "Clans : Запретить игрокам в одном клане отправлять репорты друг на друга (true - да/false - нет)")]
            public Boolean SendReportClan;
            [JsonProperty(LanguageEn ? "Clans : Prohibit the moderator from checking the members of his clan (true - yes/false - no)" : "Clans : Запретить модератору проверять участников своего клана (true - да/false - нет)")]
            public Boolean StartCheckedClan;
        }

    }

    internal class LangText
    {
        [JsonProperty(LanguageEn ? "Reason title russian" : "Причина на русском")]
        public String LanguageRU;
        [JsonProperty(LanguageEn ? "Reason title english" : "Причина на английском")]
        public String LanguageEN;
        public String GetReasonTitle(UInt64 TargetID);
    }

    public static Configuration GetNewConfiguration();
}

internal class VerdictController
{
    [JsonProperty(LanguageEn ? "Banned all the `Friends` of a player who has been given a verdict by a moderator (true - yes/false - no)" : "Блокировать всех `Друзей` игрока, которому вынес вердикт модератор (true - да/false - нет)")]
    public Boolean UseBanAllTeam;
    [JsonProperty(LanguageEn ? "Index from the list of complaints when blocking a player's `Friends` (From your list - starts from 0)" : "Индекс из списка жалоб при блокировки `Друзей` игрока  (Из вашего списка - начинается от 0)")]
    public Int32 IndexBanReason;
}

internal class CheckController
{
    [JsonProperty(LanguageEn ? "Use sound notification for players when calling for verification (true - yes / false - no) [You must upload sound files to the IQSystem/IQReportSystem/Sounds folder]" : "Использовать звуковое оповещение для игроков при вызове на проверку (true - да/false - нет) [Вы должны загрузить файлы со звуком в папку IQSystem/IQReportSystem/Sounds]")]
    public Boolean UseSoundAlert;
    [JsonProperty(LanguageEn ? "Record a demos of the player during his check" : "Записывать демо игрока во время его проверки")]
    public Boolean UseDemo;
    [JsonProperty(LanguageEn ? "Use AFK validation before calling for check (true - yes / false - no)" : "Использовать проверку на AFK перед вызовом на проверку (true - да/false - нет)")]
    public Boolean UseCheckAFK;
    [JsonProperty(LanguageEn ? "Cancel check for a player automatically with saving reports if he left the server for 15 minutes or more (true - yes/false - no)" : "Отменять проверку игроку автоматически с сохранением репортов если он покинул сервер на 15 минут и более (true - да/false - нет)")]
    public Boolean StopCheckLeavePlayer;
    [JsonProperty(LanguageEn ? "Use tracking of crafting of invaders by the player (will notify the moderator about it) (true - yes / false - no)" : "Использовать отслеживание крафта предметов игроком (будет уведомлять модератора об этом) (true - да/false - нет)")]
    public Boolean TrackCrafting;
    [JsonProperty(LanguageEn ? "Use tracking of messages sent to the chat by the player (will notify the moderator about it) (true - yes / false - no)" : "Использовать отслеживание отправки сообщений в чат игроком (будет уведомлять модератора об этом) (true - да/false - нет)")]
    public Boolean TrackChat;
    [JsonProperty(LanguageEn ? "Use player command usage tracking (will notify the moderator about it) (true - yes / false - no)" : "Использовать отслеживание использования команд игроком (будет уведомлять модератора об этом) (true - да/false - нет)")]
    public Boolean TrackCommand;
}

internal class ReportContollerModeration
{
    [JsonProperty(LanguageEn ? "Maximum number of reports to display the player in the moderator menu and moderator notifications" : "Максимальное количество репортов для отображения игрока в меню модератора и уведомления модератора")]
    public Int32 ReportCountTrigger;
    [JsonProperty(LanguageEn ? "Setting up moderator notifications about the maximum number of player reports" : "Настройка уведомления модератора о максимальном количестве репортов игрока")]
    public AlertModeration AlertModerationSettings;
    internal class AlertModeration
    {
        [JsonProperty(LanguageEn ? "Notify the moderator that the player has scored the maximum number of reports" : "Уведомлять модератора о том, что игрок набрал максимальное количество репортов")]
        public Boolean AlertModerator;
        [JsonProperty(LanguageEn ? "Enable an audio notification to the moderator during the notification of the number of reports" : "Включать звуковое уведомление модератору во время уведомления о количестве репортов")]
        public Boolean AlertSound;
        [JsonProperty(LanguageEn ? "The path to the notification sound (this is the path of the prefab of the game - you can see here : https://github.com/OrangeWulf/Rust-Docs/blob/master/Extended/Effects.md)" : "Путь до звука уведомления (это путь префаба игры - посмотреть можно тут : https://github.com/OrangeWulf/Rust-Docs/blob/master/Extended/Effects.md)")]
        public String PathSound;
    }

}

internal class AlertModeration
{
    [JsonProperty(LanguageEn ? "Notify the moderator that the player has scored the maximum number of reports" : "Уведомлять модератора о том, что игрок набрал максимальное количество репортов")]
    public Boolean AlertModerator;
    [JsonProperty(LanguageEn ? "Enable an audio notification to the moderator during the notification of the number of reports" : "Включать звуковое уведомление модератору во время уведомления о количестве репортов")]
    public Boolean AlertSound;
    [JsonProperty(LanguageEn ? "The path to the notification sound (this is the path of the prefab of the game - you can see here : https://github.com/OrangeWulf/Rust-Docs/blob/master/Extended/Effects.md)" : "Путь до звука уведомления (это путь префаба игры - посмотреть можно тут : https://github.com/OrangeWulf/Rust-Docs/blob/master/Extended/Effects.md)")]
    public String PathSound;
}

internal class ReportSendController
{
    [JsonProperty(LanguageEn ? "Prohibit a player from sending a complaint against one player several times (true - yes/false - no)" : "Запретить игроку несколько раз отправлять жалобу на одного игрока (true - да/false - нет)")]
    public Boolean NoRepeatReport;
    [JsonProperty(LanguageEn ? "Cooldown before sending a complaint to the players (in seconds) (if you don't need a recharge, leave 0)" : "Перезарядка перед отправкой жалобы на игроков (в секундах) (если вам не нужна перезарядка - оставьте 0)")]
    public Int32 CooldownReport;
    [JsonProperty(LanguageEn ? "Use cooldown before sending a complaint only for a repeated complaint against one player (true) otherwise for all players (false)" : "Использовать перезарядку перед отправкой жалобы только на повторную жалобу на одного игрока (true) иначе на всех игроков (false)")]
    public Boolean CooldownRepeatOrAll;
}

internal class ReportF7AndGameMenu
{
    [JsonProperty(LanguageEn ? "Use sending report via F7 and the RUST game menu (true - yes/false - no)" : "Использовать отправку жалоб через F7 и игровое меню RUST (true - да/false - нет)")]
    public Boolean UseFunction;
    [JsonProperty(LanguageEn ? "Index from the list of complaints when sent via F7 and the RUST game menu (From your list - starts from 0)" : "Индекс из списка жалоб при отправке через F7 и игровое меню RUST (Из вашего списка - начинается от 0)")]
    public Int32 DefaultIndexReason;
}

internal class NotifyChat
{
    [JsonProperty(LanguageEn ? "Notify players when a moderator has started checking a player (configurable in the language file)" : "Уведомлять игроков о том, что модератор начал проверку игрока (настраивается в языковом файле)")]
    public Boolean UseNotifyCheck;
    [JsonProperty(LanguageEn ? "Notify players when a moderator has finished checking a player (configurable in the language file)" : "Уведомлять игроков о том, что модератор завершил проверку игрока (настраивается в языковом файле)")]
    public Boolean UseNotifyStopCheck;
    [JsonProperty(LanguageEn ? "Notify players that the moderator has completed the verification of the player and issued a verdict (banned) (configurable in the language file)" : "Уведомлять игроков о том, что модератор завершил проверку игрока и вынес вердикт (забанил) (настраивается в языковом файле)")]
    public Boolean UseNotifyVerdictCheck;
}

internal class NotifyVK
{
    [JsonProperty(LanguageEn ? "Token from the VK group (you can find it in the community settings)" : "Токен от группы ВК (вы можете найти его в настройках сообщества)")]
    public String VKTokenGroup;
    [JsonProperty(LanguageEn ? "ID of the conversation the bot is invited to (countdown starts from 1 - every new conversation +1)" : "ID беседы в которую приглашен бот (отсчет начинается с 1 - каждую новую беседу +1)")]
    public String VKChatID;
}

internal class NotifyDiscord
{
    [JsonProperty(LanguageEn ? "Set up WebHooks to send to Discord (if you don't need this feature - leave the field blank)" : "Настройка WebHooks для отправки в Discord (если вам не нужна эта функция - оставьте поле пустым)")]
    public Webhooks WebhooksList;
    internal class Webhooks
    {
        internal class TemplatesNotify
        {
            [JsonProperty("Webhook")]
            public String WebhookNotify;
            [JsonProperty(LanguageEn ? "Discord message color (Can be found on the website - https://old.message.style/dashboard in the JSON section)" : "Цвет сообщения в Discord (Можно найти на сайте - https://old.message.style/dashboard в разделе JSON)")]
            public Int32 Color;
            [JsonProperty(LanguageEn ? "Title message" : "Заголовок сообщения")]
            public String AuthorName;
            [JsonProperty(LanguageEn ? "Link to the icon for the avatar of the message" : "Ссылка на иконку для аватарки сообщения")]
            public String IconURL;
        }

        [JsonProperty(LanguageEn ? "WebHook : Setting up sending information about the start of a check" : "WebHook : Настройка отправки информации о начале проверки")]
        public TemplatesNotify NotifyStartCheck;
        [JsonProperty(LanguageEn ? "WebHook : Setting up sending information about the stop of a check" : "WebHook : Настройка отправки информации о завершении проверки")]
        public TemplatesNotify NotifyStopCheck;
        [JsonProperty(LanguageEn ? "WebHook : Settings for sending player contact information (when a player sends their Discord)" : "WebHook : Настройка отправки информации о контактах игрока (когда игрок отправляет свой Discord)")]
        public TemplatesNotify NotifyContacts;
        [JsonProperty(LanguageEn ? "WebHook : Setting up sending information about player complaints" : "WebHook : Настройка отправки информации о жалобах игроков")]
        public TemplatesNotify NotifySendReport;
        [JsonProperty(LanguageEn ? "WebHook : Setting up sending information when a player has exceeded the maximum number of complaints" : "WebHook : Настройка отправки информации когда игрок превысил максимальное количество жалоб")]
        public TemplatesNotify NotifyMaxReport;
        [JsonProperty(LanguageEn ? "WebHook : Setting up sending information about changing the status of the player and moderator" : "WebHook : Настройка отправки информации о изменении статуса игрока и модератора")]
        public TemplatesNotify NotifyStatusPlayerOrModerator;
    }

}

internal class Webhooks
{
    internal class TemplatesNotify
    {
        [JsonProperty("Webhook")]
        public String WebhookNotify;
        [JsonProperty(LanguageEn ? "Discord message color (Can be found on the website - https://old.message.style/dashboard in the JSON section)" : "Цвет сообщения в Discord (Можно найти на сайте - https://old.message.style/dashboard в разделе JSON)")]
        public Int32 Color;
        [JsonProperty(LanguageEn ? "Title message" : "Заголовок сообщения")]
        public String AuthorName;
        [JsonProperty(LanguageEn ? "Link to the icon for the avatar of the message" : "Ссылка на иконку для аватарки сообщения")]
        public String IconURL;
    }

    [JsonProperty(LanguageEn ? "WebHook : Setting up sending information about the start of a check" : "WebHook : Настройка отправки информации о начале проверки")]
    public TemplatesNotify NotifyStartCheck;
    [JsonProperty(LanguageEn ? "WebHook : Setting up sending information about the stop of a check" : "WebHook : Настройка отправки информации о завершении проверки")]
    public TemplatesNotify NotifyStopCheck;
    [JsonProperty(LanguageEn ? "WebHook : Settings for sending player contact information (when a player sends their Discord)" : "WebHook : Настройка отправки информации о контактах игрока (когда игрок отправляет свой Discord)")]
    public TemplatesNotify NotifyContacts;
    [JsonProperty(LanguageEn ? "WebHook : Setting up sending information about player complaints" : "WebHook : Настройка отправки информации о жалобах игроков")]
    public TemplatesNotify NotifySendReport;
    [JsonProperty(LanguageEn ? "WebHook : Setting up sending information when a player has exceeded the maximum number of complaints" : "WebHook : Настройка отправки информации когда игрок превысил максимальное количество жалоб")]
    public TemplatesNotify NotifyMaxReport;
    [JsonProperty(LanguageEn ? "WebHook : Setting up sending information about changing the status of the player and moderator" : "WebHook : Настройка отправки информации о изменении статуса игрока и модератора")]
    public TemplatesNotify NotifyStatusPlayerOrModerator;
}

internal class TemplatesNotify
{
    [JsonProperty("Webhook")]
    public String WebhookNotify;
    [JsonProperty(LanguageEn ? "Discord message color (Can be found on the website - https://old.message.style/dashboard in the JSON section)" : "Цвет сообщения в Discord (Можно найти на сайте - https://old.message.style/dashboard в разделе JSON)")]
    public Int32 Color;
    [JsonProperty(LanguageEn ? "Title message" : "Заголовок сообщения")]
    public String AuthorName;
    [JsonProperty(LanguageEn ? "Link to the icon for the avatar of the message" : "Ссылка на иконку для аватарки сообщения")]
    public String IconURL;
}

internal class ReasonReport
{
    [JsonProperty(LanguageEn ? "Reason" : "Причина")]
    public LangText Title;
    [JsonProperty(LanguageEn ? "The command from your ban system to block the user ({0} - will be replaced by the player's ID)" : "Команда вашей бан-системы на блокировку пользователя ({0} - заменится на ID игрока)")]
    public String BanCommand;
    [JsonProperty(LanguageEn ? "Hide this reason from the player (true) (will only be seen by the moderator when passing a verdict)" : "Скрыть данную причину от игрока (true) (будет видеть только модератор при вынесении вердикта)")]
    public Boolean HideUser;
}

internal class Images
{
    [JsonProperty(LanguageEn ? "Reports section images" : "Изображения раздела для жалоб")]
    public PlayerListBlock PlayerListBlockSettings;
    [JsonProperty(LanguageEn ? "Images of the statistics section" : "Изображения раздела статистики")]
    public StatisticsBlock StatisticsBlockSettings;
    [JsonProperty(LanguageEn ? "Left menu images" : "Изображения левого меню")]
    public LeftBlock LeftBlockSettings;
    [JsonProperty(LanguageEn ? "Moderation section images" : "Изображения раздела модерации")]
    public ModerationBlock ModerationBlockSettings;
    [JsonProperty(LanguageEn ? "Moderator menu images when checking" : "Изображения меню модератора при проверки")]
    public ModeratorMenuChecked ModeratorMenuCheckedSettings;
    [JsonProperty(LanguageEn ? "Images of the player rating menu quality of the moderator's work" : "Изображения меню оценки игроком качество работы модератора")]
    public PlayerMenuRaiting PlayerMenuRaitingSettings;
    [JsonProperty(LanguageEn ? "PNG of the menu background (1382x950)" : "PNG заднего фона меню (1382x950)")]
    public String Background;
    [JsonProperty(LanguageEn ? "PNG down arrows (page flipping) (10x5)" : "PNG стрелки вниз(перелистывание страниц) (10x5)")]
    public String PageDown;
    [JsonProperty(LanguageEn ? "PNG up arrows (page flipping) (10x5)" : "PNG стрелки вверх(перелистывание страниц) (10x5)")]
    public String PageUp;
    [JsonProperty(LanguageEn ? "PNG icon search (16x16)" : "PNG иконки поиска (16x16)")]
    public String Search;
    [JsonProperty(LanguageEn ? "PNG : Icon for adjusting avatar (64x64)" : "PNG : Иконка для корректировки автарки (64x64)")]
    public String AvatarBlur;
    [JsonProperty(LanguageEn ? "PNG : Icon for moderation verdict or rating (307x36)" : "PNG : Иконка для вердикта модерации или рейтинга (307x36)")]
    public String ReasonModeratorAndRaiting;
    [JsonProperty(LanguageEn ? "PNG : Image on the player's screen with text about the start of the checks (1450x559)" : "PNG : Изображение на экране игрока с текстом о начале проверки (1450x559)")]
    public String PlayerAlerts;
    internal class PlayerMenuRaiting
    {
        [JsonProperty(LanguageEn ? "PNG : Menu background when evaluating a reviewer (307x98)" : "PNG : Задний фон меню при оценке проверяющего (307x98)")]
        public String PlayerMenuRaitingBackground;
    }

    internal class ModeratorMenuChecked
    {
        [JsonProperty(LanguageEn ? "PNG : Menu background when checking player (307x148)" : "PNG : Задний фон меню при проверки игрока (307x148)")]
        public String ModeratorCheckedBackground;
        [JsonProperty(LanguageEn ? "PNG : End test button (128x40)" : "PNG : Кнопка завершения проверки (128x40)")]
        public String ModeratorCheckedStopButton;
        [JsonProperty(LanguageEn ? "PNG : Verdict button (128x40)" : "PNG : Кнопка вердикта (128x40)")]
        public String ModeratorVerdictButton;
        [JsonProperty(LanguageEn ? "PNG : `Licenses` icon (if Lumia support is enabled) (16x16)" : "PNG : Иконка `Лицензии` (если включена поддержка Luma) (16x16)")]
        public String SteamIcoPlayer;
        [JsonProperty(LanguageEn ? "PNG : `Pirate` icon (if Lumia support is enabled) (16x16)" : "PNG : Иконка `Пират` (если включена поддержка Luma) (16x16)")]
        public String PirateIcoPlayer;
    }

    internal class ModerationBlock
    {
        [JsonProperty(LanguageEn ? "PNG : Player information popup background (831x599)" : "PNG : Задний фон всплывающего окна с информацией о игроке (831x599)")]
        public String ModeratorPoopUPBackgorund;
        [JsonProperty(LanguageEn ? "PNG : Element background for text (155x22)" : "PNG : Задний фон элемента для текста (155x22)")]
        public String ModeratorPoopUPTextBackgorund;
        [JsonProperty(LanguageEn ? "PNG : Information panel background (196x240)" : "PNG : Задний фон панели с информацией (196x240)")]
        public String ModeratorPoopUPPanelBackgorund;
    }

    internal class LeftBlock
    {
        [JsonProperty(LanguageEn ? "PNG : Icon for sidebar button (192x55)" : "PNG : Иконка для кнопки в боковом меню (192x55)")]
        public String ButtonBackgorund;
        [JsonProperty(LanguageEn ? "PNG : Icon for the button in `reports` (32x32)" : "PNG : Иконка для кнопки в `жалобы`(32x32)")]
        public String ReportIcon;
        [JsonProperty(LanguageEn ? "PNG : Icon for the button in `moderation` (32x32)" : "PNG : Иконка для кнопки в `модерация`(32x32)")]
        public String ModerationIcon;
    }

    internal class PlayerListBlock
    {
        [JsonProperty(LanguageEn ? "PNG : Cause selection popup background (567x599)" : "PNG : Задний фон всплывающего сообщения с выбором причины (567x599)")]
        public String PoopUpBackgorund;
        [JsonProperty(LanguageEn ? "PNG : Reason background in popup (463x87)" : "PNG : Задний фон причины в всплывающем окне (463x87)")]
        public String PoopUpReasonBackgorund;
    }

    internal class StatisticsBlock
    {
        [JsonProperty(LanguageEn ? "PNG Background of the statistics block (283x81)" : "PNG Задний фон блока статистики (283x81)")]
        public String BlockStatsModeration;
        [JsonProperty(LanguageEn ? "PNG Background of the rating block in statistics (65x28)" : "PNG Задний фон блока рейтинга в статистике (65x28)")]
        public String BlockStatsRaitingModeration;
        [JsonProperty(LanguageEn ? "PNG : Rating icon in statistics (16x15)" : "PNG : Иконка рейтинга в статистике (16x15)")]
        public String RaitingImage;
    }

}

internal class PlayerMenuRaiting
{
    [JsonProperty(LanguageEn ? "PNG : Menu background when evaluating a reviewer (307x98)" : "PNG : Задний фон меню при оценке проверяющего (307x98)")]
    public String PlayerMenuRaitingBackground;
}

internal class ModeratorMenuChecked
{
    [JsonProperty(LanguageEn ? "PNG : Menu background when checking player (307x148)" : "PNG : Задний фон меню при проверки игрока (307x148)")]
    public String ModeratorCheckedBackground;
    [JsonProperty(LanguageEn ? "PNG : End test button (128x40)" : "PNG : Кнопка завершения проверки (128x40)")]
    public String ModeratorCheckedStopButton;
    [JsonProperty(LanguageEn ? "PNG : Verdict button (128x40)" : "PNG : Кнопка вердикта (128x40)")]
    public String ModeratorVerdictButton;
    [JsonProperty(LanguageEn ? "PNG : `Licenses` icon (if Lumia support is enabled) (16x16)" : "PNG : Иконка `Лицензии` (если включена поддержка Luma) (16x16)")]
    public String SteamIcoPlayer;
    [JsonProperty(LanguageEn ? "PNG : `Pirate` icon (if Lumia support is enabled) (16x16)" : "PNG : Иконка `Пират` (если включена поддержка Luma) (16x16)")]
    public String PirateIcoPlayer;
}

internal class ModerationBlock
{
    [JsonProperty(LanguageEn ? "PNG : Player information popup background (831x599)" : "PNG : Задний фон всплывающего окна с информацией о игроке (831x599)")]
    public String ModeratorPoopUPBackgorund;
    [JsonProperty(LanguageEn ? "PNG : Element background for text (155x22)" : "PNG : Задний фон элемента для текста (155x22)")]
    public String ModeratorPoopUPTextBackgorund;
    [JsonProperty(LanguageEn ? "PNG : Information panel background (196x240)" : "PNG : Задний фон панели с информацией (196x240)")]
    public String ModeratorPoopUPPanelBackgorund;
}

internal class LeftBlock
{
    [JsonProperty(LanguageEn ? "PNG : Icon for sidebar button (192x55)" : "PNG : Иконка для кнопки в боковом меню (192x55)")]
    public String ButtonBackgorund;
    [JsonProperty(LanguageEn ? "PNG : Icon for the button in `reports` (32x32)" : "PNG : Иконка для кнопки в `жалобы`(32x32)")]
    public String ReportIcon;
    [JsonProperty(LanguageEn ? "PNG : Icon for the button in `moderation` (32x32)" : "PNG : Иконка для кнопки в `модерация`(32x32)")]
    public String ModerationIcon;
}

internal class PlayerListBlock
{
    [JsonProperty(LanguageEn ? "PNG : Cause selection popup background (567x599)" : "PNG : Задний фон всплывающего сообщения с выбором причины (567x599)")]
    public String PoopUpBackgorund;
    [JsonProperty(LanguageEn ? "PNG : Reason background in popup (463x87)" : "PNG : Задний фон причины в всплывающем окне (463x87)")]
    public String PoopUpReasonBackgorund;
}

internal class StatisticsBlock
{
    [JsonProperty(LanguageEn ? "PNG Background of the statistics block (283x81)" : "PNG Задний фон блока статистики (283x81)")]
    public String BlockStatsModeration;
    [JsonProperty(LanguageEn ? "PNG Background of the rating block in statistics (65x28)" : "PNG Задний фон блока рейтинга в статистике (65x28)")]
    public String BlockStatsRaitingModeration;
    [JsonProperty(LanguageEn ? "PNG : Rating icon in statistics (16x15)" : "PNG : Иконка рейтинга в статистике (16x15)")]
    public String RaitingImage;
}

internal class Colors
{
    [JsonProperty(LanguageEn ? "RGBA of the main text color" : "RGBA основного цвета текста")]
    public String MainColorText;
    [JsonProperty(LanguageEn ? "RGBA of additional text color" : "RGBA дополнительного цвета текста")]
    public String AdditionalColorText;
    [JsonProperty(LanguageEn ? "RGBA additional color of elements (Buttons, dies)" : "RGBA дополнительный цвет элементов (Кнопки, плашки)")]
    public String AdditionalColorElements;
    [JsonProperty(LanguageEn ? "RGBA additional color of elements (Buttons, dies) #2" : "RGBA дополнительный цвет элементов (Кнопки, плашки) #2")]
    public String AdditionalColorElementsTwo;
    [JsonProperty(LanguageEn ? "RGBA additional color of elements (Buttons, dies) #3" : "RGBA дополнительный цвет элементов (Кнопки, плашки) #3")]
    public String AdditionalColorElementsThree;
    [JsonProperty(LanguageEn ? "RGBA the main color" : "RGBA основной цвет")]
    public String MainColor;
}

internal class References
{
    [JsonProperty(LanguageEn ? "IQFakeActive : Use collaboration (true - yes/false - no)" : "IQFakeActive : Использовать совместную работу (true - да/false - нет)")]
    public Boolean IQFakeActiveUse;
    [JsonProperty(LanguageEn ? "Setting up IQChat" : "Настройка IQChat")]
    public IQChatReference IQChatSetting;
    [JsonProperty(LanguageEn ? "Setting up NoEscape" : "Настройка NoEscape")]
    public NoEscapeReference NoEscapeSetting;
    [JsonProperty(LanguageEn ? "Duels : Reschedule the player's check if he is in a duel (true - yes/false - no)" : "Duels : Перенести проверку игрока если он на дуэли (true - да/false - нет)")]
    public Boolean NoCheckedDuel;
    [JsonProperty(LanguageEn ? "Setting up Friends" : "Настройка Friends")]
    public FriendsReference FriendsSetting;
    [JsonProperty(LanguageEn ? "Setting up Clans" : "Настройка Clans")]
    public ClansReference ClansSetting;
    [JsonProperty(LanguageEn ? "Setting up MultiFighting (Luma)" : "Настройка MultiFighting (Luma)")]
    public MultiFighting MultiFightingSetting;
    [JsonProperty(LanguageEn ? "Setting up StopDamageMan" : "Настройка StopDamageMan")]
    public StopDamageMan StopDamageManSetting;
    [JsonProperty(LanguageEn ? "Setting up RCC support" : "Настройка поддержки RCC")]
    public RustCheatCheck RCCSettings;
    [JsonProperty(LanguageEn ? "Setting up OzProtect support" : "Настройка поддержки OzProtect")]
    public OzProtectCheck OzProtectSettings;
    internal class MultiFighting
    {
        [JsonProperty(LanguageEn ? "MultiFighting (Luma) : Display an icon with the player status - `Steam` / `Pirate`" : "MultiFighting (Luma) : Отображать иконку со статусом игрока - `Steam` / `Пират`")]
        public Boolean UseSteamCheck;
    }

    internal class StopDamageMan
    {
        [JsonProperty(LanguageEn ? "StopDamageMan : Disable player damage during check" : "StopDamageMan : Отключать игроку урон во время проверки")]
        public Boolean UseStopDamage;
    }

    internal class RustCheatCheck
    {
        [JsonProperty(LanguageEn ? "RCC key (if you don't need RCC support, leave the key blank)" : "Ключ RCC (если вам не нужна поддержка RCC - оставьте ключ пустым)")]
        public String RCCKey;
    }

    internal class OzProtectCheck
    {
        [JsonProperty(LanguageEn ? "OzProtect key (if you don't need OzProtect support, leave the key blank)" : "Ключ OzProtect (если вам не нужна поддержка OzProtect - оставьте ключ пустым)")]
        public String OzProtectKey;
    }

    internal class IQChatReference
    {
        [JsonProperty(LanguageEn ? "IQChat : Custom prefix in chat" : "IQChat : Кастомный префикс в чате")]
        public String CustomPrefix;
        [JsonProperty(LanguageEn ? "IQChat : Custom chat avatar (If required)" : "IQChat : Кастомный аватар в чате(Если требуется)")]
        public String CustomAvatar;
        [JsonProperty(LanguageEn ? "IQChat : Use UI notification (true - yes/false - no)" : "IQChat : Использовать UI уведомление (true - да/false - нет)")]
        public Boolean UIAlertUse;
    }

    internal class NoEscapeReference
    {
        [JsonProperty(LanguageEn ? "NoEscape : Reschedule a player's check if he has a `Raid-Block` (true - yes/false - no)" : "NoEscape : Перенести проверку игрока если у него есть `Raid-Блок` (true - да/false - нет)")]
        public Boolean NoCheckedRaidBlock;
        [JsonProperty(LanguageEn ? "NoEscape : Reschedule a player's check if he has a `Combat-Block` (true - yes/false - no)" : "NoEscape : Перенести проверку игрока если у него есть `Комбат-Блок` (true - да/false - нет)")]
        public Boolean NoCheckedCombatBlock;
    }

    internal class FriendsReference
    {
        [JsonProperty(LanguageEn ? "Friends : Prohibit players in the team from sending reports to each other (true - yes/false - no)" : "Friends : Запретить игрокам в команде отправлять репорты друг на друга (true - да/false - нет)")]
        public Boolean SendReportFriend;
        [JsonProperty(LanguageEn ? "Friends : Prohibit the moderator from checking his teammate (true - yes/false - no)" : "Friends : Запретить модератору проверять своего тиммейта (true - да/false - нет)")]
        public Boolean StartCheckedFriend;
    }

    internal class ClansReference
    {
        [JsonProperty(LanguageEn ? "Clans : Prohibit players in the same clan from sending reports to each other (true - yes/false - no)" : "Clans : Запретить игрокам в одном клане отправлять репорты друг на друга (true - да/false - нет)")]
        public Boolean SendReportClan;
        [JsonProperty(LanguageEn ? "Clans : Prohibit the moderator from checking the members of his clan (true - yes/false - no)" : "Clans : Запретить модератору проверять участников своего клана (true - да/false - нет)")]
        public Boolean StartCheckedClan;
    }

}

internal class MultiFighting
{
    [JsonProperty(LanguageEn ? "MultiFighting (Luma) : Display an icon with the player status - `Steam` / `Pirate`" : "MultiFighting (Luma) : Отображать иконку со статусом игрока - `Steam` / `Пират`")]
    public Boolean UseSteamCheck;
}

internal class StopDamageMan
{
    [JsonProperty(LanguageEn ? "StopDamageMan : Disable player damage during check" : "StopDamageMan : Отключать игроку урон во время проверки")]
    public Boolean UseStopDamage;
}

internal class RustCheatCheck
{
    [JsonProperty(LanguageEn ? "RCC key (if you don't need RCC support, leave the key blank)" : "Ключ RCC (если вам не нужна поддержка RCC - оставьте ключ пустым)")]
    public String RCCKey;
}

internal class OzProtectCheck
{
    [JsonProperty(LanguageEn ? "OzProtect key (if you don't need OzProtect support, leave the key blank)" : "Ключ OzProtect (если вам не нужна поддержка OzProtect - оставьте ключ пустым)")]
    public String OzProtectKey;
}

internal class IQChatReference
{
    [JsonProperty(LanguageEn ? "IQChat : Custom prefix in chat" : "IQChat : Кастомный префикс в чате")]
    public String CustomPrefix;
    [JsonProperty(LanguageEn ? "IQChat : Custom chat avatar (If required)" : "IQChat : Кастомный аватар в чате(Если требуется)")]
    public String CustomAvatar;
    [JsonProperty(LanguageEn ? "IQChat : Use UI notification (true - yes/false - no)" : "IQChat : Использовать UI уведомление (true - да/false - нет)")]
    public Boolean UIAlertUse;
}

internal class NoEscapeReference
{
    [JsonProperty(LanguageEn ? "NoEscape : Reschedule a player's check if he has a `Raid-Block` (true - yes/false - no)" : "NoEscape : Перенести проверку игрока если у него есть `Raid-Блок` (true - да/false - нет)")]
    public Boolean NoCheckedRaidBlock;
    [JsonProperty(LanguageEn ? "NoEscape : Reschedule a player's check if he has a `Combat-Block` (true - yes/false - no)" : "NoEscape : Перенести проверку игрока если у него есть `Комбат-Блок` (true - да/false - нет)")]
    public Boolean NoCheckedCombatBlock;
}

internal class FriendsReference
{
    [JsonProperty(LanguageEn ? "Friends : Prohibit players in the team from sending reports to each other (true - yes/false - no)" : "Friends : Запретить игрокам в команде отправлять репорты друг на друга (true - да/false - нет)")]
    public Boolean SendReportFriend;
    [JsonProperty(LanguageEn ? "Friends : Prohibit the moderator from checking his teammate (true - yes/false - no)" : "Friends : Запретить модератору проверять своего тиммейта (true - да/false - нет)")]
    public Boolean StartCheckedFriend;
}

internal class ClansReference
{
    [JsonProperty(LanguageEn ? "Clans : Prohibit players in the same clan from sending reports to each other (true - yes/false - no)" : "Clans : Запретить игрокам в одном клане отправлять репорты друг на друга (true - да/false - нет)")]
    public Boolean SendReportClan;
    [JsonProperty(LanguageEn ? "Clans : Prohibit the moderator from checking the members of his clan (true - yes/false - no)" : "Clans : Запретить модератору проверять участников своего клана (true - да/false - нет)")]
    public Boolean StartCheckedClan;
}

internal class LangText
{
    [JsonProperty(LanguageEn ? "Reason title russian" : "Причина на русском")]
    public String LanguageRU;
    [JsonProperty(LanguageEn ? "Reason title english" : "Причина на английском")]
    public String LanguageEN;
    public String GetReasonTitle(UInt64 TargetID);
}

private class PlayerInformation
{
    public Int32 Reports;
    public Int32 SendReports;
    public Int32 AmountChecked;
    public String LastModerator;
    public List<Configuration.LangText> ReasonHistory;
}

public class FakePlayer
{
    public UInt64 UserID;
    public String DisplayName;
}

public class SpeakerEntityMgr
{
    private static List<SpeakerEntity> _Entities;
    public static SpeakerEntity Create(BasePlayer listeners);
    public static void Shutdown();
    public static void Kill(SpeakerEntity entity);
    public class SpeakerEntity
    {
        public UInt64 UID_SPEAKER;
        private UInt64 UID_CHAIR;
        public BasePlayer Listeners { get; set; }
        public void SetListeners(BasePlayer listeners);
        public void SendEntitities();
        private ProtoBuf.Entity GetEntitySpeaker(BasePlayer player);
        private ProtoBuf.Entity GetEntityChair(BasePlayer player);
        public void Kill();
        private void SendEntity(Func<BasePlayer, ProtoBuf.Entity> entityGetter);
        private void DestroyEntity(UInt64 uid);
    }

}

public class SpeakerEntity
{
    public UInt64 UID_SPEAKER;
    private UInt64 UID_CHAIR;
    public BasePlayer Listeners { get; set; }
    public void SetListeners(BasePlayer listeners);
    public void SendEntitities();
    private ProtoBuf.Entity GetEntitySpeaker(BasePlayer player);
    private ProtoBuf.Entity GetEntityChair(BasePlayer player);
    public void Kill();
    private void SendEntity(Func<BasePlayer, ProtoBuf.Entity> entityGetter);
    private void DestroyEntity(UInt64 uid);
}


```

---

## IQSystemFragments

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using ConVar;
using Newtonsoft.Json;
using Oxide.Core.Plugins;
using UnityEngine;

Oxide.Plugins
[Info("IQSystemFragments", "xуй", "0.0.4")]
[Description("Система фрагментов")]
 class IQSystemFragments : RustPlugin
{
    public string ReplaceShortname;
    public string ReplaceShortnameFull;
    [PluginReference]
     Plugin IQChat;
     Plugin IQEconomic;
    public void SetBalance(ulong userID, int Balance);
    public void SendChat(BasePlayer player, string Message, Chat.ChatChannel channel);
    private static Configuration config;
    private class Configuration
    {
        [JsonProperty("Настройка плагина")]
        public GeneralSettings GeneralSetting;
        [JsonProperty("Настройка системы фрагментов")]
        public Dictionary<string, ItemSettings> ItemSetting;
        internal class GeneralSettings
        {
            [JsonProperty("Настройки IQChat")]
            public ChatSettings ChatSetting;
            [JsonProperty("Настройки IQPlagueSkill")]
            public IQPlagueSkills IQPlagueSkill;
            internal class ChatSettings
            {
                [JsonProperty("IQChat : Кастомный префикс в чате")]
                public string CustomPrefix;
                [JsonProperty("IQChat : Кастомный аватар в чате(Если требуется)")]
                public string CustomAvatar;
            }

            internal class IQPlagueSkills
            {
                [JsonProperty("На сколько увеличить шанс выпадения фрагментов")]
                public int RareUpFragments;
                [JsonProperty("На сколько увеличить шанс выпадения полных частей")]
                public int RareUpFull;
            }

        }

        internal class ItemSettings
        {
            [JsonProperty("Отображаемое имя привилегии")]
            public string DisplayName;
            [JsonProperty("Сколько требуется фрагментов на получение награды")]
            public int AmountFragmets;
            [JsonProperty("SkinID для полного комлпекта")]
            public ulong SkinID;
            [JsonProperty("Настройка фрагментов")]
            public FragmentSettings FragmentSetting;
            [JsonProperty("Настройка награды")]
            public RewardSettings RewardSetting;
            [JsonProperty("Настройка выпадения целого фрагмента.[откуда] = шанс")]
            public Dictionary<string, int> DropListRareFull;
            internal class FragmentSettings
            {
                [JsonProperty("Отображаемое имя фрагмента")]
                public string DisplayName;
                [JsonProperty("SkinID фрагмента")]
                public ulong SkinID;
                [JsonProperty("Настройка выпадения.[откуда] = шанс")]
                public Dictionary<string, int> DropListRare;
            }

            internal class RewardSettings
            {
                [JsonProperty("ТИП ПРИЗА : 0 - Команда  , 1 - Лист предметов, 2 - IQEconomic монеты, 3 - Список команд")]
                public TypeReward typeReward;
                [JsonProperty("Команда,которая отыграется,когда игрок заберет приз(ТИП ПРИЗА - 0) %USERID% - заменится на ID игрока")]
                public string Command;
                [JsonProperty("Список команд,которые отыграются,когда игрок заберет приз(ТИП ПРИЗА - 3) %USERID% - заменится на ID игрока")]
                public List<string> CommandList;
                [JsonProperty("Количество выдаваемых монет(ТИП ПРИЗА - 2)")]
                public int Balance;
                [JsonProperty("Настройка предметов из RUST'a(ТИП ПРИЗА - 1)")]
                public List<ItemRewardSettings> ItemRewardSetting;
                internal class ItemRewardSettings
                {
                    [JsonProperty("Отображаемое имя(если это не кастомный предмет,оставьте пустым)")]
                    public string DisplayName;
                    [JsonProperty("Shortname предмета")]
                    public string Shortname;
                    [JsonProperty("SkinID предмета")]
                    public ulong SkinID;
                    [JsonProperty("Количество для выдачи")]
                    public int Amount;
                }

            }

        }

        public static Configuration GetNewConfiguration();
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    [JsonProperty("Фрагменты игроков")]
    public Dictionary<ulong, Dictionary<string, int>> DataFragments;
     void ReadData();
     void WriteData();
     void RegisteredDataUser(ulong userID);
     void SendData(ulong userID, string Key, int Amount);
     void OnEntitySpawned(BaseNetworkable entity);
     object OnItemAction(Item item, string action, BasePlayer player);
    private static void ItemRemovalThink(Item item, BasePlayer player, int itemsToTake);
    private void OnServerInitialized();
     void OnPlayerConnected(BasePlayer player);
    [ConsoleCommand("iqsf")]
     void ConsoleCommandIQSF(ConsoleSystem.Arg arg);
     void SpawnMetods(TypeItem Types, BaseNetworkable entity);
     void FragmentGoToFull(BasePlayer player, string Key);
     void UnwrapFragmentFull(BasePlayer player, string Key);
    public bool IsRandom(int Rare);
    private Item CreateFragment(string DisplayName, ulong SkinID, string Shortname, int Amount);
    private Item OnItemSplit(Item item, int amount);
     object CanCombineDroppedItem(DroppedItem item, DroppedItem targetItem);
     object CanStackItem(Item item, Item targetItem);
    private new void LoadDefaultMessages();
}

private class Configuration
{
    [JsonProperty("Настройка плагина")]
    public GeneralSettings GeneralSetting;
    [JsonProperty("Настройка системы фрагментов")]
    public Dictionary<string, ItemSettings> ItemSetting;
    internal class GeneralSettings
    {
        [JsonProperty("Настройки IQChat")]
        public ChatSettings ChatSetting;
        [JsonProperty("Настройки IQPlagueSkill")]
        public IQPlagueSkills IQPlagueSkill;
        internal class ChatSettings
        {
            [JsonProperty("IQChat : Кастомный префикс в чате")]
            public string CustomPrefix;
            [JsonProperty("IQChat : Кастомный аватар в чате(Если требуется)")]
            public string CustomAvatar;
        }

        internal class IQPlagueSkills
        {
            [JsonProperty("На сколько увеличить шанс выпадения фрагментов")]
            public int RareUpFragments;
            [JsonProperty("На сколько увеличить шанс выпадения полных частей")]
            public int RareUpFull;
        }

    }

    internal class ItemSettings
    {
        [JsonProperty("Отображаемое имя привилегии")]
        public string DisplayName;
        [JsonProperty("Сколько требуется фрагментов на получение награды")]
        public int AmountFragmets;
        [JsonProperty("SkinID для полного комлпекта")]
        public ulong SkinID;
        [JsonProperty("Настройка фрагментов")]
        public FragmentSettings FragmentSetting;
        [JsonProperty("Настройка награды")]
        public RewardSettings RewardSetting;
        [JsonProperty("Настройка выпадения целого фрагмента.[откуда] = шанс")]
        public Dictionary<string, int> DropListRareFull;
        internal class FragmentSettings
        {
            [JsonProperty("Отображаемое имя фрагмента")]
            public string DisplayName;
            [JsonProperty("SkinID фрагмента")]
            public ulong SkinID;
            [JsonProperty("Настройка выпадения.[откуда] = шанс")]
            public Dictionary<string, int> DropListRare;
        }

        internal class RewardSettings
        {
            [JsonProperty("ТИП ПРИЗА : 0 - Команда  , 1 - Лист предметов, 2 - IQEconomic монеты, 3 - Список команд")]
            public TypeReward typeReward;
            [JsonProperty("Команда,которая отыграется,когда игрок заберет приз(ТИП ПРИЗА - 0) %USERID% - заменится на ID игрока")]
            public string Command;
            [JsonProperty("Список команд,которые отыграются,когда игрок заберет приз(ТИП ПРИЗА - 3) %USERID% - заменится на ID игрока")]
            public List<string> CommandList;
            [JsonProperty("Количество выдаваемых монет(ТИП ПРИЗА - 2)")]
            public int Balance;
            [JsonProperty("Настройка предметов из RUST'a(ТИП ПРИЗА - 1)")]
            public List<ItemRewardSettings> ItemRewardSetting;
            internal class ItemRewardSettings
            {
                [JsonProperty("Отображаемое имя(если это не кастомный предмет,оставьте пустым)")]
                public string DisplayName;
                [JsonProperty("Shortname предмета")]
                public string Shortname;
                [JsonProperty("SkinID предмета")]
                public ulong SkinID;
                [JsonProperty("Количество для выдачи")]
                public int Amount;
            }

        }

    }

    public static Configuration GetNewConfiguration();
}

internal class GeneralSettings
{
    [JsonProperty("Настройки IQChat")]
    public ChatSettings ChatSetting;
    [JsonProperty("Настройки IQPlagueSkill")]
    public IQPlagueSkills IQPlagueSkill;
    internal class ChatSettings
    {
        [JsonProperty("IQChat : Кастомный префикс в чате")]
        public string CustomPrefix;
        [JsonProperty("IQChat : Кастомный аватар в чате(Если требуется)")]
        public string CustomAvatar;
    }

    internal class IQPlagueSkills
    {
        [JsonProperty("На сколько увеличить шанс выпадения фрагментов")]
        public int RareUpFragments;
        [JsonProperty("На сколько увеличить шанс выпадения полных частей")]
        public int RareUpFull;
    }

}

internal class ChatSettings
{
    [JsonProperty("IQChat : Кастомный префикс в чате")]
    public string CustomPrefix;
    [JsonProperty("IQChat : Кастомный аватар в чате(Если требуется)")]
    public string CustomAvatar;
}

internal class IQPlagueSkills
{
    [JsonProperty("На сколько увеличить шанс выпадения фрагментов")]
    public int RareUpFragments;
    [JsonProperty("На сколько увеличить шанс выпадения полных частей")]
    public int RareUpFull;
}

internal class ItemSettings
{
    [JsonProperty("Отображаемое имя привилегии")]
    public string DisplayName;
    [JsonProperty("Сколько требуется фрагментов на получение награды")]
    public int AmountFragmets;
    [JsonProperty("SkinID для полного комлпекта")]
    public ulong SkinID;
    [JsonProperty("Настройка фрагментов")]
    public FragmentSettings FragmentSetting;
    [JsonProperty("Настройка награды")]
    public RewardSettings RewardSetting;
    [JsonProperty("Настройка выпадения целого фрагмента.[откуда] = шанс")]
    public Dictionary<string, int> DropListRareFull;
    internal class FragmentSettings
    {
        [JsonProperty("Отображаемое имя фрагмента")]
        public string DisplayName;
        [JsonProperty("SkinID фрагмента")]
        public ulong SkinID;
        [JsonProperty("Настройка выпадения.[откуда] = шанс")]
        public Dictionary<string, int> DropListRare;
    }

    internal class RewardSettings
    {
        [JsonProperty("ТИП ПРИЗА : 0 - Команда  , 1 - Лист предметов, 2 - IQEconomic монеты, 3 - Список команд")]
        public TypeReward typeReward;
        [JsonProperty("Команда,которая отыграется,когда игрок заберет приз(ТИП ПРИЗА - 0) %USERID% - заменится на ID игрока")]
        public string Command;
        [JsonProperty("Список команд,которые отыграются,когда игрок заберет приз(ТИП ПРИЗА - 3) %USERID% - заменится на ID игрока")]
        public List<string> CommandList;
        [JsonProperty("Количество выдаваемых монет(ТИП ПРИЗА - 2)")]
        public int Balance;
        [JsonProperty("Настройка предметов из RUST'a(ТИП ПРИЗА - 1)")]
        public List<ItemRewardSettings> ItemRewardSetting;
        internal class ItemRewardSettings
        {
            [JsonProperty("Отображаемое имя(если это не кастомный предмет,оставьте пустым)")]
            public string DisplayName;
            [JsonProperty("Shortname предмета")]
            public string Shortname;
            [JsonProperty("SkinID предмета")]
            public ulong SkinID;
            [JsonProperty("Количество для выдачи")]
            public int Amount;
        }

    }

}

internal class FragmentSettings
{
    [JsonProperty("Отображаемое имя фрагмента")]
    public string DisplayName;
    [JsonProperty("SkinID фрагмента")]
    public ulong SkinID;
    [JsonProperty("Настройка выпадения.[откуда] = шанс")]
    public Dictionary<string, int> DropListRare;
}

internal class RewardSettings
{
    [JsonProperty("ТИП ПРИЗА : 0 - Команда  , 1 - Лист предметов, 2 - IQEconomic монеты, 3 - Список команд")]
    public TypeReward typeReward;
    [JsonProperty("Команда,которая отыграется,когда игрок заберет приз(ТИП ПРИЗА - 0) %USERID% - заменится на ID игрока")]
    public string Command;
    [JsonProperty("Список команд,которые отыграются,когда игрок заберет приз(ТИП ПРИЗА - 3) %USERID% - заменится на ID игрока")]
    public List<string> CommandList;
    [JsonProperty("Количество выдаваемых монет(ТИП ПРИЗА - 2)")]
    public int Balance;
    [JsonProperty("Настройка предметов из RUST'a(ТИП ПРИЗА - 1)")]
    public List<ItemRewardSettings> ItemRewardSetting;
    internal class ItemRewardSettings
    {
        [JsonProperty("Отображаемое имя(если это не кастомный предмет,оставьте пустым)")]
        public string DisplayName;
        [JsonProperty("Shortname предмета")]
        public string Shortname;
        [JsonProperty("SkinID предмета")]
        public ulong SkinID;
        [JsonProperty("Количество для выдачи")]
        public int Amount;
    }

}

internal class ItemRewardSettings
{
    [JsonProperty("Отображаемое имя(если это не кастомный предмет,оставьте пустым)")]
    public string DisplayName;
    [JsonProperty("Shortname предмета")]
    public string Shortname;
    [JsonProperty("SkinID предмета")]
    public ulong SkinID;
    [JsonProperty("Количество для выдачи")]
    public int Amount;
}


```

---

## IQTeleportation-1.5.7

```csharp
using System.Collections.Generic;
using Oxide.Game.Rust.Cui;
using Physics = UnityEngine.Physics;
using System.Collections;
using System.Linq;
using UnityEngine.Networking;
using Pool = Facepunch.Pool;
using ConVar;
using System;
using ProtoBuf;
using UnityEngine;
using Oxide.Core.Plugins;
using Oxide.Core;
using System.Text;
using Newtonsoft.Json;
using Object = System.Object;
using Vector3 = UnityEngine.Vector3;

Oxide.Plugins
[Info("IQTeleportation", "Mercury", "1.5.7")]
[Description("IQTeleportation")]
 class IQTeleportation : RustPlugin
{
    private readonly List<String> exceptionsBuildingBlockEntities;
    private IEnumerator ProcessTWarp(BasePlayer player, Vector3 position, Int32 timeTeleportation);
    private readonly List<UInt64> ItemIdCorrecteds;
    private void ParseMonuments();
    private String CanSetHome(BasePlayer player, BuildingBlock buildingBlock, String homeName, Vector3 positionHome, Tugboat tugboatEntity);
    [ConsoleCommand("sethome")]
    private void SetHomeConsoleCMD(ConsoleSystem.Arg arg);
    private class ServerWarp
    {
        public Vector3 position;
        public String monumentParent;
        public Boolean hideWarp;
        public Vector3 GetPositionWarp();
        public ServerWarp(MonumentInfo monument, Vector3 warpPosition);
    }

    private Dictionary<UInt64, Dictionary<String, PlayerHome>> playerHomes;
    [ConsoleCommand("ui_teleportation_command")]
    private void UICommandConsole(ConsoleSystem.Arg arg);
    private Dictionary<BasePlayer, BasePlayer> casheTeleportationLast;
    private class TeleportationQueueInfo
    {
        public Boolean isAccepted;
        public Boolean isActive;
        public BasePlayer player;
    }

    [ChatCommand("sethome")]
    private void SetHomeChatCMD(BasePlayer player, String cmd, String[] arg);
    private class UserRepository
    {
        public Coroutine activeTeleportation;
        public Double cooldownWarp;
        public Double cooldownHome;
        public Double cooldownTeleportation;
        public void SetupCooldown(BasePlayer player, TeleportType teleportType);
        public String GetCooldownTitle(BasePlayer player, TeleportType teleportType);
        public Boolean IsActiveTeleportation();
        public void ActiveCoroutineController(Coroutine coroutine, BasePlayer target);
        public void DeActiveTeleportation();
    }

    [ConsoleCommand("rh")]
    private void RemoveHomeShortConsoleCMD(ConsoleSystem.Arg arg);
    private Dictionary<BasePlayer, MapNote> playerPings;
    [ChatCommand("home")]
    private void HomeChatCMD(BasePlayer player, String cmd, String[] arg);
    private Boolean AcceptRequestTeleportation(BasePlayer player, Boolean isAutoAccept);
    private const String permissionWarpAdmin;
    [ChatCommand("tp")]
    private void TpChatCMD(BasePlayer player, String cmd, String[] arg);
    private MonumentInfo GetMonumentWherePlayerStand(BasePlayer player);
    private List<MonumentInfo> localAllMonuments;
    private void ChatCommandWarp(BasePlayer player, String command, String[] args);
    [ChatCommand("gmap")]
    private void GMapTeleportChatCmd(BasePlayer player);
    private const String effectSoundTimer;
    private void CheckAllHomes(BasePlayer player, String homeName);
    private static ImageUI _imageUI;
    private Boolean IsPlayerWithinBlockedMonument(BasePlayer player);
    private void TeleportationWarp(BasePlayer player, Vector3 position);
    private Dictionary<String, List<String>> cachedUI;
    private void AutoClearData(Boolean isNewSave);
    private Boolean CheckHome(BasePlayer player, String homeName, PlayerHome pHome);
    [ConsoleCommand("homelist")]
    private void ConsoleCommand_HomeList(ConsoleSystem.Arg arg);
    private void OnServerInitialized();
    protected override void SaveConfig();
     void OnPlayerDisconnected(BasePlayer player, String reason);
    private const String tpaLog;
    private Single GetGroundPosition(Vector3 pos);
    [ConsoleCommand("sh")]
    private void SetHomeShortConsoleCMD(ConsoleSystem.Arg arg);
    private (BasePlayer, String) FindPlayerNameOrID(BasePlayer finder, String nameOrID, Boolean findSleeper);
    private void RunEffect(BasePlayer player, String effectPath);
    private static Double CurrentTime { get; set; }
    private String CanRequestTeleportation(BasePlayer player, BasePlayer targetPlayer, String error);
    private Object OnMapMarkerAdd(BasePlayer player, MapNote note);
    [ChatCommand("sh")]
    private void SetHomeShortChatCMD(BasePlayer player, String cmd, String[] arg);
    private Dictionary<String, Vector3> GetHomes(UInt64 userID);
    protected override void LoadConfig();
    private Dictionary<String, List<ServerWarp>> serverWarps;
    private void AutoTeleportTurn(BasePlayer player);
    private MonumentInfo GetMonumentInName(String nameMonument);
    private void OnEntityKill(Tugboat tugboat);
    private void LogAction(BasePlayer player, TypeLog typeLog, BasePlayer targetPlayer, Vector3 position, String homeName);
    private IEnumerator ProcessTTarget(BasePlayer player, BasePlayer targetPlayer, Int32 timeTeleportation);
    private const Single radiusCheckLayerHome;
    private void DestroyPing(BasePlayer player);
    private static void AddUI(BasePlayer player, String json);
    private void CreateMapMarkerPlayer(BasePlayer player, Vector3 position, String homeName);
    private Timer timerCheckHomes;
    private Dictionary<BasePlayer, UserRepository> localUsersRepository;
    private void Unload();
    private UInt64[] GetFriendList(BasePlayer targetPlayer);
    private void ReadData();
    private void WriteData();
    [ConsoleCommand("atp")]
    private void AutoAcceptTeleportConsoleCmd(ConsoleSystem.Arg arg);
    private class SettingPlayer
    {
        public Boolean useTeleportationHomeFromFriends;
        public Boolean autoAcceptTeleportationFriends;
        public Boolean useTeleportGMap;
        public DateTime firstConnection;
    }

    private const String effectSoundAccess;
    private void DrawUI_HomeSetup(BasePlayer player);
    private void ParseTugboats();
    private new void LoadDefaultMessages();
    private Boolean IsValidPosOtherEntity(Vector3 position, BuildingBlock buildingBlock);
    protected override void LoadDefaultConfig();
    [ConsoleCommand("tpa")]
    private void TpaConsoleCMD(ConsoleSystem.Arg arg);
    [ChatCommand("homelist")]
    private void ChatCommand_HomeList(BasePlayer player);
    private Dictionary<BasePlayer, SleepingBag> bagInstalled;
    [ChatCommand("mblock")]
    private void MBlockChatCMD(BasePlayer player);
    private void OnNewSave(String filename);
    [ChatCommand("removehome")]
    private void RemoveHomeChatCMD(BasePlayer player, String cmd, String[] arg);
    private IEnumerator ProcessTHome(BasePlayer player, PlayerHome pHome, Int32 timeTeleportation);
    private void RequestWarp(BasePlayer player, Vector3 warpPosition);
    private Dictionary<UInt64, AutoTurret> turretsEx;
    private void OnEntityBuilt(Planner plan, GameObject go);
    [ChatCommand("rh")]
    private void RemoveHomeShortChatCMD(BasePlayer player, String cmd, String[] arg);
    public Boolean IsRaidBlocked(BasePlayer player);
    private void CreatePingPlayer(BasePlayer player, Vector3 position);
    [ConsoleCommand("tpc")]
    private void CancellTeleportCmdConsole(ConsoleSystem.Arg arg);
    private class PlayerHome
    {
        public Vector3 positionHome;
        public UInt64 netIdEntity;
    }

    private void MovePlayer(BasePlayer player, Vector3 position, BaseEntity parentEntity, BaseEntity parentHomePosition);
    private void ClearHomesPlayer(UInt64 userID, String pluginName);
    private void RemoveHome(BasePlayer player, String homeName);
    private const Boolean LanguageEn;
    private void CancellTeleportation(BasePlayer player);
    [ChatCommand("tpc")]
    private void CancellTeleportCmdChat(BasePlayer player);
    public class Configuration
    {
        [JsonProperty(LanguageEn ? "" : "Основные настройки плагина")]
        public GeneralController generalController;
        [JsonProperty(LanguageEn ? "" : "Настройка телепортации к игроку")]
        public TeleportationController teleportationController;
        [JsonProperty(LanguageEn ? "" : "Настройка точек для дома")]
        public HomeController homeController;
        [JsonProperty(LanguageEn ? "" : "Настройка системы варпов")]
        public WarpsController warpsContoller;
        internal class WarpsController
        {
            [JsonProperty(LanguageEn ? "" : "Включить поддержку варпов (true - да/false - нет)")]
            public Boolean useWarps;
            [JsonProperty(LanguageEn ? "" : "Настройка времени перезарядки телепортации на варп")]
            public PresetOtherSetting teleportWarpCooldown;
            [JsonProperty(LanguageEn ? "" : "Настройка времени телепортации на варп")]
            public PresetOtherSetting teleportWarpCountdown;
        }

        internal class GeneralController
        {
            [JsonProperty(LanguageEn ? "" : "Использовать логирование действий игроков в файл (true - да/false - нет)")]
            public Boolean useLogged;
            [JsonProperty(LanguageEn ? "" : "Использовать звуковые эффекты (true - да/false - нет)")]
            public Boolean useSoundEffects;
            [JsonProperty(LanguageEn ? "" : "Использовать GameTip сообщения, вместо сообщений в чате (true - да/false - нет)")]
            public Boolean useOnlyGameTip;
            [JsonProperty(LanguageEn ? "" : "Поднимать игрока сразу после телепортации, иначе он будет в состоянии `сна` (true - да/false - нет)")]
            public Boolean stopSleepingAfterTeleport;
            [JsonProperty(LanguageEn ? "" : "Список монументов с которых запрещено телепортироваться (Распространяется на точки дома/телепортацию к игрокам/варпы)")]
            public List<String> monumentBlockedTeleportation;
            [JsonProperty(LanguageEn ? "" : "Настройки IQChat")]
            public IQChatController iqchatSetting;
            internal class IQChatController
            {
                [JsonProperty(LanguageEn ? "IQChat : Custom prefix in the chat" : "IQChat : Кастомный префикс в чате")]
                public String customPrefix;
                [JsonProperty(LanguageEn ? "IQChat : Custom avatar in the chat (Steam64ID) (If required)" : "IQChat : Кастомный аватар в чате (Steam64ID) (Если требуется)")]
                public String customAvatar;
            }

        }

        internal class TeleportationController
        {
            [JsonProperty(LanguageEn ? "" : "Настройка запросов на телепортацию")]
            public TeleportSetting teleportSetting;
            [JsonProperty(LanguageEn ? "" : "Настройка времени перезарядки телепортации к игроку")]
            public PresetOtherSetting teleportCooldown;
            [JsonProperty(LanguageEn ? "" : "Настройка времени телепортации к игроку")]
            public PresetOtherSetting teleportCountdown;
            internal class TeleportSetting
            {
                [JsonProperty(LanguageEn ? "" : "Предлагать принять запрос на телепортацию в UI (true - да/false - нет)")]
                public Boolean suggetionAcceptedUI;
                [JsonProperty(LanguageEn ? "" : "Режим телепортации, 1 (true) - телепортировать игрока на точку где был принят запрос, 2 (false) - телепортировать игрока к другому игроку не зависимо где был принят запрос")]
                public Boolean teleportPosInAccept;
                [JsonProperty(LanguageEn ? "" : "Разрешить игрокам отправлять запросы на телепортацию по G-Map (true - да/false - нет)")]
                public Boolean useGMapTeleport;
                [JsonProperty(LanguageEn ? "" : "Использовать моментальную телепортацию на точку через G-Map (true - да/false - нет) (требуются права администратора или разрешение iqteleportation.gmap)")]
                public Boolean useGMapTeleportAdmin;
                [JsonProperty(LanguageEn ? "" : "Отключить функции телепортации на сервере (tpr и tpa будут недоступны) (true - да/false - нет)")]
                public Boolean disableTeleportationPlayer;
                [JsonProperty(LanguageEn ? "" : "Запретить отправку и принятие запроса на телепортацию во время рейдблока (true - да/false - нет)")]
                public Boolean noTeleportInRaidBlock;
                [JsonProperty(LanguageEn ? "" : "Запретить отправку и принятие запроса на телепортацию когда игрок на корабле (true - да/false - нет)")]
                public Boolean noTeleportCargoShip;
                [JsonProperty(LanguageEn ? "" : "Запретить отправку и принятие запроса на телепортацию когда игрок на воздушном шаре (true - да/false - нет)")]
                public Boolean noTeleportHotAirBalloon;
                [JsonProperty(LanguageEn ? "" : "Запретить отправку и принятие запроса на телепортацию когда игроку холодно (true - да/false - нет)")]
                public Boolean noTeleportFrozen;
                [JsonProperty(LanguageEn ? "" : "Запретить отправку и принятие запроса на телепортацию когда игрок под радиацией (true - да/false - нет)")]
                public Boolean noTeleportRadiation;
                [JsonProperty(LanguageEn ? "" : "Запретить отправку и принятие запроса на телепортацию когда у игрока кровотечение(true - да/false - нет)")]
                public Boolean noTeleportBlood;
                [JsonProperty(LanguageEn ? "" : "Разрешать телепортацию только к друзьям (true - да/false - нет)")]
                public Boolean onlyTeleportationFriends;
            }

        }

        internal class HomeController
        {
            [JsonProperty(LanguageEn ? "" : "Предлагать установку точки дома в UI после установки кровати или спальника (true - да/false - нет)")]
            public Boolean suggetionSetHomeAfterInstallBed;
            [JsonProperty(LanguageEn ? "" : "Добавлять визуальный эффект (пинг) на точку дома после ее установки (true - да/false - нет)")]
            public Boolean addedPingMarker;
            [JsonProperty(LanguageEn ? "" : "Добавлять маркер на G-Map игрока после установки дома (true - да/false - нет)")]
            public Boolean addedMapMarkerHome;
            [JsonProperty(LanguageEn ? "" : "Настройка разрешений на установку точек дома")]
            public SetupHome setupHomeController;
            [JsonProperty(LanguageEn ? "" : "Настройка количества точек дома")]
            public PresetOtherSetting homeCount;
            [JsonProperty(LanguageEn ? "" : "Настройка времени перезарядки телепортации на точку дома")]
            public PresetOtherSetting homeCooldown;
            [JsonProperty(LanguageEn ? "" : "Настройка времени телепортации игрока на точку дома")]
            public PresetOtherSetting homeCountdown;
            internal class SetupHome
            {
                [JsonProperty(LanguageEn ? "" : "Запретить установку точки дома при рейдблоке (true - да/false - нет)")]
                public Boolean noSetupInRaidBlock;
                [JsonProperty(LanguageEn ? "" : "Разрешить установку точек дома на буксирах (true - да/false - нет)")]
                public Boolean canSetupTugboatHome;
                [JsonProperty(LanguageEn ? "" : "Разрешать установку точек дома только при наличии билдинг зоны (true - да/false - нет)")]
                public Boolean onlyBuildingPrivilage;
                [JsonProperty(LanguageEn ? "" : "Разрешать установку точек дома только при наличии авторизации в билдинг зоне (true - да/false - нет)")]
                public Boolean onlyBuildingAuth;
            }

        }

        internal class PresetOtherSetting
        {
            [JsonProperty(LanguageEn ? "" : "Стандартное количество для игроков без разрешений")]
            public Int32 count;
            [JsonProperty(LanguageEn ? "" : "Настройка количества для игроков с разрешениями [Разрешение] = Количество")]
            public Dictionary<String, Int32> countPermissions;
            public Int32 GetCount(BasePlayer player, Boolean descending);
        }

        public static Configuration GetNewConfiguration();
    }

    private class ImageUI
    {
        private const String _path;
        private const String _printPath;
        private readonly Dictionary<String, ImageData> _images;
        private class ImageData
        {
            public ImageStatus Status;
            public String Id { get; set; }
        }

        public String GetImage(String name);
        public void DownloadImage();
        public void UnloadImages();
        private IEnumerator ProcessDownloadImage(KeyValuePair<String, ImageData> image);
    }

    private static void DrawSphereAndText(BasePlayer player, Vector3 position, String text, Single time);
    private Dictionary<UInt64, SettingPlayer> playerSettings;
    [ChatCommand("tpa")]
    private void TpaChatCMD(BasePlayer player, String cmd, String[] arg);
    private void GenerateWarp();
    private Boolean IsFriends(BasePlayer player, UInt64 targetID);
    private static void DestroyUI(BasePlayer player, String elementName);
    private void OnPlayerConnected(BasePlayer player);
    private const String tprLog;
    [PluginReference]
     Plugin IQChat;
     Plugin Clans;
     Plugin Friends;
    private String CanRequestTeleportHome(BasePlayer player, String homeName, String friendNameOrID, UInt64 userIDFriend, String friendName);
    private Dictionary<BasePlayer, Action> teleportationActions;
    private void RegisteredPermissions();
    private void Init();
    private readonly LayerMask homelayerMaskEntity;
    private void ValidateWarps();
    private const Single uiAlertFade;
    private String GetWarpPoints(String warpName);
    private void DrawUI_TeleportPlayer(BasePlayer player, BasePlayer targetPlayer);
    [ChatCommand("tpr")]
    private void TprChatCMD(BasePlayer player, String cmd, String[] arg);
    private String CheckStatusPlayer(BasePlayer player, Boolean canSethome);
    private void GMapTeleportTurn(BasePlayer player);
    private void ConsoleCommandTeleportationWarp(ConsoleSystem.Arg arg);
    private void RegisteredUser(BasePlayer player);
    [ConsoleCommand("tpr")]
    private void TprConsoleCMD(ConsoleSystem.Arg arg);
    private static StringBuilder sb;
    [ChatCommand("a.home")]
    private void ChatCommandAdminHome(BasePlayer player, String cmd, String[] arg);
    [ConsoleCommand("tp")]
    private void TpConsoleCMD(ConsoleSystem.Arg arg);
    private const String permissionGMapTeleport;
    [ConsoleCommand("gmap")]
    private void GMapTeleportConsoleCmd(ConsoleSystem.Arg arg);
    private String CanTeleportation(BasePlayer player, UserRepository.TeleportType type, Boolean isSkipCooldown);
    [ConsoleCommand("removehome")]
    private void RemoveHomeConsoleCMD(ConsoleSystem.Arg arg);
    private String FormatTime(Double seconds, String userID);
    private void TeleportationPlayer(BasePlayer player, BasePlayer targetPlayer, Vector3 teleportationPosition);
    private class InterfaceBuilder
    {
        public static InterfaceBuilder Instance;
        public const String IQ_TELEPORT_UI_HOME;
        public const String IQ_TELEPORT_UI_TELEPORT;
        public Dictionary<String, String> Interfaces;
        public InterfaceBuilder();
        public static void AddInterface(String name, String json);
        public static String GetInterface(String name);
        public static void DestroyAll();
        private void Building_PanelButton(String templateParent);
    }

    private static Configuration config;
    private const String effectSoundError;
    private void TeleportationHome(BasePlayer player, Vector3 position, BaseEntity pEntity);
    private readonly LayerMask homelayerMaskBuilding;
    private const String effectSoundTeleport;
    private String Format(Int32 units, String form1, String form2, String form3);
    private void ChatCommandTeleportationWarp(BasePlayer player, String cmd, String[] arg);
    private const Single checkIntervalPlayerTeleportation;
    public String GetLang(String LangKey, String userID, Object[] args);
    private Dictionary<UInt64, TeleportationQueueInfo> playerTeleportationQueue;
    private void ClearQueue(BasePlayer player, BasePlayer targetPlayer);
    private static InterfaceBuilder _interface;
    private void OnEntitySpawned(Tugboat tugboat);
    private BuildingBlock GetBuildingBlock(Vector3 position);
    private Dictionary<UInt64, Tugboat> tugboatsServer;
    private const String permissionTP;
    [ConsoleCommand("home")]
    private void HomeConsoleCMD(ConsoleSystem.Arg arg);
    private IEnumerator ProcessTeleportation(BasePlayer player, Func<Boolean> additionalChecks, Action teleportationAction, Int32 timeTeleportation, BasePlayer targetPlayer);
    private void SetHome(BasePlayer player, String homeName, SleepingBag bag);
    private void SendChat(String message, BasePlayer player, Boolean isError);
    private void RequestHome(BasePlayer player, String[] arg);
    private String GetNextHomeName(Dictionary<String, PlayerHome> pData);
    private List<String> GetOrSetCacheUI(BasePlayer player, String additionalTitile, String interfaceJson);
    private ServerWarp GetRandomWarp(List<ServerWarp> warpList);
    private static IQTeleportation _;
    private void RequestTeleportation(BasePlayer player, String[] arg);
    private const String homeLog;
    private void OnEntitySpawneds(AutoTurret turret);
    [ChatCommand("atp")]
    private void AutoAcceptTeleportChatCmd(BasePlayer player);
}

private class ServerWarp
{
    public Vector3 position;
    public String monumentParent;
    public Boolean hideWarp;
    public Vector3 GetPositionWarp();
    public ServerWarp(MonumentInfo monument, Vector3 warpPosition);
}

private class TeleportationQueueInfo
{
    public Boolean isAccepted;
    public Boolean isActive;
    public BasePlayer player;
}

private class UserRepository
{
    public Coroutine activeTeleportation;
    public Double cooldownWarp;
    public Double cooldownHome;
    public Double cooldownTeleportation;
    public void SetupCooldown(BasePlayer player, TeleportType teleportType);
    public String GetCooldownTitle(BasePlayer player, TeleportType teleportType);
    public Boolean IsActiveTeleportation();
    public void ActiveCoroutineController(Coroutine coroutine, BasePlayer target);
    public void DeActiveTeleportation();
}

private class SettingPlayer
{
    public Boolean useTeleportationHomeFromFriends;
    public Boolean autoAcceptTeleportationFriends;
    public Boolean useTeleportGMap;
    public DateTime firstConnection;
}

private class PlayerHome
{
    public Vector3 positionHome;
    public UInt64 netIdEntity;
}

public class Configuration
{
    [JsonProperty(LanguageEn ? "" : "Основные настройки плагина")]
    public GeneralController generalController;
    [JsonProperty(LanguageEn ? "" : "Настройка телепортации к игроку")]
    public TeleportationController teleportationController;
    [JsonProperty(LanguageEn ? "" : "Настройка точек для дома")]
    public HomeController homeController;
    [JsonProperty(LanguageEn ? "" : "Настройка системы варпов")]
    public WarpsController warpsContoller;
    internal class WarpsController
    {
        [JsonProperty(LanguageEn ? "" : "Включить поддержку варпов (true - да/false - нет)")]
        public Boolean useWarps;
        [JsonProperty(LanguageEn ? "" : "Настройка времени перезарядки телепортации на варп")]
        public PresetOtherSetting teleportWarpCooldown;
        [JsonProperty(LanguageEn ? "" : "Настройка времени телепортации на варп")]
        public PresetOtherSetting teleportWarpCountdown;
    }

    internal class GeneralController
    {
        [JsonProperty(LanguageEn ? "" : "Использовать логирование действий игроков в файл (true - да/false - нет)")]
        public Boolean useLogged;
        [JsonProperty(LanguageEn ? "" : "Использовать звуковые эффекты (true - да/false - нет)")]
        public Boolean useSoundEffects;
        [JsonProperty(LanguageEn ? "" : "Использовать GameTip сообщения, вместо сообщений в чате (true - да/false - нет)")]
        public Boolean useOnlyGameTip;
        [JsonProperty(LanguageEn ? "" : "Поднимать игрока сразу после телепортации, иначе он будет в состоянии `сна` (true - да/false - нет)")]
        public Boolean stopSleepingAfterTeleport;
        [JsonProperty(LanguageEn ? "" : "Список монументов с которых запрещено телепортироваться (Распространяется на точки дома/телепортацию к игрокам/варпы)")]
        public List<String> monumentBlockedTeleportation;
        [JsonProperty(LanguageEn ? "" : "Настройки IQChat")]
        public IQChatController iqchatSetting;
        internal class IQChatController
        {
            [JsonProperty(LanguageEn ? "IQChat : Custom prefix in the chat" : "IQChat : Кастомный префикс в чате")]
            public String customPrefix;
            [JsonProperty(LanguageEn ? "IQChat : Custom avatar in the chat (Steam64ID) (If required)" : "IQChat : Кастомный аватар в чате (Steam64ID) (Если требуется)")]
            public String customAvatar;
        }

    }

    internal class TeleportationController
    {
        [JsonProperty(LanguageEn ? "" : "Настройка запросов на телепортацию")]
        public TeleportSetting teleportSetting;
        [JsonProperty(LanguageEn ? "" : "Настройка времени перезарядки телепортации к игроку")]
        public PresetOtherSetting teleportCooldown;
        [JsonProperty(LanguageEn ? "" : "Настройка времени телепортации к игроку")]
        public PresetOtherSetting teleportCountdown;
        internal class TeleportSetting
        {
            [JsonProperty(LanguageEn ? "" : "Предлагать принять запрос на телепортацию в UI (true - да/false - нет)")]
            public Boolean suggetionAcceptedUI;
            [JsonProperty(LanguageEn ? "" : "Режим телепортации, 1 (true) - телепортировать игрока на точку где был принят запрос, 2 (false) - телепортировать игрока к другому игроку не зависимо где был принят запрос")]
            public Boolean teleportPosInAccept;
            [JsonProperty(LanguageEn ? "" : "Разрешить игрокам отправлять запросы на телепортацию по G-Map (true - да/false - нет)")]
            public Boolean useGMapTeleport;
            [JsonProperty(LanguageEn ? "" : "Использовать моментальную телепортацию на точку через G-Map (true - да/false - нет) (требуются права администратора или разрешение iqteleportation.gmap)")]
            public Boolean useGMapTeleportAdmin;
            [JsonProperty(LanguageEn ? "" : "Отключить функции телепортации на сервере (tpr и tpa будут недоступны) (true - да/false - нет)")]
            public Boolean disableTeleportationPlayer;
            [JsonProperty(LanguageEn ? "" : "Запретить отправку и принятие запроса на телепортацию во время рейдблока (true - да/false - нет)")]
            public Boolean noTeleportInRaidBlock;
            [JsonProperty(LanguageEn ? "" : "Запретить отправку и принятие запроса на телепортацию когда игрок на корабле (true - да/false - нет)")]
            public Boolean noTeleportCargoShip;
            [JsonProperty(LanguageEn ? "" : "Запретить отправку и принятие запроса на телепортацию когда игрок на воздушном шаре (true - да/false - нет)")]
            public Boolean noTeleportHotAirBalloon;
            [JsonProperty(LanguageEn ? "" : "Запретить отправку и принятие запроса на телепортацию когда игроку холодно (true - да/false - нет)")]
            public Boolean noTeleportFrozen;
            [JsonProperty(LanguageEn ? "" : "Запретить отправку и принятие запроса на телепортацию когда игрок под радиацией (true - да/false - нет)")]
            public Boolean noTeleportRadiation;
            [JsonProperty(LanguageEn ? "" : "Запретить отправку и принятие запроса на телепортацию когда у игрока кровотечение(true - да/false - нет)")]
            public Boolean noTeleportBlood;
            [JsonProperty(LanguageEn ? "" : "Разрешать телепортацию только к друзьям (true - да/false - нет)")]
            public Boolean onlyTeleportationFriends;
        }

    }

    internal class HomeController
    {
        [JsonProperty(LanguageEn ? "" : "Предлагать установку точки дома в UI после установки кровати или спальника (true - да/false - нет)")]
        public Boolean suggetionSetHomeAfterInstallBed;
        [JsonProperty(LanguageEn ? "" : "Добавлять визуальный эффект (пинг) на точку дома после ее установки (true - да/false - нет)")]
        public Boolean addedPingMarker;
        [JsonProperty(LanguageEn ? "" : "Добавлять маркер на G-Map игрока после установки дома (true - да/false - нет)")]
        public Boolean addedMapMarkerHome;
        [JsonProperty(LanguageEn ? "" : "Настройка разрешений на установку точек дома")]
        public SetupHome setupHomeController;
        [JsonProperty(LanguageEn ? "" : "Настройка количества точек дома")]
        public PresetOtherSetting homeCount;
        [JsonProperty(LanguageEn ? "" : "Настройка времени перезарядки телепортации на точку дома")]
        public PresetOtherSetting homeCooldown;
        [JsonProperty(LanguageEn ? "" : "Настройка времени телепортации игрока на точку дома")]
        public PresetOtherSetting homeCountdown;
        internal class SetupHome
        {
            [JsonProperty(LanguageEn ? "" : "Запретить установку точки дома при рейдблоке (true - да/false - нет)")]
            public Boolean noSetupInRaidBlock;
            [JsonProperty(LanguageEn ? "" : "Разрешить установку точек дома на буксирах (true - да/false - нет)")]
            public Boolean canSetupTugboatHome;
            [JsonProperty(LanguageEn ? "" : "Разрешать установку точек дома только при наличии билдинг зоны (true - да/false - нет)")]
            public Boolean onlyBuildingPrivilage;
            [JsonProperty(LanguageEn ? "" : "Разрешать установку точек дома только при наличии авторизации в билдинг зоне (true - да/false - нет)")]
            public Boolean onlyBuildingAuth;
        }

    }

    internal class PresetOtherSetting
    {
        [JsonProperty(LanguageEn ? "" : "Стандартное количество для игроков без разрешений")]
        public Int32 count;
        [JsonProperty(LanguageEn ? "" : "Настройка количества для игроков с разрешениями [Разрешение] = Количество")]
        public Dictionary<String, Int32> countPermissions;
        public Int32 GetCount(BasePlayer player, Boolean descending);
    }

    public static Configuration GetNewConfiguration();
}

internal class WarpsController
{
    [JsonProperty(LanguageEn ? "" : "Включить поддержку варпов (true - да/false - нет)")]
    public Boolean useWarps;
    [JsonProperty(LanguageEn ? "" : "Настройка времени перезарядки телепортации на варп")]
    public PresetOtherSetting teleportWarpCooldown;
    [JsonProperty(LanguageEn ? "" : "Настройка времени телепортации на варп")]
    public PresetOtherSetting teleportWarpCountdown;
}

internal class GeneralController
{
    [JsonProperty(LanguageEn ? "" : "Использовать логирование действий игроков в файл (true - да/false - нет)")]
    public Boolean useLogged;
    [JsonProperty(LanguageEn ? "" : "Использовать звуковые эффекты (true - да/false - нет)")]
    public Boolean useSoundEffects;
    [JsonProperty(LanguageEn ? "" : "Использовать GameTip сообщения, вместо сообщений в чате (true - да/false - нет)")]
    public Boolean useOnlyGameTip;
    [JsonProperty(LanguageEn ? "" : "Поднимать игрока сразу после телепортации, иначе он будет в состоянии `сна` (true - да/false - нет)")]
    public Boolean stopSleepingAfterTeleport;
    [JsonProperty(LanguageEn ? "" : "Список монументов с которых запрещено телепортироваться (Распространяется на точки дома/телепортацию к игрокам/варпы)")]
    public List<String> monumentBlockedTeleportation;
    [JsonProperty(LanguageEn ? "" : "Настройки IQChat")]
    public IQChatController iqchatSetting;
    internal class IQChatController
    {
        [JsonProperty(LanguageEn ? "IQChat : Custom prefix in the chat" : "IQChat : Кастомный префикс в чате")]
        public String customPrefix;
        [JsonProperty(LanguageEn ? "IQChat : Custom avatar in the chat (Steam64ID) (If required)" : "IQChat : Кастомный аватар в чате (Steam64ID) (Если требуется)")]
        public String customAvatar;
    }

}

internal class IQChatController
{
    [JsonProperty(LanguageEn ? "IQChat : Custom prefix in the chat" : "IQChat : Кастомный префикс в чате")]
    public String customPrefix;
    [JsonProperty(LanguageEn ? "IQChat : Custom avatar in the chat (Steam64ID) (If required)" : "IQChat : Кастомный аватар в чате (Steam64ID) (Если требуется)")]
    public String customAvatar;
}

internal class TeleportationController
{
    [JsonProperty(LanguageEn ? "" : "Настройка запросов на телепортацию")]
    public TeleportSetting teleportSetting;
    [JsonProperty(LanguageEn ? "" : "Настройка времени перезарядки телепортации к игроку")]
    public PresetOtherSetting teleportCooldown;
    [JsonProperty(LanguageEn ? "" : "Настройка времени телепортации к игроку")]
    public PresetOtherSetting teleportCountdown;
    internal class TeleportSetting
    {
        [JsonProperty(LanguageEn ? "" : "Предлагать принять запрос на телепортацию в UI (true - да/false - нет)")]
        public Boolean suggetionAcceptedUI;
        [JsonProperty(LanguageEn ? "" : "Режим телепортации, 1 (true) - телепортировать игрока на точку где был принят запрос, 2 (false) - телепортировать игрока к другому игроку не зависимо где был принят запрос")]
        public Boolean teleportPosInAccept;
        [JsonProperty(LanguageEn ? "" : "Разрешить игрокам отправлять запросы на телепортацию по G-Map (true - да/false - нет)")]
        public Boolean useGMapTeleport;
        [JsonProperty(LanguageEn ? "" : "Использовать моментальную телепортацию на точку через G-Map (true - да/false - нет) (требуются права администратора или разрешение iqteleportation.gmap)")]
        public Boolean useGMapTeleportAdmin;
        [JsonProperty(LanguageEn ? "" : "Отключить функции телепортации на сервере (tpr и tpa будут недоступны) (true - да/false - нет)")]
        public Boolean disableTeleportationPlayer;
        [JsonProperty(LanguageEn ? "" : "Запретить отправку и принятие запроса на телепортацию во время рейдблока (true - да/false - нет)")]
        public Boolean noTeleportInRaidBlock;
        [JsonProperty(LanguageEn ? "" : "Запретить отправку и принятие запроса на телепортацию когда игрок на корабле (true - да/false - нет)")]
        public Boolean noTeleportCargoShip;
        [JsonProperty(LanguageEn ? "" : "Запретить отправку и принятие запроса на телепортацию когда игрок на воздушном шаре (true - да/false - нет)")]
        public Boolean noTeleportHotAirBalloon;
        [JsonProperty(LanguageEn ? "" : "Запретить отправку и принятие запроса на телепортацию когда игроку холодно (true - да/false - нет)")]
        public Boolean noTeleportFrozen;
        [JsonProperty(LanguageEn ? "" : "Запретить отправку и принятие запроса на телепортацию когда игрок под радиацией (true - да/false - нет)")]
        public Boolean noTeleportRadiation;
        [JsonProperty(LanguageEn ? "" : "Запретить отправку и принятие запроса на телепортацию когда у игрока кровотечение(true - да/false - нет)")]
        public Boolean noTeleportBlood;
        [JsonProperty(LanguageEn ? "" : "Разрешать телепортацию только к друзьям (true - да/false - нет)")]
        public Boolean onlyTeleportationFriends;
    }

}

internal class TeleportSetting
{
    [JsonProperty(LanguageEn ? "" : "Предлагать принять запрос на телепортацию в UI (true - да/false - нет)")]
    public Boolean suggetionAcceptedUI;
    [JsonProperty(LanguageEn ? "" : "Режим телепортации, 1 (true) - телепортировать игрока на точку где был принят запрос, 2 (false) - телепортировать игрока к другому игроку не зависимо где был принят запрос")]
    public Boolean teleportPosInAccept;
    [JsonProperty(LanguageEn ? "" : "Разрешить игрокам отправлять запросы на телепортацию по G-Map (true - да/false - нет)")]
    public Boolean useGMapTeleport;
    [JsonProperty(LanguageEn ? "" : "Использовать моментальную телепортацию на точку через G-Map (true - да/false - нет) (требуются права администратора или разрешение iqteleportation.gmap)")]
    public Boolean useGMapTeleportAdmin;
    [JsonProperty(LanguageEn ? "" : "Отключить функции телепортации на сервере (tpr и tpa будут недоступны) (true - да/false - нет)")]
    public Boolean disableTeleportationPlayer;
    [JsonProperty(LanguageEn ? "" : "Запретить отправку и принятие запроса на телепортацию во время рейдблока (true - да/false - нет)")]
    public Boolean noTeleportInRaidBlock;
    [JsonProperty(LanguageEn ? "" : "Запретить отправку и принятие запроса на телепортацию когда игрок на корабле (true - да/false - нет)")]
    public Boolean noTeleportCargoShip;
    [JsonProperty(LanguageEn ? "" : "Запретить отправку и принятие запроса на телепортацию когда игрок на воздушном шаре (true - да/false - нет)")]
    public Boolean noTeleportHotAirBalloon;
    [JsonProperty(LanguageEn ? "" : "Запретить отправку и принятие запроса на телепортацию когда игроку холодно (true - да/false - нет)")]
    public Boolean noTeleportFrozen;
    [JsonProperty(LanguageEn ? "" : "Запретить отправку и принятие запроса на телепортацию когда игрок под радиацией (true - да/false - нет)")]
    public Boolean noTeleportRadiation;
    [JsonProperty(LanguageEn ? "" : "Запретить отправку и принятие запроса на телепортацию когда у игрока кровотечение(true - да/false - нет)")]
    public Boolean noTeleportBlood;
    [JsonProperty(LanguageEn ? "" : "Разрешать телепортацию только к друзьям (true - да/false - нет)")]
    public Boolean onlyTeleportationFriends;
}

internal class HomeController
{
    [JsonProperty(LanguageEn ? "" : "Предлагать установку точки дома в UI после установки кровати или спальника (true - да/false - нет)")]
    public Boolean suggetionSetHomeAfterInstallBed;
    [JsonProperty(LanguageEn ? "" : "Добавлять визуальный эффект (пинг) на точку дома после ее установки (true - да/false - нет)")]
    public Boolean addedPingMarker;
    [JsonProperty(LanguageEn ? "" : "Добавлять маркер на G-Map игрока после установки дома (true - да/false - нет)")]
    public Boolean addedMapMarkerHome;
    [JsonProperty(LanguageEn ? "" : "Настройка разрешений на установку точек дома")]
    public SetupHome setupHomeController;
    [JsonProperty(LanguageEn ? "" : "Настройка количества точек дома")]
    public PresetOtherSetting homeCount;
    [JsonProperty(LanguageEn ? "" : "Настройка времени перезарядки телепортации на точку дома")]
    public PresetOtherSetting homeCooldown;
    [JsonProperty(LanguageEn ? "" : "Настройка времени телепортации игрока на точку дома")]
    public PresetOtherSetting homeCountdown;
    internal class SetupHome
    {
        [JsonProperty(LanguageEn ? "" : "Запретить установку точки дома при рейдблоке (true - да/false - нет)")]
        public Boolean noSetupInRaidBlock;
        [JsonProperty(LanguageEn ? "" : "Разрешить установку точек дома на буксирах (true - да/false - нет)")]
        public Boolean canSetupTugboatHome;
        [JsonProperty(LanguageEn ? "" : "Разрешать установку точек дома только при наличии билдинг зоны (true - да/false - нет)")]
        public Boolean onlyBuildingPrivilage;
        [JsonProperty(LanguageEn ? "" : "Разрешать установку точек дома только при наличии авторизации в билдинг зоне (true - да/false - нет)")]
        public Boolean onlyBuildingAuth;
    }

}

internal class SetupHome
{
    [JsonProperty(LanguageEn ? "" : "Запретить установку точки дома при рейдблоке (true - да/false - нет)")]
    public Boolean noSetupInRaidBlock;
    [JsonProperty(LanguageEn ? "" : "Разрешить установку точек дома на буксирах (true - да/false - нет)")]
    public Boolean canSetupTugboatHome;
    [JsonProperty(LanguageEn ? "" : "Разрешать установку точек дома только при наличии билдинг зоны (true - да/false - нет)")]
    public Boolean onlyBuildingPrivilage;
    [JsonProperty(LanguageEn ? "" : "Разрешать установку точек дома только при наличии авторизации в билдинг зоне (true - да/false - нет)")]
    public Boolean onlyBuildingAuth;
}

internal class PresetOtherSetting
{
    [JsonProperty(LanguageEn ? "" : "Стандартное количество для игроков без разрешений")]
    public Int32 count;
    [JsonProperty(LanguageEn ? "" : "Настройка количества для игроков с разрешениями [Разрешение] = Количество")]
    public Dictionary<String, Int32> countPermissions;
    public Int32 GetCount(BasePlayer player, Boolean descending);
}

private class ImageUI
{
    private const String _path;
    private const String _printPath;
    private readonly Dictionary<String, ImageData> _images;
    private class ImageData
    {
        public ImageStatus Status;
        public String Id { get; set; }
    }

    public String GetImage(String name);
    public void DownloadImage();
    public void UnloadImages();
    private IEnumerator ProcessDownloadImage(KeyValuePair<String, ImageData> image);
}

private class ImageData
{
    public ImageStatus Status;
    public String Id { get; set; }
}

private class InterfaceBuilder
{
    public static InterfaceBuilder Instance;
    public const String IQ_TELEPORT_UI_HOME;
    public const String IQ_TELEPORT_UI_TELEPORT;
    public Dictionary<String, String> Interfaces;
    public InterfaceBuilder();
    public static void AddInterface(String name, String json);
    public static String GetInterface(String name);
    public static void DestroyAll();
    private void Building_PanelButton(String templateParent);
}


```

---

## IQTurret

```csharp
using System.Diagnostics;
using UnityEngine;
using Oxide.Core;
using Oxide.Core.Plugins;
using System.Linq;
using System.Threading;
using System;
using System.Text;
using Newtonsoft.Json;
using System.Collections.Generic;

Oxide.Plugins
[Info("IQTurret", "Sempai#3239", "1.3.7")]
[Description("Турели без электричества с лимитами на игрока/шкаф")]
internal class IQTurret : RustPlugin
{
     void OnEntitySpawned(AutoTurret turret);
    private Dictionary<BaseEntity, ElectricSwitch> GetPlayerTurretAndSwitch(BasePlayer player);
     void OnEntitySpawned(SamSite samSite);
    private ProtectionProperties ImmortalProtection;
    private Boolean IsTurretElectricalTurned(ElectricSwitch Switch);
     object OnWireClear(BasePlayer player, IOEntity entity1, int connected, IOEntity entity2, bool flag);
     void OnServerShutdown();
    public Boolean IsRaidBlocked(BasePlayer player);
    private const Boolean LanguageEn;
    protected override void LoadConfig();
    private void RegisteredTurret(UInt64 ID, UInt64 PlayerID, ElectricSwitch smartSwitch, BaseEntity turrel, BasePlayer player, Boolean IsInit);
    private readonly List<UInt64> IDsPrefabs;
    internal class ControllerInformation
    {
        public BaseEntity turrel;
        public ElectricSwitch electricSwitch;
        public UInt64 PlayerID;
        public UInt64 BuildingID;
    }

    private readonly String PermissionTurnAllTurretsOff;
     void OnEntityKill(SamSite samSite);
    private Dictionary<UInt64, List<ControllerInformation>> TurretList;
     void Init();
    public void SendChat(String Message, BasePlayer player, ConVar.Chat.ChatChannel channel);
    [PluginReference]
    private Plugin IQChat;
    public string GetLang(String LangKey, String userID, object[] args);
    private void RemoveSwitch(BaseEntity entity);
    private readonly String PermissionTurnAllTurretsOn;
    [ConsoleCommand("t")]
     void TurretControllConsoleCommand(ConsoleSystem.Arg arg);
    private void UnloadPlugin();
    [ChatCommand("t")]
     void TurretControllChatCommand(BasePlayer player, String cmd, String[] arg);
     bool CanPickupEntity(BasePlayer player, AutoTurret turret);
    private ElectricSwitch GetSwitchForTurret(BaseEntity Turret);
    private Boolean IsLimitPlayer(UInt64 playerID, UInt64 ID);
    private void TurretToggle(BasePlayer player, ElectricSwitch electricSwitch);
    private BaseEntity GetTurretForSwitch(ElectricSwitch Switch);
    private class Configuration
    {
        internal class LimitControll
        {
            [JsonProperty(LanguageEn ? "The limit of turrets WITHOUT electricity by privileges [Permission] = Limit (Make a list from more to less)" : "Лимит турелей БЕЗ электричества по привилегиям [Права] = Лимит (Составляйте список от большего - к меньшему)")]
            public Dictionary<String, Int32> PermissionsLimits;
            [JsonProperty(LanguageEn ? "Use the limit on turrets WITHOUT electricity? (true - yes/false - no)" : "Использовать лимит на туррели БЕЗ электричества? (true - да/false - нет)")]
            public Boolean UseLimitControll;
            [JsonProperty(LanguageEn ? "Limit Type: 0 - Player, 1 - Building" : "Тип лимита : 0 - На игрока, 1 - На шкаф")]
            public TypeLimiter typeLimiter;
            [JsonProperty(LanguageEn ? "Limit turrets WITHOUT electricity (If the player does not have privileges)" : "Лимит турелей БЕЗ электричества (Если у игрока нет привилегий)")]
            public Int32 LimitAmount;
        }

        [JsonProperty(LanguageEn ? "Setting limits on turrets WITHOUT electricity" : "Настройка лимитов на турели БЕЗ электричества")]
        public LimitControll LimitController;
        internal class ReferenceSettings
        {
            [JsonProperty(LanguageEn ? "Setting up collaboration with IQChat" : "Настройка совместной работы с IQChat")]
            public IQChatPlugin IQChatSetting;
            internal class IQChatPlugin
            {
                [JsonProperty(LanguageEn ? "IQChat :Custom prefix in the chat" : "IQChat : Кастомный префикс в чате")]
                public String CustomPrefix;
                [JsonProperty(LanguageEn ? "IQChat : Custom avatar in the chat(If required)" : "IQChat : Кастомный аватар в чате(Если требуется)")]
                public String CustomAvatar;
                [JsonProperty(LanguageEn ? "IQChat : Use UI notifications" : "IQChat : Использовать UI-уведомления")]
                public Boolean UIAlertUse;
            }

            [JsonProperty(LanguageEn ? "Prohibit the use of a switch during a raidBlock?" : "Запретить использовать рубильник во время рейдблока?")]
            public Boolean BlockedTumblerRaidblock;
        }

        [JsonProperty(LanguageEn ? "Configuring plugins for Collaboration" : "Настройка плагинов для совместной работы")]
        public ReferenceSettings ReferencesPlugin;
        public static Configuration GetNewConfiguration();
    }

    private Int32 GetAmountTurretPlayer(UInt64 playerID, UInt64 ID);
    private void OnEntityTakeDamage(BaseCombatEntity entity, HitInfo hitInfo);
    private static Configuration config;
    private static StringBuilder sb;
     void Unload();
    protected override void SaveConfig();
    private Int32 GetLimitPlayer(UInt64 PlayerID);
    private void InitializeData();
    private void SetupTurret(BaseEntity turrel, Boolean IsInit);
     object OnWireConnect(BasePlayer player, IOEntity entity1, int inputs, IOEntity entity2, int outputs);
     void OnEntityKill(AutoTurret turret);
     BaseEntity API_GET_TURRET(BasePlayer player, ElectricSwitch electricSwitch);
    private new void LoadDefaultMessages();
     Boolean API_IS_TURRETLIST(BaseEntity entity);
     ElectricSwitch API_GET_SWITCH(BasePlayer player, BaseEntity turret);
     object OnSwitchToggle(IOEntity entity, BasePlayer player);
     void OnServerInitialized();
    protected override void LoadDefaultConfig();
     Boolean API_IS_TURRETLIST(UInt64 ID);
    private const String SwitchPrefab;
}

internal class ControllerInformation
{
    public BaseEntity turrel;
    public ElectricSwitch electricSwitch;
    public UInt64 PlayerID;
    public UInt64 BuildingID;
}

private class Configuration
{
    internal class LimitControll
    {
        [JsonProperty(LanguageEn ? "The limit of turrets WITHOUT electricity by privileges [Permission] = Limit (Make a list from more to less)" : "Лимит турелей БЕЗ электричества по привилегиям [Права] = Лимит (Составляйте список от большего - к меньшему)")]
        public Dictionary<String, Int32> PermissionsLimits;
        [JsonProperty(LanguageEn ? "Use the limit on turrets WITHOUT electricity? (true - yes/false - no)" : "Использовать лимит на туррели БЕЗ электричества? (true - да/false - нет)")]
        public Boolean UseLimitControll;
        [JsonProperty(LanguageEn ? "Limit Type: 0 - Player, 1 - Building" : "Тип лимита : 0 - На игрока, 1 - На шкаф")]
        public TypeLimiter typeLimiter;
        [JsonProperty(LanguageEn ? "Limit turrets WITHOUT electricity (If the player does not have privileges)" : "Лимит турелей БЕЗ электричества (Если у игрока нет привилегий)")]
        public Int32 LimitAmount;
    }

    [JsonProperty(LanguageEn ? "Setting limits on turrets WITHOUT electricity" : "Настройка лимитов на турели БЕЗ электричества")]
    public LimitControll LimitController;
    internal class ReferenceSettings
    {
        [JsonProperty(LanguageEn ? "Setting up collaboration with IQChat" : "Настройка совместной работы с IQChat")]
        public IQChatPlugin IQChatSetting;
        internal class IQChatPlugin
        {
            [JsonProperty(LanguageEn ? "IQChat :Custom prefix in the chat" : "IQChat : Кастомный префикс в чате")]
            public String CustomPrefix;
            [JsonProperty(LanguageEn ? "IQChat : Custom avatar in the chat(If required)" : "IQChat : Кастомный аватар в чате(Если требуется)")]
            public String CustomAvatar;
            [JsonProperty(LanguageEn ? "IQChat : Use UI notifications" : "IQChat : Использовать UI-уведомления")]
            public Boolean UIAlertUse;
        }

        [JsonProperty(LanguageEn ? "Prohibit the use of a switch during a raidBlock?" : "Запретить использовать рубильник во время рейдблока?")]
        public Boolean BlockedTumblerRaidblock;
    }

    [JsonProperty(LanguageEn ? "Configuring plugins for Collaboration" : "Настройка плагинов для совместной работы")]
    public ReferenceSettings ReferencesPlugin;
    public static Configuration GetNewConfiguration();
}

internal class LimitControll
{
    [JsonProperty(LanguageEn ? "The limit of turrets WITHOUT electricity by privileges [Permission] = Limit (Make a list from more to less)" : "Лимит турелей БЕЗ электричества по привилегиям [Права] = Лимит (Составляйте список от большего - к меньшему)")]
    public Dictionary<String, Int32> PermissionsLimits;
    [JsonProperty(LanguageEn ? "Use the limit on turrets WITHOUT electricity? (true - yes/false - no)" : "Использовать лимит на туррели БЕЗ электричества? (true - да/false - нет)")]
    public Boolean UseLimitControll;
    [JsonProperty(LanguageEn ? "Limit Type: 0 - Player, 1 - Building" : "Тип лимита : 0 - На игрока, 1 - На шкаф")]
    public TypeLimiter typeLimiter;
    [JsonProperty(LanguageEn ? "Limit turrets WITHOUT electricity (If the player does not have privileges)" : "Лимит турелей БЕЗ электричества (Если у игрока нет привилегий)")]
    public Int32 LimitAmount;
}

internal class ReferenceSettings
{
    [JsonProperty(LanguageEn ? "Setting up collaboration with IQChat" : "Настройка совместной работы с IQChat")]
    public IQChatPlugin IQChatSetting;
    internal class IQChatPlugin
    {
        [JsonProperty(LanguageEn ? "IQChat :Custom prefix in the chat" : "IQChat : Кастомный префикс в чате")]
        public String CustomPrefix;
        [JsonProperty(LanguageEn ? "IQChat : Custom avatar in the chat(If required)" : "IQChat : Кастомный аватар в чате(Если требуется)")]
        public String CustomAvatar;
        [JsonProperty(LanguageEn ? "IQChat : Use UI notifications" : "IQChat : Использовать UI-уведомления")]
        public Boolean UIAlertUse;
    }

    [JsonProperty(LanguageEn ? "Prohibit the use of a switch during a raidBlock?" : "Запретить использовать рубильник во время рейдблока?")]
    public Boolean BlockedTumblerRaidblock;
}

internal class IQChatPlugin
{
    [JsonProperty(LanguageEn ? "IQChat :Custom prefix in the chat" : "IQChat : Кастомный префикс в чате")]
    public String CustomPrefix;
    [JsonProperty(LanguageEn ? "IQChat : Custom avatar in the chat(If required)" : "IQChat : Кастомный аватар в чате(Если требуется)")]
    public String CustomAvatar;
    [JsonProperty(LanguageEn ? "IQChat : Use UI notifications" : "IQChat : Использовать UI-уведомления")]
    public Boolean UIAlertUse;
}


```

---

## IQWipeBlock

```csharp
using System.Linq;
using Oxide.Game.Rust.Cui;
using ConVar;
using Oxide.Core;
using UnityEngine;
using System.Text;
using Object = System.Object;
using System.Collections.Generic;
using System;
using Oxide.Core.Plugins;
using Newtonsoft.Json;
using Network;

Oxide.Plugins
[Info("IQWipeBlock", "Mercury", "1.11.10")]
[Description("Блокируй по умному с IQWipeBlock")]
 class IQWipeBlock : RustPlugin
{
    private String GetImage(String fileName, UInt64 skin);
    static Double CurrentTime { get; set; }
    private static string HexToRustFormat(String hex);
    private bool? CanEquipItem(PlayerInventory inventory, Item item);
     Boolean Interface_UI_Block(BasePlayer player, String Shortname, String ShortnameModule, UInt64 SkinID, List<Connection> playerList, Boolean IsCheckConnect);
     object CanMountEntity(BasePlayer player, MLRS entity);
     object OnLootNetworkUpdate(PlayerLoot loot);
    public void SendImage(BasePlayer player, String imageName, UInt64 imageId);
     void WriteData();
     void OnPlayerLootEnd(PlayerLoot inventory);
    private void ValidsItemTurret(AutoTurret Turret, Item item, Boolean AddedOrRemove);
    [ConsoleCommand("skip.wb")]
     void ConsoleSkipWipeBlock(ConsoleSystem.Arg arg);
    [PluginReference]
     Plugin IQChat;
     Plugin ImageLibrary;
     Plugin Battles;
     Plugin Duel;
     Plugin Duelist;
     Plugin ArenaTournament;
     Plugin AimTraining;
     Plugin XFarmRoom;
     Plugin OneVSOne;
     Plugin EventHelper;
     Plugin IQBackpack;
     Plugin IQRecycler;
     void HideButtonIsMenu(BasePlayer player);
     void HideOrUnHigePanel(BasePlayer player, Boolean IsMenu);
    [JsonProperty(LanguageEn ? "" :"Сдвиг времени в секундах")]
    public Int32 SkipTimeBlocked;
     void OnItemAddedToContainer(ItemContainer container, Item item);
    public Double TimeUnblock(Int32 BlockTime);
    private Vector3 GetDropVelocity(StorageContainer container);
     void Unload();
    public Boolean HasImage(String imageName);
    private void ValidsItemAttackHelicopter(StorageContainer container, Item item, Boolean AddedOrRemove);
     void UI_Category_More_Popup(BasePlayer player, Configuration.Blocks.BlockElement BlockElement);
    public Dictionary<UInt64, List<Connection>> ContainerGetPlayer;
    protected override void SaveConfig();
     object OnMagazineReload(BaseProjectile weapon, IAmmoContainer desiredAmount, BasePlayer player);
    protected override void LoadConfig();
    private const Boolean LanguageEn;
    private new void LoadDefaultMessages();
    [ChatCommand("block")]
     void ChatCommandBlock(BasePlayer player);
    private void CheckAllUnlock();
    [ConsoleCommand("bpanel")]
     void ConsoleCommandBlockPanel(ConsoleSystem.Arg arg);
    private Boolean IsItemBlock(BasePlayer player, String Shortname);
     Boolean Block_Skin_Controller(Configuration.Blocks.BlockElement BlockInfo, Dictionary<String, Configuration.Blocks.BlockElement> BlockAll, Boolean BlockStatusShortname, UInt64 SkinID);
     void RegisteredUser(BasePlayer player);
     void UI_IQ_WipeBlock(BasePlayer player);
    protected override void LoadDefaultConfig();
    private bool? CanWearItem(PlayerInventory inventory, Item item);
     void SetFlagItem(Item item, Boolean FlagStatus);
    public static StringBuilder sb;
     object OnItemAction(Item item, string action, BasePlayer player);
    [JsonProperty(LanguageEn ? "" :"Дата с информацией о игроках")]
    public Dictionary<UInt64, Boolean> BlockHideInfo;
    public void SendChat(string Message, BasePlayer player, Chat.ChatChannel channel);
    public readonly String IQWIPE_PARENT_PANEL_BTN;
    private static Configuration config;
    private String Format(Int32 units, String form1, String form2, String form3);
    private void OnEntitySpawned(StorageContainer containerAttackHelicopter);
     void ReadData();
    [ConsoleCommand("block")]
     void ConsoleCommandBlock(ConsoleSystem.Arg arg);
    [ConsoleCommand("iqwb")]
     void ConsoleFuncCommand(ConsoleSystem.Arg arg);
    public readonly String IQWIPE_PARENT;
     void NetworkIDGetSet(UInt64 NetID, Boolean SetOrRemove, BasePlayer player);
     void Interface_Button_Panel(BasePlayer player);
    private class Configuration
    {
        internal class Blocks
        {
            [JsonProperty(LanguageEn ? "Configuring weapon and Tool Locks" : "Настройка блокировок оружия и инструментов")]
            public Dictionary<String, BlockElement> BlockWeaponAndTools;
            [JsonProperty(LanguageEn ? "Configuring Gear locks" : "Настройка блокировок снаряжения")]
            public Dictionary<String, BlockElement> BlockArmory;
            [JsonProperty(LanguageEn ? "Setting up explosive locks" : "Настройка блокировок взрывчатки")]
            public Dictionary<String, BlockElement> BlockBoom;
            [JsonProperty(LanguageEn ? "Unlock additional items after unlocking the main one (true) or jointly (false)" : "Разблокировку дополнительных предметов запускать после разблокировки основного (true) или совместно (false)")]
            public Boolean VaribleUnlockMore;
            internal class BlockElement
            {
                [JsonProperty(LanguageEn ? "Time to lock this item(in seconds)" : "Время блокировки данного предмета(в секундах)")]
                public Int32 TimeBlock;
                [JsonProperty(LanguageEn ? "SkinID for the item(if not required, leave the value 0)" : "SkinID для предмета(если не требуется, оставьте значение 0)")]
                public UInt64 SkinID;
                [JsonProperty(LanguageEn ? "Additional list related to this subject! (Items that can be applied to the main item, example Weapons - > Ammo)" : "Дополнительный список, относящийся к этому предмету! (Предметы, которые можно применить к основному предмету, пример Оружие -> Патроны)")]
                public Dictionary<String, Int32> BlockMoreList;
            }

        }

        public static Configuration GetNewConfiguration();
        [JsonProperty(LanguageEn ? "Setting up Locks" : "Настройка блокировок")]
        public Blocks Block;
        [JsonProperty(LanguageEn ? "Configuring the plugin" : "Настройка плагина")]
        public GeneralSettings GeneralSetting;
        [JsonProperty(LanguageEn ? "Configuring the interface" : "Настройка интерфейса")]
        public Interfaces Interface;
        internal class Interfaces
        {
            [JsonProperty(LanguageEn ? "In which part of the screen will the interface with the lock of weapons and tools be located(0-Left, 1-Center, 2-Right)" : "В какой части экрана будет расположен интерфейс с блокировкой оружия и инструментов(0 - Слева, 1 - Центр, 2 - Справа)")]
            public AlignCategory CategoryWeaponTools;
            [JsonProperty(LanguageEn ? "In which part of the screen will the interface with the equipment lock be located (0-Left, 1-Center, 2-Right)" : "В какой части экрана будет расположен интерфейс с блокировкой снаряжения (0 - Слева, 1 - Центр, 2 - Справа)")]
            public AlignCategory CategoryArmory;
            [JsonProperty(LanguageEn ? "In which part of the screen will the interface with blocking explosives and ammunition be located(0-On the Left, 1-In the Center, 2-on the Right)" : "В какой части экрана будет расположен интерфейс с блокировкой взрывчатки и боеприпасов(0 - Слева, 1 - Центр, 2 - Справа)")]
            public AlignCategory CategoryBoom;
            [JsonProperty(LanguageEn ? "Display the progress of opening an item by filling in the background" : "Отображать прогресс открытия предмета заполнением заднего фона")]
            public Boolean UseProgressiveBackground;
            [JsonProperty(LanguageEn ? "Display information-instructions, which block is responsible for what" : "Отображать информацию-инструкцию, какой блок за что отвечает")]
            public Boolean ShowInformationBlocks;
            [JsonProperty(LanguageEn ? "Use consolidated view for items with the same unlock time (true)" : "Использовать объединенный вид разблокировки предметов с одинаковым временем (true)")]
            public Boolean ShowUnblockedItemCompareTime;
            [JsonProperty(LanguageEn ? "Link to your background (If not required, leave the field blank)" : "Ссылка на свой задний фон(Если не требуется, оставьте поле пустым)")]
            public String BackgroundUrl;
            [JsonProperty(LanguageEn ? "HEX background color" : "HEX цвет заднего фона")]
            public String BacgkroundColor;
            [JsonProperty(LanguageEn ? "HEX background color blurr" : "HEX цвет блюра заднего фона")]
            public String BackgroundBlurColor;
            [JsonProperty(LanguageEn ? "HEX background color blurr of additional items" : "HEX цвет блюра заднего фона дополнительных предметов")]
            public String BackgroundMoreBlurColor;
            [JsonProperty(LanguageEn ? "HEX text color" : "HEX цвет текста")]
            public String Labels;
            [JsonProperty(LanguageEn ? "HEX line color" : "HEX цвет линий")]
            public String Lines;
            [JsonProperty(LanguageEn ? "HEX line color when unblocking" : "HEX цвет линий при разблокировке")]
            public String LinesUnblock;
            [JsonProperty(LanguageEn ? "HEX background color of the blocked image" : "HEX цвет заднего фона заблокированного")]
            public String BlockedPanel;
            [JsonProperty(LanguageEn ? "HEX color of the background color of the blocked background" : "HEX цвет блюра заднего фона заблокированного")]
            public String BlurBlockedPanel;
            [JsonProperty(LanguageEn ? "HEX background color of the next item to unlock" : "HEX цвет заднего фона следующего предмета под разблокировку")]
            public String NextBlockedPanel;
            [JsonProperty(LanguageEn ? "HEX background color of the unlocked item" : "HEX цвет заднего фона разблокированного предмета")]
            public String UnblockedPanel;
            [JsonProperty(LanguageEn ? "HEX background color of an unlocked item with additional locks" : "HEX цвет заднего фона разблокированного предмета с дополнительными блокировками")]
            public String UnblockedMorePanel;
            [JsonProperty(LanguageEn ? "HEX color of the subject lines with additional locks" : "HEX цвет линий предмета с дополнительными блокировками")]
            public String PreBlockedMoreLine;
            [JsonProperty(LanguageEn ? "Sprite of the blocked element" : "Sprite заблокированного элемента")]
            public String SpriteBlocked;
            [JsonProperty(LanguageEn ? "Sprite in the quick access menu" : "Sprite в меню быстрого доступа")]
            public String SpriteBlockLogo;
            [JsonProperty(LanguageEn ? "Should I hide the interface opening button after unlocking all items" : "Скрывать ли кнопку открытия интерфейса после разблока всех предметов")]
            public Boolean ButtonIsBlock;
            [JsonProperty(LanguageEn ? "Use the time format (Full - D + H + M(/S) - true / Abbreviated - D/H/M/S - false)" : "Использовать формат времени (Полный - Д + Ч + М(/С) - true / Сокращенный - Д/Ч/М/С - false)")]
            public Boolean UseFormatTime;
            [JsonProperty(LanguageEn ? "AnchorMin : The position of the entire quick menu" : "AnchorMin : Позиция всего быстрого меню")]
            public String AnchorMinPositionQuickMenu;
            [JsonProperty(LanguageEn ? "AnchorMax : The position of the entire quick menu" : "AnchorMax : Позиция всего быстрого меню")]
            public String AnchorMaxPositionQuickMenu;
            [JsonProperty(LanguageEn ? "OffsetMin : The position of the entire quick menu" : "OffsetMin : Позиция всего быстрого меню")]
            public String OffsetMinPositionQuickMenu;
            [JsonProperty(LanguageEn ? "OffsetMax : The position of the entire quick menu" : "OffsetMax : Позиция всего быстрого меню")]
            public String OffsetMaxPositionQuickMenu;
            [JsonProperty(LanguageEn ? "AnchorMin : Icon position in the Quick access menu" : "AnchorMin : Позиция иконки в меню быстрого доступа")]
            public String AnchorMinPositionSpriteLogo;
            [JsonProperty(LanguageEn ? "AnchorMax : Icon position in the Quick access menu" : "AnchorMax : Позиция иконки в меню быстрого доступа")]
            public String AnchorMaxPositionSpriteLogo;
        }

        internal class GeneralSettings
        {
            internal class ReferenceSetting
            {
                [JsonProperty(LanguageEn ? "IQChat : Chat Settings" : "IQChat : Настройки чата")]
                public ChatSetting ChatSettings;
                internal class ChatSetting
                {
                    [JsonProperty(LanguageEn ? "IQChat : Custom prefix in chat" : "IQChat : Кастомный префикс в чате")]
                    public String CustomPrefix;
                    [JsonProperty(LanguageEn ? "IQChat : Custom avatar in the chat (If required)" : "IQChat : Кастомный аватар в чате(Если требуется)")]
                    public String CustomAvatar;
                    [JsonProperty(LanguageEn ? "IQChat : Use UI notifications" : "IQChat : Использовать UI уведомления")]
                    public Boolean UIAlertUse;
                }

            }

            [JsonProperty(LanguageEn ? "Select the label type : 0 - Grid, 1-Flame, 2-Lightning" : "Выберите тип метки : 0 - Сетка, 1 - Пламя, 2 - Молния")]
            public TypeLock TypeLock;
            [JsonProperty(LanguageEn ? "Notify players every time they log on to the server that all items are unlocked (true - yes/false - no). The message is configured in lang" : "Уведомлять игроков при каждом входе на сервер, что все предметы разблокированы (true - да/false - нет). Сообщение настраивается в lang")]
            public Boolean AlertConnectedUserUnlocked;
            [JsonProperty(LanguageEn ? "Notify players that all items have been fully unlocked (true - yes/false - no). The message is configured in lang" : "Уведомлять игроков о том, что произошла полная разблокировка всех предметов (true - да/false - нет). Сообщение настраивается в lang")]
            public Boolean AlertAllUsersUnlocked;
            [JsonProperty(LanguageEn ? "Enable the ability to hide the menu to users" : "Включить возможность скрыть меню пользователям")]
            public Boolean UseHidePanelButton;
            [JsonProperty(LanguageEn ? "Use the label in the player's inventory if the item is locked(the label will be right on the item being locked)" : "Использовать метку в инвентаре игрока если предмет заблокирован(метка будет прям на предмете блокировки)")]
            public Boolean UseFlags;
            [JsonProperty(LanguageEn ? "Settings for collaboration with other plugins" : "Настройки совместной работы с другими плагинами")]
            public ReferenceSetting ReferenceSettings;
            [JsonProperty(LanguageEn ? "Display time on all blocked items, regardless of progress" : "Отображать время на всех заблокированных предметах, независимо от прогресса")]
            public Boolean UseShowAllTime;
            [JsonProperty(LanguageEn ? "Enable menu to open wipeblock" : "Включить меню для открытия вайпблока")]
            public Boolean UsePanelButton;
        }

    }

     Configuration.Blocks.BlockElement GetBlockElement(String Shortname);
    private void Init();
    readonly static String PermissionIgnoreBlock;
    private void OnEntitySpawned(BaseSubmarine submarine);
    private Boolean IsUnlockedAll;
    private void ValidsItemSubmarine(BaseSubmarine submarine, Item item, Boolean AddedOrRemove);
    public Boolean AddImage(String url, String shortname, UInt64 skin);
    private void OnEntitySpawned(AutoTurret turret);
    private String ShortnameValidation(String Shortname);
    public String GetLang(String LangKey, String userID, object[] args);
    [ChatCommand("bpanel")]
     void ChatCommandBlockPanel(BasePlayer player, string cmd, string[] arg);
    public String FormatTime(TimeSpan time, String UserID);
    public Boolean IsDuel(UInt64 userID);
    static readonly DateTime epoch;
    private object OnWeaponReload(BaseProjectile weapon, BasePlayer player);
    private void OnServerInitialized();
     ItemContainer.CanAcceptResult? CanAcceptItem(ItemContainer container, Item item, int targetPos);
     void UI_Category_Show(BasePlayer player, CuiElementContainer container, Configuration.Interfaces Interface, Dictionary<String, Configuration.Blocks.BlockElement> CategoryList, AlignCategory Align);
     void OnPlayerConnected(BasePlayer player);
}

private class Configuration
{
    internal class Blocks
    {
        [JsonProperty(LanguageEn ? "Configuring weapon and Tool Locks" : "Настройка блокировок оружия и инструментов")]
        public Dictionary<String, BlockElement> BlockWeaponAndTools;
        [JsonProperty(LanguageEn ? "Configuring Gear locks" : "Настройка блокировок снаряжения")]
        public Dictionary<String, BlockElement> BlockArmory;
        [JsonProperty(LanguageEn ? "Setting up explosive locks" : "Настройка блокировок взрывчатки")]
        public Dictionary<String, BlockElement> BlockBoom;
        [JsonProperty(LanguageEn ? "Unlock additional items after unlocking the main one (true) or jointly (false)" : "Разблокировку дополнительных предметов запускать после разблокировки основного (true) или совместно (false)")]
        public Boolean VaribleUnlockMore;
        internal class BlockElement
        {
            [JsonProperty(LanguageEn ? "Time to lock this item(in seconds)" : "Время блокировки данного предмета(в секундах)")]
            public Int32 TimeBlock;
            [JsonProperty(LanguageEn ? "SkinID for the item(if not required, leave the value 0)" : "SkinID для предмета(если не требуется, оставьте значение 0)")]
            public UInt64 SkinID;
            [JsonProperty(LanguageEn ? "Additional list related to this subject! (Items that can be applied to the main item, example Weapons - > Ammo)" : "Дополнительный список, относящийся к этому предмету! (Предметы, которые можно применить к основному предмету, пример Оружие -> Патроны)")]
            public Dictionary<String, Int32> BlockMoreList;
        }

    }

    public static Configuration GetNewConfiguration();
    [JsonProperty(LanguageEn ? "Setting up Locks" : "Настройка блокировок")]
    public Blocks Block;
    [JsonProperty(LanguageEn ? "Configuring the plugin" : "Настройка плагина")]
    public GeneralSettings GeneralSetting;
    [JsonProperty(LanguageEn ? "Configuring the interface" : "Настройка интерфейса")]
    public Interfaces Interface;
    internal class Interfaces
    {
        [JsonProperty(LanguageEn ? "In which part of the screen will the interface with the lock of weapons and tools be located(0-Left, 1-Center, 2-Right)" : "В какой части экрана будет расположен интерфейс с блокировкой оружия и инструментов(0 - Слева, 1 - Центр, 2 - Справа)")]
        public AlignCategory CategoryWeaponTools;
        [JsonProperty(LanguageEn ? "In which part of the screen will the interface with the equipment lock be located (0-Left, 1-Center, 2-Right)" : "В какой части экрана будет расположен интерфейс с блокировкой снаряжения (0 - Слева, 1 - Центр, 2 - Справа)")]
        public AlignCategory CategoryArmory;
        [JsonProperty(LanguageEn ? "In which part of the screen will the interface with blocking explosives and ammunition be located(0-On the Left, 1-In the Center, 2-on the Right)" : "В какой части экрана будет расположен интерфейс с блокировкой взрывчатки и боеприпасов(0 - Слева, 1 - Центр, 2 - Справа)")]
        public AlignCategory CategoryBoom;
        [JsonProperty(LanguageEn ? "Display the progress of opening an item by filling in the background" : "Отображать прогресс открытия предмета заполнением заднего фона")]
        public Boolean UseProgressiveBackground;
        [JsonProperty(LanguageEn ? "Display information-instructions, which block is responsible for what" : "Отображать информацию-инструкцию, какой блок за что отвечает")]
        public Boolean ShowInformationBlocks;
        [JsonProperty(LanguageEn ? "Use consolidated view for items with the same unlock time (true)" : "Использовать объединенный вид разблокировки предметов с одинаковым временем (true)")]
        public Boolean ShowUnblockedItemCompareTime;
        [JsonProperty(LanguageEn ? "Link to your background (If not required, leave the field blank)" : "Ссылка на свой задний фон(Если не требуется, оставьте поле пустым)")]
        public String BackgroundUrl;
        [JsonProperty(LanguageEn ? "HEX background color" : "HEX цвет заднего фона")]
        public String BacgkroundColor;
        [JsonProperty(LanguageEn ? "HEX background color blurr" : "HEX цвет блюра заднего фона")]
        public String BackgroundBlurColor;
        [JsonProperty(LanguageEn ? "HEX background color blurr of additional items" : "HEX цвет блюра заднего фона дополнительных предметов")]
        public String BackgroundMoreBlurColor;
        [JsonProperty(LanguageEn ? "HEX text color" : "HEX цвет текста")]
        public String Labels;
        [JsonProperty(LanguageEn ? "HEX line color" : "HEX цвет линий")]
        public String Lines;
        [JsonProperty(LanguageEn ? "HEX line color when unblocking" : "HEX цвет линий при разблокировке")]
        public String LinesUnblock;
        [JsonProperty(LanguageEn ? "HEX background color of the blocked image" : "HEX цвет заднего фона заблокированного")]
        public String BlockedPanel;
        [JsonProperty(LanguageEn ? "HEX color of the background color of the blocked background" : "HEX цвет блюра заднего фона заблокированного")]
        public String BlurBlockedPanel;
        [JsonProperty(LanguageEn ? "HEX background color of the next item to unlock" : "HEX цвет заднего фона следующего предмета под разблокировку")]
        public String NextBlockedPanel;
        [JsonProperty(LanguageEn ? "HEX background color of the unlocked item" : "HEX цвет заднего фона разблокированного предмета")]
        public String UnblockedPanel;
        [JsonProperty(LanguageEn ? "HEX background color of an unlocked item with additional locks" : "HEX цвет заднего фона разблокированного предмета с дополнительными блокировками")]
        public String UnblockedMorePanel;
        [JsonProperty(LanguageEn ? "HEX color of the subject lines with additional locks" : "HEX цвет линий предмета с дополнительными блокировками")]
        public String PreBlockedMoreLine;
        [JsonProperty(LanguageEn ? "Sprite of the blocked element" : "Sprite заблокированного элемента")]
        public String SpriteBlocked;
        [JsonProperty(LanguageEn ? "Sprite in the quick access menu" : "Sprite в меню быстрого доступа")]
        public String SpriteBlockLogo;
        [JsonProperty(LanguageEn ? "Should I hide the interface opening button after unlocking all items" : "Скрывать ли кнопку открытия интерфейса после разблока всех предметов")]
        public Boolean ButtonIsBlock;
        [JsonProperty(LanguageEn ? "Use the time format (Full - D + H + M(/S) - true / Abbreviated - D/H/M/S - false)" : "Использовать формат времени (Полный - Д + Ч + М(/С) - true / Сокращенный - Д/Ч/М/С - false)")]
        public Boolean UseFormatTime;
        [JsonProperty(LanguageEn ? "AnchorMin : The position of the entire quick menu" : "AnchorMin : Позиция всего быстрого меню")]
        public String AnchorMinPositionQuickMenu;
        [JsonProperty(LanguageEn ? "AnchorMax : The position of the entire quick menu" : "AnchorMax : Позиция всего быстрого меню")]
        public String AnchorMaxPositionQuickMenu;
        [JsonProperty(LanguageEn ? "OffsetMin : The position of the entire quick menu" : "OffsetMin : Позиция всего быстрого меню")]
        public String OffsetMinPositionQuickMenu;
        [JsonProperty(LanguageEn ? "OffsetMax : The position of the entire quick menu" : "OffsetMax : Позиция всего быстрого меню")]
        public String OffsetMaxPositionQuickMenu;
        [JsonProperty(LanguageEn ? "AnchorMin : Icon position in the Quick access menu" : "AnchorMin : Позиция иконки в меню быстрого доступа")]
        public String AnchorMinPositionSpriteLogo;
        [JsonProperty(LanguageEn ? "AnchorMax : Icon position in the Quick access menu" : "AnchorMax : Позиция иконки в меню быстрого доступа")]
        public String AnchorMaxPositionSpriteLogo;
    }

    internal class GeneralSettings
    {
        internal class ReferenceSetting
        {
            [JsonProperty(LanguageEn ? "IQChat : Chat Settings" : "IQChat : Настройки чата")]
            public ChatSetting ChatSettings;
            internal class ChatSetting
            {
                [JsonProperty(LanguageEn ? "IQChat : Custom prefix in chat" : "IQChat : Кастомный префикс в чате")]
                public String CustomPrefix;
                [JsonProperty(LanguageEn ? "IQChat : Custom avatar in the chat (If required)" : "IQChat : Кастомный аватар в чате(Если требуется)")]
                public String CustomAvatar;
                [JsonProperty(LanguageEn ? "IQChat : Use UI notifications" : "IQChat : Использовать UI уведомления")]
                public Boolean UIAlertUse;
            }

        }

        [JsonProperty(LanguageEn ? "Select the label type : 0 - Grid, 1-Flame, 2-Lightning" : "Выберите тип метки : 0 - Сетка, 1 - Пламя, 2 - Молния")]
        public TypeLock TypeLock;
        [JsonProperty(LanguageEn ? "Notify players every time they log on to the server that all items are unlocked (true - yes/false - no). The message is configured in lang" : "Уведомлять игроков при каждом входе на сервер, что все предметы разблокированы (true - да/false - нет). Сообщение настраивается в lang")]
        public Boolean AlertConnectedUserUnlocked;
        [JsonProperty(LanguageEn ? "Notify players that all items have been fully unlocked (true - yes/false - no). The message is configured in lang" : "Уведомлять игроков о том, что произошла полная разблокировка всех предметов (true - да/false - нет). Сообщение настраивается в lang")]
        public Boolean AlertAllUsersUnlocked;
        [JsonProperty(LanguageEn ? "Enable the ability to hide the menu to users" : "Включить возможность скрыть меню пользователям")]
        public Boolean UseHidePanelButton;
        [JsonProperty(LanguageEn ? "Use the label in the player's inventory if the item is locked(the label will be right on the item being locked)" : "Использовать метку в инвентаре игрока если предмет заблокирован(метка будет прям на предмете блокировки)")]
        public Boolean UseFlags;
        [JsonProperty(LanguageEn ? "Settings for collaboration with other plugins" : "Настройки совместной работы с другими плагинами")]
        public ReferenceSetting ReferenceSettings;
        [JsonProperty(LanguageEn ? "Display time on all blocked items, regardless of progress" : "Отображать время на всех заблокированных предметах, независимо от прогресса")]
        public Boolean UseShowAllTime;
        [JsonProperty(LanguageEn ? "Enable menu to open wipeblock" : "Включить меню для открытия вайпблока")]
        public Boolean UsePanelButton;
    }

}

internal class Blocks
{
    [JsonProperty(LanguageEn ? "Configuring weapon and Tool Locks" : "Настройка блокировок оружия и инструментов")]
    public Dictionary<String, BlockElement> BlockWeaponAndTools;
    [JsonProperty(LanguageEn ? "Configuring Gear locks" : "Настройка блокировок снаряжения")]
    public Dictionary<String, BlockElement> BlockArmory;
    [JsonProperty(LanguageEn ? "Setting up explosive locks" : "Настройка блокировок взрывчатки")]
    public Dictionary<String, BlockElement> BlockBoom;
    [JsonProperty(LanguageEn ? "Unlock additional items after unlocking the main one (true) or jointly (false)" : "Разблокировку дополнительных предметов запускать после разблокировки основного (true) или совместно (false)")]
    public Boolean VaribleUnlockMore;
    internal class BlockElement
    {
        [JsonProperty(LanguageEn ? "Time to lock this item(in seconds)" : "Время блокировки данного предмета(в секундах)")]
        public Int32 TimeBlock;
        [JsonProperty(LanguageEn ? "SkinID for the item(if not required, leave the value 0)" : "SkinID для предмета(если не требуется, оставьте значение 0)")]
        public UInt64 SkinID;
        [JsonProperty(LanguageEn ? "Additional list related to this subject! (Items that can be applied to the main item, example Weapons - > Ammo)" : "Дополнительный список, относящийся к этому предмету! (Предметы, которые можно применить к основному предмету, пример Оружие -> Патроны)")]
        public Dictionary<String, Int32> BlockMoreList;
    }

}

internal class BlockElement
{
    [JsonProperty(LanguageEn ? "Time to lock this item(in seconds)" : "Время блокировки данного предмета(в секундах)")]
    public Int32 TimeBlock;
    [JsonProperty(LanguageEn ? "SkinID for the item(if not required, leave the value 0)" : "SkinID для предмета(если не требуется, оставьте значение 0)")]
    public UInt64 SkinID;
    [JsonProperty(LanguageEn ? "Additional list related to this subject! (Items that can be applied to the main item, example Weapons - > Ammo)" : "Дополнительный список, относящийся к этому предмету! (Предметы, которые можно применить к основному предмету, пример Оружие -> Патроны)")]
    public Dictionary<String, Int32> BlockMoreList;
}

internal class Interfaces
{
    [JsonProperty(LanguageEn ? "In which part of the screen will the interface with the lock of weapons and tools be located(0-Left, 1-Center, 2-Right)" : "В какой части экрана будет расположен интерфейс с блокировкой оружия и инструментов(0 - Слева, 1 - Центр, 2 - Справа)")]
    public AlignCategory CategoryWeaponTools;
    [JsonProperty(LanguageEn ? "In which part of the screen will the interface with the equipment lock be located (0-Left, 1-Center, 2-Right)" : "В какой части экрана будет расположен интерфейс с блокировкой снаряжения (0 - Слева, 1 - Центр, 2 - Справа)")]
    public AlignCategory CategoryArmory;
    [JsonProperty(LanguageEn ? "In which part of the screen will the interface with blocking explosives and ammunition be located(0-On the Left, 1-In the Center, 2-on the Right)" : "В какой части экрана будет расположен интерфейс с блокировкой взрывчатки и боеприпасов(0 - Слева, 1 - Центр, 2 - Справа)")]
    public AlignCategory CategoryBoom;
    [JsonProperty(LanguageEn ? "Display the progress of opening an item by filling in the background" : "Отображать прогресс открытия предмета заполнением заднего фона")]
    public Boolean UseProgressiveBackground;
    [JsonProperty(LanguageEn ? "Display information-instructions, which block is responsible for what" : "Отображать информацию-инструкцию, какой блок за что отвечает")]
    public Boolean ShowInformationBlocks;
    [JsonProperty(LanguageEn ? "Use consolidated view for items with the same unlock time (true)" : "Использовать объединенный вид разблокировки предметов с одинаковым временем (true)")]
    public Boolean ShowUnblockedItemCompareTime;
    [JsonProperty(LanguageEn ? "Link to your background (If not required, leave the field blank)" : "Ссылка на свой задний фон(Если не требуется, оставьте поле пустым)")]
    public String BackgroundUrl;
    [JsonProperty(LanguageEn ? "HEX background color" : "HEX цвет заднего фона")]
    public String BacgkroundColor;
    [JsonProperty(LanguageEn ? "HEX background color blurr" : "HEX цвет блюра заднего фона")]
    public String BackgroundBlurColor;
    [JsonProperty(LanguageEn ? "HEX background color blurr of additional items" : "HEX цвет блюра заднего фона дополнительных предметов")]
    public String BackgroundMoreBlurColor;
    [JsonProperty(LanguageEn ? "HEX text color" : "HEX цвет текста")]
    public String Labels;
    [JsonProperty(LanguageEn ? "HEX line color" : "HEX цвет линий")]
    public String Lines;
    [JsonProperty(LanguageEn ? "HEX line color when unblocking" : "HEX цвет линий при разблокировке")]
    public String LinesUnblock;
    [JsonProperty(LanguageEn ? "HEX background color of the blocked image" : "HEX цвет заднего фона заблокированного")]
    public String BlockedPanel;
    [JsonProperty(LanguageEn ? "HEX color of the background color of the blocked background" : "HEX цвет блюра заднего фона заблокированного")]
    public String BlurBlockedPanel;
    [JsonProperty(LanguageEn ? "HEX background color of the next item to unlock" : "HEX цвет заднего фона следующего предмета под разблокировку")]
    public String NextBlockedPanel;
    [JsonProperty(LanguageEn ? "HEX background color of the unlocked item" : "HEX цвет заднего фона разблокированного предмета")]
    public String UnblockedPanel;
    [JsonProperty(LanguageEn ? "HEX background color of an unlocked item with additional locks" : "HEX цвет заднего фона разблокированного предмета с дополнительными блокировками")]
    public String UnblockedMorePanel;
    [JsonProperty(LanguageEn ? "HEX color of the subject lines with additional locks" : "HEX цвет линий предмета с дополнительными блокировками")]
    public String PreBlockedMoreLine;
    [JsonProperty(LanguageEn ? "Sprite of the blocked element" : "Sprite заблокированного элемента")]
    public String SpriteBlocked;
    [JsonProperty(LanguageEn ? "Sprite in the quick access menu" : "Sprite в меню быстрого доступа")]
    public String SpriteBlockLogo;
    [JsonProperty(LanguageEn ? "Should I hide the interface opening button after unlocking all items" : "Скрывать ли кнопку открытия интерфейса после разблока всех предметов")]
    public Boolean ButtonIsBlock;
    [JsonProperty(LanguageEn ? "Use the time format (Full - D + H + M(/S) - true / Abbreviated - D/H/M/S - false)" : "Использовать формат времени (Полный - Д + Ч + М(/С) - true / Сокращенный - Д/Ч/М/С - false)")]
    public Boolean UseFormatTime;
    [JsonProperty(LanguageEn ? "AnchorMin : The position of the entire quick menu" : "AnchorMin : Позиция всего быстрого меню")]
    public String AnchorMinPositionQuickMenu;
    [JsonProperty(LanguageEn ? "AnchorMax : The position of the entire quick menu" : "AnchorMax : Позиция всего быстрого меню")]
    public String AnchorMaxPositionQuickMenu;
    [JsonProperty(LanguageEn ? "OffsetMin : The position of the entire quick menu" : "OffsetMin : Позиция всего быстрого меню")]
    public String OffsetMinPositionQuickMenu;
    [JsonProperty(LanguageEn ? "OffsetMax : The position of the entire quick menu" : "OffsetMax : Позиция всего быстрого меню")]
    public String OffsetMaxPositionQuickMenu;
    [JsonProperty(LanguageEn ? "AnchorMin : Icon position in the Quick access menu" : "AnchorMin : Позиция иконки в меню быстрого доступа")]
    public String AnchorMinPositionSpriteLogo;
    [JsonProperty(LanguageEn ? "AnchorMax : Icon position in the Quick access menu" : "AnchorMax : Позиция иконки в меню быстрого доступа")]
    public String AnchorMaxPositionSpriteLogo;
}

internal class GeneralSettings
{
    internal class ReferenceSetting
    {
        [JsonProperty(LanguageEn ? "IQChat : Chat Settings" : "IQChat : Настройки чата")]
        public ChatSetting ChatSettings;
        internal class ChatSetting
        {
            [JsonProperty(LanguageEn ? "IQChat : Custom prefix in chat" : "IQChat : Кастомный префикс в чате")]
            public String CustomPrefix;
            [JsonProperty(LanguageEn ? "IQChat : Custom avatar in the chat (If required)" : "IQChat : Кастомный аватар в чате(Если требуется)")]
            public String CustomAvatar;
            [JsonProperty(LanguageEn ? "IQChat : Use UI notifications" : "IQChat : Использовать UI уведомления")]
            public Boolean UIAlertUse;
        }

    }

    [JsonProperty(LanguageEn ? "Select the label type : 0 - Grid, 1-Flame, 2-Lightning" : "Выберите тип метки : 0 - Сетка, 1 - Пламя, 2 - Молния")]
    public TypeLock TypeLock;
    [JsonProperty(LanguageEn ? "Notify players every time they log on to the server that all items are unlocked (true - yes/false - no). The message is configured in lang" : "Уведомлять игроков при каждом входе на сервер, что все предметы разблокированы (true - да/false - нет). Сообщение настраивается в lang")]
    public Boolean AlertConnectedUserUnlocked;
    [JsonProperty(LanguageEn ? "Notify players that all items have been fully unlocked (true - yes/false - no). The message is configured in lang" : "Уведомлять игроков о том, что произошла полная разблокировка всех предметов (true - да/false - нет). Сообщение настраивается в lang")]
    public Boolean AlertAllUsersUnlocked;
    [JsonProperty(LanguageEn ? "Enable the ability to hide the menu to users" : "Включить возможность скрыть меню пользователям")]
    public Boolean UseHidePanelButton;
    [JsonProperty(LanguageEn ? "Use the label in the player's inventory if the item is locked(the label will be right on the item being locked)" : "Использовать метку в инвентаре игрока если предмет заблокирован(метка будет прям на предмете блокировки)")]
    public Boolean UseFlags;
    [JsonProperty(LanguageEn ? "Settings for collaboration with other plugins" : "Настройки совместной работы с другими плагинами")]
    public ReferenceSetting ReferenceSettings;
    [JsonProperty(LanguageEn ? "Display time on all blocked items, regardless of progress" : "Отображать время на всех заблокированных предметах, независимо от прогресса")]
    public Boolean UseShowAllTime;
    [JsonProperty(LanguageEn ? "Enable menu to open wipeblock" : "Включить меню для открытия вайпблока")]
    public Boolean UsePanelButton;
}

internal class ReferenceSetting
{
    [JsonProperty(LanguageEn ? "IQChat : Chat Settings" : "IQChat : Настройки чата")]
    public ChatSetting ChatSettings;
    internal class ChatSetting
    {
        [JsonProperty(LanguageEn ? "IQChat : Custom prefix in chat" : "IQChat : Кастомный префикс в чате")]
        public String CustomPrefix;
        [JsonProperty(LanguageEn ? "IQChat : Custom avatar in the chat (If required)" : "IQChat : Кастомный аватар в чате(Если требуется)")]
        public String CustomAvatar;
        [JsonProperty(LanguageEn ? "IQChat : Use UI notifications" : "IQChat : Использовать UI уведомления")]
        public Boolean UIAlertUse;
    }

}

internal class ChatSetting
{
    [JsonProperty(LanguageEn ? "IQChat : Custom prefix in chat" : "IQChat : Кастомный префикс в чате")]
    public String CustomPrefix;
    [JsonProperty(LanguageEn ? "IQChat : Custom avatar in the chat (If required)" : "IQChat : Кастомный аватар в чате(Если требуется)")]
    public String CustomAvatar;
    [JsonProperty(LanguageEn ? "IQChat : Use UI notifications" : "IQChat : Использовать UI уведомления")]
    public Boolean UIAlertUse;
}


```

---

## IRBench

```csharp
using UnityEngine;
using Oxide.Core.Plugins;

Oxide.Plugins
[Info("IRBench", "Werkrat", "0.1.0")]
[Description("Repair without losing durability!")]
 class IRBench : RustPlugin
{
    protected override void LoadDefaultConfig();
    private float conditionLost;
    [HookMethod("OnServerInitialized")]
    private void onServerInitialized();
    [HookMethod("OnEntitySpawned")]
    private void onEntitySpawned(BaseNetworkable ent);
}


```

---

## ItemSkinRandomizer

```csharp
using System.Collections.Generic;
using System.Linq;

Oxide.Plugins
[Info("Item Skin Randomizer", "Mughisi", 1.1, ResourceId = 1328)]
[Description("Simple plugin that will select a random skin for an item when crafting.")]
 class ItemSkinRandomizer : RustPlugin
{
    private readonly Dictionary<string, List<int>> skinsCache;
    private readonly List<int> randomizedTasks;
    private void OnItemCraft(ItemCraftTask task, BasePlayer crafter);
    private void OnItemCraftFinished(ItemCraftTask task, Item item);
    private List<int> GetSkins(ItemDefinition def);
}


```

---

## JoinQuitMsg

```csharp
using Oxide.Core.Plugins;
using UnityEngine;

Oxide.Plugins
[Info("Join Quit Msg", "Those45Ninjas", "0.4.0", ResourceId = 1409)]
[Description("Let users know when someone has joined or left the server.")]
public class JoinQuitMsg : RustPlugin
{
    const string conf_JoinMsg;
    const string conf_QuitMsg;
    const string conf_AdminCol;
    const string conf_PlayerCol;
    const string conf_OnlyAdmin;
    const string conf_ShowConnect;
    const string conf_ShowDisConn;
     string joinFormat;
     string quitFormat;
     string adminColour;
     string userColour;
     bool onlyShowAdmins;
     bool showConnections;
     bool showDisconnections;
    protected override void LoadDefaultConfig();
     void Loaded();
     void LoadConfigFile();
     void OnPlayerInit(BasePlayer player);
     void OnPlayerDisconnected(BasePlayer player, string reason);
}


```

---

## Juggernaut

```csharp
using System;
using System.Collections.Generic;
using System.Globalization;
using Oxide.Core;
using Oxide.Core.Configuration;
using Oxide.Game.Rust.Cui;
using Oxide.Core.Plugins;
using Newtonsoft.Json;
using Rust;
using UnityEngine;
using System.Linq;

Oxide.Plugins
[Info("Juggernaut", "k1lly0u", "0.2.43", ResourceId = 0)]
internal class Juggernaut : RustPlugin
{
    [PluginReference]
     Plugin Clans;
     Plugin Friends;
     Plugin LustyMap;
     Plugin EventManager;
     Plugin ServerRewards;
     Plugin Economics;
    private static Juggernaut ins;
    private StoredData storedData;
    private RestoreData restoreData;
    private DynamicConfigFile data;
    private DynamicConfigFile restorationData;
    private BasePlayer juggernaut;
    private BaseEntity[] finalSphere;
    private Vector3 endPos;
    private Timer nextMatch;
    private Timer startMatch;
    private Timer eventTimer;
    private Timer broadcastTimer;
    private Timer openMessage;
    private Timer uiTimer;
    private double eventEnd;
    private double nextTrigger;
    private string juggernautIcon;
    private bool isOpen;
    private bool hasStarted;
    private bool initialized;
    private bool hasDestinations;
    private static bool isUnloading;
    private List<ulong> optedIn;
    private Dictionary<ulong, Destinations> destinationCreator;
    private const string sphereEnt;
    private void Loaded();
    private void OnServerInitialized();
    private void OnServerSave();
    private void Unload();
    private void OnPlayerInit(BasePlayer player);
    private void OnPlayerDisconnected(BasePlayer player);
    private void OnEntityTakeDamage(BaseCombatEntity entity, HitInfo info);
    private void OnEntitySpawned(BaseNetworkable entity);
    private void OnEntityDeath(BaseCombatEntity entity, HitInfo info);
    private object CanNetworkTo(BaseEntity entity, BasePlayer target);
    private object OnRunCommand(ConsoleSystem.Arg arg);
    private object OnPlayerCommand(ConsoleSystem.Arg arg);
    private void LockInventory(BasePlayer player);
    private void UnlockInventory(BasePlayer player);
    private void StartEventTimer();
    private void CheckConditions();
    private void OpenEvent();
    private void StartEvent();
    private BasePlayer GetRandomPlayer(int tries);
    private void PrepareJuggernaut(BasePlayer player);
    private void MapIconUpdater();
    private void EventCancel();
    private void EventWin();
    private void DestroyEvent();
    private void TryRestorePlayer(BasePlayer player);
    private void GiveJuggernautGear();
    private void UnsubscribeHooks();
    private void SubscribeHooks();
    private double GrabCurrentTime();
    private string GetGridString(Vector3 position);
    private string NumberToString(int number);
    private void MovePosition(BasePlayer player, Vector3 destination);
    private void StartSleeping(BasePlayer player);
    public static void StripInventory(BasePlayer player);
    private IEnumerable<RestoreData.ItemData> GetItems(ItemContainer container);
    public class RestoreData
    {
        public Hash<ulong, PlayerData> restoreData;
        public void AddData(BasePlayer player);
        public void RemoveData(ulong playerId);
        public bool HasRestoreData(ulong playerId);
        public void RestorePlayer(BasePlayer player);
        private void RestoreAllItems(BasePlayer player, PlayerData playerData);
        private bool RestoreItems(BasePlayer player, ItemData[] itemData, string type);
        public Item CreateItem(ItemData itemData);
        public class PlayerData
        {
            public float[] stats;
            public float[] position;
            public ItemData[] containerMain;
            public ItemData[] containerWear;
            public ItemData[] containerBelt;
            public PlayerData();
            public PlayerData(BasePlayer player);
            private float[] GetStats(BasePlayer player);
            public void SetStats(BasePlayer player);
            private float[] GetPosition(Vector3 position);
            public Vector3 GetPosition();
        }

        public class ItemData
        {
            public int itemid;
            public ulong skin;
            public int amount;
            public float condition;
            public float maxCondition;
            public int ammo;
            public string ammotype;
            public int position;
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

    private void CreateSphere();
     class FinalSphere : MonoBehaviour
    {
        public BaseEntity entity;
         void Awake();
         void OnDestroy();
         void OnTriggerEnter(Collider obj);
    }

    private bool IsClanmate(ulong playerId, ulong friendId);
    private bool IsFriend(ulong playerID, ulong friendID);
    private bool IsPlaying(BasePlayer player);
    private void JoinedEvent(BasePlayer player);
    private void AddMapMarker(float x, float z, string markerName);
    private void RemoveMapMarker(string markerName, bool startsWith);
     object CanTrade(BasePlayer player);
     object canRemove(BasePlayer player);
     object CanTeleport(BasePlayer player);
     object canShop(BasePlayer player);
    public class UI
    {
        static public CuiElementContainer CreateElementContainer(string panelName, string color, string aMin, string aMax, bool useCursor);
        static public void AddImage(CuiElementContainer container, string panel, string png, string aMin, string aMax, float fadeOut);
        static public void CreateLabel(CuiElementContainer container, string panel, string color, string text, int size, string aMin, string aMax, TextAnchor align);
        public static string Color(string hexColor, float alpha);
    }

    private const string Main;
    private const string Compass;
    private void CreateTimerUI(BasePlayer player);
    private string GetFormatTime(double timeLeft);
    private void RefreshAllUI();
    private void ShowCompass();
    private WWW info;
    public void Add(string url);
     void TryDownloadImage();
    [ChatCommand("jug")]
     void cmdJuggernaut(BasePlayer player, string command, string[] args);
    [ConsoleCommand("jug")]
     void ccmdJuggernaut(ConsoleSystem.Arg arg);
    private ConfigData configData;
     class ConfigData
    {
        [JsonProperty(PropertyName = "Juggernaut Settings")]
        public JuggernautOptions Juggernaut { get; set; }
        [JsonProperty(PropertyName = "Event Timers")]
        public EventTimers Timers { get; set; }
        [JsonProperty(PropertyName = "UI Settings")]
        public UISettings UI { get; set; }
        [JsonProperty(PropertyName = "Event Conditions")]
        public EventConditions Conditions { get; set; }
        [JsonProperty(PropertyName = "LustyMap Integration")]
        public LustyIntegration Lusty { get; set; }
        [JsonProperty(PropertyName = "Game Settings")]
        public GameOptions Game { get; set; }
        [JsonProperty(PropertyName = "Reward Settings")]
        public PrizeOptions Prizes { get; set; }
        public class JuggernautOptions
        {
            [JsonProperty(PropertyName = "Defense damage modifier")]
            public float DefenseMod { get; set; }
            [JsonProperty(PropertyName = "Attack damage modifier")]
            public float AttackMod { get; set; }
            [JsonProperty(PropertyName = "Can damage structures")]
            public bool DamageStructures { get; set; }
            [JsonProperty(PropertyName = "Start with full metabolism")]
            public bool ResetMetabolism { get; set; }
            [JsonProperty(PropertyName = "Disable landmine damage")]
            public bool NoLandmineDamage { get; set; }
            [JsonProperty(PropertyName = "Disable beartrap damage")]
            public bool NoBeartrapDamage { get; set; }
            [JsonProperty(PropertyName = "Disable fall damage")]
            public bool NoFallDamage { get; set; }
            [JsonProperty(PropertyName = "Inventory contents")]
            public List<InventoryItem> Inventory { get; set; }
            public class InventoryItem
            {
                [JsonProperty(PropertyName = "Item shortname")]
                public string Shortname { get; set; }
                [JsonProperty(PropertyName = "Amount of item")]
                public int Amount { get; set; }
                [JsonProperty(PropertyName = "Item skin ID")]
                public ulong SkinID { get; set; }
                [JsonProperty(PropertyName = "Is this item a blueprint?")]
                public bool IsBP { get; set; }
                [JsonProperty(PropertyName = "Container (main, wear or belt)")]
                public string Container { get; set; }
            }

        }

        public class PrizeOptions
        {
            [JsonProperty(PropertyName = "Allow players to loot juggernaut as a prize")]
            public bool Inventory { get; set; }
            [JsonProperty(PropertyName = "Use Economics money as a prize")]
            public bool Economics { get; set; }
            [JsonProperty(PropertyName = "Use ServerRewards money as a prize")]
            public bool ServerRewards { get; set; }
            [JsonProperty(PropertyName = "Monetary amount")]
            public int Amount { get; set; }
        }

        public class EventTimers
        {
            [JsonProperty(PropertyName = "Amount of time to complete journey (seconds)")]
            public int Completion { get; set; }
            [JsonProperty(PropertyName = "Amount of time between events (seconds)")]
            public int Interval { get; set; }
            [JsonProperty(PropertyName = "Amount of time the entry process will remain open (seconds)")]
            public int Open { get; set; }
        }

        public class EventConditions
        {
            [JsonProperty(PropertyName = "The percentage of server players required for the event to start")]
            public float Percentage { get; set; }
            [JsonProperty(PropertyName = "The minimum amount of players on the server required to open the event")]
            public int OpenMin { get; set; }
        }

        public class LustyIntegration
        {
            [JsonProperty(PropertyName = "Show the destination icon on the map")]
            public bool Enabled { get; set; }
            [JsonProperty(PropertyName = "The amount of possible destinations to show")]
            public int DestinationAmount { get; set; }
        }

        public class GameOptions
        {
            [JsonProperty(PropertyName = "Broadcast the juggernauts position to chat every X seconds")]
            public int BroadcastEvery { get; set; }
            [JsonProperty(PropertyName = "Broadcast the juggernauts position to LustyMap ever X seconds")]
            public bool BroadcastEveryLM { get; set; }
            [JsonProperty(PropertyName = "The amount of time the juggernauts position will remain on LustyMap if enabled (seconds)")]
            public int BroadcastLMSeconds { get; set; }
            [JsonProperty(PropertyName = "Destination sphere darkness")]
            public int SphereDarkness { get; set; }
            [JsonProperty(PropertyName = "Blacklisted commands for event players")]
            public string[] CommandBlacklist { get; set; }
        }

        public class UISettings
        {
            [JsonProperty(PropertyName = "Show the juggernaut their position so they can navigate")]
            public bool ShowCoordsToJuggernaut { get; set; }
            [JsonProperty(PropertyName = "Display a timer showing how long the juggernaut has to get to their destination")]
            public bool ShowUITimer { get; set; }
            [JsonProperty(PropertyName = "The URL of the juggernaut icon")]
            public string IconUrl { get; set; }
            [JsonProperty(PropertyName = "Timer positioning")]
            public Position TimerPosition { get; set; }
            [JsonProperty(PropertyName = "Compass positioning")]
            public Position CompassPosition { get; set; }
            [JsonProperty(PropertyName = "UI background color (hex)")]
            public string UIBackgroundColor { get; set; }
            [JsonProperty(PropertyName = "UI opacity (0.0 - 1.0)")]
            public float UIOpacity { get; set; }
            public class Position
            {
                [JsonProperty(PropertyName = "Horizontal start position (left)")]
                public float XPosition { get; set; }
                [JsonProperty(PropertyName = "Vertical start position (bottom)")]
                public float YPosition { get; set; }
                [JsonProperty(PropertyName = "Horizontal dimensions")]
                public float XDimension { get; set; }
                [JsonProperty(PropertyName = "Vertical dimensions")]
                public float YDimension { get; set; }
            }

        }

        public Oxide.Core.VersionNumber Version { get; set; }
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    private ConfigData GetBaseConfig();
    protected override void SaveConfig();
    private void UpdateConfigValues();
    private class Destinations
    {
        public float x1;
        public float y1;
        public float z1;
        public float x2;
        public float y2;
        public float z2;
    }

    private class Points
    {
        public int amount;
        public bool isRp;
    }

    private void SaveData();
    private void SaveRestoreData();
    private void LoadData();
    private class StoredData
    {
        public Dictionary<ulong, List<RestoreData.ItemData>> winnerRewards;
        public Dictionary<ulong, List<Points>> winnerMoney;
        public List<Destinations> destinations;
    }

     string msg(string key, string playerId);
     Dictionary<string, string> Messages;
}

public class RestoreData
{
    public Hash<ulong, PlayerData> restoreData;
    public void AddData(BasePlayer player);
    public void RemoveData(ulong playerId);
    public bool HasRestoreData(ulong playerId);
    public void RestorePlayer(BasePlayer player);
    private void RestoreAllItems(BasePlayer player, PlayerData playerData);
    private bool RestoreItems(BasePlayer player, ItemData[] itemData, string type);
    public Item CreateItem(ItemData itemData);
    public class PlayerData
    {
        public float[] stats;
        public float[] position;
        public ItemData[] containerMain;
        public ItemData[] containerWear;
        public ItemData[] containerBelt;
        public PlayerData();
        public PlayerData(BasePlayer player);
        private float[] GetStats(BasePlayer player);
        public void SetStats(BasePlayer player);
        private float[] GetPosition(Vector3 position);
        public Vector3 GetPosition();
    }

    public class ItemData
    {
        public int itemid;
        public ulong skin;
        public int amount;
        public float condition;
        public float maxCondition;
        public int ammo;
        public string ammotype;
        public int position;
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

public class PlayerData
{
    public float[] stats;
    public float[] position;
    public ItemData[] containerMain;
    public ItemData[] containerWear;
    public ItemData[] containerBelt;
    public PlayerData();
    public PlayerData(BasePlayer player);
    private float[] GetStats(BasePlayer player);
    public void SetStats(BasePlayer player);
    private float[] GetPosition(Vector3 position);
    public Vector3 GetPosition();
}

public class ItemData
{
    public int itemid;
    public ulong skin;
    public int amount;
    public float condition;
    public float maxCondition;
    public int ammo;
    public string ammotype;
    public int position;
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

 class FinalSphere : MonoBehaviour
{
    public BaseEntity entity;
     void Awake();
     void OnDestroy();
     void OnTriggerEnter(Collider obj);
}

public class UI
{
    static public CuiElementContainer CreateElementContainer(string panelName, string color, string aMin, string aMax, bool useCursor);
    static public void AddImage(CuiElementContainer container, string panel, string png, string aMin, string aMax, float fadeOut);
    static public void CreateLabel(CuiElementContainer container, string panel, string color, string text, int size, string aMin, string aMax, TextAnchor align);
    public static string Color(string hexColor, float alpha);
}

 class ConfigData
{
    [JsonProperty(PropertyName = "Juggernaut Settings")]
    public JuggernautOptions Juggernaut { get; set; }
    [JsonProperty(PropertyName = "Event Timers")]
    public EventTimers Timers { get; set; }
    [JsonProperty(PropertyName = "UI Settings")]
    public UISettings UI { get; set; }
    [JsonProperty(PropertyName = "Event Conditions")]
    public EventConditions Conditions { get; set; }
    [JsonProperty(PropertyName = "LustyMap Integration")]
    public LustyIntegration Lusty { get; set; }
    [JsonProperty(PropertyName = "Game Settings")]
    public GameOptions Game { get; set; }
    [JsonProperty(PropertyName = "Reward Settings")]
    public PrizeOptions Prizes { get; set; }
    public class JuggernautOptions
    {
        [JsonProperty(PropertyName = "Defense damage modifier")]
        public float DefenseMod { get; set; }
        [JsonProperty(PropertyName = "Attack damage modifier")]
        public float AttackMod { get; set; }
        [JsonProperty(PropertyName = "Can damage structures")]
        public bool DamageStructures { get; set; }
        [JsonProperty(PropertyName = "Start with full metabolism")]
        public bool ResetMetabolism { get; set; }
        [JsonProperty(PropertyName = "Disable landmine damage")]
        public bool NoLandmineDamage { get; set; }
        [JsonProperty(PropertyName = "Disable beartrap damage")]
        public bool NoBeartrapDamage { get; set; }
        [JsonProperty(PropertyName = "Disable fall damage")]
        public bool NoFallDamage { get; set; }
        [JsonProperty(PropertyName = "Inventory contents")]
        public List<InventoryItem> Inventory { get; set; }
        public class InventoryItem
        {
            [JsonProperty(PropertyName = "Item shortname")]
            public string Shortname { get; set; }
            [JsonProperty(PropertyName = "Amount of item")]
            public int Amount { get; set; }
            [JsonProperty(PropertyName = "Item skin ID")]
            public ulong SkinID { get; set; }
            [JsonProperty(PropertyName = "Is this item a blueprint?")]
            public bool IsBP { get; set; }
            [JsonProperty(PropertyName = "Container (main, wear or belt)")]
            public string Container { get; set; }
        }

    }

    public class PrizeOptions
    {
        [JsonProperty(PropertyName = "Allow players to loot juggernaut as a prize")]
        public bool Inventory { get; set; }
        [JsonProperty(PropertyName = "Use Economics money as a prize")]
        public bool Economics { get; set; }
        [JsonProperty(PropertyName = "Use ServerRewards money as a prize")]
        public bool ServerRewards { get; set; }
        [JsonProperty(PropertyName = "Monetary amount")]
        public int Amount { get; set; }
    }

    public class EventTimers
    {
        [JsonProperty(PropertyName = "Amount of time to complete journey (seconds)")]
        public int Completion { get; set; }
        [JsonProperty(PropertyName = "Amount of time between events (seconds)")]
        public int Interval { get; set; }
        [JsonProperty(PropertyName = "Amount of time the entry process will remain open (seconds)")]
        public int Open { get; set; }
    }

    public class EventConditions
    {
        [JsonProperty(PropertyName = "The percentage of server players required for the event to start")]
        public float Percentage { get; set; }
        [JsonProperty(PropertyName = "The minimum amount of players on the server required to open the event")]
        public int OpenMin { get; set; }
    }

    public class LustyIntegration
    {
        [JsonProperty(PropertyName = "Show the destination icon on the map")]
        public bool Enabled { get; set; }
        [JsonProperty(PropertyName = "The amount of possible destinations to show")]
        public int DestinationAmount { get; set; }
    }

    public class GameOptions
    {
        [JsonProperty(PropertyName = "Broadcast the juggernauts position to chat every X seconds")]
        public int BroadcastEvery { get; set; }
        [JsonProperty(PropertyName = "Broadcast the juggernauts position to LustyMap ever X seconds")]
        public bool BroadcastEveryLM { get; set; }
        [JsonProperty(PropertyName = "The amount of time the juggernauts position will remain on LustyMap if enabled (seconds)")]
        public int BroadcastLMSeconds { get; set; }
        [JsonProperty(PropertyName = "Destination sphere darkness")]
        public int SphereDarkness { get; set; }
        [JsonProperty(PropertyName = "Blacklisted commands for event players")]
        public string[] CommandBlacklist { get; set; }
    }

    public class UISettings
    {
        [JsonProperty(PropertyName = "Show the juggernaut their position so they can navigate")]
        public bool ShowCoordsToJuggernaut { get; set; }
        [JsonProperty(PropertyName = "Display a timer showing how long the juggernaut has to get to their destination")]
        public bool ShowUITimer { get; set; }
        [JsonProperty(PropertyName = "The URL of the juggernaut icon")]
        public string IconUrl { get; set; }
        [JsonProperty(PropertyName = "Timer positioning")]
        public Position TimerPosition { get; set; }
        [JsonProperty(PropertyName = "Compass positioning")]
        public Position CompassPosition { get; set; }
        [JsonProperty(PropertyName = "UI background color (hex)")]
        public string UIBackgroundColor { get; set; }
        [JsonProperty(PropertyName = "UI opacity (0.0 - 1.0)")]
        public float UIOpacity { get; set; }
        public class Position
        {
            [JsonProperty(PropertyName = "Horizontal start position (left)")]
            public float XPosition { get; set; }
            [JsonProperty(PropertyName = "Vertical start position (bottom)")]
            public float YPosition { get; set; }
            [JsonProperty(PropertyName = "Horizontal dimensions")]
            public float XDimension { get; set; }
            [JsonProperty(PropertyName = "Vertical dimensions")]
            public float YDimension { get; set; }
        }

    }

    public Oxide.Core.VersionNumber Version { get; set; }
}

public class JuggernautOptions
{
    [JsonProperty(PropertyName = "Defense damage modifier")]
    public float DefenseMod { get; set; }
    [JsonProperty(PropertyName = "Attack damage modifier")]
    public float AttackMod { get; set; }
    [JsonProperty(PropertyName = "Can damage structures")]
    public bool DamageStructures { get; set; }
    [JsonProperty(PropertyName = "Start with full metabolism")]
    public bool ResetMetabolism { get; set; }
    [JsonProperty(PropertyName = "Disable landmine damage")]
    public bool NoLandmineDamage { get; set; }
    [JsonProperty(PropertyName = "Disable beartrap damage")]
    public bool NoBeartrapDamage { get; set; }
    [JsonProperty(PropertyName = "Disable fall damage")]
    public bool NoFallDamage { get; set; }
    [JsonProperty(PropertyName = "Inventory contents")]
    public List<InventoryItem> Inventory { get; set; }
    public class InventoryItem
    {
        [JsonProperty(PropertyName = "Item shortname")]
        public string Shortname { get; set; }
        [JsonProperty(PropertyName = "Amount of item")]
        public int Amount { get; set; }
        [JsonProperty(PropertyName = "Item skin ID")]
        public ulong SkinID { get; set; }
        [JsonProperty(PropertyName = "Is this item a blueprint?")]
        public bool IsBP { get; set; }
        [JsonProperty(PropertyName = "Container (main, wear or belt)")]
        public string Container { get; set; }
    }

}

public class InventoryItem
{
    [JsonProperty(PropertyName = "Item shortname")]
    public string Shortname { get; set; }
    [JsonProperty(PropertyName = "Amount of item")]
    public int Amount { get; set; }
    [JsonProperty(PropertyName = "Item skin ID")]
    public ulong SkinID { get; set; }
    [JsonProperty(PropertyName = "Is this item a blueprint?")]
    public bool IsBP { get; set; }
    [JsonProperty(PropertyName = "Container (main, wear or belt)")]
    public string Container { get; set; }
}

public class PrizeOptions
{
    [JsonProperty(PropertyName = "Allow players to loot juggernaut as a prize")]
    public bool Inventory { get; set; }
    [JsonProperty(PropertyName = "Use Economics money as a prize")]
    public bool Economics { get; set; }
    [JsonProperty(PropertyName = "Use ServerRewards money as a prize")]
    public bool ServerRewards { get; set; }
    [JsonProperty(PropertyName = "Monetary amount")]
    public int Amount { get; set; }
}

public class EventTimers
{
    [JsonProperty(PropertyName = "Amount of time to complete journey (seconds)")]
    public int Completion { get; set; }
    [JsonProperty(PropertyName = "Amount of time between events (seconds)")]
    public int Interval { get; set; }
    [JsonProperty(PropertyName = "Amount of time the entry process will remain open (seconds)")]
    public int Open { get; set; }
}

public class EventConditions
{
    [JsonProperty(PropertyName = "The percentage of server players required for the event to start")]
    public float Percentage { get; set; }
    [JsonProperty(PropertyName = "The minimum amount of players on the server required to open the event")]
    public int OpenMin { get; set; }
}

public class LustyIntegration
{
    [JsonProperty(PropertyName = "Show the destination icon on the map")]
    public bool Enabled { get; set; }
    [JsonProperty(PropertyName = "The amount of possible destinations to show")]
    public int DestinationAmount { get; set; }
}

public class GameOptions
{
    [JsonProperty(PropertyName = "Broadcast the juggernauts position to chat every X seconds")]
    public int BroadcastEvery { get; set; }
    [JsonProperty(PropertyName = "Broadcast the juggernauts position to LustyMap ever X seconds")]
    public bool BroadcastEveryLM { get; set; }
    [JsonProperty(PropertyName = "The amount of time the juggernauts position will remain on LustyMap if enabled (seconds)")]
    public int BroadcastLMSeconds { get; set; }
    [JsonProperty(PropertyName = "Destination sphere darkness")]
    public int SphereDarkness { get; set; }
    [JsonProperty(PropertyName = "Blacklisted commands for event players")]
    public string[] CommandBlacklist { get; set; }
}

public class UISettings
{
    [JsonProperty(PropertyName = "Show the juggernaut their position so they can navigate")]
    public bool ShowCoordsToJuggernaut { get; set; }
    [JsonProperty(PropertyName = "Display a timer showing how long the juggernaut has to get to their destination")]
    public bool ShowUITimer { get; set; }
    [JsonProperty(PropertyName = "The URL of the juggernaut icon")]
    public string IconUrl { get; set; }
    [JsonProperty(PropertyName = "Timer positioning")]
    public Position TimerPosition { get; set; }
    [JsonProperty(PropertyName = "Compass positioning")]
    public Position CompassPosition { get; set; }
    [JsonProperty(PropertyName = "UI background color (hex)")]
    public string UIBackgroundColor { get; set; }
    [JsonProperty(PropertyName = "UI opacity (0.0 - 1.0)")]
    public float UIOpacity { get; set; }
    public class Position
    {
        [JsonProperty(PropertyName = "Horizontal start position (left)")]
        public float XPosition { get; set; }
        [JsonProperty(PropertyName = "Vertical start position (bottom)")]
        public float YPosition { get; set; }
        [JsonProperty(PropertyName = "Horizontal dimensions")]
        public float XDimension { get; set; }
        [JsonProperty(PropertyName = "Vertical dimensions")]
        public float YDimension { get; set; }
    }

}

public class Position
{
    [JsonProperty(PropertyName = "Horizontal start position (left)")]
    public float XPosition { get; set; }
    [JsonProperty(PropertyName = "Vertical start position (bottom)")]
    public float YPosition { get; set; }
    [JsonProperty(PropertyName = "Horizontal dimensions")]
    public float XDimension { get; set; }
    [JsonProperty(PropertyName = "Vertical dimensions")]
    public float YDimension { get; set; }
}

private class Destinations
{
    public float x1;
    public float y1;
    public float z1;
    public float x2;
    public float y2;
    public float z2;
}

private class Points
{
    public int amount;
    public bool isRp;
}

private class StoredData
{
    public Dictionary<ulong, List<RestoreData.ItemData>> winnerRewards;
    public Dictionary<ulong, List<Points>> winnerMoney;
    public List<Destinations> destinations;
}


```

---

## KillStreaks

```csharp
using System.Collections.Generic;
using System;
using UnityEngine;
using Oxide.Core.Plugins;
using Oxide.Core;
using Oxide.Core.Configuration;
using System.Linq;
using Oxide.Game.Rust.Cui;

Oxide.Plugins
[Info("Killstreaks", "k1lly0u", "0.1.55", ResourceId = 1752)]
 class KillStreaks : RustPlugin
{
    [PluginReference]
     Plugin Airstrike;
    [PluginReference]
     Plugin Clans;
    [PluginReference]
     Plugin Friends;
    [PluginReference]
     Plugin EventManager;
    [PluginReference]
     Plugin Economics;
    [PluginReference]
     Plugin ServerRewards;
    private Dictionary<ulong, int> cachedData;
    private List<BaseHelicopter> activeHelis;
    private List<ulong> asGren;
    private List<ulong> ssGren;
    private List<ulong> arGren;
    private List<ulong> heGren;
    private List<ulong> mrtdm;
    private List<ulong> turret;
    private bool isSignal;
    private Dictionary<ulong, StreakType> activeGrenades;
    private Dictionary<int, StreakType> streakTypes;
     DataStorage data;
    private DynamicConfigFile KSData;
    private static Vector2 warningPos;
    private static Vector2 warningDim;
    private readonly int triggerMask;
     void OnServerInitialized();
    private void CheckDependencies();
    protected override void LoadDefaultConfig();
     void OnEntityDeath(BaseEntity entity, HitInfo hitinfo);
     void OnPlayerDisconnected(BasePlayer player);
     void OnExplosiveThrown(BasePlayer player, BaseEntity entity);
     void Unload();
    private bool HasPriv(BasePlayer player);
    private void ProcessKill(BasePlayer player, BasePlayer victim);
    private void ProcessDeath(BasePlayer player, HitInfo info, bool disconnected);
    private void ClearPlayerRewards(BasePlayer player);
    public string GetDeathType(BaseEntity entity);
    private void BroadcastToAll(string msg, string keyword);
    private void BroadcastToPlayer(BasePlayer player, string msg, string keyword);
    private void GUIToPlayer(BasePlayer player, string msg, string keyword);
    private bool IsClanmate(ulong playerId, ulong friendId);
    private List<BasePlayer> FindNearbyFriends(BasePlayer player);
    private bool IsFriend(ulong playerID, ulong friendID);
    private Item GiveSupplySignal();
    private void Deal(BasePlayer player);
    private void GiveRewardGrenade(BasePlayer player, StreakType type);
    private void CallAirstrike(Vector3 target, bool type);
    private void LaunchArtillery(Vector3 target);
    private void RocketSpread(Vector3 targetPos);
    private BaseEntity CreateRocket(Vector3 targetPos);
    private int HeliDistance;
    private static LayerMask GROUND_MASKS;
    private void CallHeli(Vector3 pos, int streaknum, bool onSmoke, BasePlayer player);
    private BaseEntity CreateHeli(Vector3 pos);
    private Vector3 calculateSpawnPos(Vector3 arenaPos);
    private float RandomRange(float distance, float difference);
    private void MoveEntity(BaseEntity entity, Vector3 pos);
    private void CheckDistance(BaseEntity entity, BasePlayer player);
    static Vector3 GetGroundPosition(Vector3 sourcePos);
    private void KillHeli();
    private void SetMartyrdom(BasePlayer player);
    private void ChooseRandomExp(Vector3 pos);
    private void dropGrenade(Vector3 deathPos);
    private void dropBeancan(Vector3 deathPos);
    private void dropExplosive(Vector3 deathPos);
    private void SendSupplyDrop(BasePlayer player, int streaknum);
    private void SpawnSignal(BasePlayer player);
    private void DropTurret(Vector3 pos, BasePlayer player);
    private AutoTurret CreateTurret(Vector3 targetPos);
    private void AssignTurretAuth(BasePlayer player, AutoTurret turret);
    private string GiveEconomics(BasePlayer player, int streaknum);
    private string GiveRP(BasePlayer player, int streaknum);
    private void dealDamage(Vector3 deathPos, float damage, float radius);
    [ChatCommand("ks")]
     void cmdTarget(BasePlayer player, string command, string[] args);
     bool isAuth(BasePlayer player);
     class KSUI : MonoBehaviour
    {
         int i;
        private BasePlayer player;
         void Awake();
        public static KSUI GetPlayer(BasePlayer player);
        public void UseUI(string msg, Vector2 pos, Vector2 dim, int size);
    }

    private void DestroyNotification(BasePlayer player, string msgNum);
    private void DestroyWarningMsg(BasePlayer player, string msgNum, int duration);
     bool Changed;
    static bool useFriendsAPI;
    static bool useClans;
    static bool useAirstrike;
    static bool broadcastMsg;
    static bool ignoreBuildPriv;
    static bool broadcastEnd;
    static int saveTimer;
    static float HeliBulletDamage;
    static float HeliHealth;
    static float MainRotorHealth;
    static float TailRotorHealth;
    static float HeliSpeed;
    static float HeliAccuracy;
    static float SpawnDistance;
    static string fontColor1;
    static string fontColor2;
    static float rocketInterval;
    static float rocketSpread;
    static int rocketAmount;
    static float grenadeRadius;
    static float grenadeDamage;
    static float beancanRadius;
    static float beancanDamage;
    static float explosiveRadius;
    static float explosiveDamage;
    static float nearbyRadius;
    static string turretAmmoTypeName;
    static int turretAmmoCount;
    private void LoadVariables();
    private void LoadConfigVariables();
    private void CheckCfg(string Key, T var);
    private void CheckCfgFloat(string Key, float var);
     object GetConfig(string menu, string datavalue, object defaultValue);
     void SaveData();
     void SaveLoop();
     void LoadData();
     void RegisterMessages();
     class DataStorage
    {
        public Dictionary<ulong, KSDATA> killStreakData;
        public Dictionary<int, Streaks> killStreaks;
        public DataStorage();
    }

     class KSDATA
    {
        public string Name;
        public int highestKS;
    }

     class Streaks
    {
        public string Message;
        public StreakType StreakType;
        public int Amount;
    }

     Dictionary<int, Streaks> ksDefault;
     Dictionary<string, string> messages;
}

 class KSUI : MonoBehaviour
{
     int i;
    private BasePlayer player;
     void Awake();
    public static KSUI GetPlayer(BasePlayer player);
    public void UseUI(string msg, Vector2 pos, Vector2 dim, int size);
}

 class DataStorage
{
    public Dictionary<ulong, KSDATA> killStreakData;
    public Dictionary<int, Streaks> killStreaks;
    public DataStorage();
}

 class KSDATA
{
    public string Name;
    public int highestKS;
}

 class Streaks
{
    public string Message;
    public StreakType StreakType;
    public int Amount;
}


```

---

## Kits

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
[Info("Kits", "OMHOMHOM", 0.1)]
 class Kits : RustPlugin
{
    private string Layer;
    private string LayerBlur;
    private string LayerBlurKitsInfo;
     object OnPlayerRespawned(BasePlayer player);
     void Loaded();
     void OnServerInitialized();
    private KitsInfo GetKitsInfo(BasePlayer player);
    private KitData GetKitData(KitsInfo kitsInfo, KitInfo kitInfo);
     void Unload();
    [ChatCommand("createkit")]
    private void CreateKit(BasePlayer player, string command, string[] args);
    [ConsoleCommand("UI_KITS")]
    private void cmdConsoleHandler(ConsoleSystem.Arg arg);
    [ChatCommand("kit")]
     void KitOpen(BasePlayer player, string command, string[] args);
    [ChatCommand("kits")]
     void KitsOpen(BasePlayer player, string command, string[] args);
    private List<KitInfo> GetKitsForPlayer(BasePlayer player);
    private void GiveItems(BasePlayer player, KitInfo kit);
    private void GiveItem(BasePlayer player, Item item, ItemContainer cont);
    private Item BuildItem(string ShortName, int Amount, ulong SkinID, float Condition, Weapon weapon, List<ItemContent> Content);
    private List<ItemInfo> GetPlayerItems(BasePlayer player);
    private ItemInfo ItemToKit(Item item, string container);
    private static string HexToRGB(string hex);
    protected override void SaveConfig();
    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    public Configuration _config;
    public class Configuration
    {
        [JsonProperty("Набор на респавне")]
        public List<ItemInfo> spawnKit;
        [JsonProperty("Наборы")]
        public List<KitInfo> kits;
    }

    public class KitInfo
    {
        [JsonProperty("Название")]
        public string kitName;
        [JsonProperty("Максимум использований")]
        public int maxUse;
        [JsonProperty("Кулдаун")]
        public double cooldownKit;
        [JsonProperty("Привилегия")]
        public string privilege;
        [JsonProperty("Предметы")]
        public List<ItemInfo> items;
    }

    public class ItemInfo
    {
        [JsonProperty("Позиция")]
        public int position;
        [JsonProperty("Shortname")]
        public string shortname;
        [JsonProperty("Количество")]
        public int amount;
        [JsonProperty("Место")]
        public string place;
        [JsonProperty("Скин")]
        public ulong skinID;
        [JsonProperty("Контейнер")]
        public List<ItemContent> Content { get; set; }
        [JsonProperty("Прочность")]
        public float Condition { get; set; }
        [JsonProperty("Оружие")]
        public Weapon Weapon { get; set; }
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

     class StoredData
    {
        public Dictionary<ulong, KitsInfo> players;
    }

     class KitsInfo
    {
        public Dictionary<string, KitData> kits;
    }

     class KitData
    {
        [JsonProperty("a")]
        public int amount;
        [JsonProperty("cd")]
        public double cooldown;
    }

     void SaveData();
     void LoadData();
     StoredData storedData;
    private static class TimeHelper
    {
        public static string FormatTime(TimeSpan time, int maxSubstr, string language);
        private static string Format(int units, string form1, string form2, string form3);
        private static DateTime Epoch;
        public static double GetTimeStamp();
    }

    public CuiRawImageComponent GetAvatarImageComponent(ulong user_id, string color);
    public CuiRawImageComponent GetImageComponent(string url, string shortName, string color);
    public CuiRawImageComponent GetItemImageComponent(string shortName);
    public bool AddImage(string url, string shortName);
}

public class Configuration
{
    [JsonProperty("Набор на респавне")]
    public List<ItemInfo> spawnKit;
    [JsonProperty("Наборы")]
    public List<KitInfo> kits;
}

public class KitInfo
{
    [JsonProperty("Название")]
    public string kitName;
    [JsonProperty("Максимум использований")]
    public int maxUse;
    [JsonProperty("Кулдаун")]
    public double cooldownKit;
    [JsonProperty("Привилегия")]
    public string privilege;
    [JsonProperty("Предметы")]
    public List<ItemInfo> items;
}

public class ItemInfo
{
    [JsonProperty("Позиция")]
    public int position;
    [JsonProperty("Shortname")]
    public string shortname;
    [JsonProperty("Количество")]
    public int amount;
    [JsonProperty("Место")]
    public string place;
    [JsonProperty("Скин")]
    public ulong skinID;
    [JsonProperty("Контейнер")]
    public List<ItemContent> Content { get; set; }
    [JsonProperty("Прочность")]
    public float Condition { get; set; }
    [JsonProperty("Оружие")]
    public Weapon Weapon { get; set; }
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

 class StoredData
{
    public Dictionary<ulong, KitsInfo> players;
}

 class KitsInfo
{
    public Dictionary<string, KitData> kits;
}

 class KitData
{
    [JsonProperty("a")]
    public int amount;
    [JsonProperty("cd")]
    public double cooldown;
}

private static class TimeHelper
{
    public static string FormatTime(TimeSpan time, int maxSubstr, string language);
    private static string Format(int units, string form1, string form2, string form3);
    private static DateTime Epoch;
    public static double GetTimeStamp();
}


```

---

## Kits (1)

```csharp
using Oxide.Game.Rust.Cui;
using System;
using System.Collections.Generic;
using System.Linq;
using UnityEngine;
using System.Globalization;
using Oxide.Core;
using System.IO;

Oxide.Plugins
[Info("Kits", "RustPlugin.ru", "3.2.51")]
 class Kits : RustPlugin
{
    private PluginConfig _config;
    private ImagesCache _imagesCache;
    private List<Kit> _kits;
    private Dictionary<ulong, Dictionary<string, KitData>> _kitsData;
    private Dictionary<BasePlayer, List<string>> _kitsGUI;
     class PluginConfig
    {
        public Position Position { get; set; }
        public Position CloseButtonPosition { get; set; }
        public Position CloseButtonPositionNext1 { get; set; }
        public Position CloseButtonPositionNext2 { get; set; }
        public Position CloseButtonPositionNext3 { get; set; }
        public string CloseButtonColor { get; set; }
        public string MainBackgroundColor { get; set; }
        public string DefaultKitImage { get; set; }
        public float MarginTop { get; set; }
        public float MarginBottom { get; set; }
        public float MarginBetween { get; set; }
        public float KitWidth { get; set; }
        public string DisableMaskColor { get; set; }
        public string KitBackgroundColor { get; set; }
        public List<string> CustomAutoKits;
        public ImageConfig Image { get; set; }
        public LabelConfig Label { get; set; }
        public LabelConfig Amount { get; set; }
        public LabelConfig Time { get; set; }
        public static PluginConfig CreateDefault();
        public class LabelConfig
        {
            public Position Position { get; set; }
            public string ForegroundColor { get; set; }
            public string BackgroundColor { get; set; }
            public int FontSize { get; set; }
            public TextAnchor TextAnchor { get; set; }
        }

        public class ImageConfig
        {
            public Position Position { get; set; }
            public string Color { get; set; }
            public string Png { get; set; }
        }

    }

    public class Kit
    {
        public string Name { get; set; }
        public string DisplayName { get; set; }
        public int Amount { get; set; }
        public double Cooldown { get; set; }
        public bool Hide { get; set; }
        public string Permission { get; set; }
        public List<KitItem> Items { get; set; }
        public string Png { get; set; }
    }

    public class KitItem
    {
        public string ShortName { get; set; }
        public int Amount { get; set; }
        public int Blueprint { get; set; }
        public ulong SkinID { get; set; }
        public string Container { get; set; }
        public float Condition { get; set; }
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

    public class ImagesCache : MonoBehaviour
    {
        private Dictionary<string, string> _images;
        public void Add(string name, string url);
        public string Get(string name);
    }

    protected override void LoadDefaultConfig();
     void OnPlayerRespawned(BasePlayer player);
    private void SaveKits();
    private void SaveData();
     void OnServerSave();
    private void LoadMessages();
    private void Loaded();
    private void Unload();
    private void OnServerInitialized();
    private void OnPlayerDisconnected(BasePlayer player);
    [ConsoleCommand("kit")]
    private void CommandConsoleKit(ConsoleSystem.Arg arg);
    [ChatCommand("kit")]
    private void CommandChatKit(BasePlayer player, string command, string[] args);
    private bool GiveKit(BasePlayer player, string kitname);
    private void KitCommandAdd(BasePlayer player, string kitname);
    private void KitCommandClone(BasePlayer player, string kitname);
    private void KitCommandRemove(BasePlayer player, string kitname);
    private void KitCommandList(BasePlayer player);
    private void KitCommandReset(BasePlayer player);
    private void KitCommandGive(BasePlayer player, BasePlayer foundPlayer, string kitname);
    private void GiveItems(BasePlayer player, Kit kit);
    private void GiveItem(BasePlayer player, Item item, ItemContainer cont);
    private Item BuildItem(string ShortName, int Amount, ulong SkinID, float Condition, int blueprintTarget, Weapon weapon, List<ItemContent> Content);
    private void TriggerUI(BasePlayer player);
    private void TriggerUI2(BasePlayer player);
    private void TriggerUI3(BasePlayer player);
    private void TriggerUI1(BasePlayer player);
    private void InitilizeUI(BasePlayer player);
    private void InitilizeUI2(BasePlayer player);
    private void InitilizeUI3(BasePlayer player);
    private void DestroyUI(BasePlayer player);
    private void RefreshCooldownKitsUI();
    private void InitilizeKitsUI(CuiElementContainer container, BasePlayer player);
    private void InitilizeKitsUINext(CuiElementContainer container, BasePlayer player);
    private void InitilizeKitsUI2(CuiElementContainer container, BasePlayer player);
    private void InitilizeKitImageUI(CuiElementContainer container, string kitname);
    private void InitilizeNameLabelUI(CuiElementContainer container, string kitname, string text);
    private void InitilizeMaskUI(CuiElementContainer container, string kitname);
    private void InitilizeAmountLabelUI(CuiElementContainer container, string kitname, string text);
    private void InitilizeButtonUI(CuiElementContainer container, string kitname);
    private void InitilizeCooldownLabelUI(CuiElementContainer container, string kitname, TimeSpan time);
    private KitData GetPlayerData(ulong userID, string name);
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

}

 class PluginConfig
{
    public Position Position { get; set; }
    public Position CloseButtonPosition { get; set; }
    public Position CloseButtonPositionNext1 { get; set; }
    public Position CloseButtonPositionNext2 { get; set; }
    public Position CloseButtonPositionNext3 { get; set; }
    public string CloseButtonColor { get; set; }
    public string MainBackgroundColor { get; set; }
    public string DefaultKitImage { get; set; }
    public float MarginTop { get; set; }
    public float MarginBottom { get; set; }
    public float MarginBetween { get; set; }
    public float KitWidth { get; set; }
    public string DisableMaskColor { get; set; }
    public string KitBackgroundColor { get; set; }
    public List<string> CustomAutoKits;
    public ImageConfig Image { get; set; }
    public LabelConfig Label { get; set; }
    public LabelConfig Amount { get; set; }
    public LabelConfig Time { get; set; }
    public static PluginConfig CreateDefault();
    public class LabelConfig
    {
        public Position Position { get; set; }
        public string ForegroundColor { get; set; }
        public string BackgroundColor { get; set; }
        public int FontSize { get; set; }
        public TextAnchor TextAnchor { get; set; }
    }

    public class ImageConfig
    {
        public Position Position { get; set; }
        public string Color { get; set; }
        public string Png { get; set; }
    }

}

public class LabelConfig
{
    public Position Position { get; set; }
    public string ForegroundColor { get; set; }
    public string BackgroundColor { get; set; }
    public int FontSize { get; set; }
    public TextAnchor TextAnchor { get; set; }
}

public class ImageConfig
{
    public Position Position { get; set; }
    public string Color { get; set; }
    public string Png { get; set; }
}

public class Kit
{
    public string Name { get; set; }
    public string DisplayName { get; set; }
    public int Amount { get; set; }
    public double Cooldown { get; set; }
    public bool Hide { get; set; }
    public string Permission { get; set; }
    public List<KitItem> Items { get; set; }
    public string Png { get; set; }
}

public class KitItem
{
    public string ShortName { get; set; }
    public int Amount { get; set; }
    public int Blueprint { get; set; }
    public ulong SkinID { get; set; }
    public string Container { get; set; }
    public float Condition { get; set; }
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

public class ImagesCache : MonoBehaviour
{
    private Dictionary<string, string> _images;
    public void Add(string name, string url);
    public string Get(string name);
}

private static class TimeExtensions
{
    public static string FormatShortTime(TimeSpan time);
    public static string FormatTime(TimeSpan time);
    private static string Format(int units, string form1, string form2, string form3);
}


```

---

## Kits (2)

```csharp
using Oxide.Game.Rust.Cui;
using System;
using System.Collections.Generic;
using System.Linq;
using UnityEngine;
using System.Globalization;
using Oxide.Core;
using System.IO;

Oxide.Plugins
[Info("Kits", "RustPlugin.ru", "3.2.4")]
 class Kits : RustPlugin
{
    public static Oxide.Core.Libraries.Permission perm { get; set; }
    private PluginConfig _config;
    private ImagesCache _imagesCache;
    private List<Kit> _kits;
    private Dictionary<ulong, Dictionary<string, KitData>> _kitsData;
    private Dictionary<BasePlayer, List<string>> _kitsGUI;
     class PluginConfig
    {
        public Position Position { get; set; }
        public Position CloseButtonPosition { get; set; }
        public Position CloseButtonPositionNext1 { get; set; }
        public Position CloseButtonPositionNext2 { get; set; }
        public Position CloseButtonPositionNext3 { get; set; }
        public string CloseButtonColor { get; set; }
        public string MainBackgroundColor { get; set; }
        public string DefaultKitImage { get; set; }
        public float MarginTop { get; set; }
        public float MarginBottom { get; set; }
        public float MarginBetween { get; set; }
        public float KitWidth { get; set; }
        public string DisableMaskColor { get; set; }
        public string KitBackgroundColor { get; set; }
        public List<string> CustomAutoKits;
        public ImageConfig Image { get; set; }
        public LabelConfig Label { get; set; }
        public LabelConfig Amount { get; set; }
        public LabelConfig Time { get; set; }
        public static PluginConfig CreateDefault();
        public class LabelConfig
        {
            public Position Position { get; set; }
            public string ForegroundColor { get; set; }
            public string BackgroundColor { get; set; }
            public int FontSize { get; set; }
            public TextAnchor TextAnchor { get; set; }
        }

        public class ImageConfig
        {
            public Position Position { get; set; }
            public string Color { get; set; }
            public string Png { get; set; }
        }

    }

    public class Kit
    {
        public string Name { get; set; }
        public string DisplayName { get; set; }
        public int Amount { get; set; }
        public double Cooldown { get; set; }
        public bool Hide { get; set; }
        public string Permission { get; set; }
        public List<KitItem> Items { get; set; }
        public string Png { get; set; }
    }

    public class KitItem
    {
        public string ShortName { get; set; }
        public int Amount { get; set; }
        public int Blueprint { get; set; }
        public ulong SkinID { get; set; }
        public string Container { get; set; }
        public float Condition { get; set; }
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

    public class ImagesCache : MonoBehaviour
    {
        private Dictionary<string, string> _images;
        public void Add(string name, string url);
        public string Get(string name);
    }

    protected override void LoadDefaultConfig();
     void OnPlayerRespawned(BasePlayer player);
    private void SaveKits();
    private void SaveData();
     void OnServerSave();
    private void LoadMessages();
    private void Loaded();
    private void Init();
    private void Unload();
    private void OnServerInitialized();
    private void OnPlayerDisconnected(BasePlayer player);
    [ConsoleCommand("kit")]
    private void CommandConsoleKit(ConsoleSystem.Arg arg);
    [ChatCommand("kit")]
    private void CommandChatKit(BasePlayer player, string command, string[] args);
    private bool GiveKit(BasePlayer player, string kitname);
    private void KitCommandAdd(BasePlayer player, string kitname);
    private void KitCommandClone(BasePlayer player, string kitname);
    private void KitCommandRemove(BasePlayer player, string kitname);
    private void KitCommandList(BasePlayer player);
    private void KitCommandReset(BasePlayer player);
    private void KitCommandGive(BasePlayer player, BasePlayer foundPlayer, string kitname);
    private void GiveItems(BasePlayer player, Kit kit);
    private void GiveItem(PlayerInventory inv, Item item, ItemContainer cont);
    private Item BuildItem(string ShortName, int Amount, ulong SkinID, float Condition, int blueprintTarget, Weapon weapon, List<ItemContent> Content);
    private void TriggerUI(BasePlayer player);
    private void TriggerUI2(BasePlayer player);
    private void TriggerUI3(BasePlayer player);
    private void TriggerUI1(BasePlayer player);
    private void InitilizeUI(BasePlayer player);
    private void InitilizeUI2(BasePlayer player);
    private void InitilizeUI3(BasePlayer player);
    private void DestroyUI(BasePlayer player);
    private void RefreshCooldownKitsUI();
    private void InitilizeKitsUI(CuiElementContainer container, BasePlayer player);
    private void InitilizeKitsUINext(CuiElementContainer container, BasePlayer player);
    private void InitilizeKitsUI2(CuiElementContainer container, BasePlayer player);
    private void InitilizeKitImageUI(CuiElementContainer container, string kitname);
    private void InitilizeNameLabelUI(CuiElementContainer container, string kitname, string text);
    private void InitilizeMaskUI(CuiElementContainer container, string kitname);
    private void InitilizeAmountLabelUI(CuiElementContainer container, string kitname, string text);
    private void InitilizeButtonUI(CuiElementContainer container, string kitname);
    private void InitilizeCooldownLabelUI(CuiElementContainer container, string kitname, TimeSpan time);
    private KitData GetPlayerData(ulong userID, string name);
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

}

 class PluginConfig
{
    public Position Position { get; set; }
    public Position CloseButtonPosition { get; set; }
    public Position CloseButtonPositionNext1 { get; set; }
    public Position CloseButtonPositionNext2 { get; set; }
    public Position CloseButtonPositionNext3 { get; set; }
    public string CloseButtonColor { get; set; }
    public string MainBackgroundColor { get; set; }
    public string DefaultKitImage { get; set; }
    public float MarginTop { get; set; }
    public float MarginBottom { get; set; }
    public float MarginBetween { get; set; }
    public float KitWidth { get; set; }
    public string DisableMaskColor { get; set; }
    public string KitBackgroundColor { get; set; }
    public List<string> CustomAutoKits;
    public ImageConfig Image { get; set; }
    public LabelConfig Label { get; set; }
    public LabelConfig Amount { get; set; }
    public LabelConfig Time { get; set; }
    public static PluginConfig CreateDefault();
    public class LabelConfig
    {
        public Position Position { get; set; }
        public string ForegroundColor { get; set; }
        public string BackgroundColor { get; set; }
        public int FontSize { get; set; }
        public TextAnchor TextAnchor { get; set; }
    }

    public class ImageConfig
    {
        public Position Position { get; set; }
        public string Color { get; set; }
        public string Png { get; set; }
    }

}

public class LabelConfig
{
    public Position Position { get; set; }
    public string ForegroundColor { get; set; }
    public string BackgroundColor { get; set; }
    public int FontSize { get; set; }
    public TextAnchor TextAnchor { get; set; }
}

public class ImageConfig
{
    public Position Position { get; set; }
    public string Color { get; set; }
    public string Png { get; set; }
}

public class Kit
{
    public string Name { get; set; }
    public string DisplayName { get; set; }
    public int Amount { get; set; }
    public double Cooldown { get; set; }
    public bool Hide { get; set; }
    public string Permission { get; set; }
    public List<KitItem> Items { get; set; }
    public string Png { get; set; }
}

public class KitItem
{
    public string ShortName { get; set; }
    public int Amount { get; set; }
    public int Blueprint { get; set; }
    public ulong SkinID { get; set; }
    public string Container { get; set; }
    public float Condition { get; set; }
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

public class ImagesCache : MonoBehaviour
{
    private Dictionary<string, string> _images;
    public void Add(string name, string url);
    public string Get(string name);
}

private static class TimeExtensions
{
    public static string FormatShortTime(TimeSpan time);
    public static string FormatTime(TimeSpan time);
    private static string Format(int units, string form1, string form2, string form3);
}


```

---

## Kits1

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
[Info("Kits", "Orange", "1.0.1")]
[Description("Kits with features for your server! Made by Orange#0900")]
public class Kits1 : RustPlugin
{
    [PluginReference]
    private Plugin ImageLibrary;
    private void AddImage(string name, string url);
    private string GetImage(string name);
    private class Kit
    {
        [JsonProperty(PropertyName = "1. Name")]
        public string name;
        [JsonProperty(PropertyName = "2. Display name")]
        public string displayName;
        [JsonProperty(PropertyName = "3. Permission")]
        public string permission;
        [JsonProperty(PropertyName = "4. Cooldown")]
        public int cooldown;
        [JsonProperty(PropertyName = "5. Wipe-block time")]
        public int block;
        [JsonProperty(PropertyName = "6. Icon")]
        public string url;
        [JsonProperty(PropertyName = "7. Description")]
        public string description;
        [JsonProperty(PropertyName = "8. Max uses")]
        public int uses;
        [JsonProperty(PropertyName = "9. Give on respawn")]
        public bool auto;
        [JsonProperty(PropertyName = "Items:")]
        public List<BaseItem> items;
    }

    private class BaseItem
    {
        public string shortname;
        public int amount;
        public ulong skin;
        public string container;
        public int position;
    }

    private void Init();
    private void Loaded();
    private void Unload();
    private void OnNewSave();
    private void OnPlayerRespawned(BasePlayer player);
    private static ConfigData config;
    private class ConfigData
    {
        [JsonProperty(PropertyName = "1. Command")]
        public string command;
        [JsonProperty(PropertyName = "Kit list:")]
        public List<Kit> kits;
    }

    private ConfigData GetDefaultConfig();
    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private const string filename;
    private PlayerData data;
    private class PlayerData
    {
        public Dictionary<ulong, Dictionary<string, double>> cooldowns;
        public Dictionary<ulong, Dictionary<string, int>> uses;
    }

    private void LoadData();
    private void SaveData();
    private Dictionary<string, string> EN;
    private void message(BasePlayer player, string key, object[] args);
    private void Command(BasePlayer player, string command, string[] args);
    private void Command(ConsoleSystem.Arg arg);
    private void Command(BasePlayer player, string[] args);
    private List<BaseItem> GetItems(BasePlayer player);
    private void AddKit(BasePlayer player, string name);
    private void RemoveKit(BasePlayer player, string name);
    private void TryGiveKit(BasePlayer player, string name);
    private void GiveKit(BasePlayer player, Kit kit);
    private void GiveKit(BasePlayer player, string name);
    private Kit GetKit(string name);
    private bool CanUse(BasePlayer player);
    private double Now();
    private int Passed(double a);
    private double SaveTime();
    private int GetBlockTime(int time);
    private int GetCooldown(ulong id, string name, int cooldown);
    private int GetUses(ulong id, string name);
    private bool HasPermission(string id, string name);
    private string GetTimeString(int time);
    private const string elemHud;
    private const string elemMain;
    private const string elemInfo;
    private const string outlineColor;
    private const string outlineDistance;
    private void CreatePanel(BasePlayer player);
    private void CreateKits(BasePlayer player);
    private void ShowInfo(BasePlayer player, string name);
}

private class Kit
{
    [JsonProperty(PropertyName = "1. Name")]
    public string name;
    [JsonProperty(PropertyName = "2. Display name")]
    public string displayName;
    [JsonProperty(PropertyName = "3. Permission")]
    public string permission;
    [JsonProperty(PropertyName = "4. Cooldown")]
    public int cooldown;
    [JsonProperty(PropertyName = "5. Wipe-block time")]
    public int block;
    [JsonProperty(PropertyName = "6. Icon")]
    public string url;
    [JsonProperty(PropertyName = "7. Description")]
    public string description;
    [JsonProperty(PropertyName = "8. Max uses")]
    public int uses;
    [JsonProperty(PropertyName = "9. Give on respawn")]
    public bool auto;
    [JsonProperty(PropertyName = "Items:")]
    public List<BaseItem> items;
}

private class BaseItem
{
    public string shortname;
    public int amount;
    public ulong skin;
    public string container;
    public int position;
}

private class ConfigData
{
    [JsonProperty(PropertyName = "1. Command")]
    public string command;
    [JsonProperty(PropertyName = "Kit list:")]
    public List<Kit> kits;
}

private class PlayerData
{
    public Dictionary<ulong, Dictionary<string, double>> cooldowns;
    public Dictionary<ulong, Dictionary<string, int>> uses;
}


```

---

## Kits2

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
[Info("Kits", "By Denzoff", "1.1.2")]
 class Kits : RustPlugin
{
    [PluginReference]
     Plugin ImageLibrary;
    static Kits ins;
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
    private void cmdConsoleGiveKit(ConsoleSystem.Arg arg);
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
    private void TriggerUI(BasePlayer player, int page);
    private void InitilizeUI(BasePlayer player, int page);
    private void DestroyUI(BasePlayer player);
    private void RefreshCooldownKitsUI();
    private string FormatTime(double time);
    private void InitilizeKitsUI(BasePlayer player, int page);
    private void InitilizeCooldown(CuiElementContainer container, BasePlayer player, Kit kit, int page);
    [ConsoleCommand("kit.drawkitinfo")]
     void cmdDrawKitInfo(ConsoleSystem.Arg args);
     void DrawKitInfo(BasePlayer player, Kit kit, int page);
    private int? ChangeSelect(int x);
    private void SendEffectToPlayer2(BasePlayer player, string effectPrefab);
    [ConsoleCommand("UIkits_adminSettings")]
     void adminSettingsKit(ConsoleSystem.Arg args);
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

## Kits3

```csharp
using System;
using System.Collections;
using System.Collections.Generic;
using System.ComponentModel;
using System.Globalization;
using System.IO;
using System.Reflection;
using Facepunch;
using Newtonsoft.Json;
using Newtonsoft.Json.Converters;
using Newtonsoft.Json.Linq;
using Oxide.Core;
using Oxide.Core.Libraries;
using Oxide.Core.Libraries.Covalence;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using Oxide.Plugins.KitsExtensionMethods;
using UnityEngine;
using UnityEngine.UI;
using Random = UnityEngine.Random;

Oxide.Plugins
[Info("Kits", "https://discord.gg/TrJ7jnS233", "2.0.7")]
public class Kits : RustPlugin
{
    [PluginReference]
    private Plugin ImageLibrary;
    private Plugin ServerPanel;
    private Plugin CopyPaste;
    private Plugin Notify;
    private Plugin UINotify;
    private Plugin NoEscape;
    private static Kits _instance;
    private const string PERM_ADMIN;
    private const string Layer;
    private const string InfoLayer;
    private const string EditingLayer;
    private const string ModalLayer;
    private bool _enabledImageLibrary;
    private const bool LangRu;
    private readonly Dictionary<ulong, Dictionary<string, object>> _kitEditing;
    private readonly Dictionary<ulong, Dictionary<string, object>> _itemEditing;
    private readonly Dictionary<string, List<(int itemID, string shortName)>> _itemsCategories;
    private readonly HashSet<ulong> _playersToRemoveFromUpdate;
    private (bool spStatus, int categoryID) _serverPanelCategory;
    private int _lastKitID;
    private Configuration _config;
    private class Configuration
    {
        [JsonProperty(PropertyName = LangRu ? "Автовайп" : "Automatic wipe on wipe")]
        public bool AutoWipe;
        [JsonProperty(PropertyName =
				LangRu
					? "Сохранять выданные наборы (с помощью команды kits.givekit) при вайпе?"
					: "Save given kits (via the kits.givekit command) on wipe?")]
        public bool SaveGivenKitsOnWipe;
        [JsonProperty(PropertyName = LangRu ? "Стандартный Цвет Набора" : "Default Kit Color")]
        public string KitColor;
        [JsonProperty(PropertyName = LangRu ? "Работать с Notify?" : "Work with Notify?")]
        public bool UseNotify;
        [JsonProperty(PropertyName =
				LangRu ? "Работать с NoEscape? (Raid/Combat block)" : "Use NoEscape? (Raid/Combat block)")]
        public bool UseNoEscape;
        [JsonProperty(PropertyName = LangRu ? "Использовать блокировку рейда?" : "Use Raid Blocked?")]
        public bool UseRaidBlock;
        [JsonProperty(PropertyName = LangRu ? "Использовать блокировку комбата?" : "Use Combat Blocked?")]
        public bool UseCombatBlock;
        [JsonProperty(PropertyName =
				LangRu ? "Могут ли администраторы редактировать предметы? (по флаг)" : "Can admins edit? (by flag)")]
        public bool FlagAdmin;
        [JsonProperty(PropertyName = LangRu ? "Whitelist для NoEscape" : "Whitelist for NoEscape",
				ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<string> NoEscapeWhiteList;
        [JsonProperty(PropertyName = LangRu ? "Команды" : "Commands",
				ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public string[] Commands;
        [JsonProperty(PropertyName = "Economy")]
        public EconomyConf Economy;
        [JsonProperty(PropertyName = LangRu ? "Настройки Редкости" : "Rarity Settings",
				ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<RarityColor> RarityColors;
        [JsonProperty(PropertyName = LangRu ? "Авто наборы" : "Auto Kits",
				ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<string> AutoKits;
        [JsonProperty(PropertyName = LangRu ? "Получение автонабора 1 раз?" : "Getting an auto kit 1 time?")]
        public bool OnceAutoKit;
        [JsonProperty(PropertyName =
				LangRu ? "Разрешить включать/выключать автокиты?" : "Allow to enable/disable autokit?")]
        public bool UseChangeAutoKit;
        [JsonProperty(PropertyName =
				LangRu ? "Разрешение для включения/выключения автокитов" : "Permission to enable/disable autokit")]
        public string ChangeAutoKitPermission;
        [JsonProperty(PropertyName = LangRu ? "Игнорировать проверку автокита?" : "Ignore auto-kit checking?")]
        public bool IgnoreAutoKitChecking;
        [JsonProperty(PropertyName =
				LangRu
					? "Обновлять меню наборов во время операций с разрешениями?"
					: "Update the kits menu during permissions operations?")]
        public bool OnPermissionsUpdate;
        [JsonProperty(PropertyName = "Logs")]
        public LogInfo Logs;
        [JsonProperty(PropertyName =
				LangRu ? "Показывать оповещение об отсутвии прав?" : "Show No Permission Description?")]
        public bool ShowNoPermDescription;
        [JsonProperty(PropertyName = LangRu ? "Показывать все наборы?" : "Show All Kits?")]
        public bool ShowAllKits;
        [JsonProperty(PropertyName =
				LangRu
					? "Показывать набор, когда закончилось количество использований?"
					: "Show the kit when the number of uses is up?")]
        public bool ShowUsesEnd;
        [JsonProperty(PropertyName = "CopyPaste Parameters",
				ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<string> CopyPasteParameters;
        [JsonProperty(PropertyName = LangRu ? "Блокировка в Building Block?" : "Block in Building Block?")]
        public bool BlockBuilding;
        [JsonProperty(PropertyName = LangRu ? "NPC Наборы" : "NPC Kits",
				ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public Dictionary<string, NpcKitsData> NpcKits;
        [JsonProperty(PropertyName = LangRu ? "Описание" : "Description")]
        public MenuDescription Description;
        [JsonProperty(PropertyName = LangRu ? "Информация о наборе" : "Info Kit Description")]
        public DescriptionSettings InfoKitDescription;
        [JsonProperty(PropertyName = LangRu ? "Интерфейс" : "Interface")]
        public UserInterface UI;
        [JsonProperty(PropertyName = LangRu ? "Интерфейс для меню" : "Menu UI")]
        public UserInterface MenuUI;
        [JsonProperty(PropertyName = LangRu ? "Пользовательские названия для наборов" : "Custom Title for Kits")]
        public CustomTitles CustomTitles;
        [JsonProperty(PropertyName = LangRu ? "Наборы, скрытые в интерфейсе" : "Kits hidden in the interface",
				ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public string[] KitsHidden;
        public VersionNumber Version;
    }

    private class LogoConf : InterfacePosition
    {
        [JsonProperty(PropertyName = LangRu ? "Включено" : "Enabled")]
        public bool Enabled;
        [JsonProperty(PropertyName = LangRu ? "Изображение" : "Image")]
        public string Image;
    }

    private class CustomTitles
    {
        [JsonProperty(PropertyName = LangRu ? "Включено" : "Enabled")]
        public bool Enabled;
        [JsonProperty(
				PropertyName = LangRu
					? "Названия наборов (название набора – настройки)"
					: "Kit Titles (kit name – settings)",
				ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public Dictionary<string, KitTitle> KitTitles;
        public class TitleConf
        {
            [JsonProperty(PropertyName = LangRu ? "Включено" : "Enabled")]
            public bool Enabled;
            [JsonProperty(PropertyName = LangRu ? "Текст (язык - текст)" : "Text (language - text)",
					ObjectCreationHandling = ObjectCreationHandling.Replace)]
            public Dictionary<string, string> Messages;
            public string GetMessage(BasePlayer player);
        }

        public class KitTitle
        {
            [JsonProperty(PropertyName = LangRu ? "Включено" : "Enabled")]
            public bool Enabled;
            [JsonProperty(PropertyName = LangRu ? "Название (ключ – настройки)" : "Titles (key – settings)",
					ObjectCreationHandling = ObjectCreationHandling.Replace)]
            public Dictionary<string, TitleConf> Titles;
        }

    }

    private class EconomyConf
    {
        [JsonProperty(PropertyName = "Type (Plugin/Item)")]
        [JsonConverter(typeof(StringEnumConverter))]
        public EconomyType Type;
        [JsonProperty(PropertyName = "Plugin name")]
        public string Plug;
        [JsonProperty(PropertyName = "Balance add hook")]
        public string AddHook;
        [JsonProperty(PropertyName = "Balance remove hook")]
        public string RemoveHook;
        [JsonProperty(PropertyName = "Balance show hook")]
        public string BalanceHook;
        [JsonProperty(PropertyName = "ShortName")]
        public string ShortName;
        [JsonProperty(PropertyName = "Display Name (empty - default)")]
        public string DisplayName;
        [JsonProperty(PropertyName = "Skin")]
        public ulong Skin;
        public double ShowBalance(BasePlayer player);
        public void AddBalance(BasePlayer player, double amount);
        public bool RemoveBalance(BasePlayer player, double amount);
        private int PlayerItemsCount(BasePlayer player, string shortname, ulong skin);
        private int ItemCount(List<Item> items, string shortname, ulong skin);
        private static void Take(List<Item> itemList, string shortname, ulong skinId, int iAmount);
        private Item ToItem(int amount);
    }

    private class UserInterface
    {
        [JsonProperty(PropertyName = LangRu ? "Стиль" : "Style")]
        [JsonConverter(typeof(StringEnumConverter))]
        public InterfaceStyle Style;
        [JsonProperty(PropertyName = LangRu ? "Высота" : "Height")]
        public float Height;
        [JsonProperty(PropertyName = LangRu ? "Ширина" : "Width")]
        public float Width;
        [JsonProperty(PropertyName = LangRu ? "Высота набора" : "Kit Height")]
        public float KitHeight;
        [JsonProperty(PropertyName = LangRu ? "Ширина набора" : "Kit Width")]
        public float KitWidth;
        [JsonProperty(PropertyName = LangRu ? "Отступ" : "Margin")]
        public float Margin;
        [JsonProperty(PropertyName = LangRu ? "Кол-во наборов на строке" : "Kits On String")]
        public int KitsOnString;
        [JsonProperty(PropertyName = LangRu ? "Кол-во строк" : "Strings")]
        public int Strings;
        [JsonProperty(PropertyName = LangRu ? "Отступ по слева" : "Left Indent")]
        public float LeftIndent;
        [JsonProperty(PropertyName = LangRu ? "Отступ по вертикали" : "Y Indent")]
        public float YIndent;
        [JsonProperty(PropertyName = LangRu ? "Настройки доступности набора" : "Kit Available Settings")]
        public InterfacePosition KitAvailable;
        [JsonProperty(PropertyName = LangRu ? "Настройки количества набора" : "Kit Amount Settings")]
        public KitAmountSettings KitAmount;
        [JsonProperty(PropertyName = LangRu ? "Настройки КД набора" : "Kit Cooldown Settings")]
        public InterfacePosition KitCooldown;
        [JsonProperty(PropertyName = LangRu ? "Настройки продажи" : "Kit Sale Settings")]
        public InterfacePosition KitSale;
        [JsonProperty(PropertyName =
				LangRu ? "Настройка КД набора (с количеством)" : "Kit Cooldown Settings (with amount)")]
        public InterfacePosition KitAmountCooldown;
        [JsonProperty(PropertyName = LangRu ? "Настройки отсутствия прав" : "No Permission Settings")]
        public InterfacePosition NoPermission;
        [JsonProperty(PropertyName =
				LangRu ? "Закрывать интерфейс после получения набора?" : "Close the interface after receiving a kit?")]
        public bool CloseAfterReceive;
        [JsonProperty(PropertyName = LangRu ? "Настройки логотипа" : "Logo Settings")]
        public LogoConf Logo;
        [JsonProperty(PropertyName = LangRu ? "Настройки заголовка" : "Header Settings")]
        public KitsPanelHeaderUI HeaderPanel;
        [JsonProperty(PropertyName = LangRu ? "Настройки панели китов" : "Kits Panel Settings")]
        public KitsPanelContentUI ContentPanel;
        [JsonProperty(PropertyName = LangRu ? "Настройки кита" : "Kit Settings")]
        public KitsPanelKitUI KitPanel;
        [JsonProperty(PropertyName = LangRu ? "Цвет 1" : "Color 1")]
        public IColor ColorOne;
        [JsonProperty(PropertyName = LangRu ? "Цвет 2" : "Color 2")]
        public IColor ColorTwo;
        [JsonProperty(PropertyName = LangRu ? "Цвет 3" : "Color 3")]
        public IColor ColorThree;
        [JsonProperty(PropertyName = LangRu ? "Цвет 4" : "Color 4")]
        public IColor ColorFour;
        [JsonProperty(PropertyName = LangRu ? "Цвет 5" : "Color 5")]
        public IColor ColorFive;
        [JsonProperty(PropertyName = LangRu ? "Цвет 6" : "Color 6")]
        public IColor ColorSix;
        [JsonProperty(PropertyName = LangRu ? "Цвет 7" : "Color 7")]
        public IColor ColorSeven;
        [JsonProperty(PropertyName = LangRu ? "Цвет Red" : "Color Red")]
        public IColor ColorRed;
        [JsonProperty(PropertyName = LangRu ? "Цвет White" : "Color White")]
        public IColor ColorWhite;
        [JsonProperty(PropertyName = LangRu ? "Цвет фона" : "Background Color")]
        public IColor ColorBackground;
        public class KitsPanelHeaderUI
        {
            [JsonProperty(PropertyName = "Background")]
            public ImageSettings Background;
            [JsonProperty(PropertyName = "Title")]
            public TextSettings Title;
            [JsonProperty(PropertyName = "Show Line?")]
            public bool ShowLine;
            [JsonProperty(PropertyName = "Line")]
            public ImageSettings Line;
            [JsonProperty(PropertyName = "Close Button")]
            public ButtonSettings ButtonClose;
        }

        public class KitsPanelContentUI
        {
            [JsonProperty(PropertyName = "Background")]
            public ImageSettings Background;
            [JsonProperty(PropertyName = "Button Back")]
            public ButtonSettings ButtonBack;
            [JsonProperty(PropertyName = "Button Next")]
            public ButtonSettings ButtonNext;
            [JsonProperty(PropertyName = "Button Create Kit")]
            public ButtonSettings ButtonCreateKit;
            [JsonProperty(PropertyName = "Show All Kits Checkbox")]
            public CheckBoxSettings CheckboxShowAllKits;
        }

        public class KitsPanelKitUI
        {
            [JsonProperty(PropertyName = "Background")]
            public ImageSettings Background;
            [JsonProperty(PropertyName = "Show Name?")]
            public bool ShowName;
            [JsonProperty(PropertyName = "Kit Name")]
            public TextSettings KitName;
            [JsonProperty(PropertyName = "Show Number?")]
            public bool ShowNumber;
            [JsonProperty(PropertyName = "Kit Number")]
            public TextSettings KitNumber;
            [JsonProperty(PropertyName = "Kit Image")]
            public InterfacePosition KitImage;
            [JsonProperty(PropertyName = "Kit Button Take")]
            public ButtonSettings KitButtonTake;
            [JsonProperty(PropertyName = "Kit Button Take (when show info)")]
            public ButtonSettings KitButtonTakeWhenShowInfo;
            [JsonProperty(PropertyName = "Kit Button Info")]
            public ButtonSettings KitButtonInfo;
            [JsonProperty(PropertyName = "Show Line?")]
            public bool ShowLine;
            [JsonProperty(PropertyName = "Kit Line")]
            public InterfacePosition KitLine;
        }

        public static UserInterface GenerateFullscreenTemplateOldStyle();
        public static UserInterface GenerateMenuTemplateRustV1();
        public static UserInterface GenerateMenuTemplateRustV2();
        public static UserInterface GenerateFullscreenTemplateRust();
    }

    private class KitAmountSettings : InterfacePosition
    {
        [JsonProperty(PropertyName = LangRu ? "Ширина" : "Width")]
        public float Width;
    }

    private class NpcKitsData
    {
        [JsonProperty(PropertyName = LangRu ? "Описание" : "Description")]
        public string Description;
        [JsonProperty(PropertyName = LangRu ? "Наборы" : "Kits",
				ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<string> Kits;
    }

    public class ScrollViewUI
    {
        [JsonProperty(PropertyName = "Scroll Type")]
        [JsonConverter(typeof(StringEnumConverter))]
        public ScrollType ScrollType;
        [JsonProperty(PropertyName = "Movement Type")]
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

    public class CheckBoxSettings
    {
        [JsonProperty(PropertyName = "Background")]
        public ImageSettings Background;
        [JsonProperty(PropertyName = "Checkbox")]
        public ButtonSettings CheckboxButton;
        [JsonProperty(PropertyName = "Checkbox Size")]
        public float CheckboxSize;
        [JsonProperty(PropertyName = "Checkbox Color")]
        public IColor CheckboxColor;
        [JsonProperty(PropertyName = "Title")]
        public TextSettings Title;
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

    private class DescriptionSettings : InterfacePosition
    {
        [JsonProperty(PropertyName = LangRu ? "Включено" : "Enabled")]
        public bool Enabled;
        [JsonProperty(PropertyName = LangRu ? "Цвет фона" : "Background Color")]
        public IColor Color;
        [JsonProperty(PropertyName = "Font Size")]
        public int FontSize;
        [JsonProperty(PropertyName = "Font")]
        public string Font;
        [JsonProperty(PropertyName = "Align")]
        [JsonConverter(typeof(StringEnumConverter))]
        public TextAnchor Align;
        [JsonProperty(PropertyName = "Text Color")]
        public IColor TextColor;
        public void Get(CuiElementContainer container, string parent, string name, string description);
    }

    private class MenuDescription : DescriptionSettings
    {
        [JsonProperty(PropertyName = LangRu ? "Описание" : "Description")]
        public string Description;
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

    private class LogInfo
    {
        [JsonProperty(PropertyName = "To Console")]
        public bool Console;
        [JsonProperty(PropertyName = "To File")]
        public bool File;
    }

    private class RarityColor
    {
        [JsonProperty(PropertyName = LangRu ? "Шанс" : "Chance")]
        public int Chance;
        [JsonProperty(PropertyName = LangRu ? "Цвет" : "Color")]
        public string Color;
        public RarityColor(int chance, string color);
    }

    protected override void LoadConfig();
    protected override void SaveConfig();
    protected override void LoadDefaultConfig();
    private void UpdateConfigValues();
    private List<ulong> _disablesAutoKits;
    private void LoadDisabledAutoKits();
    private void SaveDisabledAutoKits();
    private PluginData _data;
    private void LoadKits();
    private void SaveKits();
    private class PluginData
    {
        [JsonProperty(PropertyName = "Kits", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<Kit> Kits;
    }

    private class Kit
    {
        [JsonIgnore]
        public int ID;
        [JsonProperty(PropertyName = "Name")]
        public string Name;
        [JsonProperty(PropertyName = "Display Name")]
        public string DisplayName;
        [JsonProperty(PropertyName = "Color")]
        public string Color;
        [JsonProperty(PropertyName = "Permission")]
        public string Permission;
        [JsonProperty(PropertyName = "Description")]
        public string Description;
        [JsonProperty(PropertyName = "Image")]
        public string Image;
        [JsonProperty(PropertyName = "Hide")]
        public bool Hide;
        [JsonProperty(PropertyName = "ShowInfo")]
        [DefaultValue(true)]
        public bool ShowInfo;
        [JsonProperty(PropertyName = "Amount")]
        public int Amount;
        [JsonProperty(PropertyName = "Cooldown")]
        public double Cooldown;
        [JsonProperty(PropertyName = "Wipe Block")]
        public double CooldownAfterWipe;
        [JsonProperty(PropertyName = "Use Building")]
        public bool UseBuilding;
        [JsonProperty(PropertyName = "Building")]
        public string Building;
        [JsonProperty(PropertyName = "Enable sale")]
        public bool Sale;
        [JsonProperty(PropertyName = "Selling price")]
        public int Price;
        [JsonProperty(PropertyName = "Items", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<KitItem> Items;
        [JsonProperty(PropertyName = "Use commands on receiving?")]
        public bool UseCommandsOnReceiving;
        [JsonProperty(PropertyName = "Commands on receiving (via '|')",
				ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public string CommandsOnReceiving;
        [JsonProperty(PropertyName = "Use slot for backpack?")]
        public bool UseSlotForBackpack;
        public void Get(BasePlayer player);
        public void UseCommands(BasePlayer player);
        public void MoveRight();
        public void MoveLeft();
        [JsonIgnore]
        public Dictionary<int, KitItem> dictMainItems;
        [JsonIgnore]
        public Dictionary<int, KitItem> dictBeltItems;
        [JsonIgnore]
        public Dictionary<int, KitItem> dictWearItems;
        [JsonIgnore]
        public int beltCount;
        [JsonIgnore]
        public int wearCount;
        [JsonIgnore]
        public int mainCount;
        public void Update();
        private void LoadContainers();
        public KitItem GetItemByContainerAndPosition(string container, int position);
        [JsonIgnore]
        private JObject _jObject;
        [JsonIgnore]
        internal JObject ToJObject { get; set; }
        private void GenerateJObject();
    }

    private class KitItem
    {
        [JsonProperty(PropertyName = "Type")]
        [JsonConverter(typeof(StringEnumConverter))]
        public KitItemType Type;
        [JsonProperty(PropertyName = "Command")]
        public string Command;
        [JsonProperty(PropertyName = "ShortName")]
        public string ShortName;
        [JsonProperty(PropertyName = "DisplayName")]
        public string DisplayName;
        [JsonProperty(PropertyName = "Amount")]
        public int Amount;
        [JsonProperty(PropertyName = "Blueprint")]
        public int Blueprint;
        [JsonProperty(PropertyName = "SkinID")]
        public ulong SkinID;
        [JsonProperty(PropertyName = "Container")]
        public string Container;
        [JsonProperty(PropertyName = "Condition")]
        public float Condition;
        [JsonProperty(PropertyName = "Chance")]
        public int Chance;
        [JsonProperty(PropertyName = "Position", DefaultValueHandling = DefaultValueHandling.Populate)]
        [DefaultValue(-1)]
        public int Position;
        [JsonProperty(PropertyName = "Image")]
        public string Image;
        [JsonProperty(PropertyName = "Weapon")]
        public Weapon Weapon;
        [JsonProperty(PropertyName = "Content", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<ItemContent> Content;
        [JsonProperty(PropertyName = "Text")]
        public string Text;
        public void Get(BasePlayer player);
        private void ToCommand(BasePlayer player);
        private static void GiveItem(BasePlayer player, Item item, ItemContainer cont);
        [JsonIgnore]
        private ItemDefinition _itemDefinition;
        [JsonIgnore]
        public ItemDefinition ItemDefinition { get; set; }
        public Item BuildItem();
        public static KitItem FromOld(ItemData item, string container);
        [JsonIgnore]
        private int _itemId;
        [JsonIgnore]
        public int itemId { get; set; }
        [JsonIgnore]
        private ICuiComponent _image;
        public CuiElement GetImage(string aMin, string aMax, string oMin, string oMax, string parent, string name);
        private void GenerateNewImage();
        private void UpdateItemID();
        public void Update();
        [JsonIgnore]
        private JObject _jObject;
        [JsonIgnore]
        public JObject ToJObject { get; set; }
        private void GenerateJObject();
    }

    private class Weapon
    {
        public string ammoType;
        public int ammoAmount;
    }

    private class ItemContent
    {
        public string ShortName;
        public float Condition;
        public int Amount;
    }

    private void LoadData();
    private void Init();
    private void OnServerInitialized();
    private void Unload();
    private void OnNewSave(string filename);
    private void OnServerPanelCategoryPage(BasePlayer player, int category, int page);
    private void OnServerPanelClosed(BasePlayer player);
    private void OnReceiveCategoryInfo(int categoryID);
    private void OnPlayerRespawned(BasePlayer player);
    private void OnPlayerDisconnected(BasePlayer player);
    private void OnPlayerDeath(BasePlayer player, HitInfo info);
    private void OnUseNPC(BasePlayer npc, BasePlayer player);
    private void OnPluginLoaded(Plugin plugin);
    private void OnPluginUnloaded(Plugin plugin);
    private void OnUserPermissionGranted(string id, string permName);
    private void OnUserPermissionRevoked(string id, string permName);
    private void CmdOpenKits(IPlayer cov, string command, string[] args);
    [ConsoleCommand("UI_Kits")]
    private void CmdKitsConsole(ConsoleSystem.Arg arg);
    [ConsoleCommand("kits.reset")]
    private void CmdKitsReset(ConsoleSystem.Arg arg);
    [ConsoleCommand("kits.give")]
    private void CmdKitsGive(ConsoleSystem.Arg arg);
    [ConsoleCommand("kits.givekit")]
    private void CmdKitsGiveKit(ConsoleSystem.Arg arg);
    [ConsoleCommand("kits.template")]
    private void CmdKitsSetTemplate(ConsoleSystem.Arg arg);
    private const ulong DEFAULT_MAIN_TARGET_ID;
    private const int DEFAULT_MAIN_PAGE;
    private const bool DEFAULT_MAIN_SHOW_ALL;
    private void MainUi(BasePlayer player, bool first);
    private void MainKitsDescriptionUI(BasePlayer player, CuiElementContainer container);
    private void MainKitsContentUI(BasePlayer player, CuiElementContainer container);
    private void KitUI(BasePlayer player, CuiElementContainer container, Kit kit, int currentPage, int totalAmount, int kitIndex, bool isAdmin, bool kitsShowAll);
    private void MainKitsHeader(BasePlayer player, CuiElementContainer container);
    private void MainKitsBackground(BasePlayer player, CuiElementContainer container);
    private void InfoKitUi(BasePlayer player, Kit kit);
    private void InfoKitItemUI(BasePlayer player, Kit kit, CuiElementContainer container, int slot, string itemContainer);
    private const float Kit_Editing_Height;
    private const float Kit_Editing_Width;
    private const float Kit_Editing_Margin_X;
    private const float Kit_Editing_Margin_Y;
    private void EditingKitUi(BasePlayer player, bool creating, int kitId);
    private void EditingItemUi(BasePlayer player, int kitId, int slot, string itemContainer, bool First);
    private void SelectItem(BasePlayer player, int kitId, int slot, string itemContainer, string selectedCategory, int page, string input);
    private static void UpdateUI(BasePlayer player, Action<CuiElementContainer> callback);
    private static void ShowGridUI(CuiElementContainer container, int startIndex, int count, int itemsOnString, float marginX, float marginY, float itemWidth, float itemHeight, float offsetX, float offsetY, float aMinX, float aMaxX, float aMinY, float aMaxY, string backgroundColor, string parent, Func<int, string> panelName, Func<int, string> destroyName, Action<int> callback);
    private void AddMoveButton(CuiElementContainer container, string layerParent, string direction, int yOffset, string moveCommand);
    private void ErrorUi(BasePlayer player, string msg);
    private void EditFieldUi(BasePlayer player, CuiElementContainer container, string parent, string name, string oMin, string oMax, string command, string label, object fieldValue);
    private void EditBoolFieldUi(BasePlayer player, CuiElementContainer container, string parent, string name, string oMin, string oMax, string command, string label, bool value);
    private void CheckBoxUi(CuiElementContainer container, string parent, string name, string aMin, string aMax, string oMin, string oMax, bool enabled, string command, string text, string color, int outlineSize);
    private void InfoItemUi(CuiElementContainer container, BasePlayer player, int slot, string oMin, string oMax, Kit kit, KitItem kitItem, int total, string itemContainer);
    private void RefreshKitUi(CuiElementContainer container, BasePlayer player, Kit kit);
    private bool GiveKitToPlayer(BasePlayer player, Kit kit, bool force, bool chat, bool usingUI);
    private void HandleKitRedeemedUI(BasePlayer player, bool usingUI);
    private void UpdatePlayerData(BasePlayer player, Kit kit, bool force);
    private bool CanPlayerRedeemKit(BasePlayer player, Kit kit, bool force, bool chat);
    private (bool Success, string ErrorMessage) CheckConditionsForKit(BasePlayer player, Kit kit, PlayerData.KitData playerData);
    private void ProcessAutoKit(BasePlayer player, Kit kit);
    private const int _itemsPerTick;
    private IEnumerator GiveKitItems(BasePlayer player, Kit kit);
    private void FastGiveKitItems(BasePlayer player, Kit kit);
    private double GetCooldown(double cooldown, BasePlayer player);
    private List<KitItem> GetPlayerItems(BasePlayer player);
    private KitItem ItemToKit(Item item, string container);
    private readonly Dictionary<ulong, OpenedKits> _openKITS;
    private class OpenedKits
    {
        public BasePlayer Player;
        public bool useMainUI;
        public ulong npcID;
        public OpenedKits(BasePlayer player, ulong targetID, bool mainUI);
        public List<Kit> availableKits;
        public List<Kit> kitsToUpdate;
        public void SetKitsToUpdate(List<Kit> kits);
        public void UpdateKitsToUpdate();
        public void UpdateKits(bool first);
        public int GetTotalKitsAmount();
        public int currentPage;
        public bool showAll;
        public void OnChangeCurrentPage(int page);
        public void OnChangeShowAll(bool newShowAll);
    }

    private OpenedKits GetOpenedKits(BasePlayer player, bool mainUI);
    private bool TryCreateOpenedKits(BasePlayer player, OpenedKits openedKits, bool mainUI);
    private bool IsOpenedKits(BasePlayer player);
    private bool RemoveOpenedKits(BasePlayer player);
    private bool RemoveOpenedKits(ulong userID);
    private OpenedKits SetOpenedKits(BasePlayer player, ulong targetID, int page, bool showAll);
    private Dictionary<string, int> _kitByName;
    private Dictionary<int, int> _kitByID;
    private Kit FindKitByName(string name);
    private Kit FindKitByID(int id);
    private bool TryFindKitByID(int id, Kit kit);
    private Coroutine _wipePlayers;
    private IEnumerator StartOnAllPlayers(string[] players, Action<string> callback, Action onFinish);
    private void DoWipePlayers(Action<int> callback);
    private void CacheKits();
    private void DoRemoveKit(Kit kit);
    private void RegisterPermissions();
    private void RegisterCommands();
    private void LoadServerPanel();
    private bool RaidBlocked(BasePlayer player);
    private bool CombatBlocked(BasePlayer player);
    private void StopEditing(BasePlayer player);
    private void FillCategories();
    private void UpdatePlayerCooldownsHandler();
    private void FixItemsPositions();
    private string GetImage(string name);
    private void LoadImages();
    private void BroadcastILNotInstalled();
    private static void RegisterImage(string image, Dictionary<string, string> imagesList);
    private static string HexToCuiColor(string hex, float alpha);
    private static string FormatShortTime(TimeSpan time);
    private static void CreateOutLine(CuiElementContainer container, string parent, string color, float size);
    private List<Kit> GetAvailableKitList(BasePlayer player, string targetId, bool showAll, bool checkAmount, bool gui);
    private List<Kit> GetAvailableKits(BasePlayer player, bool checkOpenedKits);
    private bool IsKitAvailable(BasePlayer player, string targetId, bool checkAmount, bool gui, Kit kit, string ignoredTargetID);
    private List<Kit> GetAutoKits(BasePlayer player);
    private int SecondsFromWipe();
    private double LeftWipeBlockTime(double cooldown);
    private double UnBlockTime(double amount);
    private static double GetCurrentTime();
    private bool IsAdmin(BasePlayer player);
    private void UpdateOpenedUI(string id, string permName);
    private void Log(BasePlayer player, string kitname);
    private const string UI_MeventRust_InfoKit_Title;
    private const string NoILError;
    private const string KitShowInfo;
    private const string KitYouHave;
    private const string EditRemoveField;
    private const string ChangeAutoKitOn;
    private const string ChangeAutoKitOff;
    private const string NoEscapeCombatBlocked;
    private const string NoEscapeRaidBlocked;
    private const string NotMoney;
    private const string PriceFormat;
    private const string KitExist;
    private const string KitNotExist;
    private const string KitRemoved;
    private const string AccessDenied;
    private const string KitLimit;
    private const string KitCooldown;
    private const string KitCreate;
    private const string KitClaimed;
    private const string NotEnoughSpace;
    private const string NotifyTitle;
    private const string Close;
    private const string MainTitle;
    private const string Back;
    private const string Next;
    private const string NotAvailableKits;
    private const string CreateKit;
    private const string ListKits;
    private const string ShowAll;
    private const string KitInfo;
    private const string KitTake;
    private const string ComeBack;
    private const string Edit;
    private const string ContainerMain;
    private const string ContainerWear;
    private const string ContainerBelt;
    private const string CreateOrEditKit;
    private const string EnableKit;
    private const string AutoKit;
    private const string EnabledSale;
    private const string SaveKit;
    private const string CopyItems;
    private const string RemoveKit;
    private const string EditingTitle;
    private const string ItemName;
    private const string CmdName;
    private const string BtnSelect;
    private const string BluePrint;
    private const string BtnSave;
    private const string ItemSearch;
    private const string BtnClose;
    private const string KitAvailableTitle;
    private const string KitsList;
    private const string KitsHelp;
    private const string KitNotFound;
    private const string RemoveItem;
    private const string NoPermission;
    private const string BuildError;
    private const string BBlocked;
    private const string NoPermissionDescription;
    protected override void LoadDefaultMessages();
    private string Msg(string key, string userid, object[] obj);
    private string Msg(BasePlayer player, string key, object[] obj);
    private void Reply(BasePlayer player, string key, object[] obj);
    private string Msg(BasePlayer player, string kitName, string key, object[] obj);
    private void SendNotify(BasePlayer player, string key, int type, object[] obj);
    private void SendNotifyOrUI(BasePlayer player, string key, int type, bool chat, object[] obj);
    private void SendMessageToNotifyOrUI(BasePlayer player, string message, int type, bool chat);
    private bool TryClaimKit(BasePlayer player, string name, bool usingUI);
    private void GetKitNames(List<string> list);
    private string[] GetAllKits();
    private object GetKitInfo(string name);
    private string[] GetKitContents(string name);
    private double GetKitCooldown(string name);
    private double PlayerKitCooldown(ulong ID, string name);
    private int KitMax(string name);
    private double PlayerKitMax(ulong ID, string name);
    private string KitImage(string name);
    private string GetKitImage(string name);
    private string GetKitDescription(string name);
    private int GetKitMaxUses(string name);
    private int GetPlayerKitUses(ulong userId, string name);
    private int GetPlayerKitUses(string userId, string name);
    private void SetPlayerKitUses(ulong userId, string name, int amount);
    private double GetPlayerKitCooldown(string userId, string name);
    private double GetPlayerKitCooldown(ulong userId, string name);
    private bool IsKitCooldown(Kit kit, PlayerData.KitData playerData, double currentTime, bool cooldown, bool wipeBlock);
    private TimeSpan GetCooldownTimeRemaining(Kit kit, PlayerData.KitData playerData, double currentTime, bool isCooldown);
    private bool GiveKit(BasePlayer player, string name, bool usingUI);
    private bool isKit(string name);
    private bool IsKit(string name);
    private bool HasKitAccess(string userId, string name);
    private int GetPlayerKitAmount(string userId, string name);
    private JObject GetKitObject(string name);
    private IEnumerable<Item> CreateKitItems(string name);
    private CuiElementContainer API_OpenPlugin(BasePlayer player);
    [ConsoleCommand("kits.convert")]
    private void OldKitsConvert(ConsoleSystem.Arg arg);
    private class OldData
    {
        [JsonProperty]
        public Dictionary<string, OldKitsData> _kits;
    }

    private class OldKitsData
    {
        public string Name;
        public string Description;
        public string RequiredPermission;
        public int MaximumUses;
        public int RequiredAuth;
        public int Cooldown;
        public int Cost;
        public bool IsHidden;
        public string CopyPasteFile;
        public string KitImage;
        public ItemData[] MainItems;
        public ItemData[] WearItems;
        public ItemData[] BeltItems;
    }

    private class ItemData
    {
        public string Shortname;
        public ulong Skin;
        public int Amount;
        public float Condition;
        public float MaxCondition;
        public int Ammo;
        public string Ammotype;
        public int Position;
        public int Frequency;
        public string BlueprintShortname;
        public ItemData[] Contents;
    }

    private void StartConvertOldData();
    private Dictionary<ulong, Dictionary<string, OldKitData>> LoadOldData();
    private void ConvertOldData(Dictionary<ulong, Dictionary<string, OldKitData>> players);
    private class OldKitData
    {
        public int Amount;
        public double Cooldown;
        public int HasAmount;
    }

    private Dictionary<string, PlayerData> _usersData;
    private class PlayerData
    {
        [JsonProperty(PropertyName = "Kits Data", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public Dictionary<string, KitData> Kits;
        public class KitData
        {
            [JsonProperty(PropertyName = "Amount")]
            public int Amount;
            [JsonProperty(PropertyName = "Cooldown")]
            public double Cooldown;
            [JsonProperty(PropertyName = "HasAmount")]
            public int HasAmount;
        }

        private static string BaseFolder();
        public static PlayerData GetOrLoad(string userId);
        public static KitData GetOrLoadKitData(string userId, string kitName, bool addKit);
        private static PlayerData GetOrLoad(string baseFolder, string userId, bool load);
        public static PlayerData GetOrCreate(string userId);
        public static KitData GetOrCreateKitData(string userId, string kitName);
        public static void Save();
        public static void Save(string userId);
        public static void SaveAndUnload(string userId);
        public static void Unload(string userId);
        public static string[] GetFiles();
        public static string[] GetFiles(string baseFolder);
        private static PlayerData ReadOnlyObject(string name);
        public static void DoWipe(string userId, bool isWipe);
        public static void StartAll(Action<PlayerData> action);
        public static List<PlayerData> GetAll();
        public static PlayerData GetNotLoad(string userId);
        public static KitData GetNotLoadKitData(string userId, string kitName);
    }

}

private class Configuration
{
    [JsonProperty(PropertyName = LangRu ? "Автовайп" : "Automatic wipe on wipe")]
    public bool AutoWipe;
    [JsonProperty(PropertyName =
				LangRu
					? "Сохранять выданные наборы (с помощью команды kits.givekit) при вайпе?"
					: "Save given kits (via the kits.givekit command) on wipe?")]
    public bool SaveGivenKitsOnWipe;
    [JsonProperty(PropertyName = LangRu ? "Стандартный Цвет Набора" : "Default Kit Color")]
    public string KitColor;
    [JsonProperty(PropertyName = LangRu ? "Работать с Notify?" : "Work with Notify?")]
    public bool UseNotify;
    [JsonProperty(PropertyName =
				LangRu ? "Работать с NoEscape? (Raid/Combat block)" : "Use NoEscape? (Raid/Combat block)")]
    public bool UseNoEscape;
    [JsonProperty(PropertyName = LangRu ? "Использовать блокировку рейда?" : "Use Raid Blocked?")]
    public bool UseRaidBlock;
    [JsonProperty(PropertyName = LangRu ? "Использовать блокировку комбата?" : "Use Combat Blocked?")]
    public bool UseCombatBlock;
    [JsonProperty(PropertyName =
				LangRu ? "Могут ли администраторы редактировать предметы? (по флаг)" : "Can admins edit? (by flag)")]
    public bool FlagAdmin;
    [JsonProperty(PropertyName = LangRu ? "Whitelist для NoEscape" : "Whitelist for NoEscape",
				ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<string> NoEscapeWhiteList;
    [JsonProperty(PropertyName = LangRu ? "Команды" : "Commands",
				ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public string[] Commands;
    [JsonProperty(PropertyName = "Economy")]
    public EconomyConf Economy;
    [JsonProperty(PropertyName = LangRu ? "Настройки Редкости" : "Rarity Settings",
				ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<RarityColor> RarityColors;
    [JsonProperty(PropertyName = LangRu ? "Авто наборы" : "Auto Kits",
				ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<string> AutoKits;
    [JsonProperty(PropertyName = LangRu ? "Получение автонабора 1 раз?" : "Getting an auto kit 1 time?")]
    public bool OnceAutoKit;
    [JsonProperty(PropertyName =
				LangRu ? "Разрешить включать/выключать автокиты?" : "Allow to enable/disable autokit?")]
    public bool UseChangeAutoKit;
    [JsonProperty(PropertyName =
				LangRu ? "Разрешение для включения/выключения автокитов" : "Permission to enable/disable autokit")]
    public string ChangeAutoKitPermission;
    [JsonProperty(PropertyName = LangRu ? "Игнорировать проверку автокита?" : "Ignore auto-kit checking?")]
    public bool IgnoreAutoKitChecking;
    [JsonProperty(PropertyName =
				LangRu
					? "Обновлять меню наборов во время операций с разрешениями?"
					: "Update the kits menu during permissions operations?")]
    public bool OnPermissionsUpdate;
    [JsonProperty(PropertyName = "Logs")]
    public LogInfo Logs;
    [JsonProperty(PropertyName =
				LangRu ? "Показывать оповещение об отсутвии прав?" : "Show No Permission Description?")]
    public bool ShowNoPermDescription;
    [JsonProperty(PropertyName = LangRu ? "Показывать все наборы?" : "Show All Kits?")]
    public bool ShowAllKits;
    [JsonProperty(PropertyName =
				LangRu
					? "Показывать набор, когда закончилось количество использований?"
					: "Show the kit when the number of uses is up?")]
    public bool ShowUsesEnd;
    [JsonProperty(PropertyName = "CopyPaste Parameters",
				ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<string> CopyPasteParameters;
    [JsonProperty(PropertyName = LangRu ? "Блокировка в Building Block?" : "Block in Building Block?")]
    public bool BlockBuilding;
    [JsonProperty(PropertyName = LangRu ? "NPC Наборы" : "NPC Kits",
				ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public Dictionary<string, NpcKitsData> NpcKits;
    [JsonProperty(PropertyName = LangRu ? "Описание" : "Description")]
    public MenuDescription Description;
    [JsonProperty(PropertyName = LangRu ? "Информация о наборе" : "Info Kit Description")]
    public DescriptionSettings InfoKitDescription;
    [JsonProperty(PropertyName = LangRu ? "Интерфейс" : "Interface")]
    public UserInterface UI;
    [JsonProperty(PropertyName = LangRu ? "Интерфейс для меню" : "Menu UI")]
    public UserInterface MenuUI;
    [JsonProperty(PropertyName = LangRu ? "Пользовательские названия для наборов" : "Custom Title for Kits")]
    public CustomTitles CustomTitles;
    [JsonProperty(PropertyName = LangRu ? "Наборы, скрытые в интерфейсе" : "Kits hidden in the interface",
				ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public string[] KitsHidden;
    public VersionNumber Version;
}

private class LogoConf : InterfacePosition
{
    [JsonProperty(PropertyName = LangRu ? "Включено" : "Enabled")]
    public bool Enabled;
    [JsonProperty(PropertyName = LangRu ? "Изображение" : "Image")]
    public string Image;
}

private class CustomTitles
{
    [JsonProperty(PropertyName = LangRu ? "Включено" : "Enabled")]
    public bool Enabled;
    [JsonProperty(
				PropertyName = LangRu
					? "Названия наборов (название набора – настройки)"
					: "Kit Titles (kit name – settings)",
				ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public Dictionary<string, KitTitle> KitTitles;
    public class TitleConf
    {
        [JsonProperty(PropertyName = LangRu ? "Включено" : "Enabled")]
        public bool Enabled;
        [JsonProperty(PropertyName = LangRu ? "Текст (язык - текст)" : "Text (language - text)",
					ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public Dictionary<string, string> Messages;
        public string GetMessage(BasePlayer player);
    }

    public class KitTitle
    {
        [JsonProperty(PropertyName = LangRu ? "Включено" : "Enabled")]
        public bool Enabled;
        [JsonProperty(PropertyName = LangRu ? "Название (ключ – настройки)" : "Titles (key – settings)",
					ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public Dictionary<string, TitleConf> Titles;
    }

}

public class TitleConf
{
    [JsonProperty(PropertyName = LangRu ? "Включено" : "Enabled")]
    public bool Enabled;
    [JsonProperty(PropertyName = LangRu ? "Текст (язык - текст)" : "Text (language - text)",
					ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public Dictionary<string, string> Messages;
    public string GetMessage(BasePlayer player);
}

public class KitTitle
{
    [JsonProperty(PropertyName = LangRu ? "Включено" : "Enabled")]
    public bool Enabled;
    [JsonProperty(PropertyName = LangRu ? "Название (ключ – настройки)" : "Titles (key – settings)",
					ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public Dictionary<string, TitleConf> Titles;
}

private class EconomyConf
{
    [JsonProperty(PropertyName = "Type (Plugin/Item)")]
    [JsonConverter(typeof(StringEnumConverter))]
    public EconomyType Type;
    [JsonProperty(PropertyName = "Plugin name")]
    public string Plug;
    [JsonProperty(PropertyName = "Balance add hook")]
    public string AddHook;
    [JsonProperty(PropertyName = "Balance remove hook")]
    public string RemoveHook;
    [JsonProperty(PropertyName = "Balance show hook")]
    public string BalanceHook;
    [JsonProperty(PropertyName = "ShortName")]
    public string ShortName;
    [JsonProperty(PropertyName = "Display Name (empty - default)")]
    public string DisplayName;
    [JsonProperty(PropertyName = "Skin")]
    public ulong Skin;
    public double ShowBalance(BasePlayer player);
    public void AddBalance(BasePlayer player, double amount);
    public bool RemoveBalance(BasePlayer player, double amount);
    private int PlayerItemsCount(BasePlayer player, string shortname, ulong skin);
    private int ItemCount(List<Item> items, string shortname, ulong skin);
    private static void Take(List<Item> itemList, string shortname, ulong skinId, int iAmount);
    private Item ToItem(int amount);
}

private class UserInterface
{
    [JsonProperty(PropertyName = LangRu ? "Стиль" : "Style")]
    [JsonConverter(typeof(StringEnumConverter))]
    public InterfaceStyle Style;
    [JsonProperty(PropertyName = LangRu ? "Высота" : "Height")]
    public float Height;
    [JsonProperty(PropertyName = LangRu ? "Ширина" : "Width")]
    public float Width;
    [JsonProperty(PropertyName = LangRu ? "Высота набора" : "Kit Height")]
    public float KitHeight;
    [JsonProperty(PropertyName = LangRu ? "Ширина набора" : "Kit Width")]
    public float KitWidth;
    [JsonProperty(PropertyName = LangRu ? "Отступ" : "Margin")]
    public float Margin;
    [JsonProperty(PropertyName = LangRu ? "Кол-во наборов на строке" : "Kits On String")]
    public int KitsOnString;
    [JsonProperty(PropertyName = LangRu ? "Кол-во строк" : "Strings")]
    public int Strings;
    [JsonProperty(PropertyName = LangRu ? "Отступ по слева" : "Left Indent")]
    public float LeftIndent;
    [JsonProperty(PropertyName = LangRu ? "Отступ по вертикали" : "Y Indent")]
    public float YIndent;
    [JsonProperty(PropertyName = LangRu ? "Настройки доступности набора" : "Kit Available Settings")]
    public InterfacePosition KitAvailable;
    [JsonProperty(PropertyName = LangRu ? "Настройки количества набора" : "Kit Amount Settings")]
    public KitAmountSettings KitAmount;
    [JsonProperty(PropertyName = LangRu ? "Настройки КД набора" : "Kit Cooldown Settings")]
    public InterfacePosition KitCooldown;
    [JsonProperty(PropertyName = LangRu ? "Настройки продажи" : "Kit Sale Settings")]
    public InterfacePosition KitSale;
    [JsonProperty(PropertyName =
				LangRu ? "Настройка КД набора (с количеством)" : "Kit Cooldown Settings (with amount)")]
    public InterfacePosition KitAmountCooldown;
    [JsonProperty(PropertyName = LangRu ? "Настройки отсутствия прав" : "No Permission Settings")]
    public InterfacePosition NoPermission;
    [JsonProperty(PropertyName =
				LangRu ? "Закрывать интерфейс после получения набора?" : "Close the interface after receiving a kit?")]
    public bool CloseAfterReceive;
    [JsonProperty(PropertyName = LangRu ? "Настройки логотипа" : "Logo Settings")]
    public LogoConf Logo;
    [JsonProperty(PropertyName = LangRu ? "Настройки заголовка" : "Header Settings")]
    public KitsPanelHeaderUI HeaderPanel;
    [JsonProperty(PropertyName = LangRu ? "Настройки панели китов" : "Kits Panel Settings")]
    public KitsPanelContentUI ContentPanel;
    [JsonProperty(PropertyName = LangRu ? "Настройки кита" : "Kit Settings")]
    public KitsPanelKitUI KitPanel;
    [JsonProperty(PropertyName = LangRu ? "Цвет 1" : "Color 1")]
    public IColor ColorOne;
    [JsonProperty(PropertyName = LangRu ? "Цвет 2" : "Color 2")]
    public IColor ColorTwo;
    [JsonProperty(PropertyName = LangRu ? "Цвет 3" : "Color 3")]
    public IColor ColorThree;
    [JsonProperty(PropertyName = LangRu ? "Цвет 4" : "Color 4")]
    public IColor ColorFour;
    [JsonProperty(PropertyName = LangRu ? "Цвет 5" : "Color 5")]
    public IColor ColorFive;
    [JsonProperty(PropertyName = LangRu ? "Цвет 6" : "Color 6")]
    public IColor ColorSix;
    [JsonProperty(PropertyName = LangRu ? "Цвет 7" : "Color 7")]
    public IColor ColorSeven;
    [JsonProperty(PropertyName = LangRu ? "Цвет Red" : "Color Red")]
    public IColor ColorRed;
    [JsonProperty(PropertyName = LangRu ? "Цвет White" : "Color White")]
    public IColor ColorWhite;
    [JsonProperty(PropertyName = LangRu ? "Цвет фона" : "Background Color")]
    public IColor ColorBackground;
    public class KitsPanelHeaderUI
    {
        [JsonProperty(PropertyName = "Background")]
        public ImageSettings Background;
        [JsonProperty(PropertyName = "Title")]
        public TextSettings Title;
        [JsonProperty(PropertyName = "Show Line?")]
        public bool ShowLine;
        [JsonProperty(PropertyName = "Line")]
        public ImageSettings Line;
        [JsonProperty(PropertyName = "Close Button")]
        public ButtonSettings ButtonClose;
    }

    public class KitsPanelContentUI
    {
        [JsonProperty(PropertyName = "Background")]
        public ImageSettings Background;
        [JsonProperty(PropertyName = "Button Back")]
        public ButtonSettings ButtonBack;
        [JsonProperty(PropertyName = "Button Next")]
        public ButtonSettings ButtonNext;
        [JsonProperty(PropertyName = "Button Create Kit")]
        public ButtonSettings ButtonCreateKit;
        [JsonProperty(PropertyName = "Show All Kits Checkbox")]
        public CheckBoxSettings CheckboxShowAllKits;
    }

    public class KitsPanelKitUI
    {
        [JsonProperty(PropertyName = "Background")]
        public ImageSettings Background;
        [JsonProperty(PropertyName = "Show Name?")]
        public bool ShowName;
        [JsonProperty(PropertyName = "Kit Name")]
        public TextSettings KitName;
        [JsonProperty(PropertyName = "Show Number?")]
        public bool ShowNumber;
        [JsonProperty(PropertyName = "Kit Number")]
        public TextSettings KitNumber;
        [JsonProperty(PropertyName = "Kit Image")]
        public InterfacePosition KitImage;
        [JsonProperty(PropertyName = "Kit Button Take")]
        public ButtonSettings KitButtonTake;
        [JsonProperty(PropertyName = "Kit Button Take (when show info)")]
        public ButtonSettings KitButtonTakeWhenShowInfo;
        [JsonProperty(PropertyName = "Kit Button Info")]
        public ButtonSettings KitButtonInfo;
        [JsonProperty(PropertyName = "Show Line?")]
        public bool ShowLine;
        [JsonProperty(PropertyName = "Kit Line")]
        public InterfacePosition KitLine;
    }

    public static UserInterface GenerateFullscreenTemplateOldStyle();
    public static UserInterface GenerateMenuTemplateRustV1();
    public static UserInterface GenerateMenuTemplateRustV2();
    public static UserInterface GenerateFullscreenTemplateRust();
}

public class KitsPanelHeaderUI
{
    [JsonProperty(PropertyName = "Background")]
    public ImageSettings Background;
    [JsonProperty(PropertyName = "Title")]
    public TextSettings Title;
    [JsonProperty(PropertyName = "Show Line?")]
    public bool ShowLine;
    [JsonProperty(PropertyName = "Line")]
    public ImageSettings Line;
    [JsonProperty(PropertyName = "Close Button")]
    public ButtonSettings ButtonClose;
}

public class KitsPanelContentUI
{
    [JsonProperty(PropertyName = "Background")]
    public ImageSettings Background;
    [JsonProperty(PropertyName = "Button Back")]
    public ButtonSettings ButtonBack;
    [JsonProperty(PropertyName = "Button Next")]
    public ButtonSettings ButtonNext;
    [JsonProperty(PropertyName = "Button Create Kit")]
    public ButtonSettings ButtonCreateKit;
    [JsonProperty(PropertyName = "Show All Kits Checkbox")]
    public CheckBoxSettings CheckboxShowAllKits;
}

public class KitsPanelKitUI
{
    [JsonProperty(PropertyName = "Background")]
    public ImageSettings Background;
    [JsonProperty(PropertyName = "Show Name?")]
    public bool ShowName;
    [JsonProperty(PropertyName = "Kit Name")]
    public TextSettings KitName;
    [JsonProperty(PropertyName = "Show Number?")]
    public bool ShowNumber;
    [JsonProperty(PropertyName = "Kit Number")]
    public TextSettings KitNumber;
    [JsonProperty(PropertyName = "Kit Image")]
    public InterfacePosition KitImage;
    [JsonProperty(PropertyName = "Kit Button Take")]
    public ButtonSettings KitButtonTake;
    [JsonProperty(PropertyName = "Kit Button Take (when show info)")]
    public ButtonSettings KitButtonTakeWhenShowInfo;
    [JsonProperty(PropertyName = "Kit Button Info")]
    public ButtonSettings KitButtonInfo;
    [JsonProperty(PropertyName = "Show Line?")]
    public bool ShowLine;
    [JsonProperty(PropertyName = "Kit Line")]
    public InterfacePosition KitLine;
}

private class KitAmountSettings : InterfacePosition
{
    [JsonProperty(PropertyName = LangRu ? "Ширина" : "Width")]
    public float Width;
}

private class NpcKitsData
{
    [JsonProperty(PropertyName = LangRu ? "Описание" : "Description")]
    public string Description;
    [JsonProperty(PropertyName = LangRu ? "Наборы" : "Kits",
				ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<string> Kits;
}

public class ScrollViewUI
{
    [JsonProperty(PropertyName = "Scroll Type")]
    [JsonConverter(typeof(StringEnumConverter))]
    public ScrollType ScrollType;
    [JsonProperty(PropertyName = "Movement Type")]
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

public class CheckBoxSettings
{
    [JsonProperty(PropertyName = "Background")]
    public ImageSettings Background;
    [JsonProperty(PropertyName = "Checkbox")]
    public ButtonSettings CheckboxButton;
    [JsonProperty(PropertyName = "Checkbox Size")]
    public float CheckboxSize;
    [JsonProperty(PropertyName = "Checkbox Color")]
    public IColor CheckboxColor;
    [JsonProperty(PropertyName = "Title")]
    public TextSettings Title;
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

private class DescriptionSettings : InterfacePosition
{
    [JsonProperty(PropertyName = LangRu ? "Включено" : "Enabled")]
    public bool Enabled;
    [JsonProperty(PropertyName = LangRu ? "Цвет фона" : "Background Color")]
    public IColor Color;
    [JsonProperty(PropertyName = "Font Size")]
    public int FontSize;
    [JsonProperty(PropertyName = "Font")]
    public string Font;
    [JsonProperty(PropertyName = "Align")]
    [JsonConverter(typeof(StringEnumConverter))]
    public TextAnchor Align;
    [JsonProperty(PropertyName = "Text Color")]
    public IColor TextColor;
    public void Get(CuiElementContainer container, string parent, string name, string description);
}

private class MenuDescription : DescriptionSettings
{
    [JsonProperty(PropertyName = LangRu ? "Описание" : "Description")]
    public string Description;
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

private class LogInfo
{
    [JsonProperty(PropertyName = "To Console")]
    public bool Console;
    [JsonProperty(PropertyName = "To File")]
    public bool File;
}

private class RarityColor
{
    [JsonProperty(PropertyName = LangRu ? "Шанс" : "Chance")]
    public int Chance;
    [JsonProperty(PropertyName = LangRu ? "Цвет" : "Color")]
    public string Color;
    public RarityColor(int chance, string color);
}

private class PluginData
{
    [JsonProperty(PropertyName = "Kits", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<Kit> Kits;
}

private class Kit
{
    [JsonIgnore]
    public int ID;
    [JsonProperty(PropertyName = "Name")]
    public string Name;
    [JsonProperty(PropertyName = "Display Name")]
    public string DisplayName;
    [JsonProperty(PropertyName = "Color")]
    public string Color;
    [JsonProperty(PropertyName = "Permission")]
    public string Permission;
    [JsonProperty(PropertyName = "Description")]
    public string Description;
    [JsonProperty(PropertyName = "Image")]
    public string Image;
    [JsonProperty(PropertyName = "Hide")]
    public bool Hide;
    [JsonProperty(PropertyName = "ShowInfo")]
    [DefaultValue(true)]
    public bool ShowInfo;
    [JsonProperty(PropertyName = "Amount")]
    public int Amount;
    [JsonProperty(PropertyName = "Cooldown")]
    public double Cooldown;
    [JsonProperty(PropertyName = "Wipe Block")]
    public double CooldownAfterWipe;
    [JsonProperty(PropertyName = "Use Building")]
    public bool UseBuilding;
    [JsonProperty(PropertyName = "Building")]
    public string Building;
    [JsonProperty(PropertyName = "Enable sale")]
    public bool Sale;
    [JsonProperty(PropertyName = "Selling price")]
    public int Price;
    [JsonProperty(PropertyName = "Items", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<KitItem> Items;
    [JsonProperty(PropertyName = "Use commands on receiving?")]
    public bool UseCommandsOnReceiving;
    [JsonProperty(PropertyName = "Commands on receiving (via '|')",
				ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public string CommandsOnReceiving;
    [JsonProperty(PropertyName = "Use slot for backpack?")]
    public bool UseSlotForBackpack;
    public void Get(BasePlayer player);
    public void UseCommands(BasePlayer player);
    public void MoveRight();
    public void MoveLeft();
    [JsonIgnore]
    public Dictionary<int, KitItem> dictMainItems;
    [JsonIgnore]
    public Dictionary<int, KitItem> dictBeltItems;
    [JsonIgnore]
    public Dictionary<int, KitItem> dictWearItems;
    [JsonIgnore]
    public int beltCount;
    [JsonIgnore]
    public int wearCount;
    [JsonIgnore]
    public int mainCount;
    public void Update();
    private void LoadContainers();
    public KitItem GetItemByContainerAndPosition(string container, int position);
    [JsonIgnore]
    private JObject _jObject;
    [JsonIgnore]
    internal JObject ToJObject { get; set; }
    private void GenerateJObject();
}

private class KitItem
{
    [JsonProperty(PropertyName = "Type")]
    [JsonConverter(typeof(StringEnumConverter))]
    public KitItemType Type;
    [JsonProperty(PropertyName = "Command")]
    public string Command;
    [JsonProperty(PropertyName = "ShortName")]
    public string ShortName;
    [JsonProperty(PropertyName = "DisplayName")]
    public string DisplayName;
    [JsonProperty(PropertyName = "Amount")]
    public int Amount;
    [JsonProperty(PropertyName = "Blueprint")]
    public int Blueprint;
    [JsonProperty(PropertyName = "SkinID")]
    public ulong SkinID;
    [JsonProperty(PropertyName = "Container")]
    public string Container;
    [JsonProperty(PropertyName = "Condition")]
    public float Condition;
    [JsonProperty(PropertyName = "Chance")]
    public int Chance;
    [JsonProperty(PropertyName = "Position", DefaultValueHandling = DefaultValueHandling.Populate)]
    [DefaultValue(-1)]
    public int Position;
    [JsonProperty(PropertyName = "Image")]
    public string Image;
    [JsonProperty(PropertyName = "Weapon")]
    public Weapon Weapon;
    [JsonProperty(PropertyName = "Content", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<ItemContent> Content;
    [JsonProperty(PropertyName = "Text")]
    public string Text;
    public void Get(BasePlayer player);
    private void ToCommand(BasePlayer player);
    private static void GiveItem(BasePlayer player, Item item, ItemContainer cont);
    [JsonIgnore]
    private ItemDefinition _itemDefinition;
    [JsonIgnore]
    public ItemDefinition ItemDefinition { get; set; }
    public Item BuildItem();
    public static KitItem FromOld(ItemData item, string container);
    [JsonIgnore]
    private int _itemId;
    [JsonIgnore]
    public int itemId { get; set; }
    [JsonIgnore]
    private ICuiComponent _image;
    public CuiElement GetImage(string aMin, string aMax, string oMin, string oMax, string parent, string name);
    private void GenerateNewImage();
    private void UpdateItemID();
    public void Update();
    [JsonIgnore]
    private JObject _jObject;
    [JsonIgnore]
    public JObject ToJObject { get; set; }
    private void GenerateJObject();
}

private class Weapon
{
    public string ammoType;
    public int ammoAmount;
}

private class ItemContent
{
    public string ShortName;
    public float Condition;
    public int Amount;
}

private class OpenedKits
{
    public BasePlayer Player;
    public bool useMainUI;
    public ulong npcID;
    public OpenedKits(BasePlayer player, ulong targetID, bool mainUI);
    public List<Kit> availableKits;
    public List<Kit> kitsToUpdate;
    public void SetKitsToUpdate(List<Kit> kits);
    public void UpdateKitsToUpdate();
    public void UpdateKits(bool first);
    public int GetTotalKitsAmount();
    public int currentPage;
    public bool showAll;
    public void OnChangeCurrentPage(int page);
    public void OnChangeShowAll(bool newShowAll);
}

private class OldData
{
    [JsonProperty]
    public Dictionary<string, OldKitsData> _kits;
}

private class OldKitsData
{
    public string Name;
    public string Description;
    public string RequiredPermission;
    public int MaximumUses;
    public int RequiredAuth;
    public int Cooldown;
    public int Cost;
    public bool IsHidden;
    public string CopyPasteFile;
    public string KitImage;
    public ItemData[] MainItems;
    public ItemData[] WearItems;
    public ItemData[] BeltItems;
}

private class ItemData
{
    public string Shortname;
    public ulong Skin;
    public int Amount;
    public float Condition;
    public float MaxCondition;
    public int Ammo;
    public string Ammotype;
    public int Position;
    public int Frequency;
    public string BlueprintShortname;
    public ItemData[] Contents;
}

private class OldKitData
{
    public int Amount;
    public double Cooldown;
    public int HasAmount;
}

private class PlayerData
{
    [JsonProperty(PropertyName = "Kits Data", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public Dictionary<string, KitData> Kits;
    public class KitData
    {
        [JsonProperty(PropertyName = "Amount")]
        public int Amount;
        [JsonProperty(PropertyName = "Cooldown")]
        public double Cooldown;
        [JsonProperty(PropertyName = "HasAmount")]
        public int HasAmount;
    }

    private static string BaseFolder();
    public static PlayerData GetOrLoad(string userId);
    public static KitData GetOrLoadKitData(string userId, string kitName, bool addKit);
    private static PlayerData GetOrLoad(string baseFolder, string userId, bool load);
    public static PlayerData GetOrCreate(string userId);
    public static KitData GetOrCreateKitData(string userId, string kitName);
    public static void Save();
    public static void Save(string userId);
    public static void SaveAndUnload(string userId);
    public static void Unload(string userId);
    public static string[] GetFiles();
    public static string[] GetFiles(string baseFolder);
    private static PlayerData ReadOnlyObject(string name);
    public static void DoWipe(string userId, bool isWipe);
    public static void StartAll(Action<PlayerData> action);
    public static List<PlayerData> GetAll();
    public static PlayerData GetNotLoad(string userId);
    public static KitData GetNotLoadKitData(string userId, string kitName);
}

public class KitData
{
    [JsonProperty(PropertyName = "Amount")]
    public int Amount;
    [JsonProperty(PropertyName = "Cooldown")]
    public double Cooldown;
    [JsonProperty(PropertyName = "HasAmount")]
    public int HasAmount;
}

Oxide.Plugins.KitsExtensionMethods
public static class ExtensionMethods
{
    internal static Permission perm;
    public static bool IsURL(string uriName);
    public static bool All(IList<T> a, Func<T, bool> b);
    public static int Average(IList<int> a);
    public static T ElementAt(IEnumerable<T> a, int b);
    public static bool Exists(IEnumerable<T> a, Func<T, bool> b);
    public static T FirstOrDefault(IEnumerable<T> a, Func<T, bool> b);
    public static int RemoveAll(IDictionary<T, V> a, Func<T, V, bool> b);
    public static IEnumerable<V> Select(IEnumerable<T> a, Func<T, V> b);
    public static List<TResult> Select(List<T> source, Func<T, TResult> selector);
    public static List<T> SkipAndTake(List<T> source, int skip, int take);
    public static string[] Skip(string[] a, int count);
    public static List<T> Skip(IList<T> source, int count);
    public static Dictionary<T, V> Skip(IDictionary<T, V> source, int count);
    public static List<T> Take(IList<T> a, int b);
    public static Dictionary<T, V> Take(IDictionary<T, V> a, int b);
    public static Dictionary<T, V> ToDictionary(IEnumerable<S> a, Func<S, T> b, Func<S, V> c);
    public static List<T> ToList(IEnumerable<T> a);
    public static HashSet<T> ToHashSet(IEnumerable<T> a);
    public static List<T> Where(List<T> source, Predicate<T> predicate);
    public static List<T> Where(List<T> source, Func<T, int, bool> predicate);
    public static List<T> Where(IEnumerable<T> source, Func<T, bool> predicate);
    public static List<T> OfType(IEnumerable<BaseNetworkable> a);
    public static int Sum(IList<T> a, Func<T, int> b);
    public static T LastOrDefault(List<T> source);
    public static int Count(List<T> source, Func<T, bool> predicate);
    public static TAccumulate Aggregate(List<TSource> source, TAccumulate seed, Func<TAccumulate, TSource, TAccumulate> func);
    public static int Sum(IList<int> a);
    public static bool HasPermission(string userID, string b);
    public static bool HasPermission(BasePlayer a, string b);
    public static bool HasPermission(ulong a, string b);
    public static bool IsReallyConnected(BasePlayer a);
    public static bool IsKilled(BaseNetworkable a);
    public static bool IsNull(T a);
    public static bool IsNull(BasePlayer a);
    public static bool IsReallyValid(BaseNetworkable a);
    public static void SafelyKill(BaseNetworkable a);
    public static bool CanCall(Plugin o);
    public static bool IsInBounds(OBB o, Vector3 a);
    public static bool IsHuman(BasePlayer a);
    public static BasePlayer ToPlayer(IPlayer user);
    public static List<TResult> SelectMany(List<TSource> source, Func<TSource, List<TResult>> selector);
    public static IEnumerable<TResult> SelectMany(IEnumerable<TSource> source, Func<TSource, IEnumerable<TResult>> selector);
    public static int Sum(IEnumerable<TSource> source, Func<TSource, int> selector);
    public static double Sum(IEnumerable<TSource> source, Func<TSource, double> selector);
    public static bool Any(IEnumerable<TSource> source, Func<TSource, bool> predicate);
    public static string GetFieldTitle(string field);
}


```

---

## Kits4

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
[Info("Kits", "OMHOMHOM", 0.1)]
 class Kits : RustPlugin
{
    private string Layer;
    private string LayerBlur;
    private string LayerBlurKitsInfo;
     object OnPlayerRespawned(BasePlayer player);
     void Loaded();
     void OnServerInitialized();
    private KitsInfo GetKitsInfo(BasePlayer player);
    private KitData GetKitData(KitsInfo kitsInfo, KitInfo kitInfo);
     void Unload();
    [ChatCommand("createkit")]
    private void CreateKit(BasePlayer player, string command, string[] args);
    [ConsoleCommand("UI_KITS")]
    private void cmdConsoleHandler(ConsoleSystem.Arg arg);
    [ChatCommand("kit")]
     void KitOpen(BasePlayer player, string command, string[] args);
    [ChatCommand("kits")]
     void KitsOpen(BasePlayer player, string command, string[] args);
    private List<KitInfo> GetKitsForPlayer(BasePlayer player);
    private void GiveItems(BasePlayer player, KitInfo kit);
    private void GiveItem(BasePlayer player, Item item, ItemContainer cont);
    private Item BuildItem(string ShortName, int Amount, ulong SkinID, float Condition, Weapon weapon, List<ItemContent> Content);
    private List<ItemInfo> GetPlayerItems(BasePlayer player);
    private ItemInfo ItemToKit(Item item, string container);
    private static string HexToRGB(string hex);
    protected override void SaveConfig();
    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    public Configuration _config;
    public class Configuration
    {
        [JsonProperty("Набор на респавне")]
        public List<ItemInfo> spawnKit;
        [JsonProperty("Наборы")]
        public List<KitInfo> kits;
    }

    public class KitInfo
    {
        [JsonProperty("Название")]
        public string kitName;
        [JsonProperty("Максимум использований")]
        public int maxUse;
        [JsonProperty("Кулдаун")]
        public double cooldownKit;
        [JsonProperty("Привилегия")]
        public string privilege;
        [JsonProperty("Предметы")]
        public List<ItemInfo> items;
    }

    public class ItemInfo
    {
        [JsonProperty("Позиция")]
        public int position;
        [JsonProperty("Shortname")]
        public string shortname;
        [JsonProperty("Количество")]
        public int amount;
        [JsonProperty("Место")]
        public string place;
        [JsonProperty("Скин")]
        public ulong skinID;
        [JsonProperty("Контейнер")]
        public List<ItemContent> Content { get; set; }
        [JsonProperty("Прочность")]
        public float Condition { get; set; }
        [JsonProperty("Оружие")]
        public Weapon Weapon { get; set; }
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

     class StoredData
    {
        public Dictionary<ulong, KitsInfo> players;
    }

     class KitsInfo
    {
        public Dictionary<string, KitData> kits;
    }

     class KitData
    {
        [JsonProperty("a")]
        public int amount;
        [JsonProperty("cd")]
        public double cooldown;
    }

     void SaveData();
     void LoadData();
     StoredData storedData;
    private static class TimeHelper
    {
        public static string FormatTime(TimeSpan time, int maxSubstr, string language);
        private static string Format(int units, string form1, string form2, string form3);
        private static DateTime Epoch;
        public static double GetTimeStamp();
    }

    public CuiRawImageComponent GetAvatarImageComponent(ulong user_id, string color);
    public CuiRawImageComponent GetImageComponent(string url, string shortName, string color);
    public CuiRawImageComponent GetItemImageComponent(string shortName);
    public bool AddImage(string url, string shortName);
}

public class Configuration
{
    [JsonProperty("Набор на респавне")]
    public List<ItemInfo> spawnKit;
    [JsonProperty("Наборы")]
    public List<KitInfo> kits;
}

public class KitInfo
{
    [JsonProperty("Название")]
    public string kitName;
    [JsonProperty("Максимум использований")]
    public int maxUse;
    [JsonProperty("Кулдаун")]
    public double cooldownKit;
    [JsonProperty("Привилегия")]
    public string privilege;
    [JsonProperty("Предметы")]
    public List<ItemInfo> items;
}

public class ItemInfo
{
    [JsonProperty("Позиция")]
    public int position;
    [JsonProperty("Shortname")]
    public string shortname;
    [JsonProperty("Количество")]
    public int amount;
    [JsonProperty("Место")]
    public string place;
    [JsonProperty("Скин")]
    public ulong skinID;
    [JsonProperty("Контейнер")]
    public List<ItemContent> Content { get; set; }
    [JsonProperty("Прочность")]
    public float Condition { get; set; }
    [JsonProperty("Оружие")]
    public Weapon Weapon { get; set; }
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

 class StoredData
{
    public Dictionary<ulong, KitsInfo> players;
}

 class KitsInfo
{
    public Dictionary<string, KitData> kits;
}

 class KitData
{
    [JsonProperty("a")]
    public int amount;
    [JsonProperty("cd")]
    public double cooldown;
}

private static class TimeHelper
{
    public static string FormatTime(TimeSpan time, int maxSubstr, string language);
    private static string Format(int units, string form1, string form2, string form3);
    private static DateTime Epoch;
    public static double GetTimeStamp();
}


```

---

## Kitss

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using JetBrains.Annotations;
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using Rust.Workshop;
using Steamworks.ServerList;
using UnityEngine;

Oxide.Plugins
[Info("Kits", "XAVIER", "1.0.5")]
public class Kitss : RustPlugin
{
     bool HasPermission(string id, string perm);
    const string permAllowed;
    public class Kit
    {
        public string Name;
        public string DisplayName;
        public string DisplayNamePermission;
        public string CustomImage;
        public double Cooldown;
        public bool Hide;
        public string Permission;
        public List<KitItem> Items;
        public int UniversalNumber;
    }

    public class KitItem
    {
        public string ShortName;
        public int Amount;
        public int Blueprint;
        public ulong SkinID;
        public string Container;
        public float Condition;
        public Weapon Weapon;
        public List<ItemContent> Content;
    }

    public class Weapon
    {
        public string ammoType;
        public int ammoAmount;
    }

    public class ItemContent
    {
        public string ShortName;
        public float Condition;
        public int Amount;
    }

    public class KitsCooldown
    {
        public int Number;
        public double CoolDown;
    }

    public class CategoryKit
    {
        [JsonProperty("Название категории")]
        public string DisplayName;
        [JsonProperty("Картинка")]
        public string Image;
        [JsonProperty("Лист с китами")]
        public List<Kit> KitList;
    }

    public List<CategoryKit> KitLists;
    public Dictionary<ulong, List<KitsCooldown>> CooldownData;
    public List<ulong> OPENGUI;
    public string Layer;
    [PluginReference]
    private Plugin ImageLibrary;
    public Dictionary<string, string> ImageDictionary;
     void Init();
     void OnServerInitialized();
     void Unload();
     void OnPlayerConnected(BasePlayer player);
     void OnPlayerDisconnected(BasePlayer player);
    private List<KitItem> GetItemPlayer(BasePlayer player);
    private KitItem ItemAddToKit(Item item, string container);
    public List<String> AutoKit;
     void OnPlayerRespawned(BasePlayer player);
    private void GiveItems(BasePlayer player, Kit kit);
    private void GiveItem(BasePlayer player, Item item, ItemContainer cont);
    private Item BuildItem(string ShortName, int Amount, ulong SkinID, float Condition, int blueprintTarget, Weapon weapon, List<ItemContent> Content);
    [ChatCommand("kit")]
     void KitFunc(BasePlayer player, string command, string[] args);
     void CreateCategory(BasePlayer player, string category);
    [ConsoleCommand("give.kit")]
     void GivToKit(ConsoleSystem.Arg args);
    public string LayerNotif;
     void NotifUIGive(BasePlayer player, string nameImage);
     object GiveKit(BasePlayer player, string nameKit);
     void GiveKitCategory(BasePlayer player, string category, string kitname);
    public string GetImage(string shortname);
    private static class TimeExtensions
    {
        public static string FormatShortTime(TimeSpan time);
        private static string Format(int units, string form1, string form2, string form3);
    }

    static readonly DateTime epoch;
    static double CurrentTime();
     void AddKits(BasePlayer player, string name, string category, string permissions);
     List<Kit> GetKitPlayer(BasePlayer player, List<Kit> kitsList);
    [ConsoleCommand("close.kit")]
     void CloseKit(ConsoleSystem.Arg args);
    public string LayerTest;
    [ConsoleCommand("kits.open")]
     void OpenKitCategory(ConsoleSystem.Arg args);
    [ConsoleCommand("back.kits")]
     void BackKitCategory(ConsoleSystem.Arg args);
    private string GetFormatTime(TimeSpan timespan);
     void OpenCategoryKit(BasePlayer player, CategoryKit kit);
    [ConsoleCommand("kit.drawkitinfo")]
     void cmdDrawKitInfo(ConsoleSystem.Arg args);
    private void ViewInventoryKits(BasePlayer player, string kit);
     List<CategoryKit> GetCategory(BasePlayer player);
     void OpenKitMenu(BasePlayer player);
}

public class Kit
{
    public string Name;
    public string DisplayName;
    public string DisplayNamePermission;
    public string CustomImage;
    public double Cooldown;
    public bool Hide;
    public string Permission;
    public List<KitItem> Items;
    public int UniversalNumber;
}

public class KitItem
{
    public string ShortName;
    public int Amount;
    public int Blueprint;
    public ulong SkinID;
    public string Container;
    public float Condition;
    public Weapon Weapon;
    public List<ItemContent> Content;
}

public class Weapon
{
    public string ammoType;
    public int ammoAmount;
}

public class ItemContent
{
    public string ShortName;
    public float Condition;
    public int Amount;
}

public class KitsCooldown
{
    public int Number;
    public double CoolDown;
}

public class CategoryKit
{
    [JsonProperty("Название категории")]
    public string DisplayName;
    [JsonProperty("Картинка")]
    public string Image;
    [JsonProperty("Лист с китами")]
    public List<Kit> KitList;
}

private static class TimeExtensions
{
    public static string FormatShortTime(TimeSpan time);
    private static string Format(int units, string form1, string form2, string form3);
}


```

---

## KitsUI

```csharp
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using System;
using System.Collections.Generic;
using System.Linq;

Oxide.Plugins
[Info("KitsUI", "OxideBro / RustPlugin.ru", "0.1.0")]
[Description("UI оболочка для китов с Oxide - https://oxidemod.org/plugins/kits.668/")]
 class KitsUI : RustPlugin
{
    [PluginReference]
    private Plugin Kits;
    private Dictionary<BasePlayer, int> openUI;
    private void OnServerInitialized();
     void TimerHandle();
    public string BackroungPanelColor;
    protected override void LoadDefaultConfig();
    [ChatCommand("kits")]
     void cmdChatKitsUI(BasePlayer player);
    [ConsoleCommand("kitsgui_show")]
     void cmdShowKitsUI(ConsoleSystem.Arg arg);
    [ConsoleCommand("destroykitsui")]
     void cmdDestroyKitsUI(ConsoleSystem.Arg arg);
     void Unload();
    [ConsoleCommand("trytogetkit")]
     void cmdTryToGetKit(ConsoleSystem.Arg arg);
    [ConsoleCommand("trytogetkit_nextpage")]
     void cmdTryToGetKitNextPage(ConsoleSystem.Arg arg);
    private void DestroyKitsUI(BasePlayer player);
     string Button;
     string GUI;
     string ButtonBG;
     string Page;
    private void DrawKitsUI(BasePlayer player, int page);
     void DrawKitsInfo(BasePlayer player, string amin, string amax, string color, string command, string text);
    public static string FormatTime(TimeSpan time);
    private static string Format(int units, string form1, string form2, string form3);
    private void GetVeriables(string menu, string Key, T var);
}


```

---

## KitSystem

```csharp
using System;
using Oxide.Core;
using System.Collections.Generic;
using System.Linq;
using Oxide.Game.Rust.Cui;
using Oxide.Core.Plugins;
using UnityEngine;

Oxide.Plugins
[Info("KitSystem", "Sempai#3239", "1.0.0")]
 class KitSystem : RustPlugin
{
     string Layer;
    [PluginReference]
     Plugin ImageLibrary;
     Dictionary<ulong, Data> Settings;
    public class KitSettings
    {
        public string Name;
        public string DisplayName;
        public double Cooldown;
        public int Amount;
        public string Perm;
        public string Url;
        public List<ItemSettings> Items;
    }

    public class ItemSettings
    {
        public string ShortName;
        public int Amount;
        public ulong SkinID;
        public string Container;
    }

    public class Data
    {
        public Dictionary<string, KitData> SettingsData;
    }

    public class KitData
    {
        public double Cooldown;
    }

     Configuration config;
     class Configuration
    {
        public string BannerURL;
        public string Description;
        public List<KitSettings> settings;
        public static Configuration GetNewConfig();
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
     void OnServerInitialized();
     void OnPlayerConnected(BasePlayer player);
     void OnPlayerDisconnected(BasePlayer player, string reason);
     void Unload();
     void CreateDataBase(BasePlayer player);
     void SaveDataBase(ulong userId);
    [ConsoleCommand("kit")]
     void ConsoleKit(ConsoleSystem.Arg args);
     void KitUI(BasePlayer player);
     void UI(BasePlayer player, string name);
     void InterfaceKit(BasePlayer player, int page);
     KitData GetDataBase(ulong userID, string name);
    public static string FormatShortTime(TimeSpan time);
     double CurTime();
}

public class KitSettings
{
    public string Name;
    public string DisplayName;
    public double Cooldown;
    public int Amount;
    public string Perm;
    public string Url;
    public List<ItemSettings> Items;
}

public class ItemSettings
{
    public string ShortName;
    public int Amount;
    public ulong SkinID;
    public string Container;
}

public class Data
{
    public Dictionary<string, KitData> SettingsData;
}

public class KitData
{
    public double Cooldown;
}

 class Configuration
{
    public string BannerURL;
    public string Description;
    public List<KitSettings> settings;
    public static Configuration GetNewConfig();
}


```

---

## KitSystem-0.0.1

```csharp
using System;
using System.Linq;
using System.Collections.Generic;
using Oxide.Core;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using Newtonsoft.Json;
using UnityEngine;

Oxide.Plugins
[Info("KitSystem", "Chibubrik / Deversive", "0.0.1")]
 class KitSystem : RustPlugin
{
    private static string KitsDostyp;
    private string Layer;
    private string Layer1;
    [PluginReference]
     Plugin ImageLibrary;
     Plugin NoteUI;
     Dictionary<ulong, Data> Settings;
    private Dictionary<BasePlayer, List<string>> _kitsGUI;
    public class KitSettings
    {
        [JsonProperty("Название набора")]
        public string Name;
        [JsonProperty("Формат названия набора")]
        public string DisplayName;
        [JsonProperty("Картинка кита")]
        public string Image;
        [JsonProperty("Название кулдаун набора")]
        public double Cooldown;
        [JsonProperty("Привилегия")]
        public string Perm;
        [JsonProperty("Предметы набора")]
        public List<ItemSettings> Items;
    }

    public class ItemSettings
    {
        [JsonProperty("Название предмета")]
        public string ShortName;
        [JsonProperty("Количество предметов")]
        public int Amount;
        [JsonProperty("Изучение")]
        public int Blueprint;
        [JsonProperty("Condition")]
        public float Condition;
        [JsonProperty("Weapon")]
        public Weapon Weapon;
        [JsonProperty("Content")]
        public List<ItemContent> Content;
        [JsonProperty("Скин предмета")]
        public ulong SkinID;
        [JsonProperty("Место в инвентаре")]
        public string Container;
    }

    public class Weapon
    {
        [JsonProperty("Тип пули")]
        public string ammoType;
        [JsonProperty("Количество")]
        public int ammoAmount;
    }

    public class ItemContent
    {
        [JsonProperty("Название предмета")]
        public string ShortName;
        [JsonProperty("Количество предметов")]
        public int Amount;
        [JsonProperty("Condition")]
        public float Condition;
    }

    public class AutoKit
    {
        [JsonProperty("Наборы при респавне")]
        public List<string> CustomKit;
    }

    public class Data
    {
        [JsonProperty("Список наборов и их кулдаун")]
        public Dictionary<string, KitData> SettingsData;
    }

    public class KitData
    {
        [JsonProperty("Кулдаун набора")]
        public double Cooldown;
    }

     Configuration config;
     class Configuration
    {
        [JsonProperty("Настройки наборов")]
        public List<KitSettings> settings;
        public static Configuration GetNewConfig();
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
     void OnServerInitialized();
    private string GetImage(string fileName, ulong skin);
    public bool AddImage(string url, string shortname, ulong skin);
    private string fonkits;
    private string nabor;
     void LoadImage();
     void OnPlayerConnected(BasePlayer player);
     void OnPlayerDisconnected(BasePlayer player, string reason);
     void Unload();
     KitData GetDataBase(ulong userID, string name);
    public static string FormatShortTime(TimeSpan time);
     void DestroyUi(BasePlayer player);
     void UpdateInterface(BasePlayer player, string Name);
     double CurTime();
     void CreateDataBase(BasePlayer player);
     void SaveDataBase(ulong userId);
     void CreateKit(BasePlayer player, string kitname);
     List<ItemSettings> GetItems(BasePlayer player);
     ItemSettings CreateItem(Item item, string container);
    [ConsoleCommand("closeui1231")]
     void closeui1231(BasePlayer player, string command, string[] args);
    [ChatCommand("kits")]
     void ChatKits(BasePlayer player, string command, string[] args);
    [ChatCommand("kit")]
     void ChatKit(BasePlayer player, string command, string[] args);
    [ConsoleCommand("kit")]
     void ConsoleKit(ConsoleSystem.Arg args);
     float HeihtMax;
     float HeightMax;
     void CloseUI(BasePlayer player);
     void KitUI(BasePlayer player);
     void KitListUI(BasePlayer player, int page);
    private static string HexToRustFormat(string hex);
}

public class KitSettings
{
    [JsonProperty("Название набора")]
    public string Name;
    [JsonProperty("Формат названия набора")]
    public string DisplayName;
    [JsonProperty("Картинка кита")]
    public string Image;
    [JsonProperty("Название кулдаун набора")]
    public double Cooldown;
    [JsonProperty("Привилегия")]
    public string Perm;
    [JsonProperty("Предметы набора")]
    public List<ItemSettings> Items;
}

public class ItemSettings
{
    [JsonProperty("Название предмета")]
    public string ShortName;
    [JsonProperty("Количество предметов")]
    public int Amount;
    [JsonProperty("Изучение")]
    public int Blueprint;
    [JsonProperty("Condition")]
    public float Condition;
    [JsonProperty("Weapon")]
    public Weapon Weapon;
    [JsonProperty("Content")]
    public List<ItemContent> Content;
    [JsonProperty("Скин предмета")]
    public ulong SkinID;
    [JsonProperty("Место в инвентаре")]
    public string Container;
}

public class Weapon
{
    [JsonProperty("Тип пули")]
    public string ammoType;
    [JsonProperty("Количество")]
    public int ammoAmount;
}

public class ItemContent
{
    [JsonProperty("Название предмета")]
    public string ShortName;
    [JsonProperty("Количество предметов")]
    public int Amount;
    [JsonProperty("Condition")]
    public float Condition;
}

public class AutoKit
{
    [JsonProperty("Наборы при респавне")]
    public List<string> CustomKit;
}

public class Data
{
    [JsonProperty("Список наборов и их кулдаун")]
    public Dictionary<string, KitData> SettingsData;
}

public class KitData
{
    [JsonProperty("Кулдаун набора")]
    public double Cooldown;
}

 class Configuration
{
    [JsonProperty("Настройки наборов")]
    public List<KitSettings> settings;
    public static Configuration GetNewConfig();
}


```

---

## kryIncreaseWeapons

```csharp
using System.Linq;
using System.Collections.Generic;
using Newtonsoft.Json;

Oxide.Plugins
[Info("kryIncreaseWeapons", "xkrystalll", "1.0.2")]
 class kryIncreaseWeapons : RustPlugin
{
    public static ConfigData cfg;
    public class ConfigData
    {
        [JsonProperty("Улучшенное оружие")]
        public Dictionary<string, WeaponsInfo> weapons;
    }

    protected override void LoadDefaultConfig();
     void LoadConfig();
     void SaveConfig(object config);
    public List<WeaponsInfo> weaponsVal;
    public List<string> weaponsKeys;
    public class WeaponsInfo
    {
        public WeaponsInfo(string name, ulong skin, float dmulti, float cmulti, string shortname, bool maybeRepair, bool deleteOnBreak, bool entityMultiplier);
        [JsonProperty("ShortName оружия")]
        public string shortname;
        [JsonProperty("Имя оружия")]
        public string name;
        [JsonProperty("Скин оружия")]
        public ulong skin;
        [JsonProperty("Множитель урона оружия")]
        public float damageMultiplier;
        [JsonProperty("Множитель ломания оружия")]
        public float conditionMultiplier;
        [JsonProperty("Можно ли починить?")]
        public bool maybeRepair;
        [JsonProperty("Удалять оружие при поломке?")]
        public bool deleteOnBreak;
        [JsonProperty("Умножать урон по строениям?")]
        public bool entityMultiplier;
    }

     object OnEntityTakeDamage(BaseCombatEntity entity, HitInfo info);
     object OnItemRepair(BasePlayer player, Item item);
    private void ChangeItemSkin(Item item, ulong targetSkin);
     void OnLoseCondition(Item item, float amount);
     void OnServerInitialized();
    [ConsoleCommand("giveweapon")]
     void GiveWeapon(ConsoleSystem.Arg args);
}

public class ConfigData
{
    [JsonProperty("Улучшенное оружие")]
    public Dictionary<string, WeaponsInfo> weapons;
}

public class WeaponsInfo
{
    public WeaponsInfo(string name, ulong skin, float dmulti, float cmulti, string shortname, bool maybeRepair, bool deleteOnBreak, bool entityMultiplier);
    [JsonProperty("ShortName оружия")]
    public string shortname;
    [JsonProperty("Имя оружия")]
    public string name;
    [JsonProperty("Скин оружия")]
    public ulong skin;
    [JsonProperty("Множитель урона оружия")]
    public float damageMultiplier;
    [JsonProperty("Множитель ломания оружия")]
    public float conditionMultiplier;
    [JsonProperty("Можно ли починить?")]
    public bool maybeRepair;
    [JsonProperty("Удалять оружие при поломке?")]
    public bool deleteOnBreak;
    [JsonProperty("Умножать урон по строениям?")]
    public bool entityMultiplier;
}


```

---

## LadderAnywhere

```csharp
using System.Reflection;
using UnityEngine;
using System.Linq;
using Oxide.Core.Plugins;
using System.Text;

Oxide.Plugins
[Info("LadderAnywhere", "LadderAnywhere by Deicide666ra", "1.0.7", ResourceId = 1327)]
public class LadderAnywhere : RustPlugin
{
     class LadderConfig
    {
        public LadderConfig();
        public float maxDist;
        public int authLevel;
        public bool radiationCheck;
        public string[] blacklist;
    }

    private LadderConfig g_config;
     void Loaded();
    protected override void LoadDefaultConfig();
     void LoadConfigValues();
    [HookMethod("SendHelpText")]
    private void SendHelpText(BasePlayer player);
    [ChatCommand("ldr")]
     void cmdLadder(BasePlayer player, string cmd, string[] args);
    [ChatCommand("lt")]
     void cmdLt(BasePlayer player, string cmd, string[] args);
    private void DoDeploy(BasePlayer player, Collider baseentity, Quaternion currentRot, Vector3 closestHitpoint);
    private void SpawnDeployable(string prefab, Vector3 pos, Quaternion angles, BasePlayer player);
    static bool TryGetPlayerView(BasePlayer player, Quaternion viewAngle);
}

 class LadderConfig
{
    public LadderConfig();
    public float maxDist;
    public int authLevel;
    public bool radiationCheck;
    public string[] blacklist;
}


```

---

## LadderModes

```csharp
using System;
using System.Collections.Generic;
using Oxide.Core.Plugins;
using UnityEngine;

Oxide.Plugins
[Info("Ladder Modes", "Lomarine", "2.0.0")]
[Description("Плагин для управления лестницами, разработал - https://vk.com/lomarine")]
public class LadderModes : RustPlugin
{
    [PluginReference]
     Plugin NoEscape;
    private void Init();
    private void OnEntityBuilt(Planner planner, GameObject gameobject);
    protected override void LoadDefaultMessages();
    private bool raidPriv;
    private bool freePriv;
    private bool raid;
    private bool free;
    private const string config1;
    private const string config2;
    private const string config3;
    private const string config4;
    protected override void LoadDefaultConfig();
     T GetConfig(string name, T value);
}


```

---

## LastManStanding

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
[Info("LastManStanding", "TheMechanical97", "1.1.3")]
 class LastManStanding : RustPlugin
{
    [PluginReference]
     Plugin EventManager;
    [PluginReference]
     Plugin ZoneManager;
    [PluginReference]
     Plugin Spawns;
    static string DefaultKit;
    static string EventName;
    static string EventZoneName;
    static string EventSpawnFile;
    static float StartHealth;
    static int SurvivalPoints;
    static int WinnerPoints;
    static int KillPoints;
    private void LoadVariables();
    private void LoadConfigVariables();
    private void CheckCfg(string Key, T var);
    private void CheckCfgFloat(string Key, float var);
     object GetConfig(string menu, string datavalue, object defaultValue);
     object GetEventConfig(string configname);
    private bool useThisEventLMS;
    private bool EventStarted;
    private bool Changed;
    public string CurrentKit;
    private List<LastManStandingPlayers> LMSPlayers;
     class LastManStandingPlayers : MonoBehaviour
    {
        public BasePlayer player;
        public int deaths;
        public int wins;
        public int points;
         void Awake();
    }

     class StoredData
    {
        public Dictionary<ulong, double> totalkills;
        public Dictionary<ulong, double> totaldeaths;
        public Dictionary<ulong, double> totalwins;
    }

     StoredData storedData;
     void showstats(BasePlayer player);
     List<BasePlayer> onlineplayers;
     Dictionary<string, string> messages;
    private void MessageAllPlayers(string msg);
    private void MessageAllOnlinePlayers(string msg);
     void SearchWinner();
     void resetstats(BasePlayer player);
     void resetstatsconsole();
     void addSurvivalPoints();
     void addKillPoints(BasePlayer player);
     void addkillstats(BasePlayer player);
     void adddeathsstats(BasePlayer player);
     void addwinsstats(BasePlayer player);
     void Winner(BasePlayer player);
     void Loaded();
     void OnServerInitialized();
     void RegisterGame();
     void LoadDefaultConfig();
     void Unload();
     void OnSelectEventGamePost(string name);
     void OnEventPlayerSpawn(BasePlayer player);
     void OnPostZoneCreate(string name);
     object OnEventOpenPost();
     object OnEventClosePost();
     object OnEventEndPre();
     object OnEventEndPost();
     object OnEventStartPre();
     object OnSelectKit(string kitname);
     object OnEventJoinPost(BasePlayer player);
     object OnEventLeavePost(BasePlayer player);
     void OnEventPlayerAttack(BasePlayer attacker, HitInfo hitinfo);
     void OnEventPlayerDeath(BasePlayer victim, HitInfo hitinfo);
     object EventChooseSpawn(BasePlayer player, Vector3 destination);
     object OnRequestZoneName();
     object OnSelectSpawnFile(string name);
     void OnSelectEventZone(MonoBehaviour monoplayer, string radius);
     object CanEventOpen();
     object CanEventStart();
     object OnEventStartPost();
     object CanEventJoin();
    [ChatCommand("lms")]
    private void cmdStats(BasePlayer player, string command, string[] args);
    [ConsoleCommand("lms.reset")]
     void cmdlmsConsole(ConsoleSystem.Arg arg);
}

 class LastManStandingPlayers : MonoBehaviour
{
    public BasePlayer player;
    public int deaths;
    public int wins;
    public int points;
     void Awake();
}

 class StoredData
{
    public Dictionary<ulong, double> totalkills;
    public Dictionary<ulong, double> totaldeaths;
    public Dictionary<ulong, double> totalwins;
}


```

---

## LastName

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using Oxide.Core;
using Oxide.Core.Configuration;

Oxide.Plugins
[Info("LastName", "deer_SWAG", "0.1.15", ResourceId = 1227)]
[Description("Stores all usernames")]
public class LastName : RustPlugin
{
    const string databaseName;
     class StoredData
    {
        public HashSet<Player> Players;
        public StoredData();
        public void Add(Player player);
    }

     class Player
    {
        public ulong userID;
        public HashSet<string> Names;
        public Player();
        public Player(ulong userID);
        public void Add(string name);
    }

     StoredData data;
     DynamicConfigFile nameChangeData;
    protected override void LoadDefaultConfig();
    private void OnPluginLoaded();
    private void OnPlayerConnected(Network.Message packet);
    private void OnPlayerInit(BasePlayer player);
    [ChatCommand("lastname")]
    private void cmdChat(BasePlayer player, string command, string[] args);
    [ConsoleCommand("player.lastname")]
    private void cmdConsole(ConsoleSystem.Arg arg);
    private string GetNames(string[] args);
     void SendHelpText(BasePlayer player);
    private void CheckConfig();
    private void SaveData();
    private void ConfigItem(string name, object defaultValue);
    private void ConfigItem(string name1, string name2, object defaultValue);
    private bool IsPluginExists(string name);
    private bool StringContains(string source, string value, StringComparison comparison);
}

 class StoredData
{
    public HashSet<Player> Players;
    public StoredData();
    public void Add(Player player);
}

 class Player
{
    public ulong userID;
    public HashSet<string> Names;
    public Player();
    public Player(ulong userID);
    public void Add(string name);
}


```

---

## Leaderboard

```csharp
using System.Collections.Generic;
using System.Linq;
using Oxide.Game.Rust.Cui;
using UnityEngine;
using System;
using Oxide.Core;

Oxide.Plugins
[Info("Leaderboard", "Sempai#3239", "1.0.0")]
 class Leaderboard : RustPlugin
{
     string Layer;
     Dictionary<ulong, DataBase> DB;
    public class DataBase
    {
        public string DisplayName;
        public bool IsConnected;
        public int Kills;
        public int Deaths;
        public int Hs;
        public int Hits;
    }

     void OnServerInitialized();
     void OnPlayerConnected(BasePlayer player);
     void OnPlayerDisconnected(BasePlayer player);
     void Unload();
     void SaveDataBase();
     void OnPlayerDeath(BasePlayer player, HitInfo info);
     void OnEntityTakeDamage(BaseCombatEntity entity, HitInfo info);
    [ConsoleCommand("stats")]
     void ConsoleStats(ConsoleSystem.Arg args);
     void LeaderboardUI(BasePlayer player);
     void Leader(BasePlayer player, int page);
}

public class DataBase
{
    public string DisplayName;
    public bool IsConnected;
    public int Kills;
    public int Deaths;
    public int Hs;
    public int Hits;
}


```

---

## LimeBench

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using UnityEngine;
using System.Reflection;
using System.Text;
using System.Collections;
using Oxide.Core.Plugins;

Oxide.Plugins
[Info("L.I.M.E. Bench", "Deicide666ra", "1.1.2", ResourceId = 1155)]
 class LimeBench : RustPlugin
{
     float c_craftingMultiplier;
     float c_gunpowderMultiplier;
     float c_benchMultiplier;
     string[] c_craftingMultiplierBlacklist;
     string[] c_benchMultiplierBlacklist;
     string[] c_bulkCraftBlacklist;
     int c_craftingMultiplierAuthLevel;
     int c_benchMultiplierAuthLevel;
     string g_bulkCraftPermissionName;
     string g_bulkCraftNoCupboardPermissionName;
     Dictionary<int, float> r_blueprintTimes;
    private bool configChanged;
     List<ItemBlueprint> blueprintDefinitions;
    private FieldInfo buildingPrivlidges;
     void Loaded();
     void Unloaded();
    protected override void LoadDefaultConfig();
     void LoadConfigValues();
     object GetConfigValue(string category, string setting, object defaultValue);
     void Rollback();
    [HookMethod("SendHelpText")]
    private void SendHelpText(BasePlayer player);
    [ChatCommand("limebench")]
     void cmdLimebench(BasePlayer player, string cmd, string[] args);
     void OnServerInitialized();
     void BroadcastToChat(string msg);
     bool UserIsAuthorizedOnAnyCupboard(BasePlayer player);
     void OnItemCraft(ItemCraftTask task);
     int GetStackSize(string shortname);
     void GiveItemsToPlayer(BasePlayer player, string shortname, int skinID, int amount);
     bool CanBulk(BasePlayer player);
     void AdjustCraftingTime(BasePlayer player, ItemBlueprint bp, ItemCraftTask task);
     void OnItemCraftFinished(ItemCraftTask task, Item item);
}


```

---

## LimitAuthorization

```csharp
using System;
using System.Collections;
using System.Collections.Generic;
using System.Globalization;
using System.Linq;
using Mono.Security.X509;
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using ProtoBuf;
using UnityEngine;

Oxide.Plugins
[Info("LimitAuthorization", "Own3r/Nericai", "1.1.3")]
[Description(
        "Ограничивает лимит авторизаций и позволяет авторизовать в замках, шкафах, турелях,")]
 class LimitAuthorization : RustPlugin
{
    [PluginReference]
    private Plugin Clans;
    private Plugin Friends;
    private Plugin FriendSystem;
    private Plugin ImageLibrary;
    private List<AuthManager> authManager;
    private class AuthManager
    {
        public string PictureURL;
        public string Command;
        public AuthManager(string pictureUrl, string command);
    }

    private class AuthConfig
    {
        [JsonProperty("Лимит авторизаций в шкафу")]
        public int CupboardLimit;
        [JsonProperty("Лимит авторизаций в замках")]
        public int CodelockLimit;
        [JsonProperty("Лимит авторизаций в турелях")]
        public int TurretLimit;
        [JsonProperty("Разрешить авторизацию кланам")]
        public bool ClanAllow;
        [JsonProperty("Разрешить авторизацию друзьям")]
        public bool FriendsAllow;
    }

    private AuthConfig config;
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    protected override void LoadConfig();
    private void OnServerInitialized();
    private Coroutine clanListUpdater;
    private Coroutine friendListUpdater;
    public void StartClanUpdate();
    public IEnumerator UpdateMemebers(Func<BasePlayer, List<string>> cb, Dictionary<ulong, List<string>> membersList, Action finalize);
     Dictionary<ulong, List<string>> currentClanMembers;
     Dictionary<ulong, List<string>> currentFriends;
     void Unload();
     object OnCupboardAuthorize(BuildingPrivlidge privilege, BasePlayer player);
     object OnCodeEntered(CodeLock codeLock, BasePlayer player, string code);
    private object OnTurretTarget(AutoTurret turret, BaseCombatEntity targ);
     class ClanUModGetClanResult
    {
        public string tag;
        public string owner;
        public JArray moderators;
        public JArray members;
        public JArray invited;
        public JArray allies;
        public JArray invitedallies;
    }

    public List<string> GetClanMembersUmod(BasePlayer player);
    public List<string> GetClanMembersOvh(BasePlayer player);
    public List<string> GetFriendsOvh(BasePlayer player);
    public List<string> GetFriendsOxide(BasePlayer player);
    public List<string> GetFriendsApi(BasePlayer player);
    public List<string> GetFriendsAp(BasePlayer player);
    public List<string> GetFriends(BasePlayer player);
    public List<string> GetClanMembers(BasePlayer player);
    private void AuthOnEntity(BasePlayer player, string where, bool deAuth, Action<T, BasePlayer> callback);
    private static List<T> GetPlayerEnitityByType(BasePlayer player);
    private static string parseColor(string where, int pos);
    private static string HexToRustFormat(string hex);
    [ChatCommand("auth")]
    private void auth(BasePlayer player);
    private void DeAuthOnEntity(BasePlayer player, List<string> contragents, Action<T, BasePlayer> callback);
    private void DeAuthAll(BasePlayer player, List<string> contragents);
    [ConsoleCommand("UI_LimitHandler")]
    private void cmdConsoleHandler(ConsoleSystem.Arg args);
    private const string MainLayer;
    private void UI_DrawMainLayer(BasePlayer player);
}

private class AuthManager
{
    public string PictureURL;
    public string Command;
    public AuthManager(string pictureUrl, string command);
}

private class AuthConfig
{
    [JsonProperty("Лимит авторизаций в шкафу")]
    public int CupboardLimit;
    [JsonProperty("Лимит авторизаций в замках")]
    public int CodelockLimit;
    [JsonProperty("Лимит авторизаций в турелях")]
    public int TurretLimit;
    [JsonProperty("Разрешить авторизацию кланам")]
    public bool ClanAllow;
    [JsonProperty("Разрешить авторизацию друзьям")]
    public bool FriendsAllow;
}

 class ClanUModGetClanResult
{
    public string tag;
    public string owner;
    public JArray moderators;
    public JArray members;
    public JArray invited;
    public JArray allies;
    public JArray invitedallies;
}


```

---

## LimitedLadders

```csharp
using System.Linq;
using System.Text.RegularExpressions;
using Oxide.Core.Plugins;
using UnityEngine;

Oxide.Plugins
[Info("LimitedLadders", "DefaultPlayer(VVoid)", "1.0.2", ResourceId = 1051)]
public class LimitedLadders : RustPlugin
{
    private const string LadderPrefabs;
    private readonly int LayerMasks;
    private bool DisableOnlyOnConstructions;
    private string BuildingBlockedMsg;
    private bool AllowOnExternalWalls;
    private void Cfg(string key, T var);
    private void Init();
    protected override void LoadDefaultConfig();
    private void OnServerInitialized();
    [HookMethod("OnEntityBuilt")]
    private void OnEntityBuilt(HeldEntity heldentity, GameObject gameobject);
    private static void TryReturnLadder(BasePlayer player, BaseCombatEntity entity);
}


```

---

## LimitedSuicide

```csharp
using Oxide.Core.Plugins;
using System;
using System.Collections.Generic;

Oxide.Plugins
[Info("LimitedSuicide", "playrust.io / dcode", "1.0.2", ResourceId = 835)]
public class LimitedSuicide : RustPlugin
{
    private static int defaultLimit;
    private static int defaultTimespan;
    private int limit;
    private int timespan;
    private Dictionary<ulong, List<DateTime>> suicides;
    protected override void LoadDefaultConfig();
    [HookMethod("OnServerInitialized")]
     void OnServerInitialized();
    [HookMethod("OnRunCommand")]
     object OnRunCommand(ConsoleSystem.Arg arg);
}


```

---

## LimitedTurrets

```csharp
using System.Collections.Generic;
using System.Text;
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using UnityEngine;

Oxide.Plugins
[Info("LimitedTurrets", "own3r/rever", "1.0.1")]
[Description("Ограничение турелей")]
 class LimitedTurrets : RustPlugin
{
    [PluginReference]
    private Plugin ImageLibrary;
    public Dictionary<string, string> turretTypes;
    public Dictionary<ulong, Dictionary<string, int>> turrets;
    public class Configuration
    {
        [JsonProperty(PropertyName = "Версия конфига (не менять)")]
        public int version;
        [JsonProperty(PropertyName = "Привилегии")]
        public Dictionary<string, Dictionary<string, int>> privelegies { get; set; }
        [JsonProperty(PropertyName = "Разрешить привилегию limitedturrets.bypass для игнорирования лимитов")]
        public bool useBypass;
        [JsonProperty(PropertyName = "Разрешить админам игнорировать лимиты")]
        public bool allowAdmin;
        [JsonProperty(PropertyName = "Команда для открытия UI")]
        public string showUICmd;
        [JsonProperty(PropertyName = "Разрешить UI")]
        public bool allowUI;
        [JsonProperty(PropertyName = "Разрешить информирование в чате о изменении лимита")]
        public bool allowNotify;
    }

    public Configuration config;
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private void Loaded();
    private void OnServerInitialized();
     void Unload();
     void UpdatePermissions(string id, string name);
     void OnUserPermissionRevoked(string id, string perm);
     void OnUserPermissionGranted(string id, string perm);
     bool CheckBypass(BasePlayer player);
    public void ShowUIPanel(BasePlayer player);
    private void cmdShowUI(BasePlayer player, string command, string[] args);
    [ConsoleCommand("LTUIPClose")]
     void cmdLTUIPClose(ConsoleSystem.Arg arg);
    [ConsoleCommand("lt.add")]
     void cmdAddPrivelege(ConsoleSystem.Arg arg);
    [ConsoleCommand("lt.remove")]
     void cmdRemovePrivelege(ConsoleSystem.Arg arg);
    [ConsoleCommand("lt.flush")]
     void cmdFlushUser(ConsoleSystem.Arg arg);
     string GetTurretType(string turretName);
    public int GetTurretCount(string playerId, string type);
     void CheckTurret(BaseEntity entity);
     void SaveState();
     object CanBuild(Planner planner, Construction prefab, Construction.Target target);
     void OnEntityDeath(BaseCombatEntity entity, HitInfo info);
     bool CanPickupEntity(BasePlayer player, BaseCombatEntity entity);
     void OnNewSave(string filename);
     void OnServerSave();
}

public class Configuration
{
    [JsonProperty(PropertyName = "Версия конфига (не менять)")]
    public int version;
    [JsonProperty(PropertyName = "Привилегии")]
    public Dictionary<string, Dictionary<string, int>> privelegies { get; set; }
    [JsonProperty(PropertyName = "Разрешить привилегию limitedturrets.bypass для игнорирования лимитов")]
    public bool useBypass;
    [JsonProperty(PropertyName = "Разрешить админам игнорировать лимиты")]
    public bool allowAdmin;
    [JsonProperty(PropertyName = "Команда для открытия UI")]
    public string showUICmd;
    [JsonProperty(PropertyName = "Разрешить UI")]
    public bool allowUI;
    [JsonProperty(PropertyName = "Разрешить информирование в чате о изменении лимита")]
    public bool allowNotify;
}


```

---

## LoadingMessages

```csharp
using System.Collections.Generic;
using System.Linq;
using Network;
using Newtonsoft.Json;
using UnityEngine;

Oxide.Plugins
[Info("LoadingMessages", "VVoid", "1.0.1", ResourceId = 2762)]
[Description("Shows custom texts on loading screen.")]
public class LoadingMessages : RustPlugin
{
    private readonly Dictionary<ulong, Connection> _clients;
    private readonly List<ulong> _disconnectedClients;
    private MsgConfig _config;
    private Timer _timer;
    private MsgEntry _currentMsg;
    private int _msgIdx;
    private float _nextMsgChange;
    private class MsgConfig
    {
        [JsonProperty("Text Display Frequency (Seconds)")]
        public float TimerFreq;
        [JsonProperty("Enable Messages Cyclicity")]
        public bool EnableCyclicity;
        [JsonProperty("Use Random Cyclicity (Instead of sequential)")]
        public bool EnableRandomCyclicity;
        [JsonProperty("Cycle Messages Every ~N Seconds")]
        public float CyclicityFreq;
        [JsonProperty("Messages")]
        public List<MsgEntry> Messages;
        [JsonProperty("Last Message (When entering game)")]
        public MsgEntry LastMessage;
    }

    private class MsgEntry
    {
        [JsonProperty("Top Status")]
        public string TopString;
        [JsonProperty("Bottom Status")]
        public string BottomString;
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private void Init();
    private void OnUserApprove(Connection connection);
    private void OnPlayerInit(BasePlayer player);
    private void UpdateCurrentMessage();
    private void HandleClients();
    private static void DisplayMessage(Connection con, MsgEntry msgEntry);
    private static T PickRandom(List<T> list);
}

private class MsgConfig
{
    [JsonProperty("Text Display Frequency (Seconds)")]
    public float TimerFreq;
    [JsonProperty("Enable Messages Cyclicity")]
    public bool EnableCyclicity;
    [JsonProperty("Use Random Cyclicity (Instead of sequential)")]
    public bool EnableRandomCyclicity;
    [JsonProperty("Cycle Messages Every ~N Seconds")]
    public float CyclicityFreq;
    [JsonProperty("Messages")]
    public List<MsgEntry> Messages;
    [JsonProperty("Last Message (When entering game)")]
    public MsgEntry LastMessage;
}

private class MsgEntry
{
    [JsonProperty("Top Status")]
    public string TopString;
    [JsonProperty("Bottom Status")]
    public string BottomString;
}


```

---

## Logger

```csharp
using System;
using System.Collections.Generic;
using System.Text.RegularExpressions;

Oxide.Plugins
[Info("Logger", "Wulf/lukespragg", "1.2.0", ResourceId = 670)]
[Description("Configurable logging of chat, commands, and more")]
 class Logger : RustPlugin
{
     List<object> exclusions;
     bool logChat;
     bool logCommands;
     bool logConnections;
     bool logRespawns;
     bool logToConsole;
    protected override void LoadDefaultConfig();
     void Init();
     void LoadDefaultMessages();
     void OnPlayerChat(ConsoleSystem.Arg arg);
     void OnPlayerConnected(Network.Message packet);
     void OnPlayerDisconnected(BasePlayer player);
     void OnPlayerRespawned(BasePlayer player);
     void OnServerCommand(ConsoleSystem.Arg arg);
     T GetConfig(string name, T value);
     string Lang(string key, string id, object[] args);
    static string IpAddress(string ip);
     void Log(string fileName, string text);
}


```

---

## Logo

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
[Info("Logo", "poof", "1.0.0")]
public class Logo : RustPlugin
{
     string LogoName;
    private void OnServerInitialized();
     void Unload();
    private void OnPlayerInit(BasePlayer player);
    private void OnPlayerDisconnected(BasePlayer player, string reason);
     void DrawInterface(BasePlayer player);
    private static string HexToRustFormat(string hex);
}


```

---

## LogoMenu

```csharp
using System;
using System.Collections;
using System.Collections.Generic;
using System.Globalization;
using System.Linq;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using UnityEngine;

Oxide.Plugins
[Info("LogoMenu", "", "1.1.0")]
public class LogoMenu : RustPlugin
{
    [PluginReference]
    private Plugin ImageLibrary;
    public List<BasePlayer> PlayersTime;
    public string porno;
    public string Layer;
    public string LayerOnline;
    public string LayerButton;
    private const string Porno;
    public float FadeIn;
    public float FadeOut;
    private Dictionary<string, string> Images;
    private void OnServerInitialized();
     void OnPlayerConnected(BasePlayer player);
     void OnPlayerDisconnected(BasePlayer player);
    private void UpdatePlayersUI();
     void Unload();
    [ConsoleCommand("logo.menu.open")]
    private void CMD_ClickMenu(ConsoleSystem.Arg arg);
     void OnlineTextUI(BasePlayer player);
     void MenuUI(BasePlayer player);
    private void RefreshUI(BasePlayer player, string Type);
     void OnEntitySpawned(BaseNetworkable entity);
     void OnEntityKill(BaseNetworkable entity);
    private void RefreshUI(string tag);
     string HexToRustFormat(string hex);
     bool IsCargoPlane();
     bool IsBaseHelicopter();
     bool IsBradleyAPC();
     bool IsCH47Helicopter();
}


```

---

## LogoMenu2

```csharp
using System;
using System.Collections;
using System.Collections.Generic;
using System.Globalization;
using System.Linq;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using UnityEngine;

Oxide.Plugins
[Info("LogoMenu", "", "1.1.0")]
public class LogoMenu : RustPlugin
{
    [PluginReference]
    private Plugin ImageLibrary;
    public List<BasePlayer> PlayersTime;
    public string Layer;
    public string LayerOnline;
    public string LayerButton;
    public float FadeIn;
    public float FadeOut;
    private Dictionary<string, string> Images;
    private void OnServerInitialized();
     void OnPlayerConnected(BasePlayer player);
     void OnPlayerDisconnected(BasePlayer player);
    private void UpdatePlayerUI();
     void Unload();
    [ConsoleCommand("logo.menu.open")]
    private void CMD_ClickMenu(ConsoleSystem.Arg arg);
     void OnlineTextUI(BasePlayer player);
     void MenuUI(BasePlayer player);
    private void RefreshUI(BasePlayer player, string Type);
     void OnEntitySpawned(BaseNetworkable entity);
     void OnEntityKill(BaseNetworkable entity);
    private void RefreshUI(string tag);
     string HexToRustFormat(string hex);
     bool IsCargoPlane();
     bool IsBaseHelicopter();
     bool IsBradleyAPC();
     bool IsCH47Helicopter();
}


```

---

## LogoStorm

```csharp
using System;
using System.Collections.Generic;
using Network;
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Libraries.Covalence;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using UnityEngine;

Oxide.Plugins
[Info("StormLogo", "Ryamkk", "2.2.0")]
public class LogoStorm : RustPlugin
{
    [PluginReference]
    private Plugin ImageLibrary;
     string logo;
     string open;
    public static string Commnad1;
    public static string Commnad2;
    public static string Commnad3;
    public static string Commnad4;
    private string GetImage(string fileName, ulong skin);
    public bool AddImage(string url, string shortname, ulong skin);
     void LoadImage();
    private const string Layer;
     List<string> MSGList;
    private PluginData _data;
    private void SaveData();
    private void LoadData();
    private class PluginData
    {
        [JsonProperty(PropertyName = "Hided Players", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<ulong> HidedPlayers;
        public bool IsHided(BasePlayer player);
        public bool ChangeStatus(BasePlayer player);
    }

     void Loaded();
    [ChatCommand("online")]
     void CommanndOnline(BasePlayer sender, string command, string[] args);
    [ChatCommand("player")]
     void CommandPlayer(BasePlayer sender, string command, string[] args);
    [ChatCommand("players")]
     void CommandPlayers(BasePlayer sender, string command, string[] args);
    private void OnServerInitialized();
    private void Unload();
     void OnClientAuth(Connection connection);
    private void OnPlayerConnected(BasePlayer player);
    private void OnPlayerDisconnected(BasePlayer player);
    private void CmdMenuHide(IPlayer cov, string command, string[] args);
    private void MainUi(BasePlayer player);
    private void RefreshOnline();
    private void DrawLower(BasePlayer player);
    private static string HexToRustFormat(string hex);
}

private class PluginData
{
    [JsonProperty(PropertyName = "Hided Players", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<ulong> HidedPlayers;
    public bool IsHided(BasePlayer player);
    public bool ChangeStatus(BasePlayer player);
}


```

---

## LootCleaner

```csharp
using Newtonsoft.Json;
using System.Collections.Generic;

Oxide.Plugins
[Info("Loot Cleaner", "walkinrey", "1.0.2")]
 class LootCleaner : RustPlugin
{
     Configuration config;
     class Configuration
    {
        [JsonProperty("Через сколько секунд удалять ящик, если его не до конца облутал игрок?")]
        public float seconds;
    }

    protected override void SaveConfig();
    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
     void OnLootEntityEnd(BasePlayer player, BaseCombatEntity entity);
}

 class Configuration
{
    [JsonProperty("Через сколько секунд удалять ящик, если его не до конца облутал игрок?")]
    public float seconds;
}


```

---

## LootLogs

```csharp
using System;
using System.Collections.Generic;
using Oxide.Core;
using UnityEngine;
using System.Reflection;

Oxide.Plugins
[Info("LootLogs", "k1lly0u", "0.1.3", ResourceId = 2065)]
 class LootLogs : RustPlugin
{
     Dictionary<uint, StorageType> itemTracker;
    private FieldInfo serverinput;
    private bool isInit;
     void Loaded();
     void OnServerInitialized();
    private void OnEntityDeath(BaseEntity entity, HitInfo hitInfo);
     void OnItemAddedToContainer(ItemContainer container, Item item);
     void OnItemRemovedFromContainer(ItemContainer container, Item item);
     class StorageType
    {
        public string entityName;
        public string entityID;
        public string itemName;
        public int itemAmount;
        public string type;
    }

    private BaseEntity FindEntity(BasePlayer player);
    private object Ray(Vector3 Pos, Vector3 Aim);
     void Log(string playername, string item, string type, string id, string entityname, bool take);
     void DeathLog(string type, string entityname, string id, string position, string killer);
    [ChatCommand("findid")]
     void cmdFindID(BasePlayer player, string command, string[] args);
     string msg(string key, string id);
     Dictionary<string, string> Messages;
}

 class StorageType
{
    public string entityName;
    public string entityID;
    public string itemName;
    public int itemAmount;
    public string type;
}


```

---

## LootProtection

```csharp
using System.Collections.Generic;

Oxide.Plugins
[Info("LootProtection", "Wulf/lukespragg", "0.5.0", ResourceId = 1150)]
[Description("Protects corpses and/or sleepers with permission from being looted by other players")]
 class LootProtection : CovalencePlugin
{
    const string permBypass;
    const string permCorpse;
    const string permSleeper;
     void Init();
     void OnServerInitialized();
     void LoadDefaultMessages();
     object OnLootEntity(BasePlayer looter, BaseEntity entity);
     string Lang(string key, string id, object[] args);
}


```

---

## LootScaling

```csharp
using UnityEngine;
using Oxide.Core.Plugins;
using System.Collections;
using System.Collections.Generic;

Oxide.Plugins
[Info("Loot Scaling", "Kyrah Abattoir", "0.1", ResourceId = 1874)]
[Description("Scale loot spawn rate/density by player count.")]
 class LootScaling : RustPlugin
{
     List<string> cfgPopulationScaling;
    [HookMethod("OnServerInitialized")]
     void OnServerInitialized();
}


```

---

## Lottery

```csharp
using System;
using System.Collections.Generic;
using System.Globalization;
using System.Linq;
using Oxide.Core;
using Oxide.Core.Configuration;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using UnityEngine;

Oxide.Plugins
[Info("Lottery", "Sami37", "1.0.3", ResourceId = 2145)]
internal class Lottery : RustPlugin
{
    [PluginReference("Economics")]
     Plugin Economy;
    [PluginReference("ServerRewards")]
     Plugin ServerRewards;
    internal class playerinfo
    {
        public int multiplicator { get; set; }
        public double currentbet { get; set; }
        public double totalbet { get; set; }
    }

     int[] GetIntArray(int num);
    private bool newConfig;
    private bool UseSR;
    public Dictionary<ulong, playerinfo> Currentbet;
    private string container;
    private string containerwin;
    private DynamicConfigFile data;
    private double jackpot;
    private double SRMinBet;
    private double SRjackpot;
    private double MinBetjackpot;
    private int JackpotNumber;
    private int SRJackpotNumber;
    private int DefaultMaxRange;
    private int DefaultMinRange;
    public Dictionary<string, int> IndividualRates { get; set; }
    public Dictionary<string, int> SRRates { get; set; }
    private Dictionary<string, object> DefaultWinRates;
    private Dictionary<string, int> SRWinRates;
    private List<string> DefaultBasePoint;
     object GetConfig(string menu, string datavalue, object defaultValue);
    protected override void LoadDefaultConfig();
     void LoadConfig();
    static Dictionary<string, object> DefaultPay();
    static List<string> DefaultSRPay();
    static Dictionary<string, int> DefautSRWinPay();
     void LoadDefaultMessages();
     void OnServerInitialized();
     void Unload();
     void SaveData(Dictionary<ulong, playerinfo> datas);
     void GUIDestroy(BasePlayer player);
     void ShowLotery(BasePlayer player, string[] args);
    private static CuiElementContainer ChangeBonusPage(int pageless, int pagemore);
    public double FindReward(BasePlayer player, int bet, int reference, int multiplicator);
    [ChatCommand("lot")]
    private void cmdLotery(BasePlayer player, string command, string[] args);
    [ConsoleCommand("cmdDestroyUI")]
     void cmdDestroyUI(ConsoleSystem.Arg arg);
    [ConsoleCommand("cmdLess")]
     void cmdLess(ConsoleSystem.Arg arg);
    [ConsoleCommand("cmdBet")]
     void cmdBet(ConsoleSystem.Arg arg);
    [ConsoleCommand("cmdPlus")]
     void cmdPlus(ConsoleSystem.Arg arg);
    [ConsoleCommand("cmdPage")]
     void cmdPage(ConsoleSystem.Arg arg);
    [ConsoleCommand("cmdPlaceBet")]
     void cmdPlaceBet(ConsoleSystem.Arg arg);
}

internal class playerinfo
{
    public int multiplicator { get; set; }
    public double currentbet { get; set; }
    public double totalbet { get; set; }
}


```

---

## LotterySystem

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
[Info("LotterySystem", "Chibubrik", "1.1.1")]
 class LotterySystem : RustPlugin
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
    [ChatCommand("lottery")]
    private void ChatLottery(BasePlayer player);
    [ConsoleCommand("lottery")]
    private void ConsoleLottery(ConsoleSystem.Arg args);
    private void LotteryUI(BasePlayer player);
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

## Loyalty

```csharp
using System.Collections.Generic;
using Oxide.Core;
using System;
using System.Linq;
using System.Text.RegularExpressions;

Oxide.Plugins
[Info("Loyalty", "Bamabo", "1.3.1")]
[Description("Reward your players for play time with new permissions/usergroups")]
 class Loyalty : RustPlugin
{
     Data data;
     class Data
    {
        public Dictionary<ulong, Player> players;
        public HashSet<UserGroup> usergroups;
        public HashSet<LoyaltyReward> rewards;
        public Data();
    }

    public class LoyaltyReward
    {
        public string alias { get; set; }
        public string permission { get; set; }
        public uint requirement { get; set; }
        public LoyaltyReward();
        public LoyaltyReward(string alias, string permission, uint requirement);
    }

    public class Player
    {
        public string name { get; set; }
        public ulong id { get; set; }
        public uint loyalty { get; set; }
        public string group { get; set; }
        public Player();
        public Player(ulong id, string name, uint loyalty, string group);
        public override bool Equals(object obj);
        public override int GetHashCode();
    }

    public class UserGroup
    {
        public string usergroup { get; set; }
        public uint requirement { get; set; }
        public UserGroup();
        public UserGroup(string usergroup, uint requirement);
    }

     void Init();
     void Loaded();
     void Unload();
     void OnPlayerInit(BasePlayer player);
    [ChatCommand("loyalty")]
     void CmdLoyalty(BasePlayer sender, string command, string[] args);
     void CmdAdd(BasePlayer sender, string req, string perm, string alias);
     void CmdRemove(BasePlayer sender, string permission);
     void CmdReset(BasePlayer sender, string playerName);
     void CmdSet(BasePlayer sender, string playerName, string newLoyalty);
     void CmdLookup(BasePlayer sender, string player);
     void CmdTop(BasePlayer sender);
     void CmdRewards(BasePlayer sender);
     void CmdRewardsg(BasePlayer sender);
     void CmdAddUserGroup(BasePlayer sender, string requirement, string usergroup);
     void CmdRemoveUserGroup(BasePlayer sender, string usergroup);
     void SendMessage(BasePlayer receiver, string messageID, object[] args);
     void SendErrorMessage(BasePlayer receiver, string messageID, object[] args);
     void SendMessageFromID(BasePlayer receiver, string messageID, ulong senderID, object[] args);
     string FormatMessage(string messageID, object[] args);
     bool RewardExists(string permission);
     bool UserGroupExists(string usergroup);
     void RegisterMessages();
     void RegisterPermissions();
    protected override void LoadDefaultConfig();
}

 class Data
{
    public Dictionary<ulong, Player> players;
    public HashSet<UserGroup> usergroups;
    public HashSet<LoyaltyReward> rewards;
    public Data();
}

public class LoyaltyReward
{
    public string alias { get; set; }
    public string permission { get; set; }
    public uint requirement { get; set; }
    public LoyaltyReward();
    public LoyaltyReward(string alias, string permission, uint requirement);
}

public class Player
{
    public string name { get; set; }
    public ulong id { get; set; }
    public uint loyalty { get; set; }
    public string group { get; set; }
    public Player();
    public Player(ulong id, string name, uint loyalty, string group);
    public override bool Equals(object obj);
    public override int GetHashCode();
}

public class UserGroup
{
    public string usergroup { get; set; }
    public uint requirement { get; set; }
    public UserGroup();
    public UserGroup(string usergroup, uint requirement);
}


```

---

## LustyPort

```csharp
using UnityEngine;
using System;
using System.Reflection;
using System.Collections;
using System.Collections.Generic;
using Oxide.Core;
using Oxide.Core.Plugins;

Oxide.Plugins
[Info("LustyPort", "Kayzor", "1.1.11", ResourceId = 1250)]
[Description("A simple teleportation plugin!")]
public class LustyPort : RustPlugin
{
     string lustyPlugin;
     string lustyAuthor;
     string lustyVersion;
     string lustyDescription;
     List<LustyPorts> lustyPorts;
     List<LustyPlayers> lustyPlayers;
     List<LustyTeleportings> teleportingPlayers;
     int teleportDuration;
     void Init();
    [ChatCommand("tp")]
    private void tpCmd(BasePlayer player, string command, string[] args);
    [ChatCommand("tp_add")]
    private void tpAdd(BasePlayer player, string command, string[] args);
    [ChatCommand("tp_del")]
    private void tpDel(BasePlayer player, string command, string[] args);
    [ChatCommand("tp_back")]
    private void tpBack(BasePlayer player, string command, string[] args);
    [ChatCommand("tp_list")]
    private void tpList(BasePlayer player, string command, string[] args);
    [ChatCommand("tp_about")]
    private void tpAbout(BasePlayer player, string command, string[] args);
    [ChatCommand("tp_help")]
    private void tpHelp(BasePlayer player, string command, string[] args);
    private class LustyTeleportings
    {
        public ulong userid { get; set; }
        public DateTime starttime { get; set; }
        public float x { get; set; }
        public float y { get; set; }
        public float z { get; set; }
        public LustyPorts lustyPort { get; set; }
        public bool back { get; set; }
    }

    private class LustyPlayers
    {
        public ulong userid { get; set; }
        public LustyPorts lustyPort { get; set; }
    }

    private void savePorts();
    private void savePlayers();
    private class LustyPorts
    {
        public string name { get; set; }
        public float x { get; set; }
        public float y { get; set; }
        public float z { get; set; }
        public bool admin { get; set; }
    }

    private LustyPorts findPort(string name);
    private void addPort(BasePlayer player, string name, bool admin);
    private void delPort(BasePlayer player, string name);
    private void tpPort(BasePlayer player, bool back, string name);
    private void startPort(BasePlayer player, LustyPorts lustyPort, bool back);
    private LustyTeleportings findTeleportingPlayer(ulong userid);
    private BasePlayer findPlayer(ulong userid);
    private void timerPort();
    private void addPlayer(BasePlayer player);
    private bool checkTeleport(BasePlayer player, LustyTeleportings teleportingPlayer);
    private void noArg(BasePlayer player, string command);
    private void playerMsg(BasePlayer player, string msg);
     bool isAdmin(BasePlayer player);
     void TeleportPlayerPosition(BasePlayer player, Vector3 destination);
}

private class LustyTeleportings
{
    public ulong userid { get; set; }
    public DateTime starttime { get; set; }
    public float x { get; set; }
    public float y { get; set; }
    public float z { get; set; }
    public LustyPorts lustyPort { get; set; }
    public bool back { get; set; }
}

private class LustyPlayers
{
    public ulong userid { get; set; }
    public LustyPorts lustyPort { get; set; }
}

private class LustyPorts
{
    public string name { get; set; }
    public float x { get; set; }
    public float y { get; set; }
    public float z { get; set; }
    public bool admin { get; set; }
}


```

---

## MachiningTools

```csharp
using Facepunch;
using Newtonsoft.Json;
using Oxide.Core.Libraries.Covalence;
using Oxide.Core.Plugins;
using System.Collections.Generic;
using System.Linq;

Oxide.Plugins
[Info("MachiningTools", "BlackPlugin.ru", "1.3.1", ResourceId = 89)]
[Description("Creates tools that would gather refined materials from the resource nodes")]
 class MachiningTools : RustPlugin
{
    private PluginConfig _config;
    private readonly Dictionary<ItemDefinition, ItemDefinition> _itemToCookable;
    private class Tool
    {
        [JsonProperty("Короткое имя предмета")]
        public string ShortName;
        [JsonProperty("ID скина предмета (Поддерживается Workshop)")]
        public ulong SkinId;
        [JsonProperty("Название предмета (Выводится в описании предмета в инвентаре)")]
        public string Name;
        [JsonProperty("Можно ли ремонтировать предмет")]
        public bool CanRepair;
        [JsonProperty("Можно ли перерабатывать предмет")]
        public bool CanRecycle;
        [JsonProperty("Настройки переработки")]
        public Transmutations Transmutation;
        [JsonProperty("Устанавливать символ огня в углу предмета")]
        public bool? ShoutSetFire;
        private ItemDefinition _info;
        public bool ItemExists();
        public Item Create(int amount);
        public int GetCustomHash();
        public static int GetCustomHash(Item item);
    }

    private class Transmutations
    {
        [JsonProperty("Перерабатывать дерево в уголь")]
        private bool _wood;
        [JsonProperty("Перерабатывать руду МВК в металл")]
        private bool _hqm;
        [JsonProperty("Перерабатывать металлическую руду в фрагменты")]
        private bool _metal;
        [JsonProperty("Перерабатывать серную руду в серу")]
        private bool _sulfur;
        [JsonProperty("Перерабатывать мясо медведя в жаренное")]
        private bool _bear;
        [JsonProperty("Перерабатывать свинину в жаренную")]
        private bool _boar;
        [JsonProperty("Перерабатывать мясо курицы в жаренное")]
        private bool _chicken;
        [JsonProperty("Перерабатывать мясо лошади в жаренное")]
        private bool _horse;
        [JsonProperty("Перерабатывать мясо волка в жаренное")]
        private bool _wolf;
        [JsonProperty("Перерабатывать мясо оленя в жаренное")]
        private bool _deer;
        [JsonProperty("Перерабатывать человеческое мясо в жаренное")]
        private bool _human;
        public static Transmutations DefaultPick { get; set; }
        public static Transmutations DefaultAxe { get; set; }
        public bool ShouldCook(Item item);
    }

    private class PluginConfig
    {
        [JsonProperty("Привилегия для использования команд")]
        public string Permission;
        [JsonProperty("Команда(чат/консоль)")]
        public string Command;
        [JsonProperty("Список инструментов")]
        private Dictionary<string, Tool> _tools;
        [JsonIgnore]
        private readonly Dictionary<int,KeyValuePair<string, Tool>> _toolsByHash;
        [JsonIgnore]
        public readonly Dictionary<string, Tool> ToolsByKey;
        public static PluginConfig DefaultConfig { get; set; }
        public string CheckConfig(RustPlugin plugin);
        public bool TryGet(Item item, Tool tool);
        private bool TryGet(int hash, KeyValuePair<string, Tool> pair);
    }

    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    protected override void SaveConfig();
     void Init();
     void OnServerInitialized();
     void OnOvenToggle(BaseOven oven, BasePlayer player);
     void OnItemAddedToContainer(ItemContainer container, Item item);
     object OnItemAction(Item item, string action, BasePlayer player);
     object OnItemSkinChange(int skin, Item item, RepairBench bench, BasePlayer player);
     object OnItemRepair(BasePlayer player, Item item);
     object CanRecycle(Recycler recycler, Item item);
     void OnDispenserGather(ResourceDispenser dispenser, BasePlayer player, Item item);
     void OnDispenserBonus(ResourceDispenser dispenser, BasePlayer player, Item item);
    private object SB_CanReskinItem(BasePlayer player, Item item);
    private object SB_CanAcceptItem(BasePlayer player, Item item);
    private void Reply(IPlayer player, string langKey, object[] args);
    private void Reply(BasePlayer player, string langKey, object[] args);
    private string GetMsg(string langKey, string userId);
    protected override void LoadDefaultMessages();
    private void GiveToolsCommand(IPlayer player, string command, string[] args);
    [HookMethod("IsMachiningToolItem")]
     bool IsMachiningToolItem(Item item);
    private void Transmute(Item item);
}

private class Tool
{
    [JsonProperty("Короткое имя предмета")]
    public string ShortName;
    [JsonProperty("ID скина предмета (Поддерживается Workshop)")]
    public ulong SkinId;
    [JsonProperty("Название предмета (Выводится в описании предмета в инвентаре)")]
    public string Name;
    [JsonProperty("Можно ли ремонтировать предмет")]
    public bool CanRepair;
    [JsonProperty("Можно ли перерабатывать предмет")]
    public bool CanRecycle;
    [JsonProperty("Настройки переработки")]
    public Transmutations Transmutation;
    [JsonProperty("Устанавливать символ огня в углу предмета")]
    public bool? ShoutSetFire;
    private ItemDefinition _info;
    public bool ItemExists();
    public Item Create(int amount);
    public int GetCustomHash();
    public static int GetCustomHash(Item item);
}

private class Transmutations
{
    [JsonProperty("Перерабатывать дерево в уголь")]
    private bool _wood;
    [JsonProperty("Перерабатывать руду МВК в металл")]
    private bool _hqm;
    [JsonProperty("Перерабатывать металлическую руду в фрагменты")]
    private bool _metal;
    [JsonProperty("Перерабатывать серную руду в серу")]
    private bool _sulfur;
    [JsonProperty("Перерабатывать мясо медведя в жаренное")]
    private bool _bear;
    [JsonProperty("Перерабатывать свинину в жаренную")]
    private bool _boar;
    [JsonProperty("Перерабатывать мясо курицы в жаренное")]
    private bool _chicken;
    [JsonProperty("Перерабатывать мясо лошади в жаренное")]
    private bool _horse;
    [JsonProperty("Перерабатывать мясо волка в жаренное")]
    private bool _wolf;
    [JsonProperty("Перерабатывать мясо оленя в жаренное")]
    private bool _deer;
    [JsonProperty("Перерабатывать человеческое мясо в жаренное")]
    private bool _human;
    public static Transmutations DefaultPick { get; set; }
    public static Transmutations DefaultAxe { get; set; }
    public bool ShouldCook(Item item);
}

private class PluginConfig
{
    [JsonProperty("Привилегия для использования команд")]
    public string Permission;
    [JsonProperty("Команда(чат/консоль)")]
    public string Command;
    [JsonProperty("Список инструментов")]
    private Dictionary<string, Tool> _tools;
    [JsonIgnore]
    private readonly Dictionary<int,KeyValuePair<string, Tool>> _toolsByHash;
    [JsonIgnore]
    public readonly Dictionary<string, Tool> ToolsByKey;
    public static PluginConfig DefaultConfig { get; set; }
    public string CheckConfig(RustPlugin plugin);
    public bool TryGet(Item item, Tool tool);
    private bool TryGet(int hash, KeyValuePair<string, Tool> pair);
}


```

---

## MagazinBoost

```csharp
using System;
using System.Collections;
using System.Collections.Generic;
using System.Linq;
using System.Reflection;
using UnityEngine;
using Oxide.Game.Rust;
using Oxide.Core;

Oxide.Plugins
[Info("MagazinBoost", "Fujikura", "1.2.1", ResourceId = 1962)]
[Description("Can change magazines, ammo and conditon for most projectile weapons")]
public class MagazinBoost : RustPlugin
{
    private bool Changed;
     StoredWeapons storedWeapons;
    private FieldInfo _itemCondition;
    private FieldInfo _itemMaxCondition;
    private string permissionAll;
    private string permissionMaxAmmo;
    private string permissionPreLoad;
    private string permissionMaxCondition;
    private string permissionAmmoType;
    private bool checkPermission;
    private bool removeSkinIfNoRights;
    private object GetConfig(string menu, string datavalue, object defaultValue);
     void LoadVariables();
    protected override void LoadDefaultConfig();
    private void SetupDefaultWeapons();
     class StoredWeapons
    {
        public List<WeaponInventory> weaponStatsList;
        public StoredWeapons();
    }

     class WeaponInventory
    {
        private FieldInfo _itemMaxCon;
        public List<WeaponStats> weaponStatsContent;
        public WeaponInventory();
        public WeaponInventory(List<WeaponStats> list);
        public WeaponInventory(string name, string displayname, int maxammo, int preload, int maxcondition, string ammotype, ulong skinid);
        public int InventorySize();
        public List<WeaponStats> GetweaponStatsContent();
    }

    private class WeaponStats
    {
        public string name;
        public string displayname;
        public int maxammo;
        public int preload;
        public int maxcondition;
        public string ammotype;
        public ulong skinid;
        public bool settingactive;
        public int servermaxammo;
        public int serverpreload;
        public string serverammotype;
        public int servermaxcondition;
        public bool serveractive;
        public WeaponStats();
        public WeaponStats(string name, string displayname, int maxammo, int preload, int maxcondition, string ammotype, ulong skinid);
    }

    private void LoadWeaponData();
    private void SaveWeaponData();
    private WeaponStats craftedWeapon(string name);
    private bool hasAnyRight(BasePlayer player);
    private bool hasRight(BasePlayer player, string perm);
    private void OnServerInitialized();
    private void OnItemCraftFinished(ItemCraftTask task, Item item);
    private void OnItemAddedToContainer(ItemContainer container, Item item);
}

 class StoredWeapons
{
    public List<WeaponInventory> weaponStatsList;
    public StoredWeapons();
}

 class WeaponInventory
{
    private FieldInfo _itemMaxCon;
    public List<WeaponStats> weaponStatsContent;
    public WeaponInventory();
    public WeaponInventory(List<WeaponStats> list);
    public WeaponInventory(string name, string displayname, int maxammo, int preload, int maxcondition, string ammotype, ulong skinid);
    public int InventorySize();
    public List<WeaponStats> GetweaponStatsContent();
}

private class WeaponStats
{
    public string name;
    public string displayname;
    public int maxammo;
    public int preload;
    public int maxcondition;
    public string ammotype;
    public ulong skinid;
    public bool settingactive;
    public int servermaxammo;
    public int serverpreload;
    public string serverammotype;
    public int servermaxcondition;
    public bool serveractive;
    public WeaponStats();
    public WeaponStats(string name, string displayname, int maxammo, int preload, int maxcondition, string ammotype, ulong skinid);
}


```

---

## MagicArea

```csharp
using System.Collections.Generic;
using System;
using UnityEngine;
using Oxide.Core;
using Oxide.Core.Plugins;
using Oxide.Core.Configuration;

Oxide.Plugins
[Info("MagicArea", "Norn", 0.1, ResourceId = 1551)]
[Description("Areas to practice building/pvp.")]
public class MagicArea : RustPlugin
{
    [PluginReference]
     Plugin Kits;
    [PluginReference]
     Plugin MagicTeleportation;
     class MA
    {
        public Dictionary<int, AreaInfo> Areas;
        public Dictionary<ulong, PlayerData> PlayerData;
        public Dictionary<uint, AreaEntities> Entities;
        public MA();
    }

    public class AreaEntities
    {
        public uint iID;
        public int iAreaID;
        public ulong uCreatorID;
        public int iCreated;
        public int iExpire;
        public AreaEntities();
    }

     class PlayerData
    {
        public ulong uUserID;
        public int iInArea;
        public Int32 iInitStamp;
        public PlayerData();
    }

     class SpawnInfo
    {
        public float fX;
        public float fY;
        public float fZ;
        public int iEID;
        public SpawnInfo();
    }

     class AreaInfo
    {
        public int iID;
        public string tTitle;
        public string tDescription;
        public float fMinX;
        public float fMinY;
        public float fMinZ;
        public float fRadius;
        public bool uEnabled;
        public bool bGod;
        public bool bResetInv;
        public int iCount;
        public string tKit;
        public bool bCanResearch;
        public bool bRemoveEntities;
        public int iEntityExpire;
        public Dictionary<int, SpawnInfo> Spawns;
        public AreaInfo();
    }

     MA MAData;
     void LoadDefaultMessages();
     string GetMessage(string key, string steamId);
     Timer AreaSync;
     void OnServerInitialized();
    private void AreaTimer();
    private void OnPlayerExitMagicArea(BasePlayer player, int area);
    private void OnPlayerEnterMagicArea(BasePlayer player, int area);
    private int CreateMagicArea(Vector3 position, float radius, string title, string description, bool enabled);
    private bool MagicAreaExists(int id);
    public static DateTime UnixTimeStampToDateTime(double unixTimeStamp);
    private void OnEntityBuilt(Planner planner, GameObject gameObject);
     void OnEntityDeath(BaseCombatEntity entity, HitInfo info);
     void OnEntitySpawned(BaseNetworkable entity);
    private object OnItemResearch(Item item, BasePlayer player);
    [ConsoleCommand("area.create")]
    private void ccmdCreateArea(ConsoleSystem.Arg arg);
    private object OnPlayerRespawned(BasePlayer player);
    private bool PlayerToPoint(BasePlayer player, float radi, float x, float y, float z);
     void Loaded();
     void Unload();
     void SaveData();
    private bool PlayerExists(BasePlayer player);
    private bool InitPlayer(BasePlayer player);
    private bool IsPlayerInArea(BasePlayer player, float MinX, float MinY, float MaxX, float MaxY);
    protected override void LoadDefaultConfig();
    private HitInfo OnEntityTakeDamage(BaseCombatEntity entity, HitInfo hitInfo);
     object OnStructureUpgrade(BuildingBlock block, BasePlayer player, BuildingGrade.Enum grade);
    public static Int32 UnixTimeStampUTC();
    public static int GetRandomNumber(int min, int max);
}

 class MA
{
    public Dictionary<int, AreaInfo> Areas;
    public Dictionary<ulong, PlayerData> PlayerData;
    public Dictionary<uint, AreaEntities> Entities;
    public MA();
}

public class AreaEntities
{
    public uint iID;
    public int iAreaID;
    public ulong uCreatorID;
    public int iCreated;
    public int iExpire;
    public AreaEntities();
}

 class PlayerData
{
    public ulong uUserID;
    public int iInArea;
    public Int32 iInitStamp;
    public PlayerData();
}

 class SpawnInfo
{
    public float fX;
    public float fY;
    public float fZ;
    public int iEID;
    public SpawnInfo();
}

 class AreaInfo
{
    public int iID;
    public string tTitle;
    public string tDescription;
    public float fMinX;
    public float fMinY;
    public float fMinZ;
    public float fRadius;
    public bool uEnabled;
    public bool bGod;
    public bool bResetInv;
    public int iCount;
    public string tKit;
    public bool bCanResearch;
    public bool bRemoveEntities;
    public int iEntityExpire;
    public Dictionary<int, SpawnInfo> Spawns;
    public AreaInfo();
}


```

---

## MagicChat

```csharp
using System;
using System.Collections.Generic;
using Rust;
using Oxide.Core;
using Oxide.Core.Plugins;

Oxide.Plugins
[Info("MagicChat", "Norn", 0.1, ResourceId = 1437)]
[Description("An alternative chat system.")]
public class MagicChat : RustPlugin
{
     string DEFAULT_COLOR;
    [PluginReference]
     Plugin PopupNotifications;
     class StoredData
    {
        public Dictionary<ulong, UserInfo> Users;
        public StoredData();
    }

     class UserInfo
    {
        public ulong iUserId;
        public string tLastName;
        public int iWorld;
        public bool uVoiceMuted;
        public bool uPublicMuted;
        public bool uLocalMuted;
        public bool uCanColor;
        public string tColor;
        public bool uColorEnabled;
        public bool uCanCustomTag;
        public bool uCustomTagEnabled;
        public string tCustomTag;
        public int iMessagesSent;
        public int iInitTimestamp;
        public bool uShowIcon;
        public bool uIconStatus;
        public int iLastSeenTimestamp;
        public bool uShowPublicChat;
        public UserInfo();
    }

     StoredData MCData;
    private void Loaded();
     void OnServerInitialized();
     void Unload();
     string GetUserTag(BasePlayer player);
     bool UserUpdateColor(BasePlayer player, string color);
     bool UserUpdateTag(BasePlayer player, string tag);
    [ChatCommand("chat")]
    private void ChatCommand(BasePlayer player, string command, string[] args);
     void OnPlayerSleepEnded(BasePlayer player);
     void OnPlayerInit(BasePlayer player);
    public static Int32 UnixTimeStampUTC();
    private object OnPlayerChat(ConsoleSystem.Arg arg);
    private void UserTextPublic(BasePlayer player, string text);
    private string UserNameColor(BasePlayer i);
    private void UserTextRadius(BasePlayer player, double radius, string text, string name_color);
     void OnPlayerDisconnected(BasePlayer player, string reason);
    private bool InitUserData(BasePlayer player, bool debug);
    private bool CanUserToggleSteamIcon(BasePlayer player);
    private bool CanUserCustomColor(BasePlayer player);
    private bool UserColorToggle(BasePlayer player);
    private bool UserTagToggle(BasePlayer player);
    private bool UserTagEnabled(BasePlayer player);
    private bool CanUserCustomTag(BasePlayer player);
    private bool UserDataExists(BasePlayer player);
     void SaveData();
    protected override void LoadDefaultConfig();
}

 class StoredData
{
    public Dictionary<ulong, UserInfo> Users;
    public StoredData();
}

 class UserInfo
{
    public ulong iUserId;
    public string tLastName;
    public int iWorld;
    public bool uVoiceMuted;
    public bool uPublicMuted;
    public bool uLocalMuted;
    public bool uCanColor;
    public string tColor;
    public bool uColorEnabled;
    public bool uCanCustomTag;
    public bool uCustomTagEnabled;
    public string tCustomTag;
    public int iMessagesSent;
    public int iInitTimestamp;
    public bool uShowIcon;
    public bool uIconStatus;
    public int iLastSeenTimestamp;
    public bool uShowPublicChat;
    public UserInfo();
}


```

---

## MagicCraft

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using Rust;
using Oxide.Core;
using Oxide.Core.Plugins;

Oxide.Plugins
[Info("MagicCraft", "Norn", "0.2.7", ResourceId = 1347)]
[Description("An alternative crafting system.")]
public class MagicCraft : RustPlugin
{
     int MaxB;
     int MinB;
     int MAX_INV_SLOTS;
     string Lang(string key, string id, object[] args);
    [PluginReference]
     Plugin PopupNotifications;
     class StoredData
    {
        public Dictionary<string, CraftInfo> CraftList;
        public StoredData();
    }

     class CraftInfo
    {
        public int MaxBulkCraft;
        public int MinBulkCraft;
        public string displayName;
        public string shortName;
        public string description;
        public bool Enabled;
        public CraftInfo();
    }

     StoredData storedData;
    private void ConfigurationCheck();
     void Loaded();
    private void OnServerInitialized();
    protected override void LoadDefaultConfig();
     void GenerateItems(bool reset);
    public static Int32 UnixTimeStampUTC();
    public int InventorySlots(BasePlayer player, bool incwear, bool incbelt);
     void OnPlayerDisconnected(BasePlayer player, string reason);
     List<string> ExcludeList;
    public int FreeInventorySlots(BasePlayer player, bool incwear, bool incbelt);
    private Dictionary<string, ItemDefinition> mcITEMS;
    private Dictionary<BasePlayer, Int32> BulkCraftCooldown;
    private object OnItemCraft(ItemCraftTask task, BasePlayer crafter);
    private IEnumerable<int> CalculateStacks(int amount, ItemDefinition item);
    private bool SAFEGiveItem(BasePlayer player, int itemid, ulong skinid, int amount);
    private void PrintToChatEx(BasePlayer player, string result, string tcolour);
    private ItemDefinition GetItem(string shortname);
}

 class StoredData
{
    public Dictionary<string, CraftInfo> CraftList;
    public StoredData();
}

 class CraftInfo
{
    public int MaxBulkCraft;
    public int MinBulkCraft;
    public string displayName;
    public string shortName;
    public string description;
    public bool Enabled;
    public CraftInfo();
}


```

---

## MagicDescription

```csharp
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Text.RegularExpressions;

Oxide.Plugins
[Info("MagicDescription", "Wulf/lukespragg", "1.2.3", ResourceId = 1447)]
[Description("Adds dynamic information in the server description")]
 class MagicDescription : RustPlugin
{
    readonly Regex varRegex;
     List<object> exclusions;
     bool listPlugins;
     bool serverInitialized;
     int updateInterval;
     string description;
    protected override void LoadDefaultConfig();
     void OnServerInitialized();
     void OnPluginLoaded();
     void OnPluginUnloaded();
     void UpdateDescription(string text);
     object OnServerCommand(ConsoleSystem.Arg arg);
     T GetConfig(string name, T value);
}


```

---

## MagicHammer

```csharp
using System;
using System.Collections.Generic;
using Rust;
using Oxide.Core;
using Oxide.Core.Plugins;
using System.Reflection;

Oxide.Plugins
[Info("MagicHammer", "Norn", 0.4, ResourceId = 1375)]
[Description("Hit stuff with the hammer and do things.")]
public class MagicHammer : RustPlugin
{
     int MODE_REPAIR;
     int MODE_DESTROY;
     int MAX_MODES;
    [PluginReference]
     Plugin PopupNotifications;
     class StoredData
    {
        public Dictionary<ulong, MagicHammerInfo> Users;
        public StoredData();
    }

     class MagicHammerInfo
    {
        public ulong UserId;
        public int Mode;
        public bool Enabled;
        public bool Messages_Enabled;
        public MagicHammerInfo();
    }

     StoredData hammerUserData;
    static FieldInfo buildingPriv;
     void Loaded();
     void OnPlayerInit(BasePlayer player);
     bool InitPlayerData(BasePlayer player);
    protected override void LoadDefaultConfig();
     bool CanMagicHammer(BasePlayer player);
    private void PrintToChatEx(BasePlayer player, string result, string tcolour);
     void Unload();
     void SaveData();
     int GetPlayerHammerMode(BasePlayer player);
     bool SetPlayerHammerMode(BasePlayer player, int mode);
     bool SetPlayerHammerStatus(BasePlayer player, bool enabled);
     bool MagicHammerEnabled(BasePlayer player);
    [ChatCommand("mh")]
     void cmdMH(BasePlayer player, string cmd, string[] args);
     void OnStructureRepairEx(BuildingBlock block, BasePlayer player);
    static void RemoveEntity(BaseEntity entity);
    static bool hasTotalAccess(BasePlayer player);
    private void OnServerInitialized();
     void OnStructureRepair(BuildingBlock block, BasePlayer player);
}

 class StoredData
{
    public Dictionary<ulong, MagicHammerInfo> Users;
    public StoredData();
}

 class MagicHammerInfo
{
    public ulong UserId;
    public int Mode;
    public bool Enabled;
    public bool Messages_Enabled;
    public MagicHammerInfo();
}


```

---

## MagicMeat

```csharp
using System.Collections.Generic;
using System;
using Rust;

Oxide.Plugins
[Info("MagicMeat", "ignignokt84", "0.1.1", ResourceId = 2011)]
 class MagicMeat : RustPlugin
{
     void LoadDefaultMessages();
    private Dictionary<Option,object> data;
    private bool hasConfigChanged;
    public string usageString;
    private object[] def;
    private Dictionary<int,int> recipes;
     void Loaded();
     string GetMessage(string key, string userId);
     void ccmdDelegator(ConsoleSystem.Arg arg);
    private bool handleSet(ConsoleSystem.Arg arg);
    private bool handleGet(ConsoleSystem.Arg arg);
     void showUsage(ConsoleSystem.Arg arg);
    private void printValue(ConsoleSystem.Arg arg, Option opt);
    protected override void LoadDefaultConfig();
    private void LoadConfig();
    private object GetConfig(object opt, object defaultValue);
    private void SaveEntry(Option opt, object value);
     void OnItemAddedToContainer(ItemContainer container, Item item);
    static string wrapSize(int size, string input);
    static string wrapColor(string color, string input);
    private bool getBool(Option opt);
    private float getFloat(Option opt);
     void sendMessage(BasePlayer player, string message);
    private bool isAdmin(BasePlayer player);
     bool hasAccess(ConsoleSystem.Arg arg);
}


```

---

## MagicSigns

```csharp
using System;
using System.Collections.Generic;
using Oxide.Core;
using Oxide.Core.Libraries;
using System.Text.RegularExpressions;
using UnityEngine;
using System.Collections;
using System.Linq;
using System.IO;

Oxide.Plugins
[Info("MagicSigns", "Norn", 0.4, ResourceId = 1446)]
[Description("Random signs.")]
public class MagicSigns : RustPlugin
{
     bool INIT;
    private readonly WebRequests scrapeQueue;
    static GameObject WebObject;
    static UnityWeb UWeb;
     uint MaxSize;
     class QueueItem
    {
        public string url;
        public Signage sign;
        public BasePlayer sender;
        public bool raw;
        public QueueItem(string ur, BasePlayer se, Signage si, bool raw);
    }

     class UnityWeb : MonoBehaviour
    {
        internal static bool ConsoleLog;
        internal static string ConsoleLogMsg;
        internal static int MaxActiveLoads;
        private Queue<QueueItem> QueueList;
        static byte ActiveLoads;
        private MemoryStream stream;
         byte JPGCompression;
        public void Add(string url, BasePlayer player, Signage s, bool raw);
         void Next();
        private void ClearStream();
         byte[] GetImageBytes(WWW www);
         IEnumerator WaitForRequest(QueueItem info);
    }

     List<string> ScrapedImages;
    private void ParseScrapeResponse(int code, string response);
    private void PopulateImageList();
    protected override void LoadDefaultConfig();
    [ChatCommand("ms")]
    private void ChatCommand(BasePlayer player, string command, string[] args);
    private void OnEntityBuilt(Planner planner, GameObject gameObject);
     void Unload();
     System.Random rnd;
    protected int GetRandomInt(int min, int max);
     int DEFAULT_COOLDOWN;
     Dictionary<ulong, int> Cooldown;
    private void CooldownTimer();
    private void InitCooldown(BasePlayer player);
     void Loaded();
     Timer COOLDOWN_TIMER;
     void OnServerInitialized();
}

 class QueueItem
{
    public string url;
    public Signage sign;
    public BasePlayer sender;
    public bool raw;
    public QueueItem(string ur, BasePlayer se, Signage si, bool raw);
}

 class UnityWeb : MonoBehaviour
{
    internal static bool ConsoleLog;
    internal static string ConsoleLogMsg;
    internal static int MaxActiveLoads;
    private Queue<QueueItem> QueueList;
    static byte ActiveLoads;
    private MemoryStream stream;
     byte JPGCompression;
    public void Add(string url, BasePlayer player, Signage s, bool raw);
     void Next();
    private void ClearStream();
     byte[] GetImageBytes(WWW www);
     IEnumerator WaitForRequest(QueueItem info);
}


```

---

## MagicTeleportation

```csharp
using System;
using System.Collections.Generic;
using Oxide.Core;
using UnityEngine;
using Oxide.Core.Plugins;
using Rust;

Oxide.Plugins
[Info("MagicTeleportation", "Norn", 1.1, ResourceId = 1404)]
[Description("Teleportation system.")]
public class MagicTeleportation : RustPlugin
{
    [PluginReference]
     Plugin PopupNotifications;
    [PluginReference]
     Plugin BuildingOwners;
     class StoredData
    {
        public Dictionary<ulong, PlayerData> PlayerData;
        public Dictionary<string, HomeEntities> Entities;
        public Dictionary<int, TeleportInfo> Teleports;
        public StoredData();
    }

    public class TeleportInfo
    {
        public int iID;
        public string tTitle;
        public string tDescription;
        public float fX;
        public float fY;
        public float fZ;
        public bool uEnabled;
        public int iAuthLevel;
        public bool uSleepGod;
        public int iCount;
        public TeleportInfo();
    }

     class HomesLocs
    {
        public float fX;
        public float fY;
        public float fZ;
        public float fHealth;
        public int iEID;
        public string tBagName;
        public HomesLocs();
    }

     class PlayerData
    {
        public ulong uUserID;
        public bool uCooldownEnabled;
        public Dictionary<int, HomesLocs> HomesLocs;
        public PlayerData();
    }

    public class HomeEntities
    {
        public string tPrefab_name;
        public string tShortname;
        public bool bEnabled;
        public HomeEntities();
    }

     Dictionary<string, string> DEFAULT_HomeEntities;
     StoredData MTData;
    private void Loaded();
    public static Int32 UnixTimeStampUTC();
    public static int GetRandomNumber(int min, int max);
    private int CreateTeleport(string title, string description, float X, float Y, float Z, bool sleepgod, int authlevel, bool enabled);
    private bool PlayerExists(BasePlayer player);
    private int ResetCooldownForAll();
    private int PlayerHomeCount(BasePlayer player);
    private bool InitPlayer(BasePlayer player);
    private bool EntityPlayerCheck(uint netid, BasePlayer attacker);
    private bool EntityExists(BaseCombatEntity entity);
    private void OnHomeEntityDeath(BaseCombatEntity entity, HitInfo info);
     List<ulong> SLEEPING_TELEPORTERS;
     Dictionary<ulong, Timer> TELEPORT_QUEUE;
    private HitInfo OnEntityTakeDamage(BaseCombatEntity vic, HitInfo hitInfo);
     bool CancelTeleport(BasePlayer player);
     void OnEntityDeath(BaseCombatEntity entity, HitInfo info);
    private static Dictionary<string, string> displaynameToShortname;
    private static Dictionary<string, int> deployedToItem;
    private void InitializeTable();
    private void RefundHomeEntity(BasePlayer player, BaseEntity entity, int amount);
    private void OnEntityBuilt(Planner planner, GameObject gameObject);
     Dictionary<string, object> GetSleepingBagData(SleepingBag bag);
     List<Dictionary<string, object>> FindSleepingBags(ulong userid);
    private bool UnfreezePlayer(ulong steamid);
    private void CommandMessage(BasePlayer player);
    [ChatCommand("t")]
     void cmdHome(BasePlayer player, string cmd, string[] args);
    private bool InitTeleport(BasePlayer player, float init_x, float init_y, float init_z, bool type, bool printtoplayer, string title, string description, int count, int seconds);
    private void TeleportPlayerPosition(BasePlayer player, Vector3 pos);
     void Unload();
     void OnPlayerInit(BasePlayer player);
     void OnPlayerSleepEnded(BasePlayer player);
     void OnPlayerDisconnected(BasePlayer player, string reason);
     Timer SyncHomeData;
     void OnServerInitialized();
     void InitializeConfig();
     void SyncHomesEx(BasePlayer player);
     void SyncHomes();
     void SaveData();
    private string IsTeleportationCapable(BasePlayer player);
    private string FindPlayerName(ulong userId);
    private void GetDeployedItemOwner(BasePlayer player, SleepingBag ditem);
    private object FindOwnerBlock(BuildingBlock block);
    protected override void LoadDefaultConfig();
     int PopulateEntityData();
    private void PrintToChatEx(BasePlayer player, string result, string tcolour);
}

 class StoredData
{
    public Dictionary<ulong, PlayerData> PlayerData;
    public Dictionary<string, HomeEntities> Entities;
    public Dictionary<int, TeleportInfo> Teleports;
    public StoredData();
}

public class TeleportInfo
{
    public int iID;
    public string tTitle;
    public string tDescription;
    public float fX;
    public float fY;
    public float fZ;
    public bool uEnabled;
    public int iAuthLevel;
    public bool uSleepGod;
    public int iCount;
    public TeleportInfo();
}

 class HomesLocs
{
    public float fX;
    public float fY;
    public float fZ;
    public float fHealth;
    public int iEID;
    public string tBagName;
    public HomesLocs();
}

 class PlayerData
{
    public ulong uUserID;
    public bool uCooldownEnabled;
    public Dictionary<int, HomesLocs> HomesLocs;
    public PlayerData();
}

public class HomeEntities
{
    public string tPrefab_name;
    public string tShortname;
    public bool bEnabled;
    public HomeEntities();
}


```

---

## MagicTree

```csharp
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Game.Rust.Cui;
using System;
using System.Collections.Generic;
using System.Linq;
using UnityEngine;

Oxide.Plugins
[Info("MagiсTree", "OxideBro", "1.0.1")]
public class MagicTree : RustPlugin
{
    public class Seed
    {
        public string shortname;
        public string name;
        public ulong skinId;
    }

    public class Wood
    {
        [JsonProperty("UID Дерева")]
        public uint woodId;
        [JsonProperty("Осталось времени")]
        public int NeedTime;
        [JsonProperty("Этап")]
        public int CurrentStage;
        [JsonProperty("Позиция")]
        public Vector3 woodPos;
        [JsonProperty("Ящики")]
        public Dictionary<uint, BoxItemsList> BoxListed;
    }

    public class BoxItemsList
    {
        [JsonProperty("Shortname предмета")]
        public string ShortName;
        [JsonProperty("Минимальное количество")]
        public int MinAmount;
        [JsonProperty("Максимальное количество")]
        public int MaxAmount;
        [JsonProperty("Шанс что предмет будет добавлен (максимально 100%)")]
        public int Change;
        [JsonProperty("SkinID предмета")]
        public ulong SkinID;
        [JsonProperty("Имя предмета при создании (Оставьте поле пустым чтобы использовать стандартное название итема)")]
        public string Name;
        [JsonProperty("Это чертеж")]
        public bool IsBlueprnt;
        [JsonIgnore]
        public BaseEntity box;
    }

    public Dictionary<ulong, Dictionary<uint, Wood>> WoodsList;
    public Dictionary<string, string> Messages;
    private PluginConfig config;
    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    private void UpdateConfigValues();
    protected override void SaveConfig();
    private class PluginConfig
    {
        [JsonProperty("Время роста дерева в секундах")]
        public int Time;
        [JsonProperty("Количество вещей в ящике")]
        public int ItemsCount;
        [JsonProperty("Кол-во ящиков на дереве")]
        public int BoxCount;
        [JsonProperty("Список префабов этапов дерева")]
        public List<string> Stages;
        [JsonProperty("Права на выдачу")]
        public string Permission;
        [JsonProperty("Тип ящика")]
        public string CrateBasic;
        [JsonProperty("Шанс выпадения зерна с дерева (макс-100)")]
        public int Chance;
        [JsonProperty("Настройка лута в ящиках")]
        public List<BoxItemsList> casesItems;
        [JsonProperty("Ссылка на удачный эффект")]
        public string SucEffect;
        [JsonProperty("Ссылка на эффект ошибки")]
        public string ErrorEffect;
        [JsonProperty("Настройка зерна")]
        public Seed seed;
        [JsonProperty("Версия конфигурации")]
        public VersionNumber PluginVersion;
        public static PluginConfig DefaultConfig();
    }

     void LoadData();
     void SaveData();
    public static MagicTree ins;
     void OnEntityKill(BaseNetworkable entity);
    private void OnServerInitialized();
     void AddOrRemoveComponent(string type, TreeConponent component, BaseEntity tree, ulong playerid);
     void OnEntityBuilt(Planner planner, GameObject gameobject, Vector3 Pos);
     object OnDispenserBonus(ResourceDispenser dispenser, BasePlayer player, Item item);
     object OnDispenserGather(ResourceDispenser dispenser, BaseEntity entity, Item item);
     void Unload();
    public void SpawnWood(ulong player, Vector3 pos, BaseEntity tree, BaseEntity seed);
    [ChatCommand("seed")]
     void GiveSeed(BasePlayer player, string command, string[] args);
     void AddSeed(BasePlayer player, int amount);
    public void SpawnBox(Wood wood, int i, BaseEntity tree, ulong ownerID, int countBox);
    public void AddLoot(BaseEntity box);
     class TreeConponent : BaseEntity
    {
        public Dictionary<BasePlayer, bool> ColliderPlayersList;
        public BaseEntity tree;
         SphereCollider sphereCollider;
        public Wood data;
         void Awake();
        public void Init(Wood wood);
        private void OnTriggerEnter(Collider other);
        private void OnTriggerExit(Collider other);
         void DrawInfo();
         void SetPlayerFlag(BasePlayer player, BasePlayer.PlayerFlags f, bool b);
        public static string FormatShortTime(TimeSpan time);
        private static string Format(int units, string form1, string form2, string form3);
        public void DestroyComponent();
         void OnDestroy();
    }

    static void CreateInfo(ulong playeId);
}

public class Seed
{
    public string shortname;
    public string name;
    public ulong skinId;
}

public class Wood
{
    [JsonProperty("UID Дерева")]
    public uint woodId;
    [JsonProperty("Осталось времени")]
    public int NeedTime;
    [JsonProperty("Этап")]
    public int CurrentStage;
    [JsonProperty("Позиция")]
    public Vector3 woodPos;
    [JsonProperty("Ящики")]
    public Dictionary<uint, BoxItemsList> BoxListed;
}

public class BoxItemsList
{
    [JsonProperty("Shortname предмета")]
    public string ShortName;
    [JsonProperty("Минимальное количество")]
    public int MinAmount;
    [JsonProperty("Максимальное количество")]
    public int MaxAmount;
    [JsonProperty("Шанс что предмет будет добавлен (максимально 100%)")]
    public int Change;
    [JsonProperty("SkinID предмета")]
    public ulong SkinID;
    [JsonProperty("Имя предмета при создании (Оставьте поле пустым чтобы использовать стандартное название итема)")]
    public string Name;
    [JsonProperty("Это чертеж")]
    public bool IsBlueprnt;
    [JsonIgnore]
    public BaseEntity box;
}

private class PluginConfig
{
    [JsonProperty("Время роста дерева в секундах")]
    public int Time;
    [JsonProperty("Количество вещей в ящике")]
    public int ItemsCount;
    [JsonProperty("Кол-во ящиков на дереве")]
    public int BoxCount;
    [JsonProperty("Список префабов этапов дерева")]
    public List<string> Stages;
    [JsonProperty("Права на выдачу")]
    public string Permission;
    [JsonProperty("Тип ящика")]
    public string CrateBasic;
    [JsonProperty("Шанс выпадения зерна с дерева (макс-100)")]
    public int Chance;
    [JsonProperty("Настройка лута в ящиках")]
    public List<BoxItemsList> casesItems;
    [JsonProperty("Ссылка на удачный эффект")]
    public string SucEffect;
    [JsonProperty("Ссылка на эффект ошибки")]
    public string ErrorEffect;
    [JsonProperty("Настройка зерна")]
    public Seed seed;
    [JsonProperty("Версия конфигурации")]
    public VersionNumber PluginVersion;
    public static PluginConfig DefaultConfig();
}

 class TreeConponent : BaseEntity
{
    public Dictionary<BasePlayer, bool> ColliderPlayersList;
    public BaseEntity tree;
     SphereCollider sphereCollider;
    public Wood data;
     void Awake();
    public void Init(Wood wood);
    private void OnTriggerEnter(Collider other);
    private void OnTriggerExit(Collider other);
     void DrawInfo();
     void SetPlayerFlag(BasePlayer player, BasePlayer.PlayerFlags f, bool b);
    public static string FormatShortTime(TimeSpan time);
    private static string Format(int units, string form1, string form2, string form3);
    public void DestroyComponent();
     void OnDestroy();
}


```

---

## MagicVariables

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using Rust;
using Oxide.Core.Plugins;

Oxide.Plugins
[Info("MagicVariables", "Norn", 0.1, ResourceId = 1419)]
[Description("Simple static variable system.")]
public class MagicVariables : RustPlugin
{
    public Int32 UnixTimeStampUTC();
    static readonly DateTime UnixEpoch;
    static readonly double MaxUnixSeconds;
    public static DateTime UnixTimeStampToDateTime(double unixTimeStamp);
    public class StaticVariables
    {
        public int iResourceID;
        public Dictionary<string, object> Variables;
        public Hash<ulong, Dictionary<string, object>> UserVariables;
        public Dictionary<ulong, PlayersInit> Users;
        public StaticVariables();
    }

    public class PlayersInit
    {
        public ulong uUserID;
        public string tDisplayName;
        public PlayersInit();
    }

    public class PluginInfo
    {
        public Dictionary<Plugin, StaticVariables> Plugins;
        public PluginInfo();
    }

     PluginInfo Data;
    private bool PluginExists(Plugin plugin);
    private bool SetStaticVariable(Plugin plugin, string variable, string value, bool debug);
    private string GetStaticVariable(Plugin plugin, string variable, bool debug);
    private bool RemoveStaticVariable(Plugin plugin, string variable, bool debug);
    private bool DestroyPlugin(Plugin plugin, bool debug);
    private bool InitPlugin(Plugin plugin, bool debug);
     void OnServerInitialized();
    private bool RemoveStaticPlayerVariable(Plugin plugin, BasePlayer player, string variable, bool debug);
    private string GetStaticPlayerVariable(Plugin plugin, BasePlayer player, string variable, bool debug);
    private void SetStaticPlayerVariable(Plugin plugin, BasePlayer player, string variable, string value, bool debug);
    private int PlayerHasVariables(Plugin plugin, BasePlayer player);
    private bool InitPlayer(Plugin plugin, BasePlayer player, bool debug);
    private bool RemovePlayer(Plugin plugin, BasePlayer player, bool debug);
    private bool PlayerExists(Plugin plugin, BasePlayer player);
    protected override void LoadDefaultConfig();
}

public class StaticVariables
{
    public int iResourceID;
    public Dictionary<string, object> Variables;
    public Hash<ulong, Dictionary<string, object>> UserVariables;
    public Dictionary<ulong, PlayersInit> Users;
    public StaticVariables();
}

public class PlayersInit
{
    public ulong uUserID;
    public string tDisplayName;
    public PlayersInit();
}

public class PluginInfo
{
    public Dictionary<Plugin, StaticVariables> Plugins;
    public PluginInfo();
}


```

---

## MagixLogo-1.0.0

```csharp
using System.Reflection.Metadata;
using Newtonsoft.Json;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using System;
using System.Collections.Generic;
using System.Globalization;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using UnityEngine;
using Oxide.Core;
using UnityEngine.UIElements;

Oxide.Plugins
[Info("MagixLogo", "Frizen", "1.0.0")]
public class MagixLogo : RustPlugin
{
    [PluginReference]
    private Plugin ImageLibrary;
    private Plugin EventRandomizer;
    private Plugin NoEscape;
    private Plugin FreeOnline;
    private Plugin MagixNotify;
    private Dictionary<BasePlayer, bool> _menuOpen;
    public static System.Random Random;
    private float randomhashed;
    private const string _menuLayer;
    private bool _isCargoShip;
    private bool _isBradley;
    private bool _isCh47;
    private bool _isHeli;
    private const float fadeout;
    private const float fadein;
    private static Configuration _config;
    public class Configuration
    {
        [JsonProperty("Цвет активного ивента Bradley")]
        public string BradleyColor;
        [JsonProperty("Цвет активного ивента Ch47")]
        public string Ch47Color;
        [JsonProperty("Цвет активного ивента Heli")]
        public string HeliColor;
        [JsonProperty("Время уведомления рейдблока в секундах")]
        public float NotifyTime;
        [JsonProperty("Цвет активного ивента Cargo")]
        public string CargoColor;
        [JsonProperty("Список команд в выпадающем меню")]
        public Dictionary<string, string> MenuCmds;
        [JsonProperty("Картинки")]
        public Dictionary<string, string> Imgs;
        public static Configuration DefaultConfig();
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private void OnServerInitialized();
     float GetOnline();
     void OnPlayerSleep(BasePlayer player);
     void OnPlayerSleepEnded(BasePlayer player);
     void OnPlayerConnected(BasePlayer player);
     void OnPlayerDisconnected(BasePlayer player);
     void OnEntitySpawned(BaseNetworkable entity);
     void OnEntityKill(BaseEntity entity);
     void OnEntityDeath(BaseEntity entity, HitInfo info);
     void Unload();
    private void FirstCheckEvents();
    private void DrawMenu(BasePlayer player);
    private void DestroyButtons(BasePlayer player);
    private void ButtonUi(BasePlayer player);
    private void OnlineUi(BasePlayer player);
    private void SendNotify(BasePlayer player, string message, int show, string type);
    private void RefreshEvents(BasePlayer player, string events);
    [HookMethod("RaidBlockMagix")]
    public void RaidBlockUi(BasePlayer player, double time);
    [HookMethod("IsOpen")]
    public bool IsOpened(BasePlayer player);
    private static string HexToRustFormat(string hex);
    [ConsoleCommand("magix.logo")]
    private void MenuHandler(ConsoleSystem.Arg args);
    [ConsoleCommand("magix.info")]
    private void InfoHandler(ConsoleSystem.Arg args);
    private string GetFormatTime(TimeSpan timespan);
}

public class Configuration
{
    [JsonProperty("Цвет активного ивента Bradley")]
    public string BradleyColor;
    [JsonProperty("Цвет активного ивента Ch47")]
    public string Ch47Color;
    [JsonProperty("Цвет активного ивента Heli")]
    public string HeliColor;
    [JsonProperty("Время уведомления рейдблока в секундах")]
    public float NotifyTime;
    [JsonProperty("Цвет активного ивента Cargo")]
    public string CargoColor;
    [JsonProperty("Список команд в выпадающем меню")]
    public Dictionary<string, string> MenuCmds;
    [JsonProperty("Картинки")]
    public Dictionary<string, string> Imgs;
    public static Configuration DefaultConfig();
}


```

---

## Mail

```csharp
using UnityEngine;
using System.Collections;
using Oxide.Core;
using System.Collections.Generic;
using System;
using Newtonsoft.Json;
using Oxide.Game.Rust.Cui;
using System.Linq;

Oxide.Plugins
[Info("Mail", "Anonymuspro", "0.0.1")]
public class Mail : RustPlugin
{
    public class MailType
    {
        [JsonProperty("Скин")]
        public ulong skinId;
        [JsonProperty("Название в инвентаре")]
        public string name;
        [JsonProperty("Модифицировать лут?(использовать ли лут из списка, на чертежах не работает)")]
        public bool mod;
        [JsonProperty("Список лута(чертежей)")]
        public Dictionary<string, int> itemlist;
        [JsonProperty("Выдавать чертежи")]
        public bool blueprint;
        [JsonProperty("Кол-во вещей")]
        public int itemCount;
    }

    public class Data
    {
        public DateTime last;
        public int CurrentTime;
        public bool sended;
    }

    public class Text
    {
        [JsonProperty("Отступ для 3д текста")]
        public Vector3 pos;
        [JsonProperty("Цвет 3д текста")]
        public string color;
        [JsonProperty("Размер 3д текста")]
        public string size;
        [JsonProperty("3д Текст")]
        public string text;
    }

    public class ConfigData
    {
        [JsonProperty("Настройки 3д текста")]
        public Text txt;
        [JsonProperty("Настройка писем")]
        public List<MailType> types;
        [JsonProperty("Время до прихода письма")]
        public int Time;
    }

    public string mailBox;
    public string item;
    public Dictionary<ulong, Data> data;
    public List<BaseEntity> currentMails;
    public ConfigData cfg;
    protected override void LoadConfig();
    protected override void SaveConfig();
    protected override void LoadDefaultConfig();
     void OnServerInitialized();
     void Unload();
     void OnLootEntity(BasePlayer player, BaseEntity entity);
    private object OnItemAction(Item item, string action);
     void OnEntityDeath(BaseCombatEntity entity, HitInfo info);
     void OnEntitySpawned(BaseNetworkable entity);
    public void MailTick();
    public string TimeToString(double time);
    public string GetText(Data d, StorageContainer l);
    public void ShowText(BaseEntity entity, BasePlayer player);
    private void GiveThink(BasePlayer player, Item item);
    private static void ItemRemovalThink(Item item, BasePlayer player, int itemsToTake);
}

public class MailType
{
    [JsonProperty("Скин")]
    public ulong skinId;
    [JsonProperty("Название в инвентаре")]
    public string name;
    [JsonProperty("Модифицировать лут?(использовать ли лут из списка, на чертежах не работает)")]
    public bool mod;
    [JsonProperty("Список лута(чертежей)")]
    public Dictionary<string, int> itemlist;
    [JsonProperty("Выдавать чертежи")]
    public bool blueprint;
    [JsonProperty("Кол-во вещей")]
    public int itemCount;
}

public class Data
{
    public DateTime last;
    public int CurrentTime;
    public bool sended;
}

public class Text
{
    [JsonProperty("Отступ для 3д текста")]
    public Vector3 pos;
    [JsonProperty("Цвет 3д текста")]
    public string color;
    [JsonProperty("Размер 3д текста")]
    public string size;
    [JsonProperty("3д Текст")]
    public string text;
}

public class ConfigData
{
    [JsonProperty("Настройки 3д текста")]
    public Text txt;
    [JsonProperty("Настройка писем")]
    public List<MailType> types;
    [JsonProperty("Время до прихода письма")]
    public int Time;
}


```

---

## MainMenu

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
[Info("MainMenu", "OxideBro", "1.1.11")]
public class MainMenu : RustPlugin
{
    private PluginConfig config;
    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    private void UpdateConfigValues();
    protected override void SaveConfig();
     void OnPlayerConnected(BasePlayer player);
     class DefaultButton
    {
        [JsonProperty("Позиция AnchorMin")]
        public string AnchorMin;
        [JsonProperty("Позиция AnchorMax")]
        public string AnchorMax;
        [JsonProperty("Текст")]
        public string Text;
        [JsonProperty("Настройка изображения")]
        public Images ImagesSetting;
        [JsonProperty("Выполняемая команда (Если это чатовая команда, используйте через слэш)")]
        public string Command;
        [JsonProperty("Цвет кнопки")]
        public string Color;
    }

    public class Images
    {
        [JsonProperty("Ссылка на изображение")]
        public string ImageURL;
        [JsonProperty("Позиция AnchorMin")]
        public string AnchorMin;
        [JsonProperty("Позиция AnchorMax")]
        public string AnchorMax;
        [JsonProperty("Цвет изображения")]
        public string Color;
    }

     class UserAvatar
    {
        [JsonProperty("Позиция AnchorMin")]
        public string AnchorMin;
        [JsonProperty("Позиция AnchorMax")]
        public string AnchorMax;
        [JsonProperty("Прозрачность")]
        public string Color;
    }

     class MainSettings
    {
        [JsonProperty("Показывать при подключении")]
        public bool OpenToConnection;
        [JsonProperty("Показывать только при первом подключении")]
        public bool OpenToConnectionFirst;
        [JsonProperty("Команды для открытия меню")]
        public List<string> Commands;
    }

     class HomeSettings
    {
        [JsonProperty("Заголовок блока домов игрока")]
        public string Title;
        [JsonProperty("Цвет фона блока")]
        public string ColorHeader;
        [JsonProperty("Позиция AnchorMin")]
        public string AnchorMin;
        [JsonProperty("Позиция AnchorMax")]
        public string AnchorMax;
        [JsonProperty("Шапка: Позиция AnchorMin")]
        public string HeaderAnchorMin;
        [JsonProperty("Шапка: Позиция AnchorMax")]
        public string HeaderAnchorMax;
        [JsonProperty("Настройка иконки")]
        public Images Image;
        [JsonProperty("Текст кнопки сохранения новой точки")]
        public string TextSethome;
        [JsonProperty("Максимальное количество точек домов")]
        public int Limit;
        [JsonProperty("Список привилегий по количеству home (Привилегия: количество)")]
        public Dictionary<string, int> TeleportPrivilages;
        [JsonProperty("Цвет кнопок")]
        public string ColorButtons;
    }

     class FriendSettings
    {
        [JsonProperty("Заголовок блока друзей игрока")]
        public string Title;
        [JsonProperty("Цвет фона блока")]
        public string ColorHeader;
        [JsonProperty("Включить индекатор онлайна")]
        public bool EnabledOnline;
        [JsonProperty("Позиция AnchorMin")]
        public string AnchorMin;
        [JsonProperty("Позиция AnchorMax")]
        public string AnchorMax;
        [JsonProperty("Шапка: Позиция AnchorMin")]
        public string HeaderAnchorMin;
        [JsonProperty("Шапка: Позиция AnchorMax")]
        public string HeaderAnchorMax;
        [JsonProperty("Настройка иконки")]
        public Images Image;
        [JsonProperty("Текст кнопки пустой ячейки")]
        public string Text;
        [JsonProperty("Цвет кнопок")]
        public string ColorButtons;
        [JsonProperty("Лимит друзей")]
        public int Limit;
    }

     class OtherSettings
    {
        [JsonProperty("Заголовок блока Дополнительных функций")]
        public string Title;
        [JsonProperty("Цвет фона блока")]
        public string ColorHeader;
        [JsonProperty("Позиция AnchorMin")]
        public string AnchorMin;
        [JsonProperty("Позиция AnchorMax")]
        public string AnchorMax;
        [JsonProperty("Шапка: Позиция AnchorMin")]
        public string HeaderAnchorMin;
        [JsonProperty("Шапка: Позиция AnchorMax")]
        public string HeaderAnchorMax;
        [JsonProperty("Цвет кнопок")]
        public string ColorButtons;
    }

     class PluginConfig
    {
        [JsonProperty("Основные настройки")]
        public MainSettings Main;
        [JsonProperty("Заголовок главной страницы")]
        public string Title;
        [JsonProperty("Аватар")]
        public UserAvatar UserAvatar;
        [JsonProperty("Точки дома")]
        public HomeSettings Home;
        [JsonProperty("Друзья")]
        public FriendSettings Friends;
        [JsonProperty("Дополнительные")]
        public OtherSettings Other;
        [JsonProperty("Настройка кнопок")]
        public List<DefaultButton> DefaultButtons;
        [JsonProperty("Версия конфигурации")]
        public VersionNumber PluginVersion;
        public static PluginConfig DefaultConfig();
    }

    [PluginReference]
     Plugin ImageLibrary;
     void OnServerInitialized();
    public Dictionary<ulong, bool> PlayersList;
     void LoadData();
     void SaveData();
     void Unload();
     int GetFreeHomesCount(BasePlayer player);
     void DestroyUI(BasePlayer player);
    private string MainLayer;
     void CreateMenu(BasePlayer player);
    private string GetGridString(Vector3 position);
    private string NumberToString(int number);
    [ConsoleCommand("mainmenu_command")]
     void cmdRUnPlayerCommand(ConsoleSystem.Arg args);
    [ConsoleCommand("mainmenu_enabledRhome")]
     void cmdEnabledRemoveHome(ConsoleSystem.Arg args);
    [ConsoleCommand("mainmenu_removehome")]
     void cmdMainMenuRemoveHome(ConsoleSystem.Arg args);
     void CreateRemoveHomse(BasePlayer player, string home, string parrent);
    [PluginReference]
     Plugin Friends;
     Plugin NTeleportation;
     Plugin Teleport;
     Plugin Teleportation;
     Plugin HomesGUI;
     Plugin Trade;
     Plugin MutualPermission;
    public List<ulong> GetFriends(ulong playerid);
     Dictionary<string, Vector3> GetHomes(BasePlayer player);
     void cmdOpenMainMenu(BasePlayer player, string com, string[] args);
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

    private static List<Position> GetPositions(int colums, int rows, float colPadding, float rowPadding, bool columsFirst);
}

 class DefaultButton
{
    [JsonProperty("Позиция AnchorMin")]
    public string AnchorMin;
    [JsonProperty("Позиция AnchorMax")]
    public string AnchorMax;
    [JsonProperty("Текст")]
    public string Text;
    [JsonProperty("Настройка изображения")]
    public Images ImagesSetting;
    [JsonProperty("Выполняемая команда (Если это чатовая команда, используйте через слэш)")]
    public string Command;
    [JsonProperty("Цвет кнопки")]
    public string Color;
}

public class Images
{
    [JsonProperty("Ссылка на изображение")]
    public string ImageURL;
    [JsonProperty("Позиция AnchorMin")]
    public string AnchorMin;
    [JsonProperty("Позиция AnchorMax")]
    public string AnchorMax;
    [JsonProperty("Цвет изображения")]
    public string Color;
}

 class UserAvatar
{
    [JsonProperty("Позиция AnchorMin")]
    public string AnchorMin;
    [JsonProperty("Позиция AnchorMax")]
    public string AnchorMax;
    [JsonProperty("Прозрачность")]
    public string Color;
}

 class MainSettings
{
    [JsonProperty("Показывать при подключении")]
    public bool OpenToConnection;
    [JsonProperty("Показывать только при первом подключении")]
    public bool OpenToConnectionFirst;
    [JsonProperty("Команды для открытия меню")]
    public List<string> Commands;
}

 class HomeSettings
{
    [JsonProperty("Заголовок блока домов игрока")]
    public string Title;
    [JsonProperty("Цвет фона блока")]
    public string ColorHeader;
    [JsonProperty("Позиция AnchorMin")]
    public string AnchorMin;
    [JsonProperty("Позиция AnchorMax")]
    public string AnchorMax;
    [JsonProperty("Шапка: Позиция AnchorMin")]
    public string HeaderAnchorMin;
    [JsonProperty("Шапка: Позиция AnchorMax")]
    public string HeaderAnchorMax;
    [JsonProperty("Настройка иконки")]
    public Images Image;
    [JsonProperty("Текст кнопки сохранения новой точки")]
    public string TextSethome;
    [JsonProperty("Максимальное количество точек домов")]
    public int Limit;
    [JsonProperty("Список привилегий по количеству home (Привилегия: количество)")]
    public Dictionary<string, int> TeleportPrivilages;
    [JsonProperty("Цвет кнопок")]
    public string ColorButtons;
}

 class FriendSettings
{
    [JsonProperty("Заголовок блока друзей игрока")]
    public string Title;
    [JsonProperty("Цвет фона блока")]
    public string ColorHeader;
    [JsonProperty("Включить индекатор онлайна")]
    public bool EnabledOnline;
    [JsonProperty("Позиция AnchorMin")]
    public string AnchorMin;
    [JsonProperty("Позиция AnchorMax")]
    public string AnchorMax;
    [JsonProperty("Шапка: Позиция AnchorMin")]
    public string HeaderAnchorMin;
    [JsonProperty("Шапка: Позиция AnchorMax")]
    public string HeaderAnchorMax;
    [JsonProperty("Настройка иконки")]
    public Images Image;
    [JsonProperty("Текст кнопки пустой ячейки")]
    public string Text;
    [JsonProperty("Цвет кнопок")]
    public string ColorButtons;
    [JsonProperty("Лимит друзей")]
    public int Limit;
}

 class OtherSettings
{
    [JsonProperty("Заголовок блока Дополнительных функций")]
    public string Title;
    [JsonProperty("Цвет фона блока")]
    public string ColorHeader;
    [JsonProperty("Позиция AnchorMin")]
    public string AnchorMin;
    [JsonProperty("Позиция AnchorMax")]
    public string AnchorMax;
    [JsonProperty("Шапка: Позиция AnchorMin")]
    public string HeaderAnchorMin;
    [JsonProperty("Шапка: Позиция AnchorMax")]
    public string HeaderAnchorMax;
    [JsonProperty("Цвет кнопок")]
    public string ColorButtons;
}

 class PluginConfig
{
    [JsonProperty("Основные настройки")]
    public MainSettings Main;
    [JsonProperty("Заголовок главной страницы")]
    public string Title;
    [JsonProperty("Аватар")]
    public UserAvatar UserAvatar;
    [JsonProperty("Точки дома")]
    public HomeSettings Home;
    [JsonProperty("Друзья")]
    public FriendSettings Friends;
    [JsonProperty("Дополнительные")]
    public OtherSettings Other;
    [JsonProperty("Настройка кнопок")]
    public List<DefaultButton> DefaultButtons;
    [JsonProperty("Версия конфигурации")]
    public VersionNumber PluginVersion;
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

## MapGenerator

```csharp
using System.IO;
using Oxide.Core;
using UnityEngine;
using System;
using Newtonsoft.Json;
using Random = System.Random;

Oxide.Plugins
[Info("MapGenerator", "TexHik", "0.1")]
[Description("Advansed map generator")]
 class MapGenerator : RustPlugin
{
    private static ConfigFile config;
    private class ConfigFile
    {
        [JsonProperty(PropertyName = "Имя генерируемого файла карты, без расширения (.jpg), если папки указаной в имени не существует, плагин будет выдавать ошибку")]
        public string filename { get; set; }
        [JsonProperty(PropertyName = "Автоматическая генерация новой карты после вайпа")]
        public bool AutoMap { get; set; }
        [JsonProperty(PropertyName = "Размер изображения для автоматической генерации карты (0 - размер по дефолту)")]
        public int autosize { get; set; }
        [JsonProperty(PropertyName = "Расширение файла (автоматическая генерация, jpg или png)")]
        public string type { get; set; }
        public static ConfigFile DefaultConfig();
    }

    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    protected override void SaveConfig();
    private void Regenerate();
    private int _mapWidth;
    private int _mapHeight;
    private Terrain _terrain;
    private bool NewWipe;
    [ConsoleCommand("savemap")]
    private void SaveMapCMD(ConsoleSystem.Arg arg);
    private void GenerateMap(int size, string filetype, float brightness);
    private float Sigma(float a);
    private Color Sigma(Color a);
    private float Parab(float a);
    private Color Parab(Color a);
    private float GetLowestTerrainHeight();
    private float GetHighestTerrainHeight();
    public Color AdvBlend(Color a, Color b);
    private void SaveIMG(Texture2D texture, int size, string filetype);
    public bool ThumbnailCallback();
     void OnNewSave(string filename);
     void OnServerInitialized();
}

private class ConfigFile
{
    [JsonProperty(PropertyName = "Имя генерируемого файла карты, без расширения (.jpg), если папки указаной в имени не существует, плагин будет выдавать ошибку")]
    public string filename { get; set; }
    [JsonProperty(PropertyName = "Автоматическая генерация новой карты после вайпа")]
    public bool AutoMap { get; set; }
    [JsonProperty(PropertyName = "Размер изображения для автоматической генерации карты (0 - размер по дефолту)")]
    public int autosize { get; set; }
    [JsonProperty(PropertyName = "Расширение файла (автоматическая генерация, jpg или png)")]
    public string type { get; set; }
    public static ConfigFile DefaultConfig();
}


```

---

## MapGenerator (1)

```csharp
using System.IO;
using Oxide.Core;
using UnityEngine;
using System;
using Newtonsoft.Json;

Oxide.Plugins
[Info("MapGenerator", "Dids/SkiTles", "0.1")]
[Description("Simple map generator")]
 class MapGenerator : RustPlugin
{
    private static ConfigFile config;
    private class ConfigFile
    {
        [JsonProperty(PropertyName = "Имя генерируемого файла карты, без расширения (.jpg), если папки указаной в имени не существует, плагин будет выдавать ошибку")]
        public string filename { get; set; }
        [JsonProperty(PropertyName = "Автоматическая генерация новой карты после вайпа")]
        public bool AutoMap { get; set; }
        [JsonProperty(PropertyName = "Размер изображения для автоматической генерации карты (0 - размер по дефолту)")]
        public int autosize { get; set; }
        [JsonProperty(PropertyName = "Расширение файла (автоматическая генерация, jpg или png)")]
        public string type { get; set; }
        public static ConfigFile DefaultConfig();
    }

    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    protected override void SaveConfig();
    private void Regenerate();
    private int _mapWidth;
    private int _mapHeight;
    private Terrain _terrain;
    private bool NewWipe;
    [ConsoleCommand("savemap")]
    private void SaveMapCMD(ConsoleSystem.Arg arg);
    private void GenerateMap(int size, string filetype);
    private float GetLowestTerrainHeight();
    private float GetHighestTerrainHeight();
    private static Color AlphaBlend(Color top, Color bottom);
    private static float BlendSubpixel(float top, float bottom, float alphaTop, float alphaBottom);
    private void SaveIMG(Texture2D texture, int size, string filetype);
    public bool ThumbnailCallback();
     void OnNewSave(string filename);
     void OnServerInitialized();
}

private class ConfigFile
{
    [JsonProperty(PropertyName = "Имя генерируемого файла карты, без расширения (.jpg), если папки указаной в имени не существует, плагин будет выдавать ошибку")]
    public string filename { get; set; }
    [JsonProperty(PropertyName = "Автоматическая генерация новой карты после вайпа")]
    public bool AutoMap { get; set; }
    [JsonProperty(PropertyName = "Размер изображения для автоматической генерации карты (0 - размер по дефолту)")]
    public int autosize { get; set; }
    [JsonProperty(PropertyName = "Расширение файла (автоматическая генерация, jpg или png)")]
    public string type { get; set; }
    public static ConfigFile DefaultConfig();
}


```

---

## MarkerManager

```csharp
using System;
using System.Collections.Generic;
using Oxide.Core;
using UnityEngine;
using VLB;
using Color = UnityEngine.Color;

Oxide.Plugins
[Info("Marker Manager", "Orange", "2.1.1")]
[Description("Allows to place markers on in-game map")]
public class MarkerManager : RustPlugin
{
    private const string genericPrefab;
    private const string vendingPrefab;
    private const string permUse;
    private const string chatCommand;
    private void Init();
    private void OnServerInitialized();
    private void Unload();
    private void cmdMarkerChat(BasePlayer player, string command, string[] args);
    private void CreateMarker(Vector3 position, int duration, float refreshRate, string name, string displayName, float radius, string colorMarker, string colorOutline);
    private void CreateMarker(BaseEntity entity, int duration, float refreshRate, string name, string displayName, float radius, string colorMarker, string colorOutline);
    private void RemoveMarker(string name);
    private void RemoveMarkers();
    private void CreateCustomMarker(CachedMarker def, BasePlayer player);
    private void RemoveCustomMarker(string name, BasePlayer player);
    private void SaveCustomMarker(CachedMarker def);
    private void LoadCustomMarkers();
    private void RemoveSavedMarker(string name);
    protected override void LoadDefaultMessages();
    private string GetMessage(string messageKey, string playerID, object[] args);
    private void Message(BasePlayer player, string messageKey, object[] args);
    private const string filename;
    private static List<CachedMarker> data;
    private class CachedMarker
    {
        public float radius;
        public string color1;
        public string color2;
        public string displayName;
        public string name;
        public float refreshRate;
        public Vector3 position;
        public int duration;
    }

    private void LoadData();
    private void SaveData();
    private void API_CreateMarker(Vector3 position, string name, int duration, float refreshRate, float radius, string displayName, string colorMarker, string colorOutline);
    private void API_CreateMarker(BaseEntity entity, string name, int duration, float refreshRate, float radius, string displayName, string colorMarker, string colorOutline);
    private void API_RemoveMarker(string name);
    private class CustomMapMarker : MonoBehaviour
    {
        private VendingMachineMapMarker vending;
        private MapMarkerGenericRadius generic;
        public BaseEntity parent;
        private bool asChild;
        public float radius;
        public Color color1;
        public Color color2;
        public string displayName;
        public float refreshRate;
        public Vector3 position;
        public int duration;
        public bool placedByPlayer;
        private void Start();
        private void CreateMarkers();
        private void UpdatePosition();
        private void UpdateMarkers();
        private void DestroyMakers();
        private void OnDestroy();
    }

}

private class CachedMarker
{
    public float radius;
    public string color1;
    public string color2;
    public string displayName;
    public string name;
    public float refreshRate;
    public Vector3 position;
    public int duration;
}

private class CustomMapMarker : MonoBehaviour
{
    private VendingMachineMapMarker vending;
    private MapMarkerGenericRadius generic;
    public BaseEntity parent;
    private bool asChild;
    public float radius;
    public Color color1;
    public Color color2;
    public string displayName;
    public float refreshRate;
    public Vector3 position;
    public int duration;
    public bool placedByPlayer;
    private void Start();
    private void CreateMarkers();
    private void UpdatePosition();
    private void UpdateMarkers();
    private void DestroyMakers();
    private void OnDestroy();
}


```

---

## MarkerTeleporter

```csharp
using UnityEngine;
using ProtoBuf;
using System.Collections.Generic;
using Oxide.Core.Configuration;
using Oxide.Core;
using System.Linq;

Oxide.Plugins
[Info("MarkerTeleporter", "walkinrey", "1.0.0")]
 class MarkerTeleporter : RustPlugin
{
     DynamicConfigFile data;
     List<string> optionActive;
     string currentStatus;
     void OnMapMarkerAdded(BasePlayer player, MapNote note);
     void Init();
     float GetCorrectPos(Vector3 pos);
     void cmdChat(BasePlayer player, string command, string[] args);
     void ActivateFunction(BasePlayer player);
     void DisactivateFunction(BasePlayer player);
}


```

---

## Martyrdom

```csharp
using System.Collections.Generic;
using UnityEngine;

Oxide.Plugins
[Info("Martyrdom", "k1lly0u", "0.2.0", ResourceId = 1523)]
 class Martyrdom : RustPlugin
{
    private Dictionary<ulong, EType> Martyrs;
     Dictionary<EType, ExplosiveInfo> Explosives;
     void Loaded();
     void OnServerInitialized();
     void OnEntityDeath(BaseEntity entity, HitInfo info);
     void TryDropExplosive(BasePlayer player);
     void CreateExplosive(EType type, BasePlayer player);
    private bool HasEnoughRes(BasePlayer player, int itemid, int amount);
    private void TakeResources(BasePlayer player, int itemid, int amount);
    private bool HasPerm(BasePlayer player, EType type);
    private bool HasAnyPerm(BasePlayer player);
     void SetExplosiveInfo();
     class ExplosiveInfo
    {
        public int ItemID;
        public string PrefabName;
        public float Damage;
        public float Radius;
        public float Fuse;
    }

    [ChatCommand("m")]
     void cmdM(BasePlayer player, string command, string[] args);
    private ConfigData configData;
     class ExpType
    {
        public bool Activated { get; set; }
        public float Damage { get; set; }
        public float Radius { get; set; }
        public float Fuse { get; set; }
    }

     class ConfigData
    {
        public ExpType Grenade { get; set; }
        public ExpType Beancan { get; set; }
        public ExpType Explosive { get; set; }
    }

    private void LoadVariables();
    protected override void LoadDefaultConfig();
    private void LoadConfigVariables();
     void SaveConfig(ConfigData config);
     string msg(string key, string id);
     Dictionary<string, string> messages;
}

 class ExplosiveInfo
{
    public int ItemID;
    public string PrefabName;
    public float Damage;
    public float Radius;
    public float Fuse;
}

 class ExpType
{
    public bool Activated { get; set; }
    public float Damage { get; set; }
    public float Radius { get; set; }
    public float Fuse { get; set; }
}

 class ConfigData
{
    public ExpType Grenade { get; set; }
    public ExpType Beancan { get; set; }
    public ExpType Explosive { get; set; }
}


```

---

## MasterKey

```csharp
using System;
using System.Collections.Generic;
using Oxide.Core;
using Oxide.Core.Configuration;
using Oxide.Core.Libraries.Covalence;

Oxide.Plugins
[Info("MasterKey", "Wulf/lukespragg", "0.5.0", ResourceId = 1151)]
[Description("Gain access to any locked object and/or build anywhere with permission")]
 class MasterKey : CovalencePlugin
{
    readonly DynamicConfigFile dataFile;
    readonly string[] lockableTypes;
     Dictionary<string, bool> playerPrefs;
    const string permBuild;
    const string permCupboard;
     bool logUsage;
     bool showMessages;
    protected override void LoadDefaultConfig();
     void Init();
     void LoadDefaultMessages();
    [Command("masterkey", "mkey", "mk")]
     void ChatCommand(IPlayer player, string command, string[] args);
     object CanUseLock(BasePlayer player, BaseLock @lock);
     object OnCupboardAuthorize(BuildingPrivlidge priviledge, BasePlayer player);
     void OnEntityEnter(TriggerBase trigger, BaseEntity entity);
     T GetConfig(string name, T value);
     string Lang(string key, string id, object[] args);
     void LogToFile(string text);
}


```

---

## MathQuiz

```csharp
using System;
using System.Collections.Generic;
using Oxide.Core.Libraries.Covalence;
using System.Linq;

Oxide.Plugins
[Info("MathQuiz", "FREDWAY", "1.0.1")]
[Description("Mathematical quiz with rewards")]
 class MathQuiz : CovalencePlugin
{
    private bool QuizInProgress;
    private string Task;
    private string Answer;
    private string Winner;
    private string Reward;
    private Timer Reminder;
    private string Prefix;
    private string PrefixColor;
    private string SteamIDIcon;
    private Dictionary<ItemDefinition, int> Rewards;
    private Dictionary<int, string> Tasks;
    private float QuizFreq;
    private class ConfigLocalization
    {
        public string Prefix { get; set; }
        public string PrefixColor { get; set; }
        public string SteamIDIcon { get; set; }
        public string Rewards { get; set; }
        public string Tasks { get; set; }
        public string QuizFreq { get; set; }
    }

    private static ConfigLocalization ConfigRus;
    private static ConfigLocalization ConfigEn;
    protected override void LoadDefaultConfig();
    private new void LoadConfig();
    private void LoadMessages();
     void Loaded();
    private void OnServerInitialized();
     void OnPlayerSleepEnded(BasePlayer player);
    private void StartQuiz();
    private string GetAnswer(int X, int Y, int Z, int num);
    private void EndQuiz(IPlayer winner);
     void OnUserChat(IPlayer player, string message);
    private void GetConfig(string Key, T var);
    private void QuizInformToChat(bool start);
     string GetMsg(string key, object userID);
}

private class ConfigLocalization
{
    public string Prefix { get; set; }
    public string PrefixColor { get; set; }
    public string SteamIDIcon { get; set; }
    public string Rewards { get; set; }
    public string Tasks { get; set; }
    public string QuizFreq { get; set; }
}


```

---

## MedkitSaver

```csharp
using UnityEngine;
using System;
using Oxide.Core;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using Newtonsoft.Json;
using System.Collections.Generic;

Oxide.Plugins
[Info("MedkitSaver", "walkinrey", "1.0.4")]
 class MedkitSaver : RustPlugin
{
     class Conf
    {
        [JsonProperty("Сколько моментально установить хп при использовании аптечки (по умолчанию 15)")]
        public float hpMomental;
        [JsonProperty("Сколько хп восстанавливать при использовании аптечки (по умолчанию 100)")]
        public float hpRevive;
        [JsonProperty("Выводить сообщения в чат о использовании аптечки? (по умолчанию да)")]
        public bool messageUse;
        [JsonProperty("Сообщение о использовании аптечки")]
        public string messageString;
    }

    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    private Conf config;
    [PluginReference]
    private Plugin ImageLibrary;
     List<string> steamIDs;
     object OnPlayerWound(BasePlayer player, BasePlayer source);
     object OnPlayerDeath(BasePlayer player, HitInfo info);
     void Loaded();
     CuiElementContainer CreateElements();
    [ConsoleCommand("medkituse")]
    private void cmd_Use(ConsoleSystem.Arg arg);
    private void DestroyMedkitUi(BasePlayer player);
}

 class Conf
{
    [JsonProperty("Сколько моментально установить хп при использовании аптечки (по умолчанию 15)")]
    public float hpMomental;
    [JsonProperty("Сколько хп восстанавливать при использовании аптечки (по умолчанию 100)")]
    public float hpRevive;
    [JsonProperty("Выводить сообщения в чат о использовании аптечки? (по умолчанию да)")]
    public bool messageUse;
    [JsonProperty("Сообщение о использовании аптечки")]
    public string messageString;
}


```

---

## Menu

```csharp
using System.Runtime.InteropServices;
using System.Collections.Generic;
using System.Collections;
using Newtonsoft.Json;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using ru = Oxide.Game.Rust;
using UnityEngine;
using Oxide.Core.Libraries.Covalence;
using Oxide.Core;

Oxide.Plugins
[Info("Menu", "ходвард", "1.0.2")]
 class Menu : RustPlugin
{
    [PluginReference]
    private Plugin ImageLibrary;
    private const string Layer;
    private void LoadImages();
    private void OnServerInitialized();
    private readonly List<BasePlayer> MenuUsers2;
    private void CmdMenuOpen(IPlayer user, string cmd, string[] args);
    private IEnumerator StartUpdate(BasePlayer player);
     void startupdatecon(ConsoleSystem.Arg ar);
    private void OnPlayerConnected(BasePlayer player);
    private void OnPlayerDisconnected(BasePlayer player);
    private void Unload();
    public void RefreshUI(BasePlayer player, string Type);
    public void Main_menu(BasePlayer player);
    public class PluginConfig
    {
    }

    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    protected override void SaveConfig();
     PluginConfig config;
    public static class UI
    {
        public static void AddImage(CuiElementContainer container, string parrent, string name, string color, string sprite, string mat, string aMin, string aMax, string oMin, string oMax, string outline, string dist);
        public static void AddRawImage(CuiElementContainer container, string parrent, string name, string png, string color, string sprite, string mat, string aMin, string aMax, string oMin, string oMax);
        public static void AddRawImage1(CuiElementContainer container, string parrent, string name, string png, string color, string sprite, string mat, string aMin, string aMax, string oMin, string oMax);
        public static void AddText(CuiElementContainer container, string parrent, string name, string color, string text, TextAnchor align, int size, string aMin, string aMax, string oMin, string oMax, string outColor, string font, string dist, float FadeIN, float FadeOut);
        public static void AddText1(CuiElementContainer container, string parrent, string name, string color, string text, TextAnchor align, int size, string aMin, string aMax, string oMin, string oMax, string outColor, string font, string dist, float FadeIN, float FadeOut);
        public static void AddText2(CuiElementContainer container, string parrent, string name, string color, string text, TextAnchor align, int size, string aMin, string aMax, string oMin, string oMax, string outColor, string font, string dist, float FadeIN, float FadeOut);
        public static void AddButton(CuiElementContainer container, string parrent, string name, string cmd, string close, string color, string sprite, string mat, string aMin, string aMax, string oMin, string oMax, string outline, string dist);
    }

}

public class PluginConfig
{
}

public static class UI
{
    public static void AddImage(CuiElementContainer container, string parrent, string name, string color, string sprite, string mat, string aMin, string aMax, string oMin, string oMax, string outline, string dist);
    public static void AddRawImage(CuiElementContainer container, string parrent, string name, string png, string color, string sprite, string mat, string aMin, string aMax, string oMin, string oMax);
    public static void AddRawImage1(CuiElementContainer container, string parrent, string name, string png, string color, string sprite, string mat, string aMin, string aMax, string oMin, string oMax);
    public static void AddText(CuiElementContainer container, string parrent, string name, string color, string text, TextAnchor align, int size, string aMin, string aMax, string oMin, string oMax, string outColor, string font, string dist, float FadeIN, float FadeOut);
    public static void AddText1(CuiElementContainer container, string parrent, string name, string color, string text, TextAnchor align, int size, string aMin, string aMax, string oMin, string oMax, string outColor, string font, string dist, float FadeIN, float FadeOut);
    public static void AddText2(CuiElementContainer container, string parrent, string name, string color, string text, TextAnchor align, int size, string aMin, string aMax, string oMin, string oMax, string outColor, string font, string dist, float FadeIN, float FadeOut);
    public static void AddButton(CuiElementContainer container, string parrent, string name, string cmd, string close, string color, string sprite, string mat, string aMin, string aMax, string oMin, string oMax, string outline, string dist);
}


```

---

## MenuHelp

```csharp
using Oxide.Core;
using Oxide.Game.Rust.Cui;
using System;
using System.Collections.Generic;
using System.Linq;
using UnityEngine;
using Oxide.Core.Configuration;

Oxide.Plugins
[Info("Menu Help", "OxideBro", "0.1.1")]
 class MenuHelp : RustPlugin
{
    public class MenuList
    {
        public string Type;
        public string NameUI;
        public string Command;
        public int FrontSize;
        public string BackgroundColor;
        public List<Messages> MessagesList;
    }

    public class Messages
    {
        public string Message;
        public string Date;
    }

     string Title;
     string AnchorMin;
     string AnchorMax;
     string ButtonsColor;
     string BackgroundColor;
     bool EnabledLogin;
    protected override void LoadDefaultConfig();
    public static void GetVariable(DynamicConfigFile config, string name, T value, T defaultValue);
    private string Layer;
    private void HelpGUI(BasePlayer player, bool section, string name, bool bReturn, int numbr);
    [ConsoleCommand("menu")]
     void cmdConsoleHelp(ConsoleSystem.Arg args);
    [ConsoleCommand("next.page")]
     void cmdNewxt(ConsoleSystem.Arg args);
    [ChatCommand("menu")]
     void cmdChatHelp(BasePlayer player, string command, string[] args);
    [ChatCommand("news")]
     void cmdChatNews(BasePlayer player, string command, string[] args);
    [ConsoleCommand("MENUOPEN")]
     void cmdConsoleHelpOpen(ConsoleSystem.Arg args);
    public Dictionary<string, MenuList> menuElement;
    private void LoadData();
    private void CheckData();
    private void OnServerInitialized();
     void OnPlayerInit(BasePlayer player);
    private void Unload();
}

public class MenuList
{
    public string Type;
    public string NameUI;
    public string Command;
    public int FrontSize;
    public string BackgroundColor;
    public List<Messages> MessagesList;
}

public class Messages
{
    public string Message;
    public string Date;
}


```

---

## MenuRevolve

```csharp
using System;
using System.Collections.Generic;
using System.Globalization;
using System.Linq;
using ConVar;
using Newtonsoft.Json;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using UnityEngine;

Oxide.Plugins
[Info("MenuRevolve", "TopPlugin.ru", "0.0.3")]
[Description("Приятное меню для вашего сервера")]
 class MenuRevolve : RustPlugin
{
    [PluginReference]
     Plugin IQChat;
    private static Configuration config;
    private class Configuration
    {
        [JsonProperty("Оглавление меню")]
        public string DisplayNmeMenu;
        [JsonProperty("Описание меню")]
        public string DescriptionMenu;
        [JsonProperty("Страницы с текстом")]
        public List<Settings> MenuConfiguration;
        [JsonProperty("Настройки для IQChat")]
        public ChatSettings ChatSetting;
        internal class Settings
        {
            [JsonProperty("Текст кнопки")]
            public string DisplayNameButton;
            [JsonProperty("Оглавление на информативном блоке")]
            public string TitleInfoBlock;
            [JsonProperty("Текст на странице")]
            public List<string> Text;
        }

        internal class ChatSettings
        {
            [JsonProperty("Префикс(IQChat)")]
            public string CustomPrefix;
            [JsonProperty("Steam64ID для аватарки(IQChat)")]
            public string CustomAvatar;
            [JsonProperty("Цвет-HEX для префикса(IQChat)")]
            public string HexColorPrefix;
        }

        public static Configuration GetNewConfiguration();
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    public List<ulong> NoOpenMenuListUsers;
     void Unload();
    private void OnServerSave();
    private void OnServerInitialized();
     void OnPlayerConnected(BasePlayer player);
    [ChatCommand("info")]
     void ChatCommandMenu(BasePlayer player);
    [ConsoleCommand("menu")]
     void MenuConsoleCommand(ConsoleSystem.Arg arg);
    private new void LoadDefaultMessages();
    public static string MAIN_PARENT;
    public void USER_INTERFACE(BasePlayer player);
     void SelectCategory(BasePlayer player, int CategoryIndex, int Page);
    private static string HexToRustFormat(string hex);
    public void SendChat(string Message, BasePlayer player, Chat.ChatChannel channel);
}

private class Configuration
{
    [JsonProperty("Оглавление меню")]
    public string DisplayNmeMenu;
    [JsonProperty("Описание меню")]
    public string DescriptionMenu;
    [JsonProperty("Страницы с текстом")]
    public List<Settings> MenuConfiguration;
    [JsonProperty("Настройки для IQChat")]
    public ChatSettings ChatSetting;
    internal class Settings
    {
        [JsonProperty("Текст кнопки")]
        public string DisplayNameButton;
        [JsonProperty("Оглавление на информативном блоке")]
        public string TitleInfoBlock;
        [JsonProperty("Текст на странице")]
        public List<string> Text;
    }

    internal class ChatSettings
    {
        [JsonProperty("Префикс(IQChat)")]
        public string CustomPrefix;
        [JsonProperty("Steam64ID для аватарки(IQChat)")]
        public string CustomAvatar;
        [JsonProperty("Цвет-HEX для префикса(IQChat)")]
        public string HexColorPrefix;
    }

    public static Configuration GetNewConfiguration();
}

internal class Settings
{
    [JsonProperty("Текст кнопки")]
    public string DisplayNameButton;
    [JsonProperty("Оглавление на информативном блоке")]
    public string TitleInfoBlock;
    [JsonProperty("Текст на странице")]
    public List<string> Text;
}

internal class ChatSettings
{
    [JsonProperty("Префикс(IQChat)")]
    public string CustomPrefix;
    [JsonProperty("Steam64ID для аватарки(IQChat)")]
    public string CustomAvatar;
    [JsonProperty("Цвет-HEX для префикса(IQChat)")]
    public string HexColorPrefix;
}


```

---

## MenuSystem

```csharp
using System.Collections.Generic;
using System.Linq;
using Oxide.Game.Rust.Cui;
using Oxide.Core.Plugins;
using UnityEngine;
using Newtonsoft.Json;

Oxide.Plugins
[Info("MenuSystem", "Sempai#3239", "1.0.0")]
 class MenuSystem : RustPlugin
{
     string Layer;
    [PluginReference]
     Plugin ImageLibrary;
     Plugin OurServers;
     Plugin KitSystem;
     Plugin ShopSystem;
     Plugin WipeSchedule;
     Plugin Leaderboard;
     string Logo;
     string Banner;
     Dictionary<string, string> ButtonMenu;
     void OnServerInitialized();
     void OnPlayerConnected(BasePlayer player);
     void Unload();
    [ChatCommand("menu")]
     void ChatMenu(BasePlayer player);
    [ChatCommand("server")]
     void ChatServer(BasePlayer player);
    [ChatCommand("kit")]
     void ChatKit(BasePlayer player);
    [ChatCommand("shop")]
     void ChatShop(BasePlayer player);
    [ChatCommand("wipe")]
     void ChatWipe(BasePlayer player);
    [ChatCommand("stat")]
     void ChatStat(BasePlayer player);
    [ConsoleCommand("menu")]
     void ConsoleMenu(ConsoleSystem.Arg args);
     void MenuUI(BasePlayer player, string name);
     void UI(BasePlayer player, string name);
     void WelcomeUI(BasePlayer player);
     void DestroyUI(BasePlayer player);
}


```

---

## MercuryUtilites

```csharp
using System;
using System.Collections.Generic;
using UnityEngine;
using Newtonsoft.Json;
using Oxide.Game.Rust.Cui;
using Oxide.Core.Libraries.Covalence;
using ConVar;
using Newtonsoft.Json.Linq;
using Oxide.Core.Libraries;
using System.Linq;
using Oxide.Core.Plugins;

Oxide.Plugins
[Info("MercuryUtilites", "Mercury", "0.0.1")]
 class MercuryUtilites : RustPlugin
{
    public Dictionary<ulong, string> HexTakePlayer;
    public HashSet<string> IconsRust;
     List<string> Materials;
    public List<string> Fonts;
    public List<string> HexList;
    public HashSet<string> EffectRustList;
    private void OnServerInitialized();
     void OnPlayerConnected(BasePlayer player);
     void Unload();
    [ConsoleCommand("utilites")]
     void MercuryUtilitesCommands(ConsoleSystem.Arg arg);
    [ChatCommand("ut")]
     void ChatCommandUtilites(BasePlayer player);
    [ConsoleCommand("ut")]
     void ConsoleCommandUtilMer(ConsoleSystem.Arg arg);
    public static string PARENT_UI;
    public static string PARENT_UI_BUTTON;
    public static string PARENT_UI_ELEMENT;
    public static string PARENT_UI_ELEMENT_ICONS;
    public static string PARENT_UI_ELEMENT_ICONSTWO;
    public static string PARENT_UI_ELEMENT_MATERIAL;
    public static string PARENT_UI_ELEMENT_FONTS;
    public static string PARENT_UI_ELEMENT_EFFECT;
    public static string PARENT_UI_ELEMENT_EFFECT_PLAYER;
    public static string PARENT_UI_HEX_SETTINGS;
     void UI_PanelReportsPlayer(BasePlayer player);
     void UI_IconsLoaded(BasePlayer player, int Page, string Hex);
     void UI_IconsLoadedMaterial(BasePlayer player, int Page, string Hex);
     void UI_MaterialLoaded(BasePlayer player, int Page, string Hex);
     void UI_FontsLoaded(BasePlayer player, int Page, string Hex);
     void UI_EffectLoaded(BasePlayer player, int Page);
     void UI_Plaeer(BasePlayer player, string Title, string Path, int Page);
     void UI_HexSettingsMenu(BasePlayer player);
     void RunEffect(BasePlayer player, string Path);
     void DestroyedLayer(BasePlayer player);
    private static string HexToRustFormat(string hex);
}


```

---

## Metabolism

```csharp
using System;
using UnityEngine;

Oxide.Plugins
[Info("Metabolism", "Wulf/lukespragg", "2.3.1", ResourceId = 680)]
[Description("Modifies player metabolism stats and rates")]
 class Metabolism : RustPlugin
{
    const string permAllow;
     bool usePermissions;
     float caloriesLossRate;
     float caloriesSpawnValue;
     float healthGainRate;
     float healthSpawnValue;
     float hydrationLossRate;
     float hydrationSpawnValue;
    protected override void LoadDefaultConfig();
     void Init();
     void Metabolize(BasePlayer player);
     void OnPlayerRespawned(BasePlayer player);
     void OnRunPlayerMetabolism(PlayerMetabolism m, BaseCombatEntity entity);
     T GetConfig(string name, T value);
}


```

---

## MeteorFall

```csharp
using System;
using Random = System.Random;
using Oxide.Core.Plugins;
using System.Collections.Generic;
using UnityEngine;
using System.Linq;
using Rust;
using Facepunch;
using Newtonsoft.Json;
using Oxide.Game.Rust.Cui;
using System.Drawing;
using System.IO;
using System.Drawing.Imaging;
using System.Collections;
using System.Globalization;

Oxide.Plugins
[Info("MeteorFall", "EcoSmile", "2.0.6")]
 class MeteorFall : RustPlugin
{
    [PluginReference]
     Plugin EcoMap;
     Plugin RustMap;
     Plugin LustyMap;
     Plugin Map;
    private PluginConfig config;
    private class PluginConfig
    {
        [JsonProperty("Общие Настройки")]
        public Options _Options;
        public class Options
        {
            [JsonProperty("Отключать стандартную радиацию?")]
            public bool OffStandartRad;
            [JsonProperty("Включить автозапуск ивента?")]
            public bool EnableAutomaticEvents;
            [JsonProperty("Настройка автозапуска")]
            public Timers EventTimers;
            [JsonProperty("Сообщать о начале ивента в чат?")]
            public bool NotifyEvent;
            [JsonProperty("Включить эффект тряски земли от падения метеорита?")]
            public bool Earthquake { get; set; }
            [JsonProperty("Минимальное количество игроков для запуска ивента")]
            public int MinPlayer;
        }

        [JsonProperty("Настройки UI")]
        public UiSettings uiSettings;
        public class UiSettings
        {
            [JsonProperty("Включить UI?")]
            public bool useUi;
            [JsonProperty("Ссылка на картинку")]
            public string IconImage;
            [JsonProperty("Положение UI")]
            public UiTransform uiTransform;
            public class UiTransform
            {
                [JsonProperty("Координата Х Мин")]
                public string AnchorXMin;
                [JsonProperty("Координата Х Мax")]
                public string AnchorXMax;
                [JsonProperty("Координата Y Мин")]
                public string AnchorYMin;
                [JsonProperty("Координата Y Мax")]
                public string AnchorYMax;
            }

        }

        [JsonProperty("Настройки радиации")]
        public Radiations _Radiations;
        public class Radiations
        {
            [JsonProperty("Радиус зоны")]
            public float Radius;
            [JsonProperty("Сила радиации")]
            public float Strange;
        }

        public class Timers
        {
            [JsonProperty("Интервал ивента (Минуты) (Если выключен рандом)")]
            public int EventInterval;
            [JsonProperty("Включить рандомное время?")]
            public bool UseRandomTimer;
            [JsonProperty("Минимальный интервал (Минуты)")]
            public int RandomTimerMin;
            [JsonProperty("Максимальный интервал (Минуты)")]
            public int RandomTimerMax;
        }

        [JsonProperty("Настройка ивента")]
        public Intensity Settings;
        public class Intensity
        {
            [JsonProperty("Шанс распространения огня от малого метеорита")]
            public int FireRocketChance;
            [JsonProperty("Радиус на котором проходит метеоритопад")]
            public float Radius;
            [JsonProperty("Количество падающих метеоритов (малых)")]
            public int RocketAmount;
            [JsonProperty("Длительность падения малых метеоритов")]
            public int Duration;
            [JsonProperty("Множитель урона от попадания по Enemy")]
            public float DamageMultiplier;
            [JsonProperty("Настройка выпадающих ресурсов после попадания метеорита по земле")]
            public Drop ItemDropControl;
            public class Drop
            {
                [JsonProperty("Включить дроп ресурсов после метеорита?")]
                public bool EnableItemDrop;
                [JsonProperty("Настройка выпадаемых ресурсов")]
                public ItemDrops[] ItemsToDrop;
            }

            [JsonProperty("Количество NPC возле главного метеорита")]
            public int NpcAmount;
            [JsonProperty("Включить спавн NPC возле метеорита?")]
            public bool NpcSpawn;
            [JsonProperty("Количество HP у ученых")]
            public float NpcHealth;
            [JsonProperty("Множитель урона от ученых")]
            public float NpcDamage;
            [JsonProperty("Время которое будет остывать метеорит (Минуты)")]
            public float FireTime;
            [JsonProperty("Время через которое метеорит исчезнет после остывания (Минуты)")]
            public float DespawnTime;
        }

        [JsonProperty("Настройки метеорита")]
        public MeteorSetting meteorSetting;
        public class MeteorSetting
        {
            [JsonProperty("Время до падения метеорита")]
            public int MeteorTime;
            [JsonProperty("Шанс того, что метеорит будет радиоактивен (0-отключить)")]
            public float RadChacnce;
            [JsonProperty("Запускать волну радиации после приземления если метеорит радиоактивный?")]
            public bool RadWave;
            [JsonProperty("HP серной руды (стандартно 500)")]
            public float SulfurHealth;
            [JsonProperty("HP металлической руды (стандартно 500)")]
            public float MetalHealth;
            [JsonProperty("Наносить дамаг по области?")]
            public bool SplashDamage;
            [JsonProperty("Радиус области")]
            public float splashRadius;
            [JsonProperty("Наносимый дамаг (Урон наносится всем строительным блокам) ")]
            public float DamageAmount;
        }

        [JsonProperty("Настройка семечки")]
        public SeedSettings seedSettings;
        public class SeedSettings
        {
            [JsonProperty("Включить выпадение семечки?")]
            public bool IsEnable;
            [JsonProperty("Cажать семечку только на грядки?")]
            public bool PlantOnly;
            [JsonProperty("Скин семенчки")]
            public ulong SeedSkinID;
            [JsonProperty("Название семечки")]
            public string SeedName;
            [JsonProperty("Время через которое обьект вырастет (секунды)")]
            public float TimeToRelise;
            [JsonProperty("Прифаб обьекта - Настройка")]
            public Dictionary<string, ObjectSetting> PrefabSetting;
        }

    }

    public class ObjectSetting
    {
        [JsonProperty("Максимальное ХП обьекта")]
        public float MaxHealth;
        [JsonProperty("Шанс спавна обьекта")]
        public float SpawnChance;
    }

    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    protected override void SaveConfig();
     class ItemDrops
    {
        public string Shortname;
        public int Minimum;
        public int Maximum;
    }

    static MeteorFall ins;
    private bool rads;
     void OnServerInitialized();
    private void OnServerRadiation();
     void Unload();
    private Timer EventTimer;
    private Timer AlertTimer;
    private float launchStraightness;
    private float launchHeight;
    private float MapSize();
    private float projectileSpeed;
    private float gravityModifier;
    private float detonationTime;
     class ItemCarrier : MonoBehaviour
    {
        private ItemDrops[] carriedItems;
        private float multiplier;
        public void SetCarriedItems(ItemDrops[] carriedItems);
        public void SetDropMultiplier(float multiplier);
        private void OnDestroy();
    }

    private void StartEventTimer();
    private void StopTimer();
    private void StartRandomOnMap();
    private Timer rockettimer;
    private Timer effecttimer;
    private Timer timerfireball;
     List<uint> MeteorList;
     List<uint> MeteorFound;
     List<NPCPlayer> NpcList;
     List<uint> FireBall;
    public List<string> prefabOsn;
     Quaternion qTo;
     BaseEntity met;
    private Timer mystimer;
    private Timer mystimer3;
    private Timer mystimer4;
    private Timer mystimer5;
    private Timer mystimer1;
    private Timer mystimer2;
     void CollisionAtm();
     Vector3 spawnpos;
    private Vector3 MeteorSpawnPos();
    private void StartRainOfFire(Vector3 origin);
     void DamageObjects(Vector3 pos);
    private MapMarkerGenericRadius mapMarker;
    private VendingMachineMapMarker MarkerT;
    private UnityEngine.Color ConvertToColor(string color);
    private void CreatePrivateMap(Vector3 pos);
     List<string> prefabscreen;
     Random rnd;
     void Screen(BasePlayer pl);
    private void ColdMeteorite();
     Dictionary<Scientist, Vector3> npcPos;
    private void CreateNpc(Vector3 position, int amount);
    private Timer npcTimer;
     void ToSpawnPoint();
    private void CreateFireBall(Vector3 position, Vector3 additional);
    private void Despawn();
    private void RandomRocket(Vector3 origin, float radius);
    private BaseEntity CreateRocket(Vector3 startPoint, Vector3 direction, bool isFireRocket);
    private void ScaleAllDamage(List<DamageTypeEntry> damageTypes, float scale);
    private ItemDefinition GetRocket();
    private ItemDefinition GetFireRocket();
    [ConsoleCommand("meteor")]
     void ConsoleStart(ConsoleSystem.Arg args);
    [ChatCommand("meteor")]
     void ChatCmdControll(BasePlayer player, string command, string[] args, ulong playerid);
     void OnDispenserBonus(ResourceDispenser dispenser, BasePlayer player, Item item);
     object OnDispenserGather(ResourceDispenser dispenser, BaseEntity entity, Item item);
     void OnEntityBuilt(Planner planner, GameObject gameobject, Vector3 Pos);
     void GiveSeed(BasePlayer player, int amount);
    [ChatCommand("mseed")]
     void mSeed_cmd(BasePlayer player, string command, string[] arg);
     void SetFirstStage(Vector3 pos);
     string OrePrefab();
    public class MeteorSeed : FacepunchBehaviour
    {
         OreResourceEntity ore;
         StagedResourceEntity stage;
         void Awake();
         void OreProgress();
         void OnDestroy();
    }

     void OnEntityKill(BaseNetworkable entity);
     void DrawUI(BasePlayer player, string msg);
     string ui;
     bool init;
    private GameObject FileManagerObject;
    private FileManager m_FileManager;
    private string Images;
     IEnumerator LoadImages();
     void InitFileManager();
     class FileManager : MonoBehaviour
    {
         int loaded;
         int needed;
        public bool IsFinished { get; set; }
        const ulong MaxActiveLoads;
         Dictionary<string, FileInfo> files;
        private class FileInfo
        {
            public string Url;
            public string Png;
        }

        public string GetPng(string name);
        public IEnumerator LoadFile(string name, string url, int size);
         IEnumerator LoadImageCoroutine(string name, string url, int size);
        static byte[] Resize(byte[] bytes, int size);
    }

    private List<RadiationZone> radiationZone;
    private class RadiationZone : MonoBehaviour
    {
        private TriggerRadiation rads;
        public float radius;
        public float amount;
        private void Awake();
        private void OnDestroy();
        public void DestroyZone();
        public void InitializeRadiationZone(Vector3 position, float radius, float amount);
        public void Deactivate();
        public void Reactivate();
        public void AmountChange(float amount);
        private void UpdateCollider();
    }

    private void CreateZone(Vector3 pos);
    private void CrateWave();
     SpawnFilter filter;
     List<Vector3> monuments;
    static float GetGroundPosition(Vector3 pos);
    public Vector3 RandomDropPosition();
     List<int> BlockedLayers;
    static int blockedMask;
    public Vector3 GetSafeDropPosition(Vector3 position);
    public Vector3 GetEventPosition();
     Vector3 RandomCircle(Vector3 center, float radius);
    private void SendToChat(BasePlayer Player, string Message);
     string GetMsg(string key, object userID);
     void LoadMessages();
    public static string FormatTime(TimeSpan time);
    private static string Format(int units, string form1, string form2, string form3);
}

private class PluginConfig
{
    [JsonProperty("Общие Настройки")]
    public Options _Options;
    public class Options
    {
        [JsonProperty("Отключать стандартную радиацию?")]
        public bool OffStandartRad;
        [JsonProperty("Включить автозапуск ивента?")]
        public bool EnableAutomaticEvents;
        [JsonProperty("Настройка автозапуска")]
        public Timers EventTimers;
        [JsonProperty("Сообщать о начале ивента в чат?")]
        public bool NotifyEvent;
        [JsonProperty("Включить эффект тряски земли от падения метеорита?")]
        public bool Earthquake { get; set; }
        [JsonProperty("Минимальное количество игроков для запуска ивента")]
        public int MinPlayer;
    }

    [JsonProperty("Настройки UI")]
    public UiSettings uiSettings;
    public class UiSettings
    {
        [JsonProperty("Включить UI?")]
        public bool useUi;
        [JsonProperty("Ссылка на картинку")]
        public string IconImage;
        [JsonProperty("Положение UI")]
        public UiTransform uiTransform;
        public class UiTransform
        {
            [JsonProperty("Координата Х Мин")]
            public string AnchorXMin;
            [JsonProperty("Координата Х Мax")]
            public string AnchorXMax;
            [JsonProperty("Координата Y Мин")]
            public string AnchorYMin;
            [JsonProperty("Координата Y Мax")]
            public string AnchorYMax;
        }

    }

    [JsonProperty("Настройки радиации")]
    public Radiations _Radiations;
    public class Radiations
    {
        [JsonProperty("Радиус зоны")]
        public float Radius;
        [JsonProperty("Сила радиации")]
        public float Strange;
    }

    public class Timers
    {
        [JsonProperty("Интервал ивента (Минуты) (Если выключен рандом)")]
        public int EventInterval;
        [JsonProperty("Включить рандомное время?")]
        public bool UseRandomTimer;
        [JsonProperty("Минимальный интервал (Минуты)")]
        public int RandomTimerMin;
        [JsonProperty("Максимальный интервал (Минуты)")]
        public int RandomTimerMax;
    }

    [JsonProperty("Настройка ивента")]
    public Intensity Settings;
    public class Intensity
    {
        [JsonProperty("Шанс распространения огня от малого метеорита")]
        public int FireRocketChance;
        [JsonProperty("Радиус на котором проходит метеоритопад")]
        public float Radius;
        [JsonProperty("Количество падающих метеоритов (малых)")]
        public int RocketAmount;
        [JsonProperty("Длительность падения малых метеоритов")]
        public int Duration;
        [JsonProperty("Множитель урона от попадания по Enemy")]
        public float DamageMultiplier;
        [JsonProperty("Настройка выпадающих ресурсов после попадания метеорита по земле")]
        public Drop ItemDropControl;
        public class Drop
        {
            [JsonProperty("Включить дроп ресурсов после метеорита?")]
            public bool EnableItemDrop;
            [JsonProperty("Настройка выпадаемых ресурсов")]
            public ItemDrops[] ItemsToDrop;
        }

        [JsonProperty("Количество NPC возле главного метеорита")]
        public int NpcAmount;
        [JsonProperty("Включить спавн NPC возле метеорита?")]
        public bool NpcSpawn;
        [JsonProperty("Количество HP у ученых")]
        public float NpcHealth;
        [JsonProperty("Множитель урона от ученых")]
        public float NpcDamage;
        [JsonProperty("Время которое будет остывать метеорит (Минуты)")]
        public float FireTime;
        [JsonProperty("Время через которое метеорит исчезнет после остывания (Минуты)")]
        public float DespawnTime;
    }

    [JsonProperty("Настройки метеорита")]
    public MeteorSetting meteorSetting;
    public class MeteorSetting
    {
        [JsonProperty("Время до падения метеорита")]
        public int MeteorTime;
        [JsonProperty("Шанс того, что метеорит будет радиоактивен (0-отключить)")]
        public float RadChacnce;
        [JsonProperty("Запускать волну радиации после приземления если метеорит радиоактивный?")]
        public bool RadWave;
        [JsonProperty("HP серной руды (стандартно 500)")]
        public float SulfurHealth;
        [JsonProperty("HP металлической руды (стандартно 500)")]
        public float MetalHealth;
        [JsonProperty("Наносить дамаг по области?")]
        public bool SplashDamage;
        [JsonProperty("Радиус области")]
        public float splashRadius;
        [JsonProperty("Наносимый дамаг (Урон наносится всем строительным блокам) ")]
        public float DamageAmount;
    }

    [JsonProperty("Настройка семечки")]
    public SeedSettings seedSettings;
    public class SeedSettings
    {
        [JsonProperty("Включить выпадение семечки?")]
        public bool IsEnable;
        [JsonProperty("Cажать семечку только на грядки?")]
        public bool PlantOnly;
        [JsonProperty("Скин семенчки")]
        public ulong SeedSkinID;
        [JsonProperty("Название семечки")]
        public string SeedName;
        [JsonProperty("Время через которое обьект вырастет (секунды)")]
        public float TimeToRelise;
        [JsonProperty("Прифаб обьекта - Настройка")]
        public Dictionary<string, ObjectSetting> PrefabSetting;
    }

}

public class Options
{
    [JsonProperty("Отключать стандартную радиацию?")]
    public bool OffStandartRad;
    [JsonProperty("Включить автозапуск ивента?")]
    public bool EnableAutomaticEvents;
    [JsonProperty("Настройка автозапуска")]
    public Timers EventTimers;
    [JsonProperty("Сообщать о начале ивента в чат?")]
    public bool NotifyEvent;
    [JsonProperty("Включить эффект тряски земли от падения метеорита?")]
    public bool Earthquake { get; set; }
    [JsonProperty("Минимальное количество игроков для запуска ивента")]
    public int MinPlayer;
}

public class UiSettings
{
    [JsonProperty("Включить UI?")]
    public bool useUi;
    [JsonProperty("Ссылка на картинку")]
    public string IconImage;
    [JsonProperty("Положение UI")]
    public UiTransform uiTransform;
    public class UiTransform
    {
        [JsonProperty("Координата Х Мин")]
        public string AnchorXMin;
        [JsonProperty("Координата Х Мax")]
        public string AnchorXMax;
        [JsonProperty("Координата Y Мин")]
        public string AnchorYMin;
        [JsonProperty("Координата Y Мax")]
        public string AnchorYMax;
    }

}

public class UiTransform
{
    [JsonProperty("Координата Х Мин")]
    public string AnchorXMin;
    [JsonProperty("Координата Х Мax")]
    public string AnchorXMax;
    [JsonProperty("Координата Y Мин")]
    public string AnchorYMin;
    [JsonProperty("Координата Y Мax")]
    public string AnchorYMax;
}

public class Radiations
{
    [JsonProperty("Радиус зоны")]
    public float Radius;
    [JsonProperty("Сила радиации")]
    public float Strange;
}

public class Timers
{
    [JsonProperty("Интервал ивента (Минуты) (Если выключен рандом)")]
    public int EventInterval;
    [JsonProperty("Включить рандомное время?")]
    public bool UseRandomTimer;
    [JsonProperty("Минимальный интервал (Минуты)")]
    public int RandomTimerMin;
    [JsonProperty("Максимальный интервал (Минуты)")]
    public int RandomTimerMax;
}

public class Intensity
{
    [JsonProperty("Шанс распространения огня от малого метеорита")]
    public int FireRocketChance;
    [JsonProperty("Радиус на котором проходит метеоритопад")]
    public float Radius;
    [JsonProperty("Количество падающих метеоритов (малых)")]
    public int RocketAmount;
    [JsonProperty("Длительность падения малых метеоритов")]
    public int Duration;
    [JsonProperty("Множитель урона от попадания по Enemy")]
    public float DamageMultiplier;
    [JsonProperty("Настройка выпадающих ресурсов после попадания метеорита по земле")]
    public Drop ItemDropControl;
    public class Drop
    {
        [JsonProperty("Включить дроп ресурсов после метеорита?")]
        public bool EnableItemDrop;
        [JsonProperty("Настройка выпадаемых ресурсов")]
        public ItemDrops[] ItemsToDrop;
    }

    [JsonProperty("Количество NPC возле главного метеорита")]
    public int NpcAmount;
    [JsonProperty("Включить спавн NPC возле метеорита?")]
    public bool NpcSpawn;
    [JsonProperty("Количество HP у ученых")]
    public float NpcHealth;
    [JsonProperty("Множитель урона от ученых")]
    public float NpcDamage;
    [JsonProperty("Время которое будет остывать метеорит (Минуты)")]
    public float FireTime;
    [JsonProperty("Время через которое метеорит исчезнет после остывания (Минуты)")]
    public float DespawnTime;
}

public class Drop
{
    [JsonProperty("Включить дроп ресурсов после метеорита?")]
    public bool EnableItemDrop;
    [JsonProperty("Настройка выпадаемых ресурсов")]
    public ItemDrops[] ItemsToDrop;
}

public class MeteorSetting
{
    [JsonProperty("Время до падения метеорита")]
    public int MeteorTime;
    [JsonProperty("Шанс того, что метеорит будет радиоактивен (0-отключить)")]
    public float RadChacnce;
    [JsonProperty("Запускать волну радиации после приземления если метеорит радиоактивный?")]
    public bool RadWave;
    [JsonProperty("HP серной руды (стандартно 500)")]
    public float SulfurHealth;
    [JsonProperty("HP металлической руды (стандартно 500)")]
    public float MetalHealth;
    [JsonProperty("Наносить дамаг по области?")]
    public bool SplashDamage;
    [JsonProperty("Радиус области")]
    public float splashRadius;
    [JsonProperty("Наносимый дамаг (Урон наносится всем строительным блокам) ")]
    public float DamageAmount;
}

public class SeedSettings
{
    [JsonProperty("Включить выпадение семечки?")]
    public bool IsEnable;
    [JsonProperty("Cажать семечку только на грядки?")]
    public bool PlantOnly;
    [JsonProperty("Скин семенчки")]
    public ulong SeedSkinID;
    [JsonProperty("Название семечки")]
    public string SeedName;
    [JsonProperty("Время через которое обьект вырастет (секунды)")]
    public float TimeToRelise;
    [JsonProperty("Прифаб обьекта - Настройка")]
    public Dictionary<string, ObjectSetting> PrefabSetting;
}

public class ObjectSetting
{
    [JsonProperty("Максимальное ХП обьекта")]
    public float MaxHealth;
    [JsonProperty("Шанс спавна обьекта")]
    public float SpawnChance;
}

 class ItemDrops
{
    public string Shortname;
    public int Minimum;
    public int Maximum;
}

 class ItemCarrier : MonoBehaviour
{
    private ItemDrops[] carriedItems;
    private float multiplier;
    public void SetCarriedItems(ItemDrops[] carriedItems);
    public void SetDropMultiplier(float multiplier);
    private void OnDestroy();
}

public class MeteorSeed : FacepunchBehaviour
{
     OreResourceEntity ore;
     StagedResourceEntity stage;
     void Awake();
     void OreProgress();
     void OnDestroy();
}

 class FileManager : MonoBehaviour
{
     int loaded;
     int needed;
    public bool IsFinished { get; set; }
    const ulong MaxActiveLoads;
     Dictionary<string, FileInfo> files;
    private class FileInfo
    {
        public string Url;
        public string Png;
    }

    public string GetPng(string name);
    public IEnumerator LoadFile(string name, string url, int size);
     IEnumerator LoadImageCoroutine(string name, string url, int size);
    static byte[] Resize(byte[] bytes, int size);
}

private class FileInfo
{
    public string Url;
    public string Png;
}

private class RadiationZone : MonoBehaviour
{
    private TriggerRadiation rads;
    public float radius;
    public float amount;
    private void Awake();
    private void OnDestroy();
    public void DestroyZone();
    public void InitializeRadiationZone(Vector3 position, float radius, float amount);
    public void Deactivate();
    public void Reactivate();
    public void AmountChange(float amount);
    private void UpdateCollider();
}


```

---

## MicroPanel

```csharp
using System;
using System.Collections.Generic;
using System.Globalization;
using System.Linq;
using Newtonsoft.Json;
using Oxide.Core.Libraries.Covalence;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using UnityEngine;

Oxide.Plugins
[Info("MicroPanel", "LAGZYA", "2.0.9")]
public class MicroPanel : RustPlugin
{
    private void OnServerInitialized();
    private void Check();
    private void OnUserDisconnected(IPlayer player);
    private void OnPlayerConnected(BasePlayer player);
    private void Unload();
    private List<ulong> closePanel;
    [ChatCommand("panel")]
     void CloseOpenPanel(BasePlayer player);
    private static ConfigData cfg { get; set; }
    private class ConfigData
    {
        [JsonProperty("Лейбел")]
        public string ServerName;
        [JsonProperty("Иконка")]
        public string icon;
        [JsonProperty("Цвет полоски")]
        public string colorpolos;
        [JsonProperty("Включить поддержку плагина IQFakeActive")]
        public bool IQFakeActive;
        [JsonProperty("Вкдючить показ времени")]
        public bool time;
        [JsonProperty("ЦВЕТ(ВКЛЮЧЕННЫХ ИВЕНТОВ И ОНЛАЙНА)")]
        public string coloron;
        [JsonProperty("ЦВЕТ(ОФЛАЙНА И ВЫКЛЮЧЕННЫХ ИВЕНТОВ)")]
        public string coloroff;
        [JsonProperty("Двигать всю панель - MIN")]
        public string offsetmin;
        [JsonProperty("Двигать всю панель - MAX")]
        public string offsetmax;
        [JsonProperty("Двигать панель ивентов - MIN")]
        public string evoffsetmin;
        [JsonProperty("Двигать панель ивентов - MAX")]
        public string evoffsetmax;
        [JsonProperty("Показывать время?")]
        public bool stime;
        [JsonProperty("Как часто обновлять время (СЕК)?")]
        public int utime;
        [JsonProperty("Двигать панель времени - MIN")]
        public string tvoffsetmin;
        [JsonProperty("Двигать панель времени  - MAX")]
        public string tvoffsetmax;
        [JsonProperty("Показывать слипперов?")]
        public bool sleep;
        [JsonProperty("Курсор включать?")]
        public bool cursor;
        [JsonProperty("Включить авто новости?")]
        public bool newson;
        [JsonProperty("Время обновление новостей(В секундах)")]
        public float sec;
        [JsonProperty("АвтоНовости")]
        public List<string> newsList;
        [JsonProperty("Кнопки")]
        public List<Buttons> buttonList;
        public static ConfigData GetNewConf();
    }

    internal class Buttons
    {
        public string Commnad;
        public string Name;
    }

    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    protected override void LoadConfig();
    private string Layer;
    private CuiPanel _mainPanel;
     object OnPlayerSleep(BasePlayer player);
     void OnPlayerSleepEnded(BasePlayer player);
     void OnEntitySpawned(BaseNetworkable entity);
     void OnEntityKill(BaseNetworkable entity);
    private void UpdateTime(BasePlayer player);
    [PluginReference]
     Plugin IQFakeActive;
     int FakeOnline { get; set; }
     void SyncReservedFinish();
    private readonly List<ulong> listPlayer;
    [ConsoleCommand("Ui_MicroPanel")]
    private void CommandUi(ConsoleSystem.Arg arg);
    private void MenuUpdate(BasePlayer player, string type);
    private void OnlinePlayer();
    private bool IsAir;
    private bool IsHeli;
    private bool isTank;
    private bool IsCargo;
    private bool IsCh;
    private void EventInit(BasePlayer player, string type);
    private void StartUi(BasePlayer player);
    public string news;
    public int newsId;
     void GenerateNews();
     void LoadNews();
    private string GetImage(string shortname, ulong skin);
    private bool AddImage(string url, string shortname, ulong skin);
    [PluginReference]
    private Plugin ImageLibrary;
    private static string HexToRustFormat(string hex);
}

private class ConfigData
{
    [JsonProperty("Лейбел")]
    public string ServerName;
    [JsonProperty("Иконка")]
    public string icon;
    [JsonProperty("Цвет полоски")]
    public string colorpolos;
    [JsonProperty("Включить поддержку плагина IQFakeActive")]
    public bool IQFakeActive;
    [JsonProperty("Вкдючить показ времени")]
    public bool time;
    [JsonProperty("ЦВЕТ(ВКЛЮЧЕННЫХ ИВЕНТОВ И ОНЛАЙНА)")]
    public string coloron;
    [JsonProperty("ЦВЕТ(ОФЛАЙНА И ВЫКЛЮЧЕННЫХ ИВЕНТОВ)")]
    public string coloroff;
    [JsonProperty("Двигать всю панель - MIN")]
    public string offsetmin;
    [JsonProperty("Двигать всю панель - MAX")]
    public string offsetmax;
    [JsonProperty("Двигать панель ивентов - MIN")]
    public string evoffsetmin;
    [JsonProperty("Двигать панель ивентов - MAX")]
    public string evoffsetmax;
    [JsonProperty("Показывать время?")]
    public bool stime;
    [JsonProperty("Как часто обновлять время (СЕК)?")]
    public int utime;
    [JsonProperty("Двигать панель времени - MIN")]
    public string tvoffsetmin;
    [JsonProperty("Двигать панель времени  - MAX")]
    public string tvoffsetmax;
    [JsonProperty("Показывать слипперов?")]
    public bool sleep;
    [JsonProperty("Курсор включать?")]
    public bool cursor;
    [JsonProperty("Включить авто новости?")]
    public bool newson;
    [JsonProperty("Время обновление новостей(В секундах)")]
    public float sec;
    [JsonProperty("АвтоНовости")]
    public List<string> newsList;
    [JsonProperty("Кнопки")]
    public List<Buttons> buttonList;
    public static ConfigData GetNewConf();
}

internal class Buttons
{
    public string Commnad;
    public string Name;
}


```

---

## MindFreeze

```csharp
using System.Collections.Generic;
using System.Reflection;
using System;
using System.Linq;
using System.Data;
using UnityEngine;
using Oxide.Core;

Oxide.Plugins
[Info("Mind Freeze", "PaiN", "2.1.0", ResourceId = 1198)]
[Description("Allows you to freeze players with a legit way.")]
 class MindFreeze : RustPlugin
{
    private Timer _timer;
    private class FrozenPlayerInfo
    {
        public BasePlayer Player { get; set; }
        public Vector3 FrozenPosition { get; set; }
        public FrozenPlayerInfo(BasePlayer player);
    }

     List<FrozenPlayerInfo> frozenPlayers;
     void Loaded();
    [ChatCommand("freeze")]
     void cmdFreeze(BasePlayer player, string cmd, string[] args);
    [ChatCommand("unfreeze")]
     void cmdUnFreeze(BasePlayer player, string cmd, string[] args);
    [ChatCommand("unfreezeall")]
     void cmdUnFreezeAll(BasePlayer player, string cmd, string[] args);
     void OnTimer();
     void Unloaded();
}

private class FrozenPlayerInfo
{
    public BasePlayer Player { get; set; }
    public Vector3 FrozenPosition { get; set; }
    public FrozenPlayerInfo(BasePlayer player);
}


```

---

## MinicopterCombat

```csharp
using Oxide.Core;
using Oxide.Game.Rust.Cui;
using Oxide.Core.Libraries;
using System;
using System.Collections.Generic;
using UnityEngine;
using System.Linq;
using Rust;
using Newtonsoft.Json;
using ProtoBuf;
using Oxide.Core.Plugins;

Oxide.Plugins
[Info("MinicopterCombat", "Karuza", "01.00.02")]
public class MinicopterCombat : RustPlugin
{
    [PluginReference]
     Plugin BulletProjectile;
    private static MinicopterCombat instance;
    private static string[] enabledMiniWeaponTypes;
    private static string[] enabledScrapWeaponTypes;
    private static readonly Dictionary<string, int> weaponsToAmmoTypeItemIdMap;
    private static Permission permissionHelper;
    private static string clear;
    private static string green;
    private static string darkGreen;
    private static string red;
    private static string darkRed;
    private static string solidRed;
    private static string white;
    private static string orange;
    private static string gold;
    private static string permAdmin;
    private static string reticleOverlayName;
    private static string primaryAmmoOverlayName;
    private static string secondaryAmmoOverlayName;
    private static string flareAmmoOverlayName;
    private static string primaryCooldownOverlayName;
    private static string secondaryCooldownOverlayName;
    private static string flareCooldownOverlayName;
    private static string weaponTypeOverlayName;
    private static string targetLockGuiName;
    private static string targetMessageGuiName;
    private static string flarePrefab;
    private static LayerMask layerMask;
    private static Configuration configuration;
    private void RegisterPermissions();
     void DestroyMinicopterCombatWrappers();
    static void GetEnabledWeapons();
     void UpdateExistingMinicopters();
     void GetWeaponAmmoTypeItemIds();
    private void AddAmmoTypeToAmmoTypeMap(string ammoTypeShortName);
    private static float AngleOffAroundAxis(Vector3 v, Vector3 forward, Vector3 axis);
    public class Configuration
    {
        [JsonProperty]
        public float DebounceTimeSeconds { get; set; }
        [JsonProperty]
        public bool DisplayOutOfAmmoMessage { get; set; }
        [JsonProperty]
        public bool DisplaySelectedWeaponMessage { get; set; }
        [JsonProperty]
        public bool UnlimitedAmmo { get; set; }
        [JsonProperty]
        public bool DisablePermissionCheck { get; set; }
        [JsonProperty]
        public bool ApplyToScrapCopter { get; set; }
        [JsonProperty]
        public string FlareFiredSfx { get; set; }
        [JsonProperty]
        public string SwitchWeaponSfx { get; set; }
        [JsonProperty]
        public string AlarmSfx { get; set; }
        [JsonProperty]
        public float CounterMeasureDespawnTime { get; set; }
        [JsonProperty]
        public float WeaponSwitchDelay { get; set; }
        [JsonProperty]
        public BUTTON FirePrimaryButton { get; set; }
        [JsonProperty]
        public BUTTON FireSecondaryButton { get; set; }
        [JsonProperty]
        public BUTTON SwitchWeaponButton { get; set; }
        [JsonProperty]
        public BUTTON FireFlareButton { get; set; }
        [JsonProperty]
        public bool EnableScrapcopterGibs { get; set; }
        [JsonProperty]
        public float GibsDespawnTimerOverride { get; set; }
        [JsonProperty]
        public bool DisableFire { get; set; }
        [JsonProperty]
        public bool HideUnauthorizedWeapons { get; set; }
        [JsonProperty]
        public Dictionary<string, WeaponSettings> WeaponSettings;
        [JsonProperty]
        public Dictionary<string, WeaponSettings> ScrapcopterWeaponSettings;
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
     void OnServerInitialized();
     void Unload();
     void OnEntityMounted(BaseMountable entity, BasePlayer player);
     void OnEntityDismounted(BaseMountable entity, BasePlayer player);
     void OnEntitySpawned(HelicopterDebris debris);
     void OnEntitySpawned(MiniCopter miniCopter);
    public class ScrapcopterWrapper : HelicopterWrapper
    {
        protected override float maxAngle { get; set; }
        protected override float minAngle { get; set; }
        protected override float centerX { get; set; }
        protected override float centerY { get; set; }
        protected override string reticleGuiOverlayMin { get; set; }
        protected override string reticleGuiOverlayMax { get; set; }
        protected override string primaryAmmoMin { get; set; }
        protected override string primaryAmmoMax { get; set; }
        protected override string secondaryAmmoMin { get; set; }
        protected override string secondaryAmmoMax { get; set; }
        protected override string flareMin { get; set; }
        protected override string flareMax { get; set; }
        protected override string lockGuiMin { get; set; }
        protected override string lockGuiMax { get; set; }
        protected override string GetNextWeaponType(string current);
        protected override Dictionary<string, WeaponSettings> GetAvailableWeapons();
    }

    public class MinicopterWrapper : HelicopterWrapper
    {
        protected override float maxAngle { get; set; }
        protected override float minAngle { get; set; }
        protected override float centerX { get; set; }
        protected override float centerY { get; set; }
        protected override string reticleGuiOverlayMin { get; set; }
        protected override string reticleGuiOverlayMax { get; set; }
        protected override string primaryAmmoMin { get; set; }
        protected override string primaryAmmoMax { get; set; }
        protected override string secondaryAmmoMin { get; set; }
        protected override string secondaryAmmoMax { get; set; }
        protected override string flareMin { get; set; }
        protected override string flareMax { get; set; }
        protected override string lockGuiMin { get; set; }
        protected override string lockGuiMax { get; set; }
        protected override string GetNextWeaponType(string current);
        protected override Dictionary<string, WeaponSettings> GetAvailableWeapons();
    }

    public class HelicopterWrapper : MonoBehaviour, Lock
    {
        public Guid Id { get; set; }
        public float ChanceToLoseLock { get; set; }
        public bool IsAlive { get; set; }
        private BasePlayer player { get; set; }
        private string selectedWeaponType { get; set; }
        private WeaponSettings weaponSettings { get; set; }
        private bool IsLocked { get; set; }
        private bool primaryCooldownContainerActive;
        private bool primaryCooldownContainerContrastActive;
        private bool secondaryCooldownContainerActive;
        private bool secondaryCooldownContainerContrastActive;
        private bool flareCooldownContainerActive;
        private bool flareCooldownContainerContrastActive;
        private float lastMsgTime;
        private float lastPrimaryWeaponFiredTime;
        private float lastSecondaryWeaponFiredTime;
        private float lastWeaponSwitchTime;
        private int primaryRoundsFired;
        private int secondaryRoundsFired;
        private int flareRoundsFired;
        private float primaryCooldownTimer;
        private float secondaryCooldownTimer;
        private float flareCooldownTimer;
        private int primaryCurrentBarrel;
        private int secondaryCurrentBarrel;
        private BaseVehicle vehicle { get; set; }
        protected virtual string reticleGuiOverlayMin { get; set; }
        protected virtual string reticleGuiOverlayMax { get; set; }
        protected virtual string primaryAmmoMin { get; set; }
        protected virtual string primaryAmmoMax { get; set; }
        protected virtual string secondaryAmmoMin { get; set; }
        protected virtual string secondaryAmmoMax { get; set; }
        protected virtual string flareMin { get; set; }
        protected virtual string flareMax { get; set; }
        protected virtual string lockGuiMin { get; set; }
        protected virtual string lockGuiMax { get; set; }
        private float lockOnSeconds;
        private float lockedAt;
        private bool isLockAlarmPlaying;
        private bool targetLocked;
        private bool acquiringLock;
        private BaseEntity target;
        protected virtual float maxAngle { get; set; }
        protected virtual float minAngle { get; set; }
        protected virtual float centerX { get; set; }
        protected virtual float centerY { get; set; }
        private float targetAngleX;
        private float targetAngleY;
        private float timeBeforeFlaresExpire;
        private float lastFlaresFiredTime;
        private bool leftFlareFiredLast { get; set; }
        private Dictionary<Guid, Lock> locks { get; set; }
        protected virtual Dictionary<string, WeaponSettings> GetAvailableWeapons();
        private void UpdateWeapons();
        private void FireFlares();
        private void ResetRoundsFiredByConfig(WeaponConfiguration weaponConfiguration, float lastFiredTime, int roundsFired, float cooldownTimer);
        private void ResetRoundsFired(float coolDownDecay, float lastFiredTime, int roundsFired, float cooldownTimer);
        private Vector3 GetMuzzlePosition(WeaponConfiguration weaponConfiguration, int currentBarrel);
        private Vector3 GetGattlingMuzzlePos(Transform vehicleTransform, int currentBarrel);
        private void TriggerMuzzleEffect(WeaponConfiguration weaponConfiguration, int currentBarrel);
        private Vector3 GetGattlingEffectPos(int currentBarrel);
        private void FireProjectile(WeaponConfiguration weaponConfiguration);
        private void UpdateBarrel(BarrelConfiguration barrelConfig, int nextBarrel);
        private bool DoesUserHaveAmmo(string ammoTypeShortName, float currentTime);
        private void FireWithProjectileSystem(WeaponConfiguration weaponConfiguration, Vector3 muzzlePos);
        private GameObject FireWithServerProjectile(WeaponConfiguration weaponConfiguration, Vector3 muzzlePos);
        private void FireAARocket(WeaponConfiguration weaponConfiguration, Vector3 muzzlePos);
        private void UseAmmo(string ammoTypeShortName, bool? isPrimary);
        private void SwitchWeaponType();
        protected virtual string GetNextWeaponType(string current);
        public void TriggerLockAcquired(Lock lck);
        public void TriggerLockLost(Lock lck);
        private void LockTarget(Vector3 muzzlePos, WeaponConfiguration weaponConfiguration);
        private void ResetWeapons();
        public static Vector2 WorldPosToImagePos(Vector3 worldPos);
        public void ClearTarget();
        private void PlayLockAlarm();
        public void SetLockTarget(BaseEntity target);
        public void SetPlayer(BasePlayer player);
        public void RemovePlayer(BasePlayer player);
        private void Awake();
        private void FixedUpdate();
        private void OnDestroy();
        private void Toggle_WeaponTypesGui(bool show);
        private void Draw_WeaponSelectionBox(CuiElementContainer weaponSelectionContainer);
        private void Toggle_ReticleGui(bool show);
        private void UpdateTargetLock();
        private void Toggle_DrawTargetLock(bool show);
        private void Draw_ReticleBox(CuiElementContainer reticleContainer);
        private void Draw_ReticleByWeaponType(CuiElementContainer reticleContainer);
        private void Toggle_CooldownGui(bool show);
        private void Draw_FlareCooldownBox();
        private void Draw_WeaponCooldownBox(WeaponConfiguration weaponConfiguration, int roundsFired, string overlayName, bool cooldownContainerActive, bool cooldownContrastActive);
        private void Draw_CooldownBox(string min, string max, int shotsBeforeCooldown, float roundsFired, string overlayName, string color, bool cooldownContainerActive, bool cooldownContrastActive);
        private void Toggle_FlareAmmoGui(bool show);
        private void Toggle_AmmoGui(bool show, bool? isPrimary);
        private void Draw_AmmoText(string ammoShortName, string overlayName, string min, string max, string prefix);
    }

    private class AARocket : MonoBehaviour, Lock
    {
        public Guid Id { get; set; }
        public float ChanceToLoseLock { get; set; }
        public float AngleModifier { get; set; }
        private ServerProjectile projectile;
        private BaseEntity target;
        private void Awake();
        private void FixedUpdate();
        private void UpdateRocketPosition();
        private void OnDestroy();
        public void Initialize(BaseEntity target, float chanceToLoseLock);
        public void SetLockTarget(BaseEntity target);
    }

    private class CounterMeasure : MonoBehaviour
    {
        private BaseEntity baseEntity;
        private Vector3 lastPosition;
        private float timeAlive;
        private float sparkSpawnLimiter;
        protected static Effect reusableInstance;
        private void Awake();
        private void FixedUpdate();
        private void OnDestroy();
    }

    public class UI
    {
        static public CuiElementContainer CreateElementContainer(string parent, string panelName, string color, string aMin, string aMax, bool useCursor);
        static public void CreatePanel(CuiElementContainer container, string panel, string color, string aMin, string aMax, bool cursor);
        static public void CreatePanel(CuiElementContainer container, string panel, string color, RectTransform rectTransform, bool cursor);
        static public void CreateLabel(CuiElementContainer container, string panel, string color, string text, int size, string aMin, string aMax, TextAnchor align);
    }

    public class WeaponConfiguration
    {
        public string AmmoTypeShortName { get; set; }
        public string AmmoPrefabPath { get; set; }
        public string MuzzleEffect { get; set; }
        public ProjectileType ProjectileType { get; set; }
        public float WeaponSpeed { get; set; }
        public float FireRate { get; set; }
        public float MinBlastRadius { get; set; }
        public float MaxBlastRadius { get; set; }
        [JsonProperty(PropertyName = "CoolDownDecay: Speed at which the cooldown is decreased in seconds")]
        public float CoolDownDecay { get; set; }
        [JsonProperty(PropertyName = "Min Detonation Time for Timed Explosives")]
        public float DetonationTimerMin { get; set; }
        [JsonProperty(PropertyName = "Max Detonation Time for Timed Explosives")]
        public float DetonationTimerMax { get; set; }
        [JsonProperty(PropertyName = "Air2Air: Angle Modifier for Rockets (Set to Override Default 30)")]
        public float AngleModifier { get; set; }
        [JsonProperty(PropertyName = "TargetLocker: Time Before Losing the Lock")]
        public float TimeBeforeLockExpires { get; set; }
        [JsonProperty(PropertyName = "TargetLocker: Time Required Before Locking Target")]
        public float TimeToLock { get; set; }
        public float AimCone { get; set; }
        public bool RequiresAmmo { get; set; }
        public bool ClearAATargetOnFire { get; set; }
        public bool AllowCH47HSR { get; set; }
        public bool AllowPatrolHeliHSR { get; set; }
        public int ShotsBeforeCoolDown { get; set; }
        public BarrelConfiguration BarrelConfiguration { get; set; }
        public List<DamageTypeEntry> DamageTypes { get; set; }
    }

    public class WeaponSettings
    {
        public WeaponConfiguration PrimaryWeapon { get; set; }
        public WeaponConfiguration SecondaryWeapon { get; set; }
        public string DisplayShortName { get; set; }
        public string DisplayFullName { get; set; }
        public string WeaponPermission { get; set; }
        public string FlareShortName { get; set; }
        public HUDConfiguration HudConfiguration { get; set; }
        public float FlareFireRate { get; set; }
        public float FlareCooldownDecay { get; set; }
        public int FlaresBeforeCooldown { get; set; }
        [JsonProperty(PropertyName = "TargetLocker/Air2Air: % Chance to Lose Lock [0-100]")]
        public float ChanceToLoseLock { get; set; }
        public bool UnlimitedFlares { get; set; }
        public bool FlaresEnabled { get; set; }
        public bool Enabled { get; set; }
    }

}

public class Configuration
{
    [JsonProperty]
    public float DebounceTimeSeconds { get; set; }
    [JsonProperty]
    public bool DisplayOutOfAmmoMessage { get; set; }
    [JsonProperty]
    public bool DisplaySelectedWeaponMessage { get; set; }
    [JsonProperty]
    public bool UnlimitedAmmo { get; set; }
    [JsonProperty]
    public bool DisablePermissionCheck { get; set; }
    [JsonProperty]
    public bool ApplyToScrapCopter { get; set; }
    [JsonProperty]
    public string FlareFiredSfx { get; set; }
    [JsonProperty]
    public string SwitchWeaponSfx { get; set; }
    [JsonProperty]
    public string AlarmSfx { get; set; }
    [JsonProperty]
    public float CounterMeasureDespawnTime { get; set; }
    [JsonProperty]
    public float WeaponSwitchDelay { get; set; }
    [JsonProperty]
    public BUTTON FirePrimaryButton { get; set; }
    [JsonProperty]
    public BUTTON FireSecondaryButton { get; set; }
    [JsonProperty]
    public BUTTON SwitchWeaponButton { get; set; }
    [JsonProperty]
    public BUTTON FireFlareButton { get; set; }
    [JsonProperty]
    public bool EnableScrapcopterGibs { get; set; }
    [JsonProperty]
    public float GibsDespawnTimerOverride { get; set; }
    [JsonProperty]
    public bool DisableFire { get; set; }
    [JsonProperty]
    public bool HideUnauthorizedWeapons { get; set; }
    [JsonProperty]
    public Dictionary<string, WeaponSettings> WeaponSettings;
    [JsonProperty]
    public Dictionary<string, WeaponSettings> ScrapcopterWeaponSettings;
}

public class ScrapcopterWrapper : HelicopterWrapper
{
    protected override float maxAngle { get; set; }
    protected override float minAngle { get; set; }
    protected override float centerX { get; set; }
    protected override float centerY { get; set; }
    protected override string reticleGuiOverlayMin { get; set; }
    protected override string reticleGuiOverlayMax { get; set; }
    protected override string primaryAmmoMin { get; set; }
    protected override string primaryAmmoMax { get; set; }
    protected override string secondaryAmmoMin { get; set; }
    protected override string secondaryAmmoMax { get; set; }
    protected override string flareMin { get; set; }
    protected override string flareMax { get; set; }
    protected override string lockGuiMin { get; set; }
    protected override string lockGuiMax { get; set; }
    protected override string GetNextWeaponType(string current);
    protected override Dictionary<string, WeaponSettings> GetAvailableWeapons();
}

public class MinicopterWrapper : HelicopterWrapper
{
    protected override float maxAngle { get; set; }
    protected override float minAngle { get; set; }
    protected override float centerX { get; set; }
    protected override float centerY { get; set; }
    protected override string reticleGuiOverlayMin { get; set; }
    protected override string reticleGuiOverlayMax { get; set; }
    protected override string primaryAmmoMin { get; set; }
    protected override string primaryAmmoMax { get; set; }
    protected override string secondaryAmmoMin { get; set; }
    protected override string secondaryAmmoMax { get; set; }
    protected override string flareMin { get; set; }
    protected override string flareMax { get; set; }
    protected override string lockGuiMin { get; set; }
    protected override string lockGuiMax { get; set; }
    protected override string GetNextWeaponType(string current);
    protected override Dictionary<string, WeaponSettings> GetAvailableWeapons();
}

public class HelicopterWrapper : MonoBehaviour, Lock
{
    public Guid Id { get; set; }
    public float ChanceToLoseLock { get; set; }
    public bool IsAlive { get; set; }
    private BasePlayer player { get; set; }
    private string selectedWeaponType { get; set; }
    private WeaponSettings weaponSettings { get; set; }
    private bool IsLocked { get; set; }
    private bool primaryCooldownContainerActive;
    private bool primaryCooldownContainerContrastActive;
    private bool secondaryCooldownContainerActive;
    private bool secondaryCooldownContainerContrastActive;
    private bool flareCooldownContainerActive;
    private bool flareCooldownContainerContrastActive;
    private float lastMsgTime;
    private float lastPrimaryWeaponFiredTime;
    private float lastSecondaryWeaponFiredTime;
    private float lastWeaponSwitchTime;
    private int primaryRoundsFired;
    private int secondaryRoundsFired;
    private int flareRoundsFired;
    private float primaryCooldownTimer;
    private float secondaryCooldownTimer;
    private float flareCooldownTimer;
    private int primaryCurrentBarrel;
    private int secondaryCurrentBarrel;
    private BaseVehicle vehicle { get; set; }
    protected virtual string reticleGuiOverlayMin { get; set; }
    protected virtual string reticleGuiOverlayMax { get; set; }
    protected virtual string primaryAmmoMin { get; set; }
    protected virtual string primaryAmmoMax { get; set; }
    protected virtual string secondaryAmmoMin { get; set; }
    protected virtual string secondaryAmmoMax { get; set; }
    protected virtual string flareMin { get; set; }
    protected virtual string flareMax { get; set; }
    protected virtual string lockGuiMin { get; set; }
    protected virtual string lockGuiMax { get; set; }
    private float lockOnSeconds;
    private float lockedAt;
    private bool isLockAlarmPlaying;
    private bool targetLocked;
    private bool acquiringLock;
    private BaseEntity target;
    protected virtual float maxAngle { get; set; }
    protected virtual float minAngle { get; set; }
    protected virtual float centerX { get; set; }
    protected virtual float centerY { get; set; }
    private float targetAngleX;
    private float targetAngleY;
    private float timeBeforeFlaresExpire;
    private float lastFlaresFiredTime;
    private bool leftFlareFiredLast { get; set; }
    private Dictionary<Guid, Lock> locks { get; set; }
    protected virtual Dictionary<string, WeaponSettings> GetAvailableWeapons();
    private void UpdateWeapons();
    private void FireFlares();
    private void ResetRoundsFiredByConfig(WeaponConfiguration weaponConfiguration, float lastFiredTime, int roundsFired, float cooldownTimer);
    private void ResetRoundsFired(float coolDownDecay, float lastFiredTime, int roundsFired, float cooldownTimer);
    private Vector3 GetMuzzlePosition(WeaponConfiguration weaponConfiguration, int currentBarrel);
    private Vector3 GetGattlingMuzzlePos(Transform vehicleTransform, int currentBarrel);
    private void TriggerMuzzleEffect(WeaponConfiguration weaponConfiguration, int currentBarrel);
    private Vector3 GetGattlingEffectPos(int currentBarrel);
    private void FireProjectile(WeaponConfiguration weaponConfiguration);
    private void UpdateBarrel(BarrelConfiguration barrelConfig, int nextBarrel);
    private bool DoesUserHaveAmmo(string ammoTypeShortName, float currentTime);
    private void FireWithProjectileSystem(WeaponConfiguration weaponConfiguration, Vector3 muzzlePos);
    private GameObject FireWithServerProjectile(WeaponConfiguration weaponConfiguration, Vector3 muzzlePos);
    private void FireAARocket(WeaponConfiguration weaponConfiguration, Vector3 muzzlePos);
    private void UseAmmo(string ammoTypeShortName, bool? isPrimary);
    private void SwitchWeaponType();
    protected virtual string GetNextWeaponType(string current);
    public void TriggerLockAcquired(Lock lck);
    public void TriggerLockLost(Lock lck);
    private void LockTarget(Vector3 muzzlePos, WeaponConfiguration weaponConfiguration);
    private void ResetWeapons();
    public static Vector2 WorldPosToImagePos(Vector3 worldPos);
    public void ClearTarget();
    private void PlayLockAlarm();
    public void SetLockTarget(BaseEntity target);
    public void SetPlayer(BasePlayer player);
    public void RemovePlayer(BasePlayer player);
    private void Awake();
    private void FixedUpdate();
    private void OnDestroy();
    private void Toggle_WeaponTypesGui(bool show);
    private void Draw_WeaponSelectionBox(CuiElementContainer weaponSelectionContainer);
    private void Toggle_ReticleGui(bool show);
    private void UpdateTargetLock();
    private void Toggle_DrawTargetLock(bool show);
    private void Draw_ReticleBox(CuiElementContainer reticleContainer);
    private void Draw_ReticleByWeaponType(CuiElementContainer reticleContainer);
    private void Toggle_CooldownGui(bool show);
    private void Draw_FlareCooldownBox();
    private void Draw_WeaponCooldownBox(WeaponConfiguration weaponConfiguration, int roundsFired, string overlayName, bool cooldownContainerActive, bool cooldownContrastActive);
    private void Draw_CooldownBox(string min, string max, int shotsBeforeCooldown, float roundsFired, string overlayName, string color, bool cooldownContainerActive, bool cooldownContrastActive);
    private void Toggle_FlareAmmoGui(bool show);
    private void Toggle_AmmoGui(bool show, bool? isPrimary);
    private void Draw_AmmoText(string ammoShortName, string overlayName, string min, string max, string prefix);
}

private class AARocket : MonoBehaviour, Lock
{
    public Guid Id { get; set; }
    public float ChanceToLoseLock { get; set; }
    public float AngleModifier { get; set; }
    private ServerProjectile projectile;
    private BaseEntity target;
    private void Awake();
    private void FixedUpdate();
    private void UpdateRocketPosition();
    private void OnDestroy();
    public void Initialize(BaseEntity target, float chanceToLoseLock);
    public void SetLockTarget(BaseEntity target);
}

private class CounterMeasure : MonoBehaviour
{
    private BaseEntity baseEntity;
    private Vector3 lastPosition;
    private float timeAlive;
    private float sparkSpawnLimiter;
    protected static Effect reusableInstance;
    private void Awake();
    private void FixedUpdate();
    private void OnDestroy();
}

public class UI
{
    static public CuiElementContainer CreateElementContainer(string parent, string panelName, string color, string aMin, string aMax, bool useCursor);
    static public void CreatePanel(CuiElementContainer container, string panel, string color, string aMin, string aMax, bool cursor);
    static public void CreatePanel(CuiElementContainer container, string panel, string color, RectTransform rectTransform, bool cursor);
    static public void CreateLabel(CuiElementContainer container, string panel, string color, string text, int size, string aMin, string aMax, TextAnchor align);
}

public class WeaponConfiguration
{
    public string AmmoTypeShortName { get; set; }
    public string AmmoPrefabPath { get; set; }
    public string MuzzleEffect { get; set; }
    public ProjectileType ProjectileType { get; set; }
    public float WeaponSpeed { get; set; }
    public float FireRate { get; set; }
    public float MinBlastRadius { get; set; }
    public float MaxBlastRadius { get; set; }
    [JsonProperty(PropertyName = "CoolDownDecay: Speed at which the cooldown is decreased in seconds")]
    public float CoolDownDecay { get; set; }
    [JsonProperty(PropertyName = "Min Detonation Time for Timed Explosives")]
    public float DetonationTimerMin { get; set; }
    [JsonProperty(PropertyName = "Max Detonation Time for Timed Explosives")]
    public float DetonationTimerMax { get; set; }
    [JsonProperty(PropertyName = "Air2Air: Angle Modifier for Rockets (Set to Override Default 30)")]
    public float AngleModifier { get; set; }
    [JsonProperty(PropertyName = "TargetLocker: Time Before Losing the Lock")]
    public float TimeBeforeLockExpires { get; set; }
    [JsonProperty(PropertyName = "TargetLocker: Time Required Before Locking Target")]
    public float TimeToLock { get; set; }
    public float AimCone { get; set; }
    public bool RequiresAmmo { get; set; }
    public bool ClearAATargetOnFire { get; set; }
    public bool AllowCH47HSR { get; set; }
    public bool AllowPatrolHeliHSR { get; set; }
    public int ShotsBeforeCoolDown { get; set; }
    public BarrelConfiguration BarrelConfiguration { get; set; }
    public List<DamageTypeEntry> DamageTypes { get; set; }
}

public class WeaponSettings
{
    public WeaponConfiguration PrimaryWeapon { get; set; }
    public WeaponConfiguration SecondaryWeapon { get; set; }
    public string DisplayShortName { get; set; }
    public string DisplayFullName { get; set; }
    public string WeaponPermission { get; set; }
    public string FlareShortName { get; set; }
    public HUDConfiguration HudConfiguration { get; set; }
    public float FlareFireRate { get; set; }
    public float FlareCooldownDecay { get; set; }
    public int FlaresBeforeCooldown { get; set; }
    [JsonProperty(PropertyName = "TargetLocker/Air2Air: % Chance to Lose Lock [0-100]")]
    public float ChanceToLoseLock { get; set; }
    public bool UnlimitedFlares { get; set; }
    public bool FlaresEnabled { get; set; }
    public bool Enabled { get; set; }
}


```

---

## MiniCopterOptions

```csharp
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;
using Oxide.Core;
using System;
using System.Collections.Generic;
using System.Linq;
using Oxide.Core.Plugins;
using UnityEngine;

Oxide.Plugins
[Info("Mini-Copter Options", "Pho3niX90", "2.5.1")]
[Description("Provide a number of additional options for Mini-Copters, including storage and seats.")]
internal class MiniCopterOptions : CovalencePlugin
{
    private readonly string minicopterPrefab;
    private readonly string storagePrefab;
    private readonly string storageLargePrefab;
    private readonly string autoturretPrefab;
    private readonly string switchPrefab;
    private readonly string searchLightPrefab;
    private readonly string flasherBluePrefab;
    private readonly string spherePrefab;
    private const string resizableLootPanelName;
    private const int MinStorageCapacity;
    private const int MaxStorageCapacity;
    private static readonly Vector3 TurretSwitchPosition;
    private readonly object False;
    private Configuration config;
    private MiniCopterDefaults copterDefaults;
    private bool lastRanAtNight;
    private int setupTimeHooksAttempts;
    private TOD_Sky time;
    private float sunrise;
    private float sunset;
    private float lastNightCheck;
    private float[] stageFuelPerSec;
    private float[] stageLiftFraction;
    private void Init();
    private void OnServerInitialized(bool init);
    private void Unload();
    private void OnEntitySpawned(Minicopter copter);
    private void OnEntityKill(BaseNetworkable entity);
    private void OnSwitchToggled(ElectricSwitch electricSwitch, BasePlayer player);
    private object OnTurretTarget(AutoTurret turret, BaseCombatEntity target);
    private object OnServerCommand(ConsoleSystem.Arg arg);
    private void OnEntityDismounted(BaseNetworkable entity, BasePlayer player);
    private void OnItemDeployed(Deployer deployer, StorageContainer container, BaseLock baseLock);
    private void OnMinicopterFuelPerSecChange(Plugin plugin, Minicopter copter, float[] fuelPerSec);
    private void OnMinicopterLiftFractionChange(Plugin plugin, Minicopter copter, float[] liftFraction);
    private static class ExposedHooks
    {
        public static object OnMiniCopterOptions(Minicopter copter);
        public static object OnMinicopterFuelPerSecChange(Plugin plugin, Minicopter copter, float[] fuelPerSec);
        public static object OnMinicopterLiftFractionChange(Plugin plugin, Minicopter copter, float[] liftFraction);
    }

    private bool IsNight();
    private void OnHour();
    private void SetupTimeHooks();
    private StorageContainer[] GetStorage(Minicopter copter);
    private void AddLargeStorageBox(Minicopter copter);
    private void AddRearStorageBox(Minicopter copter);
    private void AddSideStorageBoxes(Minicopter copter);
    private void AddStorageBox(Minicopter copter, string prefab, Vector3 position);
    private void SetupStorage(StorageContainer box);
    private void AddStorageBox(Minicopter copter, string prefab, Vector3 position, Quaternion rotation);
    private void SetupInvincibility(BaseCombatEntity entity);
    private void SetupTailLight(FlasherLight tailLight);
    private void AddTailLight(Minicopter copter);
    private void SetupSphereEntity(SphereEntity sphereEntity);
    private void SetupSearchLight(SearchLight searchLight);
    private void AddSearchLight(Minicopter copter);
    private void SetupAutoTurret(AutoTurret turret);
    private void AddTurret(Minicopter copter);
    private void SetupSwitch(ElectricSwitch electricSwitch);
    private void AddSwitch(AutoTurret turret);
    private void DestroyGroundComp(BaseEntity ent);
    private void DestroyMeshCollider(BaseEntity ent);
    private void RestoreMiniCopter(Minicopter copter, bool removeStorage);
    private void ModifyMiniCopter(Minicopter copter);
    private bool CanModifyMiniCopter(Minicopter copter);
    private bool CanModifyFuelPerSec(Minicopter copter, float proposedFuelPerSec);
    private bool CanModifyLiftFraction(Minicopter copter, float proposedLiftFraction);
    private void ScheduleModifyMiniCopter(Minicopter copter);
    private void StoreMiniCopterDefaults();
    private class MiniCopterDefaults
    {
        public float fuelPerSec;
        public float liftFraction;
        public Vector3 torqueScale;
    }

    private class Configuration : SerializableConfiguration
    {
        [JsonProperty("Fuel per Second")]
        public float fuelPerSec;
        [JsonProperty("Lift Fraction")]
        public float liftFraction;
        [JsonProperty("Pitch Torque Scale")]
        public float torqueScalePitch;
        [JsonProperty("Yaw Torque Scale")]
        public float torqueScaleYaw;
        [JsonProperty("Roll Torque Scale")]
        public float torqueScaleRoll;
        [JsonProperty("Storage Containers")]
        public int storageContainers;
        [JsonProperty("Large Storage Containers")]
        public int storageLargeContainers;
        [JsonProperty("Restore Defaults")]
        public bool restoreDefaults;
        [JsonProperty("Reload Storage")]
        public bool reloadStorage;
        [JsonProperty("Drop Storage Loot On Death")]
        public bool dropStorage;
        [JsonProperty("Large Storage Lockable")]
        public bool largeStorageLockable;
        [JsonProperty("Large Storage Size (Max 48)")]
        public int largeStorageSize;
        [JsonProperty("Large Storage Size (Max 42)")]
        public int largeStorageSizeDeprecated { get; set; }
        [JsonProperty("Seconds to pause flyhack when dismount from heli.")]
        public int flyHackPause;
        [JsonProperty("Add auto turret to heli")]
        public bool autoturret;
        [JsonProperty("Auto turret targets players")]
        public bool autoTurretTargetsPlayers;
        [JsonProperty("Auto turret targets NPCs")]
        public bool autoTurretTargetsNPCs;
        [JsonProperty("Auto turret targets animals")]
        public bool autoTurretTargetsAnimals;
        [JsonProperty("Mini Turret Range (Default 30)")]
        public float turretRange;
        [JsonProperty("Light: Add Searchlight to heli")]
        public bool addSearchLight;
        [JsonProperty("Light: Add Nightitme Tail Light")]
        public bool lightTail;
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

private static class ExposedHooks
{
    public static object OnMiniCopterOptions(Minicopter copter);
    public static object OnMinicopterFuelPerSecChange(Plugin plugin, Minicopter copter, float[] fuelPerSec);
    public static object OnMinicopterLiftFractionChange(Plugin plugin, Minicopter copter, float[] liftFraction);
}

private class MiniCopterDefaults
{
    public float fuelPerSec;
    public float liftFraction;
    public Vector3 torqueScale;
}

private class Configuration : SerializableConfiguration
{
    [JsonProperty("Fuel per Second")]
    public float fuelPerSec;
    [JsonProperty("Lift Fraction")]
    public float liftFraction;
    [JsonProperty("Pitch Torque Scale")]
    public float torqueScalePitch;
    [JsonProperty("Yaw Torque Scale")]
    public float torqueScaleYaw;
    [JsonProperty("Roll Torque Scale")]
    public float torqueScaleRoll;
    [JsonProperty("Storage Containers")]
    public int storageContainers;
    [JsonProperty("Large Storage Containers")]
    public int storageLargeContainers;
    [JsonProperty("Restore Defaults")]
    public bool restoreDefaults;
    [JsonProperty("Reload Storage")]
    public bool reloadStorage;
    [JsonProperty("Drop Storage Loot On Death")]
    public bool dropStorage;
    [JsonProperty("Large Storage Lockable")]
    public bool largeStorageLockable;
    [JsonProperty("Large Storage Size (Max 48)")]
    public int largeStorageSize;
    [JsonProperty("Large Storage Size (Max 42)")]
    public int largeStorageSizeDeprecated { get; set; }
    [JsonProperty("Seconds to pause flyhack when dismount from heli.")]
    public int flyHackPause;
    [JsonProperty("Add auto turret to heli")]
    public bool autoturret;
    [JsonProperty("Auto turret targets players")]
    public bool autoTurretTargetsPlayers;
    [JsonProperty("Auto turret targets NPCs")]
    public bool autoTurretTargetsNPCs;
    [JsonProperty("Auto turret targets animals")]
    public bool autoTurretTargetsAnimals;
    [JsonProperty("Mini Turret Range (Default 30)")]
    public float turretRange;
    [JsonProperty("Light: Add Searchlight to heli")]
    public bool addSearchLight;
    [JsonProperty("Light: Add Nightitme Tail Light")]
    public bool lightTail;
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

## MinicopterSeating

```csharp
using System.Collections.Generic;
using UnityEngine;

Oxide.Plugins
[Info("Minicopter Seating", "Bazz3l", "1.1.6")]
[Description("Spawn extra seats on each side of the minicopter.")]
public class MinicopterSeating : CovalencePlugin
{
    private readonly GameObjectRef _gameObjectRef;
    private readonly Vector3 _seat1;
    private readonly Vector3 _seat2;
    private void OnEntitySpawned(BaseVehicle mini);
    private void SetupSeating(BaseVehicle vehicle);
    private BaseVehicle.MountPointInfo CreateMount(List<BaseVehicle.MountPointInfo> mountPoints, Vector3 position);
}


```

---

## MiningBow

```csharp
using System;
using System.Collections.Generic;
using Newtonsoft.Json;

Oxide.Plugins
[Info("MiningBow", "Sempai#3239", "1.0.1")]
public class MiningBow : RustPlugin
{
    private PluginConfig cfg;
    public class PluginConfig
    {
        [JsonProperty("Настройки лука")]
        public bowsettings Bow;
        [JsonProperty("Настройки дропа с лука")]
        public drop Drop;
    }

    public class bowdrop
    {
        [JsonProperty("Включить спавн лука в ящиках?")]
        public bool enabledropfromcrates;
        [JsonProperty("Shortprefabname ящика, шанс спавна")]
        public Dictionary<string, float> drop;
    }

    public class bowsettings
    {
        [JsonProperty("SkinID лука")]
        public ulong skinid;
        [JsonProperty("Название лука")]
        public string name;
        [JsonProperty("Настройки спавна лука")]
        public bowdrop drop;
    }

    public class drop
    {
        [JsonProperty("Shortprefabname предмета на который будет действовать лук")]
        public Dictionary<string, List<dropsettings>> bowdrop;
    }

    public class dropsettings
    {
        [JsonProperty("Shortname создаваемого предмета после разрушения обьекта")]
        public string shortname;
        [JsonProperty("SkinID создаваемого предмета после разрушения обьекта")]
        public ulong skinid;
        [JsonProperty("Минимальное количество предмета для создания")]
        public int minamount;
        [JsonProperty("Максимальное количество предмета для создания")]
        public int maxamount;
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
     void OnLootSpawn(LootContainer container);
     void OnPlayerAttack(BasePlayer attacker, HitInfo info);
    [ConsoleCommand("GiveMiningBow")]
    private void GiveMiningBow(ConsoleSystem.Arg arg);
}

public class PluginConfig
{
    [JsonProperty("Настройки лука")]
    public bowsettings Bow;
    [JsonProperty("Настройки дропа с лука")]
    public drop Drop;
}

public class bowdrop
{
    [JsonProperty("Включить спавн лука в ящиках?")]
    public bool enabledropfromcrates;
    [JsonProperty("Shortprefabname ящика, шанс спавна")]
    public Dictionary<string, float> drop;
}

public class bowsettings
{
    [JsonProperty("SkinID лука")]
    public ulong skinid;
    [JsonProperty("Название лука")]
    public string name;
    [JsonProperty("Настройки спавна лука")]
    public bowdrop drop;
}

public class drop
{
    [JsonProperty("Shortprefabname предмета на который будет действовать лук")]
    public Dictionary<string, List<dropsettings>> bowdrop;
}

public class dropsettings
{
    [JsonProperty("Shortname создаваемого предмета после разрушения обьекта")]
    public string shortname;
    [JsonProperty("SkinID создаваемого предмета после разрушения обьекта")]
    public ulong skinid;
    [JsonProperty("Минимальное количество предмета для создания")]
    public int minamount;
    [JsonProperty("Максимальное количество предмета для создания")]
    public int maxamount;
}


```

---

## MiniPanel

```csharp
using Newtonsoft.Json;
using Oxide.Game.Rust.Cui;
using System;
using System.Collections.Generic;
using System.Drawing;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using UnityEngine;

Oxide.Plugins
[Info("Дополнительное GUI", "BadMandarin", "1.0.1")]
[Description("Дополнительное GUI")]
 class MiniPanel : RustPlugin
{
    private class PluginConfig
    {
        [JsonProperty("Гл.Анчор панельки")]
        public string PanelAnchor;
        [JsonProperty("Гл.Офсет панельки (Min)")]
        public string PanelOffsetMin;
        [JsonProperty("Гл.Офсет панельки (Max)")]
        public string PanelOffsetMax;
        [JsonProperty("Гл.Текст панельки")]
        public string PanelText;
        [JsonProperty("Гл.Цвет текста")]
        public string PanelColor;
        [JsonProperty("Гл.Прозрачность текста")]
        public float PanelAlpha;
        [JsonProperty("Гл.Команда")]
        public string PanelCmd;
        [JsonProperty("Стрелочка (Цвет)")]
        public string ArrowColor;
        [JsonProperty("Стрелочка включена? (1 - да, 0 - нет)")]
        public bool ArrowMode;
        [JsonProperty("Больше панелей")]
        public List<AddtionalPanel> _listPanels;
    }

    private class AddtionalPanel
    {
        [JsonProperty("Анчор панельки")]
        public string PanelAnchor;
        [JsonProperty("Офсет панельки (Min)")]
        public string PanelOffsetMin;
        [JsonProperty("Офсет панельки (Max)")]
        public string PanelOffsetMax;
        [JsonProperty("Картинка")]
        public string PanelImageUrl;
        [JsonProperty("Команда")]
        public string PanelCmd;
    }

    private string UI_Layer;
    private PluginConfig config;
    private Dictionary<ulong, bool> _playerInfo;
    private void Init();
     void Unload();
    protected override void LoadDefaultConfig();
     void OnServerInitialized();
     void OnPlayerSleepEnded(BasePlayer player);
    private void Draw_UIMain(BasePlayer player, string MainPosition);
    [ConsoleCommand("UI_TogglePanel")]
    private void CMD_UI_TogglePanel(ConsoleSystem.Arg arg);
    private PluginConfig GetDefaultConfig();
    public static string GetColor(string hex, float alpha);
}

private class PluginConfig
{
    [JsonProperty("Гл.Анчор панельки")]
    public string PanelAnchor;
    [JsonProperty("Гл.Офсет панельки (Min)")]
    public string PanelOffsetMin;
    [JsonProperty("Гл.Офсет панельки (Max)")]
    public string PanelOffsetMax;
    [JsonProperty("Гл.Текст панельки")]
    public string PanelText;
    [JsonProperty("Гл.Цвет текста")]
    public string PanelColor;
    [JsonProperty("Гл.Прозрачность текста")]
    public float PanelAlpha;
    [JsonProperty("Гл.Команда")]
    public string PanelCmd;
    [JsonProperty("Стрелочка (Цвет)")]
    public string ArrowColor;
    [JsonProperty("Стрелочка включена? (1 - да, 0 - нет)")]
    public bool ArrowMode;
    [JsonProperty("Больше панелей")]
    public List<AddtionalPanel> _listPanels;
}

private class AddtionalPanel
{
    [JsonProperty("Анчор панельки")]
    public string PanelAnchor;
    [JsonProperty("Офсет панельки (Min)")]
    public string PanelOffsetMin;
    [JsonProperty("Офсет панельки (Max)")]
    public string PanelOffsetMax;
    [JsonProperty("Картинка")]
    public string PanelImageUrl;
    [JsonProperty("Команда")]
    public string PanelCmd;
}


```

---

## Minstrel

```csharp
using Oxide.Core;
using Oxide.Core.Plugins;
using System;
using System.Collections.Generic;
using System.Reflection;
using System.Linq;
using UnityEngine;
using System.Text;

Oxide.Plugins
[Info("Minstrel", "4seti [Lunatiq] for Rust Planet", "0.0.7", ResourceId = 981)]
public class Minstrel : RustPlugin
{
    private void Log(string message);
    private void Warn(string message);
    private void Error(string message);
    private T GetConfig(string name, T defaultValue);
     void Loaded();
    private static FieldInfo serverinput;
     Dictionary<string, List<TuneNote>> tuneDict;
     Dictionary<ulong, List<TuneNote>> tuneRecTemp;
     List<string> cfgTunes;
    protected override void LoadDefaultConfig();
     void LoadVariables();
     void Init();
     void Unload();
    private static void DestroyAll();
    [ChatCommand("ms")]
     void cmdToggleMS(BasePlayer player, string cmd, string[] args);
    private bool LoadTune(string tuneName, List<TuneNote> list);
     void SaveTune(string playerName, string tuneName, ulong userID);
    private string BuildNoteList(float adjust, float start);
    [ChatCommand("ms_tune")]
     void cmdTune(BasePlayer player, string cmd, string[] args);
    [ChatCommand("ms_rec")]
     void cmdRec(BasePlayer player, string cmd, string[] args);
     void playTune(BasePlayer player, string tuneName);
    private bool checkComponent(BasePlayer player);
     List<object> getTune(string tuneName);
    [ChatCommand("ms_adj")]
     void cmdMSAdjust(BasePlayer player, string cmd, string[] args);
    [ChatCommand("ms_list")]
     void cmdMSList(BasePlayer player, string cmd, string[] args);
    [ChatCommand("ms_rel")]
     void cmdMSReload(BasePlayer player, string cmd, string[] args);
    [ChatCommand("ms_del")]
     void cmdMSDelete(BasePlayer player, string cmd, string[] args);
    [ChatCommand("msp")]
     void cmdMSPluck(BasePlayer player, string cmd, string[] args);
    public class KeyboardGuitar : MonoBehaviour
    {
        private static float noteTime;
        public float adjust;
        public float start;
        public float NextTimeToPress;
        public float noteToPlay;
        private InputState input;
        public bool Recording;
        public BasePlayer owner;
        public List<TuneNote> recTune;
        public float nextNoteTime;
         Effect effectP;
         Effect effectS;
         void Awake();
         void FixedUpdate();
        public void PlayNote(float scale);
        public List<TuneNote> curTune;
        private Stack<TuneNote> tuneToPlay;
        public bool playingTune;
        private TuneNote nextNote;
        public void PlayTune();
        private void PlayTuneStack();
    }

    public class TuneNote
    {
        public float NoteScale;
        public float Delay;
        public bool Pluck;
        public TuneNote(float note, float delay, bool pluck);
    }

}

public class KeyboardGuitar : MonoBehaviour
{
    private static float noteTime;
    public float adjust;
    public float start;
    public float NextTimeToPress;
    public float noteToPlay;
    private InputState input;
    public bool Recording;
    public BasePlayer owner;
    public List<TuneNote> recTune;
    public float nextNoteTime;
     Effect effectP;
     Effect effectS;
     void Awake();
     void FixedUpdate();
    public void PlayNote(float scale);
    public List<TuneNote> curTune;
    private Stack<TuneNote> tuneToPlay;
    public bool playingTune;
    private TuneNote nextNote;
    public void PlayTune();
    private void PlayTuneStack();
}

public class TuneNote
{
    public float NoteScale;
    public float Delay;
    public bool Pluck;
    public TuneNote(float note, float delay, bool pluck);
}


```

---

## MLRSDamage

```csharp
using System;
using System.Collections.Generic;
using Newtonsoft.Json;
using System.Linq;
using Oxide.Core.Plugins;
using Oxide.Core;
using UnityEngine;

Oxide.Plugins
[Info("MLRS Damage", "iLakSkiL", "1.5.1")]
[Description("Edits the damage down by the MLRS.")]
public class MLRSDamage : RustPlugin
{
     int rocketsFired;
    private Configuration _config;
    public class Configuration
    {
        [JsonProperty(PropertyName = "MLRS Settings")]
        public DefSettings defsettings;
        public class DefSettings
        {
            [JsonProperty(PropertyName = "MLRS Damage Modifier")]
            public double damageMod;
            [JsonProperty(PropertyName = "Allow Damage to Player Built Bases")]
            public bool pvBase;
            [JsonProperty(PropertyName = "Allow Damage to Players")]
            public bool pvPlayer;
            [JsonProperty(PropertyName = "Allow Damage to Raidable and Abandoned Bases")]
            public bool raidable;
            [JsonProperty(PropertyName = "Allow Damage to NPCs")]
            public bool npc;
            [JsonProperty(PropertyName = "MLRS Cooldown time (in minutes)")]
            public double broken;
            [JsonProperty(PropertyName = "Total Rockets for MLRS to fire")]
            public int rocketAmount;
            [JsonProperty(PropertyName = "Rocket Launch Interval (in seconds)")]
            public float launchTime;
            [JsonProperty(PropertyName = "Requires Aiming Module")]
            public bool needModule;
        }

    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private void Unload();
    private void Loaded();
    private void OnEntitySpawned(BaseEntity entity);
    private object OnEntityTakeDamage(BaseCombatEntity entity, HitInfo info);
    private void OnMlrsRocketFired(MLRS ent, ServerProjectile serverProjectile);
    private object OnMlrsFire(MLRS ent, BasePlayer owner);
    private void OnMlrsFiringEnded(MLRS entity);
    public void SetRocketAmount(int amount);
    private void UpdateMLRSContainers(int amount);
    private void UpdateMLRSContainer(MLRS entity, int amount);
    private void ResetModule(MLRS entity);
    [ConsoleCommand("mlrsdamage.damage")]
    private void Damage(ConsoleSystem.Arg arg);
    [ConsoleCommand("mlrsdamage.cooldown")]
    private void Cooldown(ConsoleSystem.Arg arg);
    [ConsoleCommand("mlrsdamage.pvp")]
    private void PvpEnable(ConsoleSystem.Arg arg);
    [ConsoleCommand("mlrsdamage.pvpbase")]
    private void PvpBaseEnable(ConsoleSystem.Arg arg);
    [ConsoleCommand("mlrsdamage.raidable")]
    private void RaidableEnable(ConsoleSystem.Arg arg);
    [ConsoleCommand("mlrsdamage.npc")]
    private void NpcEnable(ConsoleSystem.Arg arg);
    [ConsoleCommand("mlrsdamage.rockets")]
    private void RocketsNum(ConsoleSystem.Arg arg);
    [ConsoleCommand("mlrsdamage.module")]
    private void NeedModule(ConsoleSystem.Arg arg);
    [ConsoleCommand("mlrsdamage.interval")]
    private void LaunchInterval(ConsoleSystem.Arg arg);
}

public class Configuration
{
    [JsonProperty(PropertyName = "MLRS Settings")]
    public DefSettings defsettings;
    public class DefSettings
    {
        [JsonProperty(PropertyName = "MLRS Damage Modifier")]
        public double damageMod;
        [JsonProperty(PropertyName = "Allow Damage to Player Built Bases")]
        public bool pvBase;
        [JsonProperty(PropertyName = "Allow Damage to Players")]
        public bool pvPlayer;
        [JsonProperty(PropertyName = "Allow Damage to Raidable and Abandoned Bases")]
        public bool raidable;
        [JsonProperty(PropertyName = "Allow Damage to NPCs")]
        public bool npc;
        [JsonProperty(PropertyName = "MLRS Cooldown time (in minutes)")]
        public double broken;
        [JsonProperty(PropertyName = "Total Rockets for MLRS to fire")]
        public int rocketAmount;
        [JsonProperty(PropertyName = "Rocket Launch Interval (in seconds)")]
        public float launchTime;
        [JsonProperty(PropertyName = "Requires Aiming Module")]
        public bool needModule;
    }

}

public class DefSettings
{
    [JsonProperty(PropertyName = "MLRS Damage Modifier")]
    public double damageMod;
    [JsonProperty(PropertyName = "Allow Damage to Player Built Bases")]
    public bool pvBase;
    [JsonProperty(PropertyName = "Allow Damage to Players")]
    public bool pvPlayer;
    [JsonProperty(PropertyName = "Allow Damage to Raidable and Abandoned Bases")]
    public bool raidable;
    [JsonProperty(PropertyName = "Allow Damage to NPCs")]
    public bool npc;
    [JsonProperty(PropertyName = "MLRS Cooldown time (in minutes)")]
    public double broken;
    [JsonProperty(PropertyName = "Total Rockets for MLRS to fire")]
    public int rocketAmount;
    [JsonProperty(PropertyName = "Rocket Launch Interval (in seconds)")]
    public float launchTime;
    [JsonProperty(PropertyName = "Requires Aiming Module")]
    public bool needModule;
}


```

---

## MoegicBox

```csharp
using System.Collections.Generic;
using System.Linq;
using System.Text;
using Oxide.Core;
using System;
using Oxide.Core.Plugins;
using MoegicBoxExtensions;

MoegicBoxExtensions
static class MoegicExtensions
{
    public static string MoegicId(BaseEntity entity);
}

Oxide.Plugins
[Info("MoegicBox", "Deicide666ra", "1.0.3")]
 class MoegicBox : RustPlugin
{
    public class MoegicConfig
    {
        public Dictionary<string, TradeList> TradeLists;
        public Dictionary<string, string> BoxLinks;
        public float RefundRatio;
        public Dictionary<string, int> RecyclerPrice;
        public void Save();
    }

     MoegicConfig g_config;
    private Dictionary<ulong, string> g_playerCommands;
     Dictionary<string, ItemDefinition> g_itemDefinitions;
     Dictionary<string, ItemBlueprint> g_blueprintDefinitions;
     void CreateExampleSalesLists();
     void Loaded();
    public class TradeOffer
    {
        public TradeOffer();
        public TradeOffer(Product price, Product [] reward);
        public Product Price { get; set; }
        public Product [] Reward { get; set; }
        public override string ToString();
    }

    public class Product
    {
        public Product();
        public Product(string displayName, int amount);
        public string DisplayName { get; set; }
        public int Amount { get; set; }
        public override string ToString();
    }

    public class TradeList
    {
        public TradeList();
        public TradeList(string name);
        public List<TradeOffer> Offers { get; set; }
        public string Listname { get; set; }
        public override string ToString();
    }

    public class TradeBox
    {
        public TradeBox();
         int BoxEntityId { get; set; }
         ulong OwnerId { get; set; }
         string TradeListName { get; set; }
    }

     bool IsMoethorized(BasePlayer player);
     void OnServerInitialized();
    [HookMethod("SendHelpText")]
    private void SendHelpText(BasePlayer player);
    [ChatCommand("moe")]
     void cmdMoe(BasePlayer player, string cmd, string[] args);
    [ChatCommand("mlists")]
     void cmdTradelists(BasePlayer player, string cmd, string[] args);
     string FormatError(string message);
    [ChatCommand("mshow")]
     void cmdShowlist(BasePlayer player, string cmd, string[] args);
    [ChatCommand("mrec")]
     void cmdRecycleBox(BasePlayer player, string cmd, string[] args);
    [ChatCommand("mlink")]
     void cmdLinkBox(BasePlayer player, string cmd, string[] args);
    [ChatCommand("mulink")]
     void cmdUnlinkBox(BasePlayer player, string cmd, string[] args);
     void OnPlayerLoot(PlayerLoot lootInventory, object lootable);
     void RunCommand(BasePlayer player, string command, BaseEntity box);
    private string GetActiveCommand(BasePlayer player);
    private void SetActiveCommand(BasePlayer player, string command);
    private void ClearActiveCommand(BasePlayer player);
    private TradeList GetList(string listName);
    private string GetActiveList(BaseEntity box);
    private bool SetActiveList(BaseEntity box, string list);
    private static BasePlayer GetPlayerFromContainer(ItemContainer container, Item item);
     void OnItemAddedToContainer(ItemContainer container, Item item);
     bool CheckPlayerInventoryForItems(BasePlayer player, string displayName, int amount);
     int RemoveItemsFromInventory(BasePlayer player, string displayName, int amount);
     void SalvageItem(BasePlayer player, Item item);
     string GetPriceString();
     bool HasPrice(BasePlayer player);
}

public class MoegicConfig
{
    public Dictionary<string, TradeList> TradeLists;
    public Dictionary<string, string> BoxLinks;
    public float RefundRatio;
    public Dictionary<string, int> RecyclerPrice;
    public void Save();
}

public class TradeOffer
{
    public TradeOffer();
    public TradeOffer(Product price, Product [] reward);
    public Product Price { get; set; }
    public Product [] Reward { get; set; }
    public override string ToString();
}

public class Product
{
    public Product();
    public Product(string displayName, int amount);
    public string DisplayName { get; set; }
    public int Amount { get; set; }
    public override string ToString();
}

public class TradeList
{
    public TradeList();
    public TradeList(string name);
    public List<TradeOffer> Offers { get; set; }
    public string Listname { get; set; }
    public override string ToString();
}

public class TradeBox
{
    public TradeBox();
     int BoxEntityId { get; set; }
     ulong OwnerId { get; set; }
     string TradeListName { get; set; }
}


```

---

## MonumentFinder

```csharp
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;
using Oxide.Core;
using Oxide.Core.Libraries.Covalence;
using System;
using System.Collections.Generic;
using System.Globalization;
using System.Linq;
using System.Reflection;
using System.Text;
using UnityEngine;

Oxide.Plugins
[Info("Monument Finder", "WhiteThunder", "3.1.2")]
[Description("Find monuments with commands or API.")]
internal class MonumentFinder : CovalencePlugin
{
    private static MonumentFinder _pluginInstance;
    private static Configuration _pluginConfig;
    private const string PermissionFind;
    private const float DrawDuration;
    private readonly FieldInfo DungeonBaseLinksFieldInfo;
    private Dictionary<MonumentInfo, NormalMonumentAdapter> _normalMonuments;
    private Dictionary<DungeonGridCell, TrainTunnelAdapter> _trainTunnels;
    private Dictionary<DungeonBaseLink, UnderwaterLabLinkAdapter> _labModules;
    private Dictionary<MonoBehaviour, BaseMonumentAdapter> _allMonuments;
    private Collider[] _colliderBuffer;
    private void Init();
    private void Unload();
    private void OnServerInitialized();
    private Dictionary<string, object> API_GetClosest(Vector3 position);
    private Dictionary<string, object> API_GetClosestMonument(Vector3 position);
    private Dictionary<string, object> API_GetClosestTrainTunnel(Vector3 position);
    private Dictionary<string, object> API_GetClosestUnderwaterLabModules(Vector3 position);
    private List<Dictionary<string, object>> API_FindMonuments(string filter);
    private List<Dictionary<string, object>> API_FindTrainTunnels(string filter);
    private List<Dictionary<string, object>> API_FindUnderwaterLabModules(string filter);
    private List<Dictionary<string, object>> API_Find(string filter);
    private List<Dictionary<string, object>> API_FindByShortName(string shortName);
    private List<Dictionary<string, object>> API_FindByAlias(string alias);
    private List<MonumentInfo> FindMonuments(string filter);
    private void CommandFind(IPlayer player, string command, string[] args);
    private void SubcommandHelp(IPlayer player, string command);
    private void SubcommandList(IPlayer player, string cmd, string[] args);
    private void SubcommandShow(IPlayer player, string command, string[] args);
    private void SubcommandClosest(IPlayer player, string command, string[] args);
    private static void LogError(string message);
    private static void LogWarning(string message);
    private static bool IsCustomMonument(MonumentInfo monumentInfo);
    private static Collider FindPreventBuildingVolume(Vector3 position);
    private static T GetClosestMonument(IEnumerable<T> monumentList, Vector3 position);
    private static Dictionary<string, object> GetClosestMonumentForAPI(IEnumerable<BaseMonumentAdapter> monumentList, Vector3 position);
    private static List<T> FilterMonuments(IEnumerable<T> monumentList, string filter, string shortName, string alias);
    private static List<Dictionary<string, object>> FilterMonumentsForAPI(IEnumerable<BaseMonumentAdapter> monumentList, string filter, string shortName, string alias);
    private void PrintMonumentList(IPlayer player, IEnumerable<BaseMonumentAdapter> monuments);
    private static void ShowMonumentName(BasePlayer player, BaseMonumentAdapter monument);
    private abstract class BaseMonumentAdapter
    {
        protected static string GetShortName(string prefabName);
        public MonoBehaviour Object { get; set; }
        public string PrefabName { get; set; }
        public string ShortName { get; set; }
        public string Alias { get; set; }
        public Vector3 Position { get; set; }
        public Quaternion Rotation { get; set; }
        public BaseMonumentAdapter(MonoBehaviour behavior);
        public Vector3 TransformPoint(Vector3 localPosition);
        public Vector3 InverseTransformPoint(Vector3 worldPosition);
        public abstract bool IsInBounds(Vector3 position);
        public abstract Vector3 ClosestPointOnBounds(Vector3 position);
        public virtual bool MatchesFilter(string filter, string shortName, string alias);
        private Dictionary<string, object> _cachedAPIResult;
        public Dictionary<string, object> APIResult { get; set; }
    }

    private class NormalMonumentAdapter : BaseMonumentAdapter, SingleBoundingBox
    {
        public static Dictionary<string, Bounds> MonumentBounds;
        public MonumentInfo MonumentInfo { get; set; }
        public OBB BoundingBox { get; set; }
        public NormalMonumentAdapter(MonumentInfo monumentInfo);
        public override bool IsInBounds(Vector3 position);
        public override Vector3 ClosestPointOnBounds(Vector3 position);
        public override bool MatchesFilter(string filter, string shortName, string alias);
    }

    private class TrainTunnelAdapter : BaseMonumentAdapter, SingleBoundingBox
    {
        public static readonly string[] IgnoredPrefabs;
        private abstract class BaseTunnelInfo
        {
            public Quaternion Rotation;
            public virtual Bounds Bounds { get; set; }
            public virtual string Alias { get; set; }
        }

        private class TrainStation : BaseTunnelInfo
        {
            public override Bounds Bounds { get; set; }
            public override string Alias { get; set; }
        }

        private class BarricadeTunnel : BaseTunnelInfo
        {
            public override Bounds Bounds { get; set; }
            public override string Alias { get; set; }
        }

        private class LootTunnel : BaseTunnelInfo
        {
            public override Bounds Bounds { get; set; }
            public override string Alias { get; set; }
        }

        private class SplitTunnel : BaseTunnelInfo
        {
            public override Bounds Bounds { get; set; }
            public override string Alias { get; set; }
        }

        private class Intersection : BaseTunnelInfo
        {
            public override Bounds Bounds { get; set; }
            public override string Alias { get; set; }
        }

        private class LargeIntersection : BaseTunnelInfo
        {
            public override Bounds Bounds { get; set; }
            public override string Alias { get; set; }
        }

        private class VerticalIntersection : BaseTunnelInfo
        {
            public override Bounds Bounds { get; set; }
            public override string Alias { get; set; }
        }

        private class CornerTunnel : BaseTunnelInfo
        {
            public override Bounds Bounds { get; set; }
            public override string Alias { get; set; }
        }

        private static readonly Dictionary<string, BaseTunnelInfo> PrefabToTunnelInfo;
        private static BaseTunnelInfo GetTunnelInfo(string shortName);
        public OBB BoundingBox { get; set; }
        private BaseTunnelInfo _tunnelInfo;
        public TrainTunnelAdapter(DungeonGridCell dungeonCell);
        public override bool IsInBounds(Vector3 position);
        public override Vector3 ClosestPointOnBounds(Vector3 position);
    }

    private class UnderwaterLabLinkAdapter : BaseMonumentAdapter, MultipleBoundingBoxes
    {
        public OBB[] BoundingBoxes { get; set; }
        public UnderwaterLabLinkAdapter(DungeonBaseLink dungeonLink);
        public override bool IsInBounds(Vector3 position);
        public override Vector3 ClosestPointOnBounds(Vector3 position);
    }

    private static class Ddraw
    {
        public static void Sphere(BasePlayer player, Vector3 origin, float radius, Color color, float duration);
        public static void Line(BasePlayer player, Vector3 origin, Vector3 target, Color color, float duration);
        public static void Text(BasePlayer player, Vector3 origin, string text, Color color, float duration);
        public static void Segments(BasePlayer player, Vector3 origin, Vector3 target, Color color, float duration);
        public static void Box(BasePlayer player, Vector3 center, Quaternion rotation, Vector3 halfExtents, Color color, float duration, bool showInfo);
        public static void Box(BasePlayer player, OBB boundingBox, Color color, float duration, bool showInfo);
    }

    private class CustomBounds
    {
        [JsonProperty("Size")]
        public Vector3 Size;
        [JsonProperty("Center adjustment")]
        public Vector3 CenterOffset;
        [JsonProperty("Center")]
        private Vector3 DeprecatedCenter { get; set; }
        public Bounds ToBounds();
        public CustomBounds Copy();
    }

    private class BaseDetectionSettings
    {
        [JsonProperty("Auto determine from monument marker", Order = -3)]
        public bool UseMonumentMarker;
        [JsonProperty("Auto determine from prevent building volume", Order = -2)]
        public bool UsePreventBuildingVolume;
        public BaseDetectionSettings Copy();
    }

    private class BoundSettings : BaseDetectionSettings
    {
        [JsonProperty("Use custom bounds")]
        public bool UseCustomBounds;
        [JsonProperty("Custom bounds")]
        public CustomBounds CustomBounds;
        public new BoundSettings Copy();
    }

    private class MonumentSettings
    {
        [JsonProperty("Position")]
        public BaseDetectionSettings Position;
        [JsonProperty("Rotation")]
        public BaseDetectionSettings Rotation;
        [JsonProperty("Bounds")]
        public BoundSettings Bounds;
        public MonumentSettings Copy();
    }

    private class Configuration : SerializableConfiguration
    {
        [JsonProperty(PropertyName = "Command")]
        public string Command;
        [JsonProperty("Default custom monument settings")]
        public MonumentSettings DefaultCustomMonumentSettings;
        [JsonProperty("Monuments", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        private Dictionary<string, MonumentSettings> MonumentSettingsMap;
        [JsonProperty("OverrideMonumentBounds")]
        private Dictionary<string, CustomBounds> DeprecatedOverrideMonumentBounds { get; set; }
        public MonumentSettings GetMonumentSettings(string monumentName);
        public bool AddMonument(string aliasOrShortName, BaseMonumentAdapter monument);
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
    private void ReplyToPlayer(IPlayer player, string messageName, object[] args);
    private static class Lang
    {
        public const string ErrorNoPermission;
        public const string NoMonumentsFound;
        public const string ListHeader;
        public const string AtMonument;
        public const string ClosestMonument;
        public const string ClosestConfigSuccess;
        public const string ClosestConfigAlreadyPresent;
        public const string HelpHeader;
        public const string HelpList;
        public const string HelpShow;
        public const string HelpClosest;
        public const string HelpClosestConfig;
    }

    protected override void LoadDefaultMessages();
}

private abstract class BaseMonumentAdapter
{
    protected static string GetShortName(string prefabName);
    public MonoBehaviour Object { get; set; }
    public string PrefabName { get; set; }
    public string ShortName { get; set; }
    public string Alias { get; set; }
    public Vector3 Position { get; set; }
    public Quaternion Rotation { get; set; }
    public BaseMonumentAdapter(MonoBehaviour behavior);
    public Vector3 TransformPoint(Vector3 localPosition);
    public Vector3 InverseTransformPoint(Vector3 worldPosition);
    public abstract bool IsInBounds(Vector3 position);
    public abstract Vector3 ClosestPointOnBounds(Vector3 position);
    public virtual bool MatchesFilter(string filter, string shortName, string alias);
    private Dictionary<string, object> _cachedAPIResult;
    public Dictionary<string, object> APIResult { get; set; }
}

private class NormalMonumentAdapter : BaseMonumentAdapter, SingleBoundingBox
{
    public static Dictionary<string, Bounds> MonumentBounds;
    public MonumentInfo MonumentInfo { get; set; }
    public OBB BoundingBox { get; set; }
    public NormalMonumentAdapter(MonumentInfo monumentInfo);
    public override bool IsInBounds(Vector3 position);
    public override Vector3 ClosestPointOnBounds(Vector3 position);
    public override bool MatchesFilter(string filter, string shortName, string alias);
}

private class TrainTunnelAdapter : BaseMonumentAdapter, SingleBoundingBox
{
    public static readonly string[] IgnoredPrefabs;
    private abstract class BaseTunnelInfo
    {
        public Quaternion Rotation;
        public virtual Bounds Bounds { get; set; }
        public virtual string Alias { get; set; }
    }

    private class TrainStation : BaseTunnelInfo
    {
        public override Bounds Bounds { get; set; }
        public override string Alias { get; set; }
    }

    private class BarricadeTunnel : BaseTunnelInfo
    {
        public override Bounds Bounds { get; set; }
        public override string Alias { get; set; }
    }

    private class LootTunnel : BaseTunnelInfo
    {
        public override Bounds Bounds { get; set; }
        public override string Alias { get; set; }
    }

    private class SplitTunnel : BaseTunnelInfo
    {
        public override Bounds Bounds { get; set; }
        public override string Alias { get; set; }
    }

    private class Intersection : BaseTunnelInfo
    {
        public override Bounds Bounds { get; set; }
        public override string Alias { get; set; }
    }

    private class LargeIntersection : BaseTunnelInfo
    {
        public override Bounds Bounds { get; set; }
        public override string Alias { get; set; }
    }

    private class VerticalIntersection : BaseTunnelInfo
    {
        public override Bounds Bounds { get; set; }
        public override string Alias { get; set; }
    }

    private class CornerTunnel : BaseTunnelInfo
    {
        public override Bounds Bounds { get; set; }
        public override string Alias { get; set; }
    }

    private static readonly Dictionary<string, BaseTunnelInfo> PrefabToTunnelInfo;
    private static BaseTunnelInfo GetTunnelInfo(string shortName);
    public OBB BoundingBox { get; set; }
    private BaseTunnelInfo _tunnelInfo;
    public TrainTunnelAdapter(DungeonGridCell dungeonCell);
    public override bool IsInBounds(Vector3 position);
    public override Vector3 ClosestPointOnBounds(Vector3 position);
}

private abstract class BaseTunnelInfo
{
    public Quaternion Rotation;
    public virtual Bounds Bounds { get; set; }
    public virtual string Alias { get; set; }
}

private class TrainStation : BaseTunnelInfo
{
    public override Bounds Bounds { get; set; }
    public override string Alias { get; set; }
}

private class BarricadeTunnel : BaseTunnelInfo
{
    public override Bounds Bounds { get; set; }
    public override string Alias { get; set; }
}

private class LootTunnel : BaseTunnelInfo
{
    public override Bounds Bounds { get; set; }
    public override string Alias { get; set; }
}

private class SplitTunnel : BaseTunnelInfo
{
    public override Bounds Bounds { get; set; }
    public override string Alias { get; set; }
}

private class Intersection : BaseTunnelInfo
{
    public override Bounds Bounds { get; set; }
    public override string Alias { get; set; }
}

private class LargeIntersection : BaseTunnelInfo
{
    public override Bounds Bounds { get; set; }
    public override string Alias { get; set; }
}

private class VerticalIntersection : BaseTunnelInfo
{
    public override Bounds Bounds { get; set; }
    public override string Alias { get; set; }
}

private class CornerTunnel : BaseTunnelInfo
{
    public override Bounds Bounds { get; set; }
    public override string Alias { get; set; }
}

private class UnderwaterLabLinkAdapter : BaseMonumentAdapter, MultipleBoundingBoxes
{
    public OBB[] BoundingBoxes { get; set; }
    public UnderwaterLabLinkAdapter(DungeonBaseLink dungeonLink);
    public override bool IsInBounds(Vector3 position);
    public override Vector3 ClosestPointOnBounds(Vector3 position);
}

private static class Ddraw
{
    public static void Sphere(BasePlayer player, Vector3 origin, float radius, Color color, float duration);
    public static void Line(BasePlayer player, Vector3 origin, Vector3 target, Color color, float duration);
    public static void Text(BasePlayer player, Vector3 origin, string text, Color color, float duration);
    public static void Segments(BasePlayer player, Vector3 origin, Vector3 target, Color color, float duration);
    public static void Box(BasePlayer player, Vector3 center, Quaternion rotation, Vector3 halfExtents, Color color, float duration, bool showInfo);
    public static void Box(BasePlayer player, OBB boundingBox, Color color, float duration, bool showInfo);
}

private class CustomBounds
{
    [JsonProperty("Size")]
    public Vector3 Size;
    [JsonProperty("Center adjustment")]
    public Vector3 CenterOffset;
    [JsonProperty("Center")]
    private Vector3 DeprecatedCenter { get; set; }
    public Bounds ToBounds();
    public CustomBounds Copy();
}

private class BaseDetectionSettings
{
    [JsonProperty("Auto determine from monument marker", Order = -3)]
    public bool UseMonumentMarker;
    [JsonProperty("Auto determine from prevent building volume", Order = -2)]
    public bool UsePreventBuildingVolume;
    public BaseDetectionSettings Copy();
}

private class BoundSettings : BaseDetectionSettings
{
    [JsonProperty("Use custom bounds")]
    public bool UseCustomBounds;
    [JsonProperty("Custom bounds")]
    public CustomBounds CustomBounds;
    public new BoundSettings Copy();
}

private class MonumentSettings
{
    [JsonProperty("Position")]
    public BaseDetectionSettings Position;
    [JsonProperty("Rotation")]
    public BaseDetectionSettings Rotation;
    [JsonProperty("Bounds")]
    public BoundSettings Bounds;
    public MonumentSettings Copy();
}

private class Configuration : SerializableConfiguration
{
    [JsonProperty(PropertyName = "Command")]
    public string Command;
    [JsonProperty("Default custom monument settings")]
    public MonumentSettings DefaultCustomMonumentSettings;
    [JsonProperty("Monuments", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    private Dictionary<string, MonumentSettings> MonumentSettingsMap;
    [JsonProperty("OverrideMonumentBounds")]
    private Dictionary<string, CustomBounds> DeprecatedOverrideMonumentBounds { get; set; }
    public MonumentSettings GetMonumentSettings(string monumentName);
    public bool AddMonument(string aliasOrShortName, BaseMonumentAdapter monument);
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
    public const string NoMonumentsFound;
    public const string ListHeader;
    public const string AtMonument;
    public const string ClosestMonument;
    public const string ClosestConfigSuccess;
    public const string ClosestConfigAlreadyPresent;
    public const string HelpHeader;
    public const string HelpList;
    public const string HelpShow;
    public const string HelpClosest;
    public const string HelpClosestConfig;
}


```

---

## MonumentProtection

```csharp
using System.Collections.Generic;
using Newtonsoft.Json;
using Oxide.Core.Plugins;
using UnityEngine;
using static Oxide.Plugins.MonumentProtectionEx.MonumentProtectionEx;
using System.Linq;

Oxide.Plugins
[Info( "Monument Protection", "Waggy", "1.0.4", ResourceId = 154 )]
[Description( "" )]
public class MonumentProtection : RustPlugin
{
    private static MonumentProtection PluginInstance;
    private List<SamSite> SamSites;
    public Dictionary<Vector3, MonumentInfo> SamPositions;
    private Timer RespawnTimer;
     int layer;
    private T RaycastLookDirection(BasePlayer player, string prefabName);
    public void SpawnSAMSites();
    public void RespawnSamSites();
    private void Init();
    [HookMethod( "OnServerInitialized" )]
     void OnServerInitialized();
    [HookMethod( "Unload" )]
     void Unload();
     object OnEntityTakeDamage(BaseCombatEntity entity, HitInfo info);
    private ConfigData config;
    protected override void LoadDefaultConfig();
    private new void SaveConfig();
    public class ConfigData
    {
        [JsonProperty( "Monuments with SAMs" )]
        public Dictionary<MonumentName, bool> MonumentWithSAMs;
        [JsonProperty( "Monument SAM Site Indestructible" )]
        public bool InvincibleSams;
        [JsonProperty( "Respawn Timer for Sams" )]
        public float RespawnTimer;
    }

}

public class ConfigData
{
    [JsonProperty( "Monuments with SAMs" )]
    public Dictionary<MonumentName, bool> MonumentWithSAMs;
    [JsonProperty( "Monument SAM Site Indestructible" )]
    public bool InvincibleSams;
    [JsonProperty( "Respawn Timer for Sams" )]
    public float RespawnTimer;
}

public static class MonumentProtectionEx
{
    private static Dictionary<string, MonumentName> MonumentToName;
    public static MonumentName GetMonumentName(MonumentInfo monument);
}

MonumentProtectionEx
public static class MonumentProtectionEx
{
    private static Dictionary<string, MonumentName> MonumentToName;
    public static MonumentName GetMonumentName(MonumentInfo monument);
}


```

---

## MultiEvents

```csharp
using System;
using System.Collections.Generic;
using Oxide.Core;
using UnityEngine;
using Newtonsoft.Json;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using System.Collections;
using System.Linq;
using Random = UnityEngine.Random;
using Oxide.Core.Libraries;

Oxide.Plugins
[Info("MultiEvents", "Mevent", "1.1.3")]
[Description("Maded by Mevent#4546")]
 class MultiEvents : RustPlugin
{
    [PluginReference]
    private readonly Plugin ImageLibrary;
    private static string Layer;
    private double time;
    public Timer Main;
    public Timer DestroyTimer;
    public Timer HelicopterStrafeTarget;
    public Timer HelicopterTarget;
    public BasePlayer winner;
    public BasePlayer runner;
    public List<string> Winners;
    private bool hasStarted;
    public string nowEvent;
    private Dictionary<BasePlayer, float> PlayersTop;
    private List<LootContainer> LookingLoot;
    private static MultiEvents instance;
    private static Vector3 EventPosition;
    private static FoundationDrop cEvent;
     MonumentInfo beginning_mission;
     MonumentInfo end_mission;
    public Item ItemForWinner;
    private static ConfigData config;
    private class ConfigData
    {
        public class Interface
        {
            [JsonProperty("Фоновый цвет")]
            public string color;
            [JsonProperty("Offset Min")]
            public string oMin;
            [JsonProperty("Offset Max")]
            public string oMax;
        }

        public class EventSettings
        {
            [JsonProperty("Настройки интерфейса событий")]
            public Interface ui;
            [JsonProperty("Сколько игроков требуется, чтобы начать событие?")]
            public int MinPlayers;
            [JsonProperty("Продолжительность события (в секундах)")]
            public int TimeDelay;
            [JsonProperty("Image URL")]
            public string ImageUrl;
            [JsonProperty("Настройки вознаграждений")]
            public List<Loot> loot;
        }

        public class CollectionResourcesSettings : EventSettings
        {
        }

        public class HuntAnimalSettings : EventSettings
        {
            [JsonProperty("Сколько очков дается за курицу?")]
            public int chicken;
            [JsonProperty("Сколько очков дается за волка?")]
            public int wolf;
            [JsonProperty("Сколько очков дается за кабана?")]
            public int boar;
            [JsonProperty("Сколько очков дается за оленя?")]
            public int deer;
            [JsonProperty("Сколько очков дается за лошадь?")]
            public int horse;
            [JsonProperty("Сколько очков дается за медведя?")]
            public int bear;
        }

        public class Loot
        {
            [JsonProperty("Награда это предмет?")]
            public bool itemenabled;
            [JsonProperty("Награда это деньги?")]
            public bool cashenabled;
            [JsonProperty("Пункт настройки")]
            public List<List<Items>> items;
            [JsonProperty("Настройка денег")]
            public Cash cash;
        }

        public class Cash
        {
            [JsonProperty("Функция вызова")]
            public string function;
            [JsonProperty("Имя плагина, чтобы дать")]
            public string plugin;
            [JsonProperty("Сумма стоимости мероприятия (следующий победитель получит меньше)")]
            public int amount;
        }

        public class Items
        {
            [JsonProperty("Название предмета (ShortName)")]
            public string shortname;
            [JsonProperty("Минимальное количество товара")]
            public int minrate;
            [JsonProperty("Максимальное количество товара")]
            public int maxrate;
            [JsonProperty("Это чертёж ?")]
            public bool blueprint;
            [JsonProperty("Название предмета (оставьте пустым для стандарта)")]
            public string displayname;
            [JsonProperty("SkinID (0 - дефолт)")]
            public ulong skinid;
            [JsonProperty("Состояние товара в процентах от 1 до 100 (0 - стандартно)")]
            public int condition;
        }

        public class LookingLootSettings : EventSettings
        {
            [JsonProperty("Какая добыча из бочек засчитывается на турнире?")]
            public List<string> Barrels;
        }

        public class SpecialCargoSettings : EventSettings
        {
            [JsonProperty("Время показа уведомления о запуске мероприятия")]
            public float timeStart;
            [JsonProperty("Время работы объявления объявления бегуна")]
            public float timeNewRunner;
            [JsonProperty("Имя маркера карты")]
            public string MarkerName;
        }

        public class HelicopterPetSettings : EventSettings
        {
        }

        public class KingMountainSettings : EventSettings
        {
        }

        public class FoundationDropSettings : EventSettings
        {
            [JsonProperty("Настройка интерфейса с информацией о количестве игроков и блоков")]
            public Interface infoUI;
            [JsonProperty("Размер арены в квадратах")]
            public int ArenaSize;
            [JsonProperty("Интервал между удалениями блоков")]
            public float DelayDestroy;
            [JsonProperty("Время ожидания игроков с момента объявления события")]
            public int WaitTime;
            [JsonProperty("Интенсивность создаваемого излучения")]
            public float IntensityRadiation;
            [JsonProperty(
                    "Отключить стандартное излучение при комнатной температуре (это необходимо, если у вас отключено излучение, плагин включит его снова, но удалит при комнатной температуре)?")]
            public bool DisableDefaultRadiation;
            [JsonProperty("Черные списки команд для игроков событий")]
            public List<string> commands;
        }

        [JsonProperty("Задержка между событиями")]
        public int Delay;
        [JsonProperty("События доступны для игроков")]
        public List<string> EnabledEvents;
        [JsonProperty("Разрешение на запуск мероприятия")]
        public string perm_admin;
        [JsonProperty("Настройка события ЛЮБИМЧИК ВЕРТОЛЁТА")]
        public HelicopterPetSettings HelicopterPet;
        [JsonProperty("Настройка события ОХОТА ЖИВОТНЫХ")]
        public HuntAnimalSettings HuntAnimal;
        [JsonProperty("Настройка события ЦАРЬ ГОРЫ")]
        public KingMountainSettings KingMountain;
        [JsonProperty("Настройка события СБОР РЕСУРСОВ")]
        public CollectionResourcesSettings CollectionResources;
        [JsonProperty("Настройка события ПОИСК LOOT")]
        public LookingLootSettings LookingLoot;
        [JsonProperty("Настройка события СПЕЦИАЛЬНЫЙ ГРУЗ")]
        public SpecialCargoSettings SpecialCargo;
        [JsonProperty("Установка события ПАДАЮЩИЕ ПЛАТФОРМЫ")]
        public FoundationDropSettings FoundationDrop;
    }

    private ConfigData GetDefaultConfig();
    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
     void OnServerInitialized();
     void Unload();
     void StartEvent(string type, BasePlayer target);
     void DestroyEvent(string type);
    private void DropItem(BasePlayer player, string type, int temp);
     double GrabCurrentTime();
     void AddToDictionary(BasePlayer player, float amount);
     object OnPlayerCommand(ConsoleSystem.Arg arg);
     void OnEntityKill(BaseNetworkable entity);
     void OnPlayerRespawned(BasePlayer player);
     void OnPlayerDie(BasePlayer player, HitInfo info);
     void OnLootEntity(BasePlayer player, BaseEntity entity);
     void OnEntityDeath(BaseCombatEntity entity, HitInfo info);
     object OnCollectiblePickup(Item item, BasePlayer player);
     object OnCropGather(PlantEntity plant, Item item, BasePlayer player);
     object OnDispenserBonus(ResourceDispenser dispenser, BasePlayer player, Item item);
     object OnDispenserGather(ResourceDispenser dispenser, BaseEntity entity, Item item);
     void TopUI(BasePlayer player, string Type, string TopName, string description, string oMin, string oMax, string color, List<KeyValuePair<BasePlayer, float>> list);
    private void UI_Notification(BasePlayer player, string message, string Name, string color);
     void Help_UI(BasePlayer player, int page);
    [ChatCommand("event")]
     void cmdEvent(BasePlayer player, string command, string[] args);
    [ConsoleCommand("event")]
    private void CmdConsole(ConsoleSystem.Arg args);
    protected override void LoadDefaultMessages();
    private string GetMessage(string messageKey, string playerID, object[] args);
    private string GetGridString(Vector3 position);
    private string NumberToString(int number);
    private void StartSleeping(BasePlayer player);
    private static void ClearTeleport(BasePlayer player, Vector3 position);
    private class FoundationDrop
    {
        public double StartTime;
        public bool Started;
        public bool Finished;
        public bool Given;
        public int Received;
        public Timer StartTimer;
        public Timer DestroyTimer;
        public Dictionary<ulong, Vector3> PlayerConnected;
        public List<List<BaseEntity>> BlockList;
        public void JoinEvent(BasePlayer player);
        public void LeftEvent(BasePlayer player);
        public void HandlePlayers();
        public void StartEvent(int startDelay);
        public void InitializeEvent(int startDelay);
        public void FinishEvent();
    }

    public class ZoneList
    {
        public RadZones zone;
    }

    private void OnServerRadiation();
    private Dictionary<int, ZoneList> RadiationZones;
    private static readonly int playerLayer;
    private static readonly Collider[] colBuffer;
    private void InitializeZone(Vector3 Location, float intensity, int ZoneID);
    public class RadZones : MonoBehaviour
    {
        private int ID;
        private Vector3 Position;
        private float ZoneRadius;
        private float RadiationAmount;
        private List<BasePlayer> InZone;
        private void Awake();
        public void Activate(Vector3 pos, float radius, float amount, int ZoneID);
        private void OnDestroy();
        private void UpdateCollider();
        private void UpdateTrigger();
    }

    private IEnumerator InitializeFoundation(int startDelay);
    private void DropFoundation();
    private class Downgrader : FacepunchBehaviour
    {
        private BuildingGrade.Enum gradeEnum;
        public void Downgrade(BaseEntity entity);
        private void Downgrade(BuildingBlock block);
        private void OnDestroy();
        public void Kill();
    }

    private class HeliPet : FacepunchBehaviour
    {
        private BaseHelicopter helicopter;
        private void Awake();
        private void Cheking(BaseHelicopter helicopter);
        private void OnDestroy();
        public void Kill();
    }

    private class SCmarker : FacepunchBehaviour
    {
        private MonumentInfo monument;
        private MapMarker mapMarker;
        private void Awake();
        public void spawnMarker();
        private void OnDestroy();
        public void Kill();
    }

    private class SCPlayerMarker : FacepunchBehaviour
    {
        private BasePlayer player;
        private MapMarker mapMarker;
        private void Awake();
        public void SpawnMarker();
        public void UpdatePostion();
        private void OnDestroy();
        public void Kill();
    }

}

private class ConfigData
{
    public class Interface
    {
        [JsonProperty("Фоновый цвет")]
        public string color;
        [JsonProperty("Offset Min")]
        public string oMin;
        [JsonProperty("Offset Max")]
        public string oMax;
    }

    public class EventSettings
    {
        [JsonProperty("Настройки интерфейса событий")]
        public Interface ui;
        [JsonProperty("Сколько игроков требуется, чтобы начать событие?")]
        public int MinPlayers;
        [JsonProperty("Продолжительность события (в секундах)")]
        public int TimeDelay;
        [JsonProperty("Image URL")]
        public string ImageUrl;
        [JsonProperty("Настройки вознаграждений")]
        public List<Loot> loot;
    }

    public class CollectionResourcesSettings : EventSettings
    {
    }

    public class HuntAnimalSettings : EventSettings
    {
        [JsonProperty("Сколько очков дается за курицу?")]
        public int chicken;
        [JsonProperty("Сколько очков дается за волка?")]
        public int wolf;
        [JsonProperty("Сколько очков дается за кабана?")]
        public int boar;
        [JsonProperty("Сколько очков дается за оленя?")]
        public int deer;
        [JsonProperty("Сколько очков дается за лошадь?")]
        public int horse;
        [JsonProperty("Сколько очков дается за медведя?")]
        public int bear;
    }

    public class Loot
    {
        [JsonProperty("Награда это предмет?")]
        public bool itemenabled;
        [JsonProperty("Награда это деньги?")]
        public bool cashenabled;
        [JsonProperty("Пункт настройки")]
        public List<List<Items>> items;
        [JsonProperty("Настройка денег")]
        public Cash cash;
    }

    public class Cash
    {
        [JsonProperty("Функция вызова")]
        public string function;
        [JsonProperty("Имя плагина, чтобы дать")]
        public string plugin;
        [JsonProperty("Сумма стоимости мероприятия (следующий победитель получит меньше)")]
        public int amount;
    }

    public class Items
    {
        [JsonProperty("Название предмета (ShortName)")]
        public string shortname;
        [JsonProperty("Минимальное количество товара")]
        public int minrate;
        [JsonProperty("Максимальное количество товара")]
        public int maxrate;
        [JsonProperty("Это чертёж ?")]
        public bool blueprint;
        [JsonProperty("Название предмета (оставьте пустым для стандарта)")]
        public string displayname;
        [JsonProperty("SkinID (0 - дефолт)")]
        public ulong skinid;
        [JsonProperty("Состояние товара в процентах от 1 до 100 (0 - стандартно)")]
        public int condition;
    }

    public class LookingLootSettings : EventSettings
    {
        [JsonProperty("Какая добыча из бочек засчитывается на турнире?")]
        public List<string> Barrels;
    }

    public class SpecialCargoSettings : EventSettings
    {
        [JsonProperty("Время показа уведомления о запуске мероприятия")]
        public float timeStart;
        [JsonProperty("Время работы объявления объявления бегуна")]
        public float timeNewRunner;
        [JsonProperty("Имя маркера карты")]
        public string MarkerName;
    }

    public class HelicopterPetSettings : EventSettings
    {
    }

    public class KingMountainSettings : EventSettings
    {
    }

    public class FoundationDropSettings : EventSettings
    {
        [JsonProperty("Настройка интерфейса с информацией о количестве игроков и блоков")]
        public Interface infoUI;
        [JsonProperty("Размер арены в квадратах")]
        public int ArenaSize;
        [JsonProperty("Интервал между удалениями блоков")]
        public float DelayDestroy;
        [JsonProperty("Время ожидания игроков с момента объявления события")]
        public int WaitTime;
        [JsonProperty("Интенсивность создаваемого излучения")]
        public float IntensityRadiation;
        [JsonProperty(
                    "Отключить стандартное излучение при комнатной температуре (это необходимо, если у вас отключено излучение, плагин включит его снова, но удалит при комнатной температуре)?")]
        public bool DisableDefaultRadiation;
        [JsonProperty("Черные списки команд для игроков событий")]
        public List<string> commands;
    }

    [JsonProperty("Задержка между событиями")]
    public int Delay;
    [JsonProperty("События доступны для игроков")]
    public List<string> EnabledEvents;
    [JsonProperty("Разрешение на запуск мероприятия")]
    public string perm_admin;
    [JsonProperty("Настройка события ЛЮБИМЧИК ВЕРТОЛЁТА")]
    public HelicopterPetSettings HelicopterPet;
    [JsonProperty("Настройка события ОХОТА ЖИВОТНЫХ")]
    public HuntAnimalSettings HuntAnimal;
    [JsonProperty("Настройка события ЦАРЬ ГОРЫ")]
    public KingMountainSettings KingMountain;
    [JsonProperty("Настройка события СБОР РЕСУРСОВ")]
    public CollectionResourcesSettings CollectionResources;
    [JsonProperty("Настройка события ПОИСК LOOT")]
    public LookingLootSettings LookingLoot;
    [JsonProperty("Настройка события СПЕЦИАЛЬНЫЙ ГРУЗ")]
    public SpecialCargoSettings SpecialCargo;
    [JsonProperty("Установка события ПАДАЮЩИЕ ПЛАТФОРМЫ")]
    public FoundationDropSettings FoundationDrop;
}

public class Interface
{
    [JsonProperty("Фоновый цвет")]
    public string color;
    [JsonProperty("Offset Min")]
    public string oMin;
    [JsonProperty("Offset Max")]
    public string oMax;
}

public class EventSettings
{
    [JsonProperty("Настройки интерфейса событий")]
    public Interface ui;
    [JsonProperty("Сколько игроков требуется, чтобы начать событие?")]
    public int MinPlayers;
    [JsonProperty("Продолжительность события (в секундах)")]
    public int TimeDelay;
    [JsonProperty("Image URL")]
    public string ImageUrl;
    [JsonProperty("Настройки вознаграждений")]
    public List<Loot> loot;
}

public class CollectionResourcesSettings : EventSettings
{
}

public class HuntAnimalSettings : EventSettings
{
    [JsonProperty("Сколько очков дается за курицу?")]
    public int chicken;
    [JsonProperty("Сколько очков дается за волка?")]
    public int wolf;
    [JsonProperty("Сколько очков дается за кабана?")]
    public int boar;
    [JsonProperty("Сколько очков дается за оленя?")]
    public int deer;
    [JsonProperty("Сколько очков дается за лошадь?")]
    public int horse;
    [JsonProperty("Сколько очков дается за медведя?")]
    public int bear;
}

public class Loot
{
    [JsonProperty("Награда это предмет?")]
    public bool itemenabled;
    [JsonProperty("Награда это деньги?")]
    public bool cashenabled;
    [JsonProperty("Пункт настройки")]
    public List<List<Items>> items;
    [JsonProperty("Настройка денег")]
    public Cash cash;
}

public class Cash
{
    [JsonProperty("Функция вызова")]
    public string function;
    [JsonProperty("Имя плагина, чтобы дать")]
    public string plugin;
    [JsonProperty("Сумма стоимости мероприятия (следующий победитель получит меньше)")]
    public int amount;
}

public class Items
{
    [JsonProperty("Название предмета (ShortName)")]
    public string shortname;
    [JsonProperty("Минимальное количество товара")]
    public int minrate;
    [JsonProperty("Максимальное количество товара")]
    public int maxrate;
    [JsonProperty("Это чертёж ?")]
    public bool blueprint;
    [JsonProperty("Название предмета (оставьте пустым для стандарта)")]
    public string displayname;
    [JsonProperty("SkinID (0 - дефолт)")]
    public ulong skinid;
    [JsonProperty("Состояние товара в процентах от 1 до 100 (0 - стандартно)")]
    public int condition;
}

public class LookingLootSettings : EventSettings
{
    [JsonProperty("Какая добыча из бочек засчитывается на турнире?")]
    public List<string> Barrels;
}

public class SpecialCargoSettings : EventSettings
{
    [JsonProperty("Время показа уведомления о запуске мероприятия")]
    public float timeStart;
    [JsonProperty("Время работы объявления объявления бегуна")]
    public float timeNewRunner;
    [JsonProperty("Имя маркера карты")]
    public string MarkerName;
}

public class HelicopterPetSettings : EventSettings
{
}

public class KingMountainSettings : EventSettings
{
}

public class FoundationDropSettings : EventSettings
{
    [JsonProperty("Настройка интерфейса с информацией о количестве игроков и блоков")]
    public Interface infoUI;
    [JsonProperty("Размер арены в квадратах")]
    public int ArenaSize;
    [JsonProperty("Интервал между удалениями блоков")]
    public float DelayDestroy;
    [JsonProperty("Время ожидания игроков с момента объявления события")]
    public int WaitTime;
    [JsonProperty("Интенсивность создаваемого излучения")]
    public float IntensityRadiation;
    [JsonProperty(
                    "Отключить стандартное излучение при комнатной температуре (это необходимо, если у вас отключено излучение, плагин включит его снова, но удалит при комнатной температуре)?")]
    public bool DisableDefaultRadiation;
    [JsonProperty("Черные списки команд для игроков событий")]
    public List<string> commands;
}

private class FoundationDrop
{
    public double StartTime;
    public bool Started;
    public bool Finished;
    public bool Given;
    public int Received;
    public Timer StartTimer;
    public Timer DestroyTimer;
    public Dictionary<ulong, Vector3> PlayerConnected;
    public List<List<BaseEntity>> BlockList;
    public void JoinEvent(BasePlayer player);
    public void LeftEvent(BasePlayer player);
    public void HandlePlayers();
    public void StartEvent(int startDelay);
    public void InitializeEvent(int startDelay);
    public void FinishEvent();
}

public class ZoneList
{
    public RadZones zone;
}

public class RadZones : MonoBehaviour
{
    private int ID;
    private Vector3 Position;
    private float ZoneRadius;
    private float RadiationAmount;
    private List<BasePlayer> InZone;
    private void Awake();
    public void Activate(Vector3 pos, float radius, float amount, int ZoneID);
    private void OnDestroy();
    private void UpdateCollider();
    private void UpdateTrigger();
}

private class Downgrader : FacepunchBehaviour
{
    private BuildingGrade.Enum gradeEnum;
    public void Downgrade(BaseEntity entity);
    private void Downgrade(BuildingBlock block);
    private void OnDestroy();
    public void Kill();
}

private class HeliPet : FacepunchBehaviour
{
    private BaseHelicopter helicopter;
    private void Awake();
    private void Cheking(BaseHelicopter helicopter);
    private void OnDestroy();
    public void Kill();
}

private class SCmarker : FacepunchBehaviour
{
    private MonumentInfo monument;
    private MapMarker mapMarker;
    private void Awake();
    public void spawnMarker();
    private void OnDestroy();
    public void Kill();
}

private class SCPlayerMarker : FacepunchBehaviour
{
    private BasePlayer player;
    private MapMarker mapMarker;
    private void Awake();
    public void SpawnMarker();
    public void UpdatePostion();
    private void OnDestroy();
    public void Kill();
}


```

---

## MyMiniUI

```csharp
using System;
using System.Collections.Generic;
using System.Globalization;
using System.Linq;
using Facepunch;
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Libraries.Covalence;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using UnityEngine;
using System.IO;
using System.Collections;

Oxide.Plugins
[Info("MyMiniUI","GAGA","1.0.0")]
public class MyMiniUI : RustPlugin
{
    [PluginReference]
     Plugin ImageLibrary;
    private void Unload();
    private string Layer;
     void OnServerInitialized();
    private void OnPlayerConnected(BasePlayer player);
    private void UI_DrawInterface(BasePlayer player);
}


```

---

## NameFix

```csharp
using System.Text.RegularExpressions;
using Oxide.Core.Libraries.Covalence;

Oxide.Plugins
[Info("NameFix", "Visagalis", "1.0.0")]
[Description("Removes advertisements from player names when they login.")]
 class NameFix : CovalencePlugin
{
     void OnUserConnected(IPlayer player);
}


```

---

## NameGrabber

```csharp
using System.Linq;
using UnityEngine;
using System;
using Rust;
using System.Collections.Generic;
using Oxide.Core;

Oxide.Plugins
[Info("NameGrabber", "Wolfs Darker", "1.0.1")]
 class NameGrabber : RustPlugin
{
    [ConsoleCommand("grabname")]
     void cmdGrabName(ConsoleSystem.Arg arg);
}


```

---

## NameRewards

```csharp
using Newtonsoft.Json;
using Oxide.Core.Libraries.Covalence;
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text.RegularExpressions;

Oxide.Plugins
[Info("NameRewards", "Fierrov", "1.1.1")]
[Description("Powered by humans from Fierrov")]
 class NameRewards : CovalencePlugin
{
    private PluginConfig _config;
    private class PluginConfig
    {
        [JsonProperty("Варианты текста, который должен быть в нике")]
        public List<string> Texts;
        [JsonProperty("Использовать регулярные выражения")]
        public bool UseRegex;
        [JsonProperty("Не учитывать регистр")]
        public bool IgnoreCase;
        [JsonProperty("Оповещать о получении привилегий/необходимости добавления текста")]
        public bool Notify;
        [JsonProperty("Формат сообщений в чате")]
        public string ChatFormat;
        [JsonProperty("Привилегии, которые будут выданы")]
        public List<string> Permissions;
        [JsonProperty("Группы, в которые игрок будет добавлен")]
        public List<string> Groups;
        public Message AvailableOptions();
        [JsonIgnore]
        private Func<string, string, bool> _check;
        [JsonIgnore]
        private Func<string, string, bool> Check { get; set; }
        public static PluginConfig DefaultConfig { get; set; }
        public bool IsMatch(string text, string found);
    }

    private bool CheckConfig();
    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    protected override void SaveConfig();
    private void SendResponse(IPlayer player, string langKey, object[] args);
    private string GetMsg(string key, string playerid);
    private string GetMsg(string key, ulong playerid);
    protected override void LoadDefaultMessages();
    private void Init();
    private void OnUserConnected(IPlayer player);
    private void Grant(IPlayer player);
    private void Revoke(IPlayer player);
}

private class PluginConfig
{
    [JsonProperty("Варианты текста, который должен быть в нике")]
    public List<string> Texts;
    [JsonProperty("Использовать регулярные выражения")]
    public bool UseRegex;
    [JsonProperty("Не учитывать регистр")]
    public bool IgnoreCase;
    [JsonProperty("Оповещать о получении привилегий/необходимости добавления текста")]
    public bool Notify;
    [JsonProperty("Формат сообщений в чате")]
    public string ChatFormat;
    [JsonProperty("Привилегии, которые будут выданы")]
    public List<string> Permissions;
    [JsonProperty("Группы, в которые игрок будет добавлен")]
    public List<string> Groups;
    public Message AvailableOptions();
    [JsonIgnore]
    private Func<string, string, bool> _check;
    [JsonIgnore]
    private Func<string, string, bool> Check { get; set; }
    public static PluginConfig DefaultConfig { get; set; }
    public bool IsMatch(string text, string found);
}


```

---

## NeonSkins

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using Oxide.Core;
using Oxide.Core.Configuration;
using UnityEngine;
using Network;
using Oxide.Game.Rust.Cui;

Oxide.Plugins
[Info("NeonSkins", "Colon Blow", "1.0.11")]
 class NeonSkins : RustPlugin
{
     BaseEntity newNeon;
    private bool initialized;
     void Loaded();
    private void OnServerInitialized();
     bool isAllowed(BasePlayer player, string perm);
    private void RestoreNeonSkins();
     bool Changed;
     bool BlockSignDamage;
     bool UseMaxSignChecks;
    public int maxsigns;
    public int maxvipsigns;
    static ulong sign1skin1;
    static ulong sign1skin2;
    static ulong sign1skin3;
    static ulong sign2skin1;
    static ulong sign2skin2;
    static ulong sign2skin3;
    static ulong sign3skin1;
    static ulong sign3skin2;
    static ulong sign3skin3;
    static ulong sign4skin1;
    static ulong sign4skin2;
    static ulong sign4skin3;
    static ulong sign5skin1;
    static ulong sign5skin2;
    static ulong sign5skin3;
    static ulong sign6skin1;
    static ulong sign6skin2;
    static ulong sign6skin3;
    static ulong sign7skin1;
    static ulong sign7skin2;
    static ulong sign7skin3;
    static ulong sign8skin1;
    static ulong sign8skin2;
    static ulong sign8skin3;
    static ulong sign9skin1;
    static ulong sign9skin2;
    static ulong sign9skin3;
    static ulong sign10skin1;
    static ulong sign10skin2;
    static ulong sign10skin3;
    static ulong sign11skin1;
    static ulong sign11skin2;
    static ulong sign11skin3;
    static ulong sign12skin1;
    static ulong sign12skin2;
    static ulong sign12skin3;
    static ulong sign13skin1;
    static ulong sign13skin2;
    static ulong sign13skin3;
    static ulong sign14skin1;
    static ulong sign14skin2;
    static ulong sign14skin3;
    static ulong sign15skin1;
    static ulong sign15skin2;
    static ulong sign15skin3;
    static ulong sign16skin1;
    static ulong sign16skin2;
    static ulong sign16skin3;
    static ulong sign17skin1;
    static ulong sign17skin2;
    static ulong sign17skin3;
    static ulong leftwingangel;
    static ulong leftwingfairy;
    static ulong leftwingblack;
    static ulong rightwingangel;
    static ulong rightwingfairy;
    static ulong rightwingblack;
    private void LoadVariables();
     void LoadDefaultConfig();
    private void LoadConfigVariables();
    private void CheckCfg(string Key, T var);
    private void CheckCfgFloat(string Key, float var);
     object GetConfig(string menu, string datavalue, object defaultValue);
     string msg(string key, string playerId);
     Dictionary<string, string> Messages;
    static Dictionary<ulong, PlayerSignData> loadplayer;
    public class PlayerSignData
    {
        public BasePlayer player;
        public int signcount;
    }

    static StoredData storedData;
     DynamicConfigFile dataFile;
    public class StoredData
    {
        public Dictionary<uint, StoredSkinsData> saveSkinData;
        public StoredData();
    }

    public class StoredSkinsData
    {
        public ulong ownerid;
        public string pos;
        public string eangles;
        public string rot;
        public ulong skin1;
        public ulong skin2;
        public ulong skin3;
        public int tickrate;
    }

     void LoadDataFile();
     void AddData(uint entnetid, ulong sownerid, string spos, string seangles, string srot, ulong skinid1, ulong skinid2, ulong skinid3, int signtickrate);
     void RemoveData(uint entnetid);
     void SaveData();
    public static Vector3 StringToVector3(string sVector);
    public static Quaternion StringToQuaternion(string sVector);
    [ChatCommand("neon")]
     void cmdChatNeonGUI(BasePlayer player, string command, string[] args);
    [ChatCommand("neon.add")]
     void cmdChatNeonAdd(BasePlayer player, string command, string[] args);
    [ChatCommand("neon.skinwide")]
     void cmdChatNeonSkinWide(BasePlayer player, string command, string[] args);
    [ChatCommand("neon.skintall")]
     void cmdChatNeonSkinTall(BasePlayer player, string command, string[] args);
    [ChatCommand("neon.skinattire")]
     void cmdNeonSkinAttire(BasePlayer player, string command, string[] args);
    [ChatCommand("neon.skinrug")]
     void cmdChatNeonSkinRug(BasePlayer player, string command, string[] args);
    [ChatCommand("neon.toggle")]
     void cmdChatPimpToggle(BasePlayer player, string command, string[] args);
    [ChatCommand("neon.cleardatabase")]
     void cmdChatNeonClearDataBase(BasePlayer player, string command, string[] args);
    [ChatCommand("neon.tickrate")]
     void cmdChatNeonTickRate(BasePlayer player, string command, string[] args);
    [ChatCommand("neon.wings")]
     void cmdChatToggleWings(BasePlayer player, string command, string[] args);
    [ChatCommand("neon.count")]
     void cmdChatNeonCount(BasePlayer player, string command, string[] args);
     bool IsPimpedOut(BaseEntity entity);
     void ToggleSignState(BaseEntity entity);
     void ToggleSignTickRate(BaseEntity entity, int signtickrate);
     void OnEntityDeath(BaseCombatEntity entity, HitInfo info);
     void OnEntityTakeDamage(BaseCombatEntity entity, HitInfo hitInfo);
    private object CanPickupEntity(BaseCombatEntity entity, BasePlayer player);
     bool SignLimitReached(BasePlayer player);
     void AddPlayerID(ulong ownerid);
     void RemovePlayerID(ulong ownerid);
     void PrepNeon(BasePlayer player, BaseEntity newNeon, string[] args, bool iswide, bool isrug);
     void SpawnSign(ulong ownerid, Vector3 pos, Quaternion rot, Vector3 angle, ulong skinid1, ulong skinid2, ulong skinid3, int signtickrate);
     void SkinRug(BaseEntity entity, ulong skinid1, ulong skinid2, ulong skinid3);
     void OnPlayerDisconnected(BasePlayer player, string reason);
     void OnPlayerRespawned(BasePlayer player);
     void Unload();
    static void DestroyAll();
     class AttireChanger : MonoBehaviour
    {
         BasePlayer player;
         Item item;
         int counter;
        public bool isanimated;
        public ulong skin1;
        public ulong skin2;
        public ulong skin3;
        public int framecounter;
         void Awake();
        public void GetItem(int itemID);
         bool SingleSkin();
         void FixedUpdate();
        public void RefreshAll();
    }

     class SkinChanger : MonoBehaviour
    {
         BaseEntity entity;
         Vector3 entitypos;
         Quaternion entityrot;
         int counter;
        public bool isanimated;
         int skinnum;
        public ulong skin1;
        public ulong skin2;
        public ulong skin3;
        public int tickrate;
         ulong ownerid;
         uint entityid;
         NeonSkins instance;
         void Awake();
         void ClntDstry(BaseNetworkable entity, bool recursive);
         void EnttSnpsht(BaseNetworkable entity, bool recursive);
         bool SingleSkin();
         void FixedUpdate();
        public void RefreshAll();
        public void OnDestroy();
    }

     class WingEntity : BaseEntity
    {
         BaseEntity entity;
         Vector3 entitypos;
         Quaternion entityrot;
         BaseEntity wing1;
         BaseEntity wing2;
         bool isback;
         int counter;
        public bool isfairy;
        public bool isangel;
        public bool isblack;
         void Awake();
        public void AddWings();
         void FixedUpdate();
         void RefreshEntities();
        public void OnDestroy();
    }

     class SignGUI : MonoBehaviour
    {
         BasePlayer player;
         NeonSkins neonskins;
         string b1;
         void Awake();
        public void AddSignGUI(BasePlayer player);
         void DestroyCui(BasePlayer player);
        public void OnDestroy();
    }

}

public class PlayerSignData
{
    public BasePlayer player;
    public int signcount;
}

public class StoredData
{
    public Dictionary<uint, StoredSkinsData> saveSkinData;
    public StoredData();
}

public class StoredSkinsData
{
    public ulong ownerid;
    public string pos;
    public string eangles;
    public string rot;
    public ulong skin1;
    public ulong skin2;
    public ulong skin3;
    public int tickrate;
}

 class AttireChanger : MonoBehaviour
{
     BasePlayer player;
     Item item;
     int counter;
    public bool isanimated;
    public ulong skin1;
    public ulong skin2;
    public ulong skin3;
    public int framecounter;
     void Awake();
    public void GetItem(int itemID);
     bool SingleSkin();
     void FixedUpdate();
    public void RefreshAll();
}

 class SkinChanger : MonoBehaviour
{
     BaseEntity entity;
     Vector3 entitypos;
     Quaternion entityrot;
     int counter;
    public bool isanimated;
     int skinnum;
    public ulong skin1;
    public ulong skin2;
    public ulong skin3;
    public int tickrate;
     ulong ownerid;
     uint entityid;
     NeonSkins instance;
     void Awake();
     void ClntDstry(BaseNetworkable entity, bool recursive);
     void EnttSnpsht(BaseNetworkable entity, bool recursive);
     bool SingleSkin();
     void FixedUpdate();
    public void RefreshAll();
    public void OnDestroy();
}

 class WingEntity : BaseEntity
{
     BaseEntity entity;
     Vector3 entitypos;
     Quaternion entityrot;
     BaseEntity wing1;
     BaseEntity wing2;
     bool isback;
     int counter;
    public bool isfairy;
    public bool isangel;
    public bool isblack;
     void Awake();
    public void AddWings();
     void FixedUpdate();
     void RefreshEntities();
    public void OnDestroy();
}

 class SignGUI : MonoBehaviour
{
     BasePlayer player;
     NeonSkins neonskins;
     string b1;
     void Awake();
    public void AddSignGUI(BasePlayer player);
     void DestroyCui(BasePlayer player);
    public void OnDestroy();
}


```

---

## NeverWear

```csharp
using System.Collections.Generic;

Oxide.Plugins
[Info("NeverWear", "k1lly0u", "0.1.32", ResourceId = 1816)]
 class NeverWear : RustPlugin
{
     void Loaded();
     void OnServerInitialized();
    private void RegisterPermissions();
    private bool HasPerm(BasePlayer player, string perm);
     void OnLoseCondition(Item item, float amount);
    private ConfigData configData;
     class ConfigData
    {
        public bool useWeapons { get; set; }
        public bool useTools { get; set; }
        public bool useAttire { get; set; }
        public bool useWhiteList { get; set; }
        public List<string> WhitelistedItems { get; set; }
    }

    private void LoadVariables();
    protected override void LoadDefaultConfig();
    private void LoadConfigVariables();
     void SaveConfig(ConfigData config);
}

 class ConfigData
{
    public bool useWeapons { get; set; }
    public bool useTools { get; set; }
    public bool useAttire { get; set; }
    public bool useWhiteList { get; set; }
    public List<string> WhitelistedItems { get; set; }
}


```

---

## NewFurnace

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
[Info("NewFurnace", "CASHR", "1.0.0")]
public class NewFurnace : RustPlugin
{
    [JsonProperty("Список ящиков в которых его спавнить")]
    public List<string> ListContainers;
    private class Configuration
    {
        [JsonProperty("Шанс нахождения печки в ящике")]
        public int Chance;
        [JsonProperty("Скорость плавки")]
        public double Speed;
        [JsonProperty("Название печи")]
        public string DisplayName;
        [JsonProperty("Описание печи")]
        public string Desk;
        [JsonProperty("СкинИД предмета")]
        public ulong SkinID;
        public static Configuration GetNewConf();
    }

    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private Configuration _config;
    protected override void LoadConfig();
     object OnOvenToggle(BaseOven oven, BasePlayer player);
     float CookingTemperature(BaseOven.TemperatureType temperature);
    private void OnServerInitialized();
     void OnLootSpawn(LootContainer container);
    public void ReplyWithHelper(BasePlayer player, string message, string[] args);
     void StartCooking(BaseOven oven, BaseEntity entity, double ovenMultiplier);
     Item FindBurnable(BaseOven oven);
    private static string HexToRGB(string hex);
    public Item FindItem(BasePlayer player, int itemID, ulong skinID, int amount);
    public bool HaveItem(BasePlayer player, int itemID, ulong skinID, int amount);
}

private class Configuration
{
    [JsonProperty("Шанс нахождения печки в ящике")]
    public int Chance;
    [JsonProperty("Скорость плавки")]
    public double Speed;
    [JsonProperty("Название печи")]
    public string DisplayName;
    [JsonProperty("Описание печи")]
    public string Desk;
    [JsonProperty("СкинИД предмета")]
    public ulong SkinID;
    public static Configuration GetNewConf();
}


```

---

## NewStorages

```csharp
using System.Collections.Generic;
using System.Linq;
using Oxide.Core;

Oxide.Plugins
[Info("NewStorages", "Ryamkk", "1.0.0")]
public class NewStorages : RustPlugin
{
    private void OnServerInitialized();
    private void OnEntitySpawned(BaseNetworkable entity);
    private void UpgradeLargeBox(StorageContainer container);
    private void UpgradeSmallBox(StorageContainer container);
    private void CheckAllContainers(ulong id);
    private void CheckEntity(BaseNetworkable a);
}


```

---

## NickManager

```csharp
using System;
using System.Collections.Generic;
using Oxide.Core;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using UnityEngine;
using System.Globalization;
using Newtonsoft.Json;
using Color = UnityEngine.Color;

Oxide.Plugins
[Info("NickeManager", "empty", "1.0.6")]
[Description("Плагин для управлением никнеймов игроков")]
public class NickManager : RustPlugin
{
    [PluginReference]
     Plugin ImageLibrary;
    private StoredData DataBase;
    private string GetImg(string name);
    public string GetImage(string shortname, ulong skin);
    static public ConfigData config;
    public class ConfigData
    {
        [JsonProperty(PropertyName = "ZealNickManager")]
        public NickManager ZealNickManager;
        public class NickManager
        {
            [JsonProperty(PropertyName = "Удалять запрещённые символы/фразы на ?")]
            public bool ReplaceNick;
            [JsonProperty(PropertyName = "Уведомлять игрока о замене никнейм ?")]
            public bool AlertReplaceNick;
            [JsonProperty(PropertyName = "Хранить историю никнеймов игрока ?")]
            public bool HistoryNickSave;
            [JsonProperty(PropertyName = "Разрешение на использование ZealNickManager")]
            public string PermissionUse;
            [JsonProperty(PropertyName = "Запрещенные символы/фразы")]
            public List<string> ReplacesValue;
        }

    }

    public ConfigData GetDefaultConfig();
    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private string Sharp;
    private string Blur;
    private string radial;
    private string regular;
    private string Layer;
     void PlayerList(BasePlayer player);
     void PlayerInfo(BasePlayer player, ulong SteamID);
     void HistoryNick(BasePlayer player, ulong SteamID);
     void ChangeName(BasePlayer player, ulong SteamID);
    [ChatCommand("steamid")]
    private void NickManagerGui(BasePlayer player);
    [ChatCommand("replace")]
    private void Replaces(BasePlayer player);
    [ConsoleCommand("changename.gui")]
    private void ChangeNameText(ConsoleSystem.Arg args);
    [ConsoleCommand("nickmanager.info")]
    private void NickManagerInfoPlayer(ConsoleSystem.Arg args);
    [ConsoleCommand("changename")]
    private void ChangeName(ConsoleSystem.Arg args);
    [ConsoleCommand("close.nickmanager")]
    private void NickManagerGuiClose(ConsoleSystem.Arg args);
     void CheckDataBase(BasePlayer player);
     void AddPlayer(BasePlayer player);
     void OnPlayerInit(BasePlayer player);
     void OnServerInitialized();
    private void Unload();
     class StoredData
    {
        public Dictionary<ulong, NickBD> NickManager;
    }

     class NickBD
    {
        public string Name;
        public ulong SteamID;
        public string NickReplace;
        public bool ReplaceNick;
        public Dictionary<string, string> NickHistory;
        public NickBD();
    }

    private void SaveData();
    private void LoadData();
    private static string HexToRustFormat(string hex);
}

public class ConfigData
{
    [JsonProperty(PropertyName = "ZealNickManager")]
    public NickManager ZealNickManager;
    public class NickManager
    {
        [JsonProperty(PropertyName = "Удалять запрещённые символы/фразы на ?")]
        public bool ReplaceNick;
        [JsonProperty(PropertyName = "Уведомлять игрока о замене никнейм ?")]
        public bool AlertReplaceNick;
        [JsonProperty(PropertyName = "Хранить историю никнеймов игрока ?")]
        public bool HistoryNickSave;
        [JsonProperty(PropertyName = "Разрешение на использование ZealNickManager")]
        public string PermissionUse;
        [JsonProperty(PropertyName = "Запрещенные символы/фразы")]
        public List<string> ReplacesValue;
    }

}

public class NickManager
{
    [JsonProperty(PropertyName = "Удалять запрещённые символы/фразы на ?")]
    public bool ReplaceNick;
    [JsonProperty(PropertyName = "Уведомлять игрока о замене никнейм ?")]
    public bool AlertReplaceNick;
    [JsonProperty(PropertyName = "Хранить историю никнеймов игрока ?")]
    public bool HistoryNickSave;
    [JsonProperty(PropertyName = "Разрешение на использование ZealNickManager")]
    public string PermissionUse;
    [JsonProperty(PropertyName = "Запрещенные символы/фразы")]
    public List<string> ReplacesValue;
}

 class StoredData
{
    public Dictionary<ulong, NickBD> NickManager;
}

 class NickBD
{
    public string Name;
    public ulong SteamID;
    public string NickReplace;
    public bool ReplaceNick;
    public Dictionary<string, string> NickHistory;
    public NickBD();
}


```

---

## NightLantern

```csharp
using System.Collections.Generic;
using System.Linq;

Oxide.Plugins
[Info("NightLantern", "k1lly0u", "2.0.3", ResourceId = 1182)]
 class NightLantern : RustPlugin
{
    private Timer timeCheck;
    private List<BaseOven> lights;
    private bool isActivated;
    private bool isEnabled;
    private bool lightsOn;
    private bool nfrInstalled;
     void Loaded();
     void OnServerInitialized();
     void OnConsumeFuel(BaseOven oven, Item fuel, ItemModBurnable burnable);
     void OnEntitySpawned(BaseEntity entity);
     void OnEntityDeath(BaseEntity entity, HitInfo hitInfo);
     void Unload();
     void FindLights();
     void CheckType(BaseOven oven);
     void TimeLoop();
     void CheckTime();
     void ToggleLanterns(bool status);
     ConsumeTypes StringToType(string name);
    [ChatCommand("lantern")]
     void cmdLantern(BasePlayer player, string command, string[] args);
    private ConfigData configData;
     class LightTypes
    {
        public bool Campfires { get; set; }
        public bool Lanterns { get; set; }
        public bool CeilingLights { get; set; }
        public bool Furnaces { get; set; }
        public bool JackOLanterns { get; set; }
    }

     class ConfigData
    {
        public bool ConsumeFuel { get; set; }
        public Dictionary<ConsumeTypes, bool> LightTypes { get; set; }
        public float SunriseHour { get; set; }
        public float SunsetHour { get; set; }
    }

    private void LoadVariables();
    protected override void LoadDefaultConfig();
    private void LoadConfigVariables();
     void SaveConfig(ConfigData config);
}

 class LightTypes
{
    public bool Campfires { get; set; }
    public bool Lanterns { get; set; }
    public bool CeilingLights { get; set; }
    public bool Furnaces { get; set; }
    public bool JackOLanterns { get; set; }
}

 class ConfigData
{
    public bool ConsumeFuel { get; set; }
    public Dictionary<ConsumeTypes, bool> LightTypes { get; set; }
    public float SunriseHour { get; set; }
    public float SunsetHour { get; set; }
}


```

---

## NightNoFog

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using Newtonsoft.Json;
using Oxide.Game.Rust.Cui;
using UnityEngine;

Oxide.Plugins
[Info("NightNoFog", "BaseCheaters", "1.0.0")]
 class NightNoFog : RustPlugin
{
     void OnServerInitialized();
}


```

---

## NoAdminOwnership

```csharp
using System.Collections.Generic;

Oxide.Plugins
[Info("NoAdminOwnership", "k1lly0u", "0.1.3", ResourceId = 2019)]
 class NoAdminOwnership : RustPlugin
{
    private List<ulong> itemOwnerDebug;
    private bool Initialized;
     void Loaded();
     void OnServerInitialized();
     void OnItemAddedToContainer(ItemContainer container, Item item);
     void OnItemRemovedFromContainer(ItemContainer container, Item item);
     void RemoveOwner(ulong ID, Item item);
    public void PrintOwnership(BasePlayer player, Item item);
    private bool isAllowed(BasePlayer player);
    private bool ignoreUser(BasePlayer player);
    private bool isDebug(BasePlayer player);
    [ChatCommand("ownership")]
    private void cmdOwnership(BasePlayer player, string command, string[] args);
    private string MSG(string key, string ID);
     Dictionary<string, string> messages;
    private ConfigData configData;
     class ConfigData
    {
        public bool IgnoreAuth2 { get; set; }
        public bool IgnoreAuth1 { get; set; }
        public bool IgnoreUsingPermission { get; set; }
    }

    private void LoadVariables();
    protected override void LoadDefaultConfig();
    private void LoadConfigVariables();
     void SaveConfig(ConfigData config);
}

 class ConfigData
{
    public bool IgnoreAuth2 { get; set; }
    public bool IgnoreAuth1 { get; set; }
    public bool IgnoreUsingPermission { get; set; }
}


```

---

## NoAnimals

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using UnityEngine;
using Random = UnityEngine.Random;
using Oxide.Core.Plugins;

Oxide.Plugins
[Info("NoAnimals", "Phraxxer", "0.0.1", ResourceId = 1337)]
[Description("This plugin removes all animals from your server.")]
 class NoAnimals : RustPlugin
{
    private bool serverInit;
    private List<string> animal_list;
    private bool checkEnt(BaseNetworkable entity);
     void OnServerInitialized();
     void OnEntitySpawned(BaseNetworkable entity);
}


```

---

## NoBarrels

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using UnityEngine;
using Random = UnityEngine.Random;
using Oxide.Core.Plugins;

Oxide.Plugins
[Info("NoBarrels", "Kyrah Abattoir", "0.1", ResourceId = 1872)]
[Description("This plugin removes all non-oil barrels on the server.")]
 class NoBarrels : RustPlugin
{
    private bool server_ready;
    private List<string> barrels;
    private bool ProcessBarrel(BaseNetworkable entity);
     void OnServerInitialized();
     void OnEntitySpawned(BaseNetworkable entity);
}


```

---

## NoBowRaid

```csharp
using UnityEngine;
using System.Collections.Generic;
using System.Linq;
using System;
using Oxide.Core;

Oxide.Plugins
[Info("NoBowRaid", "Bamabo", "1.0.1")]
[Description("Gets rid of one of the most broken mechanics in Rust")]
 class NoBowRaid : RustPlugin
{
    private List<string> whitelist;
    private bool adminBypass;
    private bool ignoreTwig;
    private bool usePermissions;
    private float damageScale;
     void Init();
     void OnEntityTakeDamage(BaseCombatEntity entity, HitInfo info);
    protected override void LoadDefaultConfig();
}


```

---

## NoBugs

```csharp
using UnityEngine;
using Rust;

Oxide.Plugins
[Info("NoBugs", "azalea`", "1.3", ResourceId = 1778)]
 class NoBugs : RustPlugin
{
     void OnServerInitialized();
     void OnPlayerInit(BasePlayer player);
     void OnPlayerDisconnected(BasePlayer player, string reason);
     void OnEntityBuilt(Planner plan, GameObject obj);
     void SetupObject(GameObject Object);
}


```

---

## NoBuildOnRoad

```csharp
using UnityEngine;
using System;
using System.Collections.Generic;
using Newtonsoft.Json;

Oxide.Plugins
[Info("No Build On Road", "supreme", "1.0.4")]
[Description("Disallow building over road")]
public class NoBuildOnRoad : RustPlugin
{
    const string NoBuildOnRoadIgnore;
    protected override void LoadDefaultMessages();
    private Configuration _config;
    private class Configuration
    {
        [JsonProperty(PropertyName = "Prefix")]
        public string usedPrefix;
        [JsonProperty(PropertyName = "Prefix Color")]
        public string usedPrefixColor;
        [JsonProperty(PropertyName = "Chat Icon [SteamID64]")]
        public ulong chatIcon;
    }

    protected override void LoadConfig();
    protected override void SaveConfig();
    protected override void LoadDefaultConfig();
     void OnEntityBuilt(Planner plan, GameObject go);
     void GiveRefund(BaseEntity entity, BasePlayer player);
     void Init();
     bool CheckRoad(Vector3 Position);
}

private class Configuration
{
    [JsonProperty(PropertyName = "Prefix")]
    public string usedPrefix;
    [JsonProperty(PropertyName = "Prefix Color")]
    public string usedPrefixColor;
    [JsonProperty(PropertyName = "Chat Icon [SteamID64]")]
    public ulong chatIcon;
}


```

---

## NoDecay

```csharp
using System;
using System.Collections.Generic;
using System.Text;
using Oxide.Core.Plugins;

Oxide.Plugins
[Info("NoDecay", "Deicide666ra/Piarb", "1.0.13", ResourceId = 1160)]
[Description("Scales or disables decay of items")]
 class NoDecay : RustPlugin
{
    private float c_twigMultiplier;
    private float c_woodMultiplier;
    private float c_stoneMultiplier;
    private float c_sheetMultiplier;
    private float c_armoredMultiplier;
    private float c_campfireMultiplier;
    private float c_highWoodWallMultiplier;
    private float c_highStoneWallMultiplier;
    private float c_barricadeMultiplier;
    private float c_trapMultiplier;
    private float c_deployablesMultiplier;
    private float c_boxMultiplier;
    private float c_furnaceMultiplier;
    private bool c_outputToRcon;
    private bool g_configChanged;
    private string entity_name;
     void Loaded();
    protected override void LoadDefaultConfig();
     void LoadConfigValues();
     object GetConfigValue(string category, string setting, object defaultValue);
    [HookMethod("SendHelpText")]
    private void SendHelpText(BasePlayer player);
     void OnEntityTakeDamage(BaseCombatEntity entity, HitInfo hitInfo);
     void ProcessCampfireDamage(HitInfo hitInfo);
     void ProcessBuildingDamage(BuildingBlock block, HitInfo hitInfo);
}


```

---

## NoDistanceLoot

```csharp
using Oxide.Core;
using Oxide.Core.Libraries;
using Oxide.Core.Plugins;
using System;
using System.Collections.Generic;
using System.Reflection;
using System.Linq;
using UnityEngine;
using System.Text;

Oxide.Plugins
[Info("NoDistanceLoot", "4seti [Lunatiq] for Rust Planet", "0.1.1", ResourceId = 000)]
public class NoDistanceLoot : RustPlugin
{
    private void Log(string message);
    private void Warn(string message);
    private void Error(string message);
     void Loaded();
     Dictionary<BasePlayer, string> looters;
    [HookMethod("OnPlayerLoot")]
     void OnPlayerLoot(PlayerLoot lootInventory, UnityEngine.Object entry);
    [HookMethod("OnPlayerSleepEnded")]
     void OnPlayerSleepEnded(BasePlayer player);
}


```

---

## NoDurability

```csharp
using System;

Oxide.Plugins
[Info("NoDurability", "Wulf/lukespragg", "2.0.0", ResourceId = 1061)]
public class NoDurability : CovalencePlugin
{
     void Loaded();
     bool HasPermission(string steamId, string perm);
}


```

---

## NoEscape

```csharp
using System.Collections.Generic;
using System.Linq;
using Newtonsoft.Json;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using Rust;
using UnityEngine;

Oxide.Plugins
[Info("NoEscape", "Hougan", "0.1.4")]
public class NoEscape : RustPlugin
{
    private static string Layer;
    private static List<Blocker> BlockerList;
    private class PlayerBlocked : MonoBehaviour
    {
        private BasePlayer Player;
        public Blocker CurrentBlocker;
        private float LastUpdate;
        private void Awake();
        private void Update();
        private void ControllerUpdate();
        private void BlockPlayer(Blocker blocker, bool justCreated);
        private const string UpdateLayer;
        public void UpdateUI();
        private void UnblockPlayer();
        private void OnDestroy();
    }

    private class Blocker : MonoBehaviour
    {
        public double CurrentTime;
        public double TotalTime;
        public BuildingPrivlidge Cupboard;
        public BaseEntity Alarm;
        public BaseEntity Light;
        public void Awake();
        public void Update();
        public void OnDestroy();
        public bool IsOwner(ulong userId);
        public bool IsInBlocker(BasePlayer player);
    }

    private void OnPlayerInit(BasePlayer player);
    private void Unload();
    private class Configuration
    {
        internal class BlockSetting
        {
            internal class BlockLimit
            {
                [JsonProperty("Можно ли строить? (фундаменты, стены (из соломы))")]
                public bool CanBuild;
                [JsonProperty("Можно ли размещать объекты? (спальники, двери и т.д.)")]
                public bool CanPlaceObjects;
                [JsonProperty("Можно ли чинить объекты (фундаменты, двери и т.д.)")]
                public bool CanRepair;
                [JsonProperty("Можно ли улучшать объекты (фундаменты, стены и т.д.)")]
                public bool CanUpgrade;
                [JsonProperty("Список запрещенных для использования команд")]
                public List<string> BlockedCommands;
            }

            [JsonProperty("Дистанция действия блокировки (в метрах)")]
            public float BlockerDistance;
            [JsonProperty("Время блокировки в секундах")]
            public float BlockLength;
            [JsonProperty("Создавать сигнальный фонарь при рейде")]
            public bool CreateLighter;
            [JsonProperty("Создавать звуковое оповещение при рейде (колонка)")]
            public bool CreateAlarm;
            [JsonProperty("Блокировать хозяев построек, даже если они вне зоны рейда")]
            public bool BlockOwnersIfNotInZone;
            [JsonProperty("Блокировать игрока, который вошёл в активную зону блокировки")]
            public bool ShouldBlockEnter;
            [JsonProperty("Пускать игроков без очереди, если игрока рейдят")]
            public bool ShouldByPassQueue;
            [JsonProperty("Разрешить игрокам ломать шкаф (сразу после разрушения шкафа рейд-блок спадёт)")]
            public bool CanDestroyCupBoard;
            [JsonProperty("Настройки ограничений игроков в рейд-блоке")]
            public BlockLimit BlockLimits;
        }

        internal class NotificationSetting
        {
            [JsonProperty("Цвет полосы в интерфейсе у РБ игроков")]
            public string InterfaceColor;
            [JsonProperty("Привилегия, игроки с которой могут установить оповещение о рейде")]
            public string PermissionToInstall;
            [JsonProperty("Текст уведомления о рейде для отправки")]
            public string NotificationText;
        }

        [JsonProperty("Настройка работы блокировки")]
        public BlockSetting BlockSettings;
        [JsonProperty("Настройка оповещения о рейде")]
        public NotificationSetting NotificationSettings;
        public static Configuration GetNewConfiguration();
    }

    private static Configuration Settings;
    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private bool? OnServerCommand(ConsoleSystem.Arg args);
    private bool? OnPlayerCommand(ConsoleSystem.Arg args, BasePlayer player);
    private string CupLayer;
    private void OnLootEntity(BasePlayer player, BaseEntity entity);
    [ConsoleCommand("UI_NoEscape")]
    private void CmdConsoleHandler(ConsoleSystem.Arg args);
    private void OnLootEntityEnd(BasePlayer player, BaseCombatEntity entity);
    private bool? OnEntityTakeDamage(BaseCombatEntity entity, HitInfo info);
    private void OnEntityDeath(BaseCombatEntity entity, HitInfo info);
    private bool? CanBypassQueue(Network.Connection connection);
    private void OnEntityBuilt(Planner plan, GameObject go);
    private bool? CanAffordUpgrade(BasePlayer player, BuildingBlock block, BuildingGrade.Enum grade);
    private bool? OnStructureRepair(BaseCombatEntity entity, BasePlayer player);
    [PluginReference]
    private Plugin VKBot;
    private static void NotifyOwners(BuildingPrivlidge building);
    private static NoEscape instance;
    private void OnServerInitialized();
    private bool IsBlocked(BasePlayer player);
    private bool IsRaidBlock(ulong userId);
    private bool IsRaidBlocked(BasePlayer player);
    private bool IsRaidBlocked(string player);
    private int ApiGetTime(ulong userId);
    private string CanTeleport(BasePlayer player);
    private string CanTrade(BasePlayer player);
}

private class PlayerBlocked : MonoBehaviour
{
    private BasePlayer Player;
    public Blocker CurrentBlocker;
    private float LastUpdate;
    private void Awake();
    private void Update();
    private void ControllerUpdate();
    private void BlockPlayer(Blocker blocker, bool justCreated);
    private const string UpdateLayer;
    public void UpdateUI();
    private void UnblockPlayer();
    private void OnDestroy();
}

private class Blocker : MonoBehaviour
{
    public double CurrentTime;
    public double TotalTime;
    public BuildingPrivlidge Cupboard;
    public BaseEntity Alarm;
    public BaseEntity Light;
    public void Awake();
    public void Update();
    public void OnDestroy();
    public bool IsOwner(ulong userId);
    public bool IsInBlocker(BasePlayer player);
}

private class Configuration
{
    internal class BlockSetting
    {
        internal class BlockLimit
        {
            [JsonProperty("Можно ли строить? (фундаменты, стены (из соломы))")]
            public bool CanBuild;
            [JsonProperty("Можно ли размещать объекты? (спальники, двери и т.д.)")]
            public bool CanPlaceObjects;
            [JsonProperty("Можно ли чинить объекты (фундаменты, двери и т.д.)")]
            public bool CanRepair;
            [JsonProperty("Можно ли улучшать объекты (фундаменты, стены и т.д.)")]
            public bool CanUpgrade;
            [JsonProperty("Список запрещенных для использования команд")]
            public List<string> BlockedCommands;
        }

        [JsonProperty("Дистанция действия блокировки (в метрах)")]
        public float BlockerDistance;
        [JsonProperty("Время блокировки в секундах")]
        public float BlockLength;
        [JsonProperty("Создавать сигнальный фонарь при рейде")]
        public bool CreateLighter;
        [JsonProperty("Создавать звуковое оповещение при рейде (колонка)")]
        public bool CreateAlarm;
        [JsonProperty("Блокировать хозяев построек, даже если они вне зоны рейда")]
        public bool BlockOwnersIfNotInZone;
        [JsonProperty("Блокировать игрока, который вошёл в активную зону блокировки")]
        public bool ShouldBlockEnter;
        [JsonProperty("Пускать игроков без очереди, если игрока рейдят")]
        public bool ShouldByPassQueue;
        [JsonProperty("Разрешить игрокам ломать шкаф (сразу после разрушения шкафа рейд-блок спадёт)")]
        public bool CanDestroyCupBoard;
        [JsonProperty("Настройки ограничений игроков в рейд-блоке")]
        public BlockLimit BlockLimits;
    }

    internal class NotificationSetting
    {
        [JsonProperty("Цвет полосы в интерфейсе у РБ игроков")]
        public string InterfaceColor;
        [JsonProperty("Привилегия, игроки с которой могут установить оповещение о рейде")]
        public string PermissionToInstall;
        [JsonProperty("Текст уведомления о рейде для отправки")]
        public string NotificationText;
    }

    [JsonProperty("Настройка работы блокировки")]
    public BlockSetting BlockSettings;
    [JsonProperty("Настройка оповещения о рейде")]
    public NotificationSetting NotificationSettings;
    public static Configuration GetNewConfiguration();
}

internal class BlockSetting
{
    internal class BlockLimit
    {
        [JsonProperty("Можно ли строить? (фундаменты, стены (из соломы))")]
        public bool CanBuild;
        [JsonProperty("Можно ли размещать объекты? (спальники, двери и т.д.)")]
        public bool CanPlaceObjects;
        [JsonProperty("Можно ли чинить объекты (фундаменты, двери и т.д.)")]
        public bool CanRepair;
        [JsonProperty("Можно ли улучшать объекты (фундаменты, стены и т.д.)")]
        public bool CanUpgrade;
        [JsonProperty("Список запрещенных для использования команд")]
        public List<string> BlockedCommands;
    }

    [JsonProperty("Дистанция действия блокировки (в метрах)")]
    public float BlockerDistance;
    [JsonProperty("Время блокировки в секундах")]
    public float BlockLength;
    [JsonProperty("Создавать сигнальный фонарь при рейде")]
    public bool CreateLighter;
    [JsonProperty("Создавать звуковое оповещение при рейде (колонка)")]
    public bool CreateAlarm;
    [JsonProperty("Блокировать хозяев построек, даже если они вне зоны рейда")]
    public bool BlockOwnersIfNotInZone;
    [JsonProperty("Блокировать игрока, который вошёл в активную зону блокировки")]
    public bool ShouldBlockEnter;
    [JsonProperty("Пускать игроков без очереди, если игрока рейдят")]
    public bool ShouldByPassQueue;
    [JsonProperty("Разрешить игрокам ломать шкаф (сразу после разрушения шкафа рейд-блок спадёт)")]
    public bool CanDestroyCupBoard;
    [JsonProperty("Настройки ограничений игроков в рейд-блоке")]
    public BlockLimit BlockLimits;
}

internal class BlockLimit
{
    [JsonProperty("Можно ли строить? (фундаменты, стены (из соломы))")]
    public bool CanBuild;
    [JsonProperty("Можно ли размещать объекты? (спальники, двери и т.д.)")]
    public bool CanPlaceObjects;
    [JsonProperty("Можно ли чинить объекты (фундаменты, двери и т.д.)")]
    public bool CanRepair;
    [JsonProperty("Можно ли улучшать объекты (фундаменты, стены и т.д.)")]
    public bool CanUpgrade;
    [JsonProperty("Список запрещенных для использования команд")]
    public List<string> BlockedCommands;
}

internal class NotificationSetting
{
    [JsonProperty("Цвет полосы в интерфейсе у РБ игроков")]
    public string InterfaceColor;
    [JsonProperty("Привилегия, игроки с которой могут установить оповещение о рейде")]
    public string PermissionToInstall;
    [JsonProperty("Текст уведомления о рейде для отправки")]
    public string NotificationText;
}


```

---

## NoEscape (1)

```csharp
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Configuration;
using Oxide.Core.Libraries;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using System;
using System.Collections.Generic;
using System.Drawing;
using System.Globalization;
using System.IO;
using System.Linq;
using UnityEngine;

Oxide.Plugins
[Info("NoEscape", "OxideBro", "1.0.71")]
 class NoEscape : RustPlugin
{
    [PluginReference]
     Plugin Clans;
     Plugin Friends;
     Plugin RustMap;
     Plugin VKBot;
     Plugin Duel;
     bool IsClanMember(ulong playerID, ulong targetID);
     bool IsFriends(ulong playerID, ulong friendId);
    private bool IsDuelPlayer(BasePlayer player);
     DateTime LNT;
     class Raid
    {
        public HashSet<ulong> owners;
        public HashSet<ulong> raiders;
        public Vector3 pos;
        public Raid(List<ulong> owners, List<ulong> raiders);
    }

     class DamageTimer
    {
        public List<ulong> owners;
        public int seconds;
        public DamageTimer(List<ulong> owners, int seconds);
    }

    static DynamicConfigFile config;
     float radius;
     int blockTime;
     int offlineOut;
     int blockAttackTime;
     bool blockAttack;
     int ownerBlockTime;
     bool useDamageScale;
     bool useVK;
     bool blockOwner;
     bool friedsAPI;
     bool clansAPI;
     bool canBuild;
     bool canRemove;
     bool canRemoveStan;
     bool canKits;
     bool canRepair;
     bool canUpgrade;
     bool canTeleport;
     bool EnabledGUI;
     bool EnabledGUITimer;
     bool canTrade;
     bool canRec;
     bool CanBuilt;
     bool CanBuiltNoEscape;
     bool LadderBuilding;
     float offlineScale;
     bool MsgAttB;
     string MsgAtt;
     float offlineScaleFriendsClans;
     string GUITimerAnchormin;
     string GUITimerAnchormax;
     string GUISendAnchormin;
     string GUISendAnchormax;
    private string formatMessage;
    public List<string> whitelistObject;
    private void LoadDefaultConfig();
    private void GetConfig(string menu, string Key, T var);
     Dictionary<ulong, double> timers;
     List<Raid> raids;
     List<DamageTimer> damageTimers;
    private Dictionary<BasePlayer, DateTime> CooldownsAttackDic;
    private double CooldownAttack;
    private string PERM_IGNORE;
    private string PERM_VK_NOTIFICATION;
     void OnEntityBuilt(Planner plan, GameObject go);
     void OnServerInitialized();
     void OnEntityDeath(BaseCombatEntity entity, HitInfo info, Item item);
     bool IsOnline(ulong id);
     Raid GetRaid(Vector3 pos, bool justCreated);
     void BlockPlayer(BasePlayer player, string mode, bool owner);
    private string GetFormatTime(double seconds);
    public static string FormatTime(TimeSpan time);
    private static string Format(int units, string form1, string form2, string form3);
     void Unload();
     void OnPlayerInit(BasePlayer player);
    private List<string> damageTypes;
     void OnEntityTakeDamage(BaseCombatEntity entity, HitInfo info, BaseCombatEntity victim);
     object OnStructureUpgrade(BaseCombatEntity entity, BasePlayer player, BuildingGrade.Enum grade);
     object OnStructureDemolish(BaseCombatEntity entity, BasePlayer player);
     object OnStructureRepair(BaseCombatEntity entity, BasePlayer player);
     object CanUpgrade(BasePlayer player);
     object CanBuild(Planner plan, Construction prefab);
     object CanTeleport(BasePlayer player);
     object CanTrade(BasePlayer player);
     object canRedeemKit(BasePlayer player);
     object CanRecycleCommand(BasePlayer player);
     object CanRemove(BasePlayer player, BaseEntity entity);
    [ChatCommand("ne")]
     void cmdChatRaid(BasePlayer player, string command, string[] args);
    private string TextReplace(string key, KeyValuePair<string, string>[] replacements);
     void SendOfflineMessage(ulong id, string raidername);
     void NoEscapeTimerHandle();
     void RaidZoneTimerHandle();
     List<BasePlayer> GetAroundPlayers(Vector3 position);
     List<ulong> GetOwnersByOwner(ulong owner, Vector3 position);
     string DrawGUIAnno;
     string GUItimer;
     void DrawUI(BasePlayer player, string name, string messag);
     void DrawUITimer(BasePlayer player);
     void DestroyUITimer(BasePlayer player);
     void DestroyUI(BasePlayer player);
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
    private Image raidhomeImage;
     void OnMapInitialized();
     TimeSpan TimeLeft(DateTime time);
     void InitImages();
     double ApiGetTime(ulong player);
     List<Vector3> ApiGetOwnerRaidZones(ulong uid);
     void OnServerSave();
     void LoadData();
     void SaveData();
     Dictionary<string, string> Messages;
    public static class PermissionService
    {
        public static Permission permission;
        public static bool HasPermission(ulong uid, string permissionName);
        public static void RegisterPermissions(Plugin owner, List<string> permissions);
    }

}

 class Raid
{
    public HashSet<ulong> owners;
    public HashSet<ulong> raiders;
    public Vector3 pos;
    public Raid(List<ulong> owners, List<ulong> raiders);
}

 class DamageTimer
{
    public List<ulong> owners;
    public int seconds;
    public DamageTimer(List<ulong> owners, int seconds);
}

private class Cooldown
{
    public ulong UserId;
    public double Expired;
    [JsonIgnore]
    public Action OnExpired;
}

public static class PermissionService
{
    public static Permission permission;
    public static bool HasPermission(ulong uid, string permissionName);
    public static void RegisterPermissions(Plugin owner, List<string> permissions);
}


```

---

## NoFuelRequirements

```csharp
using System.Collections.Generic;

Oxide.Plugins
[Info("NoFuelRequirements", "k1lly0u", "1.3.0", ResourceId = 1179)]
 class NoFuelRequirements : RustPlugin
{
     bool usingPermissions;
     void OnServerInitialized();
     void OnConsumeFuel(BaseOven oven, Item fuel, ItemModBurnable burnable);
     void OnConsumableUse(Item item, int amount);
     void RegisterPermissions();
     bool HasPermission(string ownerId, ConsumeTypes type);
     bool IsActiveType(ConsumeTypes type);
     ConsumeTypes StringToType(string name);
    private ConfigData configData;
     class ConfigData
    {
        public Dictionary<ConsumeTypes, bool> AffectedTypes { get; set; }
        public Dictionary<ConsumeTypes, string> Permissions { get; set; }
        public bool UsePermissions { get; set; }
    }

    private void LoadVariables();
    protected override void LoadDefaultConfig();
    private void LoadConfigVariables();
     void SaveConfig(ConfigData config);
}

 class ConfigData
{
    public Dictionary<ConsumeTypes, bool> AffectedTypes { get; set; }
    public Dictionary<ConsumeTypes, string> Permissions { get; set; }
    public bool UsePermissions { get; set; }
}


```

---

## NoGiveNotices

```csharp

Oxide.Plugins
[Info("NoGiveNotices", "Wulf/lukespragg", "0.1.0", ResourceId = 2336)]
[Description("Prevents admin item giving notices from showing in the chat")]
 class NoGiveNotices : RustPlugin
{
     object OnServerMessage(string m, string n);
}


```

---

## NoHeli

```csharp
using Oxide.Core;
using System;
using System.Reflection;
using System.Collections.Generic;
using UnityEngine;

Oxide.Plugins
[Info("NoHeli", "HoPollo", "1.0.1")]
 class NoHeli : RustPlugin
{
    private readonly string heliPrefab;
    private uint heliPrefabId;
     void OnEntitySpawned(BaseNetworkable entity);
}


```

---

## NoLoot

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using UnityEngine;
using Random = UnityEngine.Random;
using Oxide.Core.Plugins;

Oxide.Plugins
[Info("NoLoot", "Virobeast", "0.0.2", ResourceId = 1488)]
[Description("This plugin removes all loot from your server.")]
 class NoLoot : RustPlugin
{
    private bool server_ready;
    private List<string> barrels;
    private bool ProcessBarrel(BaseNetworkable entity);
     void OnServerInitialized();
     void OnEntitySpawned(BaseNetworkable entity);
}


```

---

## NoNight

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
[Info("NoNight", "BaK", "1.0.2", ResourceId = 1279)]
 class NoNight : RustPlugin
{
    public int sunsetHour;
    public int sunriseHour;
    [HookMethod("OnTick")]
    private void OnTick();
}


```

---

## NoRecoil

```csharp
using Newtonsoft.Json;
using System;
using System.Collections.Generic;
using UnityEngine;
using System.Linq;
using Oxide.Core;
using System.Text;

Oxide.Plugins
[Info("NoRecoil", "TopPlugin.ru/Sempai#3239", "0.1.7")]
[Description("Removing recoil from rapid-fire weapons")]
public class NoRecoil : RustPlugin
{
    const String PermissionAdmin;
    private string[] permittedWeapons;
    public Dictionary<UInt64, Dictionary<String, Boolean>> Users;
    private static Configuration config;
    private class Configuration
    {
        [JsonProperty("Weapon customization")]
        public RecoilWeapon recoilWeapon;
        internal class RecoilWeapon
        {
            [JsonProperty("Whether to add a prefix to the weapon")]
            public Boolean addPrefixGun;
            [JsonProperty("weapon prefix")]
            public String prefixGun;
            [JsonProperty("Item ShortName / settings")]
            public Dictionary<String, weaponModifer> settingsRecoil;
            internal class weaponModifer
            {
                [JsonProperty("Max Ammo (not more than 1000)")]
                public Int32 maxAmmo;
                [JsonProperty("Return percentage 100 - there will be no recoil. 50 - recoil reduced by 50%")]
                public Int32 recoilDeform;
                [JsonProperty("SkinId")]
                public ulong skinId;
            }

        }

        public static Configuration GetNewConfiguration();
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private new void LoadDefaultMessages();
    public static StringBuilder sb;
    public String GetLang(String LangKey, String userID, object[] args);
     void CheckValidator(BasePlayer player);
    private void OnPlayerConnected(BasePlayer player);
     void Unload();
    private void OnServerInitialized();
    private void Init();
    private void OnItemAddedToContainer(ItemContainer container, Item item);
    private void OnItemCraftFinished(ItemCraftTask task, Item item);
     object OnReloadWeapon(BasePlayer player, BaseProjectile projectile);
     object OnReloadMagazine(BasePlayer player, BaseProjectile projectile, int desiredAmount);
    [ConsoleCommand("nrg")]
     void GiveNoRecoilWeapon(ConsoleSystem.Arg arg);
    [ChatCommand("nrg")]
     void GiveNoRecoilWeaponCmd(BasePlayer player, string cmd, string[] arg);
    [ChatCommand("nr")]
     void NoRecoilWeaponCmd(BasePlayer player);
    public void SendChat(string Message, BasePlayer player);
     void CreateNoRecoilWeapon(BasePlayer player, String name);
    private void ModifiItemAll(Boolean unload);
    private void ModifiItem(Item item);
    private void ModifiItemDefault(Item item);
    private void CheckAndModifyItem(Item item, BasePlayer player, Boolean unload);
}

private class Configuration
{
    [JsonProperty("Weapon customization")]
    public RecoilWeapon recoilWeapon;
    internal class RecoilWeapon
    {
        [JsonProperty("Whether to add a prefix to the weapon")]
        public Boolean addPrefixGun;
        [JsonProperty("weapon prefix")]
        public String prefixGun;
        [JsonProperty("Item ShortName / settings")]
        public Dictionary<String, weaponModifer> settingsRecoil;
        internal class weaponModifer
        {
            [JsonProperty("Max Ammo (not more than 1000)")]
            public Int32 maxAmmo;
            [JsonProperty("Return percentage 100 - there will be no recoil. 50 - recoil reduced by 50%")]
            public Int32 recoilDeform;
            [JsonProperty("SkinId")]
            public ulong skinId;
        }

    }

    public static Configuration GetNewConfiguration();
}

internal class RecoilWeapon
{
    [JsonProperty("Whether to add a prefix to the weapon")]
    public Boolean addPrefixGun;
    [JsonProperty("weapon prefix")]
    public String prefixGun;
    [JsonProperty("Item ShortName / settings")]
    public Dictionary<String, weaponModifer> settingsRecoil;
    internal class weaponModifer
    {
        [JsonProperty("Max Ammo (not more than 1000)")]
        public Int32 maxAmmo;
        [JsonProperty("Return percentage 100 - there will be no recoil. 50 - recoil reduced by 50%")]
        public Int32 recoilDeform;
        [JsonProperty("SkinId")]
        public ulong skinId;
    }

}

internal class weaponModifer
{
    [JsonProperty("Max Ammo (not more than 1000)")]
    public Int32 maxAmmo;
    [JsonProperty("Return percentage 100 - there will be no recoil. 50 - recoil reduced by 50%")]
    public Int32 recoilDeform;
    [JsonProperty("SkinId")]
    public ulong skinId;
}


```

---

## NoSigns

```csharp
using UnityEngine;

Oxide.Plugins
[Info("No Signs", "bawNg", 0.4)]
 class NoSigns : RustPlugin
{
     string notAllowedMessage;
     void Loaded();
     void OnItemAddedToContainer(ItemContainer container, Item item);
     object OnCanCraft(ItemCrafter item_crafter, ItemBlueprint blueprint, int amount);
     void OnEntityBuilt(Planner planner, GameObject game_object);
}


```

---

## NoSkins-1.0.1

```csharp
using System.Collections;
using System.Collections.Generic;
using Facepunch;
using ProtoBuf;
using Network;
using Oxide.Core.Libraries.Covalence;
using System.Linq;
using HarmonyLib;
using Oxide.Core.Plugins;
using Newtonsoft.Json;
using UnityEngine;
using System.IO;
using Oxide.Core;

Oxide.Plugins
[Info("NoSkins", "Whispers88", "1.0.1")]
[Description("Allows you to disable other players worn skins")]
public class NoSkins : RustPlugin
{
    private static HashSet<ulong> _HideSkinsPlayers;
    private const string PermAllow;
    private const string PermHideSkins;
    private Configuration config;
     class Configuration
    {
        [JsonProperty("Command Aliases")]
        public string[] noskinCMD;
        public string ToJson();
        public Dictionary<string, object> ToDictionary();
    }

    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    protected override void SaveConfig();
    private void Init();
    private void Unload();
    private void NoSkinCMD(IPlayer iplayer, string command, string[] args);
    protected override void LoadDefaultMessages();
    private string GetLang(string langKey, string playerId);
    private void Message(BasePlayer player, string langKey);
    private void OnPlayerConnected(BasePlayer player);
    private void OnPlayerDisconnected(BasePlayer player, string reason);
    private object? OnInventoryNetworkUpdate(PlayerInventory instance, ItemContainer container, UpdateItemContainer updateItemContainer, PlayerInventory.Type type, PlayerInventory.NetworkInventoryMode mode);
    private void UpdateCurrentConnections(BasePlayer player, bool disableSkins);
    private static Dictionary<ulong, ProtoBuf.ItemContainer> _cachedContainers;
    private static void CreateAndSendUpdateItemContainer(BasePlayer player, BasePlayer current, ItemContainer container, PlayerInventory.Type type, bool disableSkins);
    private void SendUpdates(PlayerInventory playerInventory, UpdateItemContainer updateItemContainer, bool updateLocal);
    [HarmonyPatch(typeof(PlayerCorpse), "Save"), AutoPatch]
    private static class PlayerCorpse_Save_Patch
    {
        [HarmonyPostfix]
        private static void Postfix(PlayerCorpse __instance, BaseNetworkable.SaveInfo info);
    }

    [HarmonyPatch(typeof(BaseNetworkable), "SendNetworkGroupChange"), AutoPatch]
    private static class BaseNetworkable_SendNetworkGroupChange_Patch
    {
        [HarmonyPostfix]
        private static void Postfix(BaseNetworkable __instance);
    }

    [HarmonyPatch(typeof(Networkable), "OnGroupTransition"), AutoPatch]
    private static class BaseNetworkable_OnGroupTransition_Patch
    {
        [HarmonyPostfix]
        private static void Postfix(Networkable __instance, Network.Visibility.Group oldGroup);
    }

    private bool HasPerm(string id, string perm);
    private void AddLocalizedCommand(string command);
}

 class Configuration
{
    [JsonProperty("Command Aliases")]
    public string[] noskinCMD;
    public string ToJson();
    public Dictionary<string, object> ToDictionary();
}

[HarmonyPatch(typeof(PlayerCorpse), "Save"), AutoPatch]
private static class PlayerCorpse_Save_Patch
{
    [HarmonyPostfix]
    private static void Postfix(PlayerCorpse __instance, BaseNetworkable.SaveInfo info);
}

[HarmonyPatch(typeof(BaseNetworkable), "SendNetworkGroupChange"), AutoPatch]
private static class BaseNetworkable_SendNetworkGroupChange_Patch
{
    [HarmonyPostfix]
    private static void Postfix(BaseNetworkable __instance);
}

[HarmonyPatch(typeof(Networkable), "OnGroupTransition"), AutoPatch]
private static class BaseNetworkable_OnGroupTransition_Patch
{
    [HarmonyPostfix]
    private static void Postfix(Networkable __instance, Network.Visibility.Group oldGroup);
}


```

---

## NoSleepers

```csharp
using System;
using Rust;

Oxide.Plugins
[Info("NoSleepers", "Wulf/lukespragg", "0.4.1", ResourceId = 1452)]
[Description("Prevents players from sleeping and optionally removes player corpses")]
 class NoSleepers : CovalencePlugin
{
    const string permExclude;
     bool killExisting;
     bool removeCorpses;
    protected override void LoadDefaultConfig();
     void OnServerInitialized();
     void OnPlayerInit(BasePlayer player);
     void OnPlayerRespawned(BasePlayer player);
     void OnPlayerDisconnected(BasePlayer player);
     void OnEntitySpawned(BaseNetworkable entity);
     T GetConfig(string name, T value);
}


```

---

## NoStashBug

```csharp
using UnityEngine;

Oxide.Plugins
[Info("NoStashBug", "azalea`", "1.1")]
 class NoStashBug : RustPlugin
{
    static int PreventBuilding;
     void OnServerInitialized();
     void OnEntityBuilt(Planner plan, GameObject obj);
}


```

---

## NoSteamDevRust

```csharp
using Network;

Oxide.Plugins
[Info("AutoInsecure", "Zirper", "1.0.0")]
 class NoSteamDevRust : RustPlugin
{
     void OnClientAuth(Connection connection);
}


```

---

## NoSteamHelper

```csharp
using System.Collections.Generic;
using Network;
using Oxide.Core;

Oxide.Plugins
[Info("NoSteamHelper", "Kaidoz", "1.0.2")]
[Description("")]
internal class NoSteamHelper : RustPlugin
{
    private static List<DataPlayer> _players;
    private bool _loaded;
    private object IsPlayerNoSteam(ulong steamid);
    private void InitData();
    private static void SaveData();
    public class DataPlayer
    {
        public bool Steam;
        public ulong SteamId;
        public DataPlayer(ulong id, bool steam);
        public static void AddPlayer(ulong id, bool steam);
        public static bool FindPlayer(ulong steamid, DataPlayer dataPlayer);
        public bool IsSteam();
    }

    private void OnServerInitialized(bool loaded);
    private object OnSteamAuthFailed(Connection connection);
    private void OnPlayerInit(BasePlayer player);
}

public class DataPlayer
{
    public bool Steam;
    public ulong SteamId;
    public DataPlayer(ulong id, bool steam);
    public static void AddPlayer(ulong id, bool steam);
    public static bool FindPlayer(ulong steamid, DataPlayer dataPlayer);
    public bool IsSteam();
}


```

---

## NoSunGlare

```csharp
using Newtonsoft.Json;
using Oxide.Core.Plugins;
using Oxide.Plugins;
using UnityEngine;

Oxide.Plugins
[Info("No Sun Glare", "Tryhard", "2.0.0")]
[Description("Removes sun or sun glare")]
public class NoSunGlare : RustPlugin
{
    private WeatherConfig _config;
    public class WeatherConfig
    {
        [JsonProperty(PropertyName = "Clouds")]
        public float Clouds { get; set; }
        [JsonProperty(PropertyName = "Cloud Opacity")]
        public float CloudOpacity { get; set; }
        [JsonProperty(PropertyName = "Cloud Brightness")]
        public float CloudBrightness { get; set; }
        [JsonProperty(PropertyName = "Cloud Coloring")]
        public int CloudColoring { get; set; }
        [JsonProperty(PropertyName = "Cloud Saturation")]
        public int CloudSaturation { get; set; }
        [JsonProperty(PropertyName = "Cloud Scattering")]
        public int CloudScattering { get; set; }
        [JsonProperty(PropertyName = "Cloud Sharpness")]
        public int CloudSharpness { get; set; }
        [JsonProperty(PropertyName = "Cloud Size")]
        public int CloudSize { get; set; }
        [JsonProperty(PropertyName = "Cloud Coverage")]
        public int CloudCoverage { get; set; }
        [JsonProperty(PropertyName = "Cloud Attenuation")]
        public int CloudAttenuation { get; set; }
        [JsonProperty(PropertyName = "Wind")]
        public float Wind { get; set; }
        [JsonProperty(PropertyName = "Rain")]
        public float Rain { get; set; }
        [JsonProperty(PropertyName = "Fog")]
        public float Fog { get; set; }
        [JsonProperty(PropertyName = "Fogginess")]
        public float Fogginess { get; set; }
        [JsonProperty(PropertyName = "Dust Chance")]
        public float DustChance { get; set; }
        [JsonProperty(PropertyName = "Fog Chance")]
        public float FogChance { get; set; }
        [JsonProperty(PropertyName = "Overcast Chance")]
        public float OvercastChance { get; set; }
        [JsonProperty(PropertyName = "Storm Chance")]
        public float StormChance { get; set; }
        [JsonProperty(PropertyName = "Clear Chance")]
        public float ClearChance { get; set; }
        [JsonProperty(PropertyName = "Atmosphere Contrast")]
        public float AtmosphereContrast { get; set; }
        [JsonProperty(PropertyName = "Atmosphere Directionality")]
        public float AtmosphereDirectionality { get; set; }
        [JsonProperty(PropertyName = "Atmosphere Mie")]
        public float AtmosphereMie { get; set; }
        [JsonProperty(PropertyName = "Atmosphere Rayleigh")]
        public float AtmosphereRayleigh { get; set; }
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private void OnServerInitialized();
    private void SetWeatherParameters(float clouds, float cloudOpacity, float cloudBrightness, int cloudColoring, int cloudSaturation, int cloudScattering, int cloudSharpness, int cloudSize, int cloudCoverage, int cloudAttenuation, float wind, float rain, float fog, float fogginess, float dustChance, float fogChance, float overcastChance, float stormChance, float clearChance, float atmosphereContrast, float atmosphereDirectionality, float atmosphereMie, float atmosphereRayleigh);
}

public class WeatherConfig
{
    [JsonProperty(PropertyName = "Clouds")]
    public float Clouds { get; set; }
    [JsonProperty(PropertyName = "Cloud Opacity")]
    public float CloudOpacity { get; set; }
    [JsonProperty(PropertyName = "Cloud Brightness")]
    public float CloudBrightness { get; set; }
    [JsonProperty(PropertyName = "Cloud Coloring")]
    public int CloudColoring { get; set; }
    [JsonProperty(PropertyName = "Cloud Saturation")]
    public int CloudSaturation { get; set; }
    [JsonProperty(PropertyName = "Cloud Scattering")]
    public int CloudScattering { get; set; }
    [JsonProperty(PropertyName = "Cloud Sharpness")]
    public int CloudSharpness { get; set; }
    [JsonProperty(PropertyName = "Cloud Size")]
    public int CloudSize { get; set; }
    [JsonProperty(PropertyName = "Cloud Coverage")]
    public int CloudCoverage { get; set; }
    [JsonProperty(PropertyName = "Cloud Attenuation")]
    public int CloudAttenuation { get; set; }
    [JsonProperty(PropertyName = "Wind")]
    public float Wind { get; set; }
    [JsonProperty(PropertyName = "Rain")]
    public float Rain { get; set; }
    [JsonProperty(PropertyName = "Fog")]
    public float Fog { get; set; }
    [JsonProperty(PropertyName = "Fogginess")]
    public float Fogginess { get; set; }
    [JsonProperty(PropertyName = "Dust Chance")]
    public float DustChance { get; set; }
    [JsonProperty(PropertyName = "Fog Chance")]
    public float FogChance { get; set; }
    [JsonProperty(PropertyName = "Overcast Chance")]
    public float OvercastChance { get; set; }
    [JsonProperty(PropertyName = "Storm Chance")]
    public float StormChance { get; set; }
    [JsonProperty(PropertyName = "Clear Chance")]
    public float ClearChance { get; set; }
    [JsonProperty(PropertyName = "Atmosphere Contrast")]
    public float AtmosphereContrast { get; set; }
    [JsonProperty(PropertyName = "Atmosphere Directionality")]
    public float AtmosphereDirectionality { get; set; }
    [JsonProperty(PropertyName = "Atmosphere Mie")]
    public float AtmosphereMie { get; set; }
    [JsonProperty(PropertyName = "Atmosphere Rayleigh")]
    public float AtmosphereRayleigh { get; set; }
}


```

---

## NoteUI

```csharp
using System;
using System.Globalization;
using Newtonsoft.Json;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using UnityEngine;

Oxide.Plugins
[Info("NoteUI", "noname", "1.0.1")]
public class NoteUI : RustPlugin
{
    [PluginReference]
    private Plugin ImageLibrary;
     string NoteUIHandler;
    public class DataConfig
    {
        [JsonProperty("Иконка на эффект 'Взрыва'")]
        public string explosioneffecticon;
        [JsonProperty("Иконка на эффект 'Информация'")]
        public string infoeffecticon;
        [JsonProperty("Иконка на эффект 'Заблокировано'")]
        public string lockeffecticon;
        [JsonProperty("Включить звук при получении уведомления? (false - нет)")]
        public bool usesounds;
        [JsonProperty("Время постепенного появления")]
        public float fadein;
        [JsonProperty("Время через которое оповещение будет удалено")]
        public int timetodelete;
    }

    public DataConfig cfg;
    protected override void LoadConfig();
    protected override void SaveConfig();
    protected override void LoadDefaultConfig();
    [HookMethod("DrawExplosionNote")]
    public void DrawExplosionNote(BasePlayer player, string Text, string Description);
    [HookMethod("DrawLockNote")]
    public void DrawLockNote(BasePlayer player, string Name, string Description);
    [HookMethod("DrawInfoNote")]
    public void DrawInfoNote(BasePlayer player, string Text);
     void OnServerInitialized();
     void Unload();
    private void NoteUIAdd(BasePlayer player, string Type, string Name, string Description);
    private static string HexToCuiColor(string hex);
}

public class DataConfig
{
    [JsonProperty("Иконка на эффект 'Взрыва'")]
    public string explosioneffecticon;
    [JsonProperty("Иконка на эффект 'Информация'")]
    public string infoeffecticon;
    [JsonProperty("Иконка на эффект 'Заблокировано'")]
    public string lockeffecticon;
    [JsonProperty("Включить звук при получении уведомления? (false - нет)")]
    public bool usesounds;
    [JsonProperty("Время постепенного появления")]
    public float fadein;
    [JsonProperty("Время через которое оповещение будет удалено")]
    public int timetodelete;
}


```

---

## Notice

```csharp
using System.Collections.Generic;
using System.Reflection;
using System;
using System.Data;
using UnityEngine;
using Oxide.Core;

Oxide.Plugins
[Info("Notice", "LaserHydra", "1.0.1", ResourceId = 1193)]
[Description("Notice players anonymously")]
 class Notice : RustPlugin
{
     void Loaded();
    protected override void LoadDefaultConfig();
    [ChatCommand("notice")]
     void cmdNotice(BasePlayer player, string cmd, string[] args);
     BasePlayer GetPlayer(string searchedPlayer, BasePlayer executer, string prefix);
     string ArrayToString(string[] array, int first);
     void BroadcastChat(string prefix, string msg);
     void SendChatMessage(BasePlayer player, string prefix, string msg);
}


```

---

## Notifications

```csharp
using System;
using System.Collections.Generic;
using System.Globalization;
using System.Linq;
using Newtonsoft.Json;
using Newtonsoft.Json.Converters;
using Oxide.Core;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using UnityEngine;
using WebSocketSharp;

Oxide.Plugins
[Info("Notifications", "Sempai#3239", "1.0.1")]
[Description("Adds an notification system")]
public class Notifications : RustPlugin
{
    [PluginReference]
    private Plugin ImageLibrary;
    private static Notifications _instance;
    private const string Layer;
    private static Configuration _config;
    private class Configuration
    {
        [JsonProperty(PropertyName = "Настройка интерфейса | Interface Settings")]
        public IBackground Background;
    }

    private class IText : InterfacePosition
    {
        [JsonProperty(PropertyName = "Font Size")]
        public int FontSize;
        [JsonProperty(PropertyName = "Font")]
        public string Font;
        [JsonProperty(PropertyName = "Align")]
        [JsonConverter(typeof(StringEnumConverter))]
        public TextAnchor Align;
        [JsonProperty(PropertyName = "Text Color")]
        public IColor Color;
        public void Get(CuiElementContainer container, string parent, string name, string text);
    }

    private abstract class IAnchors
    {
        public string AnchorMin;
        public string AnchorMax;
    }

    private class IBackground : IAnchors
    {
        [JsonProperty(PropertyName = "Высота | Height")]
        public float Height;
        [JsonProperty(PropertyName = "Ширина | Width")]
        public float Width;
        [JsonProperty(PropertyName = "Отступ | Margin")]
        public float Margin;
        [JsonProperty(PropertyName = "Цвет | Color")]
        public IColor Color;
        [JsonProperty(PropertyName = "Иконка | Icon")]
        public InterfacePosition Icon;
        [JsonProperty(PropertyName = "Заглавие | Title")]
        public IText Title;
        [JsonProperty(PropertyName = "Описание | Description")]
        public IText Description;
        [JsonProperty(PropertyName = "Закрыть | Close")]
        public IText Close;
    }

    private class InterfacePosition : IAnchors
    {
        public string OffsetMin;
        public string OffsetMax;
    }

    private class IColor
    {
        [JsonProperty(PropertyName = "HEX")]
        public string HEX;
        [JsonProperty(PropertyName = "Непрозрачность | Opacity (0 - 100)")]
        public float Alpha;
        public string Get();
        public IColor(string hex, float alpha);
    }

    protected override void LoadConfig();
    protected override void SaveConfig();
    protected override void LoadDefaultConfig();
    private PluginData _data;
    private void SaveData();
    private void LoadData();
    private class PluginData
    {
        [JsonProperty(PropertyName = "Image List", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public Dictionary<string, string> Images;
    }

    private void Init();
    private void OnServerInitialized();
    private void Unload();
    [ConsoleCommand("UI_Notifications")]
    private void CmdConsoleNotify(ConsoleSystem.Arg arg);
    private class UiNotify
    {
        public readonly string GUID;
        public float TimeLeft;
        public readonly string Title;
        public readonly string Description;
        public readonly string Image;
        public readonly CuiElementContainer Container;
        public UiNotify(string guid, float delay, string title, string description, string image, CuiElementContainer cont);
    }

    private class NotifyComponent : FacepunchBehaviour
    {
        private BasePlayer _player;
        private readonly List<UiNotify> _notifies;
        private const float _timer;
        private void Awake();
        private void TimeHandle();
        private void RefreshUi();
        public void RemoveByGuid(string guid);
        public void RemoveByIndex(int index);
        public void AddNotify(UiNotify notyify);
        private void OnDestroy();
        public void Kill();
    }

    private void AddImage(string image, string url);
    private void ShowNotify(ulong user, float delay, string title, string description, string image, CuiElementContainer container);
    private void RemoveNotify(BasePlayer player, string guid);
    private string ShowNotify(BasePlayer player, float delay, string title, string description, string image, CuiElementContainer container);
}

private class Configuration
{
    [JsonProperty(PropertyName = "Настройка интерфейса | Interface Settings")]
    public IBackground Background;
}

private class IText : InterfacePosition
{
    [JsonProperty(PropertyName = "Font Size")]
    public int FontSize;
    [JsonProperty(PropertyName = "Font")]
    public string Font;
    [JsonProperty(PropertyName = "Align")]
    [JsonConverter(typeof(StringEnumConverter))]
    public TextAnchor Align;
    [JsonProperty(PropertyName = "Text Color")]
    public IColor Color;
    public void Get(CuiElementContainer container, string parent, string name, string text);
}

private abstract class IAnchors
{
    public string AnchorMin;
    public string AnchorMax;
}

private class IBackground : IAnchors
{
    [JsonProperty(PropertyName = "Высота | Height")]
    public float Height;
    [JsonProperty(PropertyName = "Ширина | Width")]
    public float Width;
    [JsonProperty(PropertyName = "Отступ | Margin")]
    public float Margin;
    [JsonProperty(PropertyName = "Цвет | Color")]
    public IColor Color;
    [JsonProperty(PropertyName = "Иконка | Icon")]
    public InterfacePosition Icon;
    [JsonProperty(PropertyName = "Заглавие | Title")]
    public IText Title;
    [JsonProperty(PropertyName = "Описание | Description")]
    public IText Description;
    [JsonProperty(PropertyName = "Закрыть | Close")]
    public IText Close;
}

private class InterfacePosition : IAnchors
{
    public string OffsetMin;
    public string OffsetMax;
}

private class IColor
{
    [JsonProperty(PropertyName = "HEX")]
    public string HEX;
    [JsonProperty(PropertyName = "Непрозрачность | Opacity (0 - 100)")]
    public float Alpha;
    public string Get();
    public IColor(string hex, float alpha);
}

private class PluginData
{
    [JsonProperty(PropertyName = "Image List", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public Dictionary<string, string> Images;
}

private class UiNotify
{
    public readonly string GUID;
    public float TimeLeft;
    public readonly string Title;
    public readonly string Description;
    public readonly string Image;
    public readonly CuiElementContainer Container;
    public UiNotify(string guid, float delay, string title, string description, string image, CuiElementContainer cont);
}

private class NotifyComponent : FacepunchBehaviour
{
    private BasePlayer _player;
    private readonly List<UiNotify> _notifies;
    private const float _timer;
    private void Awake();
    private void TimeHandle();
    private void RefreshUi();
    public void RemoveByGuid(string guid);
    public void RemoveByIndex(int index);
    public void AddNotify(UiNotify notyify);
    private void OnDestroy();
    public void Kill();
}


```

---

## NoWaterBuild

```csharp
using System;
using System.Collections.Generic;
using UnityEngine;

Oxide.Plugins
[Info("NoWaterBuild", "Jakkee", "0.1", ResourceId = 000)]
 class NoWaterBuild : RustPlugin
{
    private string PermBypass;
    private double Depth;
    private bool Refund;
    protected override void LoadDefaultConfig();
     void Loaded();
     void CheckConfig();
    protected void ReloadConfig();
     Dictionary<string, string> messages;
     void OnEntityBuilt(Planner planner, GameObject gameobject);
    private T GetConfig(string name, T defaultValue);
     string Lang(string key, string id, object[] args);
     bool HasPermission(string id, string perm);
}


```

---

## NoWeaponDrop

```csharp
using System;
using System.Collections.Generic;
using Oxide.Core.Plugins;
using UnityEngine;

Oxide.Plugins
[Info("NoWeaponDrop", "Fujikura", "0.2.3", ResourceId = 1960)]
[Description("Prevents dropping of active weapon on wounded or headshot/instant-kill")]
 class NoWeaponDrop : RustPlugin
{
    [PluginReference]
     Plugin RestoreUponDeath;
    private bool Changed;
    private bool usePermission;
    private string permissionName;
    private bool disableForROD;
    private object GetConfig(string menu, string datavalue, object defaultValue);
     void LoadVariables();
    protected override void LoadDefaultConfig();
     void Loaded();
     void OnItemRemovedFromContainer(ItemContainer container, Item item);
     object OnEntityTakeDamage(BaseCombatEntity entity, HitInfo info);
}


```

---

## NoWorkbench

```csharp
using Newtonsoft.Json;
using System.Linq;
using System.Collections.Generic;

Oxide.Plugins
[Info("NoWorkbench", "k1lly0u", "0.1.51")]
[Description("Eliminates the requirement of being near a bench to craft")]
 class NoWorkbench : RustPlugin
{
    private Dictionary<int, int> defaultBlueprints;
    private void OnServerInitialized();
    private void OnPlayerConnected(BasePlayer player);
    private void UnlockAllBlueprints(BasePlayer player);
    private void Unload();
    private ConfigData configData;
    private class ConfigData
    {
        [JsonProperty(PropertyName = "Disable the need for blueprints")]
        public bool NoBlueprints { get; set; }
    }

    private void LoadVariables();
    protected override void LoadDefaultConfig();
    private void LoadConfigVariables();
    private void SaveConfig(ConfigData config);
}

private class ConfigData
{
    [JsonProperty(PropertyName = "Disable the need for blueprints")]
    public bool NoBlueprints { get; set; }
}


```

---

## NPCGifts

```csharp
using System;
using System.Collections.Generic;
using UnityEngine;
using System.Linq;

Oxide.Plugins
[Info("NPCGifts", "Mabel", "1.0.2")]
 class NPCGifts : RustPlugin
{
    private class ContainerData
    {
        public string Prefab { get; set; }
        public float SpawnChance { get; set; }
        public bool IsEnabled { get; set; }
        public string Permission { get; set; }
    }

    private class CooldownData
    {
        public ulong PlayerID { get; set; }
        public DateTime LastSpawnTime { get; set; }
    }

    private List<ContainerData> containerList;
    private Dictionary<ulong, CooldownData> cooldowns;
    private List<BaseEntity> spawnedContainers;
    private float cooldownDurationMinutes;
    private System.Random random;
    private string containerReceivedMessage;
    protected override void LoadDefaultConfig();
     void Init();
    private void CheckAndCreatePermissions();
     void LoadConfigValues();
    private void OnEntityDeath(BaseCombatEntity entity, HitInfo hitInfo);
    private void SpawnRandomContainer(Vector3 position, ulong playerID);
    private void SendChatMessage(BasePlayer player, string message);
    private bool HasCooldown(ulong playerID);
    private void SetCooldown(ulong playerID);
    private string ReplacePlaceholders(string message, BasePlayer player);
    private void CleanupContainers();
     void Unload();
}

private class ContainerData
{
    public string Prefab { get; set; }
    public float SpawnChance { get; set; }
    public bool IsEnabled { get; set; }
    public string Permission { get; set; }
}

private class CooldownData
{
    public ulong PlayerID { get; set; }
    public DateTime LastSpawnTime { get; set; }
}


```

---

## NPCGrenades

```csharp
using Facepunch;
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;
using Oxide.Core;
using Oxide.Core.Libraries.Covalence;
using Oxide.Core.Plugins;
using Rust;
using System;
using System.Linq;
using System.Collections;
using System.Globalization;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.AI;

Oxide.Plugins
[Info("NPC Grenades", "Sempai#3239", "1.2.2")]
[Description("F1 grenades spawn various NPCs when thrown.")]
public class NPCGrenades : CovalencePlugin
{
    [PluginReference]
    private Plugin Friends;
    private Plugin Clans;
    private Plugin Kits;
    private static NPCGrenades plugin;
    private static System.Random random;
    private static VersionNumber previousVersion;
    public const int f1GrenadeId;
    public const ulong scientistSkinID;
    public const ulong heavySkinID;
    public const ulong juggernautSkinID;
    public const ulong tunnelSkinID;
    public const ulong underwaterSkinID;
    public const ulong murdererSkinID;
    public const ulong scarecrowSkinID;
    public const ulong mummySkinID;
    public const ulong bearSkinID;
    public const ulong polarbearSkinID;
    public const ulong wolfSkinID;
    public const ulong boarSkinID;
    public const ulong stagSkinID;
    public const ulong chickenSkinID;
    public const ulong bradleySkinID;
    public const string scientistPrefab;
    public const string heavyPrefab;
    public const string scarecrowPrefab;
    public const string bearPrefab;
    public const string polarbearPrefab;
    public const string wolfPrefab;
    public const string boarPrefab;
    public const string stagPrefab;
    public const string chickenPrefab;
    public const string bradleyPrefab;
    public const string murdererChatter;
    public const string murdererDeath;
    public const string fleshBloodImpact;
    public const string explosionSound;
    public const string bradleyExplosion;
    public const string permScientist;
    public const string permHeavy;
    public const string permJuggernaut;
    public const string permTunnel;
    public const string permUnderwater;
    public const string permMurderer;
    public const string permScarecrow;
    public const string permMummy;
    public const string permBear;
    public const string permPolarbear;
    public const string permWolf;
    public const string permBoar;
    public const string permStag;
    public const string permChicken;
    public const string permBradley;
    public const string permAdmin;
    protected override void LoadDefaultMessages();
    private string GetMessage(string messageKey, string playerID, object[] args);
    private void Message(IPlayer player, string messageKey, object[] args);
    private void Message(BasePlayer player, string messageKey, object[] args);
    private void OnServerInitialized();
    private void Init();
    private void Unload();
    private void OnServerSave();
    private void OnEntityDeath(BaseCombatEntity entity, HitInfo info);
    private void OnItemAddedToContainer(ItemContainer container, Item item);
    private void OnExplosiveFuseSet(TimedExplosive explosive, float fuseLength);
    private void OnNpcNadeThrown(BaseEntity thrower, BaseEntity entity, Item npcNade);
    private void OnExplosiveDropped(BasePlayer player, BaseEntity entity, Item item);
    private object OnEntityTakeDamage(BaseCombatEntity entity, HitInfo info);
    private object OnNpcTarget(BaseEntity entity, BaseEntity target);
    private object OnTurretTarget(AutoTurret turret, BaseEntity target);
    private object CanBradleyApcTarget(BradleyAPC bradley, BaseEntity target);
    private object OnBradleyApcInitialize(BradleyAPC bradley);
     void OnEntitySpawned(NPCPlayerCorpse corpse);
    private void SpawnScientist(BasePlayer player, Item npcNade, Vector3 position, NPCPlayerData settings);
    private void SpawnScarecrow(BasePlayer player, Item npcNade, Vector3 position, NPCPlayerData settings);
    private void SpawnAnimal(BasePlayer player, Item npcNade, Vector3 position);
    private void SpawnBradley(BasePlayer player, Item npcNade, Vector3 position);
    private void DespawnNPC(BaseEntity npc, ulong botId, float despawnTime);
    private void CancelHit(HitInfo info);
    private object OnNpcKits(ulong npcUserID);
    private void GiveNade(BasePlayer player, ulong skinId, string nadeName, string reason);
    public Item GiveInventoryItem(ItemContainer itemContainer, string shortName, int itemAmount, ulong skinId);
    private bool HasPermission(BasePlayer player, Item npcNade);
    private bool IsNpcGrenade(ulong skinId);
    public Vector3 GetNavPoint(Vector3 position);
    private bool IsInSafeZone(Vector3 position);
    private bool IsOnStructure(Vector3 position);
    private bool IsInRock(Vector3 position);
    private void DoExplosion(string sound, Vector3 position);
    private IPlayer FindPlayer(string nameOrIdOrIp);
    private bool IsFriend(ulong playerId, ulong targetId);
    private bool SpawnAborted(BasePlayer player, BaseEntity npc, Item npcNade, Vector3 position, ulong npcId);
    private void RemoveNpcData(ulong npcId);
    private void SaveData();
    private void GiveGrenadeNpcKit(NPCPlayer npc, Item npcNade);
    [Command("npcnade.give")]
    private void CmdGiveNpcNade(IPlayer player, string command, string[] args);
    private class ScarecrowAI : FacepunchBehaviour
    {
        private ScarecrowNPC Scarecrow;
        public NPCPlayerData Settings;
        public bool isEquippingWeapon;
        public AttackEntity CurrentWeapon { get; set; }
        private void Start();
        private void InitBrain();
        private void MeleeAttack();
        private void EquipWeapon();
        private IEnumerator EquippingWeapon();
    }

    private class ScientistAI : FacepunchBehaviour
    {
        private ScientistNPC Scientist;
        public NPCPlayerData Settings;
        public bool isEquippingWeapon;
        public AttackEntity CurrentWeapon { get; set; }
        private void Start();
        private void InitBrain();
        private void EquipWeapon();
        private IEnumerator EquippingWeapon();
    }

    private class ScientistMovement : MonoBehaviour
    {
        public ScientistNPC Scientist;
        public Vector3 HomeLoc;
        public NPCPlayerData Settings;
        public bool returningHome;
        public bool isRoaming;
        private void Start();
        private void Init();
        private void ClearSenses();
        private void MoveScientist();
        private void SetDest(Vector3 position);
        private void OnDestroy();
    }

    private class ScarecrowMovement : MonoBehaviour
    {
        public ScarecrowNPC Scarecrow;
        public Vector3 HomeLoc;
        public Vector3 TargetPos;
        public NPCPlayerData Settings;
        public bool returningHome;
        public bool isRoaming;
        public StateStatus status;
        private void Start();
        private void Init();
        private void CheckPosition();
        private void ClearSenses();
        private void MoveScarecrow();
        private void SetDest(Vector3 position, ScarecrowNPC scarecrow);
        private void BreathingChatter();
        private void OnDestroy();
    }

    private static ConfigData config;
    private class ConfigData
    {
        [JsonProperty(PropertyName = "General Options")]
        public Options options;
        [JsonProperty(PropertyName = "Human NPC Options")]
        public Human human;
        [JsonProperty(PropertyName = "Animal NPC Options")]
        public Animal animal;
        [JsonProperty(PropertyName = "Bradley APC Options")]
        public Bradley bradley;
        public class Options
        {
            [JsonProperty(PropertyName = "Attack Owner")]
            public bool attackOwner;
            [JsonProperty(PropertyName = "Use Friends")]
            public bool useFriends;
            [JsonProperty(PropertyName = "Use Clans")]
            public bool useClans;
            [JsonProperty(PropertyName = "Use Teams")]
            public bool useTeams;
            [JsonProperty(PropertyName = "Use Permissions")]
            public bool usePerms;
            [JsonProperty(PropertyName = "Chat Prefix")]
            public string chatPrefix;
            [JsonProperty(PropertyName = "Use Chat Prefix")]
            public bool usePrefix;
            [JsonProperty(PropertyName = "NPC Safe")]
            public bool npcSafe;
            [JsonProperty(PropertyName = "Bradley Safe")]
            public bool bradleySafe;
            [JsonProperty(PropertyName = "Turret Safe")]
            public bool turretSafe;
            [JsonProperty(PropertyName = "Animal Safe")]
            public bool animalSafe;
            [JsonProperty(PropertyName = "Sleeper Safe")]
            public bool sleeperSafe;
        }

        public class Human
        {
            [JsonProperty(PropertyName = "Scientist Enabled")]
            public bool npcScientist;
            [JsonProperty(PropertyName = "Heavy Scientist Enabled")]
            public bool npcHeavy;
            [JsonProperty(PropertyName = "Juggernaut Enabled")]
            public bool npcJuggernaut;
            [JsonProperty(PropertyName = "Tunnel Dweller Enabled")]
            public bool npcTunnel;
            [JsonProperty(PropertyName = "Underwater Dweller Enabled")]
            public bool npcUnderwater;
            [JsonProperty(PropertyName = "Murderer Enabled")]
            public bool npcMurderer;
            [JsonProperty(PropertyName = "Scarecrow Enabled")]
            public bool npcScarecrow;
            [JsonProperty(PropertyName = "Mummy Enabled")]
            public bool npcMummy;
        }

        public class Animal
        {
            [JsonProperty(PropertyName = "Bear Enabled")]
            public bool npcBear;
            [JsonProperty(PropertyName = "Polar Bear Enabled")]
            public bool npcPolarbear;
            [JsonProperty(PropertyName = "Wolf Enabled")]
            public bool npcWolf;
            [JsonProperty(PropertyName = "Boar Enabled")]
            public bool npcBoar;
            [JsonProperty(PropertyName = "Stag Enabled")]
            public bool npcStag;
            [JsonProperty(PropertyName = "Chicken Enabled")]
            public bool npcChicken;
        }

        public class Bradley
        {
            [JsonProperty(PropertyName = "Bradley APC Enabled")]
            public bool npcBradley;
            [JsonProperty(PropertyName = "Bradley Can Damage Player Bases")]
            public bool bradleyBaseDamage;
        }

        public VersionNumber Version { get; set; }
    }

    private ConfigData GetDefaultConfig();
    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private void UpdateConfigValues();
    private Dictionary<ulong, NPCPlayer> GrenadeNPCData;
    private Dictionary<ulong, BaseNpc> BaseNpcData;
    private Dictionary<ulong, BradleyAPC> BradleyAPCData;
    private Dictionary<ulong, Inv> NPCInventories;
    private Dictionary<ulong, GrenadeData> NadeInfo;
    private class Inv
    {
        public string name;
        public List<InvContents>[] inventory;
    }

    private class InvContents
    {
        public int ID;
        public int amount;
        public ulong skinID;
    }

    private class GrenadeData
    {
        public string Name;
        public ulong ID;
        public string Cmd;
        public bool Enabled;
        public string Perm;
        public string Prefab;
    }

    private void LoadNadeInfo();
    private StoredData storedData;
    private class StoredData
    {
        public Dictionary<string, NPCPlayerData> HumanNPC;
        public Dictionary<string, AnimalData> AnimalNPC;
        public Dictionary<string, APCData> BradleyNPC;
    }

    private class NPCPlayerData
    {
        public string Name;
        public string Prefab;
        public float Health;
        public float MaxRoamRange;
        public float SenseRange;
        public float ListenRange;
        public float AggroRange;
        public float DeAggroRange;
        public float TargetLostRange;
        public float MemoryDuration;
        public float VisionCone;
        public bool CheckVisionCone;
        public bool CheckLOS;
        public bool IgnoreNonVisionSneakers;
        public float DamageScale;
        public bool PeaceKeeper;
        public bool IgnoreSafeZonePlayers;
        public bool RadioChatter;
        public bool DeathSound;
        public int NumberToSpawn;
        public int SpawnRadius;
        public float DespawnTime;
        public bool KillInSafeZone;
        public bool StripCorpseLoot;
        public List<string> KitList;
        public float Speed;
        public float Acceleration;
        public float FastSpeedFraction;
        public float NormalSpeedFraction;
        public float SlowSpeedFraction;
        public float SlowestSpeedFraction;
        public float LowHealthMaxSpeedFraction;
        public float TurnSpeed;
        public ulong GrenadeSkinID;
        public string Permission;
        public string ExplosionSound;
        public List<Loadout> DefaultLoadout;
    }

    private class AnimalData
    {
        public string Name;
        public string Prefab;
        public float Health;
        public bool KillInSafeZone;
        public float DespawnTime;
        public int NumberToSpawn;
        public int SpawnRadius;
        public ulong GrenadeSkinID;
        public string Permission;
        public string ExplosionSound;
    }

    private class APCData
    {
        public string Name;
        public string Prefab;
        public float Health;
        public float ViewDistance;
        public float SearchRange;
        public float PatrolRange;
        public int PatrolPathNodes;
        public float ThrottleResponse;
        public int CratesToSpawn;
        public bool KillInSafeZone;
        public float DespawnTime;
        public int NumberToSpawn;
        public int SpawnRadius;
        public ulong GrenadeSkinID;
        public string Permission;
        public string ExplosionSound;
    }

    private class Loadout
    {
        public string Shortname;
        public ulong SkinID;
        public int Amount;
        [JsonProperty(PropertyName = "Container Type (Belt, Main, Wear)")]
        public string Container;
    }

    private void UpdateStoredData();
    private void LoadDefaultData();
    private static List<Loadout> ScientistLoadout();
    private static List<Loadout> HeavyLoadout();
    private static List<Loadout> JuggernautLoadout();
    private static List<Loadout> TunnelLoadout();
    private static List<Loadout> UnderwaterLoadout();
    private static List<Loadout> MurdererLoadout();
    private static List<Loadout> ScarecrowLoadout();
    private static List<Loadout> MummyLoadout();
}

private class ScarecrowAI : FacepunchBehaviour
{
    private ScarecrowNPC Scarecrow;
    public NPCPlayerData Settings;
    public bool isEquippingWeapon;
    public AttackEntity CurrentWeapon { get; set; }
    private void Start();
    private void InitBrain();
    private void MeleeAttack();
    private void EquipWeapon();
    private IEnumerator EquippingWeapon();
}

private class ScientistAI : FacepunchBehaviour
{
    private ScientistNPC Scientist;
    public NPCPlayerData Settings;
    public bool isEquippingWeapon;
    public AttackEntity CurrentWeapon { get; set; }
    private void Start();
    private void InitBrain();
    private void EquipWeapon();
    private IEnumerator EquippingWeapon();
}

private class ScientistMovement : MonoBehaviour
{
    public ScientistNPC Scientist;
    public Vector3 HomeLoc;
    public NPCPlayerData Settings;
    public bool returningHome;
    public bool isRoaming;
    private void Start();
    private void Init();
    private void ClearSenses();
    private void MoveScientist();
    private void SetDest(Vector3 position);
    private void OnDestroy();
}

private class ScarecrowMovement : MonoBehaviour
{
    public ScarecrowNPC Scarecrow;
    public Vector3 HomeLoc;
    public Vector3 TargetPos;
    public NPCPlayerData Settings;
    public bool returningHome;
    public bool isRoaming;
    public StateStatus status;
    private void Start();
    private void Init();
    private void CheckPosition();
    private void ClearSenses();
    private void MoveScarecrow();
    private void SetDest(Vector3 position, ScarecrowNPC scarecrow);
    private void BreathingChatter();
    private void OnDestroy();
}

private class ConfigData
{
    [JsonProperty(PropertyName = "General Options")]
    public Options options;
    [JsonProperty(PropertyName = "Human NPC Options")]
    public Human human;
    [JsonProperty(PropertyName = "Animal NPC Options")]
    public Animal animal;
    [JsonProperty(PropertyName = "Bradley APC Options")]
    public Bradley bradley;
    public class Options
    {
        [JsonProperty(PropertyName = "Attack Owner")]
        public bool attackOwner;
        [JsonProperty(PropertyName = "Use Friends")]
        public bool useFriends;
        [JsonProperty(PropertyName = "Use Clans")]
        public bool useClans;
        [JsonProperty(PropertyName = "Use Teams")]
        public bool useTeams;
        [JsonProperty(PropertyName = "Use Permissions")]
        public bool usePerms;
        [JsonProperty(PropertyName = "Chat Prefix")]
        public string chatPrefix;
        [JsonProperty(PropertyName = "Use Chat Prefix")]
        public bool usePrefix;
        [JsonProperty(PropertyName = "NPC Safe")]
        public bool npcSafe;
        [JsonProperty(PropertyName = "Bradley Safe")]
        public bool bradleySafe;
        [JsonProperty(PropertyName = "Turret Safe")]
        public bool turretSafe;
        [JsonProperty(PropertyName = "Animal Safe")]
        public bool animalSafe;
        [JsonProperty(PropertyName = "Sleeper Safe")]
        public bool sleeperSafe;
    }

    public class Human
    {
        [JsonProperty(PropertyName = "Scientist Enabled")]
        public bool npcScientist;
        [JsonProperty(PropertyName = "Heavy Scientist Enabled")]
        public bool npcHeavy;
        [JsonProperty(PropertyName = "Juggernaut Enabled")]
        public bool npcJuggernaut;
        [JsonProperty(PropertyName = "Tunnel Dweller Enabled")]
        public bool npcTunnel;
        [JsonProperty(PropertyName = "Underwater Dweller Enabled")]
        public bool npcUnderwater;
        [JsonProperty(PropertyName = "Murderer Enabled")]
        public bool npcMurderer;
        [JsonProperty(PropertyName = "Scarecrow Enabled")]
        public bool npcScarecrow;
        [JsonProperty(PropertyName = "Mummy Enabled")]
        public bool npcMummy;
    }

    public class Animal
    {
        [JsonProperty(PropertyName = "Bear Enabled")]
        public bool npcBear;
        [JsonProperty(PropertyName = "Polar Bear Enabled")]
        public bool npcPolarbear;
        [JsonProperty(PropertyName = "Wolf Enabled")]
        public bool npcWolf;
        [JsonProperty(PropertyName = "Boar Enabled")]
        public bool npcBoar;
        [JsonProperty(PropertyName = "Stag Enabled")]
        public bool npcStag;
        [JsonProperty(PropertyName = "Chicken Enabled")]
        public bool npcChicken;
    }

    public class Bradley
    {
        [JsonProperty(PropertyName = "Bradley APC Enabled")]
        public bool npcBradley;
        [JsonProperty(PropertyName = "Bradley Can Damage Player Bases")]
        public bool bradleyBaseDamage;
    }

    public VersionNumber Version { get; set; }
}

public class Options
{
    [JsonProperty(PropertyName = "Attack Owner")]
    public bool attackOwner;
    [JsonProperty(PropertyName = "Use Friends")]
    public bool useFriends;
    [JsonProperty(PropertyName = "Use Clans")]
    public bool useClans;
    [JsonProperty(PropertyName = "Use Teams")]
    public bool useTeams;
    [JsonProperty(PropertyName = "Use Permissions")]
    public bool usePerms;
    [JsonProperty(PropertyName = "Chat Prefix")]
    public string chatPrefix;
    [JsonProperty(PropertyName = "Use Chat Prefix")]
    public bool usePrefix;
    [JsonProperty(PropertyName = "NPC Safe")]
    public bool npcSafe;
    [JsonProperty(PropertyName = "Bradley Safe")]
    public bool bradleySafe;
    [JsonProperty(PropertyName = "Turret Safe")]
    public bool turretSafe;
    [JsonProperty(PropertyName = "Animal Safe")]
    public bool animalSafe;
    [JsonProperty(PropertyName = "Sleeper Safe")]
    public bool sleeperSafe;
}

public class Human
{
    [JsonProperty(PropertyName = "Scientist Enabled")]
    public bool npcScientist;
    [JsonProperty(PropertyName = "Heavy Scientist Enabled")]
    public bool npcHeavy;
    [JsonProperty(PropertyName = "Juggernaut Enabled")]
    public bool npcJuggernaut;
    [JsonProperty(PropertyName = "Tunnel Dweller Enabled")]
    public bool npcTunnel;
    [JsonProperty(PropertyName = "Underwater Dweller Enabled")]
    public bool npcUnderwater;
    [JsonProperty(PropertyName = "Murderer Enabled")]
    public bool npcMurderer;
    [JsonProperty(PropertyName = "Scarecrow Enabled")]
    public bool npcScarecrow;
    [JsonProperty(PropertyName = "Mummy Enabled")]
    public bool npcMummy;
}

public class Animal
{
    [JsonProperty(PropertyName = "Bear Enabled")]
    public bool npcBear;
    [JsonProperty(PropertyName = "Polar Bear Enabled")]
    public bool npcPolarbear;
    [JsonProperty(PropertyName = "Wolf Enabled")]
    public bool npcWolf;
    [JsonProperty(PropertyName = "Boar Enabled")]
    public bool npcBoar;
    [JsonProperty(PropertyName = "Stag Enabled")]
    public bool npcStag;
    [JsonProperty(PropertyName = "Chicken Enabled")]
    public bool npcChicken;
}

public class Bradley
{
    [JsonProperty(PropertyName = "Bradley APC Enabled")]
    public bool npcBradley;
    [JsonProperty(PropertyName = "Bradley Can Damage Player Bases")]
    public bool bradleyBaseDamage;
}

private class Inv
{
    public string name;
    public List<InvContents>[] inventory;
}

private class InvContents
{
    public int ID;
    public int amount;
    public ulong skinID;
}

private class GrenadeData
{
    public string Name;
    public ulong ID;
    public string Cmd;
    public bool Enabled;
    public string Perm;
    public string Prefab;
}

private class StoredData
{
    public Dictionary<string, NPCPlayerData> HumanNPC;
    public Dictionary<string, AnimalData> AnimalNPC;
    public Dictionary<string, APCData> BradleyNPC;
}

private class NPCPlayerData
{
    public string Name;
    public string Prefab;
    public float Health;
    public float MaxRoamRange;
    public float SenseRange;
    public float ListenRange;
    public float AggroRange;
    public float DeAggroRange;
    public float TargetLostRange;
    public float MemoryDuration;
    public float VisionCone;
    public bool CheckVisionCone;
    public bool CheckLOS;
    public bool IgnoreNonVisionSneakers;
    public float DamageScale;
    public bool PeaceKeeper;
    public bool IgnoreSafeZonePlayers;
    public bool RadioChatter;
    public bool DeathSound;
    public int NumberToSpawn;
    public int SpawnRadius;
    public float DespawnTime;
    public bool KillInSafeZone;
    public bool StripCorpseLoot;
    public List<string> KitList;
    public float Speed;
    public float Acceleration;
    public float FastSpeedFraction;
    public float NormalSpeedFraction;
    public float SlowSpeedFraction;
    public float SlowestSpeedFraction;
    public float LowHealthMaxSpeedFraction;
    public float TurnSpeed;
    public ulong GrenadeSkinID;
    public string Permission;
    public string ExplosionSound;
    public List<Loadout> DefaultLoadout;
}

private class AnimalData
{
    public string Name;
    public string Prefab;
    public float Health;
    public bool KillInSafeZone;
    public float DespawnTime;
    public int NumberToSpawn;
    public int SpawnRadius;
    public ulong GrenadeSkinID;
    public string Permission;
    public string ExplosionSound;
}

private class APCData
{
    public string Name;
    public string Prefab;
    public float Health;
    public float ViewDistance;
    public float SearchRange;
    public float PatrolRange;
    public int PatrolPathNodes;
    public float ThrottleResponse;
    public int CratesToSpawn;
    public bool KillInSafeZone;
    public float DespawnTime;
    public int NumberToSpawn;
    public int SpawnRadius;
    public ulong GrenadeSkinID;
    public string Permission;
    public string ExplosionSound;
}

private class Loadout
{
    public string Shortname;
    public ulong SkinID;
    public int Amount;
    [JsonProperty(PropertyName = "Container Type (Belt, Main, Wear)")]
    public string Container;
}


```

---

## NPCVendingManager

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

Oxide.Plugins
[Info("NPCVendingManager", "EcoSmile", "1.0.0")]
 class NPCVendingManager : RustPlugin
{
    static NPCVendingManager ins;
     PluginConfig config;
    public class PluginConfig
    {
        [JsonProperty("Настройка торговых автоматов")]
        public Dictionary<string, List<VendingOrder>> Vending { get; set; }
    }

    public class VendingOrder
    {
        [JsonProperty("Добавить товар?")]
        public bool AddItem;
        [JsonProperty("Название покупаемого товара")]
        public string ItemToBuy { get; set; }
        [JsonProperty("Максимальный стак покупаемого предмета за один раз")]
        public int BuyngItemMaxAmount { get; set; }
        [JsonProperty("SKINID покупаемого предмета")]
        public ulong BuyngItemSkinID { get; set; }
        [JsonProperty("Количество покупаемого товара за раз")]
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
    public Dictionary<string, List<VendingOrder>> GetVendingList();
     void VendingInit();
    public void AddItemForSale(VendingMachine vending, int itemID, int amountToSell, int currencyID, int currencyPerTransaction, byte bpState, int maxByStack, ulong SkinID);
}

public class PluginConfig
{
    [JsonProperty("Настройка торговых автоматов")]
    public Dictionary<string, List<VendingOrder>> Vending { get; set; }
}

public class VendingOrder
{
    [JsonProperty("Добавить товар?")]
    public bool AddItem;
    [JsonProperty("Название покупаемого товара")]
    public string ItemToBuy { get; set; }
    [JsonProperty("Максимальный стак покупаемого предмета за один раз")]
    public int BuyngItemMaxAmount { get; set; }
    [JsonProperty("SKINID покупаемого предмета")]
    public ulong BuyngItemSkinID { get; set; }
    [JsonProperty("Количество покупаемого товара за раз")]
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

## NTeleportation

```csharp
using Facepunch;
using Network;
using Newtonsoft.Json;
using Newtonsoft.Json.Converters;
using Newtonsoft.Json.Linq;
using Oxide.Core;
using Oxide.Core.Configuration;
using Oxide.Core.Libraries.Covalence;
using Oxide.Core.Plugins;
using Oxide.Game.Rust;
using Oxide.Game.Rust.Cui;
using Rust;
using System;
using System.Collections;
using System.Collections.Generic;
using System.Globalization;
using System.IO;
using System.Linq;
using System.Reflection;
using UnityEngine;

Oxide.Plugins
[Info("NTeleportation", "nivex", "1.7.8")]
[Description("Multiple teleportation systems for admin and players")]
 class NTeleportation : RustPlugin
{
    [PluginReference]
    private Plugin Clans;
    private Plugin Economics;
    private Plugin IQEconomic;
    private Plugin ServerRewards;
    private Plugin Friends;
    private Plugin CompoundTeleport;
    private Plugin ZoneManager;
    private Plugin NoEscape;
    private Plugin PopupNotifications;
    private Plugin BlockUsers;
    private Dictionary<string, BasePlayer> _ids;
    private Dictionary<BasePlayer, string> _players;
    private bool newSave;
    private const string NewLine;
    private const string TPA;
    private static readonly string[] nullArg;
    private const string PermAdmin;
    private const string PermRestrictions;
    private const string ConfigDefaultPermVip;
    private const string PermHome;
    private const string PermWipeHomes;
    private const string PermCraftHome;
    private const string PermDeleteHome;
    private const string PermHomeHomes;
    private const string PermImportHomes;
    private const string PermRadiusHome;
    private const string PermCraftTpR;
    private const string PermTpR;
    private const string PermTpA;
    private const string PermTp;
    private const string PermDisallowTpToMe;
    private const string PermTpT;
    private const string PermTpB;
    private const string PermTpN;
    private const string PermTpL;
    private const string PermTpConsole;
    private const string PermTpRemove;
    private const string PermTpSave;
    private const string PermExempt;
    private const string PermFoundationCheck;
    private const string PermTpMarker;
    private DynamicConfigFile dataConvert;
    private DynamicConfigFile dataDisabled;
    private DynamicConfigFile dataAdmin;
    private DynamicConfigFile dataHome;
    private DynamicConfigFile dataTPR;
    private DynamicConfigFile dataTPT;
    private Dictionary<ulong, AdminData> _Admin;
    private Dictionary<ulong, HomeData> _Home;
    private Dictionary<ulong, TeleportData> _TPR;
    private Dictionary<string, List<string>> TPT;
    private bool changedAdmin;
    private bool changedHome;
    private bool changedTPR;
    private bool changedTPT;
    private float boundary;
    private readonly Dictionary<ulong, float> TeleportCooldowns;
    private readonly Dictionary<ulong, TeleportTimer> TeleportTimers;
    private readonly Dictionary<ulong, Timer> PendingRequests;
    private readonly Dictionary<ulong, BasePlayer> PlayersRequests;
    private readonly Dictionary<int, string> ReverseBlockedItems;
    private readonly Dictionary<ulong, Vector3> teleporting;
    private SortedDictionary<string, Vector3> caves;
    private List<MonumentInfoEx> monuments;
    private bool outpostEnabled;
    private bool banditEnabled;
    private class MonumentInfoEx
    {
        public MonumentInfo monument;
        public Vector3 position;
        public float radius;
        public string name;
        public string prefab;
        public MonumentInfoEx();
        public MonumentInfoEx(MonumentInfo monument, Vector3 position, float radius, string name, string prefab);
        public bool IsInBounds(Vector3 target);
    }

    private static Configuration config;
    public class InterruptSettings
    {
        [JsonProperty(PropertyName = "Interrupt Teleport At Specific Monuments", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<string> Monuments { get; set; }
        [JsonProperty(PropertyName = "Above Water")]
        public bool AboveWater { get; set; }
        [JsonProperty(PropertyName = "Under Water")]
        public bool UnderWater { get; set; }
        [JsonProperty(PropertyName = "Balloon")]
        public bool Balloon { get; set; }
        [JsonProperty(PropertyName = "Boats")]
        public bool Boats { get; set; }
        [JsonProperty(PropertyName = "Cargo Ship")]
        public bool Cargo { get; set; }
        [JsonProperty(PropertyName = "Cold")]
        public bool Cold { get; set; }
        [JsonProperty(PropertyName = "Excavator")]
        public bool Excavator { get; set; }
        [JsonProperty(PropertyName = "Hot")]
        public bool Hot { get; set; }
        [JsonProperty(PropertyName = "Hostile")]
        public bool Hostile { get; set; }
        [JsonProperty(PropertyName = "Hurt")]
        public bool Hurt { get; set; }
        [JsonProperty(PropertyName = "Junkpiles")]
        public bool Junkpiles { get; set; }
        [JsonProperty(PropertyName = "Lift")]
        public bool Lift { get; set; }
        [JsonProperty(PropertyName = "Monument")]
        public bool Monument { get; set; }
        [JsonProperty(PropertyName = "Ignore Monument Marker Prefab")]
        public bool BypassMonumentMarker { get; set; }
        [JsonProperty(PropertyName = "Mounted")]
        public bool Mounted { get; set; }
        [JsonProperty(PropertyName = "Oil Rig")]
        public bool Oilrig { get; set; }
        [JsonProperty(PropertyName = "Safe Zone")]
        public bool Safe { get; set; }
        [JsonProperty(PropertyName = "Swimming")]
        public bool Swimming { get; set; }
    }

    private object OnPlayerRespawn(BasePlayer player);
    public class PluginSettings
    {
        [JsonProperty(PropertyName = "Delay Saving Data On Server Save")]
        public double SaveDelay { get; set; }
        [JsonProperty("TPB")]
        public TPBSettings TPB;
        [JsonProperty(PropertyName = "Interrupt TP")]
        public InterruptSettings Interrupt { get; set; }
        [JsonProperty(PropertyName = "Auto Wake Up After Teleport")]
        public bool AutoWakeUp { get; set; }
        [JsonProperty(PropertyName = "Respawn Players At Outpost")]
        public bool RespawnOutpost { get; set; }
        [JsonProperty(PropertyName = "Block Teleport (NoEscape)")]
        public bool BlockNoEscape { get; set; }
        [JsonProperty(PropertyName = "Block Teleport (ZoneManager)")]
        public bool BlockZoneFlag { get; set; }
        [JsonProperty(PropertyName = "Chat Name")]
        public string ChatName { get; set; }
        [JsonProperty(PropertyName = "Chat Steam64ID")]
        public ulong ChatID { get; set; }
        [JsonProperty(PropertyName = "Check Boundaries On Teleport X Y Z")]
        public bool CheckBoundaries { get; set; }
        [JsonProperty(PropertyName = "Check Boundaries Min Height")]
        public float BoundaryMin { get; set; }
        [JsonProperty(PropertyName = "Check Boundaries Max Height")]
        public float BoundaryMax { get; set; }
        [JsonProperty(PropertyName = "Check If Inside Rock")]
        public bool Rock { get; set; }
        [JsonProperty(PropertyName = "Height To Prevent Teleporting To/From (0 = disabled)")]
        public float ForcedBoundary { get; set; }
        [JsonProperty(PropertyName = "Data File Directory (Blank = Default)")]
        public string DataFileFolder { get; set; }
        [JsonProperty(PropertyName = "Draw Sphere On Set Home")]
        public bool DrawHomeSphere { get; set; }
        [JsonProperty(PropertyName = "Homes Enabled")]
        public bool HomesEnabled { get; set; }
        [JsonProperty(PropertyName = "TPR Enabled")]
        public bool TPREnabled { get; set; }
        [JsonProperty(PropertyName = "Strict Foundation Check")]
        public bool StrictFoundationCheck { get; set; }
        [JsonProperty(PropertyName = "Minimum Temp")]
        public float MinimumTemp { get; set; }
        [JsonProperty(PropertyName = "Maximum Temp")]
        public float MaximumTemp { get; set; }
        [JsonProperty(PropertyName = "Blocked Items", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public Dictionary<string, string> BlockedItems { get; set; }
        [JsonProperty(PropertyName = "Bypass CMD")]
        public string BypassCMD { get; set; }
        [JsonProperty(PropertyName = "Use Economics")]
        public bool UseEconomics { get; set; }
        [JsonProperty(PropertyName = "Use Server Rewards")]
        public bool UseServerRewards { get; set; }
        [JsonProperty(PropertyName = "Wipe On Upgrade Or Change")]
        public bool WipeOnUpgradeOrChange { get; set; }
        [JsonProperty(PropertyName = "Auto Generate Outpost Location")]
        public bool AutoGenOutpost { get; set; }
        [JsonProperty(PropertyName = "Outpost Map Prefab", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<string> Outpost { get; set; }
        [JsonProperty(PropertyName = "Auto Generate Bandit Location")]
        public bool AutoGenBandit { get; set; }
        [JsonProperty(PropertyName = "Bandit Map Prefab", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<string> Bandit { get; set; }
        [JsonProperty(PropertyName = "Show Time As Seconds Instead")]
        public bool UseSeconds { get; set; }
        [JsonProperty(PropertyName = "Use Quick Teleport")]
        public bool Quick { get; set; }
        [JsonProperty(PropertyName = "Chat Command Color")]
        public string ChatCommandColor;
        [JsonProperty(PropertyName = "Chat Command Argument Color")]
        public string ChatCommandArgumentColor;
        [JsonProperty("Enable Popup Support")]
        public bool UsePopup;
        [JsonProperty("Send Messages To Player")]
        public bool SendMessages;
        [JsonProperty("Block All Teleporting From Inside Authorized Base")]
        public bool BlockAuthorizedTeleporting;
        [JsonProperty("Global Teleport Cooldown")]
        public float Global;
        [JsonProperty("Global VIP Teleport Cooldown")]
        public float GlobalVIP;
        [JsonProperty("Play Sounds Before Teleport")]
        public bool PlaySoundsBeforeTeleport;
        [JsonProperty("Sound Effects Before Teleport", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<string> DisappearEffects;
        [JsonProperty("Play Sounds After Teleport")]
        public bool PlaySoundsAfterTeleport;
        [JsonProperty("Sound Effects After Teleport", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<string> ReappearEffects;
    }

    public class TPBSettings
    {
        [JsonProperty(PropertyName = "VIP Countdowns", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public Dictionary<string, int> Countdowns { get; set; }
        [JsonProperty("Countdown")]
        public int Countdown;
        [JsonProperty("Available After X Seconds")]
        public int Time;
    }

    public class AdminSettings
    {
        [JsonProperty(PropertyName = "Announce Teleport To Target")]
        public bool AnnounceTeleportToTarget { get; set; }
        [JsonProperty(PropertyName = "Usable By Admins")]
        public bool UseableByAdmins { get; set; }
        [JsonProperty(PropertyName = "Usable By Moderators")]
        public bool UseableByModerators { get; set; }
        [JsonProperty(PropertyName = "Location Radius")]
        public int LocationRadius { get; set; }
        [JsonProperty(PropertyName = "Teleport Near Default Distance")]
        public int TeleportNearDefaultDistance { get; set; }
        [JsonProperty(PropertyName = "Extra Distance To Block Monument Teleporting")]
        public int ExtraMonumentDistance { get; set; }
    }

    public class HomesSettings
    {
        [JsonProperty(PropertyName = "Homes Limit")]
        public int HomesLimit { get; set; }
        [JsonProperty(PropertyName = "VIP Homes Limits", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public Dictionary<string, int> VIPHomesLimits { get; set; }
        [JsonProperty(PropertyName = "Allow Sethome At Specific Monuments", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<string> AllowedMonuments { get; set; }
        [JsonProperty(PropertyName = "Allow Sethome At All Monuments")]
        public bool AllowAtAllMonuments { get; set; }
        [JsonProperty(PropertyName = "Allow Sethome On Tugboats")]
        public bool AllowTugboats { get; set; }
        [JsonProperty(PropertyName = "Allow TPB")]
        public bool AllowTPB { get; set; }
        [JsonProperty(PropertyName = "Cooldown")]
        public int Cooldown { get; set; }
        [JsonProperty(PropertyName = "Countdown")]
        public int Countdown { get; set; }
        [JsonProperty(PropertyName = "Daily Limit")]
        public int DailyLimit { get; set; }
        [JsonProperty(PropertyName = "VIP Daily Limits", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public Dictionary<string, int> VIPDailyLimits { get; set; }
        [JsonProperty(PropertyName = "VIP Cooldowns", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public Dictionary<string, int> VIPCooldowns { get; set; }
        [JsonProperty(PropertyName = "VIP Countdowns", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public Dictionary<string, int> VIPCountdowns { get; set; }
        [JsonProperty(PropertyName = "Location Radius")]
        public int LocationRadius { get; set; }
        [JsonProperty(PropertyName = "Force On Top Of Foundation")]
        public bool ForceOnTopOfFoundation { get; set; }
        [JsonProperty(PropertyName = "Check Foundation For Owner")]
        public bool CheckFoundationForOwner { get; set; }
        [JsonProperty(PropertyName = "Use Friends")]
        public bool UseFriends { get; set; }
        [JsonProperty(PropertyName = "Use Clans")]
        public bool UseClans { get; set; }
        [JsonProperty(PropertyName = "Use Teams")]
        public bool UseTeams { get; set; }
        [JsonProperty(PropertyName = "Usable Out Of Building Blocked")]
        public bool UsableOutOfBuildingBlocked { get; set; }
        [JsonProperty(PropertyName = "Usable Into Building Blocked")]
        public bool UsableIntoBuildingBlocked { get; set; }
        [JsonProperty(PropertyName = "Usable From Safe Zone Only")]
        public bool UsableFromSafeZoneOnly { get; set; }
        [JsonProperty(PropertyName = "Allow Cupboard Owner When Building Blocked")]
        public bool CupOwnerAllowOnBuildingBlocked { get; set; }
        [JsonProperty(PropertyName = "Allow Iceberg")]
        public bool AllowIceberg { get; set; }
        [JsonProperty(PropertyName = "Allow Cave")]
        public bool AllowCave { get; set; }
        [JsonProperty(PropertyName = "Allow Crafting")]
        public bool AllowCraft { get; set; }
        [JsonProperty(PropertyName = "Allow Above Foundation")]
        public bool AllowAboveFoundation { get; set; }
        [JsonProperty(PropertyName = "Check If Home Is Valid On Listhomes")]
        public bool CheckValidOnList { get; set; }
        [JsonProperty(PropertyName = "Pay")]
        public int Pay { get; set; }
        [JsonProperty(PropertyName = "Bypass")]
        public int Bypass { get; set; }
        [JsonProperty(PropertyName = "Hours Before Useable After Wipe")]
        public double Hours { get; set; }
    }

    public class TPTSettings
    {
        [JsonProperty(PropertyName = "Use Friends")]
        public bool UseFriends { get; set; }
        [JsonProperty(PropertyName = "Use Clans")]
        public bool UseClans { get; set; }
        [JsonProperty(PropertyName = "Use Teams")]
        public bool UseTeams { get; set; }
        [JsonProperty(PropertyName = "Allow Cave")]
        public bool AllowCave { get; set; }
        [JsonProperty(PropertyName = "Enabled Color")]
        public string EnabledColor { get; set; }
        [JsonProperty(PropertyName = "Disabled Color")]
        public string DisabledColor { get; set; }
    }

    public class TPRSettings
    {
        [JsonProperty("Play Sounds To Request Target")]
        public bool PlaySoundsToRequestTarget;
        [JsonProperty("Teleport Request Sound Effects", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<string> TeleportRequestEffects;
        [JsonProperty("Play Sounds When Target Accepts")]
        public bool PlaySoundsWhenTargetAccepts;
        [JsonProperty("Teleport Accept Sound Effects", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<string> TeleportAcceptEffects;
        [JsonProperty(PropertyName = "Require Player To Be Friend, Clan Mate, Or Team Mate")]
        public bool UseClans_Friends_Teams { get; set; }
        [JsonProperty(PropertyName = "Require nteleportation.tpa to accept TPR requests")]
        public bool RequireTPAPermission { get; set; }
        [JsonProperty(PropertyName = "Allow Cave")]
        public bool AllowCave { get; set; }
        [JsonProperty(PropertyName = "Allow TPB")]
        public bool AllowTPB { get; set; }
        [JsonProperty(PropertyName = "Use Blocked Users")]
        public bool UseBlockedUsers { get; set; }
        [JsonProperty(PropertyName = "Cooldown")]
        public int Cooldown { get; set; }
        [JsonProperty(PropertyName = "Countdown")]
        public int Countdown { get; set; }
        [JsonProperty(PropertyName = "Daily Limit")]
        public int DailyLimit { get; set; }
        [JsonProperty(PropertyName = "VIP Daily Limits", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public Dictionary<string, int> VIPDailyLimits { get; set; }
        [JsonProperty(PropertyName = "VIP Cooldowns", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public Dictionary<string, int> VIPCooldowns { get; set; }
        [JsonProperty(PropertyName = "VIP Countdowns", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public Dictionary<string, int> VIPCountdowns { get; set; }
        [JsonProperty(PropertyName = "Enable Request UI")]
        public bool UI { get; set; }
        [JsonProperty(PropertyName = "Request Duration")]
        public int RequestDuration { get; set; }
        [JsonProperty(PropertyName = "Block TPA On Ceiling")]
        public bool BlockTPAOnCeiling { get; set; }
        [JsonProperty(PropertyName = "Usable Out Of Building Blocked")]
        public bool UsableOutOfBuildingBlocked { get; set; }
        [JsonProperty(PropertyName = "Usable Into Building Blocked")]
        public bool UsableIntoBuildingBlocked { get; set; }
        [JsonProperty(PropertyName = "Allow Cupboard Owner When Building Blocked")]
        public bool CupOwnerAllowOnBuildingBlocked { get; set; }
        [JsonProperty(PropertyName = "Allow Crafting")]
        public bool AllowCraft { get; set; }
        [JsonProperty(PropertyName = "Pay")]
        public int Pay { get; set; }
        [JsonProperty(PropertyName = "Bypass")]
        public int Bypass { get; set; }
        [JsonProperty(PropertyName = "Hours Before Useable After Wipe")]
        public double Hours { get; set; }
    }

    public class TownSettings
    {
        [JsonProperty(PropertyName = "Command Enabled")]
        public bool Enabled { get; set; }
        [JsonProperty(PropertyName = "Set Position From Monument Marker Name")]
        public string MonumentMarkerName { get; set; }
        [JsonProperty(PropertyName = "Set Position From Monument Marker Name Offset")]
        public string MonumentMarkerNameOffset { get; set; }
        [JsonProperty(PropertyName = "Allow TPB")]
        public bool AllowTPB { get; set; }
        [JsonProperty(PropertyName = "Allow Cave")]
        public bool AllowCave { get; set; }
        [JsonProperty(PropertyName = "Cooldown")]
        public int Cooldown { get; set; }
        [JsonProperty(PropertyName = "Countdown")]
        public int Countdown { get; set; }
        [JsonProperty(PropertyName = "Daily Limit")]
        public int DailyLimit { get; set; }
        [JsonProperty(PropertyName = "VIP Daily Limits", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public Dictionary<string, int> VIPDailyLimits { get; set; }
        [JsonProperty(PropertyName = "VIP Cooldowns", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public Dictionary<string, int> VIPCooldowns { get; set; }
        [JsonProperty(PropertyName = "VIP Countdowns", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public Dictionary<string, int> VIPCountdowns { get; set; }
        [JsonProperty(PropertyName = "Location")]
        public Vector3 Location { get; set; }
        [JsonProperty(PropertyName = "Locations", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<Vector3> Locations { get; set; }
        [JsonProperty(PropertyName = "Teleport To Random Location")]
        public bool Random { get; set; }
        [JsonProperty(PropertyName = "Usable Out Of Building Blocked")]
        public bool UsableOutOfBuildingBlocked { get; set; }
        [JsonProperty(PropertyName = "Allow Crafting")]
        public bool AllowCraft { get; set; }
        [JsonProperty(PropertyName = "Pay")]
        public int Pay { get; set; }
        [JsonProperty(PropertyName = "Bypass")]
        public int Bypass { get; set; }
        [JsonProperty(PropertyName = "Hours Before Useable After Wipe")]
        public double Hours { get; set; }
        public bool CanCraft(BasePlayer player, string command);
        [JsonIgnore]
        public StoredData Teleports;
        [JsonIgnore]
        public string Command { get; set; }
    }

    private class Configuration
    {
        [JsonProperty(PropertyName = "Settings")]
        public PluginSettings Settings;
        [JsonProperty(PropertyName = "Admin")]
        public AdminSettings Admin;
        [JsonProperty(PropertyName = "Home")]
        public HomesSettings Home;
        [JsonProperty(PropertyName = "TPT")]
        public TPTSettings TPT;
        [JsonProperty(PropertyName = "TPR")]
        public TPRSettings TPR;
        [JsonProperty(PropertyName = "Dynamic Commands", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public Dictionary<string, TownSettings> DynamicCommands { get; set; }
    }

    private static Dictionary<string, TownSettings> DefaultCommands;
    public void InitializeDynamicCommands();
    protected override void LoadConfig();
    protected override void SaveConfig();
    protected override void LoadDefaultConfig();
    private class DisabledData
    {
        [JsonProperty("List of disabled commands")]
        public List<string> DisabledCommands;
        public DisabledData();
    }

     DisabledData DisabledCommandData;
    private class AdminData
    {
        [JsonProperty("pl")]
        public Vector3 PreviousLocation { get; set; }
        [JsonProperty("b")]
        public bool BuildingBlocked { get; set; }
        [JsonProperty("c")]
        public bool AllowCrafting { get; set; }
        [JsonProperty("l")]
        public Dictionary<string, Vector3> Locations { get; set; }
    }

    private class HomeData
    {
        public class Entry
        {
            public Vector3 Position;
            public BaseNetworkable Entity;
            public bool isEntity { get; set; }
            public bool wasEntity;
            public Entry();
            public Entry(Vector3 Position);
            public Vector3 Get();
        }

        public class Boat
        {
            public ulong Value;
            public Vector3 Offset;
            public Boat();
            public Boat(Entry entry);
        }

        [JsonProperty("l")]
        public Dictionary<string, Vector3> buildings { get; set; }
        [JsonProperty("b")]
        public Dictionary<string, Boat> boats { get; set; }
        [JsonProperty("t")]
        public TeleportData Teleports { get; set; }
        [JsonIgnore]
        private Dictionary<string, Entry> Cache;
        [JsonIgnore]
        public Dictionary<string, Entry> Locations { get; set; }
        private void InitializeBuildings();
        private void InitializeBoats();
        public bool TryGetValue(string key, Entry homeEntry);
        public void Set(string key, Entry homeEntry);
        public bool Remove(string key);
    }

    public class TeleportData
    {
        [JsonProperty("a")]
        public int Amount { get; set; }
        [JsonProperty("d")]
        public string Date { get; set; }
        [JsonProperty("t")]
        public int Timestamp { get; set; }
    }

    private class TeleportTimer
    {
        public Timer Timer { get; set; }
        public BasePlayer OriginPlayer { get; set; }
        public BasePlayer TargetPlayer { get; set; }
    }

    private List<string> GetMonumentMessages();
    protected override void LoadDefaultMessages();
    private void Init();
    private string OnPlayerConnected(BasePlayer player);
    private Dictionary<string, StoredData> _DynamicData;
    public class StoredData
    {
        public Dictionary<ulong, TeleportData> TPData;
        public bool Changed;
    }

    private void LoadDataAndPerms();
    private bool CanBypassRestrictions(string userid);
    private void RegisterCommand(string command, string callback, string perm);
    private void UnregisterCommand(string command);
    private void RegisterCommand(string key, TownSettings settings, bool justCreated);
    private DynamicConfigFile GetFile(string name);
    private void SetGlobalCooldown(BasePlayer player);
    private float GetGlobalCooldown(BasePlayer player);
    private bool IsEmptyMap();
    private void CheckNewSave();
     bool TrySetNewTownPosition(TownSettings town);
     void OnServerInitialized();
    private void AddCovalenceCommands();
     void OnNewSave(string strFilename);
     void OnServerSave();
     void SaveAllInstant();
     void OnServerShutdown();
     void Unload();
     void OnEntityTakeDamage(BasePlayer player, HitInfo hitInfo);
    private void SendInterruptMessage(TeleportTimer teleportTimer, BasePlayer player, string key);
    private List<ulong> insideTerrainViolations;
     object OnPlayerViolation(BasePlayer player, AntiHackType type, float amount);
     void OnPlayerSleepEnded(BasePlayer player);
     void OnPlayerDisconnected(BasePlayer player);
    private void SaveTeleportsAdmin();
    private void SaveTeleportsHome();
    private void SaveTeleportsTPR();
    private void SaveTeleportsTPT();
    private void SaveTeleportsTown();
    private void SaveLocation(BasePlayer player, Vector3 position, bool build, bool craft);
    private void RemoveLocation(BasePlayer player);
    private Coroutine _cmc;
    private bool _cmcCompleted;
    private IEnumerator SetupMonuments();
    private IEnumerator SetupOutpost(MonumentInfo monument);
    private IEnumerator SetupBandit(MonumentInfo monument);
    public IEnumerator CalculateMonumentSize(MonumentInfo monument, Vector3 from, string text, string prefab);
    public List<Vector3> GetCircumferencePositions(Vector3 center, float radius, float next);
    private bool TeleportInForcedBoundary(BasePlayer[] players);
    private void TeleportRequestUI(BasePlayer player, string displayName);
    public void DestroyTeleportRequestCUI(BasePlayer player);
    [ConsoleCommand("ntp.accept")]
    private void ccmdAccept(ConsoleSystem.Arg arg);
    [ConsoleCommand("ntp.reject")]
    private void ccmdReject(ConsoleSystem.Arg arg);
    private bool OutOfRange(MonumentInfo m, Vector3 a, bool checkHeight);
    private void CommandToggle(IPlayer user, string command, string[] args);
    private void CommandTeleport(IPlayer user, string command, string[] args);
    private Vector3 GetAdminLocation(string value);
    private void CommandTeleportNear(IPlayer user, string command, string[] args);
    private void CommandTeleportLocation(IPlayer user, string command, string[] args);
    private void CommandSaveTeleportLocation(IPlayer user, string command, string[] args);
    private void CommandRemoveTeleportLocation(IPlayer user, string command, string[] args);
    private void CommandTeleportBack(IPlayer user, string command, string[] args);
    private void TeleportBack(BasePlayer player, AdminData adminData, int countdown);
    private void CommandSetHome(IPlayer user, string command, string[] args);
    private void CommandRemoveHome(IPlayer user, string command, string[] args);
    private void CommandHome(IPlayer user, string command, string[] args);
    private void CommandHomeRadius(IPlayer user, string command, string[] args);
    private void SendHomeError(BasePlayer player, List<string> toRemove, string err, string homeName, Vector3 position, bool wasEntity, bool send);
    private void CommandHomeDelete(IPlayer user, string command, string[] args);
    private void CommandHomeAdminTP(IPlayer user, string command, string[] args);
    private bool UseEconomy();
    private bool CheckEconomy(BasePlayer player, double bypass, bool withdraw, bool deposit);
    private void cmdChatHomeTP(BasePlayer player, string command, string[] args);
    private void CommandListHomes(IPlayer user, string command, string[] args);
    private void ValidateHomes(BasePlayer player, HomeData homeData, bool showRemoved, bool showLoc);
    private void CommandHomeHomes(IPlayer user, string command, string[] args);
    private void CommandTeleportAcceptToggle(IPlayer user, string command, string[] args);
    public bool IsOnSameTeam(ulong playerId, ulong targetId);
    private bool AreFriends(string playerId, string targetId);
    private bool IsFriend(string playerId, string targetId);
    private bool IsInSameClan(string playerId, string targetId);
    private bool InstantTeleportAccept(BasePlayer target, BasePlayer player);
     bool IsEnabled(string targetId, string value);
     void ToggleTPTEnabled(BasePlayer target, string value, string command);
    private string GetMultiplePlayers(List<BasePlayer> players);
    private double GetUseableTime(double hours);
    private void CommandTeleportRequest(IPlayer user, string command, string[] args);
    private void CommandTeleportAccept(IPlayer user, string command, string[] args);
    private void CommandWipeHomes(IPlayer user, string command, string[] args);
    private void CommandTeleportHelp(IPlayer user, string command, string[] args);
    private List<string> _tpid;
    private void CommandTeleportInfo(IPlayer user, string command, string[] args);
    private void CommandTeleportCancel(IPlayer user, string command, string[] args);
    private void CommandDynamic(IPlayer user, string command, string[] args);
    private void CommandCustom(IPlayer user, string command, string[] args);
    private TownSettings GetSettings(string command, ulong userid);
    private bool IsServerCommand(IPlayer user, string command, string[] args);
    private void CommandTown(IPlayer user, string command, string[] args);
    private double SecondsUntilTomorrow();
    private void Interrupt(BasePlayer player, bool paidmoney, double bypass);
    private void CommandTeleportII(IPlayer user, string command, string[] args);
    private void CommandSphereMonuments(IPlayer user, string command, string[] args);
    private void CommandImportHomes(IPlayer user, string command, string[] args);
    private void RequestTimedOut(BasePlayer player, BasePlayer target);
    private void CommandPluginInfo(IPlayer user, string command, string[] args);
    private readonly System.Text.StringBuilder _sb;
    private string FormatTime(BasePlayer player, double seconds);
    public void Teleport(BasePlayer player, BasePlayer target, bool build, bool craft);
    public void Teleport(BasePlayer player, float x, float y, float z, bool build, bool craft);
    [HookMethod("Teleport")]
    public void Teleport(BasePlayer player, Vector3 newPosition, bool allowTPB, bool build, bool craft);
    private bool CanWake(BasePlayer player);
    public void RemoveProtections(ulong userid);
    public void StartSleeping(BasePlayer player);
    private void OnMapMarkerAdded(BasePlayer player, ProtoBuf.MapNote note);
    private string CanPlayerTeleport(BasePlayer player, Vector3[] vectors);
    private bool CanCraftHome(BasePlayer player);
    private bool CanCraftTPR(BasePlayer player);
    private List<string> monumentExceptions;
    private bool IsInAllowedMonument(Vector3 target, string mode);
    private string NearMonument(Vector3 target, bool check, string mode);
    private string CheckPlayer(BasePlayer player, bool build, bool craft, bool origin, string mode, bool allowcave);
    private bool IsWaterBlockedAbove(BasePlayer player, BaseEntity entity);
    private string CheckTargetLocation(BasePlayer player, Vector3 targetLocation, bool usableIntoBuildingBlocked, bool cupOwnerAllowOnBuildingBlocked);
    private bool CheckCupboardBlock(BuildingBlock block, BasePlayer player, bool cupOwnerAllowOnBuildingBlocked);
    private string CheckItems(BasePlayer player);
    private Collider[] colBuffer;
    private List<T> FindEntitiesOfType(Vector3 a, float n, int m);
    private bool IsInsideEntity(Vector3 a);
    private string IsInsideEntity(Vector3 targetLocation, ulong userid, string mode);
    private bool UnderneathFoundation(Vector3 a);
    private string CheckFoundation(ulong userid, Vector3 position, string mode);
    private bool IsBlockedOnIceberg(Vector3 position);
    private BuildingBlock GetFoundationOwned(Vector3 position, ulong userID);
    private bool IsAlly(ulong playerId, ulong targetId);
     bool IsBlockedUser(ulong playerid, ulong targetid);
    private bool PassesStrictCheck(BaseEntity entity, Vector3 position);
    private bool IsExternalWallOverlapped(Vector3 center, Vector3 position);
    private T FindEntity(BaseEntity entity);
    private T GetStandingOnEntity(BasePlayer player, int layerMask);
    private T GetStandingOnEntity(Vector3 a, int layerMask);
    private bool IsStandingOnEntity(Vector3 a, int layerMask, BaseEntity entity, string[] prefabs);
    private bool CheckBoundaries(float x, float y, float z);
    private Vector3 GetGroundBuilding(Vector3 a);
    public bool AboveWater(Vector3 a);
    private static bool ContainsTopology(TerrainTopology.Enum mask, Vector3 position, float radius);
    private bool IsInCave(Vector3 a);
    private bool GetLift(Vector3 position);
    private bool IsOnJunkPile(BasePlayer player);
    private bool IsAllowed(BasePlayer player, string perm);
    private bool IsAllowedMsg(BasePlayer player, string perm);
    private Effect reusableSoundEffectInstance;
    private void SendEffect(BasePlayer player, List<string> effects);
    private int GetHigher(BasePlayer player, Dictionary<string, int> limits, int limit, bool unlimited);
    private int GetLower(BasePlayer player, Dictionary<string, int> times, int time);
    private void CheckPerms(Dictionary<string, int> limits);
    private string _(string msgId, BasePlayer player, object[] args);
    private void PrintMsgL(IPlayer user, string msgId, object[] args);
    private void PrintMsgL(BasePlayer player, string msgId, object[] args);
    private void PrintMsgL(ulong userid, string msgId, object[] args);
    private void PrintMsg(BasePlayer player, string message);
    private static void DrawBox(BasePlayer player, Vector3 center, Quaternion rotation, Vector3 size);
    private static Vector3 RotatePointAroundPivot(Vector3 point, Vector3 pivot, Quaternion rotation);
    private ulong FindPlayersSingleId(string nameOrIdOrIp, BasePlayer player);
    private BasePlayer FindPlayersSingle(string value, BasePlayer player);
    private List<BasePlayer> FindPlayers(string arg, bool all);
    private class UnityVector3Converter : JsonConverter
    {
        public override void WriteJson(JsonWriter writer, object value, JsonSerializer serializer);
        public override object ReadJson(JsonReader reader, Type objectType, object existingValue, JsonSerializer serializer);
        public override bool CanConvert(Type objectType);
    }

    private class CustomComparerDictionaryCreationConverter : CustomCreationConverter<IDictionary>
    {
        private readonly IEqualityComparer<T> comparer;
        public CustomComparerDictionaryCreationConverter(IEqualityComparer<T> comparer);
        public override bool CanConvert(Type objectType);
        private static bool HasCompatibleInterface(Type objectType);
        private static bool HasGenericTypeDefinition(Type objectType, Type typeDefinition);
        private static bool HasCompatibleConstructor(Type objectType);
        public override IDictionary Create(Type objectType);
    }

    public class Exploits
    {
        public static bool TestInsideRock(Vector3 a);
        private static bool IsRockFaceDownwards(Vector3 a);
        private static bool IsRockFaceUpwards(Vector3 point);
        private static bool IsRock(string name);
    }

    [HookMethod("SendHelpText")]
    private void SendHelpText(BasePlayer player);
    private bool API_HavePendingRequest(BasePlayer player);
    private bool API_HaveAvailableHomes(BasePlayer player);
    private Dictionary<string, Vector3> API_GetHomes(BasePlayer player);
    private List<Vector3> API_GetLocations(string command);
    private Dictionary<string, List<Vector3>> API_GetAllLocations();
    private int GetLimitRemaining(BasePlayer player, string type);
    private int GetCooldownRemaining(BasePlayer player, string type);
    private int GetCountdownRemaining(BasePlayer player, string type);
}

private class MonumentInfoEx
{
    public MonumentInfo monument;
    public Vector3 position;
    public float radius;
    public string name;
    public string prefab;
    public MonumentInfoEx();
    public MonumentInfoEx(MonumentInfo monument, Vector3 position, float radius, string name, string prefab);
    public bool IsInBounds(Vector3 target);
}

public class InterruptSettings
{
    [JsonProperty(PropertyName = "Interrupt Teleport At Specific Monuments", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<string> Monuments { get; set; }
    [JsonProperty(PropertyName = "Above Water")]
    public bool AboveWater { get; set; }
    [JsonProperty(PropertyName = "Under Water")]
    public bool UnderWater { get; set; }
    [JsonProperty(PropertyName = "Balloon")]
    public bool Balloon { get; set; }
    [JsonProperty(PropertyName = "Boats")]
    public bool Boats { get; set; }
    [JsonProperty(PropertyName = "Cargo Ship")]
    public bool Cargo { get; set; }
    [JsonProperty(PropertyName = "Cold")]
    public bool Cold { get; set; }
    [JsonProperty(PropertyName = "Excavator")]
    public bool Excavator { get; set; }
    [JsonProperty(PropertyName = "Hot")]
    public bool Hot { get; set; }
    [JsonProperty(PropertyName = "Hostile")]
    public bool Hostile { get; set; }
    [JsonProperty(PropertyName = "Hurt")]
    public bool Hurt { get; set; }
    [JsonProperty(PropertyName = "Junkpiles")]
    public bool Junkpiles { get; set; }
    [JsonProperty(PropertyName = "Lift")]
    public bool Lift { get; set; }
    [JsonProperty(PropertyName = "Monument")]
    public bool Monument { get; set; }
    [JsonProperty(PropertyName = "Ignore Monument Marker Prefab")]
    public bool BypassMonumentMarker { get; set; }
    [JsonProperty(PropertyName = "Mounted")]
    public bool Mounted { get; set; }
    [JsonProperty(PropertyName = "Oil Rig")]
    public bool Oilrig { get; set; }
    [JsonProperty(PropertyName = "Safe Zone")]
    public bool Safe { get; set; }
    [JsonProperty(PropertyName = "Swimming")]
    public bool Swimming { get; set; }
}

public class PluginSettings
{
    [JsonProperty(PropertyName = "Delay Saving Data On Server Save")]
    public double SaveDelay { get; set; }
    [JsonProperty("TPB")]
    public TPBSettings TPB;
    [JsonProperty(PropertyName = "Interrupt TP")]
    public InterruptSettings Interrupt { get; set; }
    [JsonProperty(PropertyName = "Auto Wake Up After Teleport")]
    public bool AutoWakeUp { get; set; }
    [JsonProperty(PropertyName = "Respawn Players At Outpost")]
    public bool RespawnOutpost { get; set; }
    [JsonProperty(PropertyName = "Block Teleport (NoEscape)")]
    public bool BlockNoEscape { get; set; }
    [JsonProperty(PropertyName = "Block Teleport (ZoneManager)")]
    public bool BlockZoneFlag { get; set; }
    [JsonProperty(PropertyName = "Chat Name")]
    public string ChatName { get; set; }
    [JsonProperty(PropertyName = "Chat Steam64ID")]
    public ulong ChatID { get; set; }
    [JsonProperty(PropertyName = "Check Boundaries On Teleport X Y Z")]
    public bool CheckBoundaries { get; set; }
    [JsonProperty(PropertyName = "Check Boundaries Min Height")]
    public float BoundaryMin { get; set; }
    [JsonProperty(PropertyName = "Check Boundaries Max Height")]
    public float BoundaryMax { get; set; }
    [JsonProperty(PropertyName = "Check If Inside Rock")]
    public bool Rock { get; set; }
    [JsonProperty(PropertyName = "Height To Prevent Teleporting To/From (0 = disabled)")]
    public float ForcedBoundary { get; set; }
    [JsonProperty(PropertyName = "Data File Directory (Blank = Default)")]
    public string DataFileFolder { get; set; }
    [JsonProperty(PropertyName = "Draw Sphere On Set Home")]
    public bool DrawHomeSphere { get; set; }
    [JsonProperty(PropertyName = "Homes Enabled")]
    public bool HomesEnabled { get; set; }
    [JsonProperty(PropertyName = "TPR Enabled")]
    public bool TPREnabled { get; set; }
    [JsonProperty(PropertyName = "Strict Foundation Check")]
    public bool StrictFoundationCheck { get; set; }
    [JsonProperty(PropertyName = "Minimum Temp")]
    public float MinimumTemp { get; set; }
    [JsonProperty(PropertyName = "Maximum Temp")]
    public float MaximumTemp { get; set; }
    [JsonProperty(PropertyName = "Blocked Items", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public Dictionary<string, string> BlockedItems { get; set; }
    [JsonProperty(PropertyName = "Bypass CMD")]
    public string BypassCMD { get; set; }
    [JsonProperty(PropertyName = "Use Economics")]
    public bool UseEconomics { get; set; }
    [JsonProperty(PropertyName = "Use Server Rewards")]
    public bool UseServerRewards { get; set; }
    [JsonProperty(PropertyName = "Wipe On Upgrade Or Change")]
    public bool WipeOnUpgradeOrChange { get; set; }
    [JsonProperty(PropertyName = "Auto Generate Outpost Location")]
    public bool AutoGenOutpost { get; set; }
    [JsonProperty(PropertyName = "Outpost Map Prefab", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<string> Outpost { get; set; }
    [JsonProperty(PropertyName = "Auto Generate Bandit Location")]
    public bool AutoGenBandit { get; set; }
    [JsonProperty(PropertyName = "Bandit Map Prefab", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<string> Bandit { get; set; }
    [JsonProperty(PropertyName = "Show Time As Seconds Instead")]
    public bool UseSeconds { get; set; }
    [JsonProperty(PropertyName = "Use Quick Teleport")]
    public bool Quick { get; set; }
    [JsonProperty(PropertyName = "Chat Command Color")]
    public string ChatCommandColor;
    [JsonProperty(PropertyName = "Chat Command Argument Color")]
    public string ChatCommandArgumentColor;
    [JsonProperty("Enable Popup Support")]
    public bool UsePopup;
    [JsonProperty("Send Messages To Player")]
    public bool SendMessages;
    [JsonProperty("Block All Teleporting From Inside Authorized Base")]
    public bool BlockAuthorizedTeleporting;
    [JsonProperty("Global Teleport Cooldown")]
    public float Global;
    [JsonProperty("Global VIP Teleport Cooldown")]
    public float GlobalVIP;
    [JsonProperty("Play Sounds Before Teleport")]
    public bool PlaySoundsBeforeTeleport;
    [JsonProperty("Sound Effects Before Teleport", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<string> DisappearEffects;
    [JsonProperty("Play Sounds After Teleport")]
    public bool PlaySoundsAfterTeleport;
    [JsonProperty("Sound Effects After Teleport", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<string> ReappearEffects;
}

public class TPBSettings
{
    [JsonProperty(PropertyName = "VIP Countdowns", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public Dictionary<string, int> Countdowns { get; set; }
    [JsonProperty("Countdown")]
    public int Countdown;
    [JsonProperty("Available After X Seconds")]
    public int Time;
}

public class AdminSettings
{
    [JsonProperty(PropertyName = "Announce Teleport To Target")]
    public bool AnnounceTeleportToTarget { get; set; }
    [JsonProperty(PropertyName = "Usable By Admins")]
    public bool UseableByAdmins { get; set; }
    [JsonProperty(PropertyName = "Usable By Moderators")]
    public bool UseableByModerators { get; set; }
    [JsonProperty(PropertyName = "Location Radius")]
    public int LocationRadius { get; set; }
    [JsonProperty(PropertyName = "Teleport Near Default Distance")]
    public int TeleportNearDefaultDistance { get; set; }
    [JsonProperty(PropertyName = "Extra Distance To Block Monument Teleporting")]
    public int ExtraMonumentDistance { get; set; }
}

public class HomesSettings
{
    [JsonProperty(PropertyName = "Homes Limit")]
    public int HomesLimit { get; set; }
    [JsonProperty(PropertyName = "VIP Homes Limits", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public Dictionary<string, int> VIPHomesLimits { get; set; }
    [JsonProperty(PropertyName = "Allow Sethome At Specific Monuments", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<string> AllowedMonuments { get; set; }
    [JsonProperty(PropertyName = "Allow Sethome At All Monuments")]
    public bool AllowAtAllMonuments { get; set; }
    [JsonProperty(PropertyName = "Allow Sethome On Tugboats")]
    public bool AllowTugboats { get; set; }
    [JsonProperty(PropertyName = "Allow TPB")]
    public bool AllowTPB { get; set; }
    [JsonProperty(PropertyName = "Cooldown")]
    public int Cooldown { get; set; }
    [JsonProperty(PropertyName = "Countdown")]
    public int Countdown { get; set; }
    [JsonProperty(PropertyName = "Daily Limit")]
    public int DailyLimit { get; set; }
    [JsonProperty(PropertyName = "VIP Daily Limits", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public Dictionary<string, int> VIPDailyLimits { get; set; }
    [JsonProperty(PropertyName = "VIP Cooldowns", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public Dictionary<string, int> VIPCooldowns { get; set; }
    [JsonProperty(PropertyName = "VIP Countdowns", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public Dictionary<string, int> VIPCountdowns { get; set; }
    [JsonProperty(PropertyName = "Location Radius")]
    public int LocationRadius { get; set; }
    [JsonProperty(PropertyName = "Force On Top Of Foundation")]
    public bool ForceOnTopOfFoundation { get; set; }
    [JsonProperty(PropertyName = "Check Foundation For Owner")]
    public bool CheckFoundationForOwner { get; set; }
    [JsonProperty(PropertyName = "Use Friends")]
    public bool UseFriends { get; set; }
    [JsonProperty(PropertyName = "Use Clans")]
    public bool UseClans { get; set; }
    [JsonProperty(PropertyName = "Use Teams")]
    public bool UseTeams { get; set; }
    [JsonProperty(PropertyName = "Usable Out Of Building Blocked")]
    public bool UsableOutOfBuildingBlocked { get; set; }
    [JsonProperty(PropertyName = "Usable Into Building Blocked")]
    public bool UsableIntoBuildingBlocked { get; set; }
    [JsonProperty(PropertyName = "Usable From Safe Zone Only")]
    public bool UsableFromSafeZoneOnly { get; set; }
    [JsonProperty(PropertyName = "Allow Cupboard Owner When Building Blocked")]
    public bool CupOwnerAllowOnBuildingBlocked { get; set; }
    [JsonProperty(PropertyName = "Allow Iceberg")]
    public bool AllowIceberg { get; set; }
    [JsonProperty(PropertyName = "Allow Cave")]
    public bool AllowCave { get; set; }
    [JsonProperty(PropertyName = "Allow Crafting")]
    public bool AllowCraft { get; set; }
    [JsonProperty(PropertyName = "Allow Above Foundation")]
    public bool AllowAboveFoundation { get; set; }
    [JsonProperty(PropertyName = "Check If Home Is Valid On Listhomes")]
    public bool CheckValidOnList { get; set; }
    [JsonProperty(PropertyName = "Pay")]
    public int Pay { get; set; }
    [JsonProperty(PropertyName = "Bypass")]
    public int Bypass { get; set; }
    [JsonProperty(PropertyName = "Hours Before Useable After Wipe")]
    public double Hours { get; set; }
}

public class TPTSettings
{
    [JsonProperty(PropertyName = "Use Friends")]
    public bool UseFriends { get; set; }
    [JsonProperty(PropertyName = "Use Clans")]
    public bool UseClans { get; set; }
    [JsonProperty(PropertyName = "Use Teams")]
    public bool UseTeams { get; set; }
    [JsonProperty(PropertyName = "Allow Cave")]
    public bool AllowCave { get; set; }
    [JsonProperty(PropertyName = "Enabled Color")]
    public string EnabledColor { get; set; }
    [JsonProperty(PropertyName = "Disabled Color")]
    public string DisabledColor { get; set; }
}

public class TPRSettings
{
    [JsonProperty("Play Sounds To Request Target")]
    public bool PlaySoundsToRequestTarget;
    [JsonProperty("Teleport Request Sound Effects", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<string> TeleportRequestEffects;
    [JsonProperty("Play Sounds When Target Accepts")]
    public bool PlaySoundsWhenTargetAccepts;
    [JsonProperty("Teleport Accept Sound Effects", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<string> TeleportAcceptEffects;
    [JsonProperty(PropertyName = "Require Player To Be Friend, Clan Mate, Or Team Mate")]
    public bool UseClans_Friends_Teams { get; set; }
    [JsonProperty(PropertyName = "Require nteleportation.tpa to accept TPR requests")]
    public bool RequireTPAPermission { get; set; }
    [JsonProperty(PropertyName = "Allow Cave")]
    public bool AllowCave { get; set; }
    [JsonProperty(PropertyName = "Allow TPB")]
    public bool AllowTPB { get; set; }
    [JsonProperty(PropertyName = "Use Blocked Users")]
    public bool UseBlockedUsers { get; set; }
    [JsonProperty(PropertyName = "Cooldown")]
    public int Cooldown { get; set; }
    [JsonProperty(PropertyName = "Countdown")]
    public int Countdown { get; set; }
    [JsonProperty(PropertyName = "Daily Limit")]
    public int DailyLimit { get; set; }
    [JsonProperty(PropertyName = "VIP Daily Limits", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public Dictionary<string, int> VIPDailyLimits { get; set; }
    [JsonProperty(PropertyName = "VIP Cooldowns", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public Dictionary<string, int> VIPCooldowns { get; set; }
    [JsonProperty(PropertyName = "VIP Countdowns", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public Dictionary<string, int> VIPCountdowns { get; set; }
    [JsonProperty(PropertyName = "Enable Request UI")]
    public bool UI { get; set; }
    [JsonProperty(PropertyName = "Request Duration")]
    public int RequestDuration { get; set; }
    [JsonProperty(PropertyName = "Block TPA On Ceiling")]
    public bool BlockTPAOnCeiling { get; set; }
    [JsonProperty(PropertyName = "Usable Out Of Building Blocked")]
    public bool UsableOutOfBuildingBlocked { get; set; }
    [JsonProperty(PropertyName = "Usable Into Building Blocked")]
    public bool UsableIntoBuildingBlocked { get; set; }
    [JsonProperty(PropertyName = "Allow Cupboard Owner When Building Blocked")]
    public bool CupOwnerAllowOnBuildingBlocked { get; set; }
    [JsonProperty(PropertyName = "Allow Crafting")]
    public bool AllowCraft { get; set; }
    [JsonProperty(PropertyName = "Pay")]
    public int Pay { get; set; }
    [JsonProperty(PropertyName = "Bypass")]
    public int Bypass { get; set; }
    [JsonProperty(PropertyName = "Hours Before Useable After Wipe")]
    public double Hours { get; set; }
}

public class TownSettings
{
    [JsonProperty(PropertyName = "Command Enabled")]
    public bool Enabled { get; set; }
    [JsonProperty(PropertyName = "Set Position From Monument Marker Name")]
    public string MonumentMarkerName { get; set; }
    [JsonProperty(PropertyName = "Set Position From Monument Marker Name Offset")]
    public string MonumentMarkerNameOffset { get; set; }
    [JsonProperty(PropertyName = "Allow TPB")]
    public bool AllowTPB { get; set; }
    [JsonProperty(PropertyName = "Allow Cave")]
    public bool AllowCave { get; set; }
    [JsonProperty(PropertyName = "Cooldown")]
    public int Cooldown { get; set; }
    [JsonProperty(PropertyName = "Countdown")]
    public int Countdown { get; set; }
    [JsonProperty(PropertyName = "Daily Limit")]
    public int DailyLimit { get; set; }
    [JsonProperty(PropertyName = "VIP Daily Limits", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public Dictionary<string, int> VIPDailyLimits { get; set; }
    [JsonProperty(PropertyName = "VIP Cooldowns", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public Dictionary<string, int> VIPCooldowns { get; set; }
    [JsonProperty(PropertyName = "VIP Countdowns", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public Dictionary<string, int> VIPCountdowns { get; set; }
    [JsonProperty(PropertyName = "Location")]
    public Vector3 Location { get; set; }
    [JsonProperty(PropertyName = "Locations", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<Vector3> Locations { get; set; }
    [JsonProperty(PropertyName = "Teleport To Random Location")]
    public bool Random { get; set; }
    [JsonProperty(PropertyName = "Usable Out Of Building Blocked")]
    public bool UsableOutOfBuildingBlocked { get; set; }
    [JsonProperty(PropertyName = "Allow Crafting")]
    public bool AllowCraft { get; set; }
    [JsonProperty(PropertyName = "Pay")]
    public int Pay { get; set; }
    [JsonProperty(PropertyName = "Bypass")]
    public int Bypass { get; set; }
    [JsonProperty(PropertyName = "Hours Before Useable After Wipe")]
    public double Hours { get; set; }
    public bool CanCraft(BasePlayer player, string command);
    [JsonIgnore]
    public StoredData Teleports;
    [JsonIgnore]
    public string Command { get; set; }
}

private class Configuration
{
    [JsonProperty(PropertyName = "Settings")]
    public PluginSettings Settings;
    [JsonProperty(PropertyName = "Admin")]
    public AdminSettings Admin;
    [JsonProperty(PropertyName = "Home")]
    public HomesSettings Home;
    [JsonProperty(PropertyName = "TPT")]
    public TPTSettings TPT;
    [JsonProperty(PropertyName = "TPR")]
    public TPRSettings TPR;
    [JsonProperty(PropertyName = "Dynamic Commands", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public Dictionary<string, TownSettings> DynamicCommands { get; set; }
}

private class DisabledData
{
    [JsonProperty("List of disabled commands")]
    public List<string> DisabledCommands;
    public DisabledData();
}

private class AdminData
{
    [JsonProperty("pl")]
    public Vector3 PreviousLocation { get; set; }
    [JsonProperty("b")]
    public bool BuildingBlocked { get; set; }
    [JsonProperty("c")]
    public bool AllowCrafting { get; set; }
    [JsonProperty("l")]
    public Dictionary<string, Vector3> Locations { get; set; }
}

private class HomeData
{
    public class Entry
    {
        public Vector3 Position;
        public BaseNetworkable Entity;
        public bool isEntity { get; set; }
        public bool wasEntity;
        public Entry();
        public Entry(Vector3 Position);
        public Vector3 Get();
    }

    public class Boat
    {
        public ulong Value;
        public Vector3 Offset;
        public Boat();
        public Boat(Entry entry);
    }

    [JsonProperty("l")]
    public Dictionary<string, Vector3> buildings { get; set; }
    [JsonProperty("b")]
    public Dictionary<string, Boat> boats { get; set; }
    [JsonProperty("t")]
    public TeleportData Teleports { get; set; }
    [JsonIgnore]
    private Dictionary<string, Entry> Cache;
    [JsonIgnore]
    public Dictionary<string, Entry> Locations { get; set; }
    private void InitializeBuildings();
    private void InitializeBoats();
    public bool TryGetValue(string key, Entry homeEntry);
    public void Set(string key, Entry homeEntry);
    public bool Remove(string key);
}

public class Entry
{
    public Vector3 Position;
    public BaseNetworkable Entity;
    public bool isEntity { get; set; }
    public bool wasEntity;
    public Entry();
    public Entry(Vector3 Position);
    public Vector3 Get();
}

public class Boat
{
    public ulong Value;
    public Vector3 Offset;
    public Boat();
    public Boat(Entry entry);
}

public class TeleportData
{
    [JsonProperty("a")]
    public int Amount { get; set; }
    [JsonProperty("d")]
    public string Date { get; set; }
    [JsonProperty("t")]
    public int Timestamp { get; set; }
}

private class TeleportTimer
{
    public Timer Timer { get; set; }
    public BasePlayer OriginPlayer { get; set; }
    public BasePlayer TargetPlayer { get; set; }
}

public class StoredData
{
    public Dictionary<ulong, TeleportData> TPData;
    public bool Changed;
}

private class UnityVector3Converter : JsonConverter
{
    public override void WriteJson(JsonWriter writer, object value, JsonSerializer serializer);
    public override object ReadJson(JsonReader reader, Type objectType, object existingValue, JsonSerializer serializer);
    public override bool CanConvert(Type objectType);
}

private class CustomComparerDictionaryCreationConverter : CustomCreationConverter<IDictionary>
{
    private readonly IEqualityComparer<T> comparer;
    public CustomComparerDictionaryCreationConverter(IEqualityComparer<T> comparer);
    public override bool CanConvert(Type objectType);
    private static bool HasCompatibleInterface(Type objectType);
    private static bool HasGenericTypeDefinition(Type objectType, Type typeDefinition);
    private static bool HasCompatibleConstructor(Type objectType);
    public override IDictionary Create(Type objectType);
}

public class Exploits
{
    public static bool TestInsideRock(Vector3 a);
    private static bool IsRockFaceDownwards(Vector3 a);
    private static bool IsRockFaceUpwards(Vector3 point);
    private static bool IsRock(string name);
}


```

---

## NukeWeapons

```csharp
using System;
using System.Collections.Generic;
using Oxide.Core;
using Oxide.Core.Configuration;
using Oxide.Game.Rust.Cui;
using Oxide.Core.Plugins;
using UnityEngine;
using System.Linq;
using System.Reflection;
using System.Collections;
using System.IO;
using Rust;

Oxide.Plugins
[Info("NukeWeapons", "k1lly0u", "0.1.5", ResourceId = 2044)]
 class NukeWeapons : RustPlugin
{
    [PluginReference]
     Plugin LustyMap;
     NukeData nukeData;
     ItemNames itemNames;
    private DynamicConfigFile data;
    private DynamicConfigFile Item_Names;
    static GameObject webObject;
    static UnityWeb uWeb;
    static MethodInfo getFileData;
    private static readonly int playerLayer;
    private static readonly Collider[] colBuffer;
    private List<ZoneList> RadiationZones;
    private Dictionary<ulong, NukeType> activeUsers;
    private Dictionary<ulong, Dictionary<NukeType, int>> cachedAmmo;
    private Dictionary<ulong, Dictionary<NukeType, double>> craftingTimers;
    private List<Timer> nwTimers;
    private Dictionary<string, ItemDefinition> ItemDefs;
    private Dictionary<string, string> DisplayNames;
     void Loaded();
     void OnServerInitialized();
     void Unload();
     void OnPlayerDisconnected(BasePlayer player);
     void OnEntityDeath(BaseEntity entity, HitInfo hitinfo);
     void OnRocketLaunched(BasePlayer player, BaseEntity entity);
     void OnEntitySpawned(BaseNetworkable entity);
     void OnEntityDeath(BaseCombatEntity entity, HitInfo info);
     void OnPlayerAttack(BasePlayer attacker, HitInfo info);
     void OnExplosiveThrown(BasePlayer player, BaseEntity entity);
    private bool HasEnoughRes(BasePlayer player, int itemid, int amount);
    private void TakeResources(BasePlayer player, int itemid, int amount);
    static double GrabCurrentTime();
    private bool IsType(BasePlayer player, NukeType type);
    private void FindAllMines();
    private bool CanCraft(BasePlayer player, NukeType type);
    private bool AlreadyCrafting(BasePlayer player, NukeType type);
    private string CraftTimeClock(BasePlayer player, NukeType type);
    private void StartCrafting(BasePlayer player, NukeType type);
    private void FinishCraftingItems(BasePlayer player, NukeType type);
    private bool FinishedCrafting(BasePlayer player);
    private void CloseMap(BasePlayer player);
    private void OpenMap(BasePlayer player);
     class NWUI
    {
        static public CuiElementContainer CreateElementContainer(string panelName, string color, string aMin, string aMax, bool cursor, string parent);
        static public void CreatePanel(CuiElementContainer container, string panel, string color, string aMin, string aMax, bool cursor);
        static public void CreateLabel(CuiElementContainer container, string panel, string color, string text, int size, string aMin, string aMax, TextAnchor align, float fadein);
        static public void CreateButton(CuiElementContainer container, string panel, string color, string text, int size, string aMin, string aMax, string command, TextAnchor align, float fadein);
        static public void LoadImage(CuiElementContainer container, string panel, string png, string aMin, string aMax);
        public static string CreateTextOverlay(CuiElementContainer container, string panelName, string textcolor, string text, int size, string distance, string olcolor, string aMin, string aMax, TextAnchor align);
    }

    private Dictionary<string, string> UIColors;
    static string UIMain;
    static string UIPanel;
    static string UIEntry;
    static string UIIcon;
    private void OpenCraftingMenu(BasePlayer player);
    private void CraftingElement(BasePlayer player, NukeType type);
    private void CreateCraftTimer(BasePlayer player);
    private void CreateIngredientEntry(CuiElementContainer container, string panel, string name, int amountreq, int plyrhas, int number);
    private void CreateAmmoIcons(BasePlayer player);
    private void AmmoIcon(BasePlayer player, NukeType type, int number);
    private void AddButtons(BasePlayer player, int number);
    private void CreateMenuButton(CuiElementContainer container, string panelName, string buttonname, string command, int number);
    [ConsoleCommand("NWUI_Craft")]
    private void cmdNWCraft(ConsoleSystem.Arg arg);
    [ConsoleCommand("NWUI_DeactivateMenu")]
    private void cmdNWDeActivate(ConsoleSystem.Arg arg);
    [ConsoleCommand("NWUI_DeactivateButton")]
    private void cmdNWDeActivateButton(ConsoleSystem.Arg arg);
    [ConsoleCommand("NWUI_DeactivateIcons")]
    private void cmdNWDeactivateIcons(ConsoleSystem.Arg arg);
    [ConsoleCommand("NWUI_OpenMenu")]
    private void cmdNWOpenMenu(ConsoleSystem.Arg arg);
    [ConsoleCommand("NWUI_Activate")]
    private void cmdNWActivate(ConsoleSystem.Arg arg);
    [ConsoleCommand("NWUI_ChangeElement")]
    private void cmdNWChangeElement(ConsoleSystem.Arg arg);
    [ConsoleCommand("NWUI_DestroyAll")]
    private void cmdNWDestroyAll(ConsoleSystem.Arg arg);
     void DestroyCraftUI(BasePlayer player);
     void DestroyIconUI(BasePlayer player);
    private void InitializePlugin();
    private bool HasAmmo(ulong player, NukeType type);
    private Dictionary<string, int> GetCraftingComponents(NukeType type);
    private NWType GetConfigFromType(NukeType type);
    private void CheckPlayerEntry(BasePlayer player);
    private void InitializeZone(Vector3 Location, float intensity, float duration, float radius, bool explosionType);
    private void DestroyZone(ZoneList zone);
     class Nuke : MonoBehaviour
    {
        public NukeWeapons instance;
        public NukeType type;
        public RadiationStats stats;
        private void OnDestroy();
        public void InitializeComponent(NukeWeapons ins, NukeType typ, RadiationStats sta);
    }

    public class ZoneList
    {
        public RadZones zone;
        public Timer time;
    }

    public class RadZones : MonoBehaviour
    {
        private int ID;
        private Vector3 Position;
        private float ZoneRadius;
        private float RadiationAmount;
        private List<BasePlayer> InZone;
        private void Awake();
        public void Activate(Vector3 pos, float radius, float amount);
        private void OnDestroy();
        private void UpdateCollider();
        private void UpdateTrigger();
    }

    [ChatCommand("nw")]
    private void cmdNukes(BasePlayer player, string command, string[] args);
    private bool canRocket(BasePlayer player);
    private bool canBullet(BasePlayer player);
    private bool canMine(BasePlayer player);
    private bool canExplosive(BasePlayer player);
    private bool canGrenade(BasePlayer player);
    private bool canAll(BasePlayer player);
    private bool hasUnlimited(BasePlayer player);
    private ConfigData configData;
     class NWType
    {
        public bool Enabled { get; set; }
        public int MaxAllowed { get; set; }
        public int CraftTime { get; set; }
        public int CraftAmount { get; set; }
        public Dictionary<string, int> CraftingCosts { get; set; }
        public RadiationStats RadiationProperties { get; set; }
    }

     class RadiationStats
    {
        public float Intensity { get; set; }
        public float Duration { get; set; }
        public float Radius { get; set; }
    }

     class Options
    {
        public string MSG_MainColor { get; set; }
        public string MSG_SecondaryColor { get; set; }
    }

     class ConfigData
    {
        public NWType Mines { get; set; }
        public NWType Rockets { get; set; }
        public NWType Bullets { get; set; }
        public NWType Grenades { get; set; }
        public NWType Explosives { get; set; }
        public Options Options { get; set; }
        public Dictionary<string, string> URL_IconList { get; set; }
    }

    private void LoadVariables();
    protected override void LoadDefaultConfig();
    private void LoadConfigVariables();
     void SaveConfig(ConfigData config);
     void SaveData();
     void SaveDisplayNames();
     void LoadData();
     class NukeData
    {
        public Dictionary<ulong, Dictionary<NukeType, int>> ammo;
        public List<uint> Mines;
        public Dictionary<string, uint> ImageIDs;
    }

     class ItemNames
    {
        public Dictionary<string, string> DisplayNames;
    }

     class PlayerAmmo
    {
        public int Rockets;
        public int Mines;
        public int Bullets;
        public int Explosives;
        public int Grenades;
    }

     class QueueItem
    {
        public string url;
        public string imagename;
        public QueueItem(string ur, string na);
    }

     class UnityWeb : MonoBehaviour
    {
         NukeWeapons filehandler;
        const int MaxActiveLoads;
        private Queue<QueueItem> QueueList;
        static byte activeLoads;
        private MemoryStream stream;
        private void Awake();
        private void OnDestroy();
        public void Add(string url, string imagename);
         void Next();
        private void ClearStream();
         IEnumerator WaitForRequest(QueueItem info);
    }

    [ConsoleCommand("nukeicons")]
    private void cmdNukeIcons(ConsoleSystem.Arg arg);
    private void SendMSG(BasePlayer player, string message, string message2);
    private string MSG(string key, string playerid);
     Dictionary<string, string> Messages;
}

 class NWUI
{
    static public CuiElementContainer CreateElementContainer(string panelName, string color, string aMin, string aMax, bool cursor, string parent);
    static public void CreatePanel(CuiElementContainer container, string panel, string color, string aMin, string aMax, bool cursor);
    static public void CreateLabel(CuiElementContainer container, string panel, string color, string text, int size, string aMin, string aMax, TextAnchor align, float fadein);
    static public void CreateButton(CuiElementContainer container, string panel, string color, string text, int size, string aMin, string aMax, string command, TextAnchor align, float fadein);
    static public void LoadImage(CuiElementContainer container, string panel, string png, string aMin, string aMax);
    public static string CreateTextOverlay(CuiElementContainer container, string panelName, string textcolor, string text, int size, string distance, string olcolor, string aMin, string aMax, TextAnchor align);
}

 class Nuke : MonoBehaviour
{
    public NukeWeapons instance;
    public NukeType type;
    public RadiationStats stats;
    private void OnDestroy();
    public void InitializeComponent(NukeWeapons ins, NukeType typ, RadiationStats sta);
}

public class ZoneList
{
    public RadZones zone;
    public Timer time;
}

public class RadZones : MonoBehaviour
{
    private int ID;
    private Vector3 Position;
    private float ZoneRadius;
    private float RadiationAmount;
    private List<BasePlayer> InZone;
    private void Awake();
    public void Activate(Vector3 pos, float radius, float amount);
    private void OnDestroy();
    private void UpdateCollider();
    private void UpdateTrigger();
}

 class NWType
{
    public bool Enabled { get; set; }
    public int MaxAllowed { get; set; }
    public int CraftTime { get; set; }
    public int CraftAmount { get; set; }
    public Dictionary<string, int> CraftingCosts { get; set; }
    public RadiationStats RadiationProperties { get; set; }
}

 class RadiationStats
{
    public float Intensity { get; set; }
    public float Duration { get; set; }
    public float Radius { get; set; }
}

 class Options
{
    public string MSG_MainColor { get; set; }
    public string MSG_SecondaryColor { get; set; }
}

 class ConfigData
{
    public NWType Mines { get; set; }
    public NWType Rockets { get; set; }
    public NWType Bullets { get; set; }
    public NWType Grenades { get; set; }
    public NWType Explosives { get; set; }
    public Options Options { get; set; }
    public Dictionary<string, string> URL_IconList { get; set; }
}

 class NukeData
{
    public Dictionary<ulong, Dictionary<NukeType, int>> ammo;
    public List<uint> Mines;
    public Dictionary<string, uint> ImageIDs;
}

 class ItemNames
{
    public Dictionary<string, string> DisplayNames;
}

 class PlayerAmmo
{
    public int Rockets;
    public int Mines;
    public int Bullets;
    public int Explosives;
    public int Grenades;
}

 class QueueItem
{
    public string url;
    public string imagename;
    public QueueItem(string ur, string na);
}

 class UnityWeb : MonoBehaviour
{
     NukeWeapons filehandler;
    const int MaxActiveLoads;
    private Queue<QueueItem> QueueList;
    static byte activeLoads;
    private MemoryStream stream;
    private void Awake();
    private void OnDestroy();
    public void Add(string url, string imagename);
     void Next();
    private void ClearStream();
     IEnumerator WaitForRequest(QueueItem info);
}


```

---

## NukeWipe

```csharp
using System.Collections.Generic;
using UnityEngine;
using System.Linq;
using Facepunch;
using System;

Oxide.Plugins
[Info("NukeWipe", "LaserHydra", "1.0.0", ResourceId = 0)]
[Description("Wipe with style - and propably lag lol")]
 class NukeWipe : RustPlugin
{
     Type[] DestroyableTypes;
     void Loaded();
     void LoadConfig();
     void LoadMessages();
    protected override void LoadDefaultConfig();
    [ChatCommand("nuke")]
     void cmdNuke(BasePlayer player);
     void Nuke();
     void Explode(BasePlayer player);
     bool ShouldBeDestroyed(BaseEntity entity);
     List<T> GetEntities(Vector3 position, int radius);
     string ListToString(List<T> list, int first, string seperator);
     List<T> CopyList(List<T> list);
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


```

---

## OnlinePanel

```csharp
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Game.Rust.Cui;
using System;
using System.Collections;
using System.Collections.Generic;
using System.Globalization;
using System.Linq;
using UnityEngine;

Oxide.Plugins
[Info("OnlinePanel", "OxideBro", "0.1.1")]
public class OnlinePanel : RustPlugin
{
    public class PlayerTime
    {
        public TimeSpan time;
        public Coroutine coroutine;
    }

    public Dictionary<BasePlayer, PlayerTime> PlayersTime;
    private PluginConfig config;
    private Coroutine UpdateActionValues;
    public bool CargoPlane;
    public bool BradleyAPC;
    public bool BaseHelicopter;
    public bool CargoShip;
    public bool CH47Helicopter;
    public bool Init;
    private IEnumerator UpdateValues();
    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    private void UpdateConfigValues();
    protected override void SaveConfig();
     void OnPlayerConnected(BasePlayer player);
     void Unload();
    private IEnumerator StartUpdate(BasePlayer player);
     class PluginConfig
    {
        [JsonProperty("Версия конфигурации")]
        public VersionNumber PluginVersion;
        [JsonProperty("Размер текста")]
        public int TextSize;
        [JsonProperty("Шаблон верх ({0} Игровое время | {1} ВРЕМЯ ИГРОКА | {2} ОНЛАЙН | {3} ПОДКЛЮЧЕНИЯ | {4} СЛИПЕРЫ)")]
        public string TextHeader;
        [JsonProperty("Шаблон низ")]
        public string TextFooter;
        [JsonProperty("Цвет подсветки, когда есть танк, корабль и тд")]
        public string ActiveColor;
        [JsonProperty("Цвет неосновного текста с прозрачностью")]
        public string PassiveColor;
        [JsonProperty("Название танка")]
        public string PAnzerName;
        [JsonProperty("Название корбля")]
        public string ShipName;
        [JsonProperty("Название самолета")]
        public string AirName;
        [JsonProperty("Название вертолета")]
        public string HeliName;
        [JsonProperty("Название чинука")]
        public string ChinookName;
        [JsonProperty("Расоложение верхнего шаблона - MAX")]
        public string HeaderMax;
        [JsonProperty("Расоложение верхнего шаблона - MIN")]
        public string HeaderMin;
        [JsonProperty("Расоложение нижнего шаблона - MAX")]
        public string FooterMax;
        [JsonProperty("Расоложение нижнего шаблона - MIN")]
        public string FooterMin;
        [JsonProperty("Частота обновления игрового времени")]
        public float UpdateTime;
        [JsonProperty("Ключ от ipgeolocation.io (Для получения времени игрока, сервис бесплатный)")]
        public string IpGeoAPIKey;
        public static PluginConfig DefaultConfig();
    }

     class Parser
    {
        public string date_time;
    }

     void OnServerInitialized();
     void CreateMenu(BasePlayer player);
    private static string HexToRustFormat(string hex);
}

public class PlayerTime
{
    public TimeSpan time;
    public Coroutine coroutine;
}

 class PluginConfig
{
    [JsonProperty("Версия конфигурации")]
    public VersionNumber PluginVersion;
    [JsonProperty("Размер текста")]
    public int TextSize;
    [JsonProperty("Шаблон верх ({0} Игровое время | {1} ВРЕМЯ ИГРОКА | {2} ОНЛАЙН | {3} ПОДКЛЮЧЕНИЯ | {4} СЛИПЕРЫ)")]
    public string TextHeader;
    [JsonProperty("Шаблон низ")]
    public string TextFooter;
    [JsonProperty("Цвет подсветки, когда есть танк, корабль и тд")]
    public string ActiveColor;
    [JsonProperty("Цвет неосновного текста с прозрачностью")]
    public string PassiveColor;
    [JsonProperty("Название танка")]
    public string PAnzerName;
    [JsonProperty("Название корбля")]
    public string ShipName;
    [JsonProperty("Название самолета")]
    public string AirName;
    [JsonProperty("Название вертолета")]
    public string HeliName;
    [JsonProperty("Название чинука")]
    public string ChinookName;
    [JsonProperty("Расоложение верхнего шаблона - MAX")]
    public string HeaderMax;
    [JsonProperty("Расоложение верхнего шаблона - MIN")]
    public string HeaderMin;
    [JsonProperty("Расоложение нижнего шаблона - MAX")]
    public string FooterMax;
    [JsonProperty("Расоложение нижнего шаблона - MIN")]
    public string FooterMin;
    [JsonProperty("Частота обновления игрового времени")]
    public float UpdateTime;
    [JsonProperty("Ключ от ipgeolocation.io (Для получения времени игрока, сервис бесплатный)")]
    public string IpGeoAPIKey;
    public static PluginConfig DefaultConfig();
}

 class Parser
{
    public string date_time;
}


```

---

## OnlinePlus

```csharp
using System.Collections.Generic;
using System.Linq;

Oxide.Plugins
[Info("OnlinePlus", "Sempai#3239", "1.0.1")]
[Description("OnlinePlus")]
 class OnlinePlus : RustPlugin
{
    private const string Permfromuse;
    private const string DeniedEffectPrefab;
    private string Moderperm;
    private void Init();
    protected override void LoadDefaultMessages();
    [ChatCommand("online")]
     void Chat_Online(BasePlayer player);
    [ChatCommand("players")]
     void Chat_Players(BasePlayer player);
    private bool HasPermission(BasePlayer player, string perm);
    private string GetLocal(string messageKey, string userId);
    private void InChat(BasePlayer player, string msgId, object[] args);
}


```

---

## OreBonus

```csharp


```

---

## OurServers

```csharp
using System.Collections;
using System.Collections.Generic;
using System.Linq;
using Newtonsoft.Json;
using Oxide.Game.Rust.Cui;
using UnityEngine;

Oxide.Plugins
[Info("Our Servers", "Sempai#3239", "1.0.0")]
[Description("Show information about other servers (with fetching current online)")]
public class OurServers : RustPlugin
{
    private class Server
    {
        [JsonProperty("Name of your server in interface")]
        public string DisplayName;
        [JsonProperty("Description of your server in interface")]
        public string Description;
        [JsonProperty("IP:Port of server (ex: 127.0.0.1:12000)")]
        public string IPAndPort;
        [JsonProperty("Interface width (increase it, if your description is very length)")]
        public int ServerWidth;
        [JsonIgnore]
        public int CurrentOnline;
        [JsonIgnore]
        public int MaxOnline;
        [JsonIgnore]
        public bool Status;
        public string IP();
        public int Port();
        public int GetOnline();
        public void UpdateStatus(bool status);
    }

    private class Configuration
    {
        [JsonProperty("Servers configure")]
        public List<Server> Servers;
        [JsonProperty("Command to open interface")]
        public string Command;
        [JsonProperty("Use chat instead of UI")]
        public bool UseChat;
        [JsonProperty("Information update interval in seconds (should be > 30)")]
        public int Interval;
        [JsonProperty("Which API to use? (Possible options: steampowered or battlemetrics) (Default: battlemetrics)")]
        public string API;
        [JsonProperty("Steampowered API key")]
        public string SteamWebApiKey;
        public static Configuration Generate();
    }

    public class BattlemetricsApiResponse
    {
        [JsonProperty("data")]
        public List<ServerData> data;
        public class ServerData
        {
            [JsonProperty("id")]
            public string id;
            [JsonProperty("type")]
            public string type;
            [JsonProperty("attributes")]
            public Attributes attributes;
            public class Attributes
            {
                [JsonProperty("id")]
                public string id;
                [JsonProperty("name")]
                public string name;
                [JsonProperty("ip")]
                public string ip;
                [JsonProperty("port")]
                public int port;
                [JsonProperty("portQuery")]
                public int portQuery;
                [JsonProperty("players")]
                public int players;
                [JsonProperty("maxPlayers")]
                public int maxPlayers;
                [JsonProperty("status")]
                public string status;
            }

        }

    }

    public class SteampoweredApi
    {
        [JsonProperty("response")]
        public Response response;
        public class Response
        {
            [JsonProperty("servers")]
            public List<ServerData> servers;
            public class ServerData
            {
                [JsonProperty("addr")]
                public string addr;
                [JsonProperty("gameport")]
                public int gameport;
                [JsonProperty("steamid")]
                public string steamid;
                [JsonProperty("name")]
                public string name;
                [JsonProperty("appid")]
                public int appid;
                [JsonProperty("gamedir")]
                public string gamedir;
                [JsonProperty("version")]
                public string version;
                [JsonProperty("product")]
                public string product;
                [JsonProperty("region")]
                public int region;
                [JsonProperty("players")]
                public int players;
                [JsonProperty("max_players")]
                public int max_players;
                [JsonProperty("bots")]
                public int bots;
                [JsonProperty("map")]
                public string map;
                [JsonProperty("secure")]
                public bool secure;
                [JsonProperty("dedicated")]
                public bool dedicated;
                [JsonProperty("os")]
                public string os;
                [JsonProperty("gametype")]
                public string gametype;
            }

        }

    }

    private Coroutine UpdateAction;
    private Configuration Settings;
    private bool Initialized;
    private bool Broken;
    protected override void LoadConfig();
    protected override void SaveConfig();
    protected override void LoadDefaultConfig();
    private void OnServerInitialized();
    private void Unload();
    private IEnumerator UpdateOnline();
    [ConsoleCommand("UI_OurServersHandler")]
    private void CmdConsoleHandler(ConsoleSystem.Arg args);
    private void CmdShowServers(BasePlayer player, string command, string[] args);
    private const string Layer;
    private void UI_DrawInterface(BasePlayer player, int index);
}

private class Server
{
    [JsonProperty("Name of your server in interface")]
    public string DisplayName;
    [JsonProperty("Description of your server in interface")]
    public string Description;
    [JsonProperty("IP:Port of server (ex: 127.0.0.1:12000)")]
    public string IPAndPort;
    [JsonProperty("Interface width (increase it, if your description is very length)")]
    public int ServerWidth;
    [JsonIgnore]
    public int CurrentOnline;
    [JsonIgnore]
    public int MaxOnline;
    [JsonIgnore]
    public bool Status;
    public string IP();
    public int Port();
    public int GetOnline();
    public void UpdateStatus(bool status);
}

private class Configuration
{
    [JsonProperty("Servers configure")]
    public List<Server> Servers;
    [JsonProperty("Command to open interface")]
    public string Command;
    [JsonProperty("Use chat instead of UI")]
    public bool UseChat;
    [JsonProperty("Information update interval in seconds (should be > 30)")]
    public int Interval;
    [JsonProperty("Which API to use? (Possible options: steampowered or battlemetrics) (Default: battlemetrics)")]
    public string API;
    [JsonProperty("Steampowered API key")]
    public string SteamWebApiKey;
    public static Configuration Generate();
}

public class BattlemetricsApiResponse
{
    [JsonProperty("data")]
    public List<ServerData> data;
    public class ServerData
    {
        [JsonProperty("id")]
        public string id;
        [JsonProperty("type")]
        public string type;
        [JsonProperty("attributes")]
        public Attributes attributes;
        public class Attributes
        {
            [JsonProperty("id")]
            public string id;
            [JsonProperty("name")]
            public string name;
            [JsonProperty("ip")]
            public string ip;
            [JsonProperty("port")]
            public int port;
            [JsonProperty("portQuery")]
            public int portQuery;
            [JsonProperty("players")]
            public int players;
            [JsonProperty("maxPlayers")]
            public int maxPlayers;
            [JsonProperty("status")]
            public string status;
        }

    }

}

public class ServerData
{
    [JsonProperty("id")]
    public string id;
    [JsonProperty("type")]
    public string type;
    [JsonProperty("attributes")]
    public Attributes attributes;
    public class Attributes
    {
        [JsonProperty("id")]
        public string id;
        [JsonProperty("name")]
        public string name;
        [JsonProperty("ip")]
        public string ip;
        [JsonProperty("port")]
        public int port;
        [JsonProperty("portQuery")]
        public int portQuery;
        [JsonProperty("players")]
        public int players;
        [JsonProperty("maxPlayers")]
        public int maxPlayers;
        [JsonProperty("status")]
        public string status;
    }

}

public class Attributes
{
    [JsonProperty("id")]
    public string id;
    [JsonProperty("name")]
    public string name;
    [JsonProperty("ip")]
    public string ip;
    [JsonProperty("port")]
    public int port;
    [JsonProperty("portQuery")]
    public int portQuery;
    [JsonProperty("players")]
    public int players;
    [JsonProperty("maxPlayers")]
    public int maxPlayers;
    [JsonProperty("status")]
    public string status;
}

public class SteampoweredApi
{
    [JsonProperty("response")]
    public Response response;
    public class Response
    {
        [JsonProperty("servers")]
        public List<ServerData> servers;
        public class ServerData
        {
            [JsonProperty("addr")]
            public string addr;
            [JsonProperty("gameport")]
            public int gameport;
            [JsonProperty("steamid")]
            public string steamid;
            [JsonProperty("name")]
            public string name;
            [JsonProperty("appid")]
            public int appid;
            [JsonProperty("gamedir")]
            public string gamedir;
            [JsonProperty("version")]
            public string version;
            [JsonProperty("product")]
            public string product;
            [JsonProperty("region")]
            public int region;
            [JsonProperty("players")]
            public int players;
            [JsonProperty("max_players")]
            public int max_players;
            [JsonProperty("bots")]
            public int bots;
            [JsonProperty("map")]
            public string map;
            [JsonProperty("secure")]
            public bool secure;
            [JsonProperty("dedicated")]
            public bool dedicated;
            [JsonProperty("os")]
            public string os;
            [JsonProperty("gametype")]
            public string gametype;
        }

    }

}

public class Response
{
    [JsonProperty("servers")]
    public List<ServerData> servers;
    public class ServerData
    {
        [JsonProperty("addr")]
        public string addr;
        [JsonProperty("gameport")]
        public int gameport;
        [JsonProperty("steamid")]
        public string steamid;
        [JsonProperty("name")]
        public string name;
        [JsonProperty("appid")]
        public int appid;
        [JsonProperty("gamedir")]
        public string gamedir;
        [JsonProperty("version")]
        public string version;
        [JsonProperty("product")]
        public string product;
        [JsonProperty("region")]
        public int region;
        [JsonProperty("players")]
        public int players;
        [JsonProperty("max_players")]
        public int max_players;
        [JsonProperty("bots")]
        public int bots;
        [JsonProperty("map")]
        public string map;
        [JsonProperty("secure")]
        public bool secure;
        [JsonProperty("dedicated")]
        public bool dedicated;
        [JsonProperty("os")]
        public string os;
        [JsonProperty("gametype")]
        public string gametype;
    }

}

public class ServerData
{
    [JsonProperty("addr")]
    public string addr;
    [JsonProperty("gameport")]
    public int gameport;
    [JsonProperty("steamid")]
    public string steamid;
    [JsonProperty("name")]
    public string name;
    [JsonProperty("appid")]
    public int appid;
    [JsonProperty("gamedir")]
    public string gamedir;
    [JsonProperty("version")]
    public string version;
    [JsonProperty("product")]
    public string product;
    [JsonProperty("region")]
    public int region;
    [JsonProperty("players")]
    public int players;
    [JsonProperty("max_players")]
    public int max_players;
    [JsonProperty("bots")]
    public int bots;
    [JsonProperty("map")]
    public string map;
    [JsonProperty("secure")]
    public bool secure;
    [JsonProperty("dedicated")]
    public bool dedicated;
    [JsonProperty("os")]
    public string os;
    [JsonProperty("gametype")]
    public string gametype;
}


```

---

## OxidationTrollRocketMan

```csharp
using System;
using UnityEngine;

Oxide.Plugins
[Info("OxidationTrollRocketMan", "TopPlugin.ru", "1.0.2")]
[Description("Project Oxidation :: Troll's kit :: The rocketman")]
public class OxidationTrollRocketMan : RustPlugin
{
    private static readonly string UsePermission;
    private void Loaded();
    private void Unload();
    private object CanDismountEntity(BasePlayer Player, BaseMountable Entity);
    private void OnPlayerCorpseSpawned(BasePlayer Player, LootableCorpse Corpse);
    [ChatCommand("troll.rocketman")]
    private void OxidationTrollRocketManCommand(BasePlayer Player, string Command, string[] args);
    [ConsoleCommand("troll.rocketman")]
    private void OxidationTrollRocketManRcon(ConsoleSystem.Arg arg);
    private bool CanUseCommand(string SteamID);
    internal class OxidationRocketManAI : MonoBehaviour
    {
        protected BasePlayer Victim;
        internal void Awake();
        internal void OnEnable();
        internal void OnDisable();
        internal void Start();
        internal void FixedUpdate();
        internal void OnDestroy();
    }

}

internal class OxidationRocketManAI : MonoBehaviour
{
    protected BasePlayer Victim;
    internal void Awake();
    internal void OnEnable();
    internal void OnDisable();
    internal void Start();
    internal void FixedUpdate();
    internal void OnDestroy();
}


```

---

## Oysters

```csharp
using Newtonsoft.Json;
using Oxide.Core;
using System;
using System.Collections.Generic;
using System.Linq;
using UnityEngine;

Oxide.Plugins
[Info("Oysters", "OxideBro", "0.1.1")]
 class Oysters : RustPlugin
{
    private PluginConfig config;
    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    private void UpdateConfigValues();
    public class DefaultItems
    {
        [JsonProperty("Shortname предмета")]
        public string ShortName;
        [JsonProperty("Минимальное количество")]
        public int MinAmount;
        [JsonProperty("Максимальное количество")]
        public int MaxAmount;
        [JsonProperty("SkinID предмета")]
        public ulong SkinID;
        [JsonProperty("Имя предмета при создании (Оставте поле постым чтобы использовать стандартное название итема)")]
        public string Name;
        [JsonProperty("Это чертеж")]
        public bool IsBlueprnt;
    }

     class OysterSetting
    {
        [JsonProperty("SkinID устрицы")]
        public ulong SkinID;
        [JsonProperty("Имя предмета устрицы")]
        public string Name;
        [JsonProperty("Shortname предмета для устрицы")]
        public string ShortName;
        [JsonProperty("Выдаваемое количество")]
        public int Amount;
        [JsonProperty("Шанс что игроку даст данную открытую устрицу")]
        public int Change;
        [JsonProperty("Список предметов какие даёт при открытии")]
        public List<DefaultItems> Items;
    }

    protected override void SaveConfig();
     class PluginConfig
    {
        [JsonProperty("Configuration Version")]
        public VersionNumber PluginVersion;
        [JsonProperty("SkinID закрытой устрицы")]
        public ulong SkinID;
        [JsonProperty("Имя предмета закрытой устрицы ")]
        public string Name;
        [JsonProperty("Shortname предмета для закрытой устрицы")]
        public string ShortName;
        [JsonProperty("Выдаваемое количество")]
        public int Amount;
        [JsonProperty("Шанс что игроку даст данную закрытую устрицу")]
        public int Change;
        [JsonProperty("Засчитывать очки фермера друзьям")]
        public bool EnableFriendsFarm;
        [JsonProperty("Настройки открытых устриц")]
        public List<OysterSetting> OytersList;
        public static PluginConfig DefaultConfig();
    }

     void OnEntityBuilt(Planner plan, GameObject go);
     void SetPreferences(WildlifeTrap trap);
    public static Oysters ins;
     void Init();
     void OnServerSave();
     void OnServerInitialized();
     void Unload();
    private void OnNewSave();
     object OnItemAction(Item item, string action, BasePlayer player);
    private int? Find(int x);
    [ChatCommand("g")]
     void cmdGive(BasePlayer player);
     Dictionary<BasePlayer, bool> CreateLoot;
    [ChatCommand("oyloot")]
     void cmdOystersLoot(BasePlayer player, string command, string[] args);
     object OnHammerHit(BasePlayer player, HitInfo info);
     object OnItemSplit(Item item, int split_Amount);
     object CanCombineDroppedItem(DroppedItem drItem, DroppedItem anotherDrItem);
     object CanStackItem(Item item, Item anotherItem);
    private object OnItemRecycle(Item item, Recycler recycler);
    private static bool IsTextEqual(string var1, string var2);
    private static ulong GetUL(string userID);
    private class CustomTraps : BaseEntity
    {
        public WildlifeTrap trap;
        private void Awake();
        private void OnDestroy();
        public virtual void RandomTrap();
        private void UseBaitCalories(int numToUse);
        public void GiveItems(TrappableWildlife life);
        private void TryGetOyster();
        public void PlayerStoppedLooting(global::BasePlayer player);
    }

    private static bool MoveToContainer(Item itemBase, ItemContainer newcontainer, int iTargetPos, bool allowStack);
    private void GiveItem(BasePlayer player, Item item, BaseEntity.GiveItemReason reason);
    private bool GiveItem(PlayerInventory inv, Item item, ItemContainer container);
    private void GetIdealPickupContainer(PlayerInventory inv, Item item, ItemContainer container, int position);
    public class PlayerInfo
    {
        public int Opened;
        public int Caught;
        public int Write;
        public int Black;
    }

    [ChatCommand("oysters")]
     void cmdTopOysters2(BasePlayer player, string command, string[] args);
    public Dictionary<ulong, PlayerInfo> PlayersList;
     void LoadData();
     void SaveData();
}

public class DefaultItems
{
    [JsonProperty("Shortname предмета")]
    public string ShortName;
    [JsonProperty("Минимальное количество")]
    public int MinAmount;
    [JsonProperty("Максимальное количество")]
    public int MaxAmount;
    [JsonProperty("SkinID предмета")]
    public ulong SkinID;
    [JsonProperty("Имя предмета при создании (Оставте поле постым чтобы использовать стандартное название итема)")]
    public string Name;
    [JsonProperty("Это чертеж")]
    public bool IsBlueprnt;
}

 class OysterSetting
{
    [JsonProperty("SkinID устрицы")]
    public ulong SkinID;
    [JsonProperty("Имя предмета устрицы")]
    public string Name;
    [JsonProperty("Shortname предмета для устрицы")]
    public string ShortName;
    [JsonProperty("Выдаваемое количество")]
    public int Amount;
    [JsonProperty("Шанс что игроку даст данную открытую устрицу")]
    public int Change;
    [JsonProperty("Список предметов какие даёт при открытии")]
    public List<DefaultItems> Items;
}

 class PluginConfig
{
    [JsonProperty("Configuration Version")]
    public VersionNumber PluginVersion;
    [JsonProperty("SkinID закрытой устрицы")]
    public ulong SkinID;
    [JsonProperty("Имя предмета закрытой устрицы ")]
    public string Name;
    [JsonProperty("Shortname предмета для закрытой устрицы")]
    public string ShortName;
    [JsonProperty("Выдаваемое количество")]
    public int Amount;
    [JsonProperty("Шанс что игроку даст данную закрытую устрицу")]
    public int Change;
    [JsonProperty("Засчитывать очки фермера друзьям")]
    public bool EnableFriendsFarm;
    [JsonProperty("Настройки открытых устриц")]
    public List<OysterSetting> OytersList;
    public static PluginConfig DefaultConfig();
}

private class CustomTraps : BaseEntity
{
    public WildlifeTrap trap;
    private void Awake();
    private void OnDestroy();
    public virtual void RandomTrap();
    private void UseBaitCalories(int numToUse);
    public void GiveItems(TrappableWildlife life);
    private void TryGetOyster();
    public void PlayerStoppedLooting(global::BasePlayer player);
}

public class PlayerInfo
{
    public int Opened;
    public int Caught;
    public int Write;
    public int Black;
}


```

---

## PaiNAFK

```csharp
using System.Collections.Generic;
using Oxide.Core;

Oxide.Plugins
[Info("PaiN AFK", "PaiN", "0.1", ResourceId = 0)]
 class PaiNAFK : RustPlugin
{
     class Data
    {
        public List<ulong> AfkPlayers;
    }

     Data data;
     void LoadMessages();
     void Loaded();
     void Unloaded();
    [ChatCommand("afk")]
     void cmdAfk(BasePlayer player, string cmd, string[] args);
     void OnPlayerSleepEnded(BasePlayer player);
     void OnServerSave();
     bool IsAfk(ulong userid);
    private void StartAFK(BasePlayer player, bool enable);
     void SaveData();
     void ReadData();
     string GetLang(string msg, string userID);
}

 class Data
{
    public List<ulong> AfkPlayers;
}


```

---

## PanelV2

```csharp
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Plugins;
using System.Collections.Generic;
using System.Globalization;
using System.Linq;
using Random = UnityEngine.Random;
using System;
using UnityEngine;

Oxide.Plugins
[Info("PanelV2", "fermenspwnz", "2.1.6")]
[Description("Красивая панель с отображением онлайна, времени, вертолета, аирдропа, челнока, танка и корабля.")]
 class PanelV2 : RustPlugin
{
    static int downloaded;
     Plugin ImageLibrary { get; set; }
    public string GetImage(string shortname, ulong skin);
    public bool AddImage(string url, string shortname, ulong skin);
     void gettimage(string url, string name);
     List<ulong> _players;
    static Dictionary<string, string> _imagelist;
    static List<string> _messages;
    private PluginConfig config;
    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    protected override void SaveConfig();
    private class PluginConfig
    {
        [JsonProperty("Накрутка онлайна")]
        public int onliner;
        [JsonProperty("Информационная строка (если пусто, то выключена)")]
        public List<string> messages;
        [JsonProperty("Информационная строка | Частота обновлений в секундах")]
        public float infotimer;
        [JsonProperty("Сообщение | Онлайн")]
        public string message;
        [JsonProperty("Онлайн - размер текста")]
        public string onlinesize;
        [JsonProperty("Время - размер текста")]
        public string timersize;
        [JsonProperty("Картинки")]
        public Dictionary<string, string> imagelist;
        public static PluginConfig DefaultConfig();
    }

     string[] panels;
     void DestroyUI(Network.Connection player);
     void Unload();
     void OnPlayerDisconnected(BasePlayer player);
    static string GUIjsontimer;
    static string GUIjsononline;
    private void Init();
     void OnServerInitialized();
    private void OnPlayerConnected(BasePlayer player);
     int helicopters;
     int planes;
     int ships;
     int tanks;
     int chinooks;
     void OnEntityKill(BaseNetworkable Entity);
    private void OnEntitySpawned(BaseNetworkable Entity);
    [ConsoleCommand("gategui")]
     void gategui(ConsoleSystem.Arg arg);
     void CloseGUI(Network.Connection connect);
     Dictionary<ulong, DateTime> cooldown;
    const string GUIjsonmessage;
    const string GUIjsonfon;
    static string GUIjsondisable;
    static string GUIjsononplane;
    static string GUIjsononship;
    static string GUIjsonontank;
    static string GUIjsononheli;
    static string GUIjsononchinook;
    private void FarmGUI(TypeGui funct, List<Network.Connection> sendto);
}

private class PluginConfig
{
    [JsonProperty("Накрутка онлайна")]
    public int onliner;
    [JsonProperty("Информационная строка (если пусто, то выключена)")]
    public List<string> messages;
    [JsonProperty("Информационная строка | Частота обновлений в секундах")]
    public float infotimer;
    [JsonProperty("Сообщение | Онлайн")]
    public string message;
    [JsonProperty("Онлайн - размер текста")]
    public string onlinesize;
    [JsonProperty("Время - размер текста")]
    public string timersize;
    [JsonProperty("Картинки")]
    public Dictionary<string, string> imagelist;
    public static PluginConfig DefaultConfig();
}


```

---

## PathFinding

```csharp
using System;
using System.Collections.Generic;
using System.Reflection;
using Oxide.Core.Plugins;
using UnityEngine;
using static UnityEngine.Vector3;

Oxide.Plugins
[Info("PathFinding", "Reneb / Nogrod", "1.1.1")]
public class PathFinding : RustPlugin
{
    private static readonly Vector3 Up;
    public sealed class PathFinder
    {
        private static readonly sbyte[,] Direction;
        public static readonly Vector3 EyesPosition;
        public static readonly uint Size;
        public static PriorityQueue OpenList;
        public static PathFindNode[,] Grid;
        static PathFinder();
        public List<Vector3> FindPath(Vector3 sourcePos, Vector3 targetPos);
        private static void Clear();
        private static List<Vector3> RetracePath(PathFindNode startNode, PathFindNode endNode);
        private static int GetDistance(PathFindNode nodeA, PathFindNode nodeB);
        private static PathFindNode FindPathNodeOrCreate(int x, int z, float y);
    }

    public sealed class PriorityQueue
    {
        private readonly PathFindNode[] nodes;
        private int numNodes;
        public PriorityQueue(int maxNodes);
        public int Count { get; set; }
        public int MaxSize { get; set; }
        public void Clear();
        public bool Contains(PathFindNode node);
        public void Update(PathFindNode node);
        public void Enqueue(PathFindNode node);
        private void Swap(PathFindNode node1, PathFindNode node2);
        private void SortUp(PathFindNode node);
        private void SortDown(PathFindNode node);
        public PathFindNode Dequeue();
        public void Remove(PathFindNode node);
        private static int CompareTo(PathFindNode node, PathFindNode other);
    }

    public sealed class PathFindNode
    {
        public readonly int X;
        public readonly int Z;
        public int QueueIndex;
        public float H;
        public float G;
        public float F;
        public PathFindNode Parent;
        public Vector3 Position;
        public bool Walkable;
        public PathFindNode(Vector3 position);
        public override int GetHashCode();
    }

    public static bool FindRawGroundPosition(Vector3 sourcePos, Vector3 groundPos);
    private class PathFollower : MonoBehaviour
    {
        private readonly FieldInfo viewangles;
        public List<Vector3> Paths;
        public float secondsTaken;
        public float secondsToTake;
        public float waypointDone;
        public float speed;
        public Vector3 StartPos;
        public Vector3 EndPos;
        public Vector3 nextPos;
        public BaseEntity entity;
        public BasePlayer player;
        private void Awake();
        private void Move();
        private void Execute_Move();
        private void FindNextWaypoint();
        public void SetMovementPoint(Vector3 endpos, float s);
        private void SetViewAngle(BasePlayer player, Quaternion ViewAngles);
        private void FixedUpdate();
    }

    public static Vector3 jumpPosition;
    public static int groundLayer;
    public static int blockLayer;
    private static int MaxDepth;
    private readonly FieldInfo serverinput;
    protected override void LoadDefaultConfig();
    private void CheckCfg(string Key, T var);
    private void Init();
    private void OnServerInitialized();
    private void Unload();
    private bool FindAndFollowPath(BaseEntity entity, Vector3 sourcePosition, Vector3 targetPosition);
    private void FollowPath(BaseEntity entity, List<Vector3> pathpoints);
    [HookMethod("FindBestPath")]
    public List<Vector3> FindBestPath(Vector3 sourcePosition, Vector3 targetPosition);
    public List<Vector3> Go(Vector3 source, Vector3 target);
    private List<Vector3> FindPath(Vector3 sourcePosition, Vector3 targetPosition);
    private List<Vector3> FindLinePath(Vector3 sourcePosition, Vector3 targetPosition);
    private void ResetPathFollowers();
    [ChatCommand("path")]
    private void cmdChatPath(BasePlayer player, string command, string[] args);
    private bool TryGetPlayerView(BasePlayer player, Quaternion viewAngle);
    private bool TryGetClosestRayPoint(Vector3 sourcePos, Quaternion sourceDir, object closestEnt, Vector3 closestHitpoint);
}

public sealed class PathFinder
{
    private static readonly sbyte[,] Direction;
    public static readonly Vector3 EyesPosition;
    public static readonly uint Size;
    public static PriorityQueue OpenList;
    public static PathFindNode[,] Grid;
    static PathFinder();
    public List<Vector3> FindPath(Vector3 sourcePos, Vector3 targetPos);
    private static void Clear();
    private static List<Vector3> RetracePath(PathFindNode startNode, PathFindNode endNode);
    private static int GetDistance(PathFindNode nodeA, PathFindNode nodeB);
    private static PathFindNode FindPathNodeOrCreate(int x, int z, float y);
}

public sealed class PriorityQueue
{
    private readonly PathFindNode[] nodes;
    private int numNodes;
    public PriorityQueue(int maxNodes);
    public int Count { get; set; }
    public int MaxSize { get; set; }
    public void Clear();
    public bool Contains(PathFindNode node);
    public void Update(PathFindNode node);
    public void Enqueue(PathFindNode node);
    private void Swap(PathFindNode node1, PathFindNode node2);
    private void SortUp(PathFindNode node);
    private void SortDown(PathFindNode node);
    public PathFindNode Dequeue();
    public void Remove(PathFindNode node);
    private static int CompareTo(PathFindNode node, PathFindNode other);
}

public sealed class PathFindNode
{
    public readonly int X;
    public readonly int Z;
    public int QueueIndex;
    public float H;
    public float G;
    public float F;
    public PathFindNode Parent;
    public Vector3 Position;
    public bool Walkable;
    public PathFindNode(Vector3 position);
    public override int GetHashCode();
}

private class PathFollower : MonoBehaviour
{
    private readonly FieldInfo viewangles;
    public List<Vector3> Paths;
    public float secondsTaken;
    public float secondsToTake;
    public float waypointDone;
    public float speed;
    public Vector3 StartPos;
    public Vector3 EndPos;
    public Vector3 nextPos;
    public BaseEntity entity;
    public BasePlayer player;
    private void Awake();
    private void Move();
    private void Execute_Move();
    private void FindNextWaypoint();
    public void SetMovementPoint(Vector3 endpos, float s);
    private void SetViewAngle(BasePlayer player, Quaternion ViewAngles);
    private void FixedUpdate();
}


```

---

## Payback

```csharp
using Oxide.Core;
using System.Collections.Generic;
using UnityEngine;
using System.Collections;
using Oxide.Core.Configuration;
using Oxide.Game.Rust.Cui;
using System;
using System.Linq;
using Oxide.Core.Plugins;
using Newtonsoft.Json;
using System.Text.RegularExpressions;
using Oxide.Core.Libraries.Covalence;
using Rust;
using Rust.Ai;
using Network;

Oxide.Plugins
[Info("Payback", "Sempai#3239", "1.9.7")]
[Description("Специальные команды администратора для борьбы с читерами")]
 class Payback : RustPlugin
{
     Dictionary<Card, string> descriptions;
     Dictionary<string, Card> cardAliases;
    public void GiveCard(ulong userID, Card card, string[] args, BasePlayer admin);
     bool silentPacifism;
     void DoShark(BasePlayer player, string[] args, BasePlayer admin);
     IEnumerator SharkCo2(BasePlayer player, string[] args, BasePlayer admin);
     string sfx_bloodHit;
     string sfx_watersplash;
     IEnumerator MoveSharkCo(SimpleShark shark, BasePlayer player);
     void DoEmote(BasePlayer player, string[] args, BasePlayer admin);
     bool? CanUseGesture(BasePlayer player, GestureConfig gesture);
     void ResolveConflictingCommands(BasePlayer player, BasePlayer admin);
     Dictionary<ulong, BaseEntity> coilMap;
     void DoShocker(BasePlayer player, string[] args, BasePlayer admin);
     void DoBagSearch(ulong userID, string[] args, BasePlayer admin);
     IEnumerator BagSearchCo(ulong userID, bool logToDiscord, BasePlayer admin);
     bool flag_kill_no_loot;
     void GiveAdminHammer(BasePlayer admin);
     object OnStructureRepair(BaseCombatEntity entity, BasePlayer player);
     IEnumerator DeleteByCo(ulong steamid, Vector3 position, BasePlayer admin);
     HashSet<ulong> currentlyScreamingPlayers;
    public const string sound_scream;
    public const string effect_onfire;
     void GivePlayerFlamethrower(BasePlayer player);
    public void Line(BasePlayer player, Vector3 from, Vector3 to, Color color, float duration);
     void AdminSpawnChickens(BasePlayer player, BasePlayer target, string[] args);
     HashSet<BaseCombatEntity> chickens;
     void SpawnChickens(BasePlayer player, Vector3 spawnposition, string[] args);
     IEnumerator AnimalAttackCo(BasePlayer player, Vector3 spawnposition, string[] args);
     void OnPlayerRespawned(BasePlayer player);
     void OnEntityKill(BaseNetworkable entity, HitInfo info);
     HashSet<BaseNetworkable> entitiesWatchingForKilledMounts;
     void RocketManTarget(BasePlayer player);
     IEnumerator AccelerateRocketOverTime(ServerProjectile projectile);
    public const string invisibleChairPrefab;
     HashSet<BaseMountable> chairsPreventingDismount;
     BaseEntity InvisibleSit(BasePlayer targetPlayer);
    public List<string> sounds_kill_quad;
     void DoPinyataEffect(BasePlayer player);
     void DoDana(BasePlayer player, BasePlayer admin);
     void GiveItemOrDrop(BasePlayer player, Item item, bool stack);
     HashSet<ulong> thirstyPlayers;
     Dictionary<ulong, BasePlayer> basePlayerMap;
     Coroutine thirstyCoroutine;
     void DoThirsty(BasePlayer player);
     IEnumerator DoThirstyCo();
     void DoHigherGround(BasePlayer player);
     void DoCamomoCommand(BasePlayer player);
     void DoNakedCommand(BasePlayer targetPlayer);
     IEnumerator NakedOverTime(BasePlayer targetPlayer);
     string chairPrefab;
     Dictionary<ulong, BaseEntity> sitChairMap;
     void DoSitCommand(BasePlayer targetPlayer, BasePlayer adminPlayer);
     IEnumerator SitCo(BasePlayer player);
     object CanDismountEntity(BasePlayer player, BaseMountable entity);
     string guid_BSOD;
     string url_bsod;
     void DoBSOD(BasePlayer player, bool playPublic);
     string sound_bsod;
     string sound_fall;
     IEnumerator PlayBSODSounds(BasePlayer player, bool playPublic);
    [ConsoleCommand("uipaybackcommand")]
     void CommandUICommand(ConsoleSystem.Arg arg);
    [ChatCommand("setdroppercent")]
     void Command_SetDropPercent(BasePlayer player, string cmd, string[] args);
    [ConsoleCommand("setdroppercent")]
     void Console_CommandSetDropPercent(ConsoleSystem.Arg arg);
     void SetDropPercent(BasePlayer player, string[] args);
     Dictionary<ulong, float> playerMessageTimestamps;
     void SendPlayerLimitedMessage(ulong userID, string message, float rate);
     void AdminCommandToggleCard(BasePlayer admin, Card card, string[] args);
     void AdminToggleCard(BasePlayer admin, BasePlayer targetPlayer, Card card, string[] args);
    [ConsoleCommand("payback")]
     void Console_Payback(ConsoleSystem.Arg arg);
    [ChatCommand("payback")]
     void ChatCommandPayback(BasePlayer player, string cmd, string[] args);
     void CommandPayback(BasePlayer player, string cmd, string[] args);
     void DoPaybackPrintout(BasePlayer player, string[] args);
     Dictionary<ulong, HashSet<Card>> cardMap;
    public bool HasAnyCard(ulong userID);
    public bool HasCard(ulong userID, Card card);
    public void TakeCard(BasePlayer player, Card card, string[] args, BasePlayer admin);
    public void TakeCard(ulong userID, Card card, string[] args, BasePlayer admin);
     HashSet<ulong> recentPlayerVoices;
     Dictionary<ulong, float> recentPlayerVoiceTimestamps;
     Dictionary<ulong, HashSet<ulong>> listenedPlayersMap;
     Timer listenTimer;
     bool isListening;
    [ConsoleCommand("listen")]
     void ConsoleCommandListenNext(ConsoleSystem.Arg arg);
    [ChatCommand("listen")]
     void CommandListenNext(BasePlayer player);
    private object OnPlayerViolation(BasePlayer player, AntiHackType type);
     object OnPlayerDeath(BasePlayer player, HitInfo hitinfo);
     float Random();
     object OnPlayerVoice(BasePlayer player, Byte[] data);
     object OnHealingItemUse(MedicalTool tool, BasePlayer player);
     object OnPlayerHealthChange(BasePlayer player, float oldValue, float newValue);
    [ChatCommand("TestBanned")]
     void CommandTestBanned(BasePlayer player, string cmd, string[] args);
     void OnPlayerDisconnected(BasePlayer player, string reason);
     void OnPlayerBanned(Network.Connection connection, string reason);
     void OnPlayerBanned(string name, ulong id, string address, string reason);
     void OnPlayerKicked(BasePlayer player, string reason);
     List<string> banReasons;
     void CheckPublisherBan(string name, ulong id, string address, string reason);
     string TryGetDisplayName(ulong userID);
    [ConsoleCommand("bancheckexception")]
     void CommandBanCheckException(ConsoleSystem.Arg arg);
     bool test_connect;
    [ChatCommand("testconnect")]
     void CommandTestConnect(BasePlayer player);
     void OnPlayerConnected(BasePlayer player);
     void SendToDiscordWebhook(Dictionary<string, string> messageData, string title);
    private void SendWebhook(string WebhookUrl, string title, object fields);
    private class SendEmbedMessage
    {
        public SendEmbedMessage(int EmbedColour, string discordMessage, object _fields);
        [JsonProperty("embeds")]
        public object Embeds { get; set; }
        public string ToJson();
    }

    private void OnEntityTakeDamage(BaseEntity entity, HitInfo hitinfo);
     void DoScreaming(BasePlayer player, BasePlayer attacker, bool fire, bool screamSourceIsTarget);
     HashSet<BaseEntity> protectedStashes;
     void OnStashExposed(StashContainer stash, BasePlayer player);
    public BasePlayer GetPlayerWithName(string displayName);
     BaseEntity RaycastFirstEntity(Ray ray, float distance);
     void Initialize();
     void GenericChatCommand(BasePlayer player, string cmd, string[] args);
     void GenericConsoleCommand(ConsoleSystem.Arg arg);
     void OnServerInitialized(bool serverIsNOTinitialized);
    private static List<string> _viewInventoryHooks;
     void ViewTargetPlayerInventory(BasePlayer target, BasePlayer admin);
    private void ViewInvCmd(IPlayer iplayer, string command, string[] args);
    private List<LootableCorpse> _viewingcorpse;
    private void ViewInventory(BasePlayer player, BasePlayer targetplayer);
     LootableCorpse GetLootableCorpse(string title);
    private void StartLooting(BasePlayer player, BasePlayer targetplayer, LootableCorpse corpse);
    private void StartLootingContainer(BasePlayer player, ItemContainer container, LootableCorpse corpse);
    private void OnLootEntityEnd(BasePlayer player, LootableCorpse corpse);
     void OnEntityDeath(LootableCorpse corpse, HitInfo info);
    private IPlayer FindPlayer(string nameOrId);
    private bool HasPerm(string id, string perm);
    private string GetLang(string langKey, string playerId, object[] args);
    private void ChatMessage(IPlayer player, string langKey, object[] args);
    private void UnSubscribeFromHooks();
    private void SubscribeToHooks();
     string filename_data { get; set; }
     DynamicConfigFile file_payback_data;
    public PaybackData paybackData;
    public class PaybackData
    {
        public float percent_butterfingers_dropchance;
        public HashSet<ulong> bancheck_exceptions;
    }

     void Unload();
    private void SaveData();
    private void LoadData();
     void ReadDataIntoDynamicConfigFiles();
     void LoadFromDynamicConfigFiles();
    public const string permission_admin;
    public bool IsAdmin(BasePlayer player);
     void SetDespawnDuration(DroppedItem dropped, float seconds);
     void DestroyGroundCheck(BaseEntity entity);
    [ChatCommand("sound")]
     void SoundCommand(BasePlayer player, string cmd, string[] args);
     void PrintToPlayer(BasePlayer player, string text);
    public HashSet<ulong> GetPlayerTeam(ulong userID);
    public void PlaySound(List<string> effects, BasePlayer player, Vector3 worldPosition, bool playlocal);
    public void PlaySound(List<string> effects, BasePlayer player, bool playlocal);
    public void PlaySound(string effect, ListHashSet<BasePlayer> players, bool playlocal);
     bool test;
    public void PlaySound(string effect, BasePlayer player, bool playlocal, Vector3 posLocal);
    public void PlayGesture(BasePlayer target, string gestureName, bool canCancel);
    public class Worker : MonoBehaviour
    {
        public static Worker GetSingleton();
        static Worker _singleton;
        public static Coroutine StaticStartCoroutine(IEnumerator c);
    }

    private void Init();
    private PluginConfig config;
    protected override void LoadConfig();
    protected override void SaveConfig();
    protected override void LoadDefaultConfig();
    private class PluginConfig
    {
        [JsonProperty("Check temporary game bans and notify via Discord Webhook")]
        public bool enabled_nexus_gamebancheck;
        [JsonProperty("Only report temp bans younger than days")]
        public int nexus_ban_days;
        [JsonProperty("Notify Game Ban + Team")]
        public bool notify_game_ban;
        [JsonProperty("Only notify ban if has team")]
        public bool notify_only_if_has_team;
        [JsonProperty("Include bm links in ban notification")]
        public bool notify_ban_include_bm;
        [JsonProperty("These discord webhooks will get notified")]
        public List<string> webhooks;
        [JsonProperty("notify player being attacked by cheater")]
        public bool notifyCheaterAttacking;
    }

    public class UI2
    {
        public static Vector4 vectorFullscreen;
        public static string ColorText(string input, string color);
        public static void ClearUI(BasePlayer player);
        public static Dictionary<ulong, HashSet<string>> dirtyMap;
        public static HashSet<string> GetDirtyBitsForPlayer(BasePlayer player);
        public class Layout
        {
            public Vector2 startPosition;
            public Vector4 cellBounds;
            public Vector2 padding;
            public Vector4 cursor;
            public int maxRows;
            public int row;
            public int col;
            public void Init(Vector2 _startPosition, Vector4 _cellBounds, int _maxRows, Vector2 _padding);
            public void NextCell(System.Action<Vector4, int, int> populateAction);
            public void Reset();
        }

        public static string ColorToHex(Color color);
        public static string HexToRGBAString(string hex);
        public static Vector4 GetOffsetVector4(Vector2 offset);
        public static Vector4 GetOffsetVector4(float x, float y);
        public static Vector4 SubtractPadding(Vector4 input, float padding);
        public static float GetSquareFromWidth(float width, float aspect);
        public static float GetSquareFromHeight(float height, float aspect);
        public static Vector4 MakeSquareFromWidth(Vector4 bounds, float aspect);
        public static Vector4 MakeSquareFromHeight(Vector4 bounds, float aspect);
        public static Vector4 MakeRectFromWidth(Vector4 bounds, float ratio, float aspect);
        public static Vector4 MakeRectFromHeight(Vector4 bounds, float ratio, float aspect);
        public static HashSet<string> guids;
        public static string GetMinUI(Vector4 panelPosition);
        public static string GetMaxUI(Vector4 panelPosition);
        public static string GetColorString(Vector4 color);
        public static CuiElement CreateInputField(CuiElementContainer container, string parent, string panelName, string message, int textSize, string color, Vector4 bounds, string command);
        public static void CreateOutlineLabel(CuiElementContainer container, string parent, string panelName, string message, string color, int size, Vector4 bounds, TextAnchor textAlignment, float fadeOut, float fadeIn, string outlineColor, string outlineDistance);
        public static void CreateLabel(CuiElementContainer container, string parent, string panelName, string message, string color, int size, string aMin, string aMax, TextAnchor textAlignment, float fadeIn, float fadeOut);
        public static CuiButton CreateButton(CuiElementContainer container, string parent, string panelName, string color, string text, int size, Vector4 bounds, string command, TextAnchor align, string textColor);
        public static CuiPanel CreatePanel(CuiElementContainer container, string parent, string panelName, string color, Vector4 bounds, string imageUrl, bool cursor, float fadeOut, float fadeIn, bool png, bool blur, bool outline);
    }

}

private class SendEmbedMessage
{
    public SendEmbedMessage(int EmbedColour, string discordMessage, object _fields);
    [JsonProperty("embeds")]
    public object Embeds { get; set; }
    public string ToJson();
}

public class PaybackData
{
    public float percent_butterfingers_dropchance;
    public HashSet<ulong> bancheck_exceptions;
}

public class Worker : MonoBehaviour
{
    public static Worker GetSingleton();
    static Worker _singleton;
    public static Coroutine StaticStartCoroutine(IEnumerator c);
}

private class PluginConfig
{
    [JsonProperty("Check temporary game bans and notify via Discord Webhook")]
    public bool enabled_nexus_gamebancheck;
    [JsonProperty("Only report temp bans younger than days")]
    public int nexus_ban_days;
    [JsonProperty("Notify Game Ban + Team")]
    public bool notify_game_ban;
    [JsonProperty("Only notify ban if has team")]
    public bool notify_only_if_has_team;
    [JsonProperty("Include bm links in ban notification")]
    public bool notify_ban_include_bm;
    [JsonProperty("These discord webhooks will get notified")]
    public List<string> webhooks;
    [JsonProperty("notify player being attacked by cheater")]
    public bool notifyCheaterAttacking;
}

public class UI2
{
    public static Vector4 vectorFullscreen;
    public static string ColorText(string input, string color);
    public static void ClearUI(BasePlayer player);
    public static Dictionary<ulong, HashSet<string>> dirtyMap;
    public static HashSet<string> GetDirtyBitsForPlayer(BasePlayer player);
    public class Layout
    {
        public Vector2 startPosition;
        public Vector4 cellBounds;
        public Vector2 padding;
        public Vector4 cursor;
        public int maxRows;
        public int row;
        public int col;
        public void Init(Vector2 _startPosition, Vector4 _cellBounds, int _maxRows, Vector2 _padding);
        public void NextCell(System.Action<Vector4, int, int> populateAction);
        public void Reset();
    }

    public static string ColorToHex(Color color);
    public static string HexToRGBAString(string hex);
    public static Vector4 GetOffsetVector4(Vector2 offset);
    public static Vector4 GetOffsetVector4(float x, float y);
    public static Vector4 SubtractPadding(Vector4 input, float padding);
    public static float GetSquareFromWidth(float width, float aspect);
    public static float GetSquareFromHeight(float height, float aspect);
    public static Vector4 MakeSquareFromWidth(Vector4 bounds, float aspect);
    public static Vector4 MakeSquareFromHeight(Vector4 bounds, float aspect);
    public static Vector4 MakeRectFromWidth(Vector4 bounds, float ratio, float aspect);
    public static Vector4 MakeRectFromHeight(Vector4 bounds, float ratio, float aspect);
    public static HashSet<string> guids;
    public static string GetMinUI(Vector4 panelPosition);
    public static string GetMaxUI(Vector4 panelPosition);
    public static string GetColorString(Vector4 color);
    public static CuiElement CreateInputField(CuiElementContainer container, string parent, string panelName, string message, int textSize, string color, Vector4 bounds, string command);
    public static void CreateOutlineLabel(CuiElementContainer container, string parent, string panelName, string message, string color, int size, Vector4 bounds, TextAnchor textAlignment, float fadeOut, float fadeIn, string outlineColor, string outlineDistance);
    public static void CreateLabel(CuiElementContainer container, string parent, string panelName, string message, string color, int size, string aMin, string aMax, TextAnchor textAlignment, float fadeIn, float fadeOut);
    public static CuiButton CreateButton(CuiElementContainer container, string parent, string panelName, string color, string text, int size, Vector4 bounds, string command, TextAnchor align, string textColor);
    public static CuiPanel CreatePanel(CuiElementContainer container, string parent, string panelName, string color, Vector4 bounds, string imageUrl, bool cursor, float fadeOut, float fadeIn, bool png, bool blur, bool outline);
}

public class Layout
{
    public Vector2 startPosition;
    public Vector4 cellBounds;
    public Vector2 padding;
    public Vector4 cursor;
    public int maxRows;
    public int row;
    public int col;
    public void Init(Vector2 _startPosition, Vector4 _cellBounds, int _maxRows, Vector2 _padding);
    public void NextCell(System.Action<Vector4, int, int> populateAction);
    public void Reset();
}


```

---

