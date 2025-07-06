# uMod Plugins Dataset - Code Abstractions (Continued)

Chunk 4 - Generated: 2025-07-06 20:39:06

## IQChat-2.65.46

```csharp
using System;
using System.Collections;
using System.Collections.Generic;
using System.IO;
using System.Linq;
using System.Text;
using System.Text.RegularExpressions;
using CompanionServer;
using ConVar;
using Facepunch;
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;
using Oxide.Core;
using Oxide.Core.Libraries;
using Oxide.Core.Libraries.Covalence;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using UnityEngine;
using UnityEngine.Networking;
using Object = System.Object;
using Pool = Facepunch.Pool;

Oxide.Plugins
[Info("IQChat", "Mercury", "2.65.46")]
[Description("The most pleasant chat for your server from the IQ system")]
 class IQChat : RustPlugin
{
    [PluginReference]
     Plugin ImageLibrary;
     Plugin IQFakeActive;
     Plugin IQRankSystem;
     Plugin XLevels;
     Plugin Clans;
     Plugin XPrison;
     Plugin TranslationAPI;
     Plugin RustApp;
     Plugin SkillTree;
     Plugin PlayerRanks;
    private String GetClanTag(BasePlayer.EncryptedValue<UInt64> playerID);
    private String GetClanTag(UInt64 playerID);
    private String PlayerRanks_GetRanks(BasePlayer player);
    private String GetPrestigeLevel(UInt64 player);
    private String SkillTree_GetLevel(BasePlayer player);
    private String SkillTree_GetXP(BasePlayer player);
    private String[] GetInfoSkillTree(BasePlayer player);
    private String XLevel_GetLevel(BasePlayer player);
    private String XLevel_GetPrefix(BasePlayer player);
    public class FakePlayer
    {
        [JsonProperty("userId")]
        public String userId;
        [JsonProperty("displayName")]
        public String displayName;
        public Boolean isMuted;
    }

    public Boolean IsReadyIQFakeActive();
    private List<FakePlayer> GetCombinedPlayerList();
    private Boolean IsFakeUser(String idOrName);
    private Boolean SetMuteFakeUser(String idOrName, Boolean isMuted);
    private String GetFakeName(String idOrName);
     String IQRankGetRank(BasePlayer.EncryptedValue<UInt64> userID);
     String IQRankGetRank(ulong userID);
     String IQRankGetTimeGame(BasePlayer.EncryptedValue<UInt64> userID);
     String IQRankGetTimeGame(ulong userID);
     List<String> IQRankListKey(BasePlayer.EncryptedValue<UInt64> userID);
     List<String> IQRankListKey(ulong userID);
     String IQRankGetNameRankKey(string Key);
     void IQRankSetRank(BasePlayer.EncryptedValue<UInt64> userID, string RankKey);
     void IQRankSetRank(ulong userID, string RankKey);
    private String XPrison_GetPrefix(BasePlayer player);
    private const Boolean LanguageEn;
    private static IQChat _;
    private static ImageUI _imageUI;
    public class TranslationState
    {
        public Boolean IsProcessed { get; set; }
        public String Translation { get; set; }
        public String DoTranslation { get; set; }
    }

    private Dictionary<String, TranslationState> saveTranslate;
    static Double CurrentTime { get; set; }
    public Dictionary<BasePlayer, BasePlayer> PMHistory;
    public Dictionary<BasePlayer, List<String>> LastMessagesChat;
    private const String PermissionUseCmdCnick;
    private const String PermissionUseCmdMsg;
    private const String PermissionTranslationIgnore;
    private const String PermissionHideMuteName;
    private const String PermissionHideOnline;
    private const String PermissionMute;
    private const String PermissionAlert;
    private const String PermissionRename;
    private const String PermissionAntiSpam;
    private const String PermissionHideConnection;
    private const String PermissionHideDisconnection;
    private const String PermissionMutedAdmin;
     class Response
    {
        [JsonProperty("country")]
        public string Country { get; set; }
    }

    public static StringBuilder sb;
    public string GetLang(string LangKey, string userID, object[] args);
    private static Configuration config;
    private class Configuration
    {
        [JsonProperty(LanguageEn ? "Setting up player information" : "Настройка информации о игроке")]
        public ControllerConnection ControllerConnect;
        internal class ControllerConnection
        {
            [JsonProperty(LanguageEn ? "Function switches" : "Перключатели функций")]
            public Turned Turneds;
            [JsonProperty(LanguageEn ? "Setting Standard Values" : "Настройка стандартных значений")]
            public SetupDefault SetupDefaults;
            internal class SetupDefault
            {
                [JsonProperty(LanguageEn ? "This prefix will be set if the player entered the server for the first time or in case of expiration of the rights to the prefix that he had earlier" : "Данный префикс установится если игрок впервые зашел на сервер или в случае окончания прав на префикс, который у него стоял ранее")]
                public String PrefixDefault;
                [JsonProperty(LanguageEn ? "This nickname color will be set if the player entered the server for the first time or in case of expiration of the rights to the nickname color that he had earlier" : "Данный цвет ника установится если игрок впервые зашел на сервер или в случае окончания прав на цвет ника, который у него стоял ранее")]
                public String NickDefault;
                [JsonProperty(LanguageEn ? "This chat color will be set if the player entered the server for the first time or in case of expiration of the rights to the chat color that he had earlier" : "Данный цвет чата установится если игрок впервые зашел на сервер или в случае окончания прав на цвет чата, который у него стоял ранее")]
                public String MessageDefault;
            }

            internal class Turned
            {
                [JsonProperty(LanguageEn ? "Set automatically a prefix to a player when he got the rights to it" : "Устанавливать автоматически префикс игроку, когда он получил права на него")]
                public Boolean TurnAutoSetupPrefix;
                [JsonProperty(LanguageEn ? "Set automatically the color of the nickname to the player when he got the rights to it" : "Устанавливать автоматически цвет ника игроку, когда он получил права на него")]
                public Boolean TurnAutoSetupColorNick;
                [JsonProperty(LanguageEn ? "Set the chat color automatically to the player when he got the rights to it" : "Устанавливать автоматически цвет чата игроку, когда он получил права на него")]
                public Boolean TurnAutoSetupColorChat;
                [JsonProperty(LanguageEn ? "Automatically reset the prefix when the player's rights to it expire" : "Сбрасывать автоматически префикс при окончании прав на него у игрока")]
                public Boolean TurnAutoDropPrefix;
                [JsonProperty(LanguageEn ? "Automatically reset the color of the nickname when the player's rights to it expire" : "Сбрасывать автоматически цвет ника при окончании прав на него у игрока")]
                public Boolean TurnAutoDropColorNick;
                [JsonProperty(LanguageEn ? "Automatically reset the color of the chat when the rights to it from the player expire" : "Сбрасывать автоматически цвет чата при окончании прав на него у игрока")]
                public Boolean TurnAutoDropColorChat;
            }

        }

        [JsonProperty(LanguageEn ? "Setting options for the player" : "Настройка параметров для игрока")]
        public ControllerParameters ControllerParameter;
        internal class ControllerParameters
        {
            [JsonProperty(LanguageEn ? "Setting the display of options for player selection" : "Настройка отображения параметров для выбора игрока")]
            public VisualSettingParametres VisualParametres;
            [JsonProperty(LanguageEn ? "List and customization of colors for a nickname" : "Список и настройка цветов для ника")]
            public List<AdvancedFuncion> NickColorList;
            [JsonProperty(LanguageEn ? "List and customize colors for chat messages" : "Список и настройка цветов для сообщений в чате")]
            public List<AdvancedFuncion> MessageColorList;
            [JsonProperty(LanguageEn ? "List and configuration of prefixes in chat" : "Список и настройка префиксов в чате")]
            public PrefixSetting Prefixes;
            internal class PrefixSetting
            {
                [JsonProperty(LanguageEn ? "Enable support for multiple prefixes at once (true - multiple prefixes can be set/false - only 1 can be set to choose from)" : "Включить поддержку нескольких префиксов сразу (true - можно установить несколько префиксов/false - установить можно только 1 на выбор)")]
                public Boolean TurnMultiPrefixes;
                [JsonProperty(LanguageEn ? "The maximum number of prefixes that can be set at a time (This option only works if setting multiple prefixes is enabled)" : "Максимальное количество префиксов, которое можно установить за раз(Данный параметр работает только если включена установка нескольких префиксов)")]
                public Int32 MaximumMultiPrefixCount;
                [JsonProperty(LanguageEn ? "List of prefixes and their settings" : "Список префиксов и их настройка")]
                public List<AdvancedFuncion> Prefixes;
            }

            internal class AdvancedFuncion
            {
                [JsonProperty(LanguageEn ? "Permission" : "Права")]
                public String Permissions;
                [JsonProperty(LanguageEn ? "Argument" : "Значение")]
                public String Argument;
                [JsonProperty(LanguageEn ? "Block the player's ability to select this parameter in the plugin menu (true - yes/false - no)" : "Заблокировать возможность выбрать данный параметр игроком в меню плагина (true - да/false - нет)")]
                public Boolean IsBlockSelected;
            }

            internal class VisualSettingParametres
            {
                [JsonProperty(LanguageEn ? "Player prefix selection display type - (0 - dropdown list, 1 - slider (Please note that if you have multi-prefix enabled, the dropdown list will be set))" : "Тип отображения выбора префикса для игрока - (0 - выпадающий список, 1 - слайдер (Учтите, что если у вас включен мульти-префикс, будет установлен выпадающий список))")]
                public SelectedParametres PrefixType;
                [JsonProperty(LanguageEn ? "Display type of player's nickname color selection - (0 - drop-down list, 1 - slider)" : "Тип отображения выбора цвета ника для игрока - (0 - выпадающий список, 1 - слайдер)")]
                public SelectedParametres NickColorType;
                [JsonProperty(LanguageEn ? "Display type of message color choice for the player - (0 - drop-down list, 1 - slider)" : "Тип отображения выбора цвета сообщения для игрока - (0 - выпадающий список, 1 - слайдер)")]
                public SelectedParametres ChatColorType;
                [JsonProperty(LanguageEn ? "IQRankSystem : Player rank selection display type - (0 - drop-down list, 1 - slider)" : "IQRankSystem : Тип отображения выбора ранга для игрока - (0 - выпадающий список, 1 - слайдер)")]
                public SelectedParametres IQRankSystemType;
            }

        }

        [JsonProperty(LanguageEn ? "Plugin mute settings" : "Настройка мута в плагине")]
        public ControllerMute ControllerMutes;
        internal class ControllerMute
        {
            [JsonProperty(LanguageEn ? "Prohibit sending messages in /pm and /r if the player's chat is blocked" : "Запрещать отправлять сообщения в /pm, /r - если у игрока заблокирован чат")]
            public Boolean mutedPM;
            [JsonProperty(LanguageEn ? "Setting up automatic muting" : "Настройка автоматического мута")]
            public AutoMute AutoMuteSettings;
            internal class AutoMute
            {
                [JsonProperty(LanguageEn ? "Enable automatic muting for forbidden words (true - yes/false - no)" : "Включить автоматический мут по запрещенным словам(true - да/false - нет)")]
                public Boolean UseAutoMute;
                [JsonProperty(LanguageEn ? "Reason for automatic muting" : "Причина автоматического мута")]
                public Muted AutoMuted;
            }

            [JsonProperty(LanguageEn ? "Additional setting for logging about mutes in discord" : "Дополнительная настройка для логирования о мутах в дискорд")]
            public LoggedFuncion LoggedMute;
            internal class LoggedFuncion
            {
                [JsonProperty(LanguageEn ? "Support for logging the last N messages (Discord logging about mutes must be enabled)" : "Поддержка логирования последних N сообщений (Должно быть включено логирование в дискорд о мутах)")]
                public Boolean UseHistoryMessage;
                [JsonProperty(LanguageEn ? "How many latest player messages to send in logging" : "Сколько последних сообщений игрока отправлять в логировании")]
                public Int32 CountHistoryMessage;
            }

            [JsonProperty(LanguageEn ? "Reasons to block chat" : "Причины для блокировки чата")]
            public List<Muted> MuteChatReasons;
            [JsonProperty(LanguageEn ? "Reasons to block your voice" : "Причины для блокировки голоса")]
            public List<Muted> MuteVoiceReasons;
            internal class Muted
            {
                [JsonProperty(LanguageEn ? "Reason for blocking" : "Причина для блокировки")]
                public String Reason;
                [JsonProperty(LanguageEn ? "Block time (in seconds)" : "Время блокировки(в секундах)")]
                public Int32 SecondMute;
            }

        }

        [JsonProperty(LanguageEn ? "Configuring Message Processing" : "Настройка обработки сообщений")]
        public ControllerMessage ControllerMessages;
        internal class ControllerMessage
        {
            [JsonProperty(LanguageEn ? "Basic settings for chat messages from the plugin" : "Основная настройка сообщений в чат от плагина")]
            public GeneralSettings GeneralSetting;
            [JsonProperty(LanguageEn ? "Configuring functionality switching in chat" : "Настройка переключения функционала в чате")]
            public TurnedFuncional TurnedFunc;
            [JsonProperty(LanguageEn ? "Player message formatting settings" : "Настройка форматирования сообщений игроков")]
            public FormattingMessage Formatting;
            internal class GeneralSettings
            {
                [JsonProperty(LanguageEn ? "Notify the player in chat about receiving a prefix/nickname color/chat color (true - yes/false - no)" : "Уведомлять игрока в чате о получении префикса/цвета ника/цвета чата (true - да/false - нет)")]
                public Boolean alertArgumentsInfoSetup;
                [JsonProperty(LanguageEn ? "Notify the player in chat about the end of a prefix/nickname color/chat color (true - yes/false - no)" : "Уведомлять игрока в чате о окончании префикса/цвета ника/цвета чата (true - да/false - нет)")]
                public Boolean alertArgumentsInfoRemove;
                [JsonProperty(LanguageEn ? "Customizing the chat alert format" : "Настройка формата оповещения в чате")]
                public BroadcastSettings BroadcastFormat;
                [JsonProperty(LanguageEn ? "Setting the mention format in the chat, via @" : "Настройка формата упоминания в чате, через @")]
                public AlertSettings AlertFormat;
                [JsonProperty(LanguageEn ? "Additional setting" : "Дополнительная настройка")]
                public OtherSettings OtherSetting;
                internal class BroadcastSettings
                {
                    [JsonProperty(LanguageEn ? "The name of the notification in the chat" : "Наименование оповещения в чат")]
                    public String BroadcastTitle;
                    [JsonProperty(LanguageEn ? "Chat alert message color" : "Цвет сообщения оповещения в чат")]
                    public String BroadcastColor;
                    [JsonProperty(LanguageEn ? "Steam64ID for chat avatar" : "Steam64ID для аватарки в чате")]
                    public String Steam64IDAvatar;
                }

                internal class AlertSettings
                {
                    [JsonProperty(LanguageEn ? "The color of the player mention message in the chat" : "Цвет сообщения упоминания игрока в чате")]
                    public String AlertPlayerColor;
                    [JsonProperty(LanguageEn ? "Sound when receiving and sending a mention via @" : "Звук при при получении и отправки упоминания через @")]
                    public String SoundAlertPlayer;
                }

                internal class OtherSettings
                {
                    [JsonProperty(LanguageEn ? "Time after which the message will be deleted from the UI from the administrator" : "Время,через которое удалится сообщение с UI от администратора")]
                    public Int32 TimeDeleteAlertUI;
                    [JsonProperty(LanguageEn ? "The size of the message from the player in the chat" : "Размер сообщения от игрока в чате")]
                    public Int32 SizeMessage;
                    [JsonProperty(LanguageEn ? "Player nickname size in chat" : "Размер ника игрока в чате")]
                    public Int32 SizeNick;
                    [JsonProperty(LanguageEn ? "The size of the player's prefix in the chat (will be used if <size=N></size> is not set in the prefix itself)" : "Размер префикса игрока в чате (будет использовано, если в самом префиксе не установвлен <size=N></size>)")]
                    public Int32 SizePrefix;
                    [JsonProperty(LanguageEn ? "Nickname size according to privilege [permission] = size" : "Размер ника по привилегии [permission] = размер")]
                    public Dictionary<String, Int32> sizeNickPrivilages;
                    [JsonProperty(LanguageEn ? "Chat message size according to privilege [permission] = size" : "Размер сообщения в чате по привилегии [permission] = размер")]
                    public Dictionary<String, Int32> sizeMessagePrivilages;
                    public Int32 GetSizeNickOrMessage(BasePlayer player, Boolean nickOrMessage);
                }

            }

            internal class TurnedFuncional
            {
                [JsonProperty(LanguageEn ? "Configuring spam protection" : "Настройка защиты от спама")]
                public AntiSpam AntiSpamSetting;
                [JsonProperty(LanguageEn ? "Setting up a temporary chat block for newbies (who have just logged into the server)" : "Настройка временной блокировки чата новичкам (которые только зашли на сервер)")]
                public AntiNoob AntiNoobSetting;
                [JsonProperty(LanguageEn ? "Setting up private messages" : "Настройка личных сообщений")]
                public PM PMSetting;
                internal class AntiNoob
                {
                    [JsonProperty(LanguageEn ? "Newbie protection in PM/R" : "Защита от новичка в PM/R")]
                    public Settings AntiNoobPM;
                    [JsonProperty(LanguageEn ? "Newbie protection in global and team chat" : "Защита от новичка в глобальном и коммандном чате")]
                    public Settings AntiNoobChat;
                    internal class Settings
                    {
                        [JsonProperty(LanguageEn ? "Enable protection?" : "Включить защиту?")]
                        public Boolean AntiNoobActivate;
                        [JsonProperty(LanguageEn ? "Newbie Chat Lock Time" : "Время блокировки чата для новичка")]
                        public Int32 TimeBlocked;
                    }

                }

                internal class AntiSpam
                {
                    [JsonProperty(LanguageEn ? "Enable spam protection (Anti-spam)" : "Включить защиту от спама (Анти-спам)")]
                    public Boolean AntiSpamActivate;
                    [JsonProperty(LanguageEn ? "Time after which a player can send a message (AntiSpam)" : "Время через которое игрок может отправлять сообщение (АнтиСпам)")]
                    public Int32 FloodTime;
                    [JsonProperty(LanguageEn ? "Additional Anti-Spam settings" : "Дополнительная настройка Анти-Спама")]
                    public AntiSpamDuples AntiSpamDuplesSetting;
                    internal class AntiSpamDuples
                    {
                        [JsonProperty(LanguageEn ? "Enable additional spam protection (Anti-duplicates, duplicate messages)" : "Включить дополнительную защиту от спама (Анти-дубликаты, повторяющие сообщения)")]
                        public Boolean AntiSpamDuplesActivate;
                        [JsonProperty(LanguageEn ? "How many duplicate messages does a player need to make to be confused by the system" : "Сколько дублирующих сообщений нужно сделать игроку чтобы его замутила система")]
                        public Int32 TryDuples;
                        [JsonProperty(LanguageEn ? "Setting up automatic muting for duplicates" : "Настройка автоматического мута за дубликаты")]
                        public ControllerMute.Muted MuteSetting;
                    }

                }

                internal class PM
                {
                    [JsonProperty(LanguageEn ? "Enable Private Messages" : "Включить личные сообщения")]
                    public Boolean PMActivate;
                    [JsonProperty(LanguageEn ? "Sound when receiving a private message" : "Звук при при получении личного сообщения")]
                    public String SoundPM;
                }

                [JsonProperty(LanguageEn ? "Enable PM ignore for players (/ignore nick or via interface)" : "Включить игнор ЛС игрокам(/ignore nick или через интерфейс)")]
                public Boolean IgnoreUsePM;
                [JsonProperty(LanguageEn ? "Hide the issue of items to the Admin from the chat" : "Скрыть из чата выдачу предметов Админу")]
                public Boolean HideAdminGave;
                [JsonProperty(LanguageEn ? "Move mute to team chat (In case of a mute, the player will not be able to write even to the team chat)" : "Переносить мут в командный чат(В случае мута, игрок не сможет писать даже в командный чат)")]
                public Boolean MuteTeamChat;
            }

            internal class FormattingMessage
            {
                [JsonProperty(LanguageEn ? "Enable message formatting [Will control caps, message format] (true - yes/false - no)" : "Включить форматирование сообщений [Будет контроллировать капс, формат сообщения] (true - да/false - нет)")]
                public Boolean FormatMessage;
                [JsonProperty(LanguageEn ? "Use a list of banned words (true - yes/false - no)" : "Использовать список запрещенных слов (true - да/false - нет)")]
                public Boolean UseBadWords;
                [JsonProperty(LanguageEn ? "The word that will replace the forbidden word" : "Слово которое будет заменять запрещенное слово")]
                public String ReplaceBadWord;
                [JsonProperty(LanguageEn ? "The list of forbidden words [Forbidden Word] = Whether to replace it in part of the word (for example, Vasya Fucking => Vasya ***) (true - yes/false - no)" : "Список запрещенных слов [ЗапрещенноеСлово] = Заменять ли его в части слова (например ВасяБля => Вася***) (true - да/false - нет)")]
                public Dictionary<String, Boolean> BadWords;
                [JsonProperty(LanguageEn ? "Nickname controller setup" : "Настройка контроллера ников")]
                public NickController ControllerNickname;
                internal class NickController
                {
                    [JsonProperty(LanguageEn ? "Enable player nickname formatting (message formatting must be enabled)" : "Включить форматирование ников игроков (должно быть включено форматирование сообщений)")]
                    public Boolean UseNickController;
                    [JsonProperty(LanguageEn ? "The word that will replace the forbidden word (You can leave it blank and it will just delete)" : "Слово которое будет заменять запрещенное слово (Вы можете оставить пустым и будет просто удалять)")]
                    public String ReplaceBadNick;
                    [JsonProperty(LanguageEn ? "The list of forbidden nicknames [Forbidden Word] = Whether to replace it in part of the word (for example, Vasya Fucking => Vasya ***) (true - yes/false - no)" : "Список запрещенных ников [ЗапрещенноеСлово] = Заменять ли его в части слова (например ВасяБля => Вася***) (true - да/false - нет)")]
                    public Dictionary<String, Boolean> BadNicks;
                    [JsonProperty(LanguageEn ? "List of allowed links in nicknames" : "Список разрешенных ссылок в никах")]
                    public List<String> AllowedLinkNick;
                }

            }

        }

        [JsonProperty(LanguageEn ? "Setting up chat alerts" : "Настройка оповещений в чате")]
        public ControllerAlert ControllerAlertSetting;
        internal class ControllerAlert
        {
            [JsonProperty(LanguageEn ? "Setting up chat alerts" : "Настройка оповещений в чате")]
            public Alert AlertSetting;
            [JsonProperty(LanguageEn ? "Setting notifications about the status of the player's session" : "Настройка оповещений о статусе сессии игрока")]
            public PlayerSession PlayerSessionSetting;
            [JsonProperty(LanguageEn ? "Configuring administrator session status alerts" : "Настройка оповещений о статусе сессии администратора")]
            public AdminSession AdminSessionSetting;
            [JsonProperty(LanguageEn ? "Setting up personal notifications to the player when connecting" : "Настройка персональных оповоещений игроку при коннекте")]
            public PersonalAlert PersonalAlertSetting;
            internal class Alert
            {
                [JsonProperty(LanguageEn ? "Enable automatic messages in chat (true - yes/false - no)" : "Включить автоматические сообщения в чат (true - да/false - нет)")]
                public Boolean AlertMessage;
                [JsonProperty(LanguageEn ? "Type of automatic messages : true - sequential / false - random" : "Тип автоматических сообщений : true - поочередные/false - случайные")]
                public Boolean AlertMessageType;
                [JsonProperty(LanguageEn ? "List of automatic messages in chat" : "Список автоматических сообщений в чат")]
                public LanguageController MessageList;
                [JsonProperty(LanguageEn ? "Interval for sending messages to chat (Broadcaster) (in seconds)" : "Интервал отправки сообщений в чат (Броадкастер) (в секундах)")]
                public Int32 MessageListTimer;
            }

            internal class PlayerSession
            {
                [JsonProperty(LanguageEn ? "When a player is notified about the entry / exit of the player, display his avatar opposite the nickname (true - yes / false - no)" : "При уведомлении о входе/выходе игрока отображать его аватар напротив ника (true - да/false - нет)")]
                public Boolean ConnectedAvatarUse;
                [JsonProperty(LanguageEn ? "Notify in chat when a player enters (true - yes/false - no)" : "Уведомлять в чате о входе игрока (true - да/false - нет)")]
                public Boolean ConnectedAlert;
                [JsonProperty(LanguageEn ? "Enable random notifications when a player from the list enters (true - yes / false - no)" : "Включить случайные уведомления о входе игрока из списка (true - да/false - нет)")]
                public Boolean ConnectionAlertRandom;
                [JsonProperty(LanguageEn ? "Show the country of the entered player (true - yes/false - no)" : "Отображать страну зашедшего игрока (true - да/false - нет")]
                public Boolean ConnectedWorld;
                [JsonProperty(LanguageEn ? "Notify when a player enters the chat (selected from the list) (true - yes/false - no)" : "Уведомлять о выходе игрока в чат(выбираются из списка) (true - да/false - нет)")]
                public Boolean DisconnectedAlert;
                [JsonProperty(LanguageEn ? "Enable random player exit notifications (true - yes/false - no)" : "Включить случайные уведомления о выходе игрока (true - да/false - нет)")]
                public Boolean DisconnectedAlertRandom;
                [JsonProperty(LanguageEn ? "Display reason for player exit (true - yes/false - no)" : "Отображать причину выхода игрока (true - да/false - нет)")]
                public Boolean DisconnectedReason;
                [JsonProperty(LanguageEn ? "Random player entry notifications({0} - player's nickname, {1} - country (if country display is enabled)" : "Случайные уведомления о входе игрока({0} - ник игрока, {1} - страна(если включено отображение страны)")]
                public LanguageController RandomConnectionAlert;
                [JsonProperty(LanguageEn ? "Random notifications about the exit of the player ({0} - player's nickname, {1} - the reason for the exit (if the reason is enabled)" : "Случайные уведомления о выходе игрока({0} - ник игрока, {1} - причина выхода(если включена причина)")]
                public LanguageController RandomDisconnectedAlert;
            }

            internal class AdminSession
            {
                [JsonProperty(LanguageEn ? "Notify admin on the server in the chat (true - yes/false - no)" : "Уведомлять о входе админа на сервер в чат (true - да/false - нет)")]
                public Boolean ConnectedAlertAdmin;
                [JsonProperty(LanguageEn ? "Notify about admin leaving the server in chat (true - yes/false - no)" : "Уведомлять о выходе админа на сервер в чат (true - да/false - нет)")]
                public Boolean DisconnectedAlertAdmin;
            }

            internal class PersonalAlert
            {
                [JsonProperty(LanguageEn ? "Enable random message to the player who has logged in (true - yes/false - no)" : "Включить случайное сообщение зашедшему игроку (true - да/false - нет)")]
                public Boolean UseWelcomeMessage;
                [JsonProperty(LanguageEn ? "List of messages to the player when entering" : "Список сообщений игроку при входе")]
                public LanguageController WelcomeMessage;
            }

        }

        public class LanguageController
        {
            [JsonProperty(LanguageEn ? "Setting up Multilingual Messages [Language Code] = Translation Variations" : "Настройка мультиязычных сообщений [КодЯзыка] = ВариацииПеревода")]
            public Dictionary<String, List<String>> LanguageMessages;
        }

        [JsonProperty(LanguageEn ? "Settings Rust+" : "Настройка Rust+")]
        public RustPlus RustPlusSettings;
        internal class RustPlus
        {
            [JsonProperty(LanguageEn ? "Use Rust+" : "Использовать Rust+")]
            public Boolean UseRustPlus;
            [JsonProperty(LanguageEn ? "Title for notification Rust+" : "Название для уведомления Rust+")]
            public String DisplayNameAlert;
        }

        [JsonProperty(LanguageEn ? "Configuring support plugins" : "Настройка плагинов поддержки")]
        public ReferenceSettings ReferenceSetting;
        internal class ReferenceSettings
        {
            [JsonProperty(LanguageEn ? "Settings XLevels" : "Настройка XLevels")]
            public XLevels XLevelsSettings;
            [JsonProperty(LanguageEn ? "Settings IQFakeActive" : "Настройка IQFakeActive")]
            public IQFakeActive IQFakeActiveSettings;
            [JsonProperty(LanguageEn ? "Settings IQRankSystem" : "Настройка IQRankSystem")]
            public IQRankSystem IQRankSystems;
            [JsonProperty(LanguageEn ? "Settings Clans" : "Настройка Clans")]
            public Clans ClansSettings;
            [JsonProperty(LanguageEn ? "Settings TranslationAPI" : "Настройка TranslationAPI")]
            public TranslataionApi translationApiSettings;
            [JsonProperty(LanguageEn ? "Settings SkillTree" : "Настройка SkillTree")]
            public SkillTree skillTreeSettings;
            [JsonProperty(LanguageEn ? "Settings PlayerRanks" : "Настройка PlayerRanks")]
            public PlayerRanks playerRanksSettings;
            [JsonProperty(LanguageEn ? "Settings XPrison" : "Настройка XPrison")]
            public XPrison xPrisonSettings;
            internal class TranslataionApi
            {
                [JsonProperty(LanguageEn ? "To use automatic message translation using the TranslationAPI" : "Использовать автоматический перевод сообщений с помощью TranslataionAPI")]
                public Boolean useTranslationApi;
                [JsonProperty(LanguageEn ? "Translate team chat" : "Переводить командный чат")]
                public Boolean translateTeamChat;
                [JsonProperty(LanguageEn ? "Translate chat in private messages." : "Переводить чат в личных сообщениях")]
                public Boolean translatePmChat;
                [JsonProperty(LanguageEn ? "The code for the preferred language (leave it empty, and then the translation will be done in each player's language)" : "Код приоритетного языка (оставьте пустым и тогда для каждого игрока будет переводиться на его языке клиента)")]
                public String codeLanguagePrimary;
            }

            internal class Clans
            {
                [JsonProperty(LanguageEn ? "Display a clan tag in the chat (if Clans are installed)" : "Отображать в чате клановый тэг (если установлены Clans)")]
                public Boolean UseClanTag;
                [JsonProperty(LanguageEn ? "The color of the additional tag" : "Цвет дополнительного тэга")]
                public String colorTag;
            }

            internal class IQRankSystem
            {
                [JsonProperty(LanguageEn ? "Rank display format in chat ( {0} is the user's rank, do not delete this value)" : "Формат отображения ранга в чате ( {0} - это ранг юзера, не удаляйте это значение)")]
                public String FormatRank;
                [JsonProperty(LanguageEn ? "Time display format with IQRank System in chat ( {0} is the user's time, do not delete this value)" : "Формат отображения времени с IQRankSystem в чате ( {0} - это время юзера, не удаляйте это значение)")]
                public String FormatRankTime;
                [JsonProperty(LanguageEn ? "Use support IQRankSystem" : "Использовать поддержку рангов")]
                public Boolean UseRankSystem;
                [JsonProperty(LanguageEn ? "Show players their played time next to their rank" : "Отображать игрокам их отыгранное время рядом с рангом")]
                public Boolean UseTimeStandart;
            }

            internal class IQFakeActive
            {
                [JsonProperty(LanguageEn ? "Use support IQFakeActive" : "Использовать поддержку IQFakeActive")]
                public Boolean UseIQFakeActive;
            }

            internal class XLevels
            {
                [JsonProperty(LanguageEn ? "Use support XLevels" : "Использовать поддержку XLevels")]
                public Boolean UseXLevels;
                [JsonProperty(LanguageEn ? "Use full prefix with level from XLevel (true) otherwise only level (false)" : "Использовать полный префикс с уровнем из XLevel (true) иначе только уровень (false)")]
                public Boolean UseFullXLevels;
                [JsonProperty(LanguageEn ? "The color of the additional tag" : "Цвет дополнительного тэга")]
                public String colorTag;
            }

            internal class XPrison
            {
                [JsonProperty(LanguageEn ? "Use support XPrison" : "Использовать поддержку XPrison")]
                public Boolean UseXPrison;
                [JsonProperty(LanguageEn ? "The color of the additional tag" : "Цвет дополнительного тэга")]
                public String colorTag;
            }

            internal class SkillTree
            {
                [JsonProperty(LanguageEn ? "Use support SkillTree" : "Использовать поддержку SkillTree")]
                public Boolean UseSkillTree;
                [JsonProperty(LanguageEn ? "Use full XP + Level information output (true), use only Level (false)" : "Использовать полный вывод информации XP + Level (true), использовать только Level (false)")]
                public Boolean UseFullSkillTree;
                [JsonProperty(LanguageEn ? "Use prestige" : "Использовать престиж")]
                public Boolean UsePrestigeSkillTree;
                [JsonProperty(LanguageEn ? "The color of the additional tag" : "Цвет дополнительного тэга")]
                public String colorTag;
            }

            internal class PlayerRanks
            {
                [JsonProperty(LanguageEn ? "Use support PlayerRanks" : "Использовать поддержку PlayerRanks")]
                public Boolean UsePlayerRanks;
                [JsonProperty(LanguageEn ? "The color of the additional tag" : "Цвет дополнительного тэга")]
                public String colorTag;
            }

        }

        [JsonProperty(LanguageEn ? "Setting up an answering machine" : "Настройка автоответчика")]
        public AnswerMessage AnswerMessages;
        internal class AnswerMessage
        {
            [JsonProperty(LanguageEn ? "Enable auto-reply? (true - yes/false - no)" : "Включить автоответчик?(true - да/false - нет)")]
            public bool UseAnswer;
            [JsonProperty(LanguageEn ? "Customize Messages [Keyword] = Reply" : "Настройка сообщений [Ключевое слово] = Ответ")]
            public Dictionary<String, LanguageController> AnswerMessageList;
        }

        [JsonProperty(LanguageEn ? "Disable additional chat duplication in RCON" : "Отключить дополнительное дублированиеи чата в RCON")]
        public Boolean disableRconBroadcast;
        [JsonProperty(LanguageEn ? "Additional setting" : "Дополнительная настройка")]
        public OtherSettings OtherSetting;
        internal class OtherSettings
        {
            [JsonProperty("SteamApiKey (https://steamcommunity.com/dev/apikey)")]
            public String renameSteamApiKey;
            [JsonProperty(LanguageEn ? "Enable the /online command (true - yes / false - no)" : "Включить команду /online (true - да/ false - нет)")]
            public Boolean UseCommandOnline;
            [JsonProperty(LanguageEn ? "Use shortened format /online (will only display quantity)" : "Использовать сокращенный формат /online (будет отображать только количество)")]
            public Boolean UseCommandShortOnline;
            [JsonProperty(LanguageEn ? "Compact logging of messages" : "Компактное логирование сообщений")]
            public CompactLoggetChat CompactLogsChat;
            [JsonProperty(LanguageEn ? "Setting up message logging" : "Настройка логирования сообщений")]
            public LoggedChat LogsChat;
            [JsonProperty(LanguageEn ? "Setting up logging of personal messages of players" : "Настройка логирования личных сообщений игроков")]
            public General LogsPMChat;
            [JsonProperty(LanguageEn ? "Setting up chat/voice lock/unlock logging" : "Настройка логирования блокировок/разблокировок чата/голоса")]
            public General LogsMuted;
            [JsonProperty(LanguageEn ? "Setting up logging of chat commands from players" : "Настройка логирования чат-команд от игроков")]
            public General LogsChatCommands;
            internal class CompactLoggetChat
            {
                [JsonProperty(LanguageEn ? "Display Steam64ID in the log (true - yes/false - no)" : "Отображать в логе Steam64ID (true - да/false - нет)")]
                public Boolean ShowSteamID;
                [JsonProperty(LanguageEn ? "Setting up compact message logging" : "Настройка компактного логирования сообщений")]
                public LoggedChat LogsCompactChat;
            }

            internal class LoggedChat
            {
                [JsonProperty(LanguageEn ? "Setting up general chat logging" : "Настройка логирования общего чата")]
                public General GlobalChatSettings;
                [JsonProperty(LanguageEn ? "Setting up team chat logging" : "Настройка логирования тим чата")]
                public General TeamChatSettings;
            }

            internal class General
            {
                [JsonProperty(LanguageEn ? "Enable logging (true - yes/false - no)" : "Включить логирование (true - да/false - нет)")]
                public Boolean UseLogged;
                [JsonProperty(LanguageEn ? "Webhooks channel for logging" : "Webhooks канала для логирования")]
                public String Webhooks;
            }

        }

        public static Configuration GetNewConfiguration();
    }

    protected override void LoadConfig();
    private void RegisteredPermissions();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    public GeneralInformation GeneralInfo;
    public Dictionary<UInt64, User> UserInformation;
    public Dictionary<UInt64, AntiNoob> UserInformationConnection;
    internal class AntiNoob
    {
        public DateTime DateConnection;
        public Boolean IsNoob(Int32 TimeBlocked);
        public Double LeftTime(Int32 TimeBlocked);
    }

    public class User
    {
        public Information Info;
        public Setting Settings;
        public Mute MuteInfo;
        internal class Information
        {
            public String Prefix;
            public String ColorNick;
            public String ColorMessage;
            public String Rank;
            public String CustomColorNick;
            public String CustomColorMessage;
            public List<String> PrefixList;
        }

        internal class Setting
        {
            public Boolean TurnPM;
            public Boolean TurnAlert;
            public Boolean TurnBroadcast;
            public Boolean TurnSound;
            public List<UInt64> IgnoreUsers;
            public Boolean IsIgnored(UInt64 TargetID);
            public void IgnoredAddOrRemove(UInt64 TargetID);
        }

        internal class Mute
        {
            public Double TimeMuteChat;
            public Double TimeMuteVoice;
            public Double GetTime(MuteType Type);
            public void SetMute(MuteType Type, Int32 Time);
            public void UnMute(MuteType Type);
            public Boolean IsMute(MuteType Type);
        }

    }

    public class GeneralInformation
    {
        public Boolean TurnMuteAllChat;
        public Boolean TurnMuteAllVoice;
        public Dictionary<UInt64, RenameInfo> RenameList;
        internal class RenameInfo
        {
            public String RenameNick;
            public UInt64 RenameID;
        }

        public RenameInfo GetInfoRename(UInt64 UserID);
    }

    private void MigrateDataToNoob();
    private void UserConnecteionData(BasePlayer player);
     void ReadData();
     void WriteData();
    private bool OnPlayerChat(BasePlayer player, string message, Chat.ChatChannel channel);
    private object OnServerMessage(String message, String name);
     void OnPlayerCommand(BasePlayer player, string command, string[] args);
     void OnUserPermissionGranted(string id, string permName);
     void OnUserPermissionRevoked(string id, string permName);
     void OnUserGroupAdded(string id, string groupName);
     void OnUserGroupRemoved(string id, string groupName);
     void OnGroupPermissionGranted(string name, string perm);
     void OnGroupPermissionRevoked(string name, string perm);
     object OnPlayerVoice(BasePlayer player, Byte[] data);
     void Init();
    private void OnServerInitialized();
    private void CheckValidateUsers();
     void OnPlayerConnected(BasePlayer player);
    private void OnUserConnected(IPlayer player);
     void Unload();
     void OnPlayerDisconnected(BasePlayer player, string reason);
    private void DiscordLoggCommand(BasePlayer player, String Command, String[] Args);
    private void DiscordLoggChat(BasePlayer player, Chat.ChatChannel Channel, String MessageLogged);
    private void DiscordCompactLoggChat(BasePlayer player, Chat.ChatChannel Channel, String MessageLogged);
    private void DiscordLoggPM(BasePlayer Sender, BasePlayer Reciepter, String MessageLogged);
    private void DiscordLoggMuted(BasePlayer Target, MuteType Type, String Reason, String TimeBlocked, BasePlayer Moderator);
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
    public List<String> GetMesagesList(BasePlayer player, Dictionary<String, List<String>> LanguageMessages);
    public String GetMessages(BasePlayer player, Dictionary<String, List<String>> LanguageMessages);
    private void SetupParametres(String ID, String Permissions);
    private void RemoveParametres(String ID, String Permissions);
    private void ReplyPlayerChat(Chat.ChatChannel channel, BasePlayer player, BasePlayer playerSender, String OutMessage, String FormatMessage, String FormatPlayer, UInt64 RenameID);
    private ListHashSet<BasePlayer> GetPlayerList(BasePlayer player, Chat.ChatChannel channel);
    private void ReplyTranslationMessage(Chat.ChatChannel channel, BasePlayer player, BasePlayer playerSender, String OutMessage, String FormatMessage, String FormatPlayer, UInt64 RenameID);
     void ReplyChat(Chat.ChatChannel channel, BasePlayer player, String OutMessage, String FormatMessage, String FormatPlayer);
     void ReplySystem(BasePlayer player, String Message, String CustomPrefix, String CustomAvatar, String CustomHex);
     void ReplyBroadcast(String Message, String CustomPrefix, String CustomAvatar, Boolean AdminAlert);
     void ReplyBroadcast(String CustomPrefix, String CustomAvatar, Boolean AdminAlert, String LangKey, object[] args);
     void ReplyBroadcast(String CustomPrefix, String CustomAvatar, Boolean AdminAlert, Dictionary<String, List<String>> Messages, object[] args);
    public Boolean IsNoob(UInt64 userID, Int32 TimeBlocked);
    public void AnwserMessage(BasePlayer player, String Message);
    private void AddHistoryMessage(BasePlayer player, String Message);
    private String GetLastMessage(BasePlayer player, Int32 Count);
    public Dictionary<UInt64, FlooderInfo> Flooders;
    internal class FlooderInfo
    {
        public Double Time;
        public String LastMessage;
        public Int32 TryFlood;
    }

    private Tuple<String, Boolean> BadWordsCleaner(String formattingMessage, String replaceBadWord, Dictionary<String, Boolean> badWords);
    private String RemoveLinkText(String text);
    private String GetReferenceTags(BasePlayer player);
    public Object IsGradientColorValue(String value);
    public static Regex regex;
    public static String ApplyGradientToText(String text, List<String> colors);
    private void SeparatorChat(Chat.ChatChannel channel, BasePlayer player, String Message);
    public void BroadcastAuto();
    private void AlertDisconnected(BasePlayer player, String reason);
    private void AlertController(BasePlayer player);
    private void MutePlayer(BasePlayer Target, MuteType Type, Int32 ReasonIndex, BasePlayer Moderator, String ReasonCustom, Int32 TimeCustom, Boolean HideMute, Boolean Command, String fakeUserId);
    private void UnmutePlayer(BasePlayer Target, MuteType Type, BasePlayer Moderator, Boolean HideUnmute, Boolean Command, String fakeUserId);
     void AlertUI(BasePlayer Sender, string[] arg);
     void AlertUI(BasePlayer Sender, BasePlayer Recipient, string[] arg);
     void Alert(BasePlayer Sender, string[] arg, Boolean IsAdmin);
     void Alert(BasePlayer Sender, BasePlayer Recipient, string[] arg);
    private Int32 GetPlayersOnlineShort();
    private List<String> GetPlayersOnline();
    private String GetPlayerFormat(String displayName, String userId);
    private void ControlledBadNick(IPlayer player);
    private class ImageUI
    {
        private const String _path;
        private const String _printPath;
        private readonly Dictionary<String, ImageData> _images;
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

    private static InterfaceBuilder _interface;
    private Dictionary<BasePlayer, InformationOpenedUI> LocalBase;
    private class InformationOpenedUI
    {
        public List<Configuration.ControllerParameters.AdvancedFuncion> ElementsPrefix;
        public List<Configuration.ControllerParameters.AdvancedFuncion> ElementsNick;
        public List<Configuration.ControllerParameters.AdvancedFuncion> ElementsChat;
        public List<Configuration.ControllerParameters.AdvancedFuncion> ElementsRanks;
        public Int32 SlideIndexPrefix;
        public Int32 SlideIndexNick;
        public Int32 SlideIndexChat;
        public Int32 SlideIndexRank;
    }

    private void DrawUI_IQChat_Update_DisplayName(BasePlayer player);
    private void DrawUI_IQChat_Context(BasePlayer player);
    private void DrawUI_IQChat_Context_AdminAndModeration(BasePlayer player);
    private void DrawUI_IQChat_Update_MuteChat_All(BasePlayer player);
    private void DrawUI_IQChat_Update_MuteVoice_All(BasePlayer player);
    private void DrawUI_IQChat_Ignore_Alert(BasePlayer player, BasePlayer Target, UInt64 fakeUserId);
    private void DrawUI_IQChat_Mute_Alert(BasePlayer player, BasePlayer Target, UInt64 IDFake);
    private void DrawUI_IQChat_Mute_Alert_Reasons(BasePlayer player, BasePlayer Target, MuteType Type, UInt64 IDFake);
    private void DrawUI_IQChat_Mute_Alert_Reasons(BasePlayer player, BasePlayer Target, String Reason, Int32 Y, MuteType Type, UInt64 IDFake);
    private void DrawUI_IQChat_Mute_And_Ignore(BasePlayer player, SelectedAction Action);
    private void DrawUI_IQChat_Mute_And_Ignore_Player_Panel(BasePlayer player, SelectedAction Action, Int32 Page, String SearchName);
    private List<FakePlayer> GetFilteredPlayers(string searchName);
    private Boolean TryGetUserIdAsUlong(String userId, UInt64 userIdAsUlong);
    private Boolean HasMorePages(IEnumerable<T> items, Int32 page);
    private IEnumerable<T> GetPageOfPlayers(IEnumerable<T> items, Int32 page);
    private IOrderedEnumerable<BasePlayer> GetPlayerList(String searchName, SelectedAction action);
    private void DrawUI_IQChat_Mute_And_Ignore_Pages(BasePlayer player, Boolean IsNextPage, SelectedAction Action, Int32 Page);
    private void DrawUI_IQChat_Mute_And_Ignore_Player(BasePlayer player, SelectedAction Action, IEnumerable<BasePlayer> PlayerList, IEnumerable<FakePlayer> FakePlayerList);
    private void DrawUI_IQChat_Update_Check_Box(BasePlayer player, ElementsSettingsType Type, String OffsetMin, String OffsetMax, Boolean StatusCheckBox);
    private void DrawUI_IQChat_Sliders(BasePlayer player, String Name, String OffsetMin, String OffsetMax, TakeElementUser ElementType);
    private void DrawUI_IQChat_Slider_Update_Argument(BasePlayer player, TakeElementUser ElementType);
    private void DrawUI_IQChat_DropList(BasePlayer player, String OffsetMin, String OffsetMax, String Title, TakeElementUser ElementType);
    private void DrawUI_IQChat_OpenDropList(BasePlayer player, TakeElementUser ElementType, Int32 Page);
    private void DrawUI_IQChat_OpenDropListArgument(BasePlayer player, TakeElementUser ElementType, Configuration.ControllerParameters.AdvancedFuncion Info, Int32 X, Int32 Y, Int32 Count);
    private void DrawUI_IQChat_OpenDropListArgument(BasePlayer player, Int32 Count);
    private void DrawUI_IQChat_Alert(BasePlayer player, String Description, String Title);
    private class InterfaceBuilder
    {
        public static InterfaceBuilder Instance;
        public const String UI_Chat_Context;
        public const String UI_Chat_Context_Visual_Nick;
        public const String UI_Chat_Alert;
        public Dictionary<String, String> Interfaces;
        public InterfaceBuilder();
        public static void AddInterface(String name, String json);
        public static string GetInterface(String name);
        public static void DestroyAll();
        private void BuildingVisualNick();
        private void BuildingStaticContext();
        private void BuildingCheckBox();
        private void BuildingSlider();
        private void BuildingSliderUpdateArgument();
        private void BuildingMuteAndIgnore();
        private void BuildingMuteAndIgnorePlayerPanel();
        private void BuildingMuteAndIgnorePlayer();
        private void BuildingMuteAndIgnorePages();
        private void BuildingMuteAndIgnorePanelAlert();
        private void BuildingMuteAlert();
        private void BuildingMuteAlert_DropList_Title();
        private void BuildingMuteAlert_DropList_Reason();
        private void BuildingIgnoreAlert();
        private void BuildingDropList();
        private void BuildingOpenDropList();
        private void BuildingElementDropList();
        private void BuildingElementDropListTakeLine();
        private void BuildingModerationStatic();
        private void BuildingMuteAllChat();
        private void BuildingMuteAllVoice();
        private void BuildingAlertUI();
    }

    [ConsoleCommand("newui.cmd")]
    private void ConsoleCommandFuncional(ConsoleSystem.Arg arg);
    [ChatCommand("chat")]
    private void ChatCommandOpenedUI(BasePlayer player);
    [ChatCommand("cnick")]
    private void ColoredNickSetup(BasePlayer player, String cmd, String[] args);
    [ChatCommand("cmsg")]
    private void ColoredMsgSetup(BasePlayer player, String cmd, String[] args);
    private void ConsoleOrPrintMessage(BasePlayer player, String Messages);
    [ConsoleCommand("hmute")]
     void HideMuteConsole(ConsoleSystem.Arg arg);
    [ChatCommand("hmute")]
     void HideMute(BasePlayer Moderator, string cmd, string[] arg);
    [ConsoleCommand("hunmute")]
     void HideUnMuteConsole(ConsoleSystem.Arg arg);
    [ChatCommand("hunmute")]
     void HideUnMute(BasePlayer Moderator, string cmd, string[] arg);
    [ConsoleCommand("mutefull")]
     void MuteCustomAdminFull(ConsoleSystem.Arg arg);
    [ConsoleCommand("mute")]
     void MuteCustomAdmin(ConsoleSystem.Arg arg);
    [ChatCommand("mute")]
     void MuteCustomChat(BasePlayer Moderator, string cmd, string[] arg);
    [ChatCommand("mutevoice")]
     void MuteVoiceCustomChat(BasePlayer Moderator, string cmd, string[] arg);
    [ConsoleCommand("mutevoice")]
     void MuteCustomVoiceAdmin(ConsoleSystem.Arg arg);
    [ConsoleCommand("unmute")]
     void UnMuteCustomAdmin(ConsoleSystem.Arg arg);
    [ConsoleCommand("unmutevoice")]
     void UnMuteVoiceCustomAdmin(ConsoleSystem.Arg arg);
    [ChatCommand("unmute")]
     void UnMuteCustomChat(BasePlayer Moderator, string cmd, string[] arg);
    [ChatCommand("unmutevoice")]
     void UnMuteVoiceCustomChat(BasePlayer Moderator, string cmd, string[] arg);
    [ChatCommand("online")]
    private void ShowPlayerOnline(BasePlayer player);
    [ConsoleCommand("online")]
    private void ShowPlayerOnlineConsole(ConsoleSystem.Arg arg);
    [ChatCommand("alert")]
    private void AlertChatCommand(BasePlayer Sender, String cmd, String[] args);
    [ChatCommand("adminalert")]
    private void AdminAlertChatCommand(BasePlayer Sender, String cmd, String[] args);
    [ChatCommand("alertui")]
    private void AlertUIChatCommand(BasePlayer Sender, String cmd, String[] args);
    [ChatCommand("alertuip")]
    private void AlertUIPChatCommand(BasePlayer Sender, String cmd, String[] args);
    [ChatCommand("saybro")]
    private void AlertOnlyPlayerChatCommand(BasePlayer Sender, String cmd, String[] args);
    [ConsoleCommand("alert")]
    private void AlertConsoleCommand(ConsoleSystem.Arg args);
    [ConsoleCommand("adminalert")]
    private void AdminAlertConsoleCommand(ConsoleSystem.Arg args);
    [ConsoleCommand("alertui")]
    private void AlertUIConsoleCommand(ConsoleSystem.Arg args);
    [ConsoleCommand("alertuip")]
    private void AlertUIPConsoleCommand(ConsoleSystem.Arg args);
    [ConsoleCommand("saybro")]
    private void AlertOnlyPlayerConsoleCommand(ConsoleSystem.Arg args);
    [ChatCommand("rename")]
    private void ChatCommandRename(BasePlayer Renamer, string command, string[] args);
    [ChatCommand("rename.reset")]
    private void ChatCommandRenameReset(BasePlayer Renamer);
    [ConsoleCommand("rename.reset")]
    private void ConsoleCommandRenameReset(ConsoleSystem.Arg args);
    [ConsoleCommand("rename")]
    private void ConsoleCommandRename(ConsoleSystem.Arg args);
    private void RenameUpdate(BasePlayer Renamer, String NewName);
    private void RenameReset(BasePlayer Renamer);
    [ConsoleCommand("set")]
    private void CommandSet(ConsoleSystem.Arg args);
    private void ReplyTranslationPM(BasePlayer Sender, BasePlayer TargetUser, String Message, String DisplayNameSender, String TargetDisplayName);
    [ChatCommand("pm")]
     void PmChat(BasePlayer Sender, String cmd, String[] arg);
    [ChatCommand("r")]
     void RChat(BasePlayer Sender, string cmd, string[] arg);
    [ChatCommand("ignore")]
     void IgnorePlayerPM(BasePlayer player, String cmd, String[] arg);
    private Boolean HasInvalidHexColor(List<String> colorList, String invalidHex);
    private Boolean IsValidHexColor(String color);
    private String JoinStringList(List<String> inputList);
    private List<String> ConvertStringToList(String input);
    private BasePlayer GetPlayerNickOrID(String Info);
    private new void LoadDefaultMessages();
    private void Log(String LoggedMessage);
    public String FormatTime(Double Second, String UserID);
    private String GetMessageInArgs(BasePlayer Sender, String[] arg);
    private String Format(Int32 units, String form1, String form2, String form3);
     void API_SEND_PLAYER(BasePlayer player, String PlayerFormat, String Message, String Avatar, Chat.ChatChannel channel);
     void API_SEND_PLAYER_PM(BasePlayer player, string DisplayName, String userID, string Message);
     void API_SEND_PLAYER_CONNECTED(String DisplayName, String country, String userID);
     void API_SEND_PLAYER_DISCONNECTED(String DisplayName, String reason, String userID);
     void API_ALERT(String Message, Chat.ChatChannel channel, String CustomPrefix, String CustomAvatar, String CustomHex);
     void API_ALERT_PLAYER(BasePlayer player, String Message, String CustomPrefix, String CustomAvatar, String CustomHex);
     void API_ALERT_PLAYER_UI(BasePlayer player, String Message);
     Boolean API_CHECK_MUTE_CHAT(BasePlayer.EncryptedValue<UInt64> ID);
     Boolean API_CHECK_MUTE_CHAT(UInt64 ID);
     Boolean API_CHECK_VOICE_CHAT(BasePlayer.EncryptedValue<UInt64> ID);
     Boolean API_CHECK_VOICE_CHAT(UInt64 ID);
     Boolean API_IS_IGNORED(BasePlayer.EncryptedValue<UInt64> UserHas, BasePlayer.EncryptedValue<UInt64> User);
     Boolean API_IS_IGNORED(UInt64 UserHas, UInt64 User);
     String API_GET_PREFIX(BasePlayer.EncryptedValue<UInt64> ID);
     String API_GET_PREFIX(UInt64 ID);
     String API_GET_CHAT_COLOR(BasePlayer.EncryptedValue<UInt64> ID);
     String API_GET_CHAT_COLOR(UInt64 ID);
     String API_GET_NICK_COLOR(BasePlayer.EncryptedValue<UInt64> ID);
     String API_GET_NICK_COLOR(ulong ID);
     String API_GET_DEFAULT_PREFIX();
     String API_GET_DEFAULT_NICK_COLOR();
     String API_GET_DEFAULT_MESSAGE_COLOR();
     Int32 API_GET_DEFAULT_SIZE_MESSAGE();
     Int32 API_GET_DEFAULT_SIZE_NICK();
}

public class FakePlayer
{
    [JsonProperty("userId")]
    public String userId;
    [JsonProperty("displayName")]
    public String displayName;
    public Boolean isMuted;
}

public class TranslationState
{
    public Boolean IsProcessed { get; set; }
    public String Translation { get; set; }
    public String DoTranslation { get; set; }
}

 class Response
{
    [JsonProperty("country")]
    public string Country { get; set; }
}

private class Configuration
{
    [JsonProperty(LanguageEn ? "Setting up player information" : "Настройка информации о игроке")]
    public ControllerConnection ControllerConnect;
    internal class ControllerConnection
    {
        [JsonProperty(LanguageEn ? "Function switches" : "Перключатели функций")]
        public Turned Turneds;
        [JsonProperty(LanguageEn ? "Setting Standard Values" : "Настройка стандартных значений")]
        public SetupDefault SetupDefaults;
        internal class SetupDefault
        {
            [JsonProperty(LanguageEn ? "This prefix will be set if the player entered the server for the first time or in case of expiration of the rights to the prefix that he had earlier" : "Данный префикс установится если игрок впервые зашел на сервер или в случае окончания прав на префикс, который у него стоял ранее")]
            public String PrefixDefault;
            [JsonProperty(LanguageEn ? "This nickname color will be set if the player entered the server for the first time or in case of expiration of the rights to the nickname color that he had earlier" : "Данный цвет ника установится если игрок впервые зашел на сервер или в случае окончания прав на цвет ника, который у него стоял ранее")]
            public String NickDefault;
            [JsonProperty(LanguageEn ? "This chat color will be set if the player entered the server for the first time or in case of expiration of the rights to the chat color that he had earlier" : "Данный цвет чата установится если игрок впервые зашел на сервер или в случае окончания прав на цвет чата, который у него стоял ранее")]
            public String MessageDefault;
        }

        internal class Turned
        {
            [JsonProperty(LanguageEn ? "Set automatically a prefix to a player when he got the rights to it" : "Устанавливать автоматически префикс игроку, когда он получил права на него")]
            public Boolean TurnAutoSetupPrefix;
            [JsonProperty(LanguageEn ? "Set automatically the color of the nickname to the player when he got the rights to it" : "Устанавливать автоматически цвет ника игроку, когда он получил права на него")]
            public Boolean TurnAutoSetupColorNick;
            [JsonProperty(LanguageEn ? "Set the chat color automatically to the player when he got the rights to it" : "Устанавливать автоматически цвет чата игроку, когда он получил права на него")]
            public Boolean TurnAutoSetupColorChat;
            [JsonProperty(LanguageEn ? "Automatically reset the prefix when the player's rights to it expire" : "Сбрасывать автоматически префикс при окончании прав на него у игрока")]
            public Boolean TurnAutoDropPrefix;
            [JsonProperty(LanguageEn ? "Automatically reset the color of the nickname when the player's rights to it expire" : "Сбрасывать автоматически цвет ника при окончании прав на него у игрока")]
            public Boolean TurnAutoDropColorNick;
            [JsonProperty(LanguageEn ? "Automatically reset the color of the chat when the rights to it from the player expire" : "Сбрасывать автоматически цвет чата при окончании прав на него у игрока")]
            public Boolean TurnAutoDropColorChat;
        }

    }

    [JsonProperty(LanguageEn ? "Setting options for the player" : "Настройка параметров для игрока")]
    public ControllerParameters ControllerParameter;
    internal class ControllerParameters
    {
        [JsonProperty(LanguageEn ? "Setting the display of options for player selection" : "Настройка отображения параметров для выбора игрока")]
        public VisualSettingParametres VisualParametres;
        [JsonProperty(LanguageEn ? "List and customization of colors for a nickname" : "Список и настройка цветов для ника")]
        public List<AdvancedFuncion> NickColorList;
        [JsonProperty(LanguageEn ? "List and customize colors for chat messages" : "Список и настройка цветов для сообщений в чате")]
        public List<AdvancedFuncion> MessageColorList;
        [JsonProperty(LanguageEn ? "List and configuration of prefixes in chat" : "Список и настройка префиксов в чате")]
        public PrefixSetting Prefixes;
        internal class PrefixSetting
        {
            [JsonProperty(LanguageEn ? "Enable support for multiple prefixes at once (true - multiple prefixes can be set/false - only 1 can be set to choose from)" : "Включить поддержку нескольких префиксов сразу (true - можно установить несколько префиксов/false - установить можно только 1 на выбор)")]
            public Boolean TurnMultiPrefixes;
            [JsonProperty(LanguageEn ? "The maximum number of prefixes that can be set at a time (This option only works if setting multiple prefixes is enabled)" : "Максимальное количество префиксов, которое можно установить за раз(Данный параметр работает только если включена установка нескольких префиксов)")]
            public Int32 MaximumMultiPrefixCount;
            [JsonProperty(LanguageEn ? "List of prefixes and their settings" : "Список префиксов и их настройка")]
            public List<AdvancedFuncion> Prefixes;
        }

        internal class AdvancedFuncion
        {
            [JsonProperty(LanguageEn ? "Permission" : "Права")]
            public String Permissions;
            [JsonProperty(LanguageEn ? "Argument" : "Значение")]
            public String Argument;
            [JsonProperty(LanguageEn ? "Block the player's ability to select this parameter in the plugin menu (true - yes/false - no)" : "Заблокировать возможность выбрать данный параметр игроком в меню плагина (true - да/false - нет)")]
            public Boolean IsBlockSelected;
        }

        internal class VisualSettingParametres
        {
            [JsonProperty(LanguageEn ? "Player prefix selection display type - (0 - dropdown list, 1 - slider (Please note that if you have multi-prefix enabled, the dropdown list will be set))" : "Тип отображения выбора префикса для игрока - (0 - выпадающий список, 1 - слайдер (Учтите, что если у вас включен мульти-префикс, будет установлен выпадающий список))")]
            public SelectedParametres PrefixType;
            [JsonProperty(LanguageEn ? "Display type of player's nickname color selection - (0 - drop-down list, 1 - slider)" : "Тип отображения выбора цвета ника для игрока - (0 - выпадающий список, 1 - слайдер)")]
            public SelectedParametres NickColorType;
            [JsonProperty(LanguageEn ? "Display type of message color choice for the player - (0 - drop-down list, 1 - slider)" : "Тип отображения выбора цвета сообщения для игрока - (0 - выпадающий список, 1 - слайдер)")]
            public SelectedParametres ChatColorType;
            [JsonProperty(LanguageEn ? "IQRankSystem : Player rank selection display type - (0 - drop-down list, 1 - slider)" : "IQRankSystem : Тип отображения выбора ранга для игрока - (0 - выпадающий список, 1 - слайдер)")]
            public SelectedParametres IQRankSystemType;
        }

    }

    [JsonProperty(LanguageEn ? "Plugin mute settings" : "Настройка мута в плагине")]
    public ControllerMute ControllerMutes;
    internal class ControllerMute
    {
        [JsonProperty(LanguageEn ? "Prohibit sending messages in /pm and /r if the player's chat is blocked" : "Запрещать отправлять сообщения в /pm, /r - если у игрока заблокирован чат")]
        public Boolean mutedPM;
        [JsonProperty(LanguageEn ? "Setting up automatic muting" : "Настройка автоматического мута")]
        public AutoMute AutoMuteSettings;
        internal class AutoMute
        {
            [JsonProperty(LanguageEn ? "Enable automatic muting for forbidden words (true - yes/false - no)" : "Включить автоматический мут по запрещенным словам(true - да/false - нет)")]
            public Boolean UseAutoMute;
            [JsonProperty(LanguageEn ? "Reason for automatic muting" : "Причина автоматического мута")]
            public Muted AutoMuted;
        }

        [JsonProperty(LanguageEn ? "Additional setting for logging about mutes in discord" : "Дополнительная настройка для логирования о мутах в дискорд")]
        public LoggedFuncion LoggedMute;
        internal class LoggedFuncion
        {
            [JsonProperty(LanguageEn ? "Support for logging the last N messages (Discord logging about mutes must be enabled)" : "Поддержка логирования последних N сообщений (Должно быть включено логирование в дискорд о мутах)")]
            public Boolean UseHistoryMessage;
            [JsonProperty(LanguageEn ? "How many latest player messages to send in logging" : "Сколько последних сообщений игрока отправлять в логировании")]
            public Int32 CountHistoryMessage;
        }

        [JsonProperty(LanguageEn ? "Reasons to block chat" : "Причины для блокировки чата")]
        public List<Muted> MuteChatReasons;
        [JsonProperty(LanguageEn ? "Reasons to block your voice" : "Причины для блокировки голоса")]
        public List<Muted> MuteVoiceReasons;
        internal class Muted
        {
            [JsonProperty(LanguageEn ? "Reason for blocking" : "Причина для блокировки")]
            public String Reason;
            [JsonProperty(LanguageEn ? "Block time (in seconds)" : "Время блокировки(в секундах)")]
            public Int32 SecondMute;
        }

    }

    [JsonProperty(LanguageEn ? "Configuring Message Processing" : "Настройка обработки сообщений")]
    public ControllerMessage ControllerMessages;
    internal class ControllerMessage
    {
        [JsonProperty(LanguageEn ? "Basic settings for chat messages from the plugin" : "Основная настройка сообщений в чат от плагина")]
        public GeneralSettings GeneralSetting;
        [JsonProperty(LanguageEn ? "Configuring functionality switching in chat" : "Настройка переключения функционала в чате")]
        public TurnedFuncional TurnedFunc;
        [JsonProperty(LanguageEn ? "Player message formatting settings" : "Настройка форматирования сообщений игроков")]
        public FormattingMessage Formatting;
        internal class GeneralSettings
        {
            [JsonProperty(LanguageEn ? "Notify the player in chat about receiving a prefix/nickname color/chat color (true - yes/false - no)" : "Уведомлять игрока в чате о получении префикса/цвета ника/цвета чата (true - да/false - нет)")]
            public Boolean alertArgumentsInfoSetup;
            [JsonProperty(LanguageEn ? "Notify the player in chat about the end of a prefix/nickname color/chat color (true - yes/false - no)" : "Уведомлять игрока в чате о окончании префикса/цвета ника/цвета чата (true - да/false - нет)")]
            public Boolean alertArgumentsInfoRemove;
            [JsonProperty(LanguageEn ? "Customizing the chat alert format" : "Настройка формата оповещения в чате")]
            public BroadcastSettings BroadcastFormat;
            [JsonProperty(LanguageEn ? "Setting the mention format in the chat, via @" : "Настройка формата упоминания в чате, через @")]
            public AlertSettings AlertFormat;
            [JsonProperty(LanguageEn ? "Additional setting" : "Дополнительная настройка")]
            public OtherSettings OtherSetting;
            internal class BroadcastSettings
            {
                [JsonProperty(LanguageEn ? "The name of the notification in the chat" : "Наименование оповещения в чат")]
                public String BroadcastTitle;
                [JsonProperty(LanguageEn ? "Chat alert message color" : "Цвет сообщения оповещения в чат")]
                public String BroadcastColor;
                [JsonProperty(LanguageEn ? "Steam64ID for chat avatar" : "Steam64ID для аватарки в чате")]
                public String Steam64IDAvatar;
            }

            internal class AlertSettings
            {
                [JsonProperty(LanguageEn ? "The color of the player mention message in the chat" : "Цвет сообщения упоминания игрока в чате")]
                public String AlertPlayerColor;
                [JsonProperty(LanguageEn ? "Sound when receiving and sending a mention via @" : "Звук при при получении и отправки упоминания через @")]
                public String SoundAlertPlayer;
            }

            internal class OtherSettings
            {
                [JsonProperty(LanguageEn ? "Time after which the message will be deleted from the UI from the administrator" : "Время,через которое удалится сообщение с UI от администратора")]
                public Int32 TimeDeleteAlertUI;
                [JsonProperty(LanguageEn ? "The size of the message from the player in the chat" : "Размер сообщения от игрока в чате")]
                public Int32 SizeMessage;
                [JsonProperty(LanguageEn ? "Player nickname size in chat" : "Размер ника игрока в чате")]
                public Int32 SizeNick;
                [JsonProperty(LanguageEn ? "The size of the player's prefix in the chat (will be used if <size=N></size> is not set in the prefix itself)" : "Размер префикса игрока в чате (будет использовано, если в самом префиксе не установвлен <size=N></size>)")]
                public Int32 SizePrefix;
                [JsonProperty(LanguageEn ? "Nickname size according to privilege [permission] = size" : "Размер ника по привилегии [permission] = размер")]
                public Dictionary<String, Int32> sizeNickPrivilages;
                [JsonProperty(LanguageEn ? "Chat message size according to privilege [permission] = size" : "Размер сообщения в чате по привилегии [permission] = размер")]
                public Dictionary<String, Int32> sizeMessagePrivilages;
                public Int32 GetSizeNickOrMessage(BasePlayer player, Boolean nickOrMessage);
            }

        }

        internal class TurnedFuncional
        {
            [JsonProperty(LanguageEn ? "Configuring spam protection" : "Настройка защиты от спама")]
            public AntiSpam AntiSpamSetting;
            [JsonProperty(LanguageEn ? "Setting up a temporary chat block for newbies (who have just logged into the server)" : "Настройка временной блокировки чата новичкам (которые только зашли на сервер)")]
            public AntiNoob AntiNoobSetting;
            [JsonProperty(LanguageEn ? "Setting up private messages" : "Настройка личных сообщений")]
            public PM PMSetting;
            internal class AntiNoob
            {
                [JsonProperty(LanguageEn ? "Newbie protection in PM/R" : "Защита от новичка в PM/R")]
                public Settings AntiNoobPM;
                [JsonProperty(LanguageEn ? "Newbie protection in global and team chat" : "Защита от новичка в глобальном и коммандном чате")]
                public Settings AntiNoobChat;
                internal class Settings
                {
                    [JsonProperty(LanguageEn ? "Enable protection?" : "Включить защиту?")]
                    public Boolean AntiNoobActivate;
                    [JsonProperty(LanguageEn ? "Newbie Chat Lock Time" : "Время блокировки чата для новичка")]
                    public Int32 TimeBlocked;
                }

            }

            internal class AntiSpam
            {
                [JsonProperty(LanguageEn ? "Enable spam protection (Anti-spam)" : "Включить защиту от спама (Анти-спам)")]
                public Boolean AntiSpamActivate;
                [JsonProperty(LanguageEn ? "Time after which a player can send a message (AntiSpam)" : "Время через которое игрок может отправлять сообщение (АнтиСпам)")]
                public Int32 FloodTime;
                [JsonProperty(LanguageEn ? "Additional Anti-Spam settings" : "Дополнительная настройка Анти-Спама")]
                public AntiSpamDuples AntiSpamDuplesSetting;
                internal class AntiSpamDuples
                {
                    [JsonProperty(LanguageEn ? "Enable additional spam protection (Anti-duplicates, duplicate messages)" : "Включить дополнительную защиту от спама (Анти-дубликаты, повторяющие сообщения)")]
                    public Boolean AntiSpamDuplesActivate;
                    [JsonProperty(LanguageEn ? "How many duplicate messages does a player need to make to be confused by the system" : "Сколько дублирующих сообщений нужно сделать игроку чтобы его замутила система")]
                    public Int32 TryDuples;
                    [JsonProperty(LanguageEn ? "Setting up automatic muting for duplicates" : "Настройка автоматического мута за дубликаты")]
                    public ControllerMute.Muted MuteSetting;
                }

            }

            internal class PM
            {
                [JsonProperty(LanguageEn ? "Enable Private Messages" : "Включить личные сообщения")]
                public Boolean PMActivate;
                [JsonProperty(LanguageEn ? "Sound when receiving a private message" : "Звук при при получении личного сообщения")]
                public String SoundPM;
            }

            [JsonProperty(LanguageEn ? "Enable PM ignore for players (/ignore nick or via interface)" : "Включить игнор ЛС игрокам(/ignore nick или через интерфейс)")]
            public Boolean IgnoreUsePM;
            [JsonProperty(LanguageEn ? "Hide the issue of items to the Admin from the chat" : "Скрыть из чата выдачу предметов Админу")]
            public Boolean HideAdminGave;
            [JsonProperty(LanguageEn ? "Move mute to team chat (In case of a mute, the player will not be able to write even to the team chat)" : "Переносить мут в командный чат(В случае мута, игрок не сможет писать даже в командный чат)")]
            public Boolean MuteTeamChat;
        }

        internal class FormattingMessage
        {
            [JsonProperty(LanguageEn ? "Enable message formatting [Will control caps, message format] (true - yes/false - no)" : "Включить форматирование сообщений [Будет контроллировать капс, формат сообщения] (true - да/false - нет)")]
            public Boolean FormatMessage;
            [JsonProperty(LanguageEn ? "Use a list of banned words (true - yes/false - no)" : "Использовать список запрещенных слов (true - да/false - нет)")]
            public Boolean UseBadWords;
            [JsonProperty(LanguageEn ? "The word that will replace the forbidden word" : "Слово которое будет заменять запрещенное слово")]
            public String ReplaceBadWord;
            [JsonProperty(LanguageEn ? "The list of forbidden words [Forbidden Word] = Whether to replace it in part of the word (for example, Vasya Fucking => Vasya ***) (true - yes/false - no)" : "Список запрещенных слов [ЗапрещенноеСлово] = Заменять ли его в части слова (например ВасяБля => Вася***) (true - да/false - нет)")]
            public Dictionary<String, Boolean> BadWords;
            [JsonProperty(LanguageEn ? "Nickname controller setup" : "Настройка контроллера ников")]
            public NickController ControllerNickname;
            internal class NickController
            {
                [JsonProperty(LanguageEn ? "Enable player nickname formatting (message formatting must be enabled)" : "Включить форматирование ников игроков (должно быть включено форматирование сообщений)")]
                public Boolean UseNickController;
                [JsonProperty(LanguageEn ? "The word that will replace the forbidden word (You can leave it blank and it will just delete)" : "Слово которое будет заменять запрещенное слово (Вы можете оставить пустым и будет просто удалять)")]
                public String ReplaceBadNick;
                [JsonProperty(LanguageEn ? "The list of forbidden nicknames [Forbidden Word] = Whether to replace it in part of the word (for example, Vasya Fucking => Vasya ***) (true - yes/false - no)" : "Список запрещенных ников [ЗапрещенноеСлово] = Заменять ли его в части слова (например ВасяБля => Вася***) (true - да/false - нет)")]
                public Dictionary<String, Boolean> BadNicks;
                [JsonProperty(LanguageEn ? "List of allowed links in nicknames" : "Список разрешенных ссылок в никах")]
                public List<String> AllowedLinkNick;
            }

        }

    }

    [JsonProperty(LanguageEn ? "Setting up chat alerts" : "Настройка оповещений в чате")]
    public ControllerAlert ControllerAlertSetting;
    internal class ControllerAlert
    {
        [JsonProperty(LanguageEn ? "Setting up chat alerts" : "Настройка оповещений в чате")]
        public Alert AlertSetting;
        [JsonProperty(LanguageEn ? "Setting notifications about the status of the player's session" : "Настройка оповещений о статусе сессии игрока")]
        public PlayerSession PlayerSessionSetting;
        [JsonProperty(LanguageEn ? "Configuring administrator session status alerts" : "Настройка оповещений о статусе сессии администратора")]
        public AdminSession AdminSessionSetting;
        [JsonProperty(LanguageEn ? "Setting up personal notifications to the player when connecting" : "Настройка персональных оповоещений игроку при коннекте")]
        public PersonalAlert PersonalAlertSetting;
        internal class Alert
        {
            [JsonProperty(LanguageEn ? "Enable automatic messages in chat (true - yes/false - no)" : "Включить автоматические сообщения в чат (true - да/false - нет)")]
            public Boolean AlertMessage;
            [JsonProperty(LanguageEn ? "Type of automatic messages : true - sequential / false - random" : "Тип автоматических сообщений : true - поочередные/false - случайные")]
            public Boolean AlertMessageType;
            [JsonProperty(LanguageEn ? "List of automatic messages in chat" : "Список автоматических сообщений в чат")]
            public LanguageController MessageList;
            [JsonProperty(LanguageEn ? "Interval for sending messages to chat (Broadcaster) (in seconds)" : "Интервал отправки сообщений в чат (Броадкастер) (в секундах)")]
            public Int32 MessageListTimer;
        }

        internal class PlayerSession
        {
            [JsonProperty(LanguageEn ? "When a player is notified about the entry / exit of the player, display his avatar opposite the nickname (true - yes / false - no)" : "При уведомлении о входе/выходе игрока отображать его аватар напротив ника (true - да/false - нет)")]
            public Boolean ConnectedAvatarUse;
            [JsonProperty(LanguageEn ? "Notify in chat when a player enters (true - yes/false - no)" : "Уведомлять в чате о входе игрока (true - да/false - нет)")]
            public Boolean ConnectedAlert;
            [JsonProperty(LanguageEn ? "Enable random notifications when a player from the list enters (true - yes / false - no)" : "Включить случайные уведомления о входе игрока из списка (true - да/false - нет)")]
            public Boolean ConnectionAlertRandom;
            [JsonProperty(LanguageEn ? "Show the country of the entered player (true - yes/false - no)" : "Отображать страну зашедшего игрока (true - да/false - нет")]
            public Boolean ConnectedWorld;
            [JsonProperty(LanguageEn ? "Notify when a player enters the chat (selected from the list) (true - yes/false - no)" : "Уведомлять о выходе игрока в чат(выбираются из списка) (true - да/false - нет)")]
            public Boolean DisconnectedAlert;
            [JsonProperty(LanguageEn ? "Enable random player exit notifications (true - yes/false - no)" : "Включить случайные уведомления о выходе игрока (true - да/false - нет)")]
            public Boolean DisconnectedAlertRandom;
            [JsonProperty(LanguageEn ? "Display reason for player exit (true - yes/false - no)" : "Отображать причину выхода игрока (true - да/false - нет)")]
            public Boolean DisconnectedReason;
            [JsonProperty(LanguageEn ? "Random player entry notifications({0} - player's nickname, {1} - country (if country display is enabled)" : "Случайные уведомления о входе игрока({0} - ник игрока, {1} - страна(если включено отображение страны)")]
            public LanguageController RandomConnectionAlert;
            [JsonProperty(LanguageEn ? "Random notifications about the exit of the player ({0} - player's nickname, {1} - the reason for the exit (if the reason is enabled)" : "Случайные уведомления о выходе игрока({0} - ник игрока, {1} - причина выхода(если включена причина)")]
            public LanguageController RandomDisconnectedAlert;
        }

        internal class AdminSession
        {
            [JsonProperty(LanguageEn ? "Notify admin on the server in the chat (true - yes/false - no)" : "Уведомлять о входе админа на сервер в чат (true - да/false - нет)")]
            public Boolean ConnectedAlertAdmin;
            [JsonProperty(LanguageEn ? "Notify about admin leaving the server in chat (true - yes/false - no)" : "Уведомлять о выходе админа на сервер в чат (true - да/false - нет)")]
            public Boolean DisconnectedAlertAdmin;
        }

        internal class PersonalAlert
        {
            [JsonProperty(LanguageEn ? "Enable random message to the player who has logged in (true - yes/false - no)" : "Включить случайное сообщение зашедшему игроку (true - да/false - нет)")]
            public Boolean UseWelcomeMessage;
            [JsonProperty(LanguageEn ? "List of messages to the player when entering" : "Список сообщений игроку при входе")]
            public LanguageController WelcomeMessage;
        }

    }

    public class LanguageController
    {
        [JsonProperty(LanguageEn ? "Setting up Multilingual Messages [Language Code] = Translation Variations" : "Настройка мультиязычных сообщений [КодЯзыка] = ВариацииПеревода")]
        public Dictionary<String, List<String>> LanguageMessages;
    }

    [JsonProperty(LanguageEn ? "Settings Rust+" : "Настройка Rust+")]
    public RustPlus RustPlusSettings;
    internal class RustPlus
    {
        [JsonProperty(LanguageEn ? "Use Rust+" : "Использовать Rust+")]
        public Boolean UseRustPlus;
        [JsonProperty(LanguageEn ? "Title for notification Rust+" : "Название для уведомления Rust+")]
        public String DisplayNameAlert;
    }

    [JsonProperty(LanguageEn ? "Configuring support plugins" : "Настройка плагинов поддержки")]
    public ReferenceSettings ReferenceSetting;
    internal class ReferenceSettings
    {
        [JsonProperty(LanguageEn ? "Settings XLevels" : "Настройка XLevels")]
        public XLevels XLevelsSettings;
        [JsonProperty(LanguageEn ? "Settings IQFakeActive" : "Настройка IQFakeActive")]
        public IQFakeActive IQFakeActiveSettings;
        [JsonProperty(LanguageEn ? "Settings IQRankSystem" : "Настройка IQRankSystem")]
        public IQRankSystem IQRankSystems;
        [JsonProperty(LanguageEn ? "Settings Clans" : "Настройка Clans")]
        public Clans ClansSettings;
        [JsonProperty(LanguageEn ? "Settings TranslationAPI" : "Настройка TranslationAPI")]
        public TranslataionApi translationApiSettings;
        [JsonProperty(LanguageEn ? "Settings SkillTree" : "Настройка SkillTree")]
        public SkillTree skillTreeSettings;
        [JsonProperty(LanguageEn ? "Settings PlayerRanks" : "Настройка PlayerRanks")]
        public PlayerRanks playerRanksSettings;
        [JsonProperty(LanguageEn ? "Settings XPrison" : "Настройка XPrison")]
        public XPrison xPrisonSettings;
        internal class TranslataionApi
        {
            [JsonProperty(LanguageEn ? "To use automatic message translation using the TranslationAPI" : "Использовать автоматический перевод сообщений с помощью TranslataionAPI")]
            public Boolean useTranslationApi;
            [JsonProperty(LanguageEn ? "Translate team chat" : "Переводить командный чат")]
            public Boolean translateTeamChat;
            [JsonProperty(LanguageEn ? "Translate chat in private messages." : "Переводить чат в личных сообщениях")]
            public Boolean translatePmChat;
            [JsonProperty(LanguageEn ? "The code for the preferred language (leave it empty, and then the translation will be done in each player's language)" : "Код приоритетного языка (оставьте пустым и тогда для каждого игрока будет переводиться на его языке клиента)")]
            public String codeLanguagePrimary;
        }

        internal class Clans
        {
            [JsonProperty(LanguageEn ? "Display a clan tag in the chat (if Clans are installed)" : "Отображать в чате клановый тэг (если установлены Clans)")]
            public Boolean UseClanTag;
            [JsonProperty(LanguageEn ? "The color of the additional tag" : "Цвет дополнительного тэга")]
            public String colorTag;
        }

        internal class IQRankSystem
        {
            [JsonProperty(LanguageEn ? "Rank display format in chat ( {0} is the user's rank, do not delete this value)" : "Формат отображения ранга в чате ( {0} - это ранг юзера, не удаляйте это значение)")]
            public String FormatRank;
            [JsonProperty(LanguageEn ? "Time display format with IQRank System in chat ( {0} is the user's time, do not delete this value)" : "Формат отображения времени с IQRankSystem в чате ( {0} - это время юзера, не удаляйте это значение)")]
            public String FormatRankTime;
            [JsonProperty(LanguageEn ? "Use support IQRankSystem" : "Использовать поддержку рангов")]
            public Boolean UseRankSystem;
            [JsonProperty(LanguageEn ? "Show players their played time next to their rank" : "Отображать игрокам их отыгранное время рядом с рангом")]
            public Boolean UseTimeStandart;
        }

        internal class IQFakeActive
        {
            [JsonProperty(LanguageEn ? "Use support IQFakeActive" : "Использовать поддержку IQFakeActive")]
            public Boolean UseIQFakeActive;
        }

        internal class XLevels
        {
            [JsonProperty(LanguageEn ? "Use support XLevels" : "Использовать поддержку XLevels")]
            public Boolean UseXLevels;
            [JsonProperty(LanguageEn ? "Use full prefix with level from XLevel (true) otherwise only level (false)" : "Использовать полный префикс с уровнем из XLevel (true) иначе только уровень (false)")]
            public Boolean UseFullXLevels;
            [JsonProperty(LanguageEn ? "The color of the additional tag" : "Цвет дополнительного тэга")]
            public String colorTag;
        }

        internal class XPrison
        {
            [JsonProperty(LanguageEn ? "Use support XPrison" : "Использовать поддержку XPrison")]
            public Boolean UseXPrison;
            [JsonProperty(LanguageEn ? "The color of the additional tag" : "Цвет дополнительного тэга")]
            public String colorTag;
        }

        internal class SkillTree
        {
            [JsonProperty(LanguageEn ? "Use support SkillTree" : "Использовать поддержку SkillTree")]
            public Boolean UseSkillTree;
            [JsonProperty(LanguageEn ? "Use full XP + Level information output (true), use only Level (false)" : "Использовать полный вывод информации XP + Level (true), использовать только Level (false)")]
            public Boolean UseFullSkillTree;
            [JsonProperty(LanguageEn ? "Use prestige" : "Использовать престиж")]
            public Boolean UsePrestigeSkillTree;
            [JsonProperty(LanguageEn ? "The color of the additional tag" : "Цвет дополнительного тэга")]
            public String colorTag;
        }

        internal class PlayerRanks
        {
            [JsonProperty(LanguageEn ? "Use support PlayerRanks" : "Использовать поддержку PlayerRanks")]
            public Boolean UsePlayerRanks;
            [JsonProperty(LanguageEn ? "The color of the additional tag" : "Цвет дополнительного тэга")]
            public String colorTag;
        }

    }

    [JsonProperty(LanguageEn ? "Setting up an answering machine" : "Настройка автоответчика")]
    public AnswerMessage AnswerMessages;
    internal class AnswerMessage
    {
        [JsonProperty(LanguageEn ? "Enable auto-reply? (true - yes/false - no)" : "Включить автоответчик?(true - да/false - нет)")]
        public bool UseAnswer;
        [JsonProperty(LanguageEn ? "Customize Messages [Keyword] = Reply" : "Настройка сообщений [Ключевое слово] = Ответ")]
        public Dictionary<String, LanguageController> AnswerMessageList;
    }

    [JsonProperty(LanguageEn ? "Disable additional chat duplication in RCON" : "Отключить дополнительное дублированиеи чата в RCON")]
    public Boolean disableRconBroadcast;
    [JsonProperty(LanguageEn ? "Additional setting" : "Дополнительная настройка")]
    public OtherSettings OtherSetting;
    internal class OtherSettings
    {
        [JsonProperty("SteamApiKey (https://steamcommunity.com/dev/apikey)")]
        public String renameSteamApiKey;
        [JsonProperty(LanguageEn ? "Enable the /online command (true - yes / false - no)" : "Включить команду /online (true - да/ false - нет)")]
        public Boolean UseCommandOnline;
        [JsonProperty(LanguageEn ? "Use shortened format /online (will only display quantity)" : "Использовать сокращенный формат /online (будет отображать только количество)")]
        public Boolean UseCommandShortOnline;
        [JsonProperty(LanguageEn ? "Compact logging of messages" : "Компактное логирование сообщений")]
        public CompactLoggetChat CompactLogsChat;
        [JsonProperty(LanguageEn ? "Setting up message logging" : "Настройка логирования сообщений")]
        public LoggedChat LogsChat;
        [JsonProperty(LanguageEn ? "Setting up logging of personal messages of players" : "Настройка логирования личных сообщений игроков")]
        public General LogsPMChat;
        [JsonProperty(LanguageEn ? "Setting up chat/voice lock/unlock logging" : "Настройка логирования блокировок/разблокировок чата/голоса")]
        public General LogsMuted;
        [JsonProperty(LanguageEn ? "Setting up logging of chat commands from players" : "Настройка логирования чат-команд от игроков")]
        public General LogsChatCommands;
        internal class CompactLoggetChat
        {
            [JsonProperty(LanguageEn ? "Display Steam64ID in the log (true - yes/false - no)" : "Отображать в логе Steam64ID (true - да/false - нет)")]
            public Boolean ShowSteamID;
            [JsonProperty(LanguageEn ? "Setting up compact message logging" : "Настройка компактного логирования сообщений")]
            public LoggedChat LogsCompactChat;
        }

        internal class LoggedChat
        {
            [JsonProperty(LanguageEn ? "Setting up general chat logging" : "Настройка логирования общего чата")]
            public General GlobalChatSettings;
            [JsonProperty(LanguageEn ? "Setting up team chat logging" : "Настройка логирования тим чата")]
            public General TeamChatSettings;
        }

        internal class General
        {
            [JsonProperty(LanguageEn ? "Enable logging (true - yes/false - no)" : "Включить логирование (true - да/false - нет)")]
            public Boolean UseLogged;
            [JsonProperty(LanguageEn ? "Webhooks channel for logging" : "Webhooks канала для логирования")]
            public String Webhooks;
        }

    }

    public static Configuration GetNewConfiguration();
}

internal class ControllerConnection
{
    [JsonProperty(LanguageEn ? "Function switches" : "Перключатели функций")]
    public Turned Turneds;
    [JsonProperty(LanguageEn ? "Setting Standard Values" : "Настройка стандартных значений")]
    public SetupDefault SetupDefaults;
    internal class SetupDefault
    {
        [JsonProperty(LanguageEn ? "This prefix will be set if the player entered the server for the first time or in case of expiration of the rights to the prefix that he had earlier" : "Данный префикс установится если игрок впервые зашел на сервер или в случае окончания прав на префикс, который у него стоял ранее")]
        public String PrefixDefault;
        [JsonProperty(LanguageEn ? "This nickname color will be set if the player entered the server for the first time or in case of expiration of the rights to the nickname color that he had earlier" : "Данный цвет ника установится если игрок впервые зашел на сервер или в случае окончания прав на цвет ника, который у него стоял ранее")]
        public String NickDefault;
        [JsonProperty(LanguageEn ? "This chat color will be set if the player entered the server for the first time or in case of expiration of the rights to the chat color that he had earlier" : "Данный цвет чата установится если игрок впервые зашел на сервер или в случае окончания прав на цвет чата, который у него стоял ранее")]
        public String MessageDefault;
    }

    internal class Turned
    {
        [JsonProperty(LanguageEn ? "Set automatically a prefix to a player when he got the rights to it" : "Устанавливать автоматически префикс игроку, когда он получил права на него")]
        public Boolean TurnAutoSetupPrefix;
        [JsonProperty(LanguageEn ? "Set automatically the color of the nickname to the player when he got the rights to it" : "Устанавливать автоматически цвет ника игроку, когда он получил права на него")]
        public Boolean TurnAutoSetupColorNick;
        [JsonProperty(LanguageEn ? "Set the chat color automatically to the player when he got the rights to it" : "Устанавливать автоматически цвет чата игроку, когда он получил права на него")]
        public Boolean TurnAutoSetupColorChat;
        [JsonProperty(LanguageEn ? "Automatically reset the prefix when the player's rights to it expire" : "Сбрасывать автоматически префикс при окончании прав на него у игрока")]
        public Boolean TurnAutoDropPrefix;
        [JsonProperty(LanguageEn ? "Automatically reset the color of the nickname when the player's rights to it expire" : "Сбрасывать автоматически цвет ника при окончании прав на него у игрока")]
        public Boolean TurnAutoDropColorNick;
        [JsonProperty(LanguageEn ? "Automatically reset the color of the chat when the rights to it from the player expire" : "Сбрасывать автоматически цвет чата при окончании прав на него у игрока")]
        public Boolean TurnAutoDropColorChat;
    }

}

internal class SetupDefault
{
    [JsonProperty(LanguageEn ? "This prefix will be set if the player entered the server for the first time or in case of expiration of the rights to the prefix that he had earlier" : "Данный префикс установится если игрок впервые зашел на сервер или в случае окончания прав на префикс, который у него стоял ранее")]
    public String PrefixDefault;
    [JsonProperty(LanguageEn ? "This nickname color will be set if the player entered the server for the first time or in case of expiration of the rights to the nickname color that he had earlier" : "Данный цвет ника установится если игрок впервые зашел на сервер или в случае окончания прав на цвет ника, который у него стоял ранее")]
    public String NickDefault;
    [JsonProperty(LanguageEn ? "This chat color will be set if the player entered the server for the first time or in case of expiration of the rights to the chat color that he had earlier" : "Данный цвет чата установится если игрок впервые зашел на сервер или в случае окончания прав на цвет чата, который у него стоял ранее")]
    public String MessageDefault;
}

internal class Turned
{
    [JsonProperty(LanguageEn ? "Set automatically a prefix to a player when he got the rights to it" : "Устанавливать автоматически префикс игроку, когда он получил права на него")]
    public Boolean TurnAutoSetupPrefix;
    [JsonProperty(LanguageEn ? "Set automatically the color of the nickname to the player when he got the rights to it" : "Устанавливать автоматически цвет ника игроку, когда он получил права на него")]
    public Boolean TurnAutoSetupColorNick;
    [JsonProperty(LanguageEn ? "Set the chat color automatically to the player when he got the rights to it" : "Устанавливать автоматически цвет чата игроку, когда он получил права на него")]
    public Boolean TurnAutoSetupColorChat;
    [JsonProperty(LanguageEn ? "Automatically reset the prefix when the player's rights to it expire" : "Сбрасывать автоматически префикс при окончании прав на него у игрока")]
    public Boolean TurnAutoDropPrefix;
    [JsonProperty(LanguageEn ? "Automatically reset the color of the nickname when the player's rights to it expire" : "Сбрасывать автоматически цвет ника при окончании прав на него у игрока")]
    public Boolean TurnAutoDropColorNick;
    [JsonProperty(LanguageEn ? "Automatically reset the color of the chat when the rights to it from the player expire" : "Сбрасывать автоматически цвет чата при окончании прав на него у игрока")]
    public Boolean TurnAutoDropColorChat;
}

internal class ControllerParameters
{
    [JsonProperty(LanguageEn ? "Setting the display of options for player selection" : "Настройка отображения параметров для выбора игрока")]
    public VisualSettingParametres VisualParametres;
    [JsonProperty(LanguageEn ? "List and customization of colors for a nickname" : "Список и настройка цветов для ника")]
    public List<AdvancedFuncion> NickColorList;
    [JsonProperty(LanguageEn ? "List and customize colors for chat messages" : "Список и настройка цветов для сообщений в чате")]
    public List<AdvancedFuncion> MessageColorList;
    [JsonProperty(LanguageEn ? "List and configuration of prefixes in chat" : "Список и настройка префиксов в чате")]
    public PrefixSetting Prefixes;
    internal class PrefixSetting
    {
        [JsonProperty(LanguageEn ? "Enable support for multiple prefixes at once (true - multiple prefixes can be set/false - only 1 can be set to choose from)" : "Включить поддержку нескольких префиксов сразу (true - можно установить несколько префиксов/false - установить можно только 1 на выбор)")]
        public Boolean TurnMultiPrefixes;
        [JsonProperty(LanguageEn ? "The maximum number of prefixes that can be set at a time (This option only works if setting multiple prefixes is enabled)" : "Максимальное количество префиксов, которое можно установить за раз(Данный параметр работает только если включена установка нескольких префиксов)")]
        public Int32 MaximumMultiPrefixCount;
        [JsonProperty(LanguageEn ? "List of prefixes and their settings" : "Список префиксов и их настройка")]
        public List<AdvancedFuncion> Prefixes;
    }

    internal class AdvancedFuncion
    {
        [JsonProperty(LanguageEn ? "Permission" : "Права")]
        public String Permissions;
        [JsonProperty(LanguageEn ? "Argument" : "Значение")]
        public String Argument;
        [JsonProperty(LanguageEn ? "Block the player's ability to select this parameter in the plugin menu (true - yes/false - no)" : "Заблокировать возможность выбрать данный параметр игроком в меню плагина (true - да/false - нет)")]
        public Boolean IsBlockSelected;
    }

    internal class VisualSettingParametres
    {
        [JsonProperty(LanguageEn ? "Player prefix selection display type - (0 - dropdown list, 1 - slider (Please note that if you have multi-prefix enabled, the dropdown list will be set))" : "Тип отображения выбора префикса для игрока - (0 - выпадающий список, 1 - слайдер (Учтите, что если у вас включен мульти-префикс, будет установлен выпадающий список))")]
        public SelectedParametres PrefixType;
        [JsonProperty(LanguageEn ? "Display type of player's nickname color selection - (0 - drop-down list, 1 - slider)" : "Тип отображения выбора цвета ника для игрока - (0 - выпадающий список, 1 - слайдер)")]
        public SelectedParametres NickColorType;
        [JsonProperty(LanguageEn ? "Display type of message color choice for the player - (0 - drop-down list, 1 - slider)" : "Тип отображения выбора цвета сообщения для игрока - (0 - выпадающий список, 1 - слайдер)")]
        public SelectedParametres ChatColorType;
        [JsonProperty(LanguageEn ? "IQRankSystem : Player rank selection display type - (0 - drop-down list, 1 - slider)" : "IQRankSystem : Тип отображения выбора ранга для игрока - (0 - выпадающий список, 1 - слайдер)")]
        public SelectedParametres IQRankSystemType;
    }

}

internal class PrefixSetting
{
    [JsonProperty(LanguageEn ? "Enable support for multiple prefixes at once (true - multiple prefixes can be set/false - only 1 can be set to choose from)" : "Включить поддержку нескольких префиксов сразу (true - можно установить несколько префиксов/false - установить можно только 1 на выбор)")]
    public Boolean TurnMultiPrefixes;
    [JsonProperty(LanguageEn ? "The maximum number of prefixes that can be set at a time (This option only works if setting multiple prefixes is enabled)" : "Максимальное количество префиксов, которое можно установить за раз(Данный параметр работает только если включена установка нескольких префиксов)")]
    public Int32 MaximumMultiPrefixCount;
    [JsonProperty(LanguageEn ? "List of prefixes and their settings" : "Список префиксов и их настройка")]
    public List<AdvancedFuncion> Prefixes;
}

internal class AdvancedFuncion
{
    [JsonProperty(LanguageEn ? "Permission" : "Права")]
    public String Permissions;
    [JsonProperty(LanguageEn ? "Argument" : "Значение")]
    public String Argument;
    [JsonProperty(LanguageEn ? "Block the player's ability to select this parameter in the plugin menu (true - yes/false - no)" : "Заблокировать возможность выбрать данный параметр игроком в меню плагина (true - да/false - нет)")]
    public Boolean IsBlockSelected;
}

internal class VisualSettingParametres
{
    [JsonProperty(LanguageEn ? "Player prefix selection display type - (0 - dropdown list, 1 - slider (Please note that if you have multi-prefix enabled, the dropdown list will be set))" : "Тип отображения выбора префикса для игрока - (0 - выпадающий список, 1 - слайдер (Учтите, что если у вас включен мульти-префикс, будет установлен выпадающий список))")]
    public SelectedParametres PrefixType;
    [JsonProperty(LanguageEn ? "Display type of player's nickname color selection - (0 - drop-down list, 1 - slider)" : "Тип отображения выбора цвета ника для игрока - (0 - выпадающий список, 1 - слайдер)")]
    public SelectedParametres NickColorType;
    [JsonProperty(LanguageEn ? "Display type of message color choice for the player - (0 - drop-down list, 1 - slider)" : "Тип отображения выбора цвета сообщения для игрока - (0 - выпадающий список, 1 - слайдер)")]
    public SelectedParametres ChatColorType;
    [JsonProperty(LanguageEn ? "IQRankSystem : Player rank selection display type - (0 - drop-down list, 1 - slider)" : "IQRankSystem : Тип отображения выбора ранга для игрока - (0 - выпадающий список, 1 - слайдер)")]
    public SelectedParametres IQRankSystemType;
}

internal class ControllerMute
{
    [JsonProperty(LanguageEn ? "Prohibit sending messages in /pm and /r if the player's chat is blocked" : "Запрещать отправлять сообщения в /pm, /r - если у игрока заблокирован чат")]
    public Boolean mutedPM;
    [JsonProperty(LanguageEn ? "Setting up automatic muting" : "Настройка автоматического мута")]
    public AutoMute AutoMuteSettings;
    internal class AutoMute
    {
        [JsonProperty(LanguageEn ? "Enable automatic muting for forbidden words (true - yes/false - no)" : "Включить автоматический мут по запрещенным словам(true - да/false - нет)")]
        public Boolean UseAutoMute;
        [JsonProperty(LanguageEn ? "Reason for automatic muting" : "Причина автоматического мута")]
        public Muted AutoMuted;
    }

    [JsonProperty(LanguageEn ? "Additional setting for logging about mutes in discord" : "Дополнительная настройка для логирования о мутах в дискорд")]
    public LoggedFuncion LoggedMute;
    internal class LoggedFuncion
    {
        [JsonProperty(LanguageEn ? "Support for logging the last N messages (Discord logging about mutes must be enabled)" : "Поддержка логирования последних N сообщений (Должно быть включено логирование в дискорд о мутах)")]
        public Boolean UseHistoryMessage;
        [JsonProperty(LanguageEn ? "How many latest player messages to send in logging" : "Сколько последних сообщений игрока отправлять в логировании")]
        public Int32 CountHistoryMessage;
    }

    [JsonProperty(LanguageEn ? "Reasons to block chat" : "Причины для блокировки чата")]
    public List<Muted> MuteChatReasons;
    [JsonProperty(LanguageEn ? "Reasons to block your voice" : "Причины для блокировки голоса")]
    public List<Muted> MuteVoiceReasons;
    internal class Muted
    {
        [JsonProperty(LanguageEn ? "Reason for blocking" : "Причина для блокировки")]
        public String Reason;
        [JsonProperty(LanguageEn ? "Block time (in seconds)" : "Время блокировки(в секундах)")]
        public Int32 SecondMute;
    }

}

internal class AutoMute
{
    [JsonProperty(LanguageEn ? "Enable automatic muting for forbidden words (true - yes/false - no)" : "Включить автоматический мут по запрещенным словам(true - да/false - нет)")]
    public Boolean UseAutoMute;
    [JsonProperty(LanguageEn ? "Reason for automatic muting" : "Причина автоматического мута")]
    public Muted AutoMuted;
}

internal class LoggedFuncion
{
    [JsonProperty(LanguageEn ? "Support for logging the last N messages (Discord logging about mutes must be enabled)" : "Поддержка логирования последних N сообщений (Должно быть включено логирование в дискорд о мутах)")]
    public Boolean UseHistoryMessage;
    [JsonProperty(LanguageEn ? "How many latest player messages to send in logging" : "Сколько последних сообщений игрока отправлять в логировании")]
    public Int32 CountHistoryMessage;
}

internal class Muted
{
    [JsonProperty(LanguageEn ? "Reason for blocking" : "Причина для блокировки")]
    public String Reason;
    [JsonProperty(LanguageEn ? "Block time (in seconds)" : "Время блокировки(в секундах)")]
    public Int32 SecondMute;
}

internal class ControllerMessage
{
    [JsonProperty(LanguageEn ? "Basic settings for chat messages from the plugin" : "Основная настройка сообщений в чат от плагина")]
    public GeneralSettings GeneralSetting;
    [JsonProperty(LanguageEn ? "Configuring functionality switching in chat" : "Настройка переключения функционала в чате")]
    public TurnedFuncional TurnedFunc;
    [JsonProperty(LanguageEn ? "Player message formatting settings" : "Настройка форматирования сообщений игроков")]
    public FormattingMessage Formatting;
    internal class GeneralSettings
    {
        [JsonProperty(LanguageEn ? "Notify the player in chat about receiving a prefix/nickname color/chat color (true - yes/false - no)" : "Уведомлять игрока в чате о получении префикса/цвета ника/цвета чата (true - да/false - нет)")]
        public Boolean alertArgumentsInfoSetup;
        [JsonProperty(LanguageEn ? "Notify the player in chat about the end of a prefix/nickname color/chat color (true - yes/false - no)" : "Уведомлять игрока в чате о окончании префикса/цвета ника/цвета чата (true - да/false - нет)")]
        public Boolean alertArgumentsInfoRemove;
        [JsonProperty(LanguageEn ? "Customizing the chat alert format" : "Настройка формата оповещения в чате")]
        public BroadcastSettings BroadcastFormat;
        [JsonProperty(LanguageEn ? "Setting the mention format in the chat, via @" : "Настройка формата упоминания в чате, через @")]
        public AlertSettings AlertFormat;
        [JsonProperty(LanguageEn ? "Additional setting" : "Дополнительная настройка")]
        public OtherSettings OtherSetting;
        internal class BroadcastSettings
        {
            [JsonProperty(LanguageEn ? "The name of the notification in the chat" : "Наименование оповещения в чат")]
            public String BroadcastTitle;
            [JsonProperty(LanguageEn ? "Chat alert message color" : "Цвет сообщения оповещения в чат")]
            public String BroadcastColor;
            [JsonProperty(LanguageEn ? "Steam64ID for chat avatar" : "Steam64ID для аватарки в чате")]
            public String Steam64IDAvatar;
        }

        internal class AlertSettings
        {
            [JsonProperty(LanguageEn ? "The color of the player mention message in the chat" : "Цвет сообщения упоминания игрока в чате")]
            public String AlertPlayerColor;
            [JsonProperty(LanguageEn ? "Sound when receiving and sending a mention via @" : "Звук при при получении и отправки упоминания через @")]
            public String SoundAlertPlayer;
        }

        internal class OtherSettings
        {
            [JsonProperty(LanguageEn ? "Time after which the message will be deleted from the UI from the administrator" : "Время,через которое удалится сообщение с UI от администратора")]
            public Int32 TimeDeleteAlertUI;
            [JsonProperty(LanguageEn ? "The size of the message from the player in the chat" : "Размер сообщения от игрока в чате")]
            public Int32 SizeMessage;
            [JsonProperty(LanguageEn ? "Player nickname size in chat" : "Размер ника игрока в чате")]
            public Int32 SizeNick;
            [JsonProperty(LanguageEn ? "The size of the player's prefix in the chat (will be used if <size=N></size> is not set in the prefix itself)" : "Размер префикса игрока в чате (будет использовано, если в самом префиксе не установвлен <size=N></size>)")]
            public Int32 SizePrefix;
            [JsonProperty(LanguageEn ? "Nickname size according to privilege [permission] = size" : "Размер ника по привилегии [permission] = размер")]
            public Dictionary<String, Int32> sizeNickPrivilages;
            [JsonProperty(LanguageEn ? "Chat message size according to privilege [permission] = size" : "Размер сообщения в чате по привилегии [permission] = размер")]
            public Dictionary<String, Int32> sizeMessagePrivilages;
            public Int32 GetSizeNickOrMessage(BasePlayer player, Boolean nickOrMessage);
        }

    }

    internal class TurnedFuncional
    {
        [JsonProperty(LanguageEn ? "Configuring spam protection" : "Настройка защиты от спама")]
        public AntiSpam AntiSpamSetting;
        [JsonProperty(LanguageEn ? "Setting up a temporary chat block for newbies (who have just logged into the server)" : "Настройка временной блокировки чата новичкам (которые только зашли на сервер)")]
        public AntiNoob AntiNoobSetting;
        [JsonProperty(LanguageEn ? "Setting up private messages" : "Настройка личных сообщений")]
        public PM PMSetting;
        internal class AntiNoob
        {
            [JsonProperty(LanguageEn ? "Newbie protection in PM/R" : "Защита от новичка в PM/R")]
            public Settings AntiNoobPM;
            [JsonProperty(LanguageEn ? "Newbie protection in global and team chat" : "Защита от новичка в глобальном и коммандном чате")]
            public Settings AntiNoobChat;
            internal class Settings
            {
                [JsonProperty(LanguageEn ? "Enable protection?" : "Включить защиту?")]
                public Boolean AntiNoobActivate;
                [JsonProperty(LanguageEn ? "Newbie Chat Lock Time" : "Время блокировки чата для новичка")]
                public Int32 TimeBlocked;
            }

        }

        internal class AntiSpam
        {
            [JsonProperty(LanguageEn ? "Enable spam protection (Anti-spam)" : "Включить защиту от спама (Анти-спам)")]
            public Boolean AntiSpamActivate;
            [JsonProperty(LanguageEn ? "Time after which a player can send a message (AntiSpam)" : "Время через которое игрок может отправлять сообщение (АнтиСпам)")]
            public Int32 FloodTime;
            [JsonProperty(LanguageEn ? "Additional Anti-Spam settings" : "Дополнительная настройка Анти-Спама")]
            public AntiSpamDuples AntiSpamDuplesSetting;
            internal class AntiSpamDuples
            {
                [JsonProperty(LanguageEn ? "Enable additional spam protection (Anti-duplicates, duplicate messages)" : "Включить дополнительную защиту от спама (Анти-дубликаты, повторяющие сообщения)")]
                public Boolean AntiSpamDuplesActivate;
                [JsonProperty(LanguageEn ? "How many duplicate messages does a player need to make to be confused by the system" : "Сколько дублирующих сообщений нужно сделать игроку чтобы его замутила система")]
                public Int32 TryDuples;
                [JsonProperty(LanguageEn ? "Setting up automatic muting for duplicates" : "Настройка автоматического мута за дубликаты")]
                public ControllerMute.Muted MuteSetting;
            }

        }

        internal class PM
        {
            [JsonProperty(LanguageEn ? "Enable Private Messages" : "Включить личные сообщения")]
            public Boolean PMActivate;
            [JsonProperty(LanguageEn ? "Sound when receiving a private message" : "Звук при при получении личного сообщения")]
            public String SoundPM;
        }

        [JsonProperty(LanguageEn ? "Enable PM ignore for players (/ignore nick or via interface)" : "Включить игнор ЛС игрокам(/ignore nick или через интерфейс)")]
        public Boolean IgnoreUsePM;
        [JsonProperty(LanguageEn ? "Hide the issue of items to the Admin from the chat" : "Скрыть из чата выдачу предметов Админу")]
        public Boolean HideAdminGave;
        [JsonProperty(LanguageEn ? "Move mute to team chat (In case of a mute, the player will not be able to write even to the team chat)" : "Переносить мут в командный чат(В случае мута, игрок не сможет писать даже в командный чат)")]
        public Boolean MuteTeamChat;
    }

    internal class FormattingMessage
    {
        [JsonProperty(LanguageEn ? "Enable message formatting [Will control caps, message format] (true - yes/false - no)" : "Включить форматирование сообщений [Будет контроллировать капс, формат сообщения] (true - да/false - нет)")]
        public Boolean FormatMessage;
        [JsonProperty(LanguageEn ? "Use a list of banned words (true - yes/false - no)" : "Использовать список запрещенных слов (true - да/false - нет)")]
        public Boolean UseBadWords;
        [JsonProperty(LanguageEn ? "The word that will replace the forbidden word" : "Слово которое будет заменять запрещенное слово")]
        public String ReplaceBadWord;
        [JsonProperty(LanguageEn ? "The list of forbidden words [Forbidden Word] = Whether to replace it in part of the word (for example, Vasya Fucking => Vasya ***) (true - yes/false - no)" : "Список запрещенных слов [ЗапрещенноеСлово] = Заменять ли его в части слова (например ВасяБля => Вася***) (true - да/false - нет)")]
        public Dictionary<String, Boolean> BadWords;
        [JsonProperty(LanguageEn ? "Nickname controller setup" : "Настройка контроллера ников")]
        public NickController ControllerNickname;
        internal class NickController
        {
            [JsonProperty(LanguageEn ? "Enable player nickname formatting (message formatting must be enabled)" : "Включить форматирование ников игроков (должно быть включено форматирование сообщений)")]
            public Boolean UseNickController;
            [JsonProperty(LanguageEn ? "The word that will replace the forbidden word (You can leave it blank and it will just delete)" : "Слово которое будет заменять запрещенное слово (Вы можете оставить пустым и будет просто удалять)")]
            public String ReplaceBadNick;
            [JsonProperty(LanguageEn ? "The list of forbidden nicknames [Forbidden Word] = Whether to replace it in part of the word (for example, Vasya Fucking => Vasya ***) (true - yes/false - no)" : "Список запрещенных ников [ЗапрещенноеСлово] = Заменять ли его в части слова (например ВасяБля => Вася***) (true - да/false - нет)")]
            public Dictionary<String, Boolean> BadNicks;
            [JsonProperty(LanguageEn ? "List of allowed links in nicknames" : "Список разрешенных ссылок в никах")]
            public List<String> AllowedLinkNick;
        }

    }

}

internal class GeneralSettings
{
    [JsonProperty(LanguageEn ? "Notify the player in chat about receiving a prefix/nickname color/chat color (true - yes/false - no)" : "Уведомлять игрока в чате о получении префикса/цвета ника/цвета чата (true - да/false - нет)")]
    public Boolean alertArgumentsInfoSetup;
    [JsonProperty(LanguageEn ? "Notify the player in chat about the end of a prefix/nickname color/chat color (true - yes/false - no)" : "Уведомлять игрока в чате о окончании префикса/цвета ника/цвета чата (true - да/false - нет)")]
    public Boolean alertArgumentsInfoRemove;
    [JsonProperty(LanguageEn ? "Customizing the chat alert format" : "Настройка формата оповещения в чате")]
    public BroadcastSettings BroadcastFormat;
    [JsonProperty(LanguageEn ? "Setting the mention format in the chat, via @" : "Настройка формата упоминания в чате, через @")]
    public AlertSettings AlertFormat;
    [JsonProperty(LanguageEn ? "Additional setting" : "Дополнительная настройка")]
    public OtherSettings OtherSetting;
    internal class BroadcastSettings
    {
        [JsonProperty(LanguageEn ? "The name of the notification in the chat" : "Наименование оповещения в чат")]
        public String BroadcastTitle;
        [JsonProperty(LanguageEn ? "Chat alert message color" : "Цвет сообщения оповещения в чат")]
        public String BroadcastColor;
        [JsonProperty(LanguageEn ? "Steam64ID for chat avatar" : "Steam64ID для аватарки в чате")]
        public String Steam64IDAvatar;
    }

    internal class AlertSettings
    {
        [JsonProperty(LanguageEn ? "The color of the player mention message in the chat" : "Цвет сообщения упоминания игрока в чате")]
        public String AlertPlayerColor;
        [JsonProperty(LanguageEn ? "Sound when receiving and sending a mention via @" : "Звук при при получении и отправки упоминания через @")]
        public String SoundAlertPlayer;
    }

    internal class OtherSettings
    {
        [JsonProperty(LanguageEn ? "Time after which the message will be deleted from the UI from the administrator" : "Время,через которое удалится сообщение с UI от администратора")]
        public Int32 TimeDeleteAlertUI;
        [JsonProperty(LanguageEn ? "The size of the message from the player in the chat" : "Размер сообщения от игрока в чате")]
        public Int32 SizeMessage;
        [JsonProperty(LanguageEn ? "Player nickname size in chat" : "Размер ника игрока в чате")]
        public Int32 SizeNick;
        [JsonProperty(LanguageEn ? "The size of the player's prefix in the chat (will be used if <size=N></size> is not set in the prefix itself)" : "Размер префикса игрока в чате (будет использовано, если в самом префиксе не установвлен <size=N></size>)")]
        public Int32 SizePrefix;
        [JsonProperty(LanguageEn ? "Nickname size according to privilege [permission] = size" : "Размер ника по привилегии [permission] = размер")]
        public Dictionary<String, Int32> sizeNickPrivilages;
        [JsonProperty(LanguageEn ? "Chat message size according to privilege [permission] = size" : "Размер сообщения в чате по привилегии [permission] = размер")]
        public Dictionary<String, Int32> sizeMessagePrivilages;
        public Int32 GetSizeNickOrMessage(BasePlayer player, Boolean nickOrMessage);
    }

}

internal class BroadcastSettings
{
    [JsonProperty(LanguageEn ? "The name of the notification in the chat" : "Наименование оповещения в чат")]
    public String BroadcastTitle;
    [JsonProperty(LanguageEn ? "Chat alert message color" : "Цвет сообщения оповещения в чат")]
    public String BroadcastColor;
    [JsonProperty(LanguageEn ? "Steam64ID for chat avatar" : "Steam64ID для аватарки в чате")]
    public String Steam64IDAvatar;
}

internal class AlertSettings
{
    [JsonProperty(LanguageEn ? "The color of the player mention message in the chat" : "Цвет сообщения упоминания игрока в чате")]
    public String AlertPlayerColor;
    [JsonProperty(LanguageEn ? "Sound when receiving and sending a mention via @" : "Звук при при получении и отправки упоминания через @")]
    public String SoundAlertPlayer;
}

internal class OtherSettings
{
    [JsonProperty(LanguageEn ? "Time after which the message will be deleted from the UI from the administrator" : "Время,через которое удалится сообщение с UI от администратора")]
    public Int32 TimeDeleteAlertUI;
    [JsonProperty(LanguageEn ? "The size of the message from the player in the chat" : "Размер сообщения от игрока в чате")]
    public Int32 SizeMessage;
    [JsonProperty(LanguageEn ? "Player nickname size in chat" : "Размер ника игрока в чате")]
    public Int32 SizeNick;
    [JsonProperty(LanguageEn ? "The size of the player's prefix in the chat (will be used if <size=N></size> is not set in the prefix itself)" : "Размер префикса игрока в чате (будет использовано, если в самом префиксе не установвлен <size=N></size>)")]
    public Int32 SizePrefix;
    [JsonProperty(LanguageEn ? "Nickname size according to privilege [permission] = size" : "Размер ника по привилегии [permission] = размер")]
    public Dictionary<String, Int32> sizeNickPrivilages;
    [JsonProperty(LanguageEn ? "Chat message size according to privilege [permission] = size" : "Размер сообщения в чате по привилегии [permission] = размер")]
    public Dictionary<String, Int32> sizeMessagePrivilages;
    public Int32 GetSizeNickOrMessage(BasePlayer player, Boolean nickOrMessage);
}

internal class TurnedFuncional
{
    [JsonProperty(LanguageEn ? "Configuring spam protection" : "Настройка защиты от спама")]
    public AntiSpam AntiSpamSetting;
    [JsonProperty(LanguageEn ? "Setting up a temporary chat block for newbies (who have just logged into the server)" : "Настройка временной блокировки чата новичкам (которые только зашли на сервер)")]
    public AntiNoob AntiNoobSetting;
    [JsonProperty(LanguageEn ? "Setting up private messages" : "Настройка личных сообщений")]
    public PM PMSetting;
    internal class AntiNoob
    {
        [JsonProperty(LanguageEn ? "Newbie protection in PM/R" : "Защита от новичка в PM/R")]
        public Settings AntiNoobPM;
        [JsonProperty(LanguageEn ? "Newbie protection in global and team chat" : "Защита от новичка в глобальном и коммандном чате")]
        public Settings AntiNoobChat;
        internal class Settings
        {
            [JsonProperty(LanguageEn ? "Enable protection?" : "Включить защиту?")]
            public Boolean AntiNoobActivate;
            [JsonProperty(LanguageEn ? "Newbie Chat Lock Time" : "Время блокировки чата для новичка")]
            public Int32 TimeBlocked;
        }

    }

    internal class AntiSpam
    {
        [JsonProperty(LanguageEn ? "Enable spam protection (Anti-spam)" : "Включить защиту от спама (Анти-спам)")]
        public Boolean AntiSpamActivate;
        [JsonProperty(LanguageEn ? "Time after which a player can send a message (AntiSpam)" : "Время через которое игрок может отправлять сообщение (АнтиСпам)")]
        public Int32 FloodTime;
        [JsonProperty(LanguageEn ? "Additional Anti-Spam settings" : "Дополнительная настройка Анти-Спама")]
        public AntiSpamDuples AntiSpamDuplesSetting;
        internal class AntiSpamDuples
        {
            [JsonProperty(LanguageEn ? "Enable additional spam protection (Anti-duplicates, duplicate messages)" : "Включить дополнительную защиту от спама (Анти-дубликаты, повторяющие сообщения)")]
            public Boolean AntiSpamDuplesActivate;
            [JsonProperty(LanguageEn ? "How many duplicate messages does a player need to make to be confused by the system" : "Сколько дублирующих сообщений нужно сделать игроку чтобы его замутила система")]
            public Int32 TryDuples;
            [JsonProperty(LanguageEn ? "Setting up automatic muting for duplicates" : "Настройка автоматического мута за дубликаты")]
            public ControllerMute.Muted MuteSetting;
        }

    }

    internal class PM
    {
        [JsonProperty(LanguageEn ? "Enable Private Messages" : "Включить личные сообщения")]
        public Boolean PMActivate;
        [JsonProperty(LanguageEn ? "Sound when receiving a private message" : "Звук при при получении личного сообщения")]
        public String SoundPM;
    }

    [JsonProperty(LanguageEn ? "Enable PM ignore for players (/ignore nick or via interface)" : "Включить игнор ЛС игрокам(/ignore nick или через интерфейс)")]
    public Boolean IgnoreUsePM;
    [JsonProperty(LanguageEn ? "Hide the issue of items to the Admin from the chat" : "Скрыть из чата выдачу предметов Админу")]
    public Boolean HideAdminGave;
    [JsonProperty(LanguageEn ? "Move mute to team chat (In case of a mute, the player will not be able to write even to the team chat)" : "Переносить мут в командный чат(В случае мута, игрок не сможет писать даже в командный чат)")]
    public Boolean MuteTeamChat;
}

internal class AntiNoob
{
    [JsonProperty(LanguageEn ? "Newbie protection in PM/R" : "Защита от новичка в PM/R")]
    public Settings AntiNoobPM;
    [JsonProperty(LanguageEn ? "Newbie protection in global and team chat" : "Защита от новичка в глобальном и коммандном чате")]
    public Settings AntiNoobChat;
    internal class Settings
    {
        [JsonProperty(LanguageEn ? "Enable protection?" : "Включить защиту?")]
        public Boolean AntiNoobActivate;
        [JsonProperty(LanguageEn ? "Newbie Chat Lock Time" : "Время блокировки чата для новичка")]
        public Int32 TimeBlocked;
    }

}

internal class Settings
{
    [JsonProperty(LanguageEn ? "Enable protection?" : "Включить защиту?")]
    public Boolean AntiNoobActivate;
    [JsonProperty(LanguageEn ? "Newbie Chat Lock Time" : "Время блокировки чата для новичка")]
    public Int32 TimeBlocked;
}

internal class AntiSpam
{
    [JsonProperty(LanguageEn ? "Enable spam protection (Anti-spam)" : "Включить защиту от спама (Анти-спам)")]
    public Boolean AntiSpamActivate;
    [JsonProperty(LanguageEn ? "Time after which a player can send a message (AntiSpam)" : "Время через которое игрок может отправлять сообщение (АнтиСпам)")]
    public Int32 FloodTime;
    [JsonProperty(LanguageEn ? "Additional Anti-Spam settings" : "Дополнительная настройка Анти-Спама")]
    public AntiSpamDuples AntiSpamDuplesSetting;
    internal class AntiSpamDuples
    {
        [JsonProperty(LanguageEn ? "Enable additional spam protection (Anti-duplicates, duplicate messages)" : "Включить дополнительную защиту от спама (Анти-дубликаты, повторяющие сообщения)")]
        public Boolean AntiSpamDuplesActivate;
        [JsonProperty(LanguageEn ? "How many duplicate messages does a player need to make to be confused by the system" : "Сколько дублирующих сообщений нужно сделать игроку чтобы его замутила система")]
        public Int32 TryDuples;
        [JsonProperty(LanguageEn ? "Setting up automatic muting for duplicates" : "Настройка автоматического мута за дубликаты")]
        public ControllerMute.Muted MuteSetting;
    }

}

internal class AntiSpamDuples
{
    [JsonProperty(LanguageEn ? "Enable additional spam protection (Anti-duplicates, duplicate messages)" : "Включить дополнительную защиту от спама (Анти-дубликаты, повторяющие сообщения)")]
    public Boolean AntiSpamDuplesActivate;
    [JsonProperty(LanguageEn ? "How many duplicate messages does a player need to make to be confused by the system" : "Сколько дублирующих сообщений нужно сделать игроку чтобы его замутила система")]
    public Int32 TryDuples;
    [JsonProperty(LanguageEn ? "Setting up automatic muting for duplicates" : "Настройка автоматического мута за дубликаты")]
    public ControllerMute.Muted MuteSetting;
}

internal class PM
{
    [JsonProperty(LanguageEn ? "Enable Private Messages" : "Включить личные сообщения")]
    public Boolean PMActivate;
    [JsonProperty(LanguageEn ? "Sound when receiving a private message" : "Звук при при получении личного сообщения")]
    public String SoundPM;
}

internal class FormattingMessage
{
    [JsonProperty(LanguageEn ? "Enable message formatting [Will control caps, message format] (true - yes/false - no)" : "Включить форматирование сообщений [Будет контроллировать капс, формат сообщения] (true - да/false - нет)")]
    public Boolean FormatMessage;
    [JsonProperty(LanguageEn ? "Use a list of banned words (true - yes/false - no)" : "Использовать список запрещенных слов (true - да/false - нет)")]
    public Boolean UseBadWords;
    [JsonProperty(LanguageEn ? "The word that will replace the forbidden word" : "Слово которое будет заменять запрещенное слово")]
    public String ReplaceBadWord;
    [JsonProperty(LanguageEn ? "The list of forbidden words [Forbidden Word] = Whether to replace it in part of the word (for example, Vasya Fucking => Vasya ***) (true - yes/false - no)" : "Список запрещенных слов [ЗапрещенноеСлово] = Заменять ли его в части слова (например ВасяБля => Вася***) (true - да/false - нет)")]
    public Dictionary<String, Boolean> BadWords;
    [JsonProperty(LanguageEn ? "Nickname controller setup" : "Настройка контроллера ников")]
    public NickController ControllerNickname;
    internal class NickController
    {
        [JsonProperty(LanguageEn ? "Enable player nickname formatting (message formatting must be enabled)" : "Включить форматирование ников игроков (должно быть включено форматирование сообщений)")]
        public Boolean UseNickController;
        [JsonProperty(LanguageEn ? "The word that will replace the forbidden word (You can leave it blank and it will just delete)" : "Слово которое будет заменять запрещенное слово (Вы можете оставить пустым и будет просто удалять)")]
        public String ReplaceBadNick;
        [JsonProperty(LanguageEn ? "The list of forbidden nicknames [Forbidden Word] = Whether to replace it in part of the word (for example, Vasya Fucking => Vasya ***) (true - yes/false - no)" : "Список запрещенных ников [ЗапрещенноеСлово] = Заменять ли его в части слова (например ВасяБля => Вася***) (true - да/false - нет)")]
        public Dictionary<String, Boolean> BadNicks;
        [JsonProperty(LanguageEn ? "List of allowed links in nicknames" : "Список разрешенных ссылок в никах")]
        public List<String> AllowedLinkNick;
    }

}

internal class NickController
{
    [JsonProperty(LanguageEn ? "Enable player nickname formatting (message formatting must be enabled)" : "Включить форматирование ников игроков (должно быть включено форматирование сообщений)")]
    public Boolean UseNickController;
    [JsonProperty(LanguageEn ? "The word that will replace the forbidden word (You can leave it blank and it will just delete)" : "Слово которое будет заменять запрещенное слово (Вы можете оставить пустым и будет просто удалять)")]
    public String ReplaceBadNick;
    [JsonProperty(LanguageEn ? "The list of forbidden nicknames [Forbidden Word] = Whether to replace it in part of the word (for example, Vasya Fucking => Vasya ***) (true - yes/false - no)" : "Список запрещенных ников [ЗапрещенноеСлово] = Заменять ли его в части слова (например ВасяБля => Вася***) (true - да/false - нет)")]
    public Dictionary<String, Boolean> BadNicks;
    [JsonProperty(LanguageEn ? "List of allowed links in nicknames" : "Список разрешенных ссылок в никах")]
    public List<String> AllowedLinkNick;
}

internal class ControllerAlert
{
    [JsonProperty(LanguageEn ? "Setting up chat alerts" : "Настройка оповещений в чате")]
    public Alert AlertSetting;
    [JsonProperty(LanguageEn ? "Setting notifications about the status of the player's session" : "Настройка оповещений о статусе сессии игрока")]
    public PlayerSession PlayerSessionSetting;
    [JsonProperty(LanguageEn ? "Configuring administrator session status alerts" : "Настройка оповещений о статусе сессии администратора")]
    public AdminSession AdminSessionSetting;
    [JsonProperty(LanguageEn ? "Setting up personal notifications to the player when connecting" : "Настройка персональных оповоещений игроку при коннекте")]
    public PersonalAlert PersonalAlertSetting;
    internal class Alert
    {
        [JsonProperty(LanguageEn ? "Enable automatic messages in chat (true - yes/false - no)" : "Включить автоматические сообщения в чат (true - да/false - нет)")]
        public Boolean AlertMessage;
        [JsonProperty(LanguageEn ? "Type of automatic messages : true - sequential / false - random" : "Тип автоматических сообщений : true - поочередные/false - случайные")]
        public Boolean AlertMessageType;
        [JsonProperty(LanguageEn ? "List of automatic messages in chat" : "Список автоматических сообщений в чат")]
        public LanguageController MessageList;
        [JsonProperty(LanguageEn ? "Interval for sending messages to chat (Broadcaster) (in seconds)" : "Интервал отправки сообщений в чат (Броадкастер) (в секундах)")]
        public Int32 MessageListTimer;
    }

    internal class PlayerSession
    {
        [JsonProperty(LanguageEn ? "When a player is notified about the entry / exit of the player, display his avatar opposite the nickname (true - yes / false - no)" : "При уведомлении о входе/выходе игрока отображать его аватар напротив ника (true - да/false - нет)")]
        public Boolean ConnectedAvatarUse;
        [JsonProperty(LanguageEn ? "Notify in chat when a player enters (true - yes/false - no)" : "Уведомлять в чате о входе игрока (true - да/false - нет)")]
        public Boolean ConnectedAlert;
        [JsonProperty(LanguageEn ? "Enable random notifications when a player from the list enters (true - yes / false - no)" : "Включить случайные уведомления о входе игрока из списка (true - да/false - нет)")]
        public Boolean ConnectionAlertRandom;
        [JsonProperty(LanguageEn ? "Show the country of the entered player (true - yes/false - no)" : "Отображать страну зашедшего игрока (true - да/false - нет")]
        public Boolean ConnectedWorld;
        [JsonProperty(LanguageEn ? "Notify when a player enters the chat (selected from the list) (true - yes/false - no)" : "Уведомлять о выходе игрока в чат(выбираются из списка) (true - да/false - нет)")]
        public Boolean DisconnectedAlert;
        [JsonProperty(LanguageEn ? "Enable random player exit notifications (true - yes/false - no)" : "Включить случайные уведомления о выходе игрока (true - да/false - нет)")]
        public Boolean DisconnectedAlertRandom;
        [JsonProperty(LanguageEn ? "Display reason for player exit (true - yes/false - no)" : "Отображать причину выхода игрока (true - да/false - нет)")]
        public Boolean DisconnectedReason;
        [JsonProperty(LanguageEn ? "Random player entry notifications({0} - player's nickname, {1} - country (if country display is enabled)" : "Случайные уведомления о входе игрока({0} - ник игрока, {1} - страна(если включено отображение страны)")]
        public LanguageController RandomConnectionAlert;
        [JsonProperty(LanguageEn ? "Random notifications about the exit of the player ({0} - player's nickname, {1} - the reason for the exit (if the reason is enabled)" : "Случайные уведомления о выходе игрока({0} - ник игрока, {1} - причина выхода(если включена причина)")]
        public LanguageController RandomDisconnectedAlert;
    }

    internal class AdminSession
    {
        [JsonProperty(LanguageEn ? "Notify admin on the server in the chat (true - yes/false - no)" : "Уведомлять о входе админа на сервер в чат (true - да/false - нет)")]
        public Boolean ConnectedAlertAdmin;
        [JsonProperty(LanguageEn ? "Notify about admin leaving the server in chat (true - yes/false - no)" : "Уведомлять о выходе админа на сервер в чат (true - да/false - нет)")]
        public Boolean DisconnectedAlertAdmin;
    }

    internal class PersonalAlert
    {
        [JsonProperty(LanguageEn ? "Enable random message to the player who has logged in (true - yes/false - no)" : "Включить случайное сообщение зашедшему игроку (true - да/false - нет)")]
        public Boolean UseWelcomeMessage;
        [JsonProperty(LanguageEn ? "List of messages to the player when entering" : "Список сообщений игроку при входе")]
        public LanguageController WelcomeMessage;
    }

}

internal class Alert
{
    [JsonProperty(LanguageEn ? "Enable automatic messages in chat (true - yes/false - no)" : "Включить автоматические сообщения в чат (true - да/false - нет)")]
    public Boolean AlertMessage;
    [JsonProperty(LanguageEn ? "Type of automatic messages : true - sequential / false - random" : "Тип автоматических сообщений : true - поочередные/false - случайные")]
    public Boolean AlertMessageType;
    [JsonProperty(LanguageEn ? "List of automatic messages in chat" : "Список автоматических сообщений в чат")]
    public LanguageController MessageList;
    [JsonProperty(LanguageEn ? "Interval for sending messages to chat (Broadcaster) (in seconds)" : "Интервал отправки сообщений в чат (Броадкастер) (в секундах)")]
    public Int32 MessageListTimer;
}

internal class PlayerSession
{
    [JsonProperty(LanguageEn ? "When a player is notified about the entry / exit of the player, display his avatar opposite the nickname (true - yes / false - no)" : "При уведомлении о входе/выходе игрока отображать его аватар напротив ника (true - да/false - нет)")]
    public Boolean ConnectedAvatarUse;
    [JsonProperty(LanguageEn ? "Notify in chat when a player enters (true - yes/false - no)" : "Уведомлять в чате о входе игрока (true - да/false - нет)")]
    public Boolean ConnectedAlert;
    [JsonProperty(LanguageEn ? "Enable random notifications when a player from the list enters (true - yes / false - no)" : "Включить случайные уведомления о входе игрока из списка (true - да/false - нет)")]
    public Boolean ConnectionAlertRandom;
    [JsonProperty(LanguageEn ? "Show the country of the entered player (true - yes/false - no)" : "Отображать страну зашедшего игрока (true - да/false - нет")]
    public Boolean ConnectedWorld;
    [JsonProperty(LanguageEn ? "Notify when a player enters the chat (selected from the list) (true - yes/false - no)" : "Уведомлять о выходе игрока в чат(выбираются из списка) (true - да/false - нет)")]
    public Boolean DisconnectedAlert;
    [JsonProperty(LanguageEn ? "Enable random player exit notifications (true - yes/false - no)" : "Включить случайные уведомления о выходе игрока (true - да/false - нет)")]
    public Boolean DisconnectedAlertRandom;
    [JsonProperty(LanguageEn ? "Display reason for player exit (true - yes/false - no)" : "Отображать причину выхода игрока (true - да/false - нет)")]
    public Boolean DisconnectedReason;
    [JsonProperty(LanguageEn ? "Random player entry notifications({0} - player's nickname, {1} - country (if country display is enabled)" : "Случайные уведомления о входе игрока({0} - ник игрока, {1} - страна(если включено отображение страны)")]
    public LanguageController RandomConnectionAlert;
    [JsonProperty(LanguageEn ? "Random notifications about the exit of the player ({0} - player's nickname, {1} - the reason for the exit (if the reason is enabled)" : "Случайные уведомления о выходе игрока({0} - ник игрока, {1} - причина выхода(если включена причина)")]
    public LanguageController RandomDisconnectedAlert;
}

internal class AdminSession
{
    [JsonProperty(LanguageEn ? "Notify admin on the server in the chat (true - yes/false - no)" : "Уведомлять о входе админа на сервер в чат (true - да/false - нет)")]
    public Boolean ConnectedAlertAdmin;
    [JsonProperty(LanguageEn ? "Notify about admin leaving the server in chat (true - yes/false - no)" : "Уведомлять о выходе админа на сервер в чат (true - да/false - нет)")]
    public Boolean DisconnectedAlertAdmin;
}

internal class PersonalAlert
{
    [JsonProperty(LanguageEn ? "Enable random message to the player who has logged in (true - yes/false - no)" : "Включить случайное сообщение зашедшему игроку (true - да/false - нет)")]
    public Boolean UseWelcomeMessage;
    [JsonProperty(LanguageEn ? "List of messages to the player when entering" : "Список сообщений игроку при входе")]
    public LanguageController WelcomeMessage;
}

public class LanguageController
{
    [JsonProperty(LanguageEn ? "Setting up Multilingual Messages [Language Code] = Translation Variations" : "Настройка мультиязычных сообщений [КодЯзыка] = ВариацииПеревода")]
    public Dictionary<String, List<String>> LanguageMessages;
}

internal class RustPlus
{
    [JsonProperty(LanguageEn ? "Use Rust+" : "Использовать Rust+")]
    public Boolean UseRustPlus;
    [JsonProperty(LanguageEn ? "Title for notification Rust+" : "Название для уведомления Rust+")]
    public String DisplayNameAlert;
}

internal class ReferenceSettings
{
    [JsonProperty(LanguageEn ? "Settings XLevels" : "Настройка XLevels")]
    public XLevels XLevelsSettings;
    [JsonProperty(LanguageEn ? "Settings IQFakeActive" : "Настройка IQFakeActive")]
    public IQFakeActive IQFakeActiveSettings;
    [JsonProperty(LanguageEn ? "Settings IQRankSystem" : "Настройка IQRankSystem")]
    public IQRankSystem IQRankSystems;
    [JsonProperty(LanguageEn ? "Settings Clans" : "Настройка Clans")]
    public Clans ClansSettings;
    [JsonProperty(LanguageEn ? "Settings TranslationAPI" : "Настройка TranslationAPI")]
    public TranslataionApi translationApiSettings;
    [JsonProperty(LanguageEn ? "Settings SkillTree" : "Настройка SkillTree")]
    public SkillTree skillTreeSettings;
    [JsonProperty(LanguageEn ? "Settings PlayerRanks" : "Настройка PlayerRanks")]
    public PlayerRanks playerRanksSettings;
    [JsonProperty(LanguageEn ? "Settings XPrison" : "Настройка XPrison")]
    public XPrison xPrisonSettings;
    internal class TranslataionApi
    {
        [JsonProperty(LanguageEn ? "To use automatic message translation using the TranslationAPI" : "Использовать автоматический перевод сообщений с помощью TranslataionAPI")]
        public Boolean useTranslationApi;
        [JsonProperty(LanguageEn ? "Translate team chat" : "Переводить командный чат")]
        public Boolean translateTeamChat;
        [JsonProperty(LanguageEn ? "Translate chat in private messages." : "Переводить чат в личных сообщениях")]
        public Boolean translatePmChat;
        [JsonProperty(LanguageEn ? "The code for the preferred language (leave it empty, and then the translation will be done in each player's language)" : "Код приоритетного языка (оставьте пустым и тогда для каждого игрока будет переводиться на его языке клиента)")]
        public String codeLanguagePrimary;
    }

    internal class Clans
    {
        [JsonProperty(LanguageEn ? "Display a clan tag in the chat (if Clans are installed)" : "Отображать в чате клановый тэг (если установлены Clans)")]
        public Boolean UseClanTag;
        [JsonProperty(LanguageEn ? "The color of the additional tag" : "Цвет дополнительного тэга")]
        public String colorTag;
    }

    internal class IQRankSystem
    {
        [JsonProperty(LanguageEn ? "Rank display format in chat ( {0} is the user's rank, do not delete this value)" : "Формат отображения ранга в чате ( {0} - это ранг юзера, не удаляйте это значение)")]
        public String FormatRank;
        [JsonProperty(LanguageEn ? "Time display format with IQRank System in chat ( {0} is the user's time, do not delete this value)" : "Формат отображения времени с IQRankSystem в чате ( {0} - это время юзера, не удаляйте это значение)")]
        public String FormatRankTime;
        [JsonProperty(LanguageEn ? "Use support IQRankSystem" : "Использовать поддержку рангов")]
        public Boolean UseRankSystem;
        [JsonProperty(LanguageEn ? "Show players their played time next to their rank" : "Отображать игрокам их отыгранное время рядом с рангом")]
        public Boolean UseTimeStandart;
    }

    internal class IQFakeActive
    {
        [JsonProperty(LanguageEn ? "Use support IQFakeActive" : "Использовать поддержку IQFakeActive")]
        public Boolean UseIQFakeActive;
    }

    internal class XLevels
    {
        [JsonProperty(LanguageEn ? "Use support XLevels" : "Использовать поддержку XLevels")]
        public Boolean UseXLevels;
        [JsonProperty(LanguageEn ? "Use full prefix with level from XLevel (true) otherwise only level (false)" : "Использовать полный префикс с уровнем из XLevel (true) иначе только уровень (false)")]
        public Boolean UseFullXLevels;
        [JsonProperty(LanguageEn ? "The color of the additional tag" : "Цвет дополнительного тэга")]
        public String colorTag;
    }

    internal class XPrison
    {
        [JsonProperty(LanguageEn ? "Use support XPrison" : "Использовать поддержку XPrison")]
        public Boolean UseXPrison;
        [JsonProperty(LanguageEn ? "The color of the additional tag" : "Цвет дополнительного тэга")]
        public String colorTag;
    }

    internal class SkillTree
    {
        [JsonProperty(LanguageEn ? "Use support SkillTree" : "Использовать поддержку SkillTree")]
        public Boolean UseSkillTree;
        [JsonProperty(LanguageEn ? "Use full XP + Level information output (true), use only Level (false)" : "Использовать полный вывод информации XP + Level (true), использовать только Level (false)")]
        public Boolean UseFullSkillTree;
        [JsonProperty(LanguageEn ? "Use prestige" : "Использовать престиж")]
        public Boolean UsePrestigeSkillTree;
        [JsonProperty(LanguageEn ? "The color of the additional tag" : "Цвет дополнительного тэга")]
        public String colorTag;
    }

    internal class PlayerRanks
    {
        [JsonProperty(LanguageEn ? "Use support PlayerRanks" : "Использовать поддержку PlayerRanks")]
        public Boolean UsePlayerRanks;
        [JsonProperty(LanguageEn ? "The color of the additional tag" : "Цвет дополнительного тэга")]
        public String colorTag;
    }

}

internal class TranslataionApi
{
    [JsonProperty(LanguageEn ? "To use automatic message translation using the TranslationAPI" : "Использовать автоматический перевод сообщений с помощью TranslataionAPI")]
    public Boolean useTranslationApi;
    [JsonProperty(LanguageEn ? "Translate team chat" : "Переводить командный чат")]
    public Boolean translateTeamChat;
    [JsonProperty(LanguageEn ? "Translate chat in private messages." : "Переводить чат в личных сообщениях")]
    public Boolean translatePmChat;
    [JsonProperty(LanguageEn ? "The code for the preferred language (leave it empty, and then the translation will be done in each player's language)" : "Код приоритетного языка (оставьте пустым и тогда для каждого игрока будет переводиться на его языке клиента)")]
    public String codeLanguagePrimary;
}

internal class Clans
{
    [JsonProperty(LanguageEn ? "Display a clan tag in the chat (if Clans are installed)" : "Отображать в чате клановый тэг (если установлены Clans)")]
    public Boolean UseClanTag;
    [JsonProperty(LanguageEn ? "The color of the additional tag" : "Цвет дополнительного тэга")]
    public String colorTag;
}

internal class IQRankSystem
{
    [JsonProperty(LanguageEn ? "Rank display format in chat ( {0} is the user's rank, do not delete this value)" : "Формат отображения ранга в чате ( {0} - это ранг юзера, не удаляйте это значение)")]
    public String FormatRank;
    [JsonProperty(LanguageEn ? "Time display format with IQRank System in chat ( {0} is the user's time, do not delete this value)" : "Формат отображения времени с IQRankSystem в чате ( {0} - это время юзера, не удаляйте это значение)")]
    public String FormatRankTime;
    [JsonProperty(LanguageEn ? "Use support IQRankSystem" : "Использовать поддержку рангов")]
    public Boolean UseRankSystem;
    [JsonProperty(LanguageEn ? "Show players their played time next to their rank" : "Отображать игрокам их отыгранное время рядом с рангом")]
    public Boolean UseTimeStandart;
}

internal class IQFakeActive
{
    [JsonProperty(LanguageEn ? "Use support IQFakeActive" : "Использовать поддержку IQFakeActive")]
    public Boolean UseIQFakeActive;
}

internal class XLevels
{
    [JsonProperty(LanguageEn ? "Use support XLevels" : "Использовать поддержку XLevels")]
    public Boolean UseXLevels;
    [JsonProperty(LanguageEn ? "Use full prefix with level from XLevel (true) otherwise only level (false)" : "Использовать полный префикс с уровнем из XLevel (true) иначе только уровень (false)")]
    public Boolean UseFullXLevels;
    [JsonProperty(LanguageEn ? "The color of the additional tag" : "Цвет дополнительного тэга")]
    public String colorTag;
}

internal class XPrison
{
    [JsonProperty(LanguageEn ? "Use support XPrison" : "Использовать поддержку XPrison")]
    public Boolean UseXPrison;
    [JsonProperty(LanguageEn ? "The color of the additional tag" : "Цвет дополнительного тэга")]
    public String colorTag;
}

internal class SkillTree
{
    [JsonProperty(LanguageEn ? "Use support SkillTree" : "Использовать поддержку SkillTree")]
    public Boolean UseSkillTree;
    [JsonProperty(LanguageEn ? "Use full XP + Level information output (true), use only Level (false)" : "Использовать полный вывод информации XP + Level (true), использовать только Level (false)")]
    public Boolean UseFullSkillTree;
    [JsonProperty(LanguageEn ? "Use prestige" : "Использовать престиж")]
    public Boolean UsePrestigeSkillTree;
    [JsonProperty(LanguageEn ? "The color of the additional tag" : "Цвет дополнительного тэга")]
    public String colorTag;
}

internal class PlayerRanks
{
    [JsonProperty(LanguageEn ? "Use support PlayerRanks" : "Использовать поддержку PlayerRanks")]
    public Boolean UsePlayerRanks;
    [JsonProperty(LanguageEn ? "The color of the additional tag" : "Цвет дополнительного тэга")]
    public String colorTag;
}

internal class AnswerMessage
{
    [JsonProperty(LanguageEn ? "Enable auto-reply? (true - yes/false - no)" : "Включить автоответчик?(true - да/false - нет)")]
    public bool UseAnswer;
    [JsonProperty(LanguageEn ? "Customize Messages [Keyword] = Reply" : "Настройка сообщений [Ключевое слово] = Ответ")]
    public Dictionary<String, LanguageController> AnswerMessageList;
}

internal class OtherSettings
{
    [JsonProperty("SteamApiKey (https://steamcommunity.com/dev/apikey)")]
    public String renameSteamApiKey;
    [JsonProperty(LanguageEn ? "Enable the /online command (true - yes / false - no)" : "Включить команду /online (true - да/ false - нет)")]
    public Boolean UseCommandOnline;
    [JsonProperty(LanguageEn ? "Use shortened format /online (will only display quantity)" : "Использовать сокращенный формат /online (будет отображать только количество)")]
    public Boolean UseCommandShortOnline;
    [JsonProperty(LanguageEn ? "Compact logging of messages" : "Компактное логирование сообщений")]
    public CompactLoggetChat CompactLogsChat;
    [JsonProperty(LanguageEn ? "Setting up message logging" : "Настройка логирования сообщений")]
    public LoggedChat LogsChat;
    [JsonProperty(LanguageEn ? "Setting up logging of personal messages of players" : "Настройка логирования личных сообщений игроков")]
    public General LogsPMChat;
    [JsonProperty(LanguageEn ? "Setting up chat/voice lock/unlock logging" : "Настройка логирования блокировок/разблокировок чата/голоса")]
    public General LogsMuted;
    [JsonProperty(LanguageEn ? "Setting up logging of chat commands from players" : "Настройка логирования чат-команд от игроков")]
    public General LogsChatCommands;
    internal class CompactLoggetChat
    {
        [JsonProperty(LanguageEn ? "Display Steam64ID in the log (true - yes/false - no)" : "Отображать в логе Steam64ID (true - да/false - нет)")]
        public Boolean ShowSteamID;
        [JsonProperty(LanguageEn ? "Setting up compact message logging" : "Настройка компактного логирования сообщений")]
        public LoggedChat LogsCompactChat;
    }

    internal class LoggedChat
    {
        [JsonProperty(LanguageEn ? "Setting up general chat logging" : "Настройка логирования общего чата")]
        public General GlobalChatSettings;
        [JsonProperty(LanguageEn ? "Setting up team chat logging" : "Настройка логирования тим чата")]
        public General TeamChatSettings;
    }

    internal class General
    {
        [JsonProperty(LanguageEn ? "Enable logging (true - yes/false - no)" : "Включить логирование (true - да/false - нет)")]
        public Boolean UseLogged;
        [JsonProperty(LanguageEn ? "Webhooks channel for logging" : "Webhooks канала для логирования")]
        public String Webhooks;
    }

}

internal class CompactLoggetChat
{
    [JsonProperty(LanguageEn ? "Display Steam64ID in the log (true - yes/false - no)" : "Отображать в логе Steam64ID (true - да/false - нет)")]
    public Boolean ShowSteamID;
    [JsonProperty(LanguageEn ? "Setting up compact message logging" : "Настройка компактного логирования сообщений")]
    public LoggedChat LogsCompactChat;
}

internal class LoggedChat
{
    [JsonProperty(LanguageEn ? "Setting up general chat logging" : "Настройка логирования общего чата")]
    public General GlobalChatSettings;
    [JsonProperty(LanguageEn ? "Setting up team chat logging" : "Настройка логирования тим чата")]
    public General TeamChatSettings;
}

internal class General
{
    [JsonProperty(LanguageEn ? "Enable logging (true - yes/false - no)" : "Включить логирование (true - да/false - нет)")]
    public Boolean UseLogged;
    [JsonProperty(LanguageEn ? "Webhooks channel for logging" : "Webhooks канала для логирования")]
    public String Webhooks;
}

internal class AntiNoob
{
    public DateTime DateConnection;
    public Boolean IsNoob(Int32 TimeBlocked);
    public Double LeftTime(Int32 TimeBlocked);
}

public class User
{
    public Information Info;
    public Setting Settings;
    public Mute MuteInfo;
    internal class Information
    {
        public String Prefix;
        public String ColorNick;
        public String ColorMessage;
        public String Rank;
        public String CustomColorNick;
        public String CustomColorMessage;
        public List<String> PrefixList;
    }

    internal class Setting
    {
        public Boolean TurnPM;
        public Boolean TurnAlert;
        public Boolean TurnBroadcast;
        public Boolean TurnSound;
        public List<UInt64> IgnoreUsers;
        public Boolean IsIgnored(UInt64 TargetID);
        public void IgnoredAddOrRemove(UInt64 TargetID);
    }

    internal class Mute
    {
        public Double TimeMuteChat;
        public Double TimeMuteVoice;
        public Double GetTime(MuteType Type);
        public void SetMute(MuteType Type, Int32 Time);
        public void UnMute(MuteType Type);
        public Boolean IsMute(MuteType Type);
    }

}

internal class Information
{
    public String Prefix;
    public String ColorNick;
    public String ColorMessage;
    public String Rank;
    public String CustomColorNick;
    public String CustomColorMessage;
    public List<String> PrefixList;
}

internal class Setting
{
    public Boolean TurnPM;
    public Boolean TurnAlert;
    public Boolean TurnBroadcast;
    public Boolean TurnSound;
    public List<UInt64> IgnoreUsers;
    public Boolean IsIgnored(UInt64 TargetID);
    public void IgnoredAddOrRemove(UInt64 TargetID);
}

internal class Mute
{
    public Double TimeMuteChat;
    public Double TimeMuteVoice;
    public Double GetTime(MuteType Type);
    public void SetMute(MuteType Type, Int32 Time);
    public void UnMute(MuteType Type);
    public Boolean IsMute(MuteType Type);
}

public class GeneralInformation
{
    public Boolean TurnMuteAllChat;
    public Boolean TurnMuteAllVoice;
    public Dictionary<UInt64, RenameInfo> RenameList;
    internal class RenameInfo
    {
        public String RenameNick;
        public UInt64 RenameID;
    }

    public RenameInfo GetInfoRename(UInt64 UserID);
}

internal class RenameInfo
{
    public String RenameNick;
    public UInt64 RenameID;
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

internal class FlooderInfo
{
    public Double Time;
    public String LastMessage;
    public Int32 TryFlood;
}

private class ImageUI
{
    private const String _path;
    private const String _printPath;
    private readonly Dictionary<String, ImageData> _images;
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

private class InformationOpenedUI
{
    public List<Configuration.ControllerParameters.AdvancedFuncion> ElementsPrefix;
    public List<Configuration.ControllerParameters.AdvancedFuncion> ElementsNick;
    public List<Configuration.ControllerParameters.AdvancedFuncion> ElementsChat;
    public List<Configuration.ControllerParameters.AdvancedFuncion> ElementsRanks;
    public Int32 SlideIndexPrefix;
    public Int32 SlideIndexNick;
    public Int32 SlideIndexChat;
    public Int32 SlideIndexRank;
}

private class InterfaceBuilder
{
    public static InterfaceBuilder Instance;
    public const String UI_Chat_Context;
    public const String UI_Chat_Context_Visual_Nick;
    public const String UI_Chat_Alert;
    public Dictionary<String, String> Interfaces;
    public InterfaceBuilder();
    public static void AddInterface(String name, String json);
    public static string GetInterface(String name);
    public static void DestroyAll();
    private void BuildingVisualNick();
    private void BuildingStaticContext();
    private void BuildingCheckBox();
    private void BuildingSlider();
    private void BuildingSliderUpdateArgument();
    private void BuildingMuteAndIgnore();
    private void BuildingMuteAndIgnorePlayerPanel();
    private void BuildingMuteAndIgnorePlayer();
    private void BuildingMuteAndIgnorePages();
    private void BuildingMuteAndIgnorePanelAlert();
    private void BuildingMuteAlert();
    private void BuildingMuteAlert_DropList_Title();
    private void BuildingMuteAlert_DropList_Reason();
    private void BuildingIgnoreAlert();
    private void BuildingDropList();
    private void BuildingOpenDropList();
    private void BuildingElementDropList();
    private void BuildingElementDropListTakeLine();
    private void BuildingModerationStatic();
    private void BuildingMuteAllChat();
    private void BuildingMuteAllVoice();
    private void BuildingAlertUI();
}


```

---

## IQControllerSpawnCars

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using Newtonsoft.Json;
using Rust.Modular;

Oxide.Plugins
[Info("IQControllerSpawnCars", "SkuliDropek", "1.0.3")]
[Description("Хочу следовать за трендами блин")]
 class IQControllerSpawnCars : RustPlugin
{
    private static Configuration config;
    private class Configuration
    {
        [JsonProperty("Выберите тип спавна. 0 - полный спавн по тирам(настраивайте шансы и тиры в листе)(т.е все детали сразу,в разном виде качества). 1 - Спавн отдельных деталей, с ограничениями в количестве и рандомным качеством в зависимости от шанса")]
        public SpawnType spawnType;
        [JsonProperty("Настройки тиров. Номер тира(1-3) и шанс.")]
        public Dictionary<Int32, Int32> TierRare;
        [JsonProperty("Настройка деталей и их шанс спавна.Шортнейм детали и шанс ее спавна")]
        public Dictionary<String, Int32> ElementSpawnRare;
        [JsonProperty("Ограниченное количество спавна деталей. 0 - без ограничений")]
        public Int32 LimitSpawnElement;
        [JsonProperty("Настройка заполнения топливом машин")]
        public FuelSettings fuelSettings;
        internal class FuelSettings
        {
            [JsonProperty("Включить спавн топлива в машинах (true - да/false - нет)")]
            public Boolean UseFuelSpawned;
            [JsonProperty("Статичное количество топлива (Если включен рандом, то этот показатель не будет учитываться)")]
            public Int32 FuelStatic;
            [JsonProperty("Шанс заполнения топливом транспорт (0-100)")]
            public Int32 RareUsing;
            [JsonProperty("Настройка рандомного спавна топлива")]
            public RandomFuel randomFuel;
            internal class RandomFuel
            {
                [JsonProperty("Включить рандомное количество спавна топлива (true - да/false - нет)")]
                public Boolean UseRandom;
                [JsonProperty("Минимальное количество топлива")]
                public Int32 MinFuel;
                [JsonProperty("Максимальное количество топлива")]
                public Int32 MaxFuel;
            }

            public Int32 GetFuelSpawned();
        }

        public static Configuration GetNewConfiguration();
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private void OnServerInitialized();
     void OnVehicleModulesAssigned(ModularCar Car, ItemModVehicleModule[] modulePreset);
    private void CarUpgrade(ModularCar Car);
    private void FuelSystemSpawned(EntityFuelSystem FuelSystem);
     void SpawnFullTier(ModularCar Car);
     void SpawnElementsTier(ModularCar Car);
    public bool IsRare(Int32 Rare);
}

private class Configuration
{
    [JsonProperty("Выберите тип спавна. 0 - полный спавн по тирам(настраивайте шансы и тиры в листе)(т.е все детали сразу,в разном виде качества). 1 - Спавн отдельных деталей, с ограничениями в количестве и рандомным качеством в зависимости от шанса")]
    public SpawnType spawnType;
    [JsonProperty("Настройки тиров. Номер тира(1-3) и шанс.")]
    public Dictionary<Int32, Int32> TierRare;
    [JsonProperty("Настройка деталей и их шанс спавна.Шортнейм детали и шанс ее спавна")]
    public Dictionary<String, Int32> ElementSpawnRare;
    [JsonProperty("Ограниченное количество спавна деталей. 0 - без ограничений")]
    public Int32 LimitSpawnElement;
    [JsonProperty("Настройка заполнения топливом машин")]
    public FuelSettings fuelSettings;
    internal class FuelSettings
    {
        [JsonProperty("Включить спавн топлива в машинах (true - да/false - нет)")]
        public Boolean UseFuelSpawned;
        [JsonProperty("Статичное количество топлива (Если включен рандом, то этот показатель не будет учитываться)")]
        public Int32 FuelStatic;
        [JsonProperty("Шанс заполнения топливом транспорт (0-100)")]
        public Int32 RareUsing;
        [JsonProperty("Настройка рандомного спавна топлива")]
        public RandomFuel randomFuel;
        internal class RandomFuel
        {
            [JsonProperty("Включить рандомное количество спавна топлива (true - да/false - нет)")]
            public Boolean UseRandom;
            [JsonProperty("Минимальное количество топлива")]
            public Int32 MinFuel;
            [JsonProperty("Максимальное количество топлива")]
            public Int32 MaxFuel;
        }

        public Int32 GetFuelSpawned();
    }

    public static Configuration GetNewConfiguration();
}

internal class FuelSettings
{
    [JsonProperty("Включить спавн топлива в машинах (true - да/false - нет)")]
    public Boolean UseFuelSpawned;
    [JsonProperty("Статичное количество топлива (Если включен рандом, то этот показатель не будет учитываться)")]
    public Int32 FuelStatic;
    [JsonProperty("Шанс заполнения топливом транспорт (0-100)")]
    public Int32 RareUsing;
    [JsonProperty("Настройка рандомного спавна топлива")]
    public RandomFuel randomFuel;
    internal class RandomFuel
    {
        [JsonProperty("Включить рандомное количество спавна топлива (true - да/false - нет)")]
        public Boolean UseRandom;
        [JsonProperty("Минимальное количество топлива")]
        public Int32 MinFuel;
        [JsonProperty("Максимальное количество топлива")]
        public Int32 MaxFuel;
    }

    public Int32 GetFuelSpawned();
}

internal class RandomFuel
{
    [JsonProperty("Включить рандомное количество спавна топлива (true - да/false - нет)")]
    public Boolean UseRandom;
    [JsonProperty("Минимальное количество топлива")]
    public Int32 MinFuel;
    [JsonProperty("Максимальное количество топлива")]
    public Int32 MaxFuel;
}


```

---

## IQDailyStep

```csharp
using ConVar;
using Newtonsoft.Json;
using Oxide.Core.Plugins;
using System;
using System.Collections.Generic;
using System.Linq;
using UnityEngine;
using Oxide.Game.Rust.Cui;
using System.Collections;

Oxide.Plugins
[Info("IQDailyStep", "SkuliDropek", "0.0.5")]
[Description("Ежедневные награды")]
 class IQDailyStep : RustPlugin
{
    [PluginReference]
     Plugin IQChat;
     Plugin IQEconomic;
     Plugin IQCases;
     Plugin ImageLibrary;
     Plugin RustStore;
    public void SendChat(string Message, BasePlayer player, Chat.ChatChannel channel);
    private string GetImage(string fileName, ulong skin);
    public bool AddImage(string url, string shortname, ulong skin);
    public void AddAllImage();
    public string GetImageReward(int Day, int ItemIndex);
     string IQEconomicGetIL();
     void IQCasesGiveCase(ulong userID, string CaseKey, int Amount);
     bool IQCASE_EXIST(string CaseKey);
     void IQEconomicBalanceSet(ulong userID, int Balance);
    public void MoscovOVHBalanceSet(ulong userID, int Balance);
    public void GameStoresBalanceSet(ulong userID, int Balance);
    private static Configuration config;
    private class Configuration
    {
        [JsonProperty("Настройка дней")]
        public List<DaySetting> Days;
        [JsonProperty("Настройка плагина")]
        public GeneraldSettings GeneralSetting;
        [JsonProperty("Настройка интерфейса")]
        public InterfaceSettings InterfaceSetting;
        public class GeneraldSettings
        {
            [JsonProperty("MoscovOVH : Включить использование магазина(Должен быть включен обмен валют)")]
            public bool MoscovOvhUse;
            [JsonProperty("GameStores : Включить использование магазина(Должен быть включен обмен валют)")]
            public bool GameStoreshUse;
            [JsonProperty("GameStores : Настройки магазина GameStores")]
            public GameStores GameStoresSettings;
            [JsonProperty("IQChat : Настройки чата")]
            public ChatSetting ChatSettings;
            internal class ChatSetting
            {
                [JsonProperty("IQChat : Кастомный префикс в чате")]
                public string CustomPrefix;
                [JsonProperty("IQChat : Кастомный аватар в чате(Если требуется)")]
                public string CustomAvatar;
                [JsonProperty("IQChat : Использовать UI уведомления")]
                public bool UIAlertUse;
            }

            internal class GameStores
            {
                [JsonProperty("API Магазина(GameStores)")]
                public string GameStoresAPIStore;
                [JsonProperty("ID Магазина(GameStores)")]
                public string GameStoresIDStore;
                [JsonProperty("Сообщение в магазин при выдаче баланса(GameStores)")]
                public string GameStoresMessage;
            }

        }

        internal class DaySetting
        {
            [JsonProperty("Настройка награды")]
            public List<RewardSetting> Rewards;
            [JsonProperty("Ссылка на картинку")]
            public string ImageLink;
            [JsonProperty("Ссылка на картинку в случае,если награда взята(Галочка)")]
            public string ImageLinkAccess;
            [JsonProperty("День")]
            public string Day;
            internal class RewardSetting
            {
                [JsonProperty("Использовать предметы RUST'a")]
                public bool UseItem;
                [JsonProperty("Использовать команды")]
                public bool UseCommand;
                [JsonProperty("Использовать баланс на магазин")]
                public bool UseStores;
                [JsonProperty("Использовать валюты с IQEconomic.Картинка будет подставляться с плагина сама(Если есть плагин IQEconomic)")]
                public bool UseIQEconomic;
                [JsonProperty("Использовать кейсы с IQCases.Картинка будет подставляться с плагина сама(Если есть)")]
                public bool UseIQCases;
                [JsonProperty("Настройка предметов RUST'a")]
                public List<ItemSetting> ItemSettings;
                [JsonProperty("Ссылка на картинку для набора предметов RUST'a")]
                public string PNGItemRust;
                [JsonProperty("Сколько баланса выдавать в магазин")]
                public int CountMoneyStore;
                [JsonProperty("Ссылка на картинку для иконки баланса с магазина")]
                public string PNGStoreBalance;
                [JsonProperty("Настройка IQCases(Если такой плагин имеется)")]
                public IQCasesSetting IQCasesSettings;
                [JsonProperty("Сколько монет выдавать IQEconomic(Если такой плагин имеется)")]
                public int AmountEconomic;
                [JsonProperty("Настройка команды")]
                public CommandSetting CommandSettings;
                internal class ItemSetting
                {
                    [JsonProperty("DisplayName (Если требуется для кастомного предмета,в ином случае оставляйте пустым)")]
                    public string DisplayName;
                    [JsonProperty("Shortname")]
                    public string Shortname;
                    [JsonProperty("SkinID")]
                    public ulong SkinID;
                    [JsonProperty("Количество")]
                    public int Amount;
                }

                internal class IQCasesSetting
                {
                    [JsonProperty("Ключ от кейса(к примеру freecase)")]
                    public string KeyCase;
                    [JsonProperty("Количество")]
                    public int Amount;
                }

                internal class CommandSetting
                {
                    [JsonProperty("Команда")]
                    public string Command;
                    [JsonProperty("Картинка для команды")]
                    public string CommandPNG;
                }

            }

        }

        internal class InterfaceSettings
        {
            [JsonProperty("Ссылка на задний фон")]
            public string BackgroundURL;
            [JsonProperty("Включить задний фон")]
            public bool UseBackground;
        }

        public static Configuration GetNewConfiguration();
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    [JsonProperty("Дата с днями игроков")]
    public Dictionary<ulong, Dictionary<int, bool>> DataPlayer;
     void ReadData();
     void WriteData();
     void RegisteredDataUser(BasePlayer player);
    [ChatCommand("day")]
     void DayilyMenuOpen(BasePlayer player);
     List<ulong> StopSpam;
    [ConsoleCommand("daily")]
     void ConsoleCommandDayTake(ConsoleSystem.Arg arg);
     void OnPlayerConnected(BasePlayer player);
    private void OnServerInitialized();
     void Unload();
     void GiveReward(BasePlayer player, int Day);
    public static string UI_MAIN_UI_STEP;
    public static string UI_MAIN_UI_STEP_TAKE;
     void Interface_Daily(BasePlayer player);
     void Interface_Days_Take(BasePlayer player, int Day, int ThisDay);
    public IEnumerator AnimationItems(BasePlayer player, int Day);
    private new void LoadDefaultMessages();
    private static string HexToRustFormat(string hex);
     void RunEffect(BasePlayer player, string path);
    public bool IsDay(ulong userID, int Day);
     Int32 GetPlayerDays(UInt64 userID);
}

private class Configuration
{
    [JsonProperty("Настройка дней")]
    public List<DaySetting> Days;
    [JsonProperty("Настройка плагина")]
    public GeneraldSettings GeneralSetting;
    [JsonProperty("Настройка интерфейса")]
    public InterfaceSettings InterfaceSetting;
    public class GeneraldSettings
    {
        [JsonProperty("MoscovOVH : Включить использование магазина(Должен быть включен обмен валют)")]
        public bool MoscovOvhUse;
        [JsonProperty("GameStores : Включить использование магазина(Должен быть включен обмен валют)")]
        public bool GameStoreshUse;
        [JsonProperty("GameStores : Настройки магазина GameStores")]
        public GameStores GameStoresSettings;
        [JsonProperty("IQChat : Настройки чата")]
        public ChatSetting ChatSettings;
        internal class ChatSetting
        {
            [JsonProperty("IQChat : Кастомный префикс в чате")]
            public string CustomPrefix;
            [JsonProperty("IQChat : Кастомный аватар в чате(Если требуется)")]
            public string CustomAvatar;
            [JsonProperty("IQChat : Использовать UI уведомления")]
            public bool UIAlertUse;
        }

        internal class GameStores
        {
            [JsonProperty("API Магазина(GameStores)")]
            public string GameStoresAPIStore;
            [JsonProperty("ID Магазина(GameStores)")]
            public string GameStoresIDStore;
            [JsonProperty("Сообщение в магазин при выдаче баланса(GameStores)")]
            public string GameStoresMessage;
        }

    }

    internal class DaySetting
    {
        [JsonProperty("Настройка награды")]
        public List<RewardSetting> Rewards;
        [JsonProperty("Ссылка на картинку")]
        public string ImageLink;
        [JsonProperty("Ссылка на картинку в случае,если награда взята(Галочка)")]
        public string ImageLinkAccess;
        [JsonProperty("День")]
        public string Day;
        internal class RewardSetting
        {
            [JsonProperty("Использовать предметы RUST'a")]
            public bool UseItem;
            [JsonProperty("Использовать команды")]
            public bool UseCommand;
            [JsonProperty("Использовать баланс на магазин")]
            public bool UseStores;
            [JsonProperty("Использовать валюты с IQEconomic.Картинка будет подставляться с плагина сама(Если есть плагин IQEconomic)")]
            public bool UseIQEconomic;
            [JsonProperty("Использовать кейсы с IQCases.Картинка будет подставляться с плагина сама(Если есть)")]
            public bool UseIQCases;
            [JsonProperty("Настройка предметов RUST'a")]
            public List<ItemSetting> ItemSettings;
            [JsonProperty("Ссылка на картинку для набора предметов RUST'a")]
            public string PNGItemRust;
            [JsonProperty("Сколько баланса выдавать в магазин")]
            public int CountMoneyStore;
            [JsonProperty("Ссылка на картинку для иконки баланса с магазина")]
            public string PNGStoreBalance;
            [JsonProperty("Настройка IQCases(Если такой плагин имеется)")]
            public IQCasesSetting IQCasesSettings;
            [JsonProperty("Сколько монет выдавать IQEconomic(Если такой плагин имеется)")]
            public int AmountEconomic;
            [JsonProperty("Настройка команды")]
            public CommandSetting CommandSettings;
            internal class ItemSetting
            {
                [JsonProperty("DisplayName (Если требуется для кастомного предмета,в ином случае оставляйте пустым)")]
                public string DisplayName;
                [JsonProperty("Shortname")]
                public string Shortname;
                [JsonProperty("SkinID")]
                public ulong SkinID;
                [JsonProperty("Количество")]
                public int Amount;
            }

            internal class IQCasesSetting
            {
                [JsonProperty("Ключ от кейса(к примеру freecase)")]
                public string KeyCase;
                [JsonProperty("Количество")]
                public int Amount;
            }

            internal class CommandSetting
            {
                [JsonProperty("Команда")]
                public string Command;
                [JsonProperty("Картинка для команды")]
                public string CommandPNG;
            }

        }

    }

    internal class InterfaceSettings
    {
        [JsonProperty("Ссылка на задний фон")]
        public string BackgroundURL;
        [JsonProperty("Включить задний фон")]
        public bool UseBackground;
    }

    public static Configuration GetNewConfiguration();
}

public class GeneraldSettings
{
    [JsonProperty("MoscovOVH : Включить использование магазина(Должен быть включен обмен валют)")]
    public bool MoscovOvhUse;
    [JsonProperty("GameStores : Включить использование магазина(Должен быть включен обмен валют)")]
    public bool GameStoreshUse;
    [JsonProperty("GameStores : Настройки магазина GameStores")]
    public GameStores GameStoresSettings;
    [JsonProperty("IQChat : Настройки чата")]
    public ChatSetting ChatSettings;
    internal class ChatSetting
    {
        [JsonProperty("IQChat : Кастомный префикс в чате")]
        public string CustomPrefix;
        [JsonProperty("IQChat : Кастомный аватар в чате(Если требуется)")]
        public string CustomAvatar;
        [JsonProperty("IQChat : Использовать UI уведомления")]
        public bool UIAlertUse;
    }

    internal class GameStores
    {
        [JsonProperty("API Магазина(GameStores)")]
        public string GameStoresAPIStore;
        [JsonProperty("ID Магазина(GameStores)")]
        public string GameStoresIDStore;
        [JsonProperty("Сообщение в магазин при выдаче баланса(GameStores)")]
        public string GameStoresMessage;
    }

}

internal class ChatSetting
{
    [JsonProperty("IQChat : Кастомный префикс в чате")]
    public string CustomPrefix;
    [JsonProperty("IQChat : Кастомный аватар в чате(Если требуется)")]
    public string CustomAvatar;
    [JsonProperty("IQChat : Использовать UI уведомления")]
    public bool UIAlertUse;
}

internal class GameStores
{
    [JsonProperty("API Магазина(GameStores)")]
    public string GameStoresAPIStore;
    [JsonProperty("ID Магазина(GameStores)")]
    public string GameStoresIDStore;
    [JsonProperty("Сообщение в магазин при выдаче баланса(GameStores)")]
    public string GameStoresMessage;
}

internal class DaySetting
{
    [JsonProperty("Настройка награды")]
    public List<RewardSetting> Rewards;
    [JsonProperty("Ссылка на картинку")]
    public string ImageLink;
    [JsonProperty("Ссылка на картинку в случае,если награда взята(Галочка)")]
    public string ImageLinkAccess;
    [JsonProperty("День")]
    public string Day;
    internal class RewardSetting
    {
        [JsonProperty("Использовать предметы RUST'a")]
        public bool UseItem;
        [JsonProperty("Использовать команды")]
        public bool UseCommand;
        [JsonProperty("Использовать баланс на магазин")]
        public bool UseStores;
        [JsonProperty("Использовать валюты с IQEconomic.Картинка будет подставляться с плагина сама(Если есть плагин IQEconomic)")]
        public bool UseIQEconomic;
        [JsonProperty("Использовать кейсы с IQCases.Картинка будет подставляться с плагина сама(Если есть)")]
        public bool UseIQCases;
        [JsonProperty("Настройка предметов RUST'a")]
        public List<ItemSetting> ItemSettings;
        [JsonProperty("Ссылка на картинку для набора предметов RUST'a")]
        public string PNGItemRust;
        [JsonProperty("Сколько баланса выдавать в магазин")]
        public int CountMoneyStore;
        [JsonProperty("Ссылка на картинку для иконки баланса с магазина")]
        public string PNGStoreBalance;
        [JsonProperty("Настройка IQCases(Если такой плагин имеется)")]
        public IQCasesSetting IQCasesSettings;
        [JsonProperty("Сколько монет выдавать IQEconomic(Если такой плагин имеется)")]
        public int AmountEconomic;
        [JsonProperty("Настройка команды")]
        public CommandSetting CommandSettings;
        internal class ItemSetting
        {
            [JsonProperty("DisplayName (Если требуется для кастомного предмета,в ином случае оставляйте пустым)")]
            public string DisplayName;
            [JsonProperty("Shortname")]
            public string Shortname;
            [JsonProperty("SkinID")]
            public ulong SkinID;
            [JsonProperty("Количество")]
            public int Amount;
        }

        internal class IQCasesSetting
        {
            [JsonProperty("Ключ от кейса(к примеру freecase)")]
            public string KeyCase;
            [JsonProperty("Количество")]
            public int Amount;
        }

        internal class CommandSetting
        {
            [JsonProperty("Команда")]
            public string Command;
            [JsonProperty("Картинка для команды")]
            public string CommandPNG;
        }

    }

}

internal class RewardSetting
{
    [JsonProperty("Использовать предметы RUST'a")]
    public bool UseItem;
    [JsonProperty("Использовать команды")]
    public bool UseCommand;
    [JsonProperty("Использовать баланс на магазин")]
    public bool UseStores;
    [JsonProperty("Использовать валюты с IQEconomic.Картинка будет подставляться с плагина сама(Если есть плагин IQEconomic)")]
    public bool UseIQEconomic;
    [JsonProperty("Использовать кейсы с IQCases.Картинка будет подставляться с плагина сама(Если есть)")]
    public bool UseIQCases;
    [JsonProperty("Настройка предметов RUST'a")]
    public List<ItemSetting> ItemSettings;
    [JsonProperty("Ссылка на картинку для набора предметов RUST'a")]
    public string PNGItemRust;
    [JsonProperty("Сколько баланса выдавать в магазин")]
    public int CountMoneyStore;
    [JsonProperty("Ссылка на картинку для иконки баланса с магазина")]
    public string PNGStoreBalance;
    [JsonProperty("Настройка IQCases(Если такой плагин имеется)")]
    public IQCasesSetting IQCasesSettings;
    [JsonProperty("Сколько монет выдавать IQEconomic(Если такой плагин имеется)")]
    public int AmountEconomic;
    [JsonProperty("Настройка команды")]
    public CommandSetting CommandSettings;
    internal class ItemSetting
    {
        [JsonProperty("DisplayName (Если требуется для кастомного предмета,в ином случае оставляйте пустым)")]
        public string DisplayName;
        [JsonProperty("Shortname")]
        public string Shortname;
        [JsonProperty("SkinID")]
        public ulong SkinID;
        [JsonProperty("Количество")]
        public int Amount;
    }

    internal class IQCasesSetting
    {
        [JsonProperty("Ключ от кейса(к примеру freecase)")]
        public string KeyCase;
        [JsonProperty("Количество")]
        public int Amount;
    }

    internal class CommandSetting
    {
        [JsonProperty("Команда")]
        public string Command;
        [JsonProperty("Картинка для команды")]
        public string CommandPNG;
    }

}

internal class ItemSetting
{
    [JsonProperty("DisplayName (Если требуется для кастомного предмета,в ином случае оставляйте пустым)")]
    public string DisplayName;
    [JsonProperty("Shortname")]
    public string Shortname;
    [JsonProperty("SkinID")]
    public ulong SkinID;
    [JsonProperty("Количество")]
    public int Amount;
}

internal class IQCasesSetting
{
    [JsonProperty("Ключ от кейса(к примеру freecase)")]
    public string KeyCase;
    [JsonProperty("Количество")]
    public int Amount;
}

internal class CommandSetting
{
    [JsonProperty("Команда")]
    public string Command;
    [JsonProperty("Картинка для команды")]
    public string CommandPNG;
}

internal class InterfaceSettings
{
    [JsonProperty("Ссылка на задний фон")]
    public string BackgroundURL;
    [JsonProperty("Включить задний фон")]
    public bool UseBackground;
}


```

---

## IQEconomic

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
[Info("IQEconomic", "Mercury", "0.1.3")]
[Description("Экономика на ваш сервер")]
 class IQEconomic : RustPlugin
{
    [PluginReference]
     Plugin IQChat;
     Plugin Friends;
     Plugin Clans;
     Plugin Battles;
     Plugin Duel;
     Plugin RustStore;
     Plugin ImageLibrary;
    public void SendChat(string Message, BasePlayer player, Chat.ChatChannel channel);
    private string GetImage(string fileName, ulong skin);
    public bool AddImage(string url, string shortname, ulong skin);
    public void AddAllImage();
    public bool IsFriends(ulong userID, ulong targetID);
    public bool IsClans(ulong userID, ulong targetID);
    public bool IsDuel(ulong userID);
    public void MoscovOVHBalanceSet(ulong userID, int Balance, int MoneyTake);
    public void GameStoresBalanceSet(ulong userID, int Balance, int MoneyTake);
    private static Configuration config;
    private class Configuration
    {
        [JsonProperty("Монетки будут и гроков на руках - false / Иначе будет чат или интерфейсе - true")]
        public bool UseUI;
        [JsonProperty("Использовать UI интерфейс для отображения баланса(true - да(Если вы не поставили , чтобы монетки были на руках))/false - информация будет в чате по команде и при входе)")]
        public bool UseUIMoney;
        [JsonProperty("Включить обмен валюты на баланс в магазине(GameStores/MoscovOVH)")]
        public bool TransferStoreUse;
        [JsonProperty("Отображение интерфейса с балансом(true - отображает/false - скрывает)")]
        public bool ShowUI;
        [JsonProperty("Настройки обмена валют на баланс в магазине")]
        public TransferSetting TransferSettings;
        [JsonProperty("Основные настройки")]
        public GeneralSettings GeneralSetting;
        [JsonProperty("Настройки валюты(Если вид экономикик - false)")]
        public CustomMoney CustomMoneySetting;
        [JsonProperty("Настройки совместной работы с другими плагинами")]
        public ReferenceSetting ReferenceSettings;
        internal class CustomMoney
        {
            [JsonProperty("Название валюты")]
            public string DisplayName;
            [JsonProperty("Shortname монетки")]
            public string Shortname;
            [JsonProperty("SkinID монетки")]
            public ulong SkinID;
        }

        internal class GeneralSettings
        {
            [JsonProperty("Получение валюты за убийство игроков")]
            public bool BPlayerKillUse;
            [JsonProperty("Получение валюты за убийство животных")]
            public bool BPlayerAnimalUse;
            [JsonProperty("Получение валюты за убийство NPC")]
            public bool BPlayerNPCUse;
            [JsonProperty("Получение валюты за добычу ресурсов")]
            public bool BPlayerGatherUse;
            [JsonProperty("Получение валюты за уничтожение танка")]
            public bool BPlayerBradleyUse;
            [JsonProperty("Получение валюты за уничтожение вертолета")]
            public bool BPlayerHelicopterUse;
            [JsonProperty("Получение валюты за уничтожение бочек")]
            public bool BPLayerBarrelUse;
            [JsonProperty("Получение валюты проведенное время на сервере")]
            public bool BPLayerOnlineUse;
            [JsonProperty("Сколько нужно провести времени,чтобы выдали награду")]
            public int BPlayerOnlineTime;
            [JsonProperty("Сколько начислять валюты за проведенное время на сервере")]
            public int BPlayerOnlineGive;
            [JsonProperty("Настройка зачисления баланса за убийство игроков")]
            public AdvancedSetting BPlayerKillGive;
            [JsonProperty("Сколько начислять валюты за убийство животных")]
            public AdvancedSetting BPlayerAnimalGive;
            [JsonProperty("Сколько начислять валюты за убийство NPC")]
            public AdvancedSetting BPlayerNPCGive;
            [JsonProperty("Сколько начислять валюты за уничтожение танка")]
            public AdvancedSetting BPlayerBradleyGive;
            [JsonProperty("Сколько начислять валюты за уничтожение вертолета")]
            public AdvancedSetting BPlayerHelicopterGive;
            [JsonProperty("Сколько начислять валюты за уничтожение бочек")]
            public AdvancedSetting BPlayerBarrelGive;
            [JsonProperty("Сколько начислять валюты за добычу ресурсов ( [за какой ресурс давать] = { остальная настройка }")]
            public Dictionary<string,AdvancedSetting> BPlayerGatherGive;
            internal class AdvancedSetting
            {
                [JsonProperty("Шанс получить валюту")]
                public int Rare;
                [JsonProperty("Сколько выдавать валюты")]
                public int BPlayerGive;
            }

        }

        internal class TransferSetting
        {
            [JsonProperty("URL вашей монеты")]
            public string URLMoney;
            [JsonProperty("URL валюты для магазина")]
            public string URLStores;
            [JsonProperty("Сколько монет требуется для обмена")]
            public int MoneyCount;
            [JsonProperty("Сколько баланса получит игрок после обмена")]
            public int StoresMoneyCount;
        }

        internal class ReferenceSetting
        {
            [JsonProperty("Friends : Запретить получение монет за убийство друзей")]
            public bool FriendsBlockUse;
            [JsonProperty("Clans : Запретить получение монет за убийство сокланов")]
            public bool ClansBlockUse;
            [JsonProperty("Duel/Battles : Запретить получение монет за убийство на дуэлях")]
            public bool DuelBlockUse;
            [JsonProperty("MoscovOVH : Включить использование магазина(Должен быть включен обмен валют)")]
            public bool MoscovOvhUse;
            [JsonProperty("GameStores : Включить использование магазина(Должен быть включен обмен валют)")]
            public bool GameStoreshUse;
            [JsonProperty("GameStores : Настройки магазина GameStores")]
            public GameStores GameStoresSettings;
            [JsonProperty("IQChat : Настройки чата")]
            public ChatSetting ChatSettings;
            internal class ChatSetting
            {
                [JsonProperty("IQChat : Кастомный префикс в чате")]
                public string CustomPrefix;
                [JsonProperty("IQChat : Кастомный аватар в чате(Если требуется)")]
                public string CustomAvatar;
                [JsonProperty("IQChat : Использовать UI уведомления")]
                public bool UIAlertUse;
            }

            internal class GameStores
            {
                [JsonProperty("API Магазина(GameStores)")]
                public string GameStoresAPIStore;
                [JsonProperty("ID Магазина(GameStores)")]
                public string GameStoresIDStore;
                [JsonProperty("Сообщение в магазин при выдаче баланса(GameStores)")]
                public string GameStoresMessage;
            }

        }

        public static Configuration GetNewConfiguration();
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    [JsonProperty("Система экономики")]
    public Dictionary<ulong, InformationData> DataEconomics;
     void ReadData();
     void WriteData();
     void RegisteredDataUser(ulong player);
    public class InformationData
    {
        [JsonProperty("Баланс игрока")]
        public int Balance;
        [JsonProperty("Счетчик времени")]
        public int Time;
    }

    [ChatCommand("transfer")]
     void ChatCommandTransfer(BasePlayer player, string cmd, string[] arg);
    [ConsoleCommand("transfer")]
     void ConsoleCommandTransfer(ConsoleSystem.Arg args);
    [ConsoleCommand("iq.eco")]
     void IQEconomicCommandsAdmin(ConsoleSystem.Arg arg);
    public void TrackerTime();
    public void ConnectedPlayer(BasePlayer player);
    public void TransferPlayer(ulong userID, ulong transferUserID, int Balance);
    public void SetBalance(ulong userID, int SetBalance, ItemContainer cont);
    public void RemoveBalance(ulong userID, int RemoveBalance);
    public bool IsRare(int Rare);
    public bool IsData(ulong userID);
    public int GetBalance(ulong userID);
     Item CreateCustomMoney(int Amount);
    public bool IsRemoveBalance(ulong userID, int Amount);
    private BasePlayer FindPlayer(string nameOrId);
    public bool IsTransfer(int Balance);
     void Unload();
    private void OnServerInitialized();
     void OnPlayerConnected(BasePlayer player);
     void OnDispenserBonus(ResourceDispenser dispenser, BasePlayer player, Item item);
     void OnEntityDeath(BaseCombatEntity entity, HitInfo info);
    private Dictionary<uint, Dictionary<ulong, int>> HeliAttackers;
     void OnEntityTakeDamage(BaseCombatEntity victim, HitInfo info);
    private ulong GetLastAttacker(uint id);
    private Item OnItemSplit(Item item, int amount);
     object CanCombineDroppedItem(DroppedItem item, DroppedItem targetItem);
     object CanStackItem(Item item, Item targetItem);
    public static string UI_BALANCE_PARENT;
    public static string UI_CHANGER_PARENT;
    public void Interface_Balance(BasePlayer player);
    public void Interface_Changer(BasePlayer player);
    private new void LoadDefaultMessages();
    private static string HexToRustFormat(string hex);
    static DateTime epoch;
    static double CurrentTime();
     bool API_IS_USER(ulong userID);
     bool API_IS_REMOVED_BALANCE(ulong userID, int Amount);
     int API_GET_BALANCE(ulong userID);
     Item API_GET_ITEM(int Amount);
     string API_GET_MONEY_IL();
     string API_GET_STORES_IL();
     void API_SET_BALANCE(ulong userID, int Balance, ItemContainer itemContainer);
     void API_REMOVE_BALANCE(ulong userID, int Balance);
     void API_TRANSFERS(ulong userID, ulong trasferUserID, int Balance);
     bool API_MONEY_TYPE();
}

private class Configuration
{
    [JsonProperty("Монетки будут и гроков на руках - false / Иначе будет чат или интерфейсе - true")]
    public bool UseUI;
    [JsonProperty("Использовать UI интерфейс для отображения баланса(true - да(Если вы не поставили , чтобы монетки были на руках))/false - информация будет в чате по команде и при входе)")]
    public bool UseUIMoney;
    [JsonProperty("Включить обмен валюты на баланс в магазине(GameStores/MoscovOVH)")]
    public bool TransferStoreUse;
    [JsonProperty("Отображение интерфейса с балансом(true - отображает/false - скрывает)")]
    public bool ShowUI;
    [JsonProperty("Настройки обмена валют на баланс в магазине")]
    public TransferSetting TransferSettings;
    [JsonProperty("Основные настройки")]
    public GeneralSettings GeneralSetting;
    [JsonProperty("Настройки валюты(Если вид экономикик - false)")]
    public CustomMoney CustomMoneySetting;
    [JsonProperty("Настройки совместной работы с другими плагинами")]
    public ReferenceSetting ReferenceSettings;
    internal class CustomMoney
    {
        [JsonProperty("Название валюты")]
        public string DisplayName;
        [JsonProperty("Shortname монетки")]
        public string Shortname;
        [JsonProperty("SkinID монетки")]
        public ulong SkinID;
    }

    internal class GeneralSettings
    {
        [JsonProperty("Получение валюты за убийство игроков")]
        public bool BPlayerKillUse;
        [JsonProperty("Получение валюты за убийство животных")]
        public bool BPlayerAnimalUse;
        [JsonProperty("Получение валюты за убийство NPC")]
        public bool BPlayerNPCUse;
        [JsonProperty("Получение валюты за добычу ресурсов")]
        public bool BPlayerGatherUse;
        [JsonProperty("Получение валюты за уничтожение танка")]
        public bool BPlayerBradleyUse;
        [JsonProperty("Получение валюты за уничтожение вертолета")]
        public bool BPlayerHelicopterUse;
        [JsonProperty("Получение валюты за уничтожение бочек")]
        public bool BPLayerBarrelUse;
        [JsonProperty("Получение валюты проведенное время на сервере")]
        public bool BPLayerOnlineUse;
        [JsonProperty("Сколько нужно провести времени,чтобы выдали награду")]
        public int BPlayerOnlineTime;
        [JsonProperty("Сколько начислять валюты за проведенное время на сервере")]
        public int BPlayerOnlineGive;
        [JsonProperty("Настройка зачисления баланса за убийство игроков")]
        public AdvancedSetting BPlayerKillGive;
        [JsonProperty("Сколько начислять валюты за убийство животных")]
        public AdvancedSetting BPlayerAnimalGive;
        [JsonProperty("Сколько начислять валюты за убийство NPC")]
        public AdvancedSetting BPlayerNPCGive;
        [JsonProperty("Сколько начислять валюты за уничтожение танка")]
        public AdvancedSetting BPlayerBradleyGive;
        [JsonProperty("Сколько начислять валюты за уничтожение вертолета")]
        public AdvancedSetting BPlayerHelicopterGive;
        [JsonProperty("Сколько начислять валюты за уничтожение бочек")]
        public AdvancedSetting BPlayerBarrelGive;
        [JsonProperty("Сколько начислять валюты за добычу ресурсов ( [за какой ресурс давать] = { остальная настройка }")]
        public Dictionary<string,AdvancedSetting> BPlayerGatherGive;
        internal class AdvancedSetting
        {
            [JsonProperty("Шанс получить валюту")]
            public int Rare;
            [JsonProperty("Сколько выдавать валюты")]
            public int BPlayerGive;
        }

    }

    internal class TransferSetting
    {
        [JsonProperty("URL вашей монеты")]
        public string URLMoney;
        [JsonProperty("URL валюты для магазина")]
        public string URLStores;
        [JsonProperty("Сколько монет требуется для обмена")]
        public int MoneyCount;
        [JsonProperty("Сколько баланса получит игрок после обмена")]
        public int StoresMoneyCount;
    }

    internal class ReferenceSetting
    {
        [JsonProperty("Friends : Запретить получение монет за убийство друзей")]
        public bool FriendsBlockUse;
        [JsonProperty("Clans : Запретить получение монет за убийство сокланов")]
        public bool ClansBlockUse;
        [JsonProperty("Duel/Battles : Запретить получение монет за убийство на дуэлях")]
        public bool DuelBlockUse;
        [JsonProperty("MoscovOVH : Включить использование магазина(Должен быть включен обмен валют)")]
        public bool MoscovOvhUse;
        [JsonProperty("GameStores : Включить использование магазина(Должен быть включен обмен валют)")]
        public bool GameStoreshUse;
        [JsonProperty("GameStores : Настройки магазина GameStores")]
        public GameStores GameStoresSettings;
        [JsonProperty("IQChat : Настройки чата")]
        public ChatSetting ChatSettings;
        internal class ChatSetting
        {
            [JsonProperty("IQChat : Кастомный префикс в чате")]
            public string CustomPrefix;
            [JsonProperty("IQChat : Кастомный аватар в чате(Если требуется)")]
            public string CustomAvatar;
            [JsonProperty("IQChat : Использовать UI уведомления")]
            public bool UIAlertUse;
        }

        internal class GameStores
        {
            [JsonProperty("API Магазина(GameStores)")]
            public string GameStoresAPIStore;
            [JsonProperty("ID Магазина(GameStores)")]
            public string GameStoresIDStore;
            [JsonProperty("Сообщение в магазин при выдаче баланса(GameStores)")]
            public string GameStoresMessage;
        }

    }

    public static Configuration GetNewConfiguration();
}

internal class CustomMoney
{
    [JsonProperty("Название валюты")]
    public string DisplayName;
    [JsonProperty("Shortname монетки")]
    public string Shortname;
    [JsonProperty("SkinID монетки")]
    public ulong SkinID;
}

internal class GeneralSettings
{
    [JsonProperty("Получение валюты за убийство игроков")]
    public bool BPlayerKillUse;
    [JsonProperty("Получение валюты за убийство животных")]
    public bool BPlayerAnimalUse;
    [JsonProperty("Получение валюты за убийство NPC")]
    public bool BPlayerNPCUse;
    [JsonProperty("Получение валюты за добычу ресурсов")]
    public bool BPlayerGatherUse;
    [JsonProperty("Получение валюты за уничтожение танка")]
    public bool BPlayerBradleyUse;
    [JsonProperty("Получение валюты за уничтожение вертолета")]
    public bool BPlayerHelicopterUse;
    [JsonProperty("Получение валюты за уничтожение бочек")]
    public bool BPLayerBarrelUse;
    [JsonProperty("Получение валюты проведенное время на сервере")]
    public bool BPLayerOnlineUse;
    [JsonProperty("Сколько нужно провести времени,чтобы выдали награду")]
    public int BPlayerOnlineTime;
    [JsonProperty("Сколько начислять валюты за проведенное время на сервере")]
    public int BPlayerOnlineGive;
    [JsonProperty("Настройка зачисления баланса за убийство игроков")]
    public AdvancedSetting BPlayerKillGive;
    [JsonProperty("Сколько начислять валюты за убийство животных")]
    public AdvancedSetting BPlayerAnimalGive;
    [JsonProperty("Сколько начислять валюты за убийство NPC")]
    public AdvancedSetting BPlayerNPCGive;
    [JsonProperty("Сколько начислять валюты за уничтожение танка")]
    public AdvancedSetting BPlayerBradleyGive;
    [JsonProperty("Сколько начислять валюты за уничтожение вертолета")]
    public AdvancedSetting BPlayerHelicopterGive;
    [JsonProperty("Сколько начислять валюты за уничтожение бочек")]
    public AdvancedSetting BPlayerBarrelGive;
    [JsonProperty("Сколько начислять валюты за добычу ресурсов ( [за какой ресурс давать] = { остальная настройка }")]
    public Dictionary<string,AdvancedSetting> BPlayerGatherGive;
    internal class AdvancedSetting
    {
        [JsonProperty("Шанс получить валюту")]
        public int Rare;
        [JsonProperty("Сколько выдавать валюты")]
        public int BPlayerGive;
    }

}

internal class AdvancedSetting
{
    [JsonProperty("Шанс получить валюту")]
    public int Rare;
    [JsonProperty("Сколько выдавать валюты")]
    public int BPlayerGive;
}

internal class TransferSetting
{
    [JsonProperty("URL вашей монеты")]
    public string URLMoney;
    [JsonProperty("URL валюты для магазина")]
    public string URLStores;
    [JsonProperty("Сколько монет требуется для обмена")]
    public int MoneyCount;
    [JsonProperty("Сколько баланса получит игрок после обмена")]
    public int StoresMoneyCount;
}

internal class ReferenceSetting
{
    [JsonProperty("Friends : Запретить получение монет за убийство друзей")]
    public bool FriendsBlockUse;
    [JsonProperty("Clans : Запретить получение монет за убийство сокланов")]
    public bool ClansBlockUse;
    [JsonProperty("Duel/Battles : Запретить получение монет за убийство на дуэлях")]
    public bool DuelBlockUse;
    [JsonProperty("MoscovOVH : Включить использование магазина(Должен быть включен обмен валют)")]
    public bool MoscovOvhUse;
    [JsonProperty("GameStores : Включить использование магазина(Должен быть включен обмен валют)")]
    public bool GameStoreshUse;
    [JsonProperty("GameStores : Настройки магазина GameStores")]
    public GameStores GameStoresSettings;
    [JsonProperty("IQChat : Настройки чата")]
    public ChatSetting ChatSettings;
    internal class ChatSetting
    {
        [JsonProperty("IQChat : Кастомный префикс в чате")]
        public string CustomPrefix;
        [JsonProperty("IQChat : Кастомный аватар в чате(Если требуется)")]
        public string CustomAvatar;
        [JsonProperty("IQChat : Использовать UI уведомления")]
        public bool UIAlertUse;
    }

    internal class GameStores
    {
        [JsonProperty("API Магазина(GameStores)")]
        public string GameStoresAPIStore;
        [JsonProperty("ID Магазина(GameStores)")]
        public string GameStoresIDStore;
        [JsonProperty("Сообщение в магазин при выдаче баланса(GameStores)")]
        public string GameStoresMessage;
    }

}

internal class ChatSetting
{
    [JsonProperty("IQChat : Кастомный префикс в чате")]
    public string CustomPrefix;
    [JsonProperty("IQChat : Кастомный аватар в чате(Если требуется)")]
    public string CustomAvatar;
    [JsonProperty("IQChat : Использовать UI уведомления")]
    public bool UIAlertUse;
}

internal class GameStores
{
    [JsonProperty("API Магазина(GameStores)")]
    public string GameStoresAPIStore;
    [JsonProperty("ID Магазина(GameStores)")]
    public string GameStoresIDStore;
    [JsonProperty("Сообщение в магазин при выдаче баланса(GameStores)")]
    public string GameStoresMessage;
}

public class InformationData
{
    [JsonProperty("Баланс игрока")]
    public int Balance;
    [JsonProperty("Счетчик времени")]
    public int Time;
}


```

---

## IQEventSystem

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
[Info("IQEventSystem", "Mercury", "0.0.2")]
[Description("Не услышанный пионер Mercury")]
 class IQEventSystem : RustPlugin
{
    [PluginReference]
     Plugin IQChat;
    public bool EventStatus;
    public EventType EventLocal;
    public int LocalIndexEvent;
    public Dictionary<BasePlayer, int> EventPlayerList;
    private static Configuration config;
    private class Configuration
    {
        [JsonProperty("Настройка ивентов")]
        public List<Events> EventList;
        [JsonProperty("Настройка интерфейса")]
        public InterfaceSetting InterfaceSettings;
        [JsonProperty("Через сколько запуск случайный ивент(в секундах)")]
        public float TimeToStartEvent;
        [JsonProperty("Время ожидания регистрации игроков на ивент")]
        public float TimerVotesWait;
        [JsonProperty("Префикс в чате(IQChat)")]
        public string PrefixForChat;
        [JsonProperty("Минимум игроков для запуска ивента")]
        public int MinimumOnline;
        internal class Events
        {
            [JsonProperty("Тип ивента : 0 - Добыча, 1 - Поднять с пола, 2 - Найти в ящике, 3 - Убийство")]
            public EventType EventTypes;
            [JsonProperty("Время ивента ( в секундах )")]
            public float TimerEvent;
            [JsonProperty("Отображаемое имя")]
            public string DisplayName;
            [JsonProperty("Описание ивента")]
            public string Description;
            [JsonProperty("Цель ивента : Shortname")]
            public string Shortname;
            [JsonProperty("Цель ивента : SkinID (если не требуется,оставляйте 0)")]
            public ulong SkinID;
            [JsonProperty("Награда за победу в ивенте")]
            public List<Reward> Rewards;
            internal class Reward
            {
                [JsonProperty("Shortname")]
                public string Shortname;
                [JsonProperty("Команда")]
                public string Command;
                [JsonProperty("SkinID")]
                public ulong SkinID;
                [JsonProperty("Минимальное количество")]
                public int MinAmount;
                [JsonProperty("Максимальное количество")]
                public int MaxAmount;
            }

        }

        internal class InterfaceSetting
        {
            [JsonProperty("AnchorMin всей панели")]
            public string AnchorMin;
            [JsonProperty("AnchorMax всей панели")]
            public string AnchorMax;
            [JsonProperty("AnchorMin кнопки для участия")]
            public string AnchorMinVote;
            [JsonProperty("AnchorMax кнопки для участия")]
            public string AnchorMaxVote;
            [JsonProperty("Основной цвет")]
            public string MainColor;
            [JsonProperty("Дополнительный цвет")]
            public string TwoMainColor;
        }

        public static Configuration GetNewConfiguration();
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    public static string QIEVENT_PARENT;
    public static string QIEVENT_VOTE_PARENT;
    public void UIEventVotes(int IndexEvent);
    public void UIEvent(BasePlayer player, int IndexEvent);
    public void RefreshTimer(BasePlayer player, float Timer, int IndexEvent);
    private new void LoadDefaultMessages();
    private void OnServerInitialized();
     void OnDispenserGather(ResourceDispenser dispenser, BaseEntity entity, Item item);
     void OnDispenserBonus(ResourceDispenser dispenser, BasePlayer player, Item item);
     void OnEntityDeath(BaseCombatEntity entity, HitInfo info);
     void OnCollectiblePickup(Item item, BasePlayer player);
     void OnLootEntity(BasePlayer player, BaseEntity entity);
     void EventAutoStart();
    public int GetRandomEvent();
    public void WriteResult(BasePlayer player, int Amount);
    public void GiveReward(int IndexEvent);
    [ChatCommand("iqe")]
     void IQEventCommand(BasePlayer player, string cmd, string[] arg);
    [ConsoleCommand("iqe")]
     void IQECommandConsole(ConsoleSystem.Arg arg);
    private static string HexToRustFormat(string hex);
    static DateTime epoch;
    static double CurrentTime();
    public static string FormatTime(TimeSpan time);
    private static string Format(int units, string form1, string form2, string form3);
    public void SendChat(string Message, BasePlayer player, Chat.ChatChannel channel);
}

private class Configuration
{
    [JsonProperty("Настройка ивентов")]
    public List<Events> EventList;
    [JsonProperty("Настройка интерфейса")]
    public InterfaceSetting InterfaceSettings;
    [JsonProperty("Через сколько запуск случайный ивент(в секундах)")]
    public float TimeToStartEvent;
    [JsonProperty("Время ожидания регистрации игроков на ивент")]
    public float TimerVotesWait;
    [JsonProperty("Префикс в чате(IQChat)")]
    public string PrefixForChat;
    [JsonProperty("Минимум игроков для запуска ивента")]
    public int MinimumOnline;
    internal class Events
    {
        [JsonProperty("Тип ивента : 0 - Добыча, 1 - Поднять с пола, 2 - Найти в ящике, 3 - Убийство")]
        public EventType EventTypes;
        [JsonProperty("Время ивента ( в секундах )")]
        public float TimerEvent;
        [JsonProperty("Отображаемое имя")]
        public string DisplayName;
        [JsonProperty("Описание ивента")]
        public string Description;
        [JsonProperty("Цель ивента : Shortname")]
        public string Shortname;
        [JsonProperty("Цель ивента : SkinID (если не требуется,оставляйте 0)")]
        public ulong SkinID;
        [JsonProperty("Награда за победу в ивенте")]
        public List<Reward> Rewards;
        internal class Reward
        {
            [JsonProperty("Shortname")]
            public string Shortname;
            [JsonProperty("Команда")]
            public string Command;
            [JsonProperty("SkinID")]
            public ulong SkinID;
            [JsonProperty("Минимальное количество")]
            public int MinAmount;
            [JsonProperty("Максимальное количество")]
            public int MaxAmount;
        }

    }

    internal class InterfaceSetting
    {
        [JsonProperty("AnchorMin всей панели")]
        public string AnchorMin;
        [JsonProperty("AnchorMax всей панели")]
        public string AnchorMax;
        [JsonProperty("AnchorMin кнопки для участия")]
        public string AnchorMinVote;
        [JsonProperty("AnchorMax кнопки для участия")]
        public string AnchorMaxVote;
        [JsonProperty("Основной цвет")]
        public string MainColor;
        [JsonProperty("Дополнительный цвет")]
        public string TwoMainColor;
    }

    public static Configuration GetNewConfiguration();
}

internal class Events
{
    [JsonProperty("Тип ивента : 0 - Добыча, 1 - Поднять с пола, 2 - Найти в ящике, 3 - Убийство")]
    public EventType EventTypes;
    [JsonProperty("Время ивента ( в секундах )")]
    public float TimerEvent;
    [JsonProperty("Отображаемое имя")]
    public string DisplayName;
    [JsonProperty("Описание ивента")]
    public string Description;
    [JsonProperty("Цель ивента : Shortname")]
    public string Shortname;
    [JsonProperty("Цель ивента : SkinID (если не требуется,оставляйте 0)")]
    public ulong SkinID;
    [JsonProperty("Награда за победу в ивенте")]
    public List<Reward> Rewards;
    internal class Reward
    {
        [JsonProperty("Shortname")]
        public string Shortname;
        [JsonProperty("Команда")]
        public string Command;
        [JsonProperty("SkinID")]
        public ulong SkinID;
        [JsonProperty("Минимальное количество")]
        public int MinAmount;
        [JsonProperty("Максимальное количество")]
        public int MaxAmount;
    }

}

internal class Reward
{
    [JsonProperty("Shortname")]
    public string Shortname;
    [JsonProperty("Команда")]
    public string Command;
    [JsonProperty("SkinID")]
    public ulong SkinID;
    [JsonProperty("Минимальное количество")]
    public int MinAmount;
    [JsonProperty("Максимальное количество")]
    public int MaxAmount;
}

internal class InterfaceSetting
{
    [JsonProperty("AnchorMin всей панели")]
    public string AnchorMin;
    [JsonProperty("AnchorMax всей панели")]
    public string AnchorMax;
    [JsonProperty("AnchorMin кнопки для участия")]
    public string AnchorMinVote;
    [JsonProperty("AnchorMax кнопки для участия")]
    public string AnchorMaxVote;
    [JsonProperty("Основной цвет")]
    public string MainColor;
    [JsonProperty("Дополнительный цвет")]
    public string TwoMainColor;
}


```

---

## IQFakeActive

```csharp
using System;
using System.Collections.Generic;
using UnityEngine;
using Newtonsoft.Json;
using Oxide.Core.Plugins;
using ConVar;
using System.Linq;
using Oxide.Core;
using System.Collections;
using Newtonsoft.Json.Linq;
using System.Text;

Oxide.Plugins
[Info("IQFakeActive", "SkuliDropek", "1.2.15")]
[Description("Актив вашего сервера, но немного не тот :)")]
 class IQFakeActive : RustPlugin
{
    public int FakeOnline;
    public static DateTime TimeCreatedSave;
    public static DateTime RealTime;
    public static int SaveCreated;
    private Timer timeActivateMessage;
    private Timer timeActivateMessagePm;
    private Timer timerSynh;
    private Timer timerGenerateOnline;
    private Timer timerPlaySounds;
    private Coroutine RoutineInitPlugin;
    private Coroutine RoutineAddAvatars;
    private List<Configuration.ActiveSettings.SounActiveSettings.Sounds> SortedSoundList;
    [PluginReference]
     Plugin IQChat;
     Plugin ImageLibrary;
    private string GetInfoIQChat(IQChatGetType TypeInfo);
    public bool AddImage(string url, string shortname, ulong skin);
    public bool HasImage(string imageName);
    private static Configuration config;
    private class Configuration
    {
        [JsonProperty("Настройка создания фейковых игроков")]
        public FakePlayerSettings FakePlayers;
        [JsonProperty("Настройка актива")]
        public ActiveSettings FakeActive;
        [JsonProperty("Настройка онлайна")]
        public FakeOnlineSettings FakeOnline;
        [JsonProperty("Общая настройка")]
        public GeneralSettings GeneralSetting;
        [JsonProperty("Включить лоигрование действий плагина в консоль")]
        public bool UseLogConsole;
        [JsonProperty("Введите ваш стимКлюч для подгрузки аватарок(https://steamcommunity.com/dev/apikey - взять тут.Если потребует домен на сайте - введите абсолютно любой)")]
        public string APIKeySteam;
        internal class GeneralSettings
        {
            [JsonProperty("IQChat : Steam64ID для аватарки в чате")]
            public String AvatarSteamID;
            [JsonProperty("IQChat : Отображаемый префикс в чате")]
            public String PrefixName;
            [JsonProperty("Максимально допустимый предел фейкового онлайна(если вам не нужно это значение, оставьвте 0 - по умолчанию)")]
            public Int32 MaximalOnline;
        }

        internal class FakeOnlineSettings
        {
            [JsonProperty("Настройка интервала обновления кол-во онлайна(сек)")]
            public int IntervalUpdateOnline;
            [JsonProperty("Детальная настройка типов онлайна")]
            public UpdateOnline SettingsUpdateOnline;
            internal class UpdateOnline
            {
                [JsonProperty("Настройка типа онлайна (0 - Автоматический, 1 - Ручная настройка, 2 - Автоматический + ваш онлайн)")]
                public TypeOnline TypeOnline;
                [JsonProperty("Настройка обновления онлайна")]
                public StandartFormul StandartFormulSetting;
                [JsonProperty("Ручная настройка онлайна")]
                public ManualFormul ManualFormule;
                internal class StandartFormul
                {
                    [JsonProperty("Минимальный множитель онлайна(От этого показателя зависит скачок онлайна при обновлении)")]
                    public float MinimumFactor;
                    [JsonProperty("Максимальный множитель онлайна(От этого показателя зависит скачок онлайна при обновлении)")]
                    public float MaximumFactor;
                    [JsonProperty("Включить зависимость генерации оналйна от времени суток?")]
                    public bool DayTimeGerenation;
                }

                internal class ManualFormul
                {
                    [JsonProperty("Ручная настройка онлайна (будет к вашему онлайну добавлять указанный в списке) | [время(цифра)] = количество онлайна ")]
                    public Dictionary<Int32, Int32> ManualTimeOnline;
                }

            }

        }

        internal class FakePlayerSettings
        {
            [JsonProperty("Использовать игроков с общей базы игроков(true - да/false - нет, вы сами будете задавать параметры)")]
            public bool PlayersDB;
            [JsonProperty("Использовать сообщение с общей базы игроков(true - да/false - нет, вы сами будете задавать параметры)")]
            public bool ChatsDB;
            [JsonProperty("Локальный - список ников с которыми будут создаваться игроки(Общая база игроков должна быть отключена)")]
            public List<string> ListNickName;
            [JsonProperty("Локальный - список сообщений которые будут отправляться в чат(Общая база игроков должна быть отключена)")]
            public List<string> ListMessages;
        }

        internal class ActiveSettings
        {
            [JsonProperty("Настройка актива в чате")]
            public ChatActiveSetting ChatActive;
            [JsonProperty("Настройка иммитации актива с помощью звуков(будь то рейд, будь то кто-то ходит рядом или добывает)")]
            public SounActiveSettings SoundActive;
            internal class SounActiveSettings
            {
                [JsonProperty("Использовать звуки?")]
                public bool UseLocalSoundBase;
                [JsonProperty("Минимальный интервал проигрывания звука")]
                public int MinimumIntervalSound;
                [JsonProperty("Максимальный интервал проигрывания звука")]
                public int MaximumIntervalSound;
                [JsonProperty("Локальный лист звуков и их настройка")]
                public List<Sounds> SoundLists;
                public class Sounds
                {
                    [JsonProperty("Ваш звук")]
                    public string SoundPath;
                    [JsonProperty("Минимальная позиция от игрока")]
                    public int MinPos;
                    [JsonProperty("Максимальная позиция от игрока")]
                    public int MaxPos;
                    [JsonProperty("Шанс проигрывания данного звука")]
                    public int Rare;
                    [JsonProperty("На какой день после WIPE будет отыгрываться данный звук")]
                    public int DayFaktor;
                }

            }

            internal class ChatActiveSetting
            {
                [JsonProperty("HEX : Цвет ника для ботов")]
                public String ColorChatNickDefault;
                [JsonProperty("IQChat : Настройки подключения и отключения для IQChat")]
                public IQChatNetwork IQChatNetworkSetting;
                [JsonProperty("IQChat : Настройки личных сообщений для IQChat")]
                public IQChatPM IQChatPMSettings;
                [JsonProperty("Использовать черный список слов")]
                public bool UseBlackList;
                [JsonProperty("Укажите слова,которые будут запрещены в чате")]
                public List<string> BlackList;
                [JsonProperty("Укажите минимальный интервал отправки сообщения в чат(секунды)")]
                public int MinimumInterval;
                [JsonProperty("Укажите максимальный интервал отправки сообщения в чат(секунды)")]
                public int MaximumInterval;
                internal class IQChatNetwork
                {
                    [JsonProperty("IQChat : Использовать подключение/отключение в чате")]
                    public bool UseNetwork;
                    [JsonProperty("IQChat : Список стран для подключения")]
                    public List<string> CountryListConnected;
                    [JsonProperty("IQChat : Список причин отсоединения от сервера")]
                    public List<string> ReasonListDisconnected;
                }

                internal class IQChatPM
                {
                    [JsonProperty("IQChat : Использовать случайное сообщение в ЛС")]
                    public bool UseRandomPM;
                    [JsonProperty("IQChat : Список случайных сообщений в ЛС")]
                    public List<string> PMListMessage;
                    [JsonProperty("Укажите минимальный интервал отправки сообщения в ЛС(секунды)")]
                    public int MinimumInterval;
                    [JsonProperty("Укажите максимальный интервал отправки сообщения в ЛС(секунды)")]
                    public int MaximumInterval;
                }

            }

        }

        public static Configuration GetNewConfiguration();
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    public void GeneratedOnline();
    public List<FakePlayer> ReservedPlayer;
    public List<FakePlayer> FakePlayerList;
    public List<Messages> FakeMessageList;
    public class Messages
    {
        public string Message;
    }

    public class FakePlayer
    {
        public String UserID;
        public string DisplayName;
    }

     void SyncReserved();
    private void GeneratedAll();
    private void GenerateSounds();
    private ulong GeneratedSteam64ID();
    private string GeneratedNickName();
    private void GeneratedPlayer();
    private void GeneratedMessage();
    private void DumpPlayers(BasePlayer player);
    internal class MessageToJson
    {
        public String Message;
    }

    private void DumpChat(string Message);
    private void GetPlayerDB();
    private void GetMessageDB();
    public IEnumerator AddPlayerAvatar();
    private FakePlayer GetFake();
    public bool IsRare(int Rare);
    private void StartChat();
    private void ActivateMessageChatPM();
    private void ActivateMessageChat();
    public void SendMessage(BasePlayer player, String Message);
    public void ChatNetworkConnected(BasePlayer player);
    public void ChatNetworkDisconnected(BasePlayer player);
    public void SendRandomPM();
    public string GetMessage();
    public string GetCountry();
    public string GetReason();
    public string GetPM();
    private Configuration.ActiveSettings.SounActiveSettings.Sounds GetSound();
     void StartSoundEffects();
    private void PlaySoundEffects();
     void ShowFakeOnline(BasePlayer player);
    private void OnServerInitialized();
    public IEnumerator InitializePlugin();
     void Unload();
     object OnPlayerChat(BasePlayer player, string message, Chat.ChatChannel channel);
     void OnPlayerConnected(BasePlayer player);
    [ChatCommand("online")]
     void ChatCommandShowOnline(BasePlayer player);
    [ChatCommand("players")]
     void ChatCommandShowPlayers(BasePlayer player);
    [ConsoleCommand("iqfa")]
     void IQFakeActiveCommand(ConsoleSystem.Arg arg);
     bool IsFake(ulong userID);
     bool IsFake(string DisplayName);
     int GetOnline();
     ulong GetFakeIDRandom();
     string GetFakeNameRandom();
     string FindFakeName(ulong ID);
     int DayWipe();
     void RemoveReserver(UInt64 ID);
    private new void LoadDefaultMessages();
    public static StringBuilder sb;
    public String GetLang(String LangKey, String userID, object[] args);
}

private class Configuration
{
    [JsonProperty("Настройка создания фейковых игроков")]
    public FakePlayerSettings FakePlayers;
    [JsonProperty("Настройка актива")]
    public ActiveSettings FakeActive;
    [JsonProperty("Настройка онлайна")]
    public FakeOnlineSettings FakeOnline;
    [JsonProperty("Общая настройка")]
    public GeneralSettings GeneralSetting;
    [JsonProperty("Включить лоигрование действий плагина в консоль")]
    public bool UseLogConsole;
    [JsonProperty("Введите ваш стимКлюч для подгрузки аватарок(https://steamcommunity.com/dev/apikey - взять тут.Если потребует домен на сайте - введите абсолютно любой)")]
    public string APIKeySteam;
    internal class GeneralSettings
    {
        [JsonProperty("IQChat : Steam64ID для аватарки в чате")]
        public String AvatarSteamID;
        [JsonProperty("IQChat : Отображаемый префикс в чате")]
        public String PrefixName;
        [JsonProperty("Максимально допустимый предел фейкового онлайна(если вам не нужно это значение, оставьвте 0 - по умолчанию)")]
        public Int32 MaximalOnline;
    }

    internal class FakeOnlineSettings
    {
        [JsonProperty("Настройка интервала обновления кол-во онлайна(сек)")]
        public int IntervalUpdateOnline;
        [JsonProperty("Детальная настройка типов онлайна")]
        public UpdateOnline SettingsUpdateOnline;
        internal class UpdateOnline
        {
            [JsonProperty("Настройка типа онлайна (0 - Автоматический, 1 - Ручная настройка, 2 - Автоматический + ваш онлайн)")]
            public TypeOnline TypeOnline;
            [JsonProperty("Настройка обновления онлайна")]
            public StandartFormul StandartFormulSetting;
            [JsonProperty("Ручная настройка онлайна")]
            public ManualFormul ManualFormule;
            internal class StandartFormul
            {
                [JsonProperty("Минимальный множитель онлайна(От этого показателя зависит скачок онлайна при обновлении)")]
                public float MinimumFactor;
                [JsonProperty("Максимальный множитель онлайна(От этого показателя зависит скачок онлайна при обновлении)")]
                public float MaximumFactor;
                [JsonProperty("Включить зависимость генерации оналйна от времени суток?")]
                public bool DayTimeGerenation;
            }

            internal class ManualFormul
            {
                [JsonProperty("Ручная настройка онлайна (будет к вашему онлайну добавлять указанный в списке) | [время(цифра)] = количество онлайна ")]
                public Dictionary<Int32, Int32> ManualTimeOnline;
            }

        }

    }

    internal class FakePlayerSettings
    {
        [JsonProperty("Использовать игроков с общей базы игроков(true - да/false - нет, вы сами будете задавать параметры)")]
        public bool PlayersDB;
        [JsonProperty("Использовать сообщение с общей базы игроков(true - да/false - нет, вы сами будете задавать параметры)")]
        public bool ChatsDB;
        [JsonProperty("Локальный - список ников с которыми будут создаваться игроки(Общая база игроков должна быть отключена)")]
        public List<string> ListNickName;
        [JsonProperty("Локальный - список сообщений которые будут отправляться в чат(Общая база игроков должна быть отключена)")]
        public List<string> ListMessages;
    }

    internal class ActiveSettings
    {
        [JsonProperty("Настройка актива в чате")]
        public ChatActiveSetting ChatActive;
        [JsonProperty("Настройка иммитации актива с помощью звуков(будь то рейд, будь то кто-то ходит рядом или добывает)")]
        public SounActiveSettings SoundActive;
        internal class SounActiveSettings
        {
            [JsonProperty("Использовать звуки?")]
            public bool UseLocalSoundBase;
            [JsonProperty("Минимальный интервал проигрывания звука")]
            public int MinimumIntervalSound;
            [JsonProperty("Максимальный интервал проигрывания звука")]
            public int MaximumIntervalSound;
            [JsonProperty("Локальный лист звуков и их настройка")]
            public List<Sounds> SoundLists;
            public class Sounds
            {
                [JsonProperty("Ваш звук")]
                public string SoundPath;
                [JsonProperty("Минимальная позиция от игрока")]
                public int MinPos;
                [JsonProperty("Максимальная позиция от игрока")]
                public int MaxPos;
                [JsonProperty("Шанс проигрывания данного звука")]
                public int Rare;
                [JsonProperty("На какой день после WIPE будет отыгрываться данный звук")]
                public int DayFaktor;
            }

        }

        internal class ChatActiveSetting
        {
            [JsonProperty("HEX : Цвет ника для ботов")]
            public String ColorChatNickDefault;
            [JsonProperty("IQChat : Настройки подключения и отключения для IQChat")]
            public IQChatNetwork IQChatNetworkSetting;
            [JsonProperty("IQChat : Настройки личных сообщений для IQChat")]
            public IQChatPM IQChatPMSettings;
            [JsonProperty("Использовать черный список слов")]
            public bool UseBlackList;
            [JsonProperty("Укажите слова,которые будут запрещены в чате")]
            public List<string> BlackList;
            [JsonProperty("Укажите минимальный интервал отправки сообщения в чат(секунды)")]
            public int MinimumInterval;
            [JsonProperty("Укажите максимальный интервал отправки сообщения в чат(секунды)")]
            public int MaximumInterval;
            internal class IQChatNetwork
            {
                [JsonProperty("IQChat : Использовать подключение/отключение в чате")]
                public bool UseNetwork;
                [JsonProperty("IQChat : Список стран для подключения")]
                public List<string> CountryListConnected;
                [JsonProperty("IQChat : Список причин отсоединения от сервера")]
                public List<string> ReasonListDisconnected;
            }

            internal class IQChatPM
            {
                [JsonProperty("IQChat : Использовать случайное сообщение в ЛС")]
                public bool UseRandomPM;
                [JsonProperty("IQChat : Список случайных сообщений в ЛС")]
                public List<string> PMListMessage;
                [JsonProperty("Укажите минимальный интервал отправки сообщения в ЛС(секунды)")]
                public int MinimumInterval;
                [JsonProperty("Укажите максимальный интервал отправки сообщения в ЛС(секунды)")]
                public int MaximumInterval;
            }

        }

    }

    public static Configuration GetNewConfiguration();
}

internal class GeneralSettings
{
    [JsonProperty("IQChat : Steam64ID для аватарки в чате")]
    public String AvatarSteamID;
    [JsonProperty("IQChat : Отображаемый префикс в чате")]
    public String PrefixName;
    [JsonProperty("Максимально допустимый предел фейкового онлайна(если вам не нужно это значение, оставьвте 0 - по умолчанию)")]
    public Int32 MaximalOnline;
}

internal class FakeOnlineSettings
{
    [JsonProperty("Настройка интервала обновления кол-во онлайна(сек)")]
    public int IntervalUpdateOnline;
    [JsonProperty("Детальная настройка типов онлайна")]
    public UpdateOnline SettingsUpdateOnline;
    internal class UpdateOnline
    {
        [JsonProperty("Настройка типа онлайна (0 - Автоматический, 1 - Ручная настройка, 2 - Автоматический + ваш онлайн)")]
        public TypeOnline TypeOnline;
        [JsonProperty("Настройка обновления онлайна")]
        public StandartFormul StandartFormulSetting;
        [JsonProperty("Ручная настройка онлайна")]
        public ManualFormul ManualFormule;
        internal class StandartFormul
        {
            [JsonProperty("Минимальный множитель онлайна(От этого показателя зависит скачок онлайна при обновлении)")]
            public float MinimumFactor;
            [JsonProperty("Максимальный множитель онлайна(От этого показателя зависит скачок онлайна при обновлении)")]
            public float MaximumFactor;
            [JsonProperty("Включить зависимость генерации оналйна от времени суток?")]
            public bool DayTimeGerenation;
        }

        internal class ManualFormul
        {
            [JsonProperty("Ручная настройка онлайна (будет к вашему онлайну добавлять указанный в списке) | [время(цифра)] = количество онлайна ")]
            public Dictionary<Int32, Int32> ManualTimeOnline;
        }

    }

}

internal class UpdateOnline
{
    [JsonProperty("Настройка типа онлайна (0 - Автоматический, 1 - Ручная настройка, 2 - Автоматический + ваш онлайн)")]
    public TypeOnline TypeOnline;
    [JsonProperty("Настройка обновления онлайна")]
    public StandartFormul StandartFormulSetting;
    [JsonProperty("Ручная настройка онлайна")]
    public ManualFormul ManualFormule;
    internal class StandartFormul
    {
        [JsonProperty("Минимальный множитель онлайна(От этого показателя зависит скачок онлайна при обновлении)")]
        public float MinimumFactor;
        [JsonProperty("Максимальный множитель онлайна(От этого показателя зависит скачок онлайна при обновлении)")]
        public float MaximumFactor;
        [JsonProperty("Включить зависимость генерации оналйна от времени суток?")]
        public bool DayTimeGerenation;
    }

    internal class ManualFormul
    {
        [JsonProperty("Ручная настройка онлайна (будет к вашему онлайну добавлять указанный в списке) | [время(цифра)] = количество онлайна ")]
        public Dictionary<Int32, Int32> ManualTimeOnline;
    }

}

internal class StandartFormul
{
    [JsonProperty("Минимальный множитель онлайна(От этого показателя зависит скачок онлайна при обновлении)")]
    public float MinimumFactor;
    [JsonProperty("Максимальный множитель онлайна(От этого показателя зависит скачок онлайна при обновлении)")]
    public float MaximumFactor;
    [JsonProperty("Включить зависимость генерации оналйна от времени суток?")]
    public bool DayTimeGerenation;
}

internal class ManualFormul
{
    [JsonProperty("Ручная настройка онлайна (будет к вашему онлайну добавлять указанный в списке) | [время(цифра)] = количество онлайна ")]
    public Dictionary<Int32, Int32> ManualTimeOnline;
}

internal class FakePlayerSettings
{
    [JsonProperty("Использовать игроков с общей базы игроков(true - да/false - нет, вы сами будете задавать параметры)")]
    public bool PlayersDB;
    [JsonProperty("Использовать сообщение с общей базы игроков(true - да/false - нет, вы сами будете задавать параметры)")]
    public bool ChatsDB;
    [JsonProperty("Локальный - список ников с которыми будут создаваться игроки(Общая база игроков должна быть отключена)")]
    public List<string> ListNickName;
    [JsonProperty("Локальный - список сообщений которые будут отправляться в чат(Общая база игроков должна быть отключена)")]
    public List<string> ListMessages;
}

internal class ActiveSettings
{
    [JsonProperty("Настройка актива в чате")]
    public ChatActiveSetting ChatActive;
    [JsonProperty("Настройка иммитации актива с помощью звуков(будь то рейд, будь то кто-то ходит рядом или добывает)")]
    public SounActiveSettings SoundActive;
    internal class SounActiveSettings
    {
        [JsonProperty("Использовать звуки?")]
        public bool UseLocalSoundBase;
        [JsonProperty("Минимальный интервал проигрывания звука")]
        public int MinimumIntervalSound;
        [JsonProperty("Максимальный интервал проигрывания звука")]
        public int MaximumIntervalSound;
        [JsonProperty("Локальный лист звуков и их настройка")]
        public List<Sounds> SoundLists;
        public class Sounds
        {
            [JsonProperty("Ваш звук")]
            public string SoundPath;
            [JsonProperty("Минимальная позиция от игрока")]
            public int MinPos;
            [JsonProperty("Максимальная позиция от игрока")]
            public int MaxPos;
            [JsonProperty("Шанс проигрывания данного звука")]
            public int Rare;
            [JsonProperty("На какой день после WIPE будет отыгрываться данный звук")]
            public int DayFaktor;
        }

    }

    internal class ChatActiveSetting
    {
        [JsonProperty("HEX : Цвет ника для ботов")]
        public String ColorChatNickDefault;
        [JsonProperty("IQChat : Настройки подключения и отключения для IQChat")]
        public IQChatNetwork IQChatNetworkSetting;
        [JsonProperty("IQChat : Настройки личных сообщений для IQChat")]
        public IQChatPM IQChatPMSettings;
        [JsonProperty("Использовать черный список слов")]
        public bool UseBlackList;
        [JsonProperty("Укажите слова,которые будут запрещены в чате")]
        public List<string> BlackList;
        [JsonProperty("Укажите минимальный интервал отправки сообщения в чат(секунды)")]
        public int MinimumInterval;
        [JsonProperty("Укажите максимальный интервал отправки сообщения в чат(секунды)")]
        public int MaximumInterval;
        internal class IQChatNetwork
        {
            [JsonProperty("IQChat : Использовать подключение/отключение в чате")]
            public bool UseNetwork;
            [JsonProperty("IQChat : Список стран для подключения")]
            public List<string> CountryListConnected;
            [JsonProperty("IQChat : Список причин отсоединения от сервера")]
            public List<string> ReasonListDisconnected;
        }

        internal class IQChatPM
        {
            [JsonProperty("IQChat : Использовать случайное сообщение в ЛС")]
            public bool UseRandomPM;
            [JsonProperty("IQChat : Список случайных сообщений в ЛС")]
            public List<string> PMListMessage;
            [JsonProperty("Укажите минимальный интервал отправки сообщения в ЛС(секунды)")]
            public int MinimumInterval;
            [JsonProperty("Укажите максимальный интервал отправки сообщения в ЛС(секунды)")]
            public int MaximumInterval;
        }

    }

}

internal class SounActiveSettings
{
    [JsonProperty("Использовать звуки?")]
    public bool UseLocalSoundBase;
    [JsonProperty("Минимальный интервал проигрывания звука")]
    public int MinimumIntervalSound;
    [JsonProperty("Максимальный интервал проигрывания звука")]
    public int MaximumIntervalSound;
    [JsonProperty("Локальный лист звуков и их настройка")]
    public List<Sounds> SoundLists;
    public class Sounds
    {
        [JsonProperty("Ваш звук")]
        public string SoundPath;
        [JsonProperty("Минимальная позиция от игрока")]
        public int MinPos;
        [JsonProperty("Максимальная позиция от игрока")]
        public int MaxPos;
        [JsonProperty("Шанс проигрывания данного звука")]
        public int Rare;
        [JsonProperty("На какой день после WIPE будет отыгрываться данный звук")]
        public int DayFaktor;
    }

}

public class Sounds
{
    [JsonProperty("Ваш звук")]
    public string SoundPath;
    [JsonProperty("Минимальная позиция от игрока")]
    public int MinPos;
    [JsonProperty("Максимальная позиция от игрока")]
    public int MaxPos;
    [JsonProperty("Шанс проигрывания данного звука")]
    public int Rare;
    [JsonProperty("На какой день после WIPE будет отыгрываться данный звук")]
    public int DayFaktor;
}

internal class ChatActiveSetting
{
    [JsonProperty("HEX : Цвет ника для ботов")]
    public String ColorChatNickDefault;
    [JsonProperty("IQChat : Настройки подключения и отключения для IQChat")]
    public IQChatNetwork IQChatNetworkSetting;
    [JsonProperty("IQChat : Настройки личных сообщений для IQChat")]
    public IQChatPM IQChatPMSettings;
    [JsonProperty("Использовать черный список слов")]
    public bool UseBlackList;
    [JsonProperty("Укажите слова,которые будут запрещены в чате")]
    public List<string> BlackList;
    [JsonProperty("Укажите минимальный интервал отправки сообщения в чат(секунды)")]
    public int MinimumInterval;
    [JsonProperty("Укажите максимальный интервал отправки сообщения в чат(секунды)")]
    public int MaximumInterval;
    internal class IQChatNetwork
    {
        [JsonProperty("IQChat : Использовать подключение/отключение в чате")]
        public bool UseNetwork;
        [JsonProperty("IQChat : Список стран для подключения")]
        public List<string> CountryListConnected;
        [JsonProperty("IQChat : Список причин отсоединения от сервера")]
        public List<string> ReasonListDisconnected;
    }

    internal class IQChatPM
    {
        [JsonProperty("IQChat : Использовать случайное сообщение в ЛС")]
        public bool UseRandomPM;
        [JsonProperty("IQChat : Список случайных сообщений в ЛС")]
        public List<string> PMListMessage;
        [JsonProperty("Укажите минимальный интервал отправки сообщения в ЛС(секунды)")]
        public int MinimumInterval;
        [JsonProperty("Укажите максимальный интервал отправки сообщения в ЛС(секунды)")]
        public int MaximumInterval;
    }

}

internal class IQChatNetwork
{
    [JsonProperty("IQChat : Использовать подключение/отключение в чате")]
    public bool UseNetwork;
    [JsonProperty("IQChat : Список стран для подключения")]
    public List<string> CountryListConnected;
    [JsonProperty("IQChat : Список причин отсоединения от сервера")]
    public List<string> ReasonListDisconnected;
}

internal class IQChatPM
{
    [JsonProperty("IQChat : Использовать случайное сообщение в ЛС")]
    public bool UseRandomPM;
    [JsonProperty("IQChat : Список случайных сообщений в ЛС")]
    public List<string> PMListMessage;
    [JsonProperty("Укажите минимальный интервал отправки сообщения в ЛС(секунды)")]
    public int MinimumInterval;
    [JsonProperty("Укажите максимальный интервал отправки сообщения в ЛС(секунды)")]
    public int MaximumInterval;
}

public class Messages
{
    public string Message;
}

public class FakePlayer
{
    public String UserID;
    public string DisplayName;
}

internal class MessageToJson
{
    public String Message;
}


```

---

## IQFestivalEvents

```csharp
using Newtonsoft.Json;
using Newtonsoft.Json.Serialization;
using System.Collections.Generic;
using System.Linq;
using UnityEngine;
using System;
using System.Collections;
using Oxide.Core.Plugins;
using System.Text;

Oxide.Plugins
[Info("IQFestivalEvents", "TopPlugin.ru", "0.0.5")]
public class IQFestivalEvents : RustPlugin
{
    [PluginReference]
     Plugin IQChat;
    public void SendChat(string Message, BasePlayer player, ConVar.Chat.ChatChannel channel);
    private Dictionary<BaseEntity, EffectComponent> EffectsComponent;
    public Dictionary<String, Coroutine> RoutineList;
    public static IQFestivalEvents _;
    private Dictionary<String, List<BaseEntity>> EventStarted;
    private const String PermissionStartEvent;
    private const String PermissionStopEvent;
    private static Configuration config;
    private class Configuration
    {
        [JsonProperty("IQ Chat : Custom prefix in the chat")]
        public String CustomPrefix;
        [JsonProperty("IQChat : Custom avatar in the chat (If required)")]
        public String CustomAvatar;
        [JsonProperty("IQChat : Use UI notifications")]
        public Boolean UIAlertUse;
        [JsonProperty("Notification in the chat about the start of the event (true - enabled / false - disabled)")]
        public Boolean UseAlertPlayer;
        [JsonProperty("List of events and their settings : [Unique name] = Setup")]
        public Dictionary<String, EventSetting> EventList;
        public class EventSetting
        {
            [JsonProperty("Enable the event (true - yes/false - no)")]
            public Boolean TurnedEvent;
            [JsonProperty("When to launch an event online (For example: 10 (it will start if there are >= 10 players on the server)")]
            public Int32 OnlineStart;
            [JsonProperty("The distance of the spawn Entity at this event (For example: The Entity, subject to all conditions, will spawn at a distance of more than 5 meters from each other)")]
            public Int32 DistanceSpawnEntity;
            [JsonProperty("A message at the start of the event")]
            public String StartMessageEvent;
            [JsonProperty("After how long to start the event (Cyclically, i.e., for example, every 300 seconds)")]
            public Int32 StartTimeEvent;
            [JsonProperty("The list of monuments on which to compare the event")]
            public List<String> MonumentsSpawn;
            [JsonProperty("Setting up an Entity at an event")]
            public SettingEntity SettingEntitys;
            internal class SettingEntity
            {
                [JsonProperty("Effect for playback. If the effect is not needed, leave the field empty (if you do not know what it is, do not touch this item)")]
                public String EffectPath;
                [JsonProperty("Setting the amount of Spawn Entity")]
                public CountSetting CountSettings;
                [JsonProperty("Shortname for Spawn Entity (multiple can be used, they are randomly selected)")]
                public List<String> ShortnameListEntity;
                [JsonProperty("X-damage by Entity on the monument")]
                public Single DamageEntity;
                [JsonProperty("Setting up drop-down items")]
                public List<DropItems> DropItemList;
            }

            internal class DropItems
            {
                [JsonProperty("The type of item to drop out: 0 - Item, 1 - Drawing (Do not forget to specify the Shortname in the paragraph below)")]
                public ItemType TypeItem;
                [JsonProperty("Shortname")]
                public String Shortname;
                [JsonProperty("SkinID")]
                public UInt64 SkinID;
                [JsonProperty("Display name (If left blank, it will be standard)")]
                public String DisplayName;
                [JsonProperty("Setting up the dropout of items")]
                public DropSettings DropsSetting;
                internal class DropSettings
                {
                    [JsonProperty("Setting up the quantity")]
                    public CountSetting CountSettings;
                    [JsonProperty("Chance of falling out (0.0 - 100.0)")]
                    public Single RareDrop;
                }

                public Item CrateItem();
            }

        }

        internal class CountSetting
        {
            [JsonProperty("Minimum amount")]
            public Int32 MinCount;
            [JsonProperty("Maximum amount")]
            public Int32 MaxCount;
        }

        [JsonProperty("List of available monuments (When changing the map, they may change according to availability")]
        public List<Monument> Monuments;
        internal class Monument
        {
            public String Name;
            public Vector3 Position;
        }

        public static Configuration GetNewConfiguration();
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
     void OnServerInitialized();
     void OnEntityDeath(BaseEntity entity, HitInfo info);
    private object OnEntityTakeDamage(BaseCombatEntity entity, HitInfo info);
     void Unload();
     void RegisteredPermissions();
    static float GetGroundPosition(Vector3 pos);
    static Vector3 RandomCircle(Vector3 center, float radius);
    private IEnumerator StartEvent(String KeyEvent);
     void EndEvent(String EventKey);
    [ChatCommand("iqfe")]
     void CommandIQFEChat(BasePlayer player, String cmd, String[] arg);
    [ConsoleCommand("iqfe")]
     void CommandIQFEConsole(ConsoleSystem.Arg arg);
    private void StartEventUser(BasePlayer player, String KeyEvent);
    private void StopEventUser(BasePlayer player, String KeyEvent);
    public class EffectComponent : FacepunchBehaviour
    {
        public String EventKey;
        private String EffectPath;
        private BaseEntity entity;
         void Awake();
        public void Initialize(String EventKey);
         void EffectSpawn();
        public void DropItems(Vector3 DropPosition);
         void OnDestroy();
    }

    private void DestroyAll();
    private StringBuilder sb;
    public string GetLang(string LangKey, string userID, object[] args);
    private new void LoadDefaultMessages();
}

private class Configuration
{
    [JsonProperty("IQ Chat : Custom prefix in the chat")]
    public String CustomPrefix;
    [JsonProperty("IQChat : Custom avatar in the chat (If required)")]
    public String CustomAvatar;
    [JsonProperty("IQChat : Use UI notifications")]
    public Boolean UIAlertUse;
    [JsonProperty("Notification in the chat about the start of the event (true - enabled / false - disabled)")]
    public Boolean UseAlertPlayer;
    [JsonProperty("List of events and their settings : [Unique name] = Setup")]
    public Dictionary<String, EventSetting> EventList;
    public class EventSetting
    {
        [JsonProperty("Enable the event (true - yes/false - no)")]
        public Boolean TurnedEvent;
        [JsonProperty("When to launch an event online (For example: 10 (it will start if there are >= 10 players on the server)")]
        public Int32 OnlineStart;
        [JsonProperty("The distance of the spawn Entity at this event (For example: The Entity, subject to all conditions, will spawn at a distance of more than 5 meters from each other)")]
        public Int32 DistanceSpawnEntity;
        [JsonProperty("A message at the start of the event")]
        public String StartMessageEvent;
        [JsonProperty("After how long to start the event (Cyclically, i.e., for example, every 300 seconds)")]
        public Int32 StartTimeEvent;
        [JsonProperty("The list of monuments on which to compare the event")]
        public List<String> MonumentsSpawn;
        [JsonProperty("Setting up an Entity at an event")]
        public SettingEntity SettingEntitys;
        internal class SettingEntity
        {
            [JsonProperty("Effect for playback. If the effect is not needed, leave the field empty (if you do not know what it is, do not touch this item)")]
            public String EffectPath;
            [JsonProperty("Setting the amount of Spawn Entity")]
            public CountSetting CountSettings;
            [JsonProperty("Shortname for Spawn Entity (multiple can be used, they are randomly selected)")]
            public List<String> ShortnameListEntity;
            [JsonProperty("X-damage by Entity on the monument")]
            public Single DamageEntity;
            [JsonProperty("Setting up drop-down items")]
            public List<DropItems> DropItemList;
        }

        internal class DropItems
        {
            [JsonProperty("The type of item to drop out: 0 - Item, 1 - Drawing (Do not forget to specify the Shortname in the paragraph below)")]
            public ItemType TypeItem;
            [JsonProperty("Shortname")]
            public String Shortname;
            [JsonProperty("SkinID")]
            public UInt64 SkinID;
            [JsonProperty("Display name (If left blank, it will be standard)")]
            public String DisplayName;
            [JsonProperty("Setting up the dropout of items")]
            public DropSettings DropsSetting;
            internal class DropSettings
            {
                [JsonProperty("Setting up the quantity")]
                public CountSetting CountSettings;
                [JsonProperty("Chance of falling out (0.0 - 100.0)")]
                public Single RareDrop;
            }

            public Item CrateItem();
        }

    }

    internal class CountSetting
    {
        [JsonProperty("Minimum amount")]
        public Int32 MinCount;
        [JsonProperty("Maximum amount")]
        public Int32 MaxCount;
    }

    [JsonProperty("List of available monuments (When changing the map, they may change according to availability")]
    public List<Monument> Monuments;
    internal class Monument
    {
        public String Name;
        public Vector3 Position;
    }

    public static Configuration GetNewConfiguration();
}

public class EventSetting
{
    [JsonProperty("Enable the event (true - yes/false - no)")]
    public Boolean TurnedEvent;
    [JsonProperty("When to launch an event online (For example: 10 (it will start if there are >= 10 players on the server)")]
    public Int32 OnlineStart;
    [JsonProperty("The distance of the spawn Entity at this event (For example: The Entity, subject to all conditions, will spawn at a distance of more than 5 meters from each other)")]
    public Int32 DistanceSpawnEntity;
    [JsonProperty("A message at the start of the event")]
    public String StartMessageEvent;
    [JsonProperty("After how long to start the event (Cyclically, i.e., for example, every 300 seconds)")]
    public Int32 StartTimeEvent;
    [JsonProperty("The list of monuments on which to compare the event")]
    public List<String> MonumentsSpawn;
    [JsonProperty("Setting up an Entity at an event")]
    public SettingEntity SettingEntitys;
    internal class SettingEntity
    {
        [JsonProperty("Effect for playback. If the effect is not needed, leave the field empty (if you do not know what it is, do not touch this item)")]
        public String EffectPath;
        [JsonProperty("Setting the amount of Spawn Entity")]
        public CountSetting CountSettings;
        [JsonProperty("Shortname for Spawn Entity (multiple can be used, they are randomly selected)")]
        public List<String> ShortnameListEntity;
        [JsonProperty("X-damage by Entity on the monument")]
        public Single DamageEntity;
        [JsonProperty("Setting up drop-down items")]
        public List<DropItems> DropItemList;
    }

    internal class DropItems
    {
        [JsonProperty("The type of item to drop out: 0 - Item, 1 - Drawing (Do not forget to specify the Shortname in the paragraph below)")]
        public ItemType TypeItem;
        [JsonProperty("Shortname")]
        public String Shortname;
        [JsonProperty("SkinID")]
        public UInt64 SkinID;
        [JsonProperty("Display name (If left blank, it will be standard)")]
        public String DisplayName;
        [JsonProperty("Setting up the dropout of items")]
        public DropSettings DropsSetting;
        internal class DropSettings
        {
            [JsonProperty("Setting up the quantity")]
            public CountSetting CountSettings;
            [JsonProperty("Chance of falling out (0.0 - 100.0)")]
            public Single RareDrop;
        }

        public Item CrateItem();
    }

}

internal class SettingEntity
{
    [JsonProperty("Effect for playback. If the effect is not needed, leave the field empty (if you do not know what it is, do not touch this item)")]
    public String EffectPath;
    [JsonProperty("Setting the amount of Spawn Entity")]
    public CountSetting CountSettings;
    [JsonProperty("Shortname for Spawn Entity (multiple can be used, they are randomly selected)")]
    public List<String> ShortnameListEntity;
    [JsonProperty("X-damage by Entity on the monument")]
    public Single DamageEntity;
    [JsonProperty("Setting up drop-down items")]
    public List<DropItems> DropItemList;
}

internal class DropItems
{
    [JsonProperty("The type of item to drop out: 0 - Item, 1 - Drawing (Do not forget to specify the Shortname in the paragraph below)")]
    public ItemType TypeItem;
    [JsonProperty("Shortname")]
    public String Shortname;
    [JsonProperty("SkinID")]
    public UInt64 SkinID;
    [JsonProperty("Display name (If left blank, it will be standard)")]
    public String DisplayName;
    [JsonProperty("Setting up the dropout of items")]
    public DropSettings DropsSetting;
    internal class DropSettings
    {
        [JsonProperty("Setting up the quantity")]
        public CountSetting CountSettings;
        [JsonProperty("Chance of falling out (0.0 - 100.0)")]
        public Single RareDrop;
    }

    public Item CrateItem();
}

internal class DropSettings
{
    [JsonProperty("Setting up the quantity")]
    public CountSetting CountSettings;
    [JsonProperty("Chance of falling out (0.0 - 100.0)")]
    public Single RareDrop;
}

internal class CountSetting
{
    [JsonProperty("Minimum amount")]
    public Int32 MinCount;
    [JsonProperty("Maximum amount")]
    public Int32 MaxCount;
}

internal class Monument
{
    public String Name;
    public Vector3 Position;
}

public class EffectComponent : FacepunchBehaviour
{
    public String EventKey;
    private String EffectPath;
    private BaseEntity entity;
     void Awake();
    public void Initialize(String EventKey);
     void EffectSpawn();
    public void DropItems(Vector3 DropPosition);
     void OnDestroy();
}


```

---

## IQFireSpear

```csharp
using System.Collections.Generic;
using ConVar;
using Newtonsoft.Json;
using Oxide.Core.Plugins;

Oxide.Plugins
[Info("IQFireSpear", "Sempai#3239", "0.0.1")]
[Description("Огненное копье,при ударе есть шанс оставить искру,при броске поджигает под собой все")]
 class IQFireSpear : RustPlugin
{
    [PluginReference]
     Plugin IQChat;
    private static Configuration config;
    private class Configuration
    {
        [JsonProperty("При поджигании копья,ломать его постепенно(true/false)")]
        public bool ConditionUse;
        [JsonProperty("SkinID для предмета(Пример : 2000653461)")]
        public ulong SkinID;
        [JsonProperty("DisplayName для предмета")]
        public string DisplayName;
        [JsonProperty("Shortname для предмета(обязательно , которое можно кинуть и нанести урон)")]
        public string Shortname;
        [JsonProperty("Шанс возгарания при ударе копьем в игрока")]
        public int RareFireDamagePlayer;
        [JsonProperty("Шанс возгарания при броске копья")]
        public int RareFireThrow;
        public static Configuration GetNewConfiguration();
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
     void OnPlayerAttack(BasePlayer attacker, HitInfo info);
    private new void LoadDefaultMessages();
    public void SendChat(string Message, BasePlayer player, Chat.ChatChannel channel);
}

private class Configuration
{
    [JsonProperty("При поджигании копья,ломать его постепенно(true/false)")]
    public bool ConditionUse;
    [JsonProperty("SkinID для предмета(Пример : 2000653461)")]
    public ulong SkinID;
    [JsonProperty("DisplayName для предмета")]
    public string DisplayName;
    [JsonProperty("Shortname для предмета(обязательно , которое можно кинуть и нанести урон)")]
    public string Shortname;
    [JsonProperty("Шанс возгарания при ударе копьем в игрока")]
    public int RareFireDamagePlayer;
    [JsonProperty("Шанс возгарания при броске копья")]
    public int RareFireThrow;
    public static Configuration GetNewConfiguration();
}


```

---

## IQGradeRemove

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Text.RegularExpressions;
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using UnityEngine;

Oxide.Plugins
[Info("IQGradeRemove", "Mercury", "0.1.5")]
[Description("Умный Grade Remove")]
 class IQGradeRemove : RustPlugin
{
    public static String PermissionGRMenu;
    public static String PermissionGRNoResource;
    public readonly Dictionary<Int32, String> StatusLevels;
    public readonly Dictionary<Int32, String> StatusLevelsAllGrades;
    public readonly Dictionary<Int32, String> SoundLevelsGrade;
    public static readonly Dictionary<Int32, String> PermissionsLevel;
    static Int32 CurrentTime();
    public static String FormatTime(TimeSpan time);
    private static String Format(Int32 units, String form1, String form2, String form3);
    [PluginReference]
     Plugin Friends;
    public Boolean IsRaidBlocked(BasePlayer player);
    public Boolean IsFriends(UInt64 userID, UInt64 targetID);
     void RegisteredPermissions();
    private static Configuration config;
    private class Configuration
    {
        [JsonProperty("Настройка удаления объектов")]
        public RemoveSettings RemoveSetting;
        [JsonProperty("Настройка улучшения объектов")]
        public GradeSettings GradeSetting;
        [JsonProperty("Настройка интерфейса")]
        public InterfaceSettings InterfaceSetting;
        [JsonProperty("Использовать пермишенсы для включения определенного UP или ремува(смотрите в описании плагина, там указаны права)")]
        public bool UsePermission;
        internal class RemoveSettings
        {
            [JsonProperty("Настройка возврата предметов после удаления")]
            public ReturnedSettings ReturnedSetting;
            [JsonProperty("Настройка удаления через время")]
            public TimedSettings TimedSetting;
            [JsonProperty("Настройка запретов")]
            public SettingsBlocks SettingsBlock;
            [JsonProperty("Время действия удаления")]
            public int RemoveTime;
            internal class ReturnedSettings
            {
                [JsonProperty("Возвращать ресурсы за удаление строений?(true- да/false - нет)")]
                public bool UseReturnedResource;
                [JsonProperty("Процент возврата ресурсов за удаление строений?(true- да/false - нет)")]
                public int PercentReturnRecource;
                [JsonProperty("Возвращать все предметы при удалении?(true- да/false - нет)")]
                public bool UseAllowedReturned;
                [JsonProperty("Снижать состояние предмета при возврате?Эффект будто он поднял его через RUST систему")]
                public bool UseDamageReturned;
                [JsonProperty("Предметы,которые не возвращаются при удалении(Shortname)")]
                public List<string> ShortnameNoteReturned;
            }

            internal class TimedSettings
            {
                [JsonProperty("Включить полный запрет на удаление объекта через N время(Пример : Через 3 часа после постройки,его нельзя будет удалить вообще)")]
                public bool UseAllBlock;
                [JsonProperty("Через сколько нельзя будет удалять постройку вообще")]
                public int TimeAllBlock;
                [JsonProperty("Кастомный список предметов,которые нельзя будет удалить через время по правам. [[IQGradeRemove.NAME]] - Время(в сек)")]
                public Dictionary<string, int> ItemsTimesAllPermissions;
                [JsonProperty("Использовать запрет на удаление постройки на время(После постройки объекта,его N количество времени нельзя будет удалить)")]
                public bool UseTimesBlock;
                [JsonProperty("Через сколько можно будет удалять постройку(если включено)")]
                public int TimesBlock;
                [JsonProperty("Кастомный список предметов,которые можно будет удалить через время по правам. [[IQGradeRemove.NAME]] - Время(в сек)")]
                public Dictionary<string, int> ItemsTimesPermissions;
            }

            internal class SettingsBlocks
            {
                [JsonProperty("[NoEscape] Запретить удаление во время рейдблока(true - да/false - нет)")]
                public bool NoEscape;
                [JsonProperty("[Friends] Удалять постройки могут только друзья(Иначе все,кто есть в шкафу)(true - да/false - нет)")]
                public bool Friends;
                [JsonProperty("Предметы,которые нельзя удалить(Shortname)")]
                public List<string> ShortnameNoteReturned;
            }

        }

        internal class GradeSettings
        {
            [JsonProperty("Время действия улучшения")]
            public int GradeTime;
            [JsonProperty("Разрешить обратное улучшение?(Пример : МВК стенку откатить в деревянную)(true - да/false - нет)")]
            public bool UseBackUp;
            [JsonProperty("Настройка запретов")]
            public SettingsBlocks SettingsBlock;
            internal class SettingsBlocks
            {
                [JsonProperty("[NoEscape] Запретить улучшение во время рейдблока(true - да/false - нет)")]
                public bool NoEscape;
            }

        }

        internal class InterfaceSettings
        {
            [JsonProperty("Настройка интерфейса")]
            public MainInterface MainInterfaces;
            internal class MainInterface
            {
                [JsonProperty("Скрывать интерфейс по истечению таймера")]
                public Boolean HideMenuTimer;
                [JsonProperty("Отображать уровни улучшений в интерфейсе")]
                public Boolean ShowLevelUp;
                [JsonProperty("Отображать кнопку закрыть в меню")]
                public Boolean ShowCloseButton;
                [JsonProperty("Символ для кнопки закрыть")]
                public String SymbolCloseButton;
                [JsonProperty("Цвет панели(HEX)")]
                public string ColorPanel;
                [JsonProperty("Цвет текста(HEX)")]
                public string ColorText;
            }

        }

        public static Configuration GetNewConfiguration();
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    public Dictionary<ulong, GradeRemove> DataPlayer;
    public Dictionary<uint, int> BuildingRemoveTimers;
    public Dictionary<uint, int> BuildingRemoveBlock;
     void ReadData();
     void WriteData();
    public class GradeRemove
    {
        public int ActiveTime;
        public int GradeLevel;
        public Boolean GradeAllObject;
        public Timer TimerEvent;
        public void GradeUP(BasePlayer player, bool UseMyGrade, int CustomGrade);
        public void RebootTimer();
    }

     void RegisteredUser(BasePlayer player);
    public String GradeErrorParse(BasePlayer player, BuildingBlock buildingBlock, BuildingGrade.Enum grade);
    public void GradeBuilding(BasePlayer player, BuildingBlock buildingBlock);
    public void GradeAll(BasePlayer player, BuildingBlock buildingBlock);
    private void AddRemoveBlockBuild(UInt32 netID, UInt64 userID);
    private Boolean IsBlockAvailablePermanent(UInt32 netID);
    private Boolean IsBlockBuildPemanent(UInt32 netID);
    private Int32 GetTimeBlockPermanent(UInt64 userID);
    private void AddBlockBuild(UInt32 netID, UInt64 userID);
    private Boolean IsBlockAvailable(UInt32 netID);
    private Boolean IsBlockBuild(UInt32 netID);
    private Int32 GetTimeBlock(UInt64 userID);
    private Int32 GetTimerBlock(UInt32 netID);
     void ReturnedRemoveItems(BasePlayer player, BaseEntity buildingBlock);
    public void RemoveAll(BasePlayer player, BaseEntity buildingBlock);
    public string RemoveErrorParseBuilding(BasePlayer player, BaseEntity buildingBlock);
    public void RemoveBuilding(BasePlayer player, BaseEntity buildingBlock);
    [ConsoleCommand("up")]
     void UPCommandConsole(ConsoleSystem.Arg arg);
    [ConsoleCommand("building.upgrade")]
     void BUpgradeCommandConsole(ConsoleSystem.Arg arg);
    [ConsoleCommand("remove")]
     void RemoveCommandConsole(ConsoleSystem.Arg arg);
    [ChatCommand("up")]
     void UPCommand(BasePlayer player, string cmd, string[] arg);
    [ChatCommand("bgrade")]
     void BGradeChatCommand(BasePlayer player, string cmd, string[] arg);
    [ChatCommand("remove")]
     void RemoveCommand(BasePlayer player, string cmd, string[] arg);
    [ConsoleCommand("gr.func.turned")]
     void UI_ConsoleCommandAdminMenu(ConsoleSystem.Arg arg);
     void OnHammerHit(BasePlayer player, HitInfo info);
     object OnStructureUpgrade(BaseCombatEntity entity, BasePlayer player, BuildingGrade.Enum grade);
    private void OnEntityBuilt(Planner plan, GameObject go);
     void OnPlayerDeath(BasePlayer player, HitInfo info);
    public static IQGradeRemove inst;
     void Init();
    private void OnServerInitialized();
     void OnPlayerConnected(BasePlayer player);
     void Unload();
     void OnNewSave(string filename);
    public static String GradeRemoveOverlay;
    [ChatCommand("grade")]
     void Interface_New_Main(BasePlayer player);
     void Update_Take_Button_UP(BasePlayer player);
     void Update_Take_Button_REMOVE(BasePlayer player);
     void GradeRemove_Status(BasePlayer player);
     void GradeRemove_AllObject_Turned(BasePlayer player);
     void UpdateButton_Upgrade(BasePlayer player);
     void UpdateLabelStatus(BasePlayer player);
     void Interface_Error(BasePlayer player, String Message);
    private static string HexToRustFormat(string hex);
    public static StringBuilder sb;
    public string GetLang(string LangKey, string userID, object[] args);
    private new void LoadDefaultMessages();
}

private class Configuration
{
    [JsonProperty("Настройка удаления объектов")]
    public RemoveSettings RemoveSetting;
    [JsonProperty("Настройка улучшения объектов")]
    public GradeSettings GradeSetting;
    [JsonProperty("Настройка интерфейса")]
    public InterfaceSettings InterfaceSetting;
    [JsonProperty("Использовать пермишенсы для включения определенного UP или ремува(смотрите в описании плагина, там указаны права)")]
    public bool UsePermission;
    internal class RemoveSettings
    {
        [JsonProperty("Настройка возврата предметов после удаления")]
        public ReturnedSettings ReturnedSetting;
        [JsonProperty("Настройка удаления через время")]
        public TimedSettings TimedSetting;
        [JsonProperty("Настройка запретов")]
        public SettingsBlocks SettingsBlock;
        [JsonProperty("Время действия удаления")]
        public int RemoveTime;
        internal class ReturnedSettings
        {
            [JsonProperty("Возвращать ресурсы за удаление строений?(true- да/false - нет)")]
            public bool UseReturnedResource;
            [JsonProperty("Процент возврата ресурсов за удаление строений?(true- да/false - нет)")]
            public int PercentReturnRecource;
            [JsonProperty("Возвращать все предметы при удалении?(true- да/false - нет)")]
            public bool UseAllowedReturned;
            [JsonProperty("Снижать состояние предмета при возврате?Эффект будто он поднял его через RUST систему")]
            public bool UseDamageReturned;
            [JsonProperty("Предметы,которые не возвращаются при удалении(Shortname)")]
            public List<string> ShortnameNoteReturned;
        }

        internal class TimedSettings
        {
            [JsonProperty("Включить полный запрет на удаление объекта через N время(Пример : Через 3 часа после постройки,его нельзя будет удалить вообще)")]
            public bool UseAllBlock;
            [JsonProperty("Через сколько нельзя будет удалять постройку вообще")]
            public int TimeAllBlock;
            [JsonProperty("Кастомный список предметов,которые нельзя будет удалить через время по правам. [[IQGradeRemove.NAME]] - Время(в сек)")]
            public Dictionary<string, int> ItemsTimesAllPermissions;
            [JsonProperty("Использовать запрет на удаление постройки на время(После постройки объекта,его N количество времени нельзя будет удалить)")]
            public bool UseTimesBlock;
            [JsonProperty("Через сколько можно будет удалять постройку(если включено)")]
            public int TimesBlock;
            [JsonProperty("Кастомный список предметов,которые можно будет удалить через время по правам. [[IQGradeRemove.NAME]] - Время(в сек)")]
            public Dictionary<string, int> ItemsTimesPermissions;
        }

        internal class SettingsBlocks
        {
            [JsonProperty("[NoEscape] Запретить удаление во время рейдблока(true - да/false - нет)")]
            public bool NoEscape;
            [JsonProperty("[Friends] Удалять постройки могут только друзья(Иначе все,кто есть в шкафу)(true - да/false - нет)")]
            public bool Friends;
            [JsonProperty("Предметы,которые нельзя удалить(Shortname)")]
            public List<string> ShortnameNoteReturned;
        }

    }

    internal class GradeSettings
    {
        [JsonProperty("Время действия улучшения")]
        public int GradeTime;
        [JsonProperty("Разрешить обратное улучшение?(Пример : МВК стенку откатить в деревянную)(true - да/false - нет)")]
        public bool UseBackUp;
        [JsonProperty("Настройка запретов")]
        public SettingsBlocks SettingsBlock;
        internal class SettingsBlocks
        {
            [JsonProperty("[NoEscape] Запретить улучшение во время рейдблока(true - да/false - нет)")]
            public bool NoEscape;
        }

    }

    internal class InterfaceSettings
    {
        [JsonProperty("Настройка интерфейса")]
        public MainInterface MainInterfaces;
        internal class MainInterface
        {
            [JsonProperty("Скрывать интерфейс по истечению таймера")]
            public Boolean HideMenuTimer;
            [JsonProperty("Отображать уровни улучшений в интерфейсе")]
            public Boolean ShowLevelUp;
            [JsonProperty("Отображать кнопку закрыть в меню")]
            public Boolean ShowCloseButton;
            [JsonProperty("Символ для кнопки закрыть")]
            public String SymbolCloseButton;
            [JsonProperty("Цвет панели(HEX)")]
            public string ColorPanel;
            [JsonProperty("Цвет текста(HEX)")]
            public string ColorText;
        }

    }

    public static Configuration GetNewConfiguration();
}

internal class RemoveSettings
{
    [JsonProperty("Настройка возврата предметов после удаления")]
    public ReturnedSettings ReturnedSetting;
    [JsonProperty("Настройка удаления через время")]
    public TimedSettings TimedSetting;
    [JsonProperty("Настройка запретов")]
    public SettingsBlocks SettingsBlock;
    [JsonProperty("Время действия удаления")]
    public int RemoveTime;
    internal class ReturnedSettings
    {
        [JsonProperty("Возвращать ресурсы за удаление строений?(true- да/false - нет)")]
        public bool UseReturnedResource;
        [JsonProperty("Процент возврата ресурсов за удаление строений?(true- да/false - нет)")]
        public int PercentReturnRecource;
        [JsonProperty("Возвращать все предметы при удалении?(true- да/false - нет)")]
        public bool UseAllowedReturned;
        [JsonProperty("Снижать состояние предмета при возврате?Эффект будто он поднял его через RUST систему")]
        public bool UseDamageReturned;
        [JsonProperty("Предметы,которые не возвращаются при удалении(Shortname)")]
        public List<string> ShortnameNoteReturned;
    }

    internal class TimedSettings
    {
        [JsonProperty("Включить полный запрет на удаление объекта через N время(Пример : Через 3 часа после постройки,его нельзя будет удалить вообще)")]
        public bool UseAllBlock;
        [JsonProperty("Через сколько нельзя будет удалять постройку вообще")]
        public int TimeAllBlock;
        [JsonProperty("Кастомный список предметов,которые нельзя будет удалить через время по правам. [[IQGradeRemove.NAME]] - Время(в сек)")]
        public Dictionary<string, int> ItemsTimesAllPermissions;
        [JsonProperty("Использовать запрет на удаление постройки на время(После постройки объекта,его N количество времени нельзя будет удалить)")]
        public bool UseTimesBlock;
        [JsonProperty("Через сколько можно будет удалять постройку(если включено)")]
        public int TimesBlock;
        [JsonProperty("Кастомный список предметов,которые можно будет удалить через время по правам. [[IQGradeRemove.NAME]] - Время(в сек)")]
        public Dictionary<string, int> ItemsTimesPermissions;
    }

    internal class SettingsBlocks
    {
        [JsonProperty("[NoEscape] Запретить удаление во время рейдблока(true - да/false - нет)")]
        public bool NoEscape;
        [JsonProperty("[Friends] Удалять постройки могут только друзья(Иначе все,кто есть в шкафу)(true - да/false - нет)")]
        public bool Friends;
        [JsonProperty("Предметы,которые нельзя удалить(Shortname)")]
        public List<string> ShortnameNoteReturned;
    }

}

internal class ReturnedSettings
{
    [JsonProperty("Возвращать ресурсы за удаление строений?(true- да/false - нет)")]
    public bool UseReturnedResource;
    [JsonProperty("Процент возврата ресурсов за удаление строений?(true- да/false - нет)")]
    public int PercentReturnRecource;
    [JsonProperty("Возвращать все предметы при удалении?(true- да/false - нет)")]
    public bool UseAllowedReturned;
    [JsonProperty("Снижать состояние предмета при возврате?Эффект будто он поднял его через RUST систему")]
    public bool UseDamageReturned;
    [JsonProperty("Предметы,которые не возвращаются при удалении(Shortname)")]
    public List<string> ShortnameNoteReturned;
}

internal class TimedSettings
{
    [JsonProperty("Включить полный запрет на удаление объекта через N время(Пример : Через 3 часа после постройки,его нельзя будет удалить вообще)")]
    public bool UseAllBlock;
    [JsonProperty("Через сколько нельзя будет удалять постройку вообще")]
    public int TimeAllBlock;
    [JsonProperty("Кастомный список предметов,которые нельзя будет удалить через время по правам. [[IQGradeRemove.NAME]] - Время(в сек)")]
    public Dictionary<string, int> ItemsTimesAllPermissions;
    [JsonProperty("Использовать запрет на удаление постройки на время(После постройки объекта,его N количество времени нельзя будет удалить)")]
    public bool UseTimesBlock;
    [JsonProperty("Через сколько можно будет удалять постройку(если включено)")]
    public int TimesBlock;
    [JsonProperty("Кастомный список предметов,которые можно будет удалить через время по правам. [[IQGradeRemove.NAME]] - Время(в сек)")]
    public Dictionary<string, int> ItemsTimesPermissions;
}

internal class SettingsBlocks
{
    [JsonProperty("[NoEscape] Запретить удаление во время рейдблока(true - да/false - нет)")]
    public bool NoEscape;
    [JsonProperty("[Friends] Удалять постройки могут только друзья(Иначе все,кто есть в шкафу)(true - да/false - нет)")]
    public bool Friends;
    [JsonProperty("Предметы,которые нельзя удалить(Shortname)")]
    public List<string> ShortnameNoteReturned;
}

internal class GradeSettings
{
    [JsonProperty("Время действия улучшения")]
    public int GradeTime;
    [JsonProperty("Разрешить обратное улучшение?(Пример : МВК стенку откатить в деревянную)(true - да/false - нет)")]
    public bool UseBackUp;
    [JsonProperty("Настройка запретов")]
    public SettingsBlocks SettingsBlock;
    internal class SettingsBlocks
    {
        [JsonProperty("[NoEscape] Запретить улучшение во время рейдблока(true - да/false - нет)")]
        public bool NoEscape;
    }

}

internal class SettingsBlocks
{
    [JsonProperty("[NoEscape] Запретить улучшение во время рейдблока(true - да/false - нет)")]
    public bool NoEscape;
}

internal class InterfaceSettings
{
    [JsonProperty("Настройка интерфейса")]
    public MainInterface MainInterfaces;
    internal class MainInterface
    {
        [JsonProperty("Скрывать интерфейс по истечению таймера")]
        public Boolean HideMenuTimer;
        [JsonProperty("Отображать уровни улучшений в интерфейсе")]
        public Boolean ShowLevelUp;
        [JsonProperty("Отображать кнопку закрыть в меню")]
        public Boolean ShowCloseButton;
        [JsonProperty("Символ для кнопки закрыть")]
        public String SymbolCloseButton;
        [JsonProperty("Цвет панели(HEX)")]
        public string ColorPanel;
        [JsonProperty("Цвет текста(HEX)")]
        public string ColorText;
    }

}

internal class MainInterface
{
    [JsonProperty("Скрывать интерфейс по истечению таймера")]
    public Boolean HideMenuTimer;
    [JsonProperty("Отображать уровни улучшений в интерфейсе")]
    public Boolean ShowLevelUp;
    [JsonProperty("Отображать кнопку закрыть в меню")]
    public Boolean ShowCloseButton;
    [JsonProperty("Символ для кнопки закрыть")]
    public String SymbolCloseButton;
    [JsonProperty("Цвет панели(HEX)")]
    public string ColorPanel;
    [JsonProperty("Цвет текста(HEX)")]
    public string ColorText;
}

public class GradeRemove
{
    public int ActiveTime;
    public int GradeLevel;
    public Boolean GradeAllObject;
    public Timer TimerEvent;
    public void GradeUP(BasePlayer player, bool UseMyGrade, int CustomGrade);
    public void RebootTimer();
}


```

---

## IQHeadReward

```csharp
using System;
using System.Collections.Generic;
using ConVar;
using Newtonsoft.Json;
using Oxide.Core.Plugins;
using UnityEngine;
using Oxide.Game.Rust.Cui;
using System.Linq;
using System.Text;
using System.Text.RegularExpressions;
using System.Collections;
using Oxide.Core;

Oxide.Plugins
[Info("IQHeadReward", "SkuliDropek", "1.0.9")]
[Description("Reward your heads")]
 class IQHeadReward : RustPlugin
{
    private const Boolean LanguageRu;
    public const String PermissionImmunitete;
    public Dictionary<BaseEntity, CustomMapMarker> MapMarkers;
    static Double CurrentTime();
     String PrefabStash;
     String PrefabBarricade;
    [PluginReference]
     Plugin ImageLibrary;
     Plugin IQChat;
     Plugin IQEconomic;
     Plugin IQPlagueSkill;
     Plugin Friends;
     Plugin Clans;
     Plugin Battles;
     Plugin Duel;
     Plugin Duelist;
     Plugin IQRankSystem;
    private String GetImage(String fileName, UInt64 skin);
    public Boolean AddImage(String url, String shortname, UInt64 skin);
    public void SendImage(BasePlayer player, String imageName, UInt64 imageId);
    public Boolean HasImage(String imageName);
    public Boolean IsFriends(UInt64 userID, UInt64 targetID);
    private bool IsClans(String userID, String targetID);
    public Boolean IsDuel(UInt64 userID);
    private Dictionary<UInt64, List<UInt64>> WantedTeams;
    private Boolean IsTeamController(BasePlayer Wanted);
    private void TeamControllerAdd(BasePlayer Wanted);
    private void TeamControllerRemove(BasePlayer Wanted);
    public String IQEcoMoney { get; set; }
    public void IQEconomicSetBalance(UInt64 userID, Int32 Balance);
     Int32 IQEconomicGetBalance(UInt64 userID);
     void IQEconomicRemoveBalance(UInt64 userID, Int32 Balance);
    public Boolean IQPlagueSkillUse(BasePlayer player);
    public void SendChat(BasePlayer player, String Message, Chat.ChatChannel channel);
    public Boolean IsRaidBlocked(BasePlayer player);
     Boolean IsRank(UInt64 userID, String Key);
     String GetRankName(String Key);
    private Dictionary<UInt64, HeadTask> PrePlayerHeads;
    private Dictionary<UInt64, List<Configuration.Settings.ItemList>> DistributionPlayerReturned;
    [JsonProperty(LanguageRu ? "Задания на головы"  :  "Tasks for heads")]
    private List<HeadTask> PlayerHeads;
    public class HeadTask
    {
        public String WantedName;
        public UInt64 WantedID;
        public Double Cooldown;
        public Dictionary<UInt64, List<Configuration.Settings.ItemList>> RewardList;
        public Double CooldownItem;
    }

     void ReadData();
     void WriteData();
    private static Configuration config;
    public class Configuration
    {
        [JsonProperty("Настройка плагина | Settings plugin")]
        public Settings Setting;
        internal class Settings
        {
            [JsonProperty("Настройки IQChat | Settings IQChat")]
            public ChatSettings ChatSetting;
            [JsonProperty("Настройки IQRankSystem | Settings IQRankSystem")]
            public IQRankSystem IQRankSystemSetting;
            [JsonProperty("Настройки UI уведомления | Notification UI Settings")]
            public AlertUI AlertUISetting;
            [JsonProperty("Настройка создания объявления за голову игроками | Setting up the creation of ads for the head of players")]
            public CustomCreated CustomCreatedSetting;
            [JsonProperty("Настройка метки на карте для игрока | Setting up a placemark on the map for the player")]
            public MapMark mapMark;
            internal class IQRankSystem
            {
                [JsonProperty("Использовать IQRankSystem [true-да/false-нет] (При наличии ранга игрок сможет создавать объявления на голову игрока) | Use IQRankSystem [true-yes/false-no] (If there is a rank, the player will be able to create ads on the player's head)")]
                public Boolean UseRank;
                [JsonProperty("Впишите ключ ранга, который требуется для разблокировки возможности | Enter the rank key that is required to unlock the feature")]
                public String Rank;
            }

            internal class MapMark
            {
                [JsonProperty("Включить отображение игрока на G карте | Enable the display of the player on the G map")]
                public Boolean UseMark;
                [JsonProperty("Включить отображение ящиков с наградой для игрока на G карте | Enable display of reward boxes for the player on the G map")]
                public Boolean UseMarkCrates;
                [JsonProperty("Радиус отображения метки на карте | The radius of the placemark display on the map")]
                public Single Radius;
                [JsonProperty("Время обновления метки (В секундах) | Time to update the placemark (In seconds)")]
                public Single RefreshRate;
                [JsonProperty("Название метки (%NAME% - выведет имя игрока) | Marker name (%NAME% - displays the player's name)")]
                public String DisplayName;
                [JsonProperty("HEX цвет метки | HEX Marker color")]
                public String Color;
                [JsonProperty("HEX цвет обводки метки | HEX color of the marker outline")]
                public String OutLineColor;
            }

            [JsonProperty("Включить внутреннюю защиту от попытки абуза со своей тимой | Enable internal protection against an attempt to abuse with your team")]
            public Boolean AntiAbuse;
            [JsonProperty("Автоматический поиск игроков(true - включен/false - отключен) | Automatic search for players(true-enabled/false-disabled)")]
            public Boolean TurnAutoFilling;
            [JsonProperty("Через сколько искать игроков для задания цели | After how long to search for players to set a goal")]
            public Int32 TimeFinding;
            [JsonProperty("Оповещать всех игроков о том,что появилась новая награда за голову(ture - да/false - нет) | Notify all players that a new head reward has appeared(ture-yes/false-no)")]
            public Boolean UseAlertHead;
            [JsonProperty("Максимальное количество наград за голову(Исходя из списка наград,будет выбираться рандомное количество) | The maximum number of awards per head(Based on the list of awards, a random number will be selected)")]
            public Int32 MaxReward;
            [JsonProperty("Настройка наград за голову(Рандомно будет выбираться N количество) | Setting up head rewards(N numbers will be randomly selected)")]
            public List<ItemList> itemLists;
            public class ItemList
            {
                [JsonProperty("Тип награды(от этого зависит,что будут выдавать) : 0 - Предмет , 1 - Команда, 2 - IQEconomic | The type of reward (it depends on what will be given): 0-Item, 1-Command, 2-IQEconomic")]
                public TypeReward TypeRewards;
                [JsonProperty("Команда(%STEAMID% - замениться на ID игрока) | Command (%STEAMID% - change to player ID)")]
                public String Command;
                [JsonProperty("Отображаемое имя | Display name")]
                public String DisplayName;
                [JsonProperty("Shortname")]
                public String Shortname;
                [JsonProperty("SkinID")]
                public UInt64 SkinID;
                [JsonProperty("Минимальное количество | Minimal amount")]
                public Int32 AmountMin;
                [JsonProperty("Максимальное количество | Maximum amount")]
                public Int32 AmountMax;
                [JsonProperty("Настройки IQEconomic | Setting IQEconomic")]
                public IQEconomics IQEconomic;
                internal class IQEconomics
                {
                    [JsonProperty("IQEconomic : Минимальный баланс | IQEconomic : Minimal balance")]
                    public Int32 MinBalance;
                    [JsonProperty("IQEconomic : Максимальный баланс | IQEconomic : Maximum balance")]
                    public Int32 MaxBalance;
                }

            }

            internal class ChatSettings
            {
                [JsonProperty("IQChat : Кастомный префикс в чате | IQChat : Custom prefix in the chat")]
                public String CustomPrefix;
                [JsonProperty("IQChat : Кастомный аватар в чате(Если требуется) | IQChat : Custom avatar in the chat(If required)")]
                public String CustomAvatar;
            }

            internal class AlertUI
            {
                [JsonProperty("Использовать UI уведомление о том,что за голову игрока назначена награда | Use the UI notification that a reward is assigned for the player's head")]
                public Boolean UseAlertUI;
                [JsonProperty("Возможность скрывать игрокам UI уведомление о том,что за голову назначена награда (по клику на уведомление) (true - да/false - нет) | The ability to hide the UI notification to players that a reward is assigned for the head (by clicking on the notification) (true-yes/false-no)")]
                public Boolean UserCloseUI;
                [JsonProperty("Ссылка на PNG | Link PNG (En Image - https://i.imgur.com/S4NxpUa.png)")]
                public String PNG;
                [JsonProperty("AnchorMin")]
                public String AnchorMin;
                [JsonProperty("AnchorMax")]
                public String AnchorMax;
                [JsonProperty("OffsetMin")]
                public String OffsetMin;
                [JsonProperty("OffsetMax")]
                public String OffsetMax;
            }

            internal class CustomCreated
            {
                [JsonProperty("Настройка возврата предметов при предсоздании награды за голову(не объявив ее) | Setting up the return of items when pre-creating a head reward(without declaring it)")]
                public ReturnPreCreated ReturnCreated;
                [JsonProperty("Разрешить создавать игрокам использовать команду /ih во время рейдблока(true - да/false - нет) | Allow players to create and use the /ih command during a raid block(true-yes/false-no)")]
                public Boolean UseCommandBlock;
                [JsonProperty("Разрешить создавать игрокам награды за голову | Allow players to create rewards for their heads")]
                public Boolean UseCustomReward;
                [JsonProperty("Разрешить игрокам выбирать время из списка иначе будет устанавливаться из конфига по умолчанию | Allow players to choose the time from the list otherwise it will be set from the default config")]
                public Boolean UseCutomTime;
                [JsonProperty("Максимальное количество предметов в качестве награды от одного игрока | The maximum number of items as a reward from one player")]
                public Int32 MaximumRewardCountUser;
                [JsonProperty("Настройки времени | Time Settings")]
                public TimeSettings TimeSetting;
                internal class ReturnPreCreated
                {
                    [JsonProperty("Возвращать предметы игрокам через время, которые они вложили в предсоздания объявления (Не создав его до конца) | Return items to players after the time that they have invested in the pre-creation of the ad (Without creating it until the end)")]
                    public Boolean UseReturnTimer;
                    [JsonProperty("Время через которое будет возврат | The time after which the refund will be made")]
                    public Int32 Time;
                }

                internal class TimeSettings
                {
                    [JsonProperty("Время по умолчанию(секунды) | Default time(seconds)")]
                    public Int32 TimeDefault;
                    [JsonProperty("Настраиваемый список для выбора времени игркоками | Customizable list for selecting the time of playcocks")]
                    public List<TimeCreatedMore> CustomTimeSettings;
                    internal class TimeCreatedMore
                    {
                        [JsonProperty("Использовать поддержку IQEconomic | Use IQEconomic support")]
                        public Boolean IQEconomic_CustomTime;
                        [JsonProperty("Время(секунды) | Time(seconds)")]
                        public Int32 Time;
                        [JsonProperty("Цена за установку данного времени, должна быть поддержка IQEconomic | The price for the installation of this time, there must be support for IQEconomic")]
                        public Int32 IQEconomic_Balance;
                    }

                }

            }

        }

        public static Configuration GetNewConfiguration();
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private void FillingPlayer();
    private int maxTry;
    private List<Vector3>[] patternPositionsAboveWater;
    private List<Vector3>[] patternPositionsUnderWater;
    private List<Vector3> busyPoints3D;
    private readonly Quaternion[] directions;
    private void FillPatterns();
    public bool IsFlat(Vector3 position);
    public bool IsDistancePoint(Vector3 point);
    private void GenerateSpawnPoints();
    private bool Is3DPointValid(Vector3 point);
    private void AcceptValue(Vector3 point);
    private void RewardCreated(List<Configuration.Settings.ItemList> Rewards, BasePlayer Killer);
    private void CreatedCrate(List<Configuration.Settings.ItemList> Rewards, Vector3 Position, BasePlayer Killer);
    private Item CreatedItem(Configuration.Settings.ItemList Reward);
    private void CheckHeadItemsCooldown();
    private void CheckHeadsCooldown();
    private Boolean IsCooldown(HeadTask Task);
    private Boolean IsCooldown(Double Cooldown);
    private void DistributionReturnedItems(HeadTask Task, Boolean IsPreCreated);
    private void ReturnedItems(BasePlayer player, Boolean IsPreCreated);
    private void KilledTask(BasePlayer Killer, BasePlayer Wanted);
    private void CreatedTask(BasePlayer player);
    private void PlayerHeadAlert(UInt64 WantedID, TypeTask typeTask);
    private void LocalCreatedTask(TypeCreatedTask TypeCreatedTask, UInt64 OwnerID, Item ItemTake, Int32 Amount, UInt64 WantedID, Int32 Cooldown);
    private void LocalTaskRemoveItem(BasePlayer player, Item ItemTake);
    private void ReheadPlayer(BasePlayer player, UInt64 WantedID);
    private IEnumerator DownloadImages();
     void LoadedImage();
     void CahedImages(BasePlayer player);
     List<Configuration.Settings.ItemList> GetRandomReward(List<Configuration.Settings.ItemList> RewardList, Int32 CountReward);
    private Single GetPercentStatus(BasePlayer player, HeadTask Data);
    public String FormatTime(TimeSpan time, String UserID);
    private String Format(Int32 units, String form1, String form2, String form3);
    private void Init();
    private void OnServerInitialized();
     void OnPlayerConnected(BasePlayer player);
     void OnPlayerDeath(BasePlayer Wanted, HitInfo info);
     void Unload();
    [ChatCommand("ih")]
     void ChatCommandHeads(BasePlayer player);
    [ChatCommand("ir")]
     void ChatCommandReturned(BasePlayer player);
    [ConsoleCommand("ih")]
     void ConsoleCommandHeads(ConsoleSystem.Arg args);
    [ChatCommand("debugih")]
     void DevDebugCommand(BasePlayer player);
    public static string ALERT_UI;
     void UI_Interface(BasePlayer player);
     void ShowPlayerPanel(BasePlayer player, CuiElementContainer container, Int32 Count, HeadTask Heads, String AnchorMin, String AnchorMax);
     void ShowItemsPlayerPanel(BasePlayer player, CuiElementContainer container, Int32 Count, HeadTask Task, String AnchorMin, String AnchorMax);
     void ShowItemsPlayerPanelMore(BasePlayer player, String AnchorMin, String AnchorMax, Int32 CountUI, List<Configuration.Settings.ItemList> ItemList);
     void Created_Head_Task(BasePlayer player, Boolean Rehead);
     void Take_Reward_Items(BasePlayer player);
    private Int32 GetAmountReward(BasePlayer player);
     void Show_Amount_Drop_Input(BasePlayer player, Item Item);
     void Search_Player_Tasked(BasePlayer player);
     void Show_Confirm_Player_Tasked(BasePlayer player, BasePlayer TaskPlayer);
     void Take_Time_Custom(BasePlayer player);
     void Show_Item_Reward(BasePlayer player, List<Configuration.Settings.ItemList> ItemList);
     void Update_Task(BasePlayer player);
     void Update_Task_Items(BasePlayer player);
     void Update_Status_Created(BasePlayer player);
     void AlertUI(BasePlayer player);
    private static string HexToRustFormat(string hex);
    public static StringBuilder sb;
    public String GetLang(String LangKey, String userID, object[] args);
    private new void LoadDefaultMessages();
    private object CanNetworkTo(BaseNetworkable entity, BasePlayer target);
    private void CreateMarker(BaseEntity WantedEntity);
    private void CreateMarker(BaseEntity Entity, BasePlayer Killer);
    private void RemoveMarkers();
    private const String GenericPrefab;
    private const String VendingPrefab;
    public class CustomMapMarker : FacepunchBehaviour
    {
        private VendingMachineMapMarker Vending;
        private MapMarkerGenericRadius Generic;
        public BaseEntity Parent;
        public Single Radius;
        public Color Color;
        public Color OutLineColor;
        public String DisplayName;
        public Single RefreshRate;
        public UInt64 IDMark;
        public void Init(CustomMapMarker Info);
        private void Start();
        private void CreateMarkers();
        private void UpdateMarkers();
        private void DestroyMakers();
        private void OnDestroy();
    }

}

public class HeadTask
{
    public String WantedName;
    public UInt64 WantedID;
    public Double Cooldown;
    public Dictionary<UInt64, List<Configuration.Settings.ItemList>> RewardList;
    public Double CooldownItem;
}

public class Configuration
{
    [JsonProperty("Настройка плагина | Settings plugin")]
    public Settings Setting;
    internal class Settings
    {
        [JsonProperty("Настройки IQChat | Settings IQChat")]
        public ChatSettings ChatSetting;
        [JsonProperty("Настройки IQRankSystem | Settings IQRankSystem")]
        public IQRankSystem IQRankSystemSetting;
        [JsonProperty("Настройки UI уведомления | Notification UI Settings")]
        public AlertUI AlertUISetting;
        [JsonProperty("Настройка создания объявления за голову игроками | Setting up the creation of ads for the head of players")]
        public CustomCreated CustomCreatedSetting;
        [JsonProperty("Настройка метки на карте для игрока | Setting up a placemark on the map for the player")]
        public MapMark mapMark;
        internal class IQRankSystem
        {
            [JsonProperty("Использовать IQRankSystem [true-да/false-нет] (При наличии ранга игрок сможет создавать объявления на голову игрока) | Use IQRankSystem [true-yes/false-no] (If there is a rank, the player will be able to create ads on the player's head)")]
            public Boolean UseRank;
            [JsonProperty("Впишите ключ ранга, который требуется для разблокировки возможности | Enter the rank key that is required to unlock the feature")]
            public String Rank;
        }

        internal class MapMark
        {
            [JsonProperty("Включить отображение игрока на G карте | Enable the display of the player on the G map")]
            public Boolean UseMark;
            [JsonProperty("Включить отображение ящиков с наградой для игрока на G карте | Enable display of reward boxes for the player on the G map")]
            public Boolean UseMarkCrates;
            [JsonProperty("Радиус отображения метки на карте | The radius of the placemark display on the map")]
            public Single Radius;
            [JsonProperty("Время обновления метки (В секундах) | Time to update the placemark (In seconds)")]
            public Single RefreshRate;
            [JsonProperty("Название метки (%NAME% - выведет имя игрока) | Marker name (%NAME% - displays the player's name)")]
            public String DisplayName;
            [JsonProperty("HEX цвет метки | HEX Marker color")]
            public String Color;
            [JsonProperty("HEX цвет обводки метки | HEX color of the marker outline")]
            public String OutLineColor;
        }

        [JsonProperty("Включить внутреннюю защиту от попытки абуза со своей тимой | Enable internal protection against an attempt to abuse with your team")]
        public Boolean AntiAbuse;
        [JsonProperty("Автоматический поиск игроков(true - включен/false - отключен) | Automatic search for players(true-enabled/false-disabled)")]
        public Boolean TurnAutoFilling;
        [JsonProperty("Через сколько искать игроков для задания цели | After how long to search for players to set a goal")]
        public Int32 TimeFinding;
        [JsonProperty("Оповещать всех игроков о том,что появилась новая награда за голову(ture - да/false - нет) | Notify all players that a new head reward has appeared(ture-yes/false-no)")]
        public Boolean UseAlertHead;
        [JsonProperty("Максимальное количество наград за голову(Исходя из списка наград,будет выбираться рандомное количество) | The maximum number of awards per head(Based on the list of awards, a random number will be selected)")]
        public Int32 MaxReward;
        [JsonProperty("Настройка наград за голову(Рандомно будет выбираться N количество) | Setting up head rewards(N numbers will be randomly selected)")]
        public List<ItemList> itemLists;
        public class ItemList
        {
            [JsonProperty("Тип награды(от этого зависит,что будут выдавать) : 0 - Предмет , 1 - Команда, 2 - IQEconomic | The type of reward (it depends on what will be given): 0-Item, 1-Command, 2-IQEconomic")]
            public TypeReward TypeRewards;
            [JsonProperty("Команда(%STEAMID% - замениться на ID игрока) | Command (%STEAMID% - change to player ID)")]
            public String Command;
            [JsonProperty("Отображаемое имя | Display name")]
            public String DisplayName;
            [JsonProperty("Shortname")]
            public String Shortname;
            [JsonProperty("SkinID")]
            public UInt64 SkinID;
            [JsonProperty("Минимальное количество | Minimal amount")]
            public Int32 AmountMin;
            [JsonProperty("Максимальное количество | Maximum amount")]
            public Int32 AmountMax;
            [JsonProperty("Настройки IQEconomic | Setting IQEconomic")]
            public IQEconomics IQEconomic;
            internal class IQEconomics
            {
                [JsonProperty("IQEconomic : Минимальный баланс | IQEconomic : Minimal balance")]
                public Int32 MinBalance;
                [JsonProperty("IQEconomic : Максимальный баланс | IQEconomic : Maximum balance")]
                public Int32 MaxBalance;
            }

        }

        internal class ChatSettings
        {
            [JsonProperty("IQChat : Кастомный префикс в чате | IQChat : Custom prefix in the chat")]
            public String CustomPrefix;
            [JsonProperty("IQChat : Кастомный аватар в чате(Если требуется) | IQChat : Custom avatar in the chat(If required)")]
            public String CustomAvatar;
        }

        internal class AlertUI
        {
            [JsonProperty("Использовать UI уведомление о том,что за голову игрока назначена награда | Use the UI notification that a reward is assigned for the player's head")]
            public Boolean UseAlertUI;
            [JsonProperty("Возможность скрывать игрокам UI уведомление о том,что за голову назначена награда (по клику на уведомление) (true - да/false - нет) | The ability to hide the UI notification to players that a reward is assigned for the head (by clicking on the notification) (true-yes/false-no)")]
            public Boolean UserCloseUI;
            [JsonProperty("Ссылка на PNG | Link PNG (En Image - https://i.imgur.com/S4NxpUa.png)")]
            public String PNG;
            [JsonProperty("AnchorMin")]
            public String AnchorMin;
            [JsonProperty("AnchorMax")]
            public String AnchorMax;
            [JsonProperty("OffsetMin")]
            public String OffsetMin;
            [JsonProperty("OffsetMax")]
            public String OffsetMax;
        }

        internal class CustomCreated
        {
            [JsonProperty("Настройка возврата предметов при предсоздании награды за голову(не объявив ее) | Setting up the return of items when pre-creating a head reward(without declaring it)")]
            public ReturnPreCreated ReturnCreated;
            [JsonProperty("Разрешить создавать игрокам использовать команду /ih во время рейдблока(true - да/false - нет) | Allow players to create and use the /ih command during a raid block(true-yes/false-no)")]
            public Boolean UseCommandBlock;
            [JsonProperty("Разрешить создавать игрокам награды за голову | Allow players to create rewards for their heads")]
            public Boolean UseCustomReward;
            [JsonProperty("Разрешить игрокам выбирать время из списка иначе будет устанавливаться из конфига по умолчанию | Allow players to choose the time from the list otherwise it will be set from the default config")]
            public Boolean UseCutomTime;
            [JsonProperty("Максимальное количество предметов в качестве награды от одного игрока | The maximum number of items as a reward from one player")]
            public Int32 MaximumRewardCountUser;
            [JsonProperty("Настройки времени | Time Settings")]
            public TimeSettings TimeSetting;
            internal class ReturnPreCreated
            {
                [JsonProperty("Возвращать предметы игрокам через время, которые они вложили в предсоздания объявления (Не создав его до конца) | Return items to players after the time that they have invested in the pre-creation of the ad (Without creating it until the end)")]
                public Boolean UseReturnTimer;
                [JsonProperty("Время через которое будет возврат | The time after which the refund will be made")]
                public Int32 Time;
            }

            internal class TimeSettings
            {
                [JsonProperty("Время по умолчанию(секунды) | Default time(seconds)")]
                public Int32 TimeDefault;
                [JsonProperty("Настраиваемый список для выбора времени игркоками | Customizable list for selecting the time of playcocks")]
                public List<TimeCreatedMore> CustomTimeSettings;
                internal class TimeCreatedMore
                {
                    [JsonProperty("Использовать поддержку IQEconomic | Use IQEconomic support")]
                    public Boolean IQEconomic_CustomTime;
                    [JsonProperty("Время(секунды) | Time(seconds)")]
                    public Int32 Time;
                    [JsonProperty("Цена за установку данного времени, должна быть поддержка IQEconomic | The price for the installation of this time, there must be support for IQEconomic")]
                    public Int32 IQEconomic_Balance;
                }

            }

        }

    }

    public static Configuration GetNewConfiguration();
}

internal class Settings
{
    [JsonProperty("Настройки IQChat | Settings IQChat")]
    public ChatSettings ChatSetting;
    [JsonProperty("Настройки IQRankSystem | Settings IQRankSystem")]
    public IQRankSystem IQRankSystemSetting;
    [JsonProperty("Настройки UI уведомления | Notification UI Settings")]
    public AlertUI AlertUISetting;
    [JsonProperty("Настройка создания объявления за голову игроками | Setting up the creation of ads for the head of players")]
    public CustomCreated CustomCreatedSetting;
    [JsonProperty("Настройка метки на карте для игрока | Setting up a placemark on the map for the player")]
    public MapMark mapMark;
    internal class IQRankSystem
    {
        [JsonProperty("Использовать IQRankSystem [true-да/false-нет] (При наличии ранга игрок сможет создавать объявления на голову игрока) | Use IQRankSystem [true-yes/false-no] (If there is a rank, the player will be able to create ads on the player's head)")]
        public Boolean UseRank;
        [JsonProperty("Впишите ключ ранга, который требуется для разблокировки возможности | Enter the rank key that is required to unlock the feature")]
        public String Rank;
    }

    internal class MapMark
    {
        [JsonProperty("Включить отображение игрока на G карте | Enable the display of the player on the G map")]
        public Boolean UseMark;
        [JsonProperty("Включить отображение ящиков с наградой для игрока на G карте | Enable display of reward boxes for the player on the G map")]
        public Boolean UseMarkCrates;
        [JsonProperty("Радиус отображения метки на карте | The radius of the placemark display on the map")]
        public Single Radius;
        [JsonProperty("Время обновления метки (В секундах) | Time to update the placemark (In seconds)")]
        public Single RefreshRate;
        [JsonProperty("Название метки (%NAME% - выведет имя игрока) | Marker name (%NAME% - displays the player's name)")]
        public String DisplayName;
        [JsonProperty("HEX цвет метки | HEX Marker color")]
        public String Color;
        [JsonProperty("HEX цвет обводки метки | HEX color of the marker outline")]
        public String OutLineColor;
    }

    [JsonProperty("Включить внутреннюю защиту от попытки абуза со своей тимой | Enable internal protection against an attempt to abuse with your team")]
    public Boolean AntiAbuse;
    [JsonProperty("Автоматический поиск игроков(true - включен/false - отключен) | Automatic search for players(true-enabled/false-disabled)")]
    public Boolean TurnAutoFilling;
    [JsonProperty("Через сколько искать игроков для задания цели | After how long to search for players to set a goal")]
    public Int32 TimeFinding;
    [JsonProperty("Оповещать всех игроков о том,что появилась новая награда за голову(ture - да/false - нет) | Notify all players that a new head reward has appeared(ture-yes/false-no)")]
    public Boolean UseAlertHead;
    [JsonProperty("Максимальное количество наград за голову(Исходя из списка наград,будет выбираться рандомное количество) | The maximum number of awards per head(Based on the list of awards, a random number will be selected)")]
    public Int32 MaxReward;
    [JsonProperty("Настройка наград за голову(Рандомно будет выбираться N количество) | Setting up head rewards(N numbers will be randomly selected)")]
    public List<ItemList> itemLists;
    public class ItemList
    {
        [JsonProperty("Тип награды(от этого зависит,что будут выдавать) : 0 - Предмет , 1 - Команда, 2 - IQEconomic | The type of reward (it depends on what will be given): 0-Item, 1-Command, 2-IQEconomic")]
        public TypeReward TypeRewards;
        [JsonProperty("Команда(%STEAMID% - замениться на ID игрока) | Command (%STEAMID% - change to player ID)")]
        public String Command;
        [JsonProperty("Отображаемое имя | Display name")]
        public String DisplayName;
        [JsonProperty("Shortname")]
        public String Shortname;
        [JsonProperty("SkinID")]
        public UInt64 SkinID;
        [JsonProperty("Минимальное количество | Minimal amount")]
        public Int32 AmountMin;
        [JsonProperty("Максимальное количество | Maximum amount")]
        public Int32 AmountMax;
        [JsonProperty("Настройки IQEconomic | Setting IQEconomic")]
        public IQEconomics IQEconomic;
        internal class IQEconomics
        {
            [JsonProperty("IQEconomic : Минимальный баланс | IQEconomic : Minimal balance")]
            public Int32 MinBalance;
            [JsonProperty("IQEconomic : Максимальный баланс | IQEconomic : Maximum balance")]
            public Int32 MaxBalance;
        }

    }

    internal class ChatSettings
    {
        [JsonProperty("IQChat : Кастомный префикс в чате | IQChat : Custom prefix in the chat")]
        public String CustomPrefix;
        [JsonProperty("IQChat : Кастомный аватар в чате(Если требуется) | IQChat : Custom avatar in the chat(If required)")]
        public String CustomAvatar;
    }

    internal class AlertUI
    {
        [JsonProperty("Использовать UI уведомление о том,что за голову игрока назначена награда | Use the UI notification that a reward is assigned for the player's head")]
        public Boolean UseAlertUI;
        [JsonProperty("Возможность скрывать игрокам UI уведомление о том,что за голову назначена награда (по клику на уведомление) (true - да/false - нет) | The ability to hide the UI notification to players that a reward is assigned for the head (by clicking on the notification) (true-yes/false-no)")]
        public Boolean UserCloseUI;
        [JsonProperty("Ссылка на PNG | Link PNG (En Image - https://i.imgur.com/S4NxpUa.png)")]
        public String PNG;
        [JsonProperty("AnchorMin")]
        public String AnchorMin;
        [JsonProperty("AnchorMax")]
        public String AnchorMax;
        [JsonProperty("OffsetMin")]
        public String OffsetMin;
        [JsonProperty("OffsetMax")]
        public String OffsetMax;
    }

    internal class CustomCreated
    {
        [JsonProperty("Настройка возврата предметов при предсоздании награды за голову(не объявив ее) | Setting up the return of items when pre-creating a head reward(without declaring it)")]
        public ReturnPreCreated ReturnCreated;
        [JsonProperty("Разрешить создавать игрокам использовать команду /ih во время рейдблока(true - да/false - нет) | Allow players to create and use the /ih command during a raid block(true-yes/false-no)")]
        public Boolean UseCommandBlock;
        [JsonProperty("Разрешить создавать игрокам награды за голову | Allow players to create rewards for their heads")]
        public Boolean UseCustomReward;
        [JsonProperty("Разрешить игрокам выбирать время из списка иначе будет устанавливаться из конфига по умолчанию | Allow players to choose the time from the list otherwise it will be set from the default config")]
        public Boolean UseCutomTime;
        [JsonProperty("Максимальное количество предметов в качестве награды от одного игрока | The maximum number of items as a reward from one player")]
        public Int32 MaximumRewardCountUser;
        [JsonProperty("Настройки времени | Time Settings")]
        public TimeSettings TimeSetting;
        internal class ReturnPreCreated
        {
            [JsonProperty("Возвращать предметы игрокам через время, которые они вложили в предсоздания объявления (Не создав его до конца) | Return items to players after the time that they have invested in the pre-creation of the ad (Without creating it until the end)")]
            public Boolean UseReturnTimer;
            [JsonProperty("Время через которое будет возврат | The time after which the refund will be made")]
            public Int32 Time;
        }

        internal class TimeSettings
        {
            [JsonProperty("Время по умолчанию(секунды) | Default time(seconds)")]
            public Int32 TimeDefault;
            [JsonProperty("Настраиваемый список для выбора времени игркоками | Customizable list for selecting the time of playcocks")]
            public List<TimeCreatedMore> CustomTimeSettings;
            internal class TimeCreatedMore
            {
                [JsonProperty("Использовать поддержку IQEconomic | Use IQEconomic support")]
                public Boolean IQEconomic_CustomTime;
                [JsonProperty("Время(секунды) | Time(seconds)")]
                public Int32 Time;
                [JsonProperty("Цена за установку данного времени, должна быть поддержка IQEconomic | The price for the installation of this time, there must be support for IQEconomic")]
                public Int32 IQEconomic_Balance;
            }

        }

    }

}

internal class IQRankSystem
{
    [JsonProperty("Использовать IQRankSystem [true-да/false-нет] (При наличии ранга игрок сможет создавать объявления на голову игрока) | Use IQRankSystem [true-yes/false-no] (If there is a rank, the player will be able to create ads on the player's head)")]
    public Boolean UseRank;
    [JsonProperty("Впишите ключ ранга, который требуется для разблокировки возможности | Enter the rank key that is required to unlock the feature")]
    public String Rank;
}

internal class MapMark
{
    [JsonProperty("Включить отображение игрока на G карте | Enable the display of the player on the G map")]
    public Boolean UseMark;
    [JsonProperty("Включить отображение ящиков с наградой для игрока на G карте | Enable display of reward boxes for the player on the G map")]
    public Boolean UseMarkCrates;
    [JsonProperty("Радиус отображения метки на карте | The radius of the placemark display on the map")]
    public Single Radius;
    [JsonProperty("Время обновления метки (В секундах) | Time to update the placemark (In seconds)")]
    public Single RefreshRate;
    [JsonProperty("Название метки (%NAME% - выведет имя игрока) | Marker name (%NAME% - displays the player's name)")]
    public String DisplayName;
    [JsonProperty("HEX цвет метки | HEX Marker color")]
    public String Color;
    [JsonProperty("HEX цвет обводки метки | HEX color of the marker outline")]
    public String OutLineColor;
}

public class ItemList
{
    [JsonProperty("Тип награды(от этого зависит,что будут выдавать) : 0 - Предмет , 1 - Команда, 2 - IQEconomic | The type of reward (it depends on what will be given): 0-Item, 1-Command, 2-IQEconomic")]
    public TypeReward TypeRewards;
    [JsonProperty("Команда(%STEAMID% - замениться на ID игрока) | Command (%STEAMID% - change to player ID)")]
    public String Command;
    [JsonProperty("Отображаемое имя | Display name")]
    public String DisplayName;
    [JsonProperty("Shortname")]
    public String Shortname;
    [JsonProperty("SkinID")]
    public UInt64 SkinID;
    [JsonProperty("Минимальное количество | Minimal amount")]
    public Int32 AmountMin;
    [JsonProperty("Максимальное количество | Maximum amount")]
    public Int32 AmountMax;
    [JsonProperty("Настройки IQEconomic | Setting IQEconomic")]
    public IQEconomics IQEconomic;
    internal class IQEconomics
    {
        [JsonProperty("IQEconomic : Минимальный баланс | IQEconomic : Minimal balance")]
        public Int32 MinBalance;
        [JsonProperty("IQEconomic : Максимальный баланс | IQEconomic : Maximum balance")]
        public Int32 MaxBalance;
    }

}

internal class IQEconomics
{
    [JsonProperty("IQEconomic : Минимальный баланс | IQEconomic : Minimal balance")]
    public Int32 MinBalance;
    [JsonProperty("IQEconomic : Максимальный баланс | IQEconomic : Maximum balance")]
    public Int32 MaxBalance;
}

internal class ChatSettings
{
    [JsonProperty("IQChat : Кастомный префикс в чате | IQChat : Custom prefix in the chat")]
    public String CustomPrefix;
    [JsonProperty("IQChat : Кастомный аватар в чате(Если требуется) | IQChat : Custom avatar in the chat(If required)")]
    public String CustomAvatar;
}

internal class AlertUI
{
    [JsonProperty("Использовать UI уведомление о том,что за голову игрока назначена награда | Use the UI notification that a reward is assigned for the player's head")]
    public Boolean UseAlertUI;
    [JsonProperty("Возможность скрывать игрокам UI уведомление о том,что за голову назначена награда (по клику на уведомление) (true - да/false - нет) | The ability to hide the UI notification to players that a reward is assigned for the head (by clicking on the notification) (true-yes/false-no)")]
    public Boolean UserCloseUI;
    [JsonProperty("Ссылка на PNG | Link PNG (En Image - https://i.imgur.com/S4NxpUa.png)")]
    public String PNG;
    [JsonProperty("AnchorMin")]
    public String AnchorMin;
    [JsonProperty("AnchorMax")]
    public String AnchorMax;
    [JsonProperty("OffsetMin")]
    public String OffsetMin;
    [JsonProperty("OffsetMax")]
    public String OffsetMax;
}

internal class CustomCreated
{
    [JsonProperty("Настройка возврата предметов при предсоздании награды за голову(не объявив ее) | Setting up the return of items when pre-creating a head reward(without declaring it)")]
    public ReturnPreCreated ReturnCreated;
    [JsonProperty("Разрешить создавать игрокам использовать команду /ih во время рейдблока(true - да/false - нет) | Allow players to create and use the /ih command during a raid block(true-yes/false-no)")]
    public Boolean UseCommandBlock;
    [JsonProperty("Разрешить создавать игрокам награды за голову | Allow players to create rewards for their heads")]
    public Boolean UseCustomReward;
    [JsonProperty("Разрешить игрокам выбирать время из списка иначе будет устанавливаться из конфига по умолчанию | Allow players to choose the time from the list otherwise it will be set from the default config")]
    public Boolean UseCutomTime;
    [JsonProperty("Максимальное количество предметов в качестве награды от одного игрока | The maximum number of items as a reward from one player")]
    public Int32 MaximumRewardCountUser;
    [JsonProperty("Настройки времени | Time Settings")]
    public TimeSettings TimeSetting;
    internal class ReturnPreCreated
    {
        [JsonProperty("Возвращать предметы игрокам через время, которые они вложили в предсоздания объявления (Не создав его до конца) | Return items to players after the time that they have invested in the pre-creation of the ad (Without creating it until the end)")]
        public Boolean UseReturnTimer;
        [JsonProperty("Время через которое будет возврат | The time after which the refund will be made")]
        public Int32 Time;
    }

    internal class TimeSettings
    {
        [JsonProperty("Время по умолчанию(секунды) | Default time(seconds)")]
        public Int32 TimeDefault;
        [JsonProperty("Настраиваемый список для выбора времени игркоками | Customizable list for selecting the time of playcocks")]
        public List<TimeCreatedMore> CustomTimeSettings;
        internal class TimeCreatedMore
        {
            [JsonProperty("Использовать поддержку IQEconomic | Use IQEconomic support")]
            public Boolean IQEconomic_CustomTime;
            [JsonProperty("Время(секунды) | Time(seconds)")]
            public Int32 Time;
            [JsonProperty("Цена за установку данного времени, должна быть поддержка IQEconomic | The price for the installation of this time, there must be support for IQEconomic")]
            public Int32 IQEconomic_Balance;
        }

    }

}

internal class ReturnPreCreated
{
    [JsonProperty("Возвращать предметы игрокам через время, которые они вложили в предсоздания объявления (Не создав его до конца) | Return items to players after the time that they have invested in the pre-creation of the ad (Without creating it until the end)")]
    public Boolean UseReturnTimer;
    [JsonProperty("Время через которое будет возврат | The time after which the refund will be made")]
    public Int32 Time;
}

internal class TimeSettings
{
    [JsonProperty("Время по умолчанию(секунды) | Default time(seconds)")]
    public Int32 TimeDefault;
    [JsonProperty("Настраиваемый список для выбора времени игркоками | Customizable list for selecting the time of playcocks")]
    public List<TimeCreatedMore> CustomTimeSettings;
    internal class TimeCreatedMore
    {
        [JsonProperty("Использовать поддержку IQEconomic | Use IQEconomic support")]
        public Boolean IQEconomic_CustomTime;
        [JsonProperty("Время(секунды) | Time(seconds)")]
        public Int32 Time;
        [JsonProperty("Цена за установку данного времени, должна быть поддержка IQEconomic | The price for the installation of this time, there must be support for IQEconomic")]
        public Int32 IQEconomic_Balance;
    }

}

internal class TimeCreatedMore
{
    [JsonProperty("Использовать поддержку IQEconomic | Use IQEconomic support")]
    public Boolean IQEconomic_CustomTime;
    [JsonProperty("Время(секунды) | Time(seconds)")]
    public Int32 Time;
    [JsonProperty("Цена за установку данного времени, должна быть поддержка IQEconomic | The price for the installation of this time, there must be support for IQEconomic")]
    public Int32 IQEconomic_Balance;
}

public class CustomMapMarker : FacepunchBehaviour
{
    private VendingMachineMapMarker Vending;
    private MapMarkerGenericRadius Generic;
    public BaseEntity Parent;
    public Single Radius;
    public Color Color;
    public Color OutLineColor;
    public String DisplayName;
    public Single RefreshRate;
    public UInt64 IDMark;
    public void Init(CustomMapMarker Info);
    private void Start();
    private void CreateMarkers();
    private void UpdateMarkers();
    private void DestroyMakers();
    private void OnDestroy();
}


```

---

## IQInfo

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using Newtonsoft.Json;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using UnityEngine;

Oxide.Plugins
[Info("IQInfo", "Mercury", "0.0.1")]
[Description("Приятное меню для вашего сервера")]
 class IQInfo : RustPlugin
{
    [PluginReference]
     Plugin ImageLibrary;
    private String GetImage(String fileName, UInt64 skin);
    public Boolean AddImage(String url, String shortname, UInt64 skin);
    public void SendImage(BasePlayer player, String imageName, UInt64 imageId);
    public Boolean HasImage(String imageName);
     void AddAllImage();
     void CachedImage(BasePlayer player);
    private static Configuration config;
    private class Configuration
    {
        [JsonProperty("Введите команду для открытия меню")]
        public String CommandCustom;
        [JsonProperty("Настройка интерфейса")]
        public Interface InterfaceSetting;
        [JsonProperty("Настройка контента меню")]
        public Content ContenteSetting;
        internal class Interface
        {
            [JsonProperty("Ссылка на задний фон плагина (Ссылка на картинку без картинки внутри - https://i.imgur.com/cSyXwis.png). Вы можете использовать любую свою фотку PNG (1474х965)")]
            public String BackgroundImage;
            [JsonProperty("Ссылка на картинку с страницей НАЗАД PNG (11х21)")]
            public String BackPageImage;
            [JsonProperty("Ссылка на картинку с страницей ВПЕРЕД PNG (11х21)")]
            public String NextPageImage;
            [JsonProperty("Ссылка на картинку с выделенной категории PNG (242x53)")]
            public String CategorySelectImage;
            [JsonProperty("Ссылка на картинку с кнопки ВЫХОД PNG (23x23)")]
            public String ExitButtonImage;
            [JsonProperty("Ссылка на картинку с чек боксом PNG (21х20)")]
            public String CheckBoxImage;
            [JsonProperty("Ссылка на картинку с галочкой для чек бокса PNG (13х9)")]
            public String CheckImage;
            [JsonProperty("RGBA цвет для текста")]
            public String RGBAColor;
            [JsonProperty("RGBA цвет для активного текста")]
            public String RGBAActiveColor;
        }

        internal class Content
        {
            [JsonProperty("Разрешить юзерам скрывать меню для последующих открытий при входе на сервер")]
            public Boolean UsePlayerHide;
            [JsonProperty("Использовать картинку в лого сервера (true - да/false - нет)")]
            public Boolean UseLogoImage;
            [JsonProperty("Ссылка на лого вашего сервера PNG (128x128)")]
            public String LogoServerImage;
            [JsonProperty("Отображаемое название вашего сервера")]
            public String LogoServerName;
            [JsonProperty("Текст с вашими контактами (Можете оставить поле пустым, если оно вам не нужно0")]
            public String ContactInformation;
            [JsonProperty("Настройка категорий и страниц к ним")]
            public List<Categories> CategoriesSettings;
            internal class Categories
            {
                [JsonProperty("Название категории в меню")]
                public String CategoryName;
                [JsonProperty("Ссылка ни PNG картинку для категории (64x64) для точного отображения используйте ПОЛНОСТЬЮ БЕЛУЮ КАРТИНКУ.Плагин сам ее покрасит")]
                public String CategoryImage;
                [JsonProperty("Название открытой категории на странице")]
                public String PageHeaderName;
                [JsonProperty("Настройка страниц с контентом")]
                public List<Pages> PageList;
                internal class Pages
                {
                    [JsonProperty("Использовать изображение вместо текста(true - да/false - нет)")]
                    public Boolean UseImage;
                    [JsonProperty("Ссылка на ваше изображение(795х500 PNG)")]
                    public String Image;
                    [JsonProperty("Строки с текстом на одной странице(Максимум 25 строк на страницу)")]
                    public List<String> TextLines;
                }

            }

        }

        public static Configuration GetNewConfiguration();
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private void Init();
    private void OnServerInitialized();
     void OnPlayerConnected(BasePlayer player);
     void Unload();
    public List<UInt64> HidePlayers;
     void ReadData();
     void WriteData();
    private void TurnedPlayer(BasePlayer player);
    private void PageControllerCategory(BasePlayer player, String PageAction, Int32 Page);
    private void PageControllerInfo(BasePlayer player, String PageAction, Int32 IndexCategory, Int32 Page);
    [ConsoleCommand("iqinfo")]
     void ConsoleSystemIQInfo(ConsoleSystem.Arg arg);
    private static String IQInfo_Hud;
    private static String IQInfo_Background;
    private void InterfaceMenu(BasePlayer player);
    private void HideTurned(BasePlayer player);
    private void InterfaceContent(BasePlayer player, Int32 IndexCategory, Int32 Page);
    private void InterfaceCategory(BasePlayer player, Int32 Page, Int32 SelectedIndex);
}

private class Configuration
{
    [JsonProperty("Введите команду для открытия меню")]
    public String CommandCustom;
    [JsonProperty("Настройка интерфейса")]
    public Interface InterfaceSetting;
    [JsonProperty("Настройка контента меню")]
    public Content ContenteSetting;
    internal class Interface
    {
        [JsonProperty("Ссылка на задний фон плагина (Ссылка на картинку без картинки внутри - https://i.imgur.com/cSyXwis.png). Вы можете использовать любую свою фотку PNG (1474х965)")]
        public String BackgroundImage;
        [JsonProperty("Ссылка на картинку с страницей НАЗАД PNG (11х21)")]
        public String BackPageImage;
        [JsonProperty("Ссылка на картинку с страницей ВПЕРЕД PNG (11х21)")]
        public String NextPageImage;
        [JsonProperty("Ссылка на картинку с выделенной категории PNG (242x53)")]
        public String CategorySelectImage;
        [JsonProperty("Ссылка на картинку с кнопки ВЫХОД PNG (23x23)")]
        public String ExitButtonImage;
        [JsonProperty("Ссылка на картинку с чек боксом PNG (21х20)")]
        public String CheckBoxImage;
        [JsonProperty("Ссылка на картинку с галочкой для чек бокса PNG (13х9)")]
        public String CheckImage;
        [JsonProperty("RGBA цвет для текста")]
        public String RGBAColor;
        [JsonProperty("RGBA цвет для активного текста")]
        public String RGBAActiveColor;
    }

    internal class Content
    {
        [JsonProperty("Разрешить юзерам скрывать меню для последующих открытий при входе на сервер")]
        public Boolean UsePlayerHide;
        [JsonProperty("Использовать картинку в лого сервера (true - да/false - нет)")]
        public Boolean UseLogoImage;
        [JsonProperty("Ссылка на лого вашего сервера PNG (128x128)")]
        public String LogoServerImage;
        [JsonProperty("Отображаемое название вашего сервера")]
        public String LogoServerName;
        [JsonProperty("Текст с вашими контактами (Можете оставить поле пустым, если оно вам не нужно0")]
        public String ContactInformation;
        [JsonProperty("Настройка категорий и страниц к ним")]
        public List<Categories> CategoriesSettings;
        internal class Categories
        {
            [JsonProperty("Название категории в меню")]
            public String CategoryName;
            [JsonProperty("Ссылка ни PNG картинку для категории (64x64) для точного отображения используйте ПОЛНОСТЬЮ БЕЛУЮ КАРТИНКУ.Плагин сам ее покрасит")]
            public String CategoryImage;
            [JsonProperty("Название открытой категории на странице")]
            public String PageHeaderName;
            [JsonProperty("Настройка страниц с контентом")]
            public List<Pages> PageList;
            internal class Pages
            {
                [JsonProperty("Использовать изображение вместо текста(true - да/false - нет)")]
                public Boolean UseImage;
                [JsonProperty("Ссылка на ваше изображение(795х500 PNG)")]
                public String Image;
                [JsonProperty("Строки с текстом на одной странице(Максимум 25 строк на страницу)")]
                public List<String> TextLines;
            }

        }

    }

    public static Configuration GetNewConfiguration();
}

internal class Interface
{
    [JsonProperty("Ссылка на задний фон плагина (Ссылка на картинку без картинки внутри - https://i.imgur.com/cSyXwis.png). Вы можете использовать любую свою фотку PNG (1474х965)")]
    public String BackgroundImage;
    [JsonProperty("Ссылка на картинку с страницей НАЗАД PNG (11х21)")]
    public String BackPageImage;
    [JsonProperty("Ссылка на картинку с страницей ВПЕРЕД PNG (11х21)")]
    public String NextPageImage;
    [JsonProperty("Ссылка на картинку с выделенной категории PNG (242x53)")]
    public String CategorySelectImage;
    [JsonProperty("Ссылка на картинку с кнопки ВЫХОД PNG (23x23)")]
    public String ExitButtonImage;
    [JsonProperty("Ссылка на картинку с чек боксом PNG (21х20)")]
    public String CheckBoxImage;
    [JsonProperty("Ссылка на картинку с галочкой для чек бокса PNG (13х9)")]
    public String CheckImage;
    [JsonProperty("RGBA цвет для текста")]
    public String RGBAColor;
    [JsonProperty("RGBA цвет для активного текста")]
    public String RGBAActiveColor;
}

internal class Content
{
    [JsonProperty("Разрешить юзерам скрывать меню для последующих открытий при входе на сервер")]
    public Boolean UsePlayerHide;
    [JsonProperty("Использовать картинку в лого сервера (true - да/false - нет)")]
    public Boolean UseLogoImage;
    [JsonProperty("Ссылка на лого вашего сервера PNG (128x128)")]
    public String LogoServerImage;
    [JsonProperty("Отображаемое название вашего сервера")]
    public String LogoServerName;
    [JsonProperty("Текст с вашими контактами (Можете оставить поле пустым, если оно вам не нужно0")]
    public String ContactInformation;
    [JsonProperty("Настройка категорий и страниц к ним")]
    public List<Categories> CategoriesSettings;
    internal class Categories
    {
        [JsonProperty("Название категории в меню")]
        public String CategoryName;
        [JsonProperty("Ссылка ни PNG картинку для категории (64x64) для точного отображения используйте ПОЛНОСТЬЮ БЕЛУЮ КАРТИНКУ.Плагин сам ее покрасит")]
        public String CategoryImage;
        [JsonProperty("Название открытой категории на странице")]
        public String PageHeaderName;
        [JsonProperty("Настройка страниц с контентом")]
        public List<Pages> PageList;
        internal class Pages
        {
            [JsonProperty("Использовать изображение вместо текста(true - да/false - нет)")]
            public Boolean UseImage;
            [JsonProperty("Ссылка на ваше изображение(795х500 PNG)")]
            public String Image;
            [JsonProperty("Строки с текстом на одной странице(Максимум 25 строк на страницу)")]
            public List<String> TextLines;
        }

    }

}

internal class Categories
{
    [JsonProperty("Название категории в меню")]
    public String CategoryName;
    [JsonProperty("Ссылка ни PNG картинку для категории (64x64) для точного отображения используйте ПОЛНОСТЬЮ БЕЛУЮ КАРТИНКУ.Плагин сам ее покрасит")]
    public String CategoryImage;
    [JsonProperty("Название открытой категории на странице")]
    public String PageHeaderName;
    [JsonProperty("Настройка страниц с контентом")]
    public List<Pages> PageList;
    internal class Pages
    {
        [JsonProperty("Использовать изображение вместо текста(true - да/false - нет)")]
        public Boolean UseImage;
        [JsonProperty("Ссылка на ваше изображение(795х500 PNG)")]
        public String Image;
        [JsonProperty("Строки с текстом на одной странице(Максимум 25 строк на страницу)")]
        public List<String> TextLines;
    }

}

internal class Pages
{
    [JsonProperty("Использовать изображение вместо текста(true - да/false - нет)")]
    public Boolean UseImage;
    [JsonProperty("Ссылка на ваше изображение(795х500 PNG)")]
    public String Image;
    [JsonProperty("Строки с текстом на одной странице(Максимум 25 строк на страницу)")]
    public List<String> TextLines;
}


```

---

## IQKits

```csharp
using System;
using System.Collections;
using System.Collections.Generic;
using Newtonsoft.Json;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using UnityEngine;
using System.Linq;
using System.Text;
using ConVar;

Oxide.Plugins
[Info("IQKits", "Mercury", "0.0.3")]
[Description("Лучшие наборы из всех,которые есть")]
 class IQKits : RustPlugin
{
    [PluginReference]
     Plugin ImageLibrary;
     Plugin IQPlagueSkill;
     Plugin IQChat;
    public void SendChat(BasePlayer player, string Message, Chat.ChatChannel channel);
    private string GetImage(string fileName, ulong skin);
    public bool AddImage(string url, string shortname, ulong skin);
    public void SendImage(BasePlayer player, string imageName, ulong imageId);
    public bool HasImage(string imageName);
     bool IS_SKILL_COOLDOWN(BasePlayer player);
     bool IS_SKILL_RARE(BasePlayer player);
     int GET_SKILL_COOLDOWN_PERCENT();
     int GET_SKILL_RARE_PERCENT();
    public static DateTime TimeCreatedSave;
    public static DateTime RealTime;
    public static int WipeTime;
    public Dictionary<BasePlayer, int> PagePlayers;
    private static Configuration config;
    private class Configuration
    {
        [JsonProperty("Общие настройки")]
        public GeneralSettings GeneralSetting;
        [JsonProperty("Настройки интерфейса")]
        public InterfaceSettings InterfaceSetting;
        [JsonProperty("Настройка наборов")]
        public Dictionary<string, Kits> KitList;
        [JsonProperty("Настройки плагинов совместимости")]
        public ReferenceSettings ReferenceSetting;
        internal class ReferenceSettings
        {
            [JsonProperty("Настройки IQChat")]
            public IQChatSettings IQChatSetting;
            internal class IQChatSettings
            {
                [JsonProperty("IQChat : Кастомный префикс в чате")]
                public string CustomPrefix;
                [JsonProperty("IQChat : Кастомный аватар в чате(Если требуется)")]
                public string CustomAvatar;
            }

        }

        internal class GeneralSettings
        {
            [JsonProperty("Ключ стартового набора(Дается при возрождении)(если включен список нескольких наборов, данная функция отключается)")]
            public string StartKitKey;
            [JsonProperty("Использовать сразу несколько стартовых наборов(они будут выбираться случайно)")]
            public bool UseStartedKitList;
            [JsonProperty("Список ключей стартового набора(Дается при возрождении)")]
            public List<KitRandom> StartedKitList;
            internal class KitRandom
            {
                [JsonProperty("Ключ набора")]
                public string StartKitKey;
                [JsonProperty("Права для набора(не оставляйте это поле пустым)")]
                public string Permissions;
            }

        }

        internal class InterfaceSettings
        {
            [JsonProperty("HEX: Цвет заднего фона")]
            public string HEXBackground;
            [JsonProperty("HEX: Цвет текста")]
            public string HEXLabels;
            [JsonProperty("HEX: Кнопки с информацией")]
            public string HEXInfoItemButton;
            [JsonProperty("HEX: Цвет текста на кнопке с информацией")]
            public string HEXLabelsInfoItemButton;
            [JsonProperty("HEX: Цвет кнопки забрать")]
            public string HEXAccesButton;
            [JsonProperty("HEX: Цвет текста на кнопке забрать")]
            public string HEXLabelsAccesButton;
            [JsonProperty("HEX: Цвет полосы перезарядки")]
            public string HEXCooldowns;
            [JsonProperty("HEX: Цвет блоков с информацией")]
            public string HEXBlock;
            [JsonProperty("HEX: Цвет блоков на которых будут лежать предметы")]
            public string HEXBlockItemInfo;
            [JsonProperty("Время появления интерфейса(его плавность)")]
            public float InterfaceFadeOut;
            [JsonProperty("Время исчезновения интерфейса(его плавность)")]
            public float InterfaceFadeIn;
            [JsonProperty("PNG заднего фона с информацией о том,что находится в наборе")]
            public string PNGInfoPanel;
            [JsonProperty("PNG заднего фона уведомления")]
            public string PNGAlert;
        }

        internal class Kits
        {
            [JsonProperty("Тип набора(0 - С перезарядкой, 1 - Лимитированый, 2 - Стартовый(АвтоКит), 3 - Лимитированый с перезарядкой)")]
            public TypeKits TypeKit;
            [JsonProperty("Отображаемое имя")]
            public string DisplayName;
            [JsonProperty("Через сколько дней вайпа будет доступен набор")]
            public int WipeOpened;
            [JsonProperty("Права")]
            public string Permission;
            [JsonProperty("PNG(128x128)")]
            public string PNG;
            [JsonProperty("Sprite(Установится если отсутствует PNG)")]
            public string Sprite;
            [JsonProperty("Shortname(Установится если отсутствует PNG и Sprite)")]
            public string Shortname;
            [JsonProperty("Время перезарядки набора")]
            public int CoolDown;
            [JsonProperty("Количество сколько наборов можно взять")]
            public int Amount;
            [JsonProperty("Предметы , которые будут даваться в данном наборе")]
            public List<ItemsKit> ItemKits;
            internal class ItemsKit
            {
                [JsonProperty("Выберите контейнер в который будет перенесен предмет(0 - Одежда, 1 - Панель быстрого доступа, 2 - Рюкзак)")]
                public ContainerItem ContainerItemType;
                [JsonProperty("Название предмета")]
                public string DisplayName;
                [JsonProperty("Shortname предмета")]
                public string Shortname;
                [JsonProperty("Количество(Если это команда,так-же указывайте число)")]
                public int Amount;
                [JsonProperty("Шанс на выпадения предмета(Оставьте 0 - если не нужен шанс)")]
                public int Rare;
                [JsonProperty("SkinID предмета")]
                public ulong SkinID;
                [JsonProperty("PNG предмета(если установлена команда)")]
                public string PNG;
                [JsonProperty("Sprite(Если установлена команда и не установлен PNG)")]
                public string Sprite;
                [JsonProperty("Команда(%STEAMID% заменится на ID пользователя)")]
                public string Command;
            }

        }

        public static Configuration GetNewConfiguration();
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    [JsonProperty("Дата с информацией о игроках")]
    public Dictionary<ulong, DataKitsUser> DataKitsUserList;
    public class DataKitsUser
    {
        [JsonProperty("Информация о наборах игрока")]
        public Dictionary<string, InfoKits> InfoKitsList;
        internal class InfoKits
        {
            public int Amount;
            public int Cooldown;
        }

    }

     void ReadData();
     void WriteData();
     void RegisteredDataUser(BasePlayer player);
     void LoadedImage();
    private IEnumerator DownloadImages();
     void CachingImage(BasePlayer player);
     void RegisteredPermissions();
     void AutoKitGive(BasePlayer player);
     void TakeKit(BasePlayer player, string KitKey);
     void ParseAndGive(BasePlayer player, string KitKey);
    private void GiveItem(BasePlayer player, Item item, ItemContainer cont);
     void CreateNewKit(BasePlayer player, string NameKit);
    private List<Configuration.Kits.ItemsKit> GetPlayerItems(BasePlayer player);
    private Configuration.Kits.ItemsKit ItemToKit(Item item, ContainerItem containerItem);
     void KitRemove(BasePlayer player, string NameKit);
    public bool IsRareDrop(int Rare);
     object OnPlayerRespawned(BasePlayer player);
    private void OnServerInitialized();
     void OnPlayerConnected(BasePlayer player);
     void Unload();
    [ChatCommand("kit")]
     void IQKITS_ChatCommand(BasePlayer player, string cmd, string[] arg);
    [ConsoleCommand("kit_ui_func")]
     void IQKITS_UI_Func(ConsoleSystem.Arg arg);
     void DestroyAll(BasePlayer player);
     void DestroyKits(BasePlayer player);
     void DestroyAlert(BasePlayer player);
     void DestroyInfoKits(BasePlayer player);
    public static string IQKITS_OVERLAY;
     void Interface_IQ_Kits(BasePlayer player);
     void Interface_Loaded_Kits(BasePlayer player);
    private IEnumerator UI_UpdateCooldown(BasePlayer player);
     void Interface_Info_Kits(BasePlayer player, string KitKey);
     void Interface_Alert_Kits(BasePlayer player, string Message);
    private static string HexToRustFormat(string hex);
    private new void LoadDefaultMessages();
    public static StringBuilder sb;
    public string GetLang(string LangKey, string userID, object[] args);
    static DateTime epoch;
    static double CurrentTime();
    public static string FormatTime(TimeSpan time);
    private static string Format(int units, string form1, string form2, string form3);
}

private class Configuration
{
    [JsonProperty("Общие настройки")]
    public GeneralSettings GeneralSetting;
    [JsonProperty("Настройки интерфейса")]
    public InterfaceSettings InterfaceSetting;
    [JsonProperty("Настройка наборов")]
    public Dictionary<string, Kits> KitList;
    [JsonProperty("Настройки плагинов совместимости")]
    public ReferenceSettings ReferenceSetting;
    internal class ReferenceSettings
    {
        [JsonProperty("Настройки IQChat")]
        public IQChatSettings IQChatSetting;
        internal class IQChatSettings
        {
            [JsonProperty("IQChat : Кастомный префикс в чате")]
            public string CustomPrefix;
            [JsonProperty("IQChat : Кастомный аватар в чате(Если требуется)")]
            public string CustomAvatar;
        }

    }

    internal class GeneralSettings
    {
        [JsonProperty("Ключ стартового набора(Дается при возрождении)(если включен список нескольких наборов, данная функция отключается)")]
        public string StartKitKey;
        [JsonProperty("Использовать сразу несколько стартовых наборов(они будут выбираться случайно)")]
        public bool UseStartedKitList;
        [JsonProperty("Список ключей стартового набора(Дается при возрождении)")]
        public List<KitRandom> StartedKitList;
        internal class KitRandom
        {
            [JsonProperty("Ключ набора")]
            public string StartKitKey;
            [JsonProperty("Права для набора(не оставляйте это поле пустым)")]
            public string Permissions;
        }

    }

    internal class InterfaceSettings
    {
        [JsonProperty("HEX: Цвет заднего фона")]
        public string HEXBackground;
        [JsonProperty("HEX: Цвет текста")]
        public string HEXLabels;
        [JsonProperty("HEX: Кнопки с информацией")]
        public string HEXInfoItemButton;
        [JsonProperty("HEX: Цвет текста на кнопке с информацией")]
        public string HEXLabelsInfoItemButton;
        [JsonProperty("HEX: Цвет кнопки забрать")]
        public string HEXAccesButton;
        [JsonProperty("HEX: Цвет текста на кнопке забрать")]
        public string HEXLabelsAccesButton;
        [JsonProperty("HEX: Цвет полосы перезарядки")]
        public string HEXCooldowns;
        [JsonProperty("HEX: Цвет блоков с информацией")]
        public string HEXBlock;
        [JsonProperty("HEX: Цвет блоков на которых будут лежать предметы")]
        public string HEXBlockItemInfo;
        [JsonProperty("Время появления интерфейса(его плавность)")]
        public float InterfaceFadeOut;
        [JsonProperty("Время исчезновения интерфейса(его плавность)")]
        public float InterfaceFadeIn;
        [JsonProperty("PNG заднего фона с информацией о том,что находится в наборе")]
        public string PNGInfoPanel;
        [JsonProperty("PNG заднего фона уведомления")]
        public string PNGAlert;
    }

    internal class Kits
    {
        [JsonProperty("Тип набора(0 - С перезарядкой, 1 - Лимитированый, 2 - Стартовый(АвтоКит), 3 - Лимитированый с перезарядкой)")]
        public TypeKits TypeKit;
        [JsonProperty("Отображаемое имя")]
        public string DisplayName;
        [JsonProperty("Через сколько дней вайпа будет доступен набор")]
        public int WipeOpened;
        [JsonProperty("Права")]
        public string Permission;
        [JsonProperty("PNG(128x128)")]
        public string PNG;
        [JsonProperty("Sprite(Установится если отсутствует PNG)")]
        public string Sprite;
        [JsonProperty("Shortname(Установится если отсутствует PNG и Sprite)")]
        public string Shortname;
        [JsonProperty("Время перезарядки набора")]
        public int CoolDown;
        [JsonProperty("Количество сколько наборов можно взять")]
        public int Amount;
        [JsonProperty("Предметы , которые будут даваться в данном наборе")]
        public List<ItemsKit> ItemKits;
        internal class ItemsKit
        {
            [JsonProperty("Выберите контейнер в который будет перенесен предмет(0 - Одежда, 1 - Панель быстрого доступа, 2 - Рюкзак)")]
            public ContainerItem ContainerItemType;
            [JsonProperty("Название предмета")]
            public string DisplayName;
            [JsonProperty("Shortname предмета")]
            public string Shortname;
            [JsonProperty("Количество(Если это команда,так-же указывайте число)")]
            public int Amount;
            [JsonProperty("Шанс на выпадения предмета(Оставьте 0 - если не нужен шанс)")]
            public int Rare;
            [JsonProperty("SkinID предмета")]
            public ulong SkinID;
            [JsonProperty("PNG предмета(если установлена команда)")]
            public string PNG;
            [JsonProperty("Sprite(Если установлена команда и не установлен PNG)")]
            public string Sprite;
            [JsonProperty("Команда(%STEAMID% заменится на ID пользователя)")]
            public string Command;
        }

    }

    public static Configuration GetNewConfiguration();
}

internal class ReferenceSettings
{
    [JsonProperty("Настройки IQChat")]
    public IQChatSettings IQChatSetting;
    internal class IQChatSettings
    {
        [JsonProperty("IQChat : Кастомный префикс в чате")]
        public string CustomPrefix;
        [JsonProperty("IQChat : Кастомный аватар в чате(Если требуется)")]
        public string CustomAvatar;
    }

}

internal class IQChatSettings
{
    [JsonProperty("IQChat : Кастомный префикс в чате")]
    public string CustomPrefix;
    [JsonProperty("IQChat : Кастомный аватар в чате(Если требуется)")]
    public string CustomAvatar;
}

internal class GeneralSettings
{
    [JsonProperty("Ключ стартового набора(Дается при возрождении)(если включен список нескольких наборов, данная функция отключается)")]
    public string StartKitKey;
    [JsonProperty("Использовать сразу несколько стартовых наборов(они будут выбираться случайно)")]
    public bool UseStartedKitList;
    [JsonProperty("Список ключей стартового набора(Дается при возрождении)")]
    public List<KitRandom> StartedKitList;
    internal class KitRandom
    {
        [JsonProperty("Ключ набора")]
        public string StartKitKey;
        [JsonProperty("Права для набора(не оставляйте это поле пустым)")]
        public string Permissions;
    }

}

internal class KitRandom
{
    [JsonProperty("Ключ набора")]
    public string StartKitKey;
    [JsonProperty("Права для набора(не оставляйте это поле пустым)")]
    public string Permissions;
}

internal class InterfaceSettings
{
    [JsonProperty("HEX: Цвет заднего фона")]
    public string HEXBackground;
    [JsonProperty("HEX: Цвет текста")]
    public string HEXLabels;
    [JsonProperty("HEX: Кнопки с информацией")]
    public string HEXInfoItemButton;
    [JsonProperty("HEX: Цвет текста на кнопке с информацией")]
    public string HEXLabelsInfoItemButton;
    [JsonProperty("HEX: Цвет кнопки забрать")]
    public string HEXAccesButton;
    [JsonProperty("HEX: Цвет текста на кнопке забрать")]
    public string HEXLabelsAccesButton;
    [JsonProperty("HEX: Цвет полосы перезарядки")]
    public string HEXCooldowns;
    [JsonProperty("HEX: Цвет блоков с информацией")]
    public string HEXBlock;
    [JsonProperty("HEX: Цвет блоков на которых будут лежать предметы")]
    public string HEXBlockItemInfo;
    [JsonProperty("Время появления интерфейса(его плавность)")]
    public float InterfaceFadeOut;
    [JsonProperty("Время исчезновения интерфейса(его плавность)")]
    public float InterfaceFadeIn;
    [JsonProperty("PNG заднего фона с информацией о том,что находится в наборе")]
    public string PNGInfoPanel;
    [JsonProperty("PNG заднего фона уведомления")]
    public string PNGAlert;
}

internal class Kits
{
    [JsonProperty("Тип набора(0 - С перезарядкой, 1 - Лимитированый, 2 - Стартовый(АвтоКит), 3 - Лимитированый с перезарядкой)")]
    public TypeKits TypeKit;
    [JsonProperty("Отображаемое имя")]
    public string DisplayName;
    [JsonProperty("Через сколько дней вайпа будет доступен набор")]
    public int WipeOpened;
    [JsonProperty("Права")]
    public string Permission;
    [JsonProperty("PNG(128x128)")]
    public string PNG;
    [JsonProperty("Sprite(Установится если отсутствует PNG)")]
    public string Sprite;
    [JsonProperty("Shortname(Установится если отсутствует PNG и Sprite)")]
    public string Shortname;
    [JsonProperty("Время перезарядки набора")]
    public int CoolDown;
    [JsonProperty("Количество сколько наборов можно взять")]
    public int Amount;
    [JsonProperty("Предметы , которые будут даваться в данном наборе")]
    public List<ItemsKit> ItemKits;
    internal class ItemsKit
    {
        [JsonProperty("Выберите контейнер в который будет перенесен предмет(0 - Одежда, 1 - Панель быстрого доступа, 2 - Рюкзак)")]
        public ContainerItem ContainerItemType;
        [JsonProperty("Название предмета")]
        public string DisplayName;
        [JsonProperty("Shortname предмета")]
        public string Shortname;
        [JsonProperty("Количество(Если это команда,так-же указывайте число)")]
        public int Amount;
        [JsonProperty("Шанс на выпадения предмета(Оставьте 0 - если не нужен шанс)")]
        public int Rare;
        [JsonProperty("SkinID предмета")]
        public ulong SkinID;
        [JsonProperty("PNG предмета(если установлена команда)")]
        public string PNG;
        [JsonProperty("Sprite(Если установлена команда и не установлен PNG)")]
        public string Sprite;
        [JsonProperty("Команда(%STEAMID% заменится на ID пользователя)")]
        public string Command;
    }

}

internal class ItemsKit
{
    [JsonProperty("Выберите контейнер в который будет перенесен предмет(0 - Одежда, 1 - Панель быстрого доступа, 2 - Рюкзак)")]
    public ContainerItem ContainerItemType;
    [JsonProperty("Название предмета")]
    public string DisplayName;
    [JsonProperty("Shortname предмета")]
    public string Shortname;
    [JsonProperty("Количество(Если это команда,так-же указывайте число)")]
    public int Amount;
    [JsonProperty("Шанс на выпадения предмета(Оставьте 0 - если не нужен шанс)")]
    public int Rare;
    [JsonProperty("SkinID предмета")]
    public ulong SkinID;
    [JsonProperty("PNG предмета(если установлена команда)")]
    public string PNG;
    [JsonProperty("Sprite(Если установлена команда и не установлен PNG)")]
    public string Sprite;
    [JsonProperty("Команда(%STEAMID% заменится на ID пользователя)")]
    public string Command;
}

public class DataKitsUser
{
    [JsonProperty("Информация о наборах игрока")]
    public Dictionary<string, InfoKits> InfoKitsList;
    internal class InfoKits
    {
        public int Amount;
        public int Cooldown;
    }

}

internal class InfoKits
{
    public int Amount;
    public int Cooldown;
}


```

---

## IQMarker

```csharp
using System;
using System.Collections.Generic;
using UnityEngine;
using Newtonsoft.Json;
using Oxide.Game.Rust.Cui;
using Oxide.Core.Plugins;

Oxide.Plugins
[Info("IQMarker", "Mercury", "0.0.5")]
[Description("\u041c\u0430\u0440\u043a\u0435\u0440\u044b \u0434\u043b\u044f \u0432\u0430\u0448\u0438\u0445 \u043b\u044e\u0431\u0438\u043c\u0447\u0438\u043a\u043e\u0432")]
 class IQMarker : RustPlugin
{
    [PluginReference]
     Plugin ImageLibrary;
     Plugin Friends;
    public string GetImage(string shortname, ulong skin);
    public bool AddImage(string url, string shortname, ulong skin);
    public void SendImage(BasePlayer player, string imageName, ulong imageId);
    public bool HasImage(string imageName);
    public bool IsFriends(ulong TargetID, ulong PlayerID);
    public void LoadedImage();
    public void CachedImage(BasePlayer player);
    public static string PermissionUseMarker;
    public static string PermissionUseColorList;
    public static string PermissionUseCustomColor;
    public static string PermissionUseSizer;
    public void RegisteredPermissions();
    public void ReturnSettingsMore(BasePlayer player);
    public void Register(string Permissions);
    public static bool IsNullOrWhiteSpace(string value);
    private static Configuration config;
    private class Configuration
    {
        [JsonProperty("\u041d\u0430\u0441\u0442\u0440\u043e\u0439\u043a\u0430 \u043f\u043b\u0430\u0433\u0438\u043d\u0430")]
        public AccessSetting AccessSettings;
        [JsonProperty("\u041d\u0430\u0441\u0442\u0440\u043e\u0439\u043a\u0430 \u0438\u043d\u0442\u0435\u0440\u0444\u0435\u0439\u0441\u0430 \u043f\u043b\u0430\u0433\u0438\u043d\u0430")]
        public SettingsInterface SettingsInterfaces;
        [JsonProperty("\u041d\u0430\u0441\u0442\u0440\u043e\u0439\u043a\u0430 \u043c\u0430\u0440\u043a\u0435\u0440\u043e\u0432")]
        public MarkersSetting MarkesSettings;
        [JsonProperty("\u041d\u0430\u0441\u0442\u0440\u043e\u0439\u043a\u0430 \u0434\u043b\u044f \u043d\u043e\u0432\u044b\u0445 \u043f\u043e\u043b\u044c\u0437\u043e\u0432\u0430\u0442\u0435\u043b\u0435\u0439(\u0411\u0443\u0434\u044c\u0442\u0435 \u0432\u043d\u0438\u043c\u0430\u0442\u0435\u043b\u044c\u043d\u044b,\u044d\u0442\u0438 \u043d\u0430\u0441\u0442\u0440\u043e\u0439\u043a\u0438 \u0434\u0430\u044e\u0442\u0441\u044f \u043f\u043e \u0443\u043c\u043e\u043b\u0447\u0430\u043d\u0438\u044e \u0432\u043d\u0435 \u0437\u0430\u0432\u0438\u0441\u0438\u043c\u043e\u0441\u0442\u0438 \u043e\u0442 \u043f\u0440\u0430\u0432,\u0435\u0441\u043b\u0438 \u0438\u0433\u0440\u043e\u043a \u0438\u0445 \u0432\u044b\u043a\u043b\u044e\u0447\u0438\u0442 \u0438 \u0443 \u043d\u0435\u0433\u043e \u043d\u0435 \u0431\u0443\u0434\u0435\u0442 \u043f\u0440\u0430\u0432, \u043e\u043d\u0438 \u0435\u043c\u0443 \u043f\u043e\u0442\u0440\u0435\u0431\u0443\u044e\u0442\u0441\u044f \u0434\u043b\u044f \u043f\u043e\u0432\u0442\u043e\u0440\u043d\u043e\u0433\u043e \u0432\u043a\u043b\u044e\u0447\u0435\u043d\u0438\u044f)")]
        public UserSettings UserSetting;
        internal class AccessSetting
        {
            [JsonProperty("true - \u0432\u043a\u043b\u044e\u0447\u0438\u0442\u044c \u043d\u0430\u0441\u0442\u0440\u043e\u0439\u043a\u0443 \u043c\u0430\u0440\u043a\u0435\u0440\u043e\u0432/false - \u043e\u0442\u043a\u043b\u044e\u0447\u0438\u0442\u044c \u043d\u0430\u0441\u0442\u0440\u043e\u0439\u043a\u0443")]
            public bool UseSettingsMarker;
            [JsonProperty("\u041d\u0430\u0441\u0442\u0440\u043e\u0439\u043a\u0430 \u0434\u043e\u0441\u0442\u0443\u043f\u043d\u044b\u0445 \u0446\u0432\u0435\u0442\u043e\u0432 \u0434\u043b\u044f \u043c\u0430\u0440\u043a\u0435\u0440\u0430")]
            public List<ColorList> ColorListMarker;
            internal class ColorList
            {
                [JsonProperty("\u041f\u0440\u0430\u0432\u0430 \u0434\u043e\u0441\u0442\u0443\u043f\u0430 \u0434\u043b\u044f \u044d\u0442\u043e\u0433\u043e \u0446\u0432\u0435\u0442\u0430")]
                public string Permissions;
                [JsonProperty("HEX \u0446\u0432\u0435\u0442")]
                public string HexColor;
            }

        }

        internal class UserSettings
        {
            [JsonProperty("\u0412\u043a\u043b\u044e\u0447\u0435\u043d \u043b\u0438 \u043c\u0430\u0440\u043a\u0435\u0440 \u0434\u043b\u044f \u043d\u043e\u0432\u044b\u0445 \u0438\u0433\u0440\u043e\u043a\u043e\u0432?(true - \u0434\u0430/false - \u043d\u0435\u0442)")]
            public bool DefaultTurnOnMarker;
            [JsonProperty("\u0421\u0442\u0430\u043d\u0434\u0430\u0440\u0442\u043d\u044b\u0439 \u0446\u0432\u0435\u0442 \u043c\u0430\u0440\u043a\u0435\u0440\u0430 \u0434\u043b\u044f \u043d\u043e\u0432\u044b\u0445 \u0438\u0433\u0440\u043e\u043a\u043e\u0432(HEX)")]
            public string DefaultHexMarker;
            [JsonProperty("\u0421\u0442\u0430\u043d\u0434\u0430\u0440\u0442\u043d\u044b\u0439 \u0432\u0438\u0434 \u043c\u0430\u0440\u043a\u0435\u0440\u0430 \u0434\u043b\u044f \u043d\u043e\u0432\u044b\u0445 \u0438\u0433\u0440\u043a\u043e\u043e\u0432 (0 - \u0422\u0435\u043a\u0441\u0442 \u0441 \u0434\u0430\u043c\u0430\u0433\u043e\u043c | 1 - \u041f\u043e\u043b\u043e\u0441\u0430 \u0425\u041f | 2 - \u0418\u043a\u043e\u043d\u043a\u0430)")]
            public MarkerType DefaultMarkerType;
            [JsonProperty("\u0421\u0442\u0430\u043d\u0434\u0430\u0440\u0442\u043d\u044b\u0439 \u0440\u0430\u0437\u043c\u0435\u0440 \u043c\u0430\u0440\u043a\u0435\u0440\u0430 \u0434\u043b\u044f \u043d\u043e\u0432\u044b\u0445 \u0438\u0433\u0440\u043a\u043e\u043e\u0432 (0 - \u041c\u0430\u043b\u0435\u043d\u044c\u043a\u0438\u0439 | 1 - \u0421\u0440\u0435\u0434\u043d\u0438\u0439 | 2 - \u0411\u043e\u043b\u044c\u0448\u043e\u0439)")]
            public TypeSize DefaultMarkerSize;
            [JsonProperty("\u0414\u043e\u043f\u043e\u043b\u043d\u0438\u0442\u0435\u043b\u044c\u043d\u044b\u0435 \u043d\u0430\u0441\u0442\u0440\u043e\u0439\u043a\u0438 \u043f\u043e\u043b\u043e\u0441\u044b \u0425\u041f \u0434\u043b\u044f \u043d\u043e\u0432\u044b\u0445 \u0438\u0433\u0440\u043a\u043e\u0432")]
            public HealthBar DefaultHealthBar;
            [JsonProperty("\u0414\u043e\u043f\u043e\u043b\u043d\u0438\u0442\u0435\u043b\u044c\u043d\u044b\u0435 \u043d\u0430\u0441\u0442\u0440\u043e\u0439\u043a\u0438 \u0442\u0435\u043a\u0441\u0442\u0430 \u0441 \u0443\u0440\u043e\u043d\u043e\u043c \u0434\u043b\u044f \u043d\u043e\u0432\u044b\u0445 \u0438\u0433\u0440\u043a\u043e\u0432")]
            public DamageText DefaultDamageText;
            [JsonProperty("\u0414\u043e\u043f\u043e\u043b\u043d\u0438\u0442\u0435\u043b\u044c\u043d\u044b\u0435 \u043d\u0430\u0441\u0442\u0440\u043e\u0439\u043a\u0438 \u0438\u043a\u043e\u043d\u043a\u0438 \u0434\u043b\u044f \u043d\u043e\u0432\u044b\u0445 \u0438\u0433\u0440\u043a\u043e\u0432")]
            public Icon DefaultIcon;
            internal class HealthBar
            {
                [JsonProperty("\u0412\u043a\u043b\u044e\u0447\u0438\u0442\u044c \u043e\u0442\u043e\u0431\u0440\u0430\u0436\u0435\u043d\u0438\u0435 \u0442\u0435\u043a\u0441\u0442\u0430 \u0441 \u0434\u0430\u043c\u0430\u0433\u043e\u043c(true - \u0434\u0430/false - \u043d\u0435\u0442)")]
                public bool TurnDamageText;
                [JsonProperty("\u0412\u043a\u043b\u044e\u0447\u0438\u0442\u044c \u043e\u0442\u043e\u0431\u0440\u0430\u0436\u0435\u043d\u0438\u0435 \u0438\u043a\u043e\u043d\u043a\u0438 \u043f\u0430\u0434\u0435\u043d\u0438\u044f(true - \u0434\u0430/false - \u043d\u0435\u0442)")]
                public bool TurnWounded;
            }

            internal class DamageText
            {
                [JsonProperty("\u0412\u043a\u043b\u044e\u0447\u0438\u0442\u044c \u043e\u0442\u043e\u0431\u0440\u0430\u0436\u0435\u043d\u0438\u0435 \u0442\u0435\u043a\u0441\u0442\u0430 \u0441 \u0434\u0430\u043c\u0430\u0433\u043e\u043c(true - \u0434\u0430 /false - \u043d\u0435\u0442)")]
                public bool TurnWounded;
            }

            internal class Icon
            {
                [JsonProperty("\u0412\u043a\u043b\u044e\u0447\u0438\u0442\u044c \u043e\u0442\u043e\u0431\u0440\u0430\u0436\u0435\u043d\u0438\u0435 \u0442\u0435\u043a\u0441\u0442\u0430 \u0441 \u0434\u0430\u043c\u0430\u0433\u043e\u043c(true - \u0434\u0430 / false - \u043d\u0435\u0442)")]
                public bool TurnDamageText;
                [JsonProperty("\u0412\u043a\u043b\u044e\u0447\u0438\u0442\u044c \u043e\u0442\u043e\u0431\u0440\u0430\u0436\u0435\u043d\u0438\u0435 \u0438\u043a\u043e\u043d\u043a\u0438 \u043f\u0430\u0434\u0435\u043d\u0438\u044f(true - \u0434\u0430/false - \u043d\u0435\u0442)")]
                public bool TurnWounded;
            }

        }

        internal class MarkersSetting
        {
            [JsonProperty("\u041d\u0430\u0441\u0442\u0440\u043e\u0439\u043a\u0430 \u043c\u0430\u0440\u043a\u0435\u0440\u0430 HealthBar")]
            public HealthBarSettings HealthBarSetting;
            [JsonProperty("\u041d\u0430\u0441\u0442\u0440\u043e\u0439\u043a\u0430 \u043c\u0430\u0440\u043a\u0435\u0440\u0430 DamageText")]
            public DamageText DamageTextSetting;
            [JsonProperty("\u041d\u0430\u0441\u0442\u0440\u043e\u0439\u043a\u0430 \u043c\u0430\u0440\u043a\u0435\u0440\u0430 Icon")]
            public Icon IconSetting;
            internal class HealthBarSettings
            {
                [JsonProperty("\u041e\u0431\u0449\u0430\u044f \u043d\u0430\u0441\u0442\u0440\u043e\u0439\u043a\u0430")]
                public GeneralSettings GeneralSetting;
                [JsonProperty("\u041f\u0435\u0440\u043c\u0438\u0448\u0435\u043d\u0441 \u0434\u043b\u044f \u0434\u043e\u0441\u0442\u0443\u043f\u0430 \u043a \u0442\u0438\u043f\u0443 \u043c\u0430\u0440\u043a\u0435\u0440\u0430 HealthBar")]
                public string PermissionsHealthBar;
                [JsonProperty("\u041f\u0435\u0440\u043c\u0438\u0448\u0435\u043d\u0441 \u0434\u043b\u044f \u0434\u043e\u0441\u0442\u0443\u043f\u0430 \u0434\u043e\u043f\u043e\u043b\u043d\u0435\u043d\u0438\u044e HealthBar , \u043e\u0442\u043e\u0431\u0440\u0430\u0436\u0435\u043d\u0438\u0435 \u0443\u0440\u043e\u043d\u0430 \u0440\u044f\u0434\u043e\u043c \u0441 \u043f\u043e\u043a\u0430\u0437\u0430\u0442\u0435\u043b\u0435\u043c")]
                public string PermissionsHealtBarDamageText;
                [JsonProperty("\u041f\u0435\u0440\u043c\u0438\u0448\u0435\u043d\u0441 \u0434\u043b\u044f \u0434\u043e\u0441\u0442\u0443\u043f\u0430 \u0434\u043e\u043f\u043e\u043b\u043d\u0435\u043d\u0438\u044e HealthBar , \u043e\u0442\u043e\u0431\u0440\u0430\u0436\u0435\u043d\u0438\u0435 \u0438\u043a\u043e\u043d\u043a\u0438 \u0441 \u043f\u0430\u0434\u0435\u043d\u0438\u0435\u043c \u0438\u0433\u0440\u043e\u043a\u0430")]
                public string PermissionsHealtBarWounded;
            }

            internal class DamageText
            {
                [JsonProperty("\u041e\u0431\u0449\u0430\u044f \u043d\u0430\u0441\u0442\u0440\u043e\u0439\u043a\u0430")]
                public GeneralSettings GeneralSetting;
                [JsonProperty("\u041f\u0435\u0440\u043c\u0438\u0448\u0435\u043d\u0441 \u0434\u043b\u044f \u0434\u043e\u0441\u0442\u0443\u043f\u0430 \u043a \u0442\u0438\u043f\u0443 \u043c\u0430\u0440\u043a\u0435\u0440\u0430 DamageText")]
                public string PermissionsDamageText;
                [JsonProperty("\u041f\u0435\u0440\u043c\u0438\u0448\u0435\u043d\u0441 \u0434\u043b\u044f \u0434\u043e\u0441\u0442\u0443\u043f\u0430 \u0434\u043e\u043f\u043e\u043b\u043d\u0435\u043d\u0438\u044e DamageText , \u043e\u0442\u043e\u0431\u0440\u0430\u0436\u0435\u043d\u0438\u0435 \u0438\u043a\u043e\u043d\u043a\u0438 \u0441 \u043f\u0430\u0434\u0435\u043d\u0438\u0435\u043c \u0438\u0433\u0440\u043e\u043a\u0430")]
                public string PermissionsDamageTextWounded;
            }

            internal class Icon
            {
                [JsonProperty("\u041e\u0431\u0449\u0430\u044f \u043d\u0430\u0441\u0442\u0440\u043e\u0439\u043a\u0430")]
                public GeneralSettings GeneralSetting;
                [JsonProperty("\u041f\u0435\u0440\u043c\u0438\u0448\u0435\u043d\u0441 \u0434\u043b\u044f \u0434\u043e\u0441\u0442\u0443\u043f\u0430 \u043a \u0442\u0438\u043f\u0443 \u043c\u0430\u0440\u043a\u0435\u0440\u0430 Icon")]
                public string PermissionsIcon;
                [JsonProperty("\u041f\u0435\u0440\u043c\u0438\u0448\u0435\u043d\u0441 \u0434\u043b\u044f \u0434\u043e\u0441\u0442\u0443\u043f\u0430 \u0434\u043e\u043f\u043e\u043b\u043d\u0435\u043d\u0438\u044e Icon , \u043e\u0442\u043e\u0431\u0440\u0430\u0436\u0435\u043d\u0438\u0435 \u0443\u0440\u043e\u043d\u0430 \u0440\u044f\u0434\u043e\u043c \u0441 \u043f\u043e\u043a\u0430\u0437\u0430\u0442\u0435\u043b\u0435\u043c")]
                public string PermissionsIconDamageText;
                [JsonProperty("\u041f\u0435\u0440\u043c\u0438\u0448\u0435\u043d\u0441 \u0434\u043b\u044f \u0434\u043e\u0441\u0442\u0443\u043f\u0430 \u0434\u043e\u043f\u043e\u043b\u043d\u0435\u043d\u0438\u044e HealthBar , \u043e\u0442\u043e\u0431\u0440\u0430\u0436\u0435\u043d\u0438\u0435 \u0438\u043a\u043e\u043d\u043a\u0438 \u0441 \u043f\u0430\u0434\u0435\u043d\u0438\u0435\u043c \u0438\u0433\u0440\u043e\u043a\u0430")]
                public string PermissionsIconWounded;
                [JsonProperty("\u0421\u043f\u0438\u0441\u043e\u043a \u0438\u043a\u043e\u043d\u043e\u043a(\u041d\u0430\u0437\u0432\u0430\u043d\u0438\u0435 - \u0441\u0441\u044b\u043b\u043a\u0430 \u043d\u0430 \u0438\u043a\u043e\u043d\u043a\u0443 64\u044564)")]
                public List<IconListClass> IconList;
                internal class IconListClass
                {
                    public string Permissions;
                    public string PNG;
                }

            }

            internal class GeneralSettings
            {
                [JsonProperty("\u041e\u0442\u043e\u0431\u0440\u0430\u0436\u0430\u0435\u043c\u043e\u0435 \u0438\u043c\u044f")]
                public string DisplayName;
                [JsonProperty("\u041e\u043f\u0438\u0441\u0430\u043d\u0438\u0435")]
                public string Description;
            }

        }

        internal class SettingsInterface
        {
            [JsonProperty("\u041d\u0430\u0441\u0442\u0440\u043e\u0439\u043a\u0430 \u043e\u0441\u043d\u043e\u0432\u043d\u0432\u043e\u0439 \u043f\u0430\u043d\u0435\u043b\u0438")]
            public MainPanel InterfaceMainPanel;
            [JsonProperty("\u041d\u0430\u0441\u0442\u0440\u043e\u0439\u043a\u0430 \u043f\u0430\u043d\u0435\u043b\u0438 \u0441 \u043d\u0430\u0441\u0442\u0440\u043e\u0439\u043a\u043e\u0439 \u043c\u0430\u0440\u043a\u0435\u0440\u0430")]
            public SettingsMarker InterfaceSettingsMarker;
            [JsonProperty("HEX \u0446\u0432\u0435\u0442\u0430 \u0442\u0435\u043a\u0441\u0442\u0430")]
            public string HexLabels;
            internal class MainPanel
            {
                [JsonProperty("HEX \u0437\u0430\u0434\u043d\u0435\u0433\u043e \u0444\u043e\u043d\u0430")]
                public string HexBackground;
                [JsonProperty("Material \u0437\u0430\u0434\u043d\u0435\u0433\u043e \u0444\u043e\u043d\u0430")]
                public string MaterialBackground;
                [JsonProperty("Sprite \u043b\u043e\u0433\u043e\u0442\u0438\u043f\u0430")]
                public string SpriteLogoBackground;
            }

            internal class SettingsMarker
            {
                [JsonProperty("HEX \u043a\u043d\u043e\u043f\u043a\u0438 \u043d\u0430\u0441\u0442\u0440\u043e\u0439\u043a\u0438")]
                public string HexButtonMain;
                [JsonProperty("HEX \u043f\u043e\u043b\u044f \u0432\u0432\u043e\u0434\u0430 \u0434\u043b\u044f \u043d\u0430\u0441\u0442\u0440\u043e\u0439\u043a\u0438 \u0446\u0432\u0435\u0442\u0430 \u043c\u0430\u0440\u043a\u0435\u0440\u0430")]
                public string HexButtonInputPanel;
                [JsonProperty("HEX \u0432\u044b\u0431\u0440\u0430\u043d\u043d\u043e\u0433\u043e \u044d\u043b\u0435\u043c\u0435\u043d\u0442\u0430 \u043e\u0442\u043e\u0431\u0440\u0430\u0436\u0435\u043d\u0438\u044f \u043c\u0430\u0440\u043a\u0435\u0440\u0430")]
                public string HexActiveShowMarker;
                [JsonProperty("HEX \u043d\u0435 \u0432\u044b\u0431\u0440\u0430\u043d\u043d\u043e\u0433\u043e \u044d\u043b\u0435\u043c\u0435\u043d\u0442\u0430 \u043e\u0442\u043e\u0431\u0440\u0430\u0436\u0435\u043d\u0438\u044f \u043c\u0430\u0440\u043a\u0435\u0440\u0430")]
                public string HexInActiveShowMarker;
                [JsonProperty("Sprite \u043d\u0435 \u0432\u044b\u0431\u0440\u0430\u043d\u043d\u043e\u0433\u043e \u044d\u043b\u0435\u043c\u0435\u043d\u0442\u0430 \u043e\u0442\u043e\u0431\u0440\u0430\u0436\u0435\u043d\u0438\u044f \u043c\u0430\u0440\u043a\u0435\u0440\u0430")]
                public string SpriteActiveShowMarker;
                [JsonProperty("Sprite \u0432\u044b\u0431\u0440\u0430\u043d\u043d\u043e\u0433\u043e \u044d\u043b\u0435\u043c\u0435\u043d\u0442\u0430 \u043e\u0442\u043e\u0431\u0440\u0430\u0436\u0435\u043d\u0438\u044f \u043c\u0430\u0440\u043a\u0435\u0440\u0430")]
                public string SpriteInActiveShowMarker;
                [JsonProperty("HEX \u043f\u0430\u043d\u0435\u043b\u0438 \u0441 \u0442\u0438\u043f\u043e\u043c \u043c\u0430\u0440\u043a\u0435\u0440\u0430")]
                public string HexTypeMarkerPanel;
                [JsonProperty("HEX \u043a\u043d\u043e\u043f\u043a\u0438 \u0441 \u0434\u043e\u0441\u0442\u0443\u043f\u043d\u044b\u043c \u0442\u0438\u043f\u043e\u043c \u043c\u0430\u0440\u043a\u0435\u0440\u0430")]
                public string HexTypeMarkerAcces;
                [JsonProperty("HEX \u043a\u043d\u043e\u043f\u043a\u0438 \u0441 \u043d\u0435\u0434\u043e\u0441\u0442\u0443\u043f\u043d\u044b\u043c \u0442\u0438\u043f\u043e\u043c \u043c\u0430\u0440\u043a\u0435\u0440\u0430")]
                public string HexTypeMarkerDeading;
                [JsonProperty("HEX \u043a\u043d\u043e\u043f\u043a\u0438 \u0441 \u0432\u044b\u0431\u0440\u0430\u043d\u043d\u044b\u043c \u0442\u0438\u043f\u043e\u043c \u043c\u0430\u0440\u043a\u0435\u0440\u0430")]
                public string HexTypeMarkerTaking;
            }

        }

        public static Configuration GetNewConfiguration();
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    [JsonProperty("\u0418\u043d\u0444\u043e\u0440\u043c\u0430\u0446\u0438\u044f \u043e \u043d\u0430\u0441\u0442\u0440\u043e\u0439\u043a\u0430\u0445 \u0445\u0438\u0442\u043e\u0432 \u0438\u0433\u0440\u043e\u043a\u0430")]
    public Dictionary<ulong, SettingsUser> DataInformation;
    public class SettingsUser
    {
        public bool TurnMarker;
        public bool HitAnimal;
        public bool HitPlayer;
        public bool HitBuilding;
        public bool HitEntity;
        public bool HitTransport;
        public bool HitNPC;
        public string HexMarker;
        public TypeSize typeSize;
        public MarkerType markerType;
        public HealthBar HealthBarMore;
        public DamageText DamageTextMore;
        public Icon IconMore;
        internal class HealthBar
        {
            public bool TurnDamageText;
            public bool TurnWounded;
        }

        internal class DamageText
        {
            public bool TurnWounded;
        }

        internal class Icon
        {
            public bool TurnDamageText;
            public bool TurnWounded;
            public int IndexMarker;
        }

    }

     void ReadData();
     void WriteData();
     void RegisteredDataUser(BasePlayer player);
    private void OnServerInitialized();
     void OnPlayerConnected(BasePlayer player);
     void OnEntityTakeDamage(BaseCombatEntity entity, HitInfo info);
    [ChatCommand("hit")]
     void ChatCommandHit(BasePlayer player);
    [ConsoleCommand("hit")]
     void ConsoleCommandsHit(ConsoleSystem.Arg arg);
    public void SelectType(BasePlayer player, string Type);
     void DebugPlayerHit(BasePlayer player);
    private void GrantedPermission(string id, string perm);
    public static string PARENT_IQ_MARKER_MAIN;
    public static string PARENT_IQ_MARKER_MARKERS_PANEL;
    public static string PARENT_IQ_MARKER_MARKERS_PANEL_MORE_SETTINGS;
    public static string PARENT_IQ_MARKER_ATTACK_HIT;
    public void Interface_Main_Menu(BasePlayer player);
    public void ButtonUpdateTurn(BasePlayer player);
    public void Interface_Settings_Menu(BasePlayer player);
    public void Interface_Hex_Settings_Menu(BasePlayer player);
    public void Interface_Marker_Panels(BasePlayer player);
    public void Interface_MP_Loaded_Colors(BasePlayer player);
    public void Interface_MP_Loaded_Size(BasePlayer player);
    public void Interface_MP_Loaded_Markers(BasePlayer player);
    public void Show_More_Settings_Marker(BasePlayer player, MarkerType markerType);
    public readonly Dictionary<TypeSize, ResizeClass> Resizer;
    public class ResizeClass
    {
        public HealthBar healthBarCoordinate;
        public string AnchorMin;
        public string AnchorMax;
        public int FontSize;
        internal class HealthBar
        {
            public string AnchorMin;
            public string AnchorMax;
        }

    }

    public void Interface_Update_Marker_Show(BasePlayer player);
    public class ResizerAttackHit
    {
        public readonly Dictionary<TypeSize, TypeResize> ResizerAttack;
        internal class TypeResize
        {
            public Params IconParams;
            public Params HealthBar;
            public int FontSizeText;
            internal class Params
            {
                public string AnchorMin;
                public string AnchorMax;
            }

        }

    }

    public void Interface_Attack_Hit(BasePlayer player, HitInfo targetInfo);
    private static string HexToRustFormat(string hex);
    private new void LoadDefaultMessages();
}

private class Configuration
{
    [JsonProperty("\u041d\u0430\u0441\u0442\u0440\u043e\u0439\u043a\u0430 \u043f\u043b\u0430\u0433\u0438\u043d\u0430")]
    public AccessSetting AccessSettings;
    [JsonProperty("\u041d\u0430\u0441\u0442\u0440\u043e\u0439\u043a\u0430 \u0438\u043d\u0442\u0435\u0440\u0444\u0435\u0439\u0441\u0430 \u043f\u043b\u0430\u0433\u0438\u043d\u0430")]
    public SettingsInterface SettingsInterfaces;
    [JsonProperty("\u041d\u0430\u0441\u0442\u0440\u043e\u0439\u043a\u0430 \u043c\u0430\u0440\u043a\u0435\u0440\u043e\u0432")]
    public MarkersSetting MarkesSettings;
    [JsonProperty("\u041d\u0430\u0441\u0442\u0440\u043e\u0439\u043a\u0430 \u0434\u043b\u044f \u043d\u043e\u0432\u044b\u0445 \u043f\u043e\u043b\u044c\u0437\u043e\u0432\u0430\u0442\u0435\u043b\u0435\u0439(\u0411\u0443\u0434\u044c\u0442\u0435 \u0432\u043d\u0438\u043c\u0430\u0442\u0435\u043b\u044c\u043d\u044b,\u044d\u0442\u0438 \u043d\u0430\u0441\u0442\u0440\u043e\u0439\u043a\u0438 \u0434\u0430\u044e\u0442\u0441\u044f \u043f\u043e \u0443\u043c\u043e\u043b\u0447\u0430\u043d\u0438\u044e \u0432\u043d\u0435 \u0437\u0430\u0432\u0438\u0441\u0438\u043c\u043e\u0441\u0442\u0438 \u043e\u0442 \u043f\u0440\u0430\u0432,\u0435\u0441\u043b\u0438 \u0438\u0433\u0440\u043e\u043a \u0438\u0445 \u0432\u044b\u043a\u043b\u044e\u0447\u0438\u0442 \u0438 \u0443 \u043d\u0435\u0433\u043e \u043d\u0435 \u0431\u0443\u0434\u0435\u0442 \u043f\u0440\u0430\u0432, \u043e\u043d\u0438 \u0435\u043c\u0443 \u043f\u043e\u0442\u0440\u0435\u0431\u0443\u044e\u0442\u0441\u044f \u0434\u043b\u044f \u043f\u043e\u0432\u0442\u043e\u0440\u043d\u043e\u0433\u043e \u0432\u043a\u043b\u044e\u0447\u0435\u043d\u0438\u044f)")]
    public UserSettings UserSetting;
    internal class AccessSetting
    {
        [JsonProperty("true - \u0432\u043a\u043b\u044e\u0447\u0438\u0442\u044c \u043d\u0430\u0441\u0442\u0440\u043e\u0439\u043a\u0443 \u043c\u0430\u0440\u043a\u0435\u0440\u043e\u0432/false - \u043e\u0442\u043a\u043b\u044e\u0447\u0438\u0442\u044c \u043d\u0430\u0441\u0442\u0440\u043e\u0439\u043a\u0443")]
        public bool UseSettingsMarker;
        [JsonProperty("\u041d\u0430\u0441\u0442\u0440\u043e\u0439\u043a\u0430 \u0434\u043e\u0441\u0442\u0443\u043f\u043d\u044b\u0445 \u0446\u0432\u0435\u0442\u043e\u0432 \u0434\u043b\u044f \u043c\u0430\u0440\u043a\u0435\u0440\u0430")]
        public List<ColorList> ColorListMarker;
        internal class ColorList
        {
            [JsonProperty("\u041f\u0440\u0430\u0432\u0430 \u0434\u043e\u0441\u0442\u0443\u043f\u0430 \u0434\u043b\u044f \u044d\u0442\u043e\u0433\u043e \u0446\u0432\u0435\u0442\u0430")]
            public string Permissions;
            [JsonProperty("HEX \u0446\u0432\u0435\u0442")]
            public string HexColor;
        }

    }

    internal class UserSettings
    {
        [JsonProperty("\u0412\u043a\u043b\u044e\u0447\u0435\u043d \u043b\u0438 \u043c\u0430\u0440\u043a\u0435\u0440 \u0434\u043b\u044f \u043d\u043e\u0432\u044b\u0445 \u0438\u0433\u0440\u043e\u043a\u043e\u0432?(true - \u0434\u0430/false - \u043d\u0435\u0442)")]
        public bool DefaultTurnOnMarker;
        [JsonProperty("\u0421\u0442\u0430\u043d\u0434\u0430\u0440\u0442\u043d\u044b\u0439 \u0446\u0432\u0435\u0442 \u043c\u0430\u0440\u043a\u0435\u0440\u0430 \u0434\u043b\u044f \u043d\u043e\u0432\u044b\u0445 \u0438\u0433\u0440\u043e\u043a\u043e\u0432(HEX)")]
        public string DefaultHexMarker;
        [JsonProperty("\u0421\u0442\u0430\u043d\u0434\u0430\u0440\u0442\u043d\u044b\u0439 \u0432\u0438\u0434 \u043c\u0430\u0440\u043a\u0435\u0440\u0430 \u0434\u043b\u044f \u043d\u043e\u0432\u044b\u0445 \u0438\u0433\u0440\u043a\u043e\u043e\u0432 (0 - \u0422\u0435\u043a\u0441\u0442 \u0441 \u0434\u0430\u043c\u0430\u0433\u043e\u043c | 1 - \u041f\u043e\u043b\u043e\u0441\u0430 \u0425\u041f | 2 - \u0418\u043a\u043e\u043d\u043a\u0430)")]
        public MarkerType DefaultMarkerType;
        [JsonProperty("\u0421\u0442\u0430\u043d\u0434\u0430\u0440\u0442\u043d\u044b\u0439 \u0440\u0430\u0437\u043c\u0435\u0440 \u043c\u0430\u0440\u043a\u0435\u0440\u0430 \u0434\u043b\u044f \u043d\u043e\u0432\u044b\u0445 \u0438\u0433\u0440\u043a\u043e\u043e\u0432 (0 - \u041c\u0430\u043b\u0435\u043d\u044c\u043a\u0438\u0439 | 1 - \u0421\u0440\u0435\u0434\u043d\u0438\u0439 | 2 - \u0411\u043e\u043b\u044c\u0448\u043e\u0439)")]
        public TypeSize DefaultMarkerSize;
        [JsonProperty("\u0414\u043e\u043f\u043e\u043b\u043d\u0438\u0442\u0435\u043b\u044c\u043d\u044b\u0435 \u043d\u0430\u0441\u0442\u0440\u043e\u0439\u043a\u0438 \u043f\u043e\u043b\u043e\u0441\u044b \u0425\u041f \u0434\u043b\u044f \u043d\u043e\u0432\u044b\u0445 \u0438\u0433\u0440\u043a\u043e\u0432")]
        public HealthBar DefaultHealthBar;
        [JsonProperty("\u0414\u043e\u043f\u043e\u043b\u043d\u0438\u0442\u0435\u043b\u044c\u043d\u044b\u0435 \u043d\u0430\u0441\u0442\u0440\u043e\u0439\u043a\u0438 \u0442\u0435\u043a\u0441\u0442\u0430 \u0441 \u0443\u0440\u043e\u043d\u043e\u043c \u0434\u043b\u044f \u043d\u043e\u0432\u044b\u0445 \u0438\u0433\u0440\u043a\u043e\u0432")]
        public DamageText DefaultDamageText;
        [JsonProperty("\u0414\u043e\u043f\u043e\u043b\u043d\u0438\u0442\u0435\u043b\u044c\u043d\u044b\u0435 \u043d\u0430\u0441\u0442\u0440\u043e\u0439\u043a\u0438 \u0438\u043a\u043e\u043d\u043a\u0438 \u0434\u043b\u044f \u043d\u043e\u0432\u044b\u0445 \u0438\u0433\u0440\u043a\u043e\u0432")]
        public Icon DefaultIcon;
        internal class HealthBar
        {
            [JsonProperty("\u0412\u043a\u043b\u044e\u0447\u0438\u0442\u044c \u043e\u0442\u043e\u0431\u0440\u0430\u0436\u0435\u043d\u0438\u0435 \u0442\u0435\u043a\u0441\u0442\u0430 \u0441 \u0434\u0430\u043c\u0430\u0433\u043e\u043c(true - \u0434\u0430/false - \u043d\u0435\u0442)")]
            public bool TurnDamageText;
            [JsonProperty("\u0412\u043a\u043b\u044e\u0447\u0438\u0442\u044c \u043e\u0442\u043e\u0431\u0440\u0430\u0436\u0435\u043d\u0438\u0435 \u0438\u043a\u043e\u043d\u043a\u0438 \u043f\u0430\u0434\u0435\u043d\u0438\u044f(true - \u0434\u0430/false - \u043d\u0435\u0442)")]
            public bool TurnWounded;
        }

        internal class DamageText
        {
            [JsonProperty("\u0412\u043a\u043b\u044e\u0447\u0438\u0442\u044c \u043e\u0442\u043e\u0431\u0440\u0430\u0436\u0435\u043d\u0438\u0435 \u0442\u0435\u043a\u0441\u0442\u0430 \u0441 \u0434\u0430\u043c\u0430\u0433\u043e\u043c(true - \u0434\u0430 /false - \u043d\u0435\u0442)")]
            public bool TurnWounded;
        }

        internal class Icon
        {
            [JsonProperty("\u0412\u043a\u043b\u044e\u0447\u0438\u0442\u044c \u043e\u0442\u043e\u0431\u0440\u0430\u0436\u0435\u043d\u0438\u0435 \u0442\u0435\u043a\u0441\u0442\u0430 \u0441 \u0434\u0430\u043c\u0430\u0433\u043e\u043c(true - \u0434\u0430 / false - \u043d\u0435\u0442)")]
            public bool TurnDamageText;
            [JsonProperty("\u0412\u043a\u043b\u044e\u0447\u0438\u0442\u044c \u043e\u0442\u043e\u0431\u0440\u0430\u0436\u0435\u043d\u0438\u0435 \u0438\u043a\u043e\u043d\u043a\u0438 \u043f\u0430\u0434\u0435\u043d\u0438\u044f(true - \u0434\u0430/false - \u043d\u0435\u0442)")]
            public bool TurnWounded;
        }

    }

    internal class MarkersSetting
    {
        [JsonProperty("\u041d\u0430\u0441\u0442\u0440\u043e\u0439\u043a\u0430 \u043c\u0430\u0440\u043a\u0435\u0440\u0430 HealthBar")]
        public HealthBarSettings HealthBarSetting;
        [JsonProperty("\u041d\u0430\u0441\u0442\u0440\u043e\u0439\u043a\u0430 \u043c\u0430\u0440\u043a\u0435\u0440\u0430 DamageText")]
        public DamageText DamageTextSetting;
        [JsonProperty("\u041d\u0430\u0441\u0442\u0440\u043e\u0439\u043a\u0430 \u043c\u0430\u0440\u043a\u0435\u0440\u0430 Icon")]
        public Icon IconSetting;
        internal class HealthBarSettings
        {
            [JsonProperty("\u041e\u0431\u0449\u0430\u044f \u043d\u0430\u0441\u0442\u0440\u043e\u0439\u043a\u0430")]
            public GeneralSettings GeneralSetting;
            [JsonProperty("\u041f\u0435\u0440\u043c\u0438\u0448\u0435\u043d\u0441 \u0434\u043b\u044f \u0434\u043e\u0441\u0442\u0443\u043f\u0430 \u043a \u0442\u0438\u043f\u0443 \u043c\u0430\u0440\u043a\u0435\u0440\u0430 HealthBar")]
            public string PermissionsHealthBar;
            [JsonProperty("\u041f\u0435\u0440\u043c\u0438\u0448\u0435\u043d\u0441 \u0434\u043b\u044f \u0434\u043e\u0441\u0442\u0443\u043f\u0430 \u0434\u043e\u043f\u043e\u043b\u043d\u0435\u043d\u0438\u044e HealthBar , \u043e\u0442\u043e\u0431\u0440\u0430\u0436\u0435\u043d\u0438\u0435 \u0443\u0440\u043e\u043d\u0430 \u0440\u044f\u0434\u043e\u043c \u0441 \u043f\u043e\u043a\u0430\u0437\u0430\u0442\u0435\u043b\u0435\u043c")]
            public string PermissionsHealtBarDamageText;
            [JsonProperty("\u041f\u0435\u0440\u043c\u0438\u0448\u0435\u043d\u0441 \u0434\u043b\u044f \u0434\u043e\u0441\u0442\u0443\u043f\u0430 \u0434\u043e\u043f\u043e\u043b\u043d\u0435\u043d\u0438\u044e HealthBar , \u043e\u0442\u043e\u0431\u0440\u0430\u0436\u0435\u043d\u0438\u0435 \u0438\u043a\u043e\u043d\u043a\u0438 \u0441 \u043f\u0430\u0434\u0435\u043d\u0438\u0435\u043c \u0438\u0433\u0440\u043e\u043a\u0430")]
            public string PermissionsHealtBarWounded;
        }

        internal class DamageText
        {
            [JsonProperty("\u041e\u0431\u0449\u0430\u044f \u043d\u0430\u0441\u0442\u0440\u043e\u0439\u043a\u0430")]
            public GeneralSettings GeneralSetting;
            [JsonProperty("\u041f\u0435\u0440\u043c\u0438\u0448\u0435\u043d\u0441 \u0434\u043b\u044f \u0434\u043e\u0441\u0442\u0443\u043f\u0430 \u043a \u0442\u0438\u043f\u0443 \u043c\u0430\u0440\u043a\u0435\u0440\u0430 DamageText")]
            public string PermissionsDamageText;
            [JsonProperty("\u041f\u0435\u0440\u043c\u0438\u0448\u0435\u043d\u0441 \u0434\u043b\u044f \u0434\u043e\u0441\u0442\u0443\u043f\u0430 \u0434\u043e\u043f\u043e\u043b\u043d\u0435\u043d\u0438\u044e DamageText , \u043e\u0442\u043e\u0431\u0440\u0430\u0436\u0435\u043d\u0438\u0435 \u0438\u043a\u043e\u043d\u043a\u0438 \u0441 \u043f\u0430\u0434\u0435\u043d\u0438\u0435\u043c \u0438\u0433\u0440\u043e\u043a\u0430")]
            public string PermissionsDamageTextWounded;
        }

        internal class Icon
        {
            [JsonProperty("\u041e\u0431\u0449\u0430\u044f \u043d\u0430\u0441\u0442\u0440\u043e\u0439\u043a\u0430")]
            public GeneralSettings GeneralSetting;
            [JsonProperty("\u041f\u0435\u0440\u043c\u0438\u0448\u0435\u043d\u0441 \u0434\u043b\u044f \u0434\u043e\u0441\u0442\u0443\u043f\u0430 \u043a \u0442\u0438\u043f\u0443 \u043c\u0430\u0440\u043a\u0435\u0440\u0430 Icon")]
            public string PermissionsIcon;
            [JsonProperty("\u041f\u0435\u0440\u043c\u0438\u0448\u0435\u043d\u0441 \u0434\u043b\u044f \u0434\u043e\u0441\u0442\u0443\u043f\u0430 \u0434\u043e\u043f\u043e\u043b\u043d\u0435\u043d\u0438\u044e Icon , \u043e\u0442\u043e\u0431\u0440\u0430\u0436\u0435\u043d\u0438\u0435 \u0443\u0440\u043e\u043d\u0430 \u0440\u044f\u0434\u043e\u043c \u0441 \u043f\u043e\u043a\u0430\u0437\u0430\u0442\u0435\u043b\u0435\u043c")]
            public string PermissionsIconDamageText;
            [JsonProperty("\u041f\u0435\u0440\u043c\u0438\u0448\u0435\u043d\u0441 \u0434\u043b\u044f \u0434\u043e\u0441\u0442\u0443\u043f\u0430 \u0434\u043e\u043f\u043e\u043b\u043d\u0435\u043d\u0438\u044e HealthBar , \u043e\u0442\u043e\u0431\u0440\u0430\u0436\u0435\u043d\u0438\u0435 \u0438\u043a\u043e\u043d\u043a\u0438 \u0441 \u043f\u0430\u0434\u0435\u043d\u0438\u0435\u043c \u0438\u0433\u0440\u043e\u043a\u0430")]
            public string PermissionsIconWounded;
            [JsonProperty("\u0421\u043f\u0438\u0441\u043e\u043a \u0438\u043a\u043e\u043d\u043e\u043a(\u041d\u0430\u0437\u0432\u0430\u043d\u0438\u0435 - \u0441\u0441\u044b\u043b\u043a\u0430 \u043d\u0430 \u0438\u043a\u043e\u043d\u043a\u0443 64\u044564)")]
            public List<IconListClass> IconList;
            internal class IconListClass
            {
                public string Permissions;
                public string PNG;
            }

        }

        internal class GeneralSettings
        {
            [JsonProperty("\u041e\u0442\u043e\u0431\u0440\u0430\u0436\u0430\u0435\u043c\u043e\u0435 \u0438\u043c\u044f")]
            public string DisplayName;
            [JsonProperty("\u041e\u043f\u0438\u0441\u0430\u043d\u0438\u0435")]
            public string Description;
        }

    }

    internal class SettingsInterface
    {
        [JsonProperty("\u041d\u0430\u0441\u0442\u0440\u043e\u0439\u043a\u0430 \u043e\u0441\u043d\u043e\u0432\u043d\u0432\u043e\u0439 \u043f\u0430\u043d\u0435\u043b\u0438")]
        public MainPanel InterfaceMainPanel;
        [JsonProperty("\u041d\u0430\u0441\u0442\u0440\u043e\u0439\u043a\u0430 \u043f\u0430\u043d\u0435\u043b\u0438 \u0441 \u043d\u0430\u0441\u0442\u0440\u043e\u0439\u043a\u043e\u0439 \u043c\u0430\u0440\u043a\u0435\u0440\u0430")]
        public SettingsMarker InterfaceSettingsMarker;
        [JsonProperty("HEX \u0446\u0432\u0435\u0442\u0430 \u0442\u0435\u043a\u0441\u0442\u0430")]
        public string HexLabels;
        internal class MainPanel
        {
            [JsonProperty("HEX \u0437\u0430\u0434\u043d\u0435\u0433\u043e \u0444\u043e\u043d\u0430")]
            public string HexBackground;
            [JsonProperty("Material \u0437\u0430\u0434\u043d\u0435\u0433\u043e \u0444\u043e\u043d\u0430")]
            public string MaterialBackground;
            [JsonProperty("Sprite \u043b\u043e\u0433\u043e\u0442\u0438\u043f\u0430")]
            public string SpriteLogoBackground;
        }

        internal class SettingsMarker
        {
            [JsonProperty("HEX \u043a\u043d\u043e\u043f\u043a\u0438 \u043d\u0430\u0441\u0442\u0440\u043e\u0439\u043a\u0438")]
            public string HexButtonMain;
            [JsonProperty("HEX \u043f\u043e\u043b\u044f \u0432\u0432\u043e\u0434\u0430 \u0434\u043b\u044f \u043d\u0430\u0441\u0442\u0440\u043e\u0439\u043a\u0438 \u0446\u0432\u0435\u0442\u0430 \u043c\u0430\u0440\u043a\u0435\u0440\u0430")]
            public string HexButtonInputPanel;
            [JsonProperty("HEX \u0432\u044b\u0431\u0440\u0430\u043d\u043d\u043e\u0433\u043e \u044d\u043b\u0435\u043c\u0435\u043d\u0442\u0430 \u043e\u0442\u043e\u0431\u0440\u0430\u0436\u0435\u043d\u0438\u044f \u043c\u0430\u0440\u043a\u0435\u0440\u0430")]
            public string HexActiveShowMarker;
            [JsonProperty("HEX \u043d\u0435 \u0432\u044b\u0431\u0440\u0430\u043d\u043d\u043e\u0433\u043e \u044d\u043b\u0435\u043c\u0435\u043d\u0442\u0430 \u043e\u0442\u043e\u0431\u0440\u0430\u0436\u0435\u043d\u0438\u044f \u043c\u0430\u0440\u043a\u0435\u0440\u0430")]
            public string HexInActiveShowMarker;
            [JsonProperty("Sprite \u043d\u0435 \u0432\u044b\u0431\u0440\u0430\u043d\u043d\u043e\u0433\u043e \u044d\u043b\u0435\u043c\u0435\u043d\u0442\u0430 \u043e\u0442\u043e\u0431\u0440\u0430\u0436\u0435\u043d\u0438\u044f \u043c\u0430\u0440\u043a\u0435\u0440\u0430")]
            public string SpriteActiveShowMarker;
            [JsonProperty("Sprite \u0432\u044b\u0431\u0440\u0430\u043d\u043d\u043e\u0433\u043e \u044d\u043b\u0435\u043c\u0435\u043d\u0442\u0430 \u043e\u0442\u043e\u0431\u0440\u0430\u0436\u0435\u043d\u0438\u044f \u043c\u0430\u0440\u043a\u0435\u0440\u0430")]
            public string SpriteInActiveShowMarker;
            [JsonProperty("HEX \u043f\u0430\u043d\u0435\u043b\u0438 \u0441 \u0442\u0438\u043f\u043e\u043c \u043c\u0430\u0440\u043a\u0435\u0440\u0430")]
            public string HexTypeMarkerPanel;
            [JsonProperty("HEX \u043a\u043d\u043e\u043f\u043a\u0438 \u0441 \u0434\u043e\u0441\u0442\u0443\u043f\u043d\u044b\u043c \u0442\u0438\u043f\u043e\u043c \u043c\u0430\u0440\u043a\u0435\u0440\u0430")]
            public string HexTypeMarkerAcces;
            [JsonProperty("HEX \u043a\u043d\u043e\u043f\u043a\u0438 \u0441 \u043d\u0435\u0434\u043e\u0441\u0442\u0443\u043f\u043d\u044b\u043c \u0442\u0438\u043f\u043e\u043c \u043c\u0430\u0440\u043a\u0435\u0440\u0430")]
            public string HexTypeMarkerDeading;
            [JsonProperty("HEX \u043a\u043d\u043e\u043f\u043a\u0438 \u0441 \u0432\u044b\u0431\u0440\u0430\u043d\u043d\u044b\u043c \u0442\u0438\u043f\u043e\u043c \u043c\u0430\u0440\u043a\u0435\u0440\u0430")]
            public string HexTypeMarkerTaking;
        }

    }

    public static Configuration GetNewConfiguration();
}

internal class AccessSetting
{
    [JsonProperty("true - \u0432\u043a\u043b\u044e\u0447\u0438\u0442\u044c \u043d\u0430\u0441\u0442\u0440\u043e\u0439\u043a\u0443 \u043c\u0430\u0440\u043a\u0435\u0440\u043e\u0432/false - \u043e\u0442\u043a\u043b\u044e\u0447\u0438\u0442\u044c \u043d\u0430\u0441\u0442\u0440\u043e\u0439\u043a\u0443")]
    public bool UseSettingsMarker;
    [JsonProperty("\u041d\u0430\u0441\u0442\u0440\u043e\u0439\u043a\u0430 \u0434\u043e\u0441\u0442\u0443\u043f\u043d\u044b\u0445 \u0446\u0432\u0435\u0442\u043e\u0432 \u0434\u043b\u044f \u043c\u0430\u0440\u043a\u0435\u0440\u0430")]
    public List<ColorList> ColorListMarker;
    internal class ColorList
    {
        [JsonProperty("\u041f\u0440\u0430\u0432\u0430 \u0434\u043e\u0441\u0442\u0443\u043f\u0430 \u0434\u043b\u044f \u044d\u0442\u043e\u0433\u043e \u0446\u0432\u0435\u0442\u0430")]
        public string Permissions;
        [JsonProperty("HEX \u0446\u0432\u0435\u0442")]
        public string HexColor;
    }

}

internal class ColorList
{
    [JsonProperty("\u041f\u0440\u0430\u0432\u0430 \u0434\u043e\u0441\u0442\u0443\u043f\u0430 \u0434\u043b\u044f \u044d\u0442\u043e\u0433\u043e \u0446\u0432\u0435\u0442\u0430")]
    public string Permissions;
    [JsonProperty("HEX \u0446\u0432\u0435\u0442")]
    public string HexColor;
}

internal class UserSettings
{
    [JsonProperty("\u0412\u043a\u043b\u044e\u0447\u0435\u043d \u043b\u0438 \u043c\u0430\u0440\u043a\u0435\u0440 \u0434\u043b\u044f \u043d\u043e\u0432\u044b\u0445 \u0438\u0433\u0440\u043e\u043a\u043e\u0432?(true - \u0434\u0430/false - \u043d\u0435\u0442)")]
    public bool DefaultTurnOnMarker;
    [JsonProperty("\u0421\u0442\u0430\u043d\u0434\u0430\u0440\u0442\u043d\u044b\u0439 \u0446\u0432\u0435\u0442 \u043c\u0430\u0440\u043a\u0435\u0440\u0430 \u0434\u043b\u044f \u043d\u043e\u0432\u044b\u0445 \u0438\u0433\u0440\u043e\u043a\u043e\u0432(HEX)")]
    public string DefaultHexMarker;
    [JsonProperty("\u0421\u0442\u0430\u043d\u0434\u0430\u0440\u0442\u043d\u044b\u0439 \u0432\u0438\u0434 \u043c\u0430\u0440\u043a\u0435\u0440\u0430 \u0434\u043b\u044f \u043d\u043e\u0432\u044b\u0445 \u0438\u0433\u0440\u043a\u043e\u043e\u0432 (0 - \u0422\u0435\u043a\u0441\u0442 \u0441 \u0434\u0430\u043c\u0430\u0433\u043e\u043c | 1 - \u041f\u043e\u043b\u043e\u0441\u0430 \u0425\u041f | 2 - \u0418\u043a\u043e\u043d\u043a\u0430)")]
    public MarkerType DefaultMarkerType;
    [JsonProperty("\u0421\u0442\u0430\u043d\u0434\u0430\u0440\u0442\u043d\u044b\u0439 \u0440\u0430\u0437\u043c\u0435\u0440 \u043c\u0430\u0440\u043a\u0435\u0440\u0430 \u0434\u043b\u044f \u043d\u043e\u0432\u044b\u0445 \u0438\u0433\u0440\u043a\u043e\u043e\u0432 (0 - \u041c\u0430\u043b\u0435\u043d\u044c\u043a\u0438\u0439 | 1 - \u0421\u0440\u0435\u0434\u043d\u0438\u0439 | 2 - \u0411\u043e\u043b\u044c\u0448\u043e\u0439)")]
    public TypeSize DefaultMarkerSize;
    [JsonProperty("\u0414\u043e\u043f\u043e\u043b\u043d\u0438\u0442\u0435\u043b\u044c\u043d\u044b\u0435 \u043d\u0430\u0441\u0442\u0440\u043e\u0439\u043a\u0438 \u043f\u043e\u043b\u043e\u0441\u044b \u0425\u041f \u0434\u043b\u044f \u043d\u043e\u0432\u044b\u0445 \u0438\u0433\u0440\u043a\u043e\u0432")]
    public HealthBar DefaultHealthBar;
    [JsonProperty("\u0414\u043e\u043f\u043e\u043b\u043d\u0438\u0442\u0435\u043b\u044c\u043d\u044b\u0435 \u043d\u0430\u0441\u0442\u0440\u043e\u0439\u043a\u0438 \u0442\u0435\u043a\u0441\u0442\u0430 \u0441 \u0443\u0440\u043e\u043d\u043e\u043c \u0434\u043b\u044f \u043d\u043e\u0432\u044b\u0445 \u0438\u0433\u0440\u043a\u043e\u0432")]
    public DamageText DefaultDamageText;
    [JsonProperty("\u0414\u043e\u043f\u043e\u043b\u043d\u0438\u0442\u0435\u043b\u044c\u043d\u044b\u0435 \u043d\u0430\u0441\u0442\u0440\u043e\u0439\u043a\u0438 \u0438\u043a\u043e\u043d\u043a\u0438 \u0434\u043b\u044f \u043d\u043e\u0432\u044b\u0445 \u0438\u0433\u0440\u043a\u043e\u0432")]
    public Icon DefaultIcon;
    internal class HealthBar
    {
        [JsonProperty("\u0412\u043a\u043b\u044e\u0447\u0438\u0442\u044c \u043e\u0442\u043e\u0431\u0440\u0430\u0436\u0435\u043d\u0438\u0435 \u0442\u0435\u043a\u0441\u0442\u0430 \u0441 \u0434\u0430\u043c\u0430\u0433\u043e\u043c(true - \u0434\u0430/false - \u043d\u0435\u0442)")]
        public bool TurnDamageText;
        [JsonProperty("\u0412\u043a\u043b\u044e\u0447\u0438\u0442\u044c \u043e\u0442\u043e\u0431\u0440\u0430\u0436\u0435\u043d\u0438\u0435 \u0438\u043a\u043e\u043d\u043a\u0438 \u043f\u0430\u0434\u0435\u043d\u0438\u044f(true - \u0434\u0430/false - \u043d\u0435\u0442)")]
        public bool TurnWounded;
    }

    internal class DamageText
    {
        [JsonProperty("\u0412\u043a\u043b\u044e\u0447\u0438\u0442\u044c \u043e\u0442\u043e\u0431\u0440\u0430\u0436\u0435\u043d\u0438\u0435 \u0442\u0435\u043a\u0441\u0442\u0430 \u0441 \u0434\u0430\u043c\u0430\u0433\u043e\u043c(true - \u0434\u0430 /false - \u043d\u0435\u0442)")]
        public bool TurnWounded;
    }

    internal class Icon
    {
        [JsonProperty("\u0412\u043a\u043b\u044e\u0447\u0438\u0442\u044c \u043e\u0442\u043e\u0431\u0440\u0430\u0436\u0435\u043d\u0438\u0435 \u0442\u0435\u043a\u0441\u0442\u0430 \u0441 \u0434\u0430\u043c\u0430\u0433\u043e\u043c(true - \u0434\u0430 / false - \u043d\u0435\u0442)")]
        public bool TurnDamageText;
        [JsonProperty("\u0412\u043a\u043b\u044e\u0447\u0438\u0442\u044c \u043e\u0442\u043e\u0431\u0440\u0430\u0436\u0435\u043d\u0438\u0435 \u0438\u043a\u043e\u043d\u043a\u0438 \u043f\u0430\u0434\u0435\u043d\u0438\u044f(true - \u0434\u0430/false - \u043d\u0435\u0442)")]
        public bool TurnWounded;
    }

}

internal class HealthBar
{
    [JsonProperty("\u0412\u043a\u043b\u044e\u0447\u0438\u0442\u044c \u043e\u0442\u043e\u0431\u0440\u0430\u0436\u0435\u043d\u0438\u0435 \u0442\u0435\u043a\u0441\u0442\u0430 \u0441 \u0434\u0430\u043c\u0430\u0433\u043e\u043c(true - \u0434\u0430/false - \u043d\u0435\u0442)")]
    public bool TurnDamageText;
    [JsonProperty("\u0412\u043a\u043b\u044e\u0447\u0438\u0442\u044c \u043e\u0442\u043e\u0431\u0440\u0430\u0436\u0435\u043d\u0438\u0435 \u0438\u043a\u043e\u043d\u043a\u0438 \u043f\u0430\u0434\u0435\u043d\u0438\u044f(true - \u0434\u0430/false - \u043d\u0435\u0442)")]
    public bool TurnWounded;
}

internal class DamageText
{
    [JsonProperty("\u0412\u043a\u043b\u044e\u0447\u0438\u0442\u044c \u043e\u0442\u043e\u0431\u0440\u0430\u0436\u0435\u043d\u0438\u0435 \u0442\u0435\u043a\u0441\u0442\u0430 \u0441 \u0434\u0430\u043c\u0430\u0433\u043e\u043c(true - \u0434\u0430 /false - \u043d\u0435\u0442)")]
    public bool TurnWounded;
}

internal class Icon
{
    [JsonProperty("\u0412\u043a\u043b\u044e\u0447\u0438\u0442\u044c \u043e\u0442\u043e\u0431\u0440\u0430\u0436\u0435\u043d\u0438\u0435 \u0442\u0435\u043a\u0441\u0442\u0430 \u0441 \u0434\u0430\u043c\u0430\u0433\u043e\u043c(true - \u0434\u0430 / false - \u043d\u0435\u0442)")]
    public bool TurnDamageText;
    [JsonProperty("\u0412\u043a\u043b\u044e\u0447\u0438\u0442\u044c \u043e\u0442\u043e\u0431\u0440\u0430\u0436\u0435\u043d\u0438\u0435 \u0438\u043a\u043e\u043d\u043a\u0438 \u043f\u0430\u0434\u0435\u043d\u0438\u044f(true - \u0434\u0430/false - \u043d\u0435\u0442)")]
    public bool TurnWounded;
}

internal class MarkersSetting
{
    [JsonProperty("\u041d\u0430\u0441\u0442\u0440\u043e\u0439\u043a\u0430 \u043c\u0430\u0440\u043a\u0435\u0440\u0430 HealthBar")]
    public HealthBarSettings HealthBarSetting;
    [JsonProperty("\u041d\u0430\u0441\u0442\u0440\u043e\u0439\u043a\u0430 \u043c\u0430\u0440\u043a\u0435\u0440\u0430 DamageText")]
    public DamageText DamageTextSetting;
    [JsonProperty("\u041d\u0430\u0441\u0442\u0440\u043e\u0439\u043a\u0430 \u043c\u0430\u0440\u043a\u0435\u0440\u0430 Icon")]
    public Icon IconSetting;
    internal class HealthBarSettings
    {
        [JsonProperty("\u041e\u0431\u0449\u0430\u044f \u043d\u0430\u0441\u0442\u0440\u043e\u0439\u043a\u0430")]
        public GeneralSettings GeneralSetting;
        [JsonProperty("\u041f\u0435\u0440\u043c\u0438\u0448\u0435\u043d\u0441 \u0434\u043b\u044f \u0434\u043e\u0441\u0442\u0443\u043f\u0430 \u043a \u0442\u0438\u043f\u0443 \u043c\u0430\u0440\u043a\u0435\u0440\u0430 HealthBar")]
        public string PermissionsHealthBar;
        [JsonProperty("\u041f\u0435\u0440\u043c\u0438\u0448\u0435\u043d\u0441 \u0434\u043b\u044f \u0434\u043e\u0441\u0442\u0443\u043f\u0430 \u0434\u043e\u043f\u043e\u043b\u043d\u0435\u043d\u0438\u044e HealthBar , \u043e\u0442\u043e\u0431\u0440\u0430\u0436\u0435\u043d\u0438\u0435 \u0443\u0440\u043e\u043d\u0430 \u0440\u044f\u0434\u043e\u043c \u0441 \u043f\u043e\u043a\u0430\u0437\u0430\u0442\u0435\u043b\u0435\u043c")]
        public string PermissionsHealtBarDamageText;
        [JsonProperty("\u041f\u0435\u0440\u043c\u0438\u0448\u0435\u043d\u0441 \u0434\u043b\u044f \u0434\u043e\u0441\u0442\u0443\u043f\u0430 \u0434\u043e\u043f\u043e\u043b\u043d\u0435\u043d\u0438\u044e HealthBar , \u043e\u0442\u043e\u0431\u0440\u0430\u0436\u0435\u043d\u0438\u0435 \u0438\u043a\u043e\u043d\u043a\u0438 \u0441 \u043f\u0430\u0434\u0435\u043d\u0438\u0435\u043c \u0438\u0433\u0440\u043e\u043a\u0430")]
        public string PermissionsHealtBarWounded;
    }

    internal class DamageText
    {
        [JsonProperty("\u041e\u0431\u0449\u0430\u044f \u043d\u0430\u0441\u0442\u0440\u043e\u0439\u043a\u0430")]
        public GeneralSettings GeneralSetting;
        [JsonProperty("\u041f\u0435\u0440\u043c\u0438\u0448\u0435\u043d\u0441 \u0434\u043b\u044f \u0434\u043e\u0441\u0442\u0443\u043f\u0430 \u043a \u0442\u0438\u043f\u0443 \u043c\u0430\u0440\u043a\u0435\u0440\u0430 DamageText")]
        public string PermissionsDamageText;
        [JsonProperty("\u041f\u0435\u0440\u043c\u0438\u0448\u0435\u043d\u0441 \u0434\u043b\u044f \u0434\u043e\u0441\u0442\u0443\u043f\u0430 \u0434\u043e\u043f\u043e\u043b\u043d\u0435\u043d\u0438\u044e DamageText , \u043e\u0442\u043e\u0431\u0440\u0430\u0436\u0435\u043d\u0438\u0435 \u0438\u043a\u043e\u043d\u043a\u0438 \u0441 \u043f\u0430\u0434\u0435\u043d\u0438\u0435\u043c \u0438\u0433\u0440\u043e\u043a\u0430")]
        public string PermissionsDamageTextWounded;
    }

    internal class Icon
    {
        [JsonProperty("\u041e\u0431\u0449\u0430\u044f \u043d\u0430\u0441\u0442\u0440\u043e\u0439\u043a\u0430")]
        public GeneralSettings GeneralSetting;
        [JsonProperty("\u041f\u0435\u0440\u043c\u0438\u0448\u0435\u043d\u0441 \u0434\u043b\u044f \u0434\u043e\u0441\u0442\u0443\u043f\u0430 \u043a \u0442\u0438\u043f\u0443 \u043c\u0430\u0440\u043a\u0435\u0440\u0430 Icon")]
        public string PermissionsIcon;
        [JsonProperty("\u041f\u0435\u0440\u043c\u0438\u0448\u0435\u043d\u0441 \u0434\u043b\u044f \u0434\u043e\u0441\u0442\u0443\u043f\u0430 \u0434\u043e\u043f\u043e\u043b\u043d\u0435\u043d\u0438\u044e Icon , \u043e\u0442\u043e\u0431\u0440\u0430\u0436\u0435\u043d\u0438\u0435 \u0443\u0440\u043e\u043d\u0430 \u0440\u044f\u0434\u043e\u043c \u0441 \u043f\u043e\u043a\u0430\u0437\u0430\u0442\u0435\u043b\u0435\u043c")]
        public string PermissionsIconDamageText;
        [JsonProperty("\u041f\u0435\u0440\u043c\u0438\u0448\u0435\u043d\u0441 \u0434\u043b\u044f \u0434\u043e\u0441\u0442\u0443\u043f\u0430 \u0434\u043e\u043f\u043e\u043b\u043d\u0435\u043d\u0438\u044e HealthBar , \u043e\u0442\u043e\u0431\u0440\u0430\u0436\u0435\u043d\u0438\u0435 \u0438\u043a\u043e\u043d\u043a\u0438 \u0441 \u043f\u0430\u0434\u0435\u043d\u0438\u0435\u043c \u0438\u0433\u0440\u043e\u043a\u0430")]
        public string PermissionsIconWounded;
        [JsonProperty("\u0421\u043f\u0438\u0441\u043e\u043a \u0438\u043a\u043e\u043d\u043e\u043a(\u041d\u0430\u0437\u0432\u0430\u043d\u0438\u0435 - \u0441\u0441\u044b\u043b\u043a\u0430 \u043d\u0430 \u0438\u043a\u043e\u043d\u043a\u0443 64\u044564)")]
        public List<IconListClass> IconList;
        internal class IconListClass
        {
            public string Permissions;
            public string PNG;
        }

    }

    internal class GeneralSettings
    {
        [JsonProperty("\u041e\u0442\u043e\u0431\u0440\u0430\u0436\u0430\u0435\u043c\u043e\u0435 \u0438\u043c\u044f")]
        public string DisplayName;
        [JsonProperty("\u041e\u043f\u0438\u0441\u0430\u043d\u0438\u0435")]
        public string Description;
    }

}

internal class HealthBarSettings
{
    [JsonProperty("\u041e\u0431\u0449\u0430\u044f \u043d\u0430\u0441\u0442\u0440\u043e\u0439\u043a\u0430")]
    public GeneralSettings GeneralSetting;
    [JsonProperty("\u041f\u0435\u0440\u043c\u0438\u0448\u0435\u043d\u0441 \u0434\u043b\u044f \u0434\u043e\u0441\u0442\u0443\u043f\u0430 \u043a \u0442\u0438\u043f\u0443 \u043c\u0430\u0440\u043a\u0435\u0440\u0430 HealthBar")]
    public string PermissionsHealthBar;
    [JsonProperty("\u041f\u0435\u0440\u043c\u0438\u0448\u0435\u043d\u0441 \u0434\u043b\u044f \u0434\u043e\u0441\u0442\u0443\u043f\u0430 \u0434\u043e\u043f\u043e\u043b\u043d\u0435\u043d\u0438\u044e HealthBar , \u043e\u0442\u043e\u0431\u0440\u0430\u0436\u0435\u043d\u0438\u0435 \u0443\u0440\u043e\u043d\u0430 \u0440\u044f\u0434\u043e\u043c \u0441 \u043f\u043e\u043a\u0430\u0437\u0430\u0442\u0435\u043b\u0435\u043c")]
    public string PermissionsHealtBarDamageText;
    [JsonProperty("\u041f\u0435\u0440\u043c\u0438\u0448\u0435\u043d\u0441 \u0434\u043b\u044f \u0434\u043e\u0441\u0442\u0443\u043f\u0430 \u0434\u043e\u043f\u043e\u043b\u043d\u0435\u043d\u0438\u044e HealthBar , \u043e\u0442\u043e\u0431\u0440\u0430\u0436\u0435\u043d\u0438\u0435 \u0438\u043a\u043e\u043d\u043a\u0438 \u0441 \u043f\u0430\u0434\u0435\u043d\u0438\u0435\u043c \u0438\u0433\u0440\u043e\u043a\u0430")]
    public string PermissionsHealtBarWounded;
}

internal class DamageText
{
    [JsonProperty("\u041e\u0431\u0449\u0430\u044f \u043d\u0430\u0441\u0442\u0440\u043e\u0439\u043a\u0430")]
    public GeneralSettings GeneralSetting;
    [JsonProperty("\u041f\u0435\u0440\u043c\u0438\u0448\u0435\u043d\u0441 \u0434\u043b\u044f \u0434\u043e\u0441\u0442\u0443\u043f\u0430 \u043a \u0442\u0438\u043f\u0443 \u043c\u0430\u0440\u043a\u0435\u0440\u0430 DamageText")]
    public string PermissionsDamageText;
    [JsonProperty("\u041f\u0435\u0440\u043c\u0438\u0448\u0435\u043d\u0441 \u0434\u043b\u044f \u0434\u043e\u0441\u0442\u0443\u043f\u0430 \u0434\u043e\u043f\u043e\u043b\u043d\u0435\u043d\u0438\u044e DamageText , \u043e\u0442\u043e\u0431\u0440\u0430\u0436\u0435\u043d\u0438\u0435 \u0438\u043a\u043e\u043d\u043a\u0438 \u0441 \u043f\u0430\u0434\u0435\u043d\u0438\u0435\u043c \u0438\u0433\u0440\u043e\u043a\u0430")]
    public string PermissionsDamageTextWounded;
}

internal class Icon
{
    [JsonProperty("\u041e\u0431\u0449\u0430\u044f \u043d\u0430\u0441\u0442\u0440\u043e\u0439\u043a\u0430")]
    public GeneralSettings GeneralSetting;
    [JsonProperty("\u041f\u0435\u0440\u043c\u0438\u0448\u0435\u043d\u0441 \u0434\u043b\u044f \u0434\u043e\u0441\u0442\u0443\u043f\u0430 \u043a \u0442\u0438\u043f\u0443 \u043c\u0430\u0440\u043a\u0435\u0440\u0430 Icon")]
    public string PermissionsIcon;
    [JsonProperty("\u041f\u0435\u0440\u043c\u0438\u0448\u0435\u043d\u0441 \u0434\u043b\u044f \u0434\u043e\u0441\u0442\u0443\u043f\u0430 \u0434\u043e\u043f\u043e\u043b\u043d\u0435\u043d\u0438\u044e Icon , \u043e\u0442\u043e\u0431\u0440\u0430\u0436\u0435\u043d\u0438\u0435 \u0443\u0440\u043e\u043d\u0430 \u0440\u044f\u0434\u043e\u043c \u0441 \u043f\u043e\u043a\u0430\u0437\u0430\u0442\u0435\u043b\u0435\u043c")]
    public string PermissionsIconDamageText;
    [JsonProperty("\u041f\u0435\u0440\u043c\u0438\u0448\u0435\u043d\u0441 \u0434\u043b\u044f \u0434\u043e\u0441\u0442\u0443\u043f\u0430 \u0434\u043e\u043f\u043e\u043b\u043d\u0435\u043d\u0438\u044e HealthBar , \u043e\u0442\u043e\u0431\u0440\u0430\u0436\u0435\u043d\u0438\u0435 \u0438\u043a\u043e\u043d\u043a\u0438 \u0441 \u043f\u0430\u0434\u0435\u043d\u0438\u0435\u043c \u0438\u0433\u0440\u043e\u043a\u0430")]
    public string PermissionsIconWounded;
    [JsonProperty("\u0421\u043f\u0438\u0441\u043e\u043a \u0438\u043a\u043e\u043d\u043e\u043a(\u041d\u0430\u0437\u0432\u0430\u043d\u0438\u0435 - \u0441\u0441\u044b\u043b\u043a\u0430 \u043d\u0430 \u0438\u043a\u043e\u043d\u043a\u0443 64\u044564)")]
    public List<IconListClass> IconList;
    internal class IconListClass
    {
        public string Permissions;
        public string PNG;
    }

}

internal class IconListClass
{
    public string Permissions;
    public string PNG;
}

internal class GeneralSettings
{
    [JsonProperty("\u041e\u0442\u043e\u0431\u0440\u0430\u0436\u0430\u0435\u043c\u043e\u0435 \u0438\u043c\u044f")]
    public string DisplayName;
    [JsonProperty("\u041e\u043f\u0438\u0441\u0430\u043d\u0438\u0435")]
    public string Description;
}

internal class SettingsInterface
{
    [JsonProperty("\u041d\u0430\u0441\u0442\u0440\u043e\u0439\u043a\u0430 \u043e\u0441\u043d\u043e\u0432\u043d\u0432\u043e\u0439 \u043f\u0430\u043d\u0435\u043b\u0438")]
    public MainPanel InterfaceMainPanel;
    [JsonProperty("\u041d\u0430\u0441\u0442\u0440\u043e\u0439\u043a\u0430 \u043f\u0430\u043d\u0435\u043b\u0438 \u0441 \u043d\u0430\u0441\u0442\u0440\u043e\u0439\u043a\u043e\u0439 \u043c\u0430\u0440\u043a\u0435\u0440\u0430")]
    public SettingsMarker InterfaceSettingsMarker;
    [JsonProperty("HEX \u0446\u0432\u0435\u0442\u0430 \u0442\u0435\u043a\u0441\u0442\u0430")]
    public string HexLabels;
    internal class MainPanel
    {
        [JsonProperty("HEX \u0437\u0430\u0434\u043d\u0435\u0433\u043e \u0444\u043e\u043d\u0430")]
        public string HexBackground;
        [JsonProperty("Material \u0437\u0430\u0434\u043d\u0435\u0433\u043e \u0444\u043e\u043d\u0430")]
        public string MaterialBackground;
        [JsonProperty("Sprite \u043b\u043e\u0433\u043e\u0442\u0438\u043f\u0430")]
        public string SpriteLogoBackground;
    }

    internal class SettingsMarker
    {
        [JsonProperty("HEX \u043a\u043d\u043e\u043f\u043a\u0438 \u043d\u0430\u0441\u0442\u0440\u043e\u0439\u043a\u0438")]
        public string HexButtonMain;
        [JsonProperty("HEX \u043f\u043e\u043b\u044f \u0432\u0432\u043e\u0434\u0430 \u0434\u043b\u044f \u043d\u0430\u0441\u0442\u0440\u043e\u0439\u043a\u0438 \u0446\u0432\u0435\u0442\u0430 \u043c\u0430\u0440\u043a\u0435\u0440\u0430")]
        public string HexButtonInputPanel;
        [JsonProperty("HEX \u0432\u044b\u0431\u0440\u0430\u043d\u043d\u043e\u0433\u043e \u044d\u043b\u0435\u043c\u0435\u043d\u0442\u0430 \u043e\u0442\u043e\u0431\u0440\u0430\u0436\u0435\u043d\u0438\u044f \u043c\u0430\u0440\u043a\u0435\u0440\u0430")]
        public string HexActiveShowMarker;
        [JsonProperty("HEX \u043d\u0435 \u0432\u044b\u0431\u0440\u0430\u043d\u043d\u043e\u0433\u043e \u044d\u043b\u0435\u043c\u0435\u043d\u0442\u0430 \u043e\u0442\u043e\u0431\u0440\u0430\u0436\u0435\u043d\u0438\u044f \u043c\u0430\u0440\u043a\u0435\u0440\u0430")]
        public string HexInActiveShowMarker;
        [JsonProperty("Sprite \u043d\u0435 \u0432\u044b\u0431\u0440\u0430\u043d\u043d\u043e\u0433\u043e \u044d\u043b\u0435\u043c\u0435\u043d\u0442\u0430 \u043e\u0442\u043e\u0431\u0440\u0430\u0436\u0435\u043d\u0438\u044f \u043c\u0430\u0440\u043a\u0435\u0440\u0430")]
        public string SpriteActiveShowMarker;
        [JsonProperty("Sprite \u0432\u044b\u0431\u0440\u0430\u043d\u043d\u043e\u0433\u043e \u044d\u043b\u0435\u043c\u0435\u043d\u0442\u0430 \u043e\u0442\u043e\u0431\u0440\u0430\u0436\u0435\u043d\u0438\u044f \u043c\u0430\u0440\u043a\u0435\u0440\u0430")]
        public string SpriteInActiveShowMarker;
        [JsonProperty("HEX \u043f\u0430\u043d\u0435\u043b\u0438 \u0441 \u0442\u0438\u043f\u043e\u043c \u043c\u0430\u0440\u043a\u0435\u0440\u0430")]
        public string HexTypeMarkerPanel;
        [JsonProperty("HEX \u043a\u043d\u043e\u043f\u043a\u0438 \u0441 \u0434\u043e\u0441\u0442\u0443\u043f\u043d\u044b\u043c \u0442\u0438\u043f\u043e\u043c \u043c\u0430\u0440\u043a\u0435\u0440\u0430")]
        public string HexTypeMarkerAcces;
        [JsonProperty("HEX \u043a\u043d\u043e\u043f\u043a\u0438 \u0441 \u043d\u0435\u0434\u043e\u0441\u0442\u0443\u043f\u043d\u044b\u043c \u0442\u0438\u043f\u043e\u043c \u043c\u0430\u0440\u043a\u0435\u0440\u0430")]
        public string HexTypeMarkerDeading;
        [JsonProperty("HEX \u043a\u043d\u043e\u043f\u043a\u0438 \u0441 \u0432\u044b\u0431\u0440\u0430\u043d\u043d\u044b\u043c \u0442\u0438\u043f\u043e\u043c \u043c\u0430\u0440\u043a\u0435\u0440\u0430")]
        public string HexTypeMarkerTaking;
    }

}

internal class MainPanel
{
    [JsonProperty("HEX \u0437\u0430\u0434\u043d\u0435\u0433\u043e \u0444\u043e\u043d\u0430")]
    public string HexBackground;
    [JsonProperty("Material \u0437\u0430\u0434\u043d\u0435\u0433\u043e \u0444\u043e\u043d\u0430")]
    public string MaterialBackground;
    [JsonProperty("Sprite \u043b\u043e\u0433\u043e\u0442\u0438\u043f\u0430")]
    public string SpriteLogoBackground;
}

internal class SettingsMarker
{
    [JsonProperty("HEX \u043a\u043d\u043e\u043f\u043a\u0438 \u043d\u0430\u0441\u0442\u0440\u043e\u0439\u043a\u0438")]
    public string HexButtonMain;
    [JsonProperty("HEX \u043f\u043e\u043b\u044f \u0432\u0432\u043e\u0434\u0430 \u0434\u043b\u044f \u043d\u0430\u0441\u0442\u0440\u043e\u0439\u043a\u0438 \u0446\u0432\u0435\u0442\u0430 \u043c\u0430\u0440\u043a\u0435\u0440\u0430")]
    public string HexButtonInputPanel;
    [JsonProperty("HEX \u0432\u044b\u0431\u0440\u0430\u043d\u043d\u043e\u0433\u043e \u044d\u043b\u0435\u043c\u0435\u043d\u0442\u0430 \u043e\u0442\u043e\u0431\u0440\u0430\u0436\u0435\u043d\u0438\u044f \u043c\u0430\u0440\u043a\u0435\u0440\u0430")]
    public string HexActiveShowMarker;
    [JsonProperty("HEX \u043d\u0435 \u0432\u044b\u0431\u0440\u0430\u043d\u043d\u043e\u0433\u043e \u044d\u043b\u0435\u043c\u0435\u043d\u0442\u0430 \u043e\u0442\u043e\u0431\u0440\u0430\u0436\u0435\u043d\u0438\u044f \u043c\u0430\u0440\u043a\u0435\u0440\u0430")]
    public string HexInActiveShowMarker;
    [JsonProperty("Sprite \u043d\u0435 \u0432\u044b\u0431\u0440\u0430\u043d\u043d\u043e\u0433\u043e \u044d\u043b\u0435\u043c\u0435\u043d\u0442\u0430 \u043e\u0442\u043e\u0431\u0440\u0430\u0436\u0435\u043d\u0438\u044f \u043c\u0430\u0440\u043a\u0435\u0440\u0430")]
    public string SpriteActiveShowMarker;
    [JsonProperty("Sprite \u0432\u044b\u0431\u0440\u0430\u043d\u043d\u043e\u0433\u043e \u044d\u043b\u0435\u043c\u0435\u043d\u0442\u0430 \u043e\u0442\u043e\u0431\u0440\u0430\u0436\u0435\u043d\u0438\u044f \u043c\u0430\u0440\u043a\u0435\u0440\u0430")]
    public string SpriteInActiveShowMarker;
    [JsonProperty("HEX \u043f\u0430\u043d\u0435\u043b\u0438 \u0441 \u0442\u0438\u043f\u043e\u043c \u043c\u0430\u0440\u043a\u0435\u0440\u0430")]
    public string HexTypeMarkerPanel;
    [JsonProperty("HEX \u043a\u043d\u043e\u043f\u043a\u0438 \u0441 \u0434\u043e\u0441\u0442\u0443\u043f\u043d\u044b\u043c \u0442\u0438\u043f\u043e\u043c \u043c\u0430\u0440\u043a\u0435\u0440\u0430")]
    public string HexTypeMarkerAcces;
    [JsonProperty("HEX \u043a\u043d\u043e\u043f\u043a\u0438 \u0441 \u043d\u0435\u0434\u043e\u0441\u0442\u0443\u043f\u043d\u044b\u043c \u0442\u0438\u043f\u043e\u043c \u043c\u0430\u0440\u043a\u0435\u0440\u0430")]
    public string HexTypeMarkerDeading;
    [JsonProperty("HEX \u043a\u043d\u043e\u043f\u043a\u0438 \u0441 \u0432\u044b\u0431\u0440\u0430\u043d\u043d\u044b\u043c \u0442\u0438\u043f\u043e\u043c \u043c\u0430\u0440\u043a\u0435\u0440\u0430")]
    public string HexTypeMarkerTaking;
}

public class SettingsUser
{
    public bool TurnMarker;
    public bool HitAnimal;
    public bool HitPlayer;
    public bool HitBuilding;
    public bool HitEntity;
    public bool HitTransport;
    public bool HitNPC;
    public string HexMarker;
    public TypeSize typeSize;
    public MarkerType markerType;
    public HealthBar HealthBarMore;
    public DamageText DamageTextMore;
    public Icon IconMore;
    internal class HealthBar
    {
        public bool TurnDamageText;
        public bool TurnWounded;
    }

    internal class DamageText
    {
        public bool TurnWounded;
    }

    internal class Icon
    {
        public bool TurnDamageText;
        public bool TurnWounded;
        public int IndexMarker;
    }

}

internal class HealthBar
{
    public bool TurnDamageText;
    public bool TurnWounded;
}

internal class DamageText
{
    public bool TurnWounded;
}

internal class Icon
{
    public bool TurnDamageText;
    public bool TurnWounded;
    public int IndexMarker;
}

public class ResizeClass
{
    public HealthBar healthBarCoordinate;
    public string AnchorMin;
    public string AnchorMax;
    public int FontSize;
    internal class HealthBar
    {
        public string AnchorMin;
        public string AnchorMax;
    }

}

internal class HealthBar
{
    public string AnchorMin;
    public string AnchorMax;
}

public class ResizerAttackHit
{
    public readonly Dictionary<TypeSize, TypeResize> ResizerAttack;
    internal class TypeResize
    {
        public Params IconParams;
        public Params HealthBar;
        public int FontSizeText;
        internal class Params
        {
            public string AnchorMin;
            public string AnchorMax;
        }

    }

}

internal class TypeResize
{
    public Params IconParams;
    public Params HealthBar;
    public int FontSizeText;
    internal class Params
    {
        public string AnchorMin;
        public string AnchorMax;
    }

}

internal class Params
{
    public string AnchorMin;
    public string AnchorMax;
}


```

---

## IQMenu

```csharp
using System;
using System.Collections.Generic;
using System.Globalization;
using Newtonsoft.Json;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using UnityEngine;

Oxide.Plugins
[Info("IQMenu", "Mercury", "0.0.1")]
[Description("Ясно клоун")]
 class IQMenu : RustPlugin
{
    [PluginReference]
     Plugin ImageLibrary;
    private string GetImage(string fileName, ulong skin);
    public bool AddImage(string url, string shortname, ulong skin);
    public List<ulong> PlayerOpenMenu;
    private static Configuration config;
    private class Configuration
    {
        [JsonProperty("Настройка меню")]
        public List<MenuClass> MenuSettings;
        [JsonProperty("Иконка для главного меню")]
        public string UrlMenu;
        [JsonProperty("Название сервера для главного меню")]
        public string ServerName;
        [JsonProperty("Название кнопки главного меню")]
        public string ButtonName;
        [JsonProperty("Настройка броадкаста")]
        public List<string> BroadCastList;
        internal class MenuClass
        {
            [JsonProperty("Иконка для кнопки")]
            public string URLIco;
            [JsonProperty("Название для кнопки")]
            public string DisplayName;
            [JsonProperty("Команда для кнопки")]
            public string Command;
        }

        public static Configuration GetNewConfiguration();
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    public static string MENU_PARENT;
    public static string DROP_MENU_PANEL;
    public static string BROADCAST_PARENT;
     void InterfaceMenu(BasePlayer player);
     void UpdateOnlineLabel();
     void BroadCast();
     void DropListMenu(BasePlayer player);
    private void OnServerInitialized();
    private void OnPlayerInit(BasePlayer player);
     void Unload();
    [ConsoleCommand("pmenu")]
     void PerMentCommand(ConsoleSystem.Arg arg);
     void LoadImage();
     bool IsOpenMenu(BasePlayer player);
    private static string HexToRustFormat(string hex);
}

private class Configuration
{
    [JsonProperty("Настройка меню")]
    public List<MenuClass> MenuSettings;
    [JsonProperty("Иконка для главного меню")]
    public string UrlMenu;
    [JsonProperty("Название сервера для главного меню")]
    public string ServerName;
    [JsonProperty("Название кнопки главного меню")]
    public string ButtonName;
    [JsonProperty("Настройка броадкаста")]
    public List<string> BroadCastList;
    internal class MenuClass
    {
        [JsonProperty("Иконка для кнопки")]
        public string URLIco;
        [JsonProperty("Название для кнопки")]
        public string DisplayName;
        [JsonProperty("Команда для кнопки")]
        public string Command;
    }

    public static Configuration GetNewConfiguration();
}

internal class MenuClass
{
    [JsonProperty("Иконка для кнопки")]
    public string URLIco;
    [JsonProperty("Название для кнопки")]
    public string DisplayName;
    [JsonProperty("Команда для кнопки")]
    public string Command;
}


```

---

## IQPermissions

```csharp
using Newtonsoft.Json;
using Oxide.Core.Plugins;
using System;
using System.Collections.Generic;
using System.Linq;
using Oxide.Core;
using Oxide.Game.Rust.Cui;
using System.Collections;
using System.Text;
using UnityEngine;
using UnityEngine.Networking;
using Object = System.Object;
using System.Text.RegularExpressions;
using ConVar;
using Newtonsoft.Json.Linq;
using Oxide.Core.Libraries;
using Oxide.Core.Database;

Oxide.Plugins
[Info("IQPermissions", "", "1.7.1")]
[Description("Extended privilege granting system")]
public class IQPermissions : RustPlugin
{
    private Boolean IsFullLoaded;
    private static IQPermissions _;
    private static InterfaceBuilder _interface;
    private const Boolean LanguageEn;
    [PluginReference]
     Plugin IQChat;
     Plugin ImageLibrary;
     Plugin UAlertSystem;
    public void SendChat(String Message, BasePlayer player, ConVar.Chat.ChatChannel channel);
    private String GetImage(String fileName, UInt64 skin);
    private static Configuration config;
    private class Configuration
    {
        [JsonProperty(LanguageEn ? "Basic Settings" : "Основные настройки")]
        public GeneralSetting GeneralSettings;
        [JsonProperty(LanguageEn ? "Configuring the interface" : "Настройка интерфейса")]
        public InterfaceConfiguration InterfaceSetting;
        [JsonProperty(LanguageEn ? "Setting up Notifications" : "Настройка уведомлений")]
        public AlertConfiguration AlertConfigurations;
        [JsonProperty(LanguageEn ? "PERMISSIONS ON THE SERVER : [Permission] = Customization" : "ПРАВА НА СЕРВЕРЕ : [Permission] = Настройка")]
        public Dictionary<String, StructureLanguage> PermissionList;
        [JsonProperty(LanguageEn ? "GROUPS WITH RIGHTS ON THE SERVER : [Group] = Customization" : "ГРУППЫ С ПРАВАМИ НА СЕРВЕРЕ : [Group] = Настройка")]
        public Dictionary<String, StructureLanguage> GroupList;
        internal class GeneralSetting
        {
            [JsonProperty(LanguageEn ? "Webhook from the Discord channel for logging" : "Webhook от канала Discord для логирования")]
            public String WebHooksDiscord;
            [JsonProperty(LanguageEn
					? "Connecting data storage on the MySQL side"
					: "Подключение хранения данных на стороне MySQL")]
            public MySQLInitialize MySQLSetting;
            [JsonProperty(LanguageEn ? "Use the removal of temporary groups from the player when unloading the plugin (when loading, they will be returned)" : "Использовать удаление временных групп у игрока при выгрузке плагина (при загрузке - они будут возвращены)")]
            public Boolean UseRemoveGroupsWhenUnload;
            [JsonProperty(LanguageEn ? "Use the removal of temporary permission from the player when unloading the plugin (when loading, they will be returned)" : "Использовать удаление временных прав у игрока при выгрузке плагина (при загрузке - они будут возвращены)")]
            public Boolean UseRemovePermissionsХWhenUnload;
            [JsonProperty(LanguageEn ? "Settings for automatically granting privileges to server novices (They will be considered novices - while the player is not in the database or datafile)" : "Настройки автоматической выдачи привилегии новичкам сервера (Они будут считаться новичками - пока игрока нет в базе или датафайле)")]
            public AutoGive AutoGiveSettings;
            internal class AutoGive
            {
                [JsonProperty(LanguageEn ? "Use automatic granting of permissions to a new player" : "Использовать автоматическую выдачу прав новому игроку")]
                public Boolean UseAutGivePermissions;
                [JsonProperty(LanguageEn ? "Use automatic granting of groups to a new player" : "Использовать автоматическую выдачу групп новому игроку")]
                public Boolean UseAutGiveGroups;
                [JsonProperty(LanguageEn ? "List of permissions to issue [group] = time in the format (1d/h/m/s)" : "Список прав для выдачи [группа] = время в формате (1d/h/m/s)")]
                public Dictionary<String, String> PermissionList;
                [JsonProperty(LanguageEn ? "List of groups to issue [group] = time in the format (1d/h/m/s)" : "Список групп для выдачи [группа] = время в формате (1d/h/m/s)")]
                public Dictionary<String, String> GroupList;
            }

            internal class MySQLInitialize
            {
                [JsonProperty(LanguageEn ? "Setting up a MySQL connection" : "Настройка MySQL соединения")]
                public MySQLConnect ConnectSetting;
                [JsonProperty(LanguageEn ? "The desired table name in MySQL" : "Желаемое название таблицы в MySQL")]
                public String TableName;
                [JsonProperty(LanguageEn
						? "Settings privilege migration between servers connected to the same MySQL (Example: Server #1 and #2 are connected to the same MySQL - a player with privileges on server #1 - going to server #2 - will get it there)"
						: "Настройка миграцию привилегий между среверами подключенными к одной MySQL (Пример : Сервер #1 и #2 подключены к одной MySQL - игрок с привилегий на сервере #1 - зайдя на сервер #2 - получит ее и там)")]
                public MigrationPrivilage MigrationSettings;
                internal class MigrationPrivilage
                {
                    [JsonProperty(LanguageEn ? "Use the list of permissions available for migration (otherwise they will be all that the player has in MySQL)" : "Использовать список прав доступных для миграции (иначе будут все, что есть у игрока в MySQL)")]
                    public Boolean UseMigrationPermissionsList;
                    [JsonProperty(LanguageEn ? "List of permissions in available for synchronization on another server in case of player migration" : "Список прав в доступных для синхронизации на другом сервере случае миграции игрока")]
                    public List<String> MigrationPermissionList;
                    [JsonProperty(LanguageEn ? "Use the list of groups available for migration (otherwise they will be all that the player has in MySQL)" : "Использовать список групп доступных для миграции (иначе будут все, что есть у игрока в MySQL)")]
                    public Boolean UseMigrationGroupList;
                    [JsonProperty(LanguageEn ? "List of groups in available for synchronization on another server in case of player migration" : "Список групп в доступных для синхронизации на другом сервере случае миграции игрока")]
                    public List<String> MigrationGroupList;
                }

                internal class MySQLConnect
                {
                    public String Host;
                    public String Port;
                    public String DatabaseName;
                    public String UserName;
                    public String Passowrd;
                }

            }

        }

        internal class AlertConfiguration
        {
            [JsonProperty(LanguageEn ? "Use the plugin's UI notification for notifications (otherwise it will be displayed in the chat)" : "Использовать для уведомлений UI-уведомление плагина (иначе будет отображаться в чате)")]
            public Boolean UsePoopUp;
            [JsonProperty(LanguageEn
					? "Remind players that they are running out of group?"
					: "Напоминать игрокам о том, что у них заканчивается группа?")]
            public Boolean UseAlertedGroup;
            [JsonProperty(LanguageEn
					? "The list of groups for which the reminder will work"
					: "Список групп на которые сработает напоминание")]
            public List<String> GrpupListAlerted;
            [JsonProperty(LanguageEn
					? "Remind players that they are running out of permission?"
					: "Напоминать игрокам о том, что у них заканчивается права?")]
            public Boolean UseAlertedPermission;
            [JsonProperty(LanguageEn
					? "The list of permissions for which the reminder will work"
					: "Список прав на которые сработает напоминание")]
            public List<String> PermissionsListAlerted;
            [JsonProperty(LanguageEn ? "How many days before the end of the privilege to remind the player about it" : "За сколько дней до окончания привилегии напоминать игроку об этом")]
            public Int32 DayAlerted;
            [JsonProperty(LanguageEn ? "How many seconds to show notifications to the player" : "Сколько секунд показывать уведомления игроку")]
            public Single TimeAlert;
            [JsonProperty(LanguageEn ? "Setting up IQChat (If installed)" : "Настройка IQChat (Если установлен)")]
            public IQChatSetting SettingIQChat;
            internal class IQChatSetting
            {
                [JsonProperty(LanguageEn ? "IQChat : Custom prefix in chat" : "IQChat : Кастомный префикс в чате")]
                public String CustomPrefix;
                [JsonProperty(LanguageEn
						? "IQChat : Custom avatar in the chat (If required)"
						: "IQChat : Кастомный аватар в чате(Если требуется)")]
                public String CustomAvatar;
                [JsonProperty(LanguageEn
						? "IQChat : Use UI notifications"
						: "IQChat : Использовать UI уведомления")]
                public Boolean UIAlertUse;
            }

        }

        internal class InterfaceConfiguration
        {
            [JsonProperty(LanguageEn ? "PNG : Link to the background of the notification" : "PNG : Ссылка на задний фон уведомления")]
            public String PNGBackground;
            [JsonProperty(LanguageEn ? "PNG : Link to the image when receiving the privilege" : "PNG : Ссылка на картинку при получении привилегии")]
            public String PNGPrivilageAdd;
            [JsonProperty(LanguageEn ? "PNG : Link to the picture at the end of the privilege" : "PNG : Ссылка на картинку при окончании привилегии")]
            public String PNGPrivilageExpired;
            [JsonProperty(LanguageEn ? "PNG : Link to the picture at the notification of the end of the privilege" : "PNG : Ссылка на картинку при уведомлении об окончании привилегии")]
            public String PNGPrivilageAlert;
            [JsonProperty(LanguageEn ? "RGBA : Header Color" : "RGBA : Цвет заголовка")]
            public String RGBAColorTitle;
            [JsonProperty(LanguageEn ? "RGBA : Additional text color" : "RGBA : Цвет дополнительного текста")]
            public String RGBAColorDescription;
        }

        internal class StructureLanguage
        {
            [JsonProperty(LanguageEn ? "Name in Russian (Not obligatory. Only for Russian players)" : "Название на русском")]
            public String RussianName;
            [JsonProperty(LanguageEn ? "Name in English" : "Название на английском")]
            public String EnglishName;
            public StructureLanguage(String russianName, String englishName);
        }

        public static Configuration GetNewConfiguration();
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private static String GetIcon(TypeAlert typeAlert);
    private String GetTitle(TypeAlert typeAlert);
    private String GetDescription(String UserID, TypeAlert typeAlert, String ReplaceKey, String DataExpired);
    private String GetLanguageNamePrivilage(BasePlayer player, String Key, TypeData type);
    [JsonProperty(LanguageEn ? "Information about privileges" : "Информация о привилегиях")]
    private StructureData InformationPrivilagesUser;
    [JsonProperty(LanguageEn
			? "Players who are no longer considered beginners"
			: "Игроки, которые более не считают новичками")]
    private List<UInt64> OldPlayers;
    private void SetupAutoPrivilages(BasePlayer player);
    internal class StructureData
    {
        [JsonProperty(LanguageEn ? "Information about player permissions" : "Информация о привилегиях игрока")]
        public Dictionary<String, Dictionary<UInt64, DateTime>> UserPermissions;
        [JsonProperty(LanguageEn ? "Information about player groups" : "Информация о группах игрока")]
        public Dictionary<String, Dictionary<UInt64, DateTime>> UserGrpups;
        public Dictionary<String, DateTime> GetParametresUser(UInt64 userID, TypeData typeDictonary);
        public Dictionary<UInt64, DateTime> GePlayerList(String Key, TypeData typeDictonary);
        public DateTime GetExpiredPermissionData(String Key, UInt64 userID, TypeData typeDictonary);
        public void SetParametres(String Key, UInt64 userID, DateTime dateExpired, TypeData typeDictonary, Boolean IsMigration);
        public void RemoveParametres(String Key, UInt64 userID, TypeData typeDictonary, DateTime timeRevoed);
        public void SetParametresUnload();
        public void RemoveParametresUnload();
        private Boolean IsExpiredPermission(String Key, UInt64 userID, TypeData typeDictonary);
        public void TrackerExpireds();
    }

    private void ReadData();
    private void WriteData();
    private void Init();
    private void OnServerInitialized();
    private void Unload();
    private void OnPlayerConnected(BasePlayer player);
    private void OnPermissionRegistered(String name, Plugin owner);
    private void OnGroupCreated(String name);
    private void OnUserGroupRemoved(String id, String groupName);
    private void OnUserPermissionRevoked(String id, String permName);
    private readonly Core.MySql.Libraries.MySql sqlLibrary;
    private Connection sqlConnection;
    private String SQL_Query_SelectedDatabase();
    private String SQL_Query_CreatedDatabase();
    private String SQL_Query_InsertUser();
    private void SQL_OpenConnection();
    private void InserDatabase(String UserID, TypeData typeData, String Key, DateTime dateExpired);
    private void DeleteDatabase(String UserID, TypeData typeData, String Key);
    private void MigrationPrivilage(BasePlayer player);
    private void SendDiscord(List<Fields> fields);
    private Dictionary<UInt64, Coroutine> RoutineQueue;
    private IEnumerator JoinedAlertPlayer(BasePlayer player);
    private void AlertPlayer(UInt64 userID, TypeAlert typeAlert, String Key, TypeData typeData, String DataExpired);
    private void NewPermissionRegistered(String Permission);
    private void NewGroupRegistered(String Group);
    private void GrablingGroups();
    private void GrablingPermissions();
    private void PreLoadedPlugin();
    private void StartPluginLoad();
    [ChatCommand("pinfo")]
    private void ChatCommand_Pinfo(BasePlayer player);
    [ConsoleCommand("migration.timedpermissions")]
    private void TimedPermissionsMigration(ConsoleSystem.Arg arg);
    [ConsoleCommand("migration.grant")]
    private void GrantMigration(ConsoleSystem.Arg arg);
    [ConsoleCommand("migration.timeprivilage")]
    private void TimePrivilageMigration(ConsoleSystem.Arg arg);
    [ConsoleCommand("grantperm")]
    private void GrantPerm_ConsoleCommand(ConsoleSystem.Arg arg);
    [ConsoleCommand("grant.permission")]
    private void GrantPerm_TwoConsoleCommand(ConsoleSystem.Arg arg);
    [ConsoleCommand("revokeperm")]
    private void RevokePerm_ConsoleCommand(ConsoleSystem.Arg arg);
    [ConsoleCommand("revoke.permission")]
    private void RevokePerm_TwoConsoleCommand(ConsoleSystem.Arg arg);
    [ConsoleCommand("addgroup")]
    private void AddGroup_ConsoleCommand(ConsoleSystem.Arg arg);
    [ConsoleCommand("grant.group")]
    private void AddGroup_TwoConsoleCommand(ConsoleSystem.Arg arg);
    [ConsoleCommand("revokegroup")]
    private void RemoveGroup_ConsoleCommand(ConsoleSystem.Arg arg);
    [ConsoleCommand("revoke.group")]
    private void RevomeGroup_TwoConsoleCommand(ConsoleSystem.Arg arg);
    [ConsoleCommand("perm.users")]
    private void GetInfoPerm_ConsoleCommand(ConsoleSystem.Arg arg);
    [ConsoleCommand("group.users")]
    private void GetInfoGroup_ConsoleCommand(ConsoleSystem.Arg arg);
    [ConsoleCommand("user.perms")]
    private void GetUserPerms_ConsoleCommand(ConsoleSystem.Arg arg);
    [ConsoleCommand("user.groups")]
    private void GetUserGroups_ConsoleCommand(ConsoleSystem.Arg arg);
    private void DrawUI_PoopUp(BasePlayer player, TypeAlert typeAlert, String Key, TypeData typeData, String DataExpired);
    private class InterfaceBuilder
    {
        public static InterfaceBuilder Instance;
        public const String UI_POOPUP_PANEL;
        public Dictionary<String, String> Interfaces;
        public InterfaceBuilder();
        public static void AddInterface(String name, String json);
        public static String GetInterface(String name);
        public static void DestroyAll();
        private void BuildingPlayer_PopUp();
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
            public Footer footer { get; set; }
            public Authors author { get; set; }
            public Embeds(String title, Int32 color, List<Fields> fields, Authors author, Footer footer);
        }

        public FancyMessage(String content, bool tts, Embeds[] embeds);
        public String toJSON();
    }

    public class Footer
    {
        public String text { get; set; }
        public String icon_url { get; set; }
        public String proxy_icon_url { get; set; }
        public Footer(String text, String icon_url, String proxy_icon_url);
    }

    public class Authors
    {
        public String name { get; set; }
        public String url { get; set; }
        public String icon_url { get; set; }
        public String proxy_icon_url { get; set; }
        public Authors(String name, String url, String icon_url, String proxy_icon_url);
    }

    public class Fields
    {
        public String name { get; set; }
        public String value { get; set; }
        public bool inline { get; set; }
        public Fields(String name, String value, bool inline);
    }

    private void Request(String url, String payload, Action<Int32> callback);
    private Boolean TryParseTimeSpan(String source, TimeSpan timeSpan);
    private String GetLeftTime(DateTime dateExpired, String userID);
    public String FormatTime(TimeSpan time, String UserID);
    private String Format(Int32 units, String form1, String form2, String form3);
    private class ImageUi
    {
        private static Coroutine coroutineImg;
        private static Dictionary<String, String> Images;
        private static List<String> KeyImages;
        public static void DownloadImages();
        private static IEnumerator AddImage();
        public static String GetImage(String ImgKey);
        public static void Initialize();
        public static void Unload();
    }

    private static StringBuilder sb;
    public String GetLang(String LangKey, String userID, Object[] args);
    private new void LoadDefaultMessages();
    private static Coroutine coroutineMigration;
    private IEnumerator MigrationTimedPermissions();
    private class PlayerInformation
    {
        [JsonProperty("Id")]
        public string Id { get; set; }
        [JsonProperty("Name")]
        public string Name { get; set; }
        [JsonProperty("Permissions")]
        public List<ExpiringAccessValue> _permissions;
        [JsonProperty("Groups")]
        public List<ExpiringAccessValue> _groups;
    }

    private class ExpiringAccessValue
    {
        [JsonProperty]
        public string Value { get; set; }
        [JsonProperty]
        public DateTime ExpireDate { get; set; }
        [JsonIgnore]
        public bool IsExpired { get; set; }
    }

    private static Int64 ToEpochTime(DateTime dateTime);
    private IEnumerator MigrationGrant();
    public class GrantData
    {
        public Dictionary<String, Dictionary<String, Int64>> Group;
        public Dictionary<String, Dictionary<String, Int64>> Permission;
    }

    private IEnumerator MigrationTimePrivilage();
     class StructureTimePrivilage
    {
        public Dictionary<String, String> groups;
        public Dictionary<String, String> permissions;
    }

    public Dictionary<String, DateTime> GetPermissions(UInt64 userID);
    public Dictionary<String, DateTime> GetGroups(UInt64 userID);
    public void SetPermission(UInt64 userID, String Permission, DateTime DataExpired);
    public void SetPermission(UInt64 userID, String Permission, String DataExpired);
    public void SetGroup(UInt64 userID, String Group, DateTime DataExpired);
    public void SetGroup(UInt64 userID, String Group, String DataExpired);
    public void RevokePermission(UInt64 userID, String Permission, DateTime DataExpired);
    public void RevokePermission(UInt64 userID, String Permission, String DataExpired);
    public void RevokeGroup(UInt64 userID, String Group, DateTime DataExpired);
    public void RevokeGroup(UInt64 userID, String Group, String DataExpired);
}

private class Configuration
{
    [JsonProperty(LanguageEn ? "Basic Settings" : "Основные настройки")]
    public GeneralSetting GeneralSettings;
    [JsonProperty(LanguageEn ? "Configuring the interface" : "Настройка интерфейса")]
    public InterfaceConfiguration InterfaceSetting;
    [JsonProperty(LanguageEn ? "Setting up Notifications" : "Настройка уведомлений")]
    public AlertConfiguration AlertConfigurations;
    [JsonProperty(LanguageEn ? "PERMISSIONS ON THE SERVER : [Permission] = Customization" : "ПРАВА НА СЕРВЕРЕ : [Permission] = Настройка")]
    public Dictionary<String, StructureLanguage> PermissionList;
    [JsonProperty(LanguageEn ? "GROUPS WITH RIGHTS ON THE SERVER : [Group] = Customization" : "ГРУППЫ С ПРАВАМИ НА СЕРВЕРЕ : [Group] = Настройка")]
    public Dictionary<String, StructureLanguage> GroupList;
    internal class GeneralSetting
    {
        [JsonProperty(LanguageEn ? "Webhook from the Discord channel for logging" : "Webhook от канала Discord для логирования")]
        public String WebHooksDiscord;
        [JsonProperty(LanguageEn
					? "Connecting data storage on the MySQL side"
					: "Подключение хранения данных на стороне MySQL")]
        public MySQLInitialize MySQLSetting;
        [JsonProperty(LanguageEn ? "Use the removal of temporary groups from the player when unloading the plugin (when loading, they will be returned)" : "Использовать удаление временных групп у игрока при выгрузке плагина (при загрузке - они будут возвращены)")]
        public Boolean UseRemoveGroupsWhenUnload;
        [JsonProperty(LanguageEn ? "Use the removal of temporary permission from the player when unloading the plugin (when loading, they will be returned)" : "Использовать удаление временных прав у игрока при выгрузке плагина (при загрузке - они будут возвращены)")]
        public Boolean UseRemovePermissionsХWhenUnload;
        [JsonProperty(LanguageEn ? "Settings for automatically granting privileges to server novices (They will be considered novices - while the player is not in the database or datafile)" : "Настройки автоматической выдачи привилегии новичкам сервера (Они будут считаться новичками - пока игрока нет в базе или датафайле)")]
        public AutoGive AutoGiveSettings;
        internal class AutoGive
        {
            [JsonProperty(LanguageEn ? "Use automatic granting of permissions to a new player" : "Использовать автоматическую выдачу прав новому игроку")]
            public Boolean UseAutGivePermissions;
            [JsonProperty(LanguageEn ? "Use automatic granting of groups to a new player" : "Использовать автоматическую выдачу групп новому игроку")]
            public Boolean UseAutGiveGroups;
            [JsonProperty(LanguageEn ? "List of permissions to issue [group] = time in the format (1d/h/m/s)" : "Список прав для выдачи [группа] = время в формате (1d/h/m/s)")]
            public Dictionary<String, String> PermissionList;
            [JsonProperty(LanguageEn ? "List of groups to issue [group] = time in the format (1d/h/m/s)" : "Список групп для выдачи [группа] = время в формате (1d/h/m/s)")]
            public Dictionary<String, String> GroupList;
        }

        internal class MySQLInitialize
        {
            [JsonProperty(LanguageEn ? "Setting up a MySQL connection" : "Настройка MySQL соединения")]
            public MySQLConnect ConnectSetting;
            [JsonProperty(LanguageEn ? "The desired table name in MySQL" : "Желаемое название таблицы в MySQL")]
            public String TableName;
            [JsonProperty(LanguageEn
						? "Settings privilege migration between servers connected to the same MySQL (Example: Server #1 and #2 are connected to the same MySQL - a player with privileges on server #1 - going to server #2 - will get it there)"
						: "Настройка миграцию привилегий между среверами подключенными к одной MySQL (Пример : Сервер #1 и #2 подключены к одной MySQL - игрок с привилегий на сервере #1 - зайдя на сервер #2 - получит ее и там)")]
            public MigrationPrivilage MigrationSettings;
            internal class MigrationPrivilage
            {
                [JsonProperty(LanguageEn ? "Use the list of permissions available for migration (otherwise they will be all that the player has in MySQL)" : "Использовать список прав доступных для миграции (иначе будут все, что есть у игрока в MySQL)")]
                public Boolean UseMigrationPermissionsList;
                [JsonProperty(LanguageEn ? "List of permissions in available for synchronization on another server in case of player migration" : "Список прав в доступных для синхронизации на другом сервере случае миграции игрока")]
                public List<String> MigrationPermissionList;
                [JsonProperty(LanguageEn ? "Use the list of groups available for migration (otherwise they will be all that the player has in MySQL)" : "Использовать список групп доступных для миграции (иначе будут все, что есть у игрока в MySQL)")]
                public Boolean UseMigrationGroupList;
                [JsonProperty(LanguageEn ? "List of groups in available for synchronization on another server in case of player migration" : "Список групп в доступных для синхронизации на другом сервере случае миграции игрока")]
                public List<String> MigrationGroupList;
            }

            internal class MySQLConnect
            {
                public String Host;
                public String Port;
                public String DatabaseName;
                public String UserName;
                public String Passowrd;
            }

        }

    }

    internal class AlertConfiguration
    {
        [JsonProperty(LanguageEn ? "Use the plugin's UI notification for notifications (otherwise it will be displayed in the chat)" : "Использовать для уведомлений UI-уведомление плагина (иначе будет отображаться в чате)")]
        public Boolean UsePoopUp;
        [JsonProperty(LanguageEn
					? "Remind players that they are running out of group?"
					: "Напоминать игрокам о том, что у них заканчивается группа?")]
        public Boolean UseAlertedGroup;
        [JsonProperty(LanguageEn
					? "The list of groups for which the reminder will work"
					: "Список групп на которые сработает напоминание")]
        public List<String> GrpupListAlerted;
        [JsonProperty(LanguageEn
					? "Remind players that they are running out of permission?"
					: "Напоминать игрокам о том, что у них заканчивается права?")]
        public Boolean UseAlertedPermission;
        [JsonProperty(LanguageEn
					? "The list of permissions for which the reminder will work"
					: "Список прав на которые сработает напоминание")]
        public List<String> PermissionsListAlerted;
        [JsonProperty(LanguageEn ? "How many days before the end of the privilege to remind the player about it" : "За сколько дней до окончания привилегии напоминать игроку об этом")]
        public Int32 DayAlerted;
        [JsonProperty(LanguageEn ? "How many seconds to show notifications to the player" : "Сколько секунд показывать уведомления игроку")]
        public Single TimeAlert;
        [JsonProperty(LanguageEn ? "Setting up IQChat (If installed)" : "Настройка IQChat (Если установлен)")]
        public IQChatSetting SettingIQChat;
        internal class IQChatSetting
        {
            [JsonProperty(LanguageEn ? "IQChat : Custom prefix in chat" : "IQChat : Кастомный префикс в чате")]
            public String CustomPrefix;
            [JsonProperty(LanguageEn
						? "IQChat : Custom avatar in the chat (If required)"
						: "IQChat : Кастомный аватар в чате(Если требуется)")]
            public String CustomAvatar;
            [JsonProperty(LanguageEn
						? "IQChat : Use UI notifications"
						: "IQChat : Использовать UI уведомления")]
            public Boolean UIAlertUse;
        }

    }

    internal class InterfaceConfiguration
    {
        [JsonProperty(LanguageEn ? "PNG : Link to the background of the notification" : "PNG : Ссылка на задний фон уведомления")]
        public String PNGBackground;
        [JsonProperty(LanguageEn ? "PNG : Link to the image when receiving the privilege" : "PNG : Ссылка на картинку при получении привилегии")]
        public String PNGPrivilageAdd;
        [JsonProperty(LanguageEn ? "PNG : Link to the picture at the end of the privilege" : "PNG : Ссылка на картинку при окончании привилегии")]
        public String PNGPrivilageExpired;
        [JsonProperty(LanguageEn ? "PNG : Link to the picture at the notification of the end of the privilege" : "PNG : Ссылка на картинку при уведомлении об окончании привилегии")]
        public String PNGPrivilageAlert;
        [JsonProperty(LanguageEn ? "RGBA : Header Color" : "RGBA : Цвет заголовка")]
        public String RGBAColorTitle;
        [JsonProperty(LanguageEn ? "RGBA : Additional text color" : "RGBA : Цвет дополнительного текста")]
        public String RGBAColorDescription;
    }

    internal class StructureLanguage
    {
        [JsonProperty(LanguageEn ? "Name in Russian (Not obligatory. Only for Russian players)" : "Название на русском")]
        public String RussianName;
        [JsonProperty(LanguageEn ? "Name in English" : "Название на английском")]
        public String EnglishName;
        public StructureLanguage(String russianName, String englishName);
    }

    public static Configuration GetNewConfiguration();
}

internal class GeneralSetting
{
    [JsonProperty(LanguageEn ? "Webhook from the Discord channel for logging" : "Webhook от канала Discord для логирования")]
    public String WebHooksDiscord;
    [JsonProperty(LanguageEn
					? "Connecting data storage on the MySQL side"
					: "Подключение хранения данных на стороне MySQL")]
    public MySQLInitialize MySQLSetting;
    [JsonProperty(LanguageEn ? "Use the removal of temporary groups from the player when unloading the plugin (when loading, they will be returned)" : "Использовать удаление временных групп у игрока при выгрузке плагина (при загрузке - они будут возвращены)")]
    public Boolean UseRemoveGroupsWhenUnload;
    [JsonProperty(LanguageEn ? "Use the removal of temporary permission from the player when unloading the plugin (when loading, they will be returned)" : "Использовать удаление временных прав у игрока при выгрузке плагина (при загрузке - они будут возвращены)")]
    public Boolean UseRemovePermissionsХWhenUnload;
    [JsonProperty(LanguageEn ? "Settings for automatically granting privileges to server novices (They will be considered novices - while the player is not in the database or datafile)" : "Настройки автоматической выдачи привилегии новичкам сервера (Они будут считаться новичками - пока игрока нет в базе или датафайле)")]
    public AutoGive AutoGiveSettings;
    internal class AutoGive
    {
        [JsonProperty(LanguageEn ? "Use automatic granting of permissions to a new player" : "Использовать автоматическую выдачу прав новому игроку")]
        public Boolean UseAutGivePermissions;
        [JsonProperty(LanguageEn ? "Use automatic granting of groups to a new player" : "Использовать автоматическую выдачу групп новому игроку")]
        public Boolean UseAutGiveGroups;
        [JsonProperty(LanguageEn ? "List of permissions to issue [group] = time in the format (1d/h/m/s)" : "Список прав для выдачи [группа] = время в формате (1d/h/m/s)")]
        public Dictionary<String, String> PermissionList;
        [JsonProperty(LanguageEn ? "List of groups to issue [group] = time in the format (1d/h/m/s)" : "Список групп для выдачи [группа] = время в формате (1d/h/m/s)")]
        public Dictionary<String, String> GroupList;
    }

    internal class MySQLInitialize
    {
        [JsonProperty(LanguageEn ? "Setting up a MySQL connection" : "Настройка MySQL соединения")]
        public MySQLConnect ConnectSetting;
        [JsonProperty(LanguageEn ? "The desired table name in MySQL" : "Желаемое название таблицы в MySQL")]
        public String TableName;
        [JsonProperty(LanguageEn
						? "Settings privilege migration between servers connected to the same MySQL (Example: Server #1 and #2 are connected to the same MySQL - a player with privileges on server #1 - going to server #2 - will get it there)"
						: "Настройка миграцию привилегий между среверами подключенными к одной MySQL (Пример : Сервер #1 и #2 подключены к одной MySQL - игрок с привилегий на сервере #1 - зайдя на сервер #2 - получит ее и там)")]
        public MigrationPrivilage MigrationSettings;
        internal class MigrationPrivilage
        {
            [JsonProperty(LanguageEn ? "Use the list of permissions available for migration (otherwise they will be all that the player has in MySQL)" : "Использовать список прав доступных для миграции (иначе будут все, что есть у игрока в MySQL)")]
            public Boolean UseMigrationPermissionsList;
            [JsonProperty(LanguageEn ? "List of permissions in available for synchronization on another server in case of player migration" : "Список прав в доступных для синхронизации на другом сервере случае миграции игрока")]
            public List<String> MigrationPermissionList;
            [JsonProperty(LanguageEn ? "Use the list of groups available for migration (otherwise they will be all that the player has in MySQL)" : "Использовать список групп доступных для миграции (иначе будут все, что есть у игрока в MySQL)")]
            public Boolean UseMigrationGroupList;
            [JsonProperty(LanguageEn ? "List of groups in available for synchronization on another server in case of player migration" : "Список групп в доступных для синхронизации на другом сервере случае миграции игрока")]
            public List<String> MigrationGroupList;
        }

        internal class MySQLConnect
        {
            public String Host;
            public String Port;
            public String DatabaseName;
            public String UserName;
            public String Passowrd;
        }

    }

}

internal class AutoGive
{
    [JsonProperty(LanguageEn ? "Use automatic granting of permissions to a new player" : "Использовать автоматическую выдачу прав новому игроку")]
    public Boolean UseAutGivePermissions;
    [JsonProperty(LanguageEn ? "Use automatic granting of groups to a new player" : "Использовать автоматическую выдачу групп новому игроку")]
    public Boolean UseAutGiveGroups;
    [JsonProperty(LanguageEn ? "List of permissions to issue [group] = time in the format (1d/h/m/s)" : "Список прав для выдачи [группа] = время в формате (1d/h/m/s)")]
    public Dictionary<String, String> PermissionList;
    [JsonProperty(LanguageEn ? "List of groups to issue [group] = time in the format (1d/h/m/s)" : "Список групп для выдачи [группа] = время в формате (1d/h/m/s)")]
    public Dictionary<String, String> GroupList;
}

internal class MySQLInitialize
{
    [JsonProperty(LanguageEn ? "Setting up a MySQL connection" : "Настройка MySQL соединения")]
    public MySQLConnect ConnectSetting;
    [JsonProperty(LanguageEn ? "The desired table name in MySQL" : "Желаемое название таблицы в MySQL")]
    public String TableName;
    [JsonProperty(LanguageEn
						? "Settings privilege migration between servers connected to the same MySQL (Example: Server #1 and #2 are connected to the same MySQL - a player with privileges on server #1 - going to server #2 - will get it there)"
						: "Настройка миграцию привилегий между среверами подключенными к одной MySQL (Пример : Сервер #1 и #2 подключены к одной MySQL - игрок с привилегий на сервере #1 - зайдя на сервер #2 - получит ее и там)")]
    public MigrationPrivilage MigrationSettings;
    internal class MigrationPrivilage
    {
        [JsonProperty(LanguageEn ? "Use the list of permissions available for migration (otherwise they will be all that the player has in MySQL)" : "Использовать список прав доступных для миграции (иначе будут все, что есть у игрока в MySQL)")]
        public Boolean UseMigrationPermissionsList;
        [JsonProperty(LanguageEn ? "List of permissions in available for synchronization on another server in case of player migration" : "Список прав в доступных для синхронизации на другом сервере случае миграции игрока")]
        public List<String> MigrationPermissionList;
        [JsonProperty(LanguageEn ? "Use the list of groups available for migration (otherwise they will be all that the player has in MySQL)" : "Использовать список групп доступных для миграции (иначе будут все, что есть у игрока в MySQL)")]
        public Boolean UseMigrationGroupList;
        [JsonProperty(LanguageEn ? "List of groups in available for synchronization on another server in case of player migration" : "Список групп в доступных для синхронизации на другом сервере случае миграции игрока")]
        public List<String> MigrationGroupList;
    }

    internal class MySQLConnect
    {
        public String Host;
        public String Port;
        public String DatabaseName;
        public String UserName;
        public String Passowrd;
    }

}

internal class MigrationPrivilage
{
    [JsonProperty(LanguageEn ? "Use the list of permissions available for migration (otherwise they will be all that the player has in MySQL)" : "Использовать список прав доступных для миграции (иначе будут все, что есть у игрока в MySQL)")]
    public Boolean UseMigrationPermissionsList;
    [JsonProperty(LanguageEn ? "List of permissions in available for synchronization on another server in case of player migration" : "Список прав в доступных для синхронизации на другом сервере случае миграции игрока")]
    public List<String> MigrationPermissionList;
    [JsonProperty(LanguageEn ? "Use the list of groups available for migration (otherwise they will be all that the player has in MySQL)" : "Использовать список групп доступных для миграции (иначе будут все, что есть у игрока в MySQL)")]
    public Boolean UseMigrationGroupList;
    [JsonProperty(LanguageEn ? "List of groups in available for synchronization on another server in case of player migration" : "Список групп в доступных для синхронизации на другом сервере случае миграции игрока")]
    public List<String> MigrationGroupList;
}

internal class MySQLConnect
{
    public String Host;
    public String Port;
    public String DatabaseName;
    public String UserName;
    public String Passowrd;
}

internal class AlertConfiguration
{
    [JsonProperty(LanguageEn ? "Use the plugin's UI notification for notifications (otherwise it will be displayed in the chat)" : "Использовать для уведомлений UI-уведомление плагина (иначе будет отображаться в чате)")]
    public Boolean UsePoopUp;
    [JsonProperty(LanguageEn
					? "Remind players that they are running out of group?"
					: "Напоминать игрокам о том, что у них заканчивается группа?")]
    public Boolean UseAlertedGroup;
    [JsonProperty(LanguageEn
					? "The list of groups for which the reminder will work"
					: "Список групп на которые сработает напоминание")]
    public List<String> GrpupListAlerted;
    [JsonProperty(LanguageEn
					? "Remind players that they are running out of permission?"
					: "Напоминать игрокам о том, что у них заканчивается права?")]
    public Boolean UseAlertedPermission;
    [JsonProperty(LanguageEn
					? "The list of permissions for which the reminder will work"
					: "Список прав на которые сработает напоминание")]
    public List<String> PermissionsListAlerted;
    [JsonProperty(LanguageEn ? "How many days before the end of the privilege to remind the player about it" : "За сколько дней до окончания привилегии напоминать игроку об этом")]
    public Int32 DayAlerted;
    [JsonProperty(LanguageEn ? "How many seconds to show notifications to the player" : "Сколько секунд показывать уведомления игроку")]
    public Single TimeAlert;
    [JsonProperty(LanguageEn ? "Setting up IQChat (If installed)" : "Настройка IQChat (Если установлен)")]
    public IQChatSetting SettingIQChat;
    internal class IQChatSetting
    {
        [JsonProperty(LanguageEn ? "IQChat : Custom prefix in chat" : "IQChat : Кастомный префикс в чате")]
        public String CustomPrefix;
        [JsonProperty(LanguageEn
						? "IQChat : Custom avatar in the chat (If required)"
						: "IQChat : Кастомный аватар в чате(Если требуется)")]
        public String CustomAvatar;
        [JsonProperty(LanguageEn
						? "IQChat : Use UI notifications"
						: "IQChat : Использовать UI уведомления")]
        public Boolean UIAlertUse;
    }

}

internal class IQChatSetting
{
    [JsonProperty(LanguageEn ? "IQChat : Custom prefix in chat" : "IQChat : Кастомный префикс в чате")]
    public String CustomPrefix;
    [JsonProperty(LanguageEn
						? "IQChat : Custom avatar in the chat (If required)"
						: "IQChat : Кастомный аватар в чате(Если требуется)")]
    public String CustomAvatar;
    [JsonProperty(LanguageEn
						? "IQChat : Use UI notifications"
						: "IQChat : Использовать UI уведомления")]
    public Boolean UIAlertUse;
}

internal class InterfaceConfiguration
{
    [JsonProperty(LanguageEn ? "PNG : Link to the background of the notification" : "PNG : Ссылка на задний фон уведомления")]
    public String PNGBackground;
    [JsonProperty(LanguageEn ? "PNG : Link to the image when receiving the privilege" : "PNG : Ссылка на картинку при получении привилегии")]
    public String PNGPrivilageAdd;
    [JsonProperty(LanguageEn ? "PNG : Link to the picture at the end of the privilege" : "PNG : Ссылка на картинку при окончании привилегии")]
    public String PNGPrivilageExpired;
    [JsonProperty(LanguageEn ? "PNG : Link to the picture at the notification of the end of the privilege" : "PNG : Ссылка на картинку при уведомлении об окончании привилегии")]
    public String PNGPrivilageAlert;
    [JsonProperty(LanguageEn ? "RGBA : Header Color" : "RGBA : Цвет заголовка")]
    public String RGBAColorTitle;
    [JsonProperty(LanguageEn ? "RGBA : Additional text color" : "RGBA : Цвет дополнительного текста")]
    public String RGBAColorDescription;
}

internal class StructureLanguage
{
    [JsonProperty(LanguageEn ? "Name in Russian (Not obligatory. Only for Russian players)" : "Название на русском")]
    public String RussianName;
    [JsonProperty(LanguageEn ? "Name in English" : "Название на английском")]
    public String EnglishName;
    public StructureLanguage(String russianName, String englishName);
}

internal class StructureData
{
    [JsonProperty(LanguageEn ? "Information about player permissions" : "Информация о привилегиях игрока")]
    public Dictionary<String, Dictionary<UInt64, DateTime>> UserPermissions;
    [JsonProperty(LanguageEn ? "Information about player groups" : "Информация о группах игрока")]
    public Dictionary<String, Dictionary<UInt64, DateTime>> UserGrpups;
    public Dictionary<String, DateTime> GetParametresUser(UInt64 userID, TypeData typeDictonary);
    public Dictionary<UInt64, DateTime> GePlayerList(String Key, TypeData typeDictonary);
    public DateTime GetExpiredPermissionData(String Key, UInt64 userID, TypeData typeDictonary);
    public void SetParametres(String Key, UInt64 userID, DateTime dateExpired, TypeData typeDictonary, Boolean IsMigration);
    public void RemoveParametres(String Key, UInt64 userID, TypeData typeDictonary, DateTime timeRevoed);
    public void SetParametresUnload();
    public void RemoveParametresUnload();
    private Boolean IsExpiredPermission(String Key, UInt64 userID, TypeData typeDictonary);
    public void TrackerExpireds();
}

private class InterfaceBuilder
{
    public static InterfaceBuilder Instance;
    public const String UI_POOPUP_PANEL;
    public Dictionary<String, String> Interfaces;
    public InterfaceBuilder();
    public static void AddInterface(String name, String json);
    public static String GetInterface(String name);
    public static void DestroyAll();
    private void BuildingPlayer_PopUp();
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
        public Footer footer { get; set; }
        public Authors author { get; set; }
        public Embeds(String title, Int32 color, List<Fields> fields, Authors author, Footer footer);
    }

    public FancyMessage(String content, bool tts, Embeds[] embeds);
    public String toJSON();
}

public class Embeds
{
    public String title { get; set; }
    public Int32 color { get; set; }
    public List<Fields> fields { get; set; }
    public Footer footer { get; set; }
    public Authors author { get; set; }
    public Embeds(String title, Int32 color, List<Fields> fields, Authors author, Footer footer);
}

public class Footer
{
    public String text { get; set; }
    public String icon_url { get; set; }
    public String proxy_icon_url { get; set; }
    public Footer(String text, String icon_url, String proxy_icon_url);
}

public class Authors
{
    public String name { get; set; }
    public String url { get; set; }
    public String icon_url { get; set; }
    public String proxy_icon_url { get; set; }
    public Authors(String name, String url, String icon_url, String proxy_icon_url);
}

public class Fields
{
    public String name { get; set; }
    public String value { get; set; }
    public bool inline { get; set; }
    public Fields(String name, String value, bool inline);
}

private class ImageUi
{
    private static Coroutine coroutineImg;
    private static Dictionary<String, String> Images;
    private static List<String> KeyImages;
    public static void DownloadImages();
    private static IEnumerator AddImage();
    public static String GetImage(String ImgKey);
    public static void Initialize();
    public static void Unload();
}

private class PlayerInformation
{
    [JsonProperty("Id")]
    public string Id { get; set; }
    [JsonProperty("Name")]
    public string Name { get; set; }
    [JsonProperty("Permissions")]
    public List<ExpiringAccessValue> _permissions;
    [JsonProperty("Groups")]
    public List<ExpiringAccessValue> _groups;
}

private class ExpiringAccessValue
{
    [JsonProperty]
    public string Value { get; set; }
    [JsonProperty]
    public DateTime ExpireDate { get; set; }
    [JsonIgnore]
    public bool IsExpired { get; set; }
}

public class GrantData
{
    public Dictionary<String, Dictionary<String, Int64>> Group;
    public Dictionary<String, Dictionary<String, Int64>> Permission;
}

 class StructureTimePrivilage
{
    public Dictionary<String, String> groups;
    public Dictionary<String, String> permissions;
}


```

---

## IQPersonal

```csharp
using System;
using System.Collections.Generic;
using ConVar;
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;
using Oxide.Core.Libraries;
using Oxide.Core.Plugins;

Oxide.Plugins
[Info("IQPersonal", "Mercury", "1.0.0")]
[Description("Властвуй по пионерски. Система контроля персонала")]
 class IQPersonal : RustPlugin
{
    [PluginReference]
     Plugin IQChat;
    public void SendChat(BasePlayer player, string Message, Chat.ChatChannel channel);
    public static string PermissionUse;
    private static Configuration config;
    private class Configuration
    {
        [JsonProperty("Основная настройка плагина")]
        public ControllerSettings ControllerSetting;
        [JsonProperty("Настройка дневной нормы для персонала")]
        public Dictionary<string, ControllerDay> Settings;
        [JsonProperty("Использовать логирование в беседу ВК")]
        public bool UseLogged;
        [JsonProperty("Использовать логирование в Discord канал")]
        public bool UseDiscordLogged;
        [JsonProperty("Настройка ВК для логирования в беседу(Если включено)")]
        public VKInformation VKSetting;
        [JsonProperty("Настройка Discord для логирования в канал(Если включено)")]
        public DiscordInformation DiscordSetting;
        [JsonProperty("Настройка магазинов")]
        public StoreSettings StoreSetting;
        [JsonProperty("Дополнительные настройки")]
        public MoreSettings MoreSetting;
        internal class ControllerDay
        {
            [JsonProperty("Учитывать время в суточной норме")]
            public bool DayTimeUse;
            [JsonProperty("Суточная норма отыгранного времени на сервере[секунды]")]
            public int DayTime;
            [JsonProperty("Учитывать муты в суточной норме(IQChat)")]
            public bool DayMuteUse;
            [JsonProperty("Суточная норма выданных мутов(IQChat)")]
            public int DayMute;
            [JsonProperty("Учитывать проверки в суточной норме(IQReportSystem)")]
            public bool DayCheckUse;
            [JsonProperty("Суточная норма проверок(IQReportSystem)")]
            public int DayCheck;
        }

        internal class ControllerSettings
        {
            [JsonProperty("Учитывать время")]
            public bool ControllerTime;
            [JsonProperty("Учитывать муты(IQChat)")]
            public bool ControllerMute;
            [JsonProperty("Учитывать проверки(IQReportSystem)")]
            public bool ControllerCheck;
            [JsonProperty("Учитывать блокировки(IQReportSystem)")]
            public bool ControllerBans;
            [JsonProperty("Учитывать ругань персонала(IQChat)")]
            public bool ControllerBadWords;
            [JsonProperty("Сколько очков репутации снимать за 1 запрещенное слово(IQChat)")]
            public int ScoreRemoveBadWords;
            [JsonProperty("Сколько очков репутации нужно для вывода 1 рубля")]
            public int ScoreChanged;
            [JsonProperty("Включить суточную норму")]
            public bool ControllerDays;
            [JsonProperty("Снимать репутацию в случае не выполнения суточной нормы")]
            public bool UseControllerDaysFailed;
            [JsonProperty("Включить автоматическое снятие,при низкой репутации")]
            public bool ControllerAutoKick;
            [JsonProperty("При каком отрицательном показателе репутации снимать персонал")]
            public int AutoKickMinimalReputation;
            [JsonProperty("Сколько очков репутации начислять за выполнение дневной нормы")]
            public int ScoreAddDayNormal;
            [JsonProperty("Сколько репутации начислять за мут")]
            public int ScoreMute;
            [JsonProperty("Сколько репутации начислять за проверку игрока")]
            public int ScoreCheck;
            [JsonProperty("Сколько репутации начислять за блокировку игрока")]
            public int ScoreBans;
            [JsonProperty("Сколько репутации снимать за не выполнение суточной нормы")]
            public int ScoreFailedControllerDays;
        }

        internal class VKInformation
        {
            [JsonProperty("Токен от группы ВК(От группы будут идти сообщения в беседу.Вам нужно добавить свою группу в беседу!)")]
            public string VKToken;
            [JsonProperty("ID беседы для отправки логов(Отчет о времени,статистика)")]
            public string VKChatID;
            [JsonProperty("ID беседы с персоналом(если имеется. Туда будет приходить информация о добавлении персонала и удалять тех,кого кикнули(автоматически))")]
            public string VKChatIDPersonal;
        }

        internal class DiscordInformation
        {
            [JsonProperty("Webhooks для дискорда")]
            public string WebHook;
        }

        internal class MoreSettings
        {
            [JsonProperty("Префикс для чата(IQChat)")]
            public string CustomPrefix;
            [JsonProperty("Аватарка для чата(IQChat)")]
            public string CustomAvatar;
            [JsonProperty("Цвет для сообщения в чате(IQChat)")]
            public string CustomHex;
            [JsonProperty("Выдавать дополнительные права,при добавлении в персонал(при удалении будут сниматься)")]
            public bool GiveMorePermission;
            [JsonProperty("Лист дополительных привилегий(Если включено)(Выдадутся все сразу,при добавлении в персонал.При удалении снимутся)(Пример : iqchat.vip)")]
            public List<string> Permissions;
        }

        internal class StoreSettings
        {
            [JsonProperty("Использовать магазин MoscowOVH")]
            public bool MoscowOVH;
            [JsonProperty("Использовать магазин GameStores")]
            public bool GameStores;
            [JsonProperty("Настройки для магазина GameStores")]
            public GameStoresInformation InfoGameStores;
            internal class GameStoresInformation
            {
                [JsonProperty("API Магазина(GameStores)")]
                public string GameStoresAPIStore;
                [JsonProperty("ID Магазина(GameStores)")]
                public string GameStoresIDStore;
                [JsonProperty("Сообщение в магазин при выдаче баланса(GameStores)")]
                public string GameStoresMessage;
            }

        }

        public static Configuration GetNewConfiguration();
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    [JsonProperty("Информация о персонале")]
    public Dictionary<ulong, InformationUser> DataInformation;
    [JsonProperty("Хранение времени")]
    public int TimeDay;
    public class InformationUser
    {
        [JsonProperty("Ник")]
        public string DisplayName;
        [JsonProperty("Ссылка на ВК")]
        public string VKLink;
        [JsonProperty("Дневное отыгранное время на сервере")]
        public int DayTime;
        [JsonProperty("Дневное количество выданных мутов")]
        public int DayMute;
        [JsonProperty("Дневное количество проверок")]
        public int DayCheck;
        [JsonProperty("Дневное количество выданных блокировок")]
        public int DayBans;
        [JsonProperty("Очки репутации игрока")]
        public int ScorePlayer;
        [JsonProperty("Статус дневной нормы(true - выполнил/false - нет)")]
        public bool StatusDayNormal;
    }

     void ReadData();
     void WriteData();
     void RegisteredDataUser(ulong player, string VKLink);
     void NormalDaySet();
     void DayNormalCheck();
     void DayNormalFinish(ulong UserID);
     void TrackerTime(BasePlayer player);
     void BalanceSet(ulong UserID);
     void AddPersonal(ulong userID, string VKLink);
     void RemovePersonal(ulong userID);
     void ChangePersonalVK(ulong userID, string VKLink);
    private void OnServerInitialized();
     void OnServerSave();
     void Unload();
     void OnPlayerConnected(BasePlayer player);
     void OnPlayerDisconnected(BasePlayer player, string reason);
     void OnNewSave(string filename);
    [ChatCommand("rep")]
     void ChatCommandRep(BasePlayer player);
    [ConsoleCommand("iqp")]
     void ConsoleCommandIQP(ConsoleSystem.Arg arg);
    private new void LoadDefaultMessages();
     void RegisteredPermission();
     void VKSendMessage(string Message, string CustomChatID);
     void DiscordSendMessage(string key, ulong userID, object[] args);
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
     int ConvertReputation(ulong UserID);
     bool IsDayNormal();
     bool IsPersonalNormalCompleted(ulong UserID);
     bool IsPersonal(ulong UserID);
     bool HasPermission(ulong UserID, string Permissions);
    static DateTime epoch;
    static double CurrentTime();
     void API_SET_BANS(ulong UserID);
     void API_SET_CHECK(ulong UserID);
     void API_SET_MUTE(ulong UserID);
     void API_DETECTED_BAD_WORDS(ulong UserID);
     void API_SET_SCORE(ulong UserID, int Amount);
     void API_REMOVE_SCORE(ulong UserID, int Amount);
     int API_GET_BANS(ulong UserID);
     int API_GET_CHECK(ulong UserID);
     int API_GET_MUTE(ulong UserID);
     int API_GET_SCORE(ulong UserID);
}

private class Configuration
{
    [JsonProperty("Основная настройка плагина")]
    public ControllerSettings ControllerSetting;
    [JsonProperty("Настройка дневной нормы для персонала")]
    public Dictionary<string, ControllerDay> Settings;
    [JsonProperty("Использовать логирование в беседу ВК")]
    public bool UseLogged;
    [JsonProperty("Использовать логирование в Discord канал")]
    public bool UseDiscordLogged;
    [JsonProperty("Настройка ВК для логирования в беседу(Если включено)")]
    public VKInformation VKSetting;
    [JsonProperty("Настройка Discord для логирования в канал(Если включено)")]
    public DiscordInformation DiscordSetting;
    [JsonProperty("Настройка магазинов")]
    public StoreSettings StoreSetting;
    [JsonProperty("Дополнительные настройки")]
    public MoreSettings MoreSetting;
    internal class ControllerDay
    {
        [JsonProperty("Учитывать время в суточной норме")]
        public bool DayTimeUse;
        [JsonProperty("Суточная норма отыгранного времени на сервере[секунды]")]
        public int DayTime;
        [JsonProperty("Учитывать муты в суточной норме(IQChat)")]
        public bool DayMuteUse;
        [JsonProperty("Суточная норма выданных мутов(IQChat)")]
        public int DayMute;
        [JsonProperty("Учитывать проверки в суточной норме(IQReportSystem)")]
        public bool DayCheckUse;
        [JsonProperty("Суточная норма проверок(IQReportSystem)")]
        public int DayCheck;
    }

    internal class ControllerSettings
    {
        [JsonProperty("Учитывать время")]
        public bool ControllerTime;
        [JsonProperty("Учитывать муты(IQChat)")]
        public bool ControllerMute;
        [JsonProperty("Учитывать проверки(IQReportSystem)")]
        public bool ControllerCheck;
        [JsonProperty("Учитывать блокировки(IQReportSystem)")]
        public bool ControllerBans;
        [JsonProperty("Учитывать ругань персонала(IQChat)")]
        public bool ControllerBadWords;
        [JsonProperty("Сколько очков репутации снимать за 1 запрещенное слово(IQChat)")]
        public int ScoreRemoveBadWords;
        [JsonProperty("Сколько очков репутации нужно для вывода 1 рубля")]
        public int ScoreChanged;
        [JsonProperty("Включить суточную норму")]
        public bool ControllerDays;
        [JsonProperty("Снимать репутацию в случае не выполнения суточной нормы")]
        public bool UseControllerDaysFailed;
        [JsonProperty("Включить автоматическое снятие,при низкой репутации")]
        public bool ControllerAutoKick;
        [JsonProperty("При каком отрицательном показателе репутации снимать персонал")]
        public int AutoKickMinimalReputation;
        [JsonProperty("Сколько очков репутации начислять за выполнение дневной нормы")]
        public int ScoreAddDayNormal;
        [JsonProperty("Сколько репутации начислять за мут")]
        public int ScoreMute;
        [JsonProperty("Сколько репутации начислять за проверку игрока")]
        public int ScoreCheck;
        [JsonProperty("Сколько репутации начислять за блокировку игрока")]
        public int ScoreBans;
        [JsonProperty("Сколько репутации снимать за не выполнение суточной нормы")]
        public int ScoreFailedControllerDays;
    }

    internal class VKInformation
    {
        [JsonProperty("Токен от группы ВК(От группы будут идти сообщения в беседу.Вам нужно добавить свою группу в беседу!)")]
        public string VKToken;
        [JsonProperty("ID беседы для отправки логов(Отчет о времени,статистика)")]
        public string VKChatID;
        [JsonProperty("ID беседы с персоналом(если имеется. Туда будет приходить информация о добавлении персонала и удалять тех,кого кикнули(автоматически))")]
        public string VKChatIDPersonal;
    }

    internal class DiscordInformation
    {
        [JsonProperty("Webhooks для дискорда")]
        public string WebHook;
    }

    internal class MoreSettings
    {
        [JsonProperty("Префикс для чата(IQChat)")]
        public string CustomPrefix;
        [JsonProperty("Аватарка для чата(IQChat)")]
        public string CustomAvatar;
        [JsonProperty("Цвет для сообщения в чате(IQChat)")]
        public string CustomHex;
        [JsonProperty("Выдавать дополнительные права,при добавлении в персонал(при удалении будут сниматься)")]
        public bool GiveMorePermission;
        [JsonProperty("Лист дополительных привилегий(Если включено)(Выдадутся все сразу,при добавлении в персонал.При удалении снимутся)(Пример : iqchat.vip)")]
        public List<string> Permissions;
    }

    internal class StoreSettings
    {
        [JsonProperty("Использовать магазин MoscowOVH")]
        public bool MoscowOVH;
        [JsonProperty("Использовать магазин GameStores")]
        public bool GameStores;
        [JsonProperty("Настройки для магазина GameStores")]
        public GameStoresInformation InfoGameStores;
        internal class GameStoresInformation
        {
            [JsonProperty("API Магазина(GameStores)")]
            public string GameStoresAPIStore;
            [JsonProperty("ID Магазина(GameStores)")]
            public string GameStoresIDStore;
            [JsonProperty("Сообщение в магазин при выдаче баланса(GameStores)")]
            public string GameStoresMessage;
        }

    }

    public static Configuration GetNewConfiguration();
}

internal class ControllerDay
{
    [JsonProperty("Учитывать время в суточной норме")]
    public bool DayTimeUse;
    [JsonProperty("Суточная норма отыгранного времени на сервере[секунды]")]
    public int DayTime;
    [JsonProperty("Учитывать муты в суточной норме(IQChat)")]
    public bool DayMuteUse;
    [JsonProperty("Суточная норма выданных мутов(IQChat)")]
    public int DayMute;
    [JsonProperty("Учитывать проверки в суточной норме(IQReportSystem)")]
    public bool DayCheckUse;
    [JsonProperty("Суточная норма проверок(IQReportSystem)")]
    public int DayCheck;
}

internal class ControllerSettings
{
    [JsonProperty("Учитывать время")]
    public bool ControllerTime;
    [JsonProperty("Учитывать муты(IQChat)")]
    public bool ControllerMute;
    [JsonProperty("Учитывать проверки(IQReportSystem)")]
    public bool ControllerCheck;
    [JsonProperty("Учитывать блокировки(IQReportSystem)")]
    public bool ControllerBans;
    [JsonProperty("Учитывать ругань персонала(IQChat)")]
    public bool ControllerBadWords;
    [JsonProperty("Сколько очков репутации снимать за 1 запрещенное слово(IQChat)")]
    public int ScoreRemoveBadWords;
    [JsonProperty("Сколько очков репутации нужно для вывода 1 рубля")]
    public int ScoreChanged;
    [JsonProperty("Включить суточную норму")]
    public bool ControllerDays;
    [JsonProperty("Снимать репутацию в случае не выполнения суточной нормы")]
    public bool UseControllerDaysFailed;
    [JsonProperty("Включить автоматическое снятие,при низкой репутации")]
    public bool ControllerAutoKick;
    [JsonProperty("При каком отрицательном показателе репутации снимать персонал")]
    public int AutoKickMinimalReputation;
    [JsonProperty("Сколько очков репутации начислять за выполнение дневной нормы")]
    public int ScoreAddDayNormal;
    [JsonProperty("Сколько репутации начислять за мут")]
    public int ScoreMute;
    [JsonProperty("Сколько репутации начислять за проверку игрока")]
    public int ScoreCheck;
    [JsonProperty("Сколько репутации начислять за блокировку игрока")]
    public int ScoreBans;
    [JsonProperty("Сколько репутации снимать за не выполнение суточной нормы")]
    public int ScoreFailedControllerDays;
}

internal class VKInformation
{
    [JsonProperty("Токен от группы ВК(От группы будут идти сообщения в беседу.Вам нужно добавить свою группу в беседу!)")]
    public string VKToken;
    [JsonProperty("ID беседы для отправки логов(Отчет о времени,статистика)")]
    public string VKChatID;
    [JsonProperty("ID беседы с персоналом(если имеется. Туда будет приходить информация о добавлении персонала и удалять тех,кого кикнули(автоматически))")]
    public string VKChatIDPersonal;
}

internal class DiscordInformation
{
    [JsonProperty("Webhooks для дискорда")]
    public string WebHook;
}

internal class MoreSettings
{
    [JsonProperty("Префикс для чата(IQChat)")]
    public string CustomPrefix;
    [JsonProperty("Аватарка для чата(IQChat)")]
    public string CustomAvatar;
    [JsonProperty("Цвет для сообщения в чате(IQChat)")]
    public string CustomHex;
    [JsonProperty("Выдавать дополнительные права,при добавлении в персонал(при удалении будут сниматься)")]
    public bool GiveMorePermission;
    [JsonProperty("Лист дополительных привилегий(Если включено)(Выдадутся все сразу,при добавлении в персонал.При удалении снимутся)(Пример : iqchat.vip)")]
    public List<string> Permissions;
}

internal class StoreSettings
{
    [JsonProperty("Использовать магазин MoscowOVH")]
    public bool MoscowOVH;
    [JsonProperty("Использовать магазин GameStores")]
    public bool GameStores;
    [JsonProperty("Настройки для магазина GameStores")]
    public GameStoresInformation InfoGameStores;
    internal class GameStoresInformation
    {
        [JsonProperty("API Магазина(GameStores)")]
        public string GameStoresAPIStore;
        [JsonProperty("ID Магазина(GameStores)")]
        public string GameStoresIDStore;
        [JsonProperty("Сообщение в магазин при выдаче баланса(GameStores)")]
        public string GameStoresMessage;
    }

}

internal class GameStoresInformation
{
    [JsonProperty("API Магазина(GameStores)")]
    public string GameStoresAPIStore;
    [JsonProperty("ID Магазина(GameStores)")]
    public string GameStoresIDStore;
    [JsonProperty("Сообщение в магазин при выдаче баланса(GameStores)")]
    public string GameStoresMessage;
}

public class InformationUser
{
    [JsonProperty("Ник")]
    public string DisplayName;
    [JsonProperty("Ссылка на ВК")]
    public string VKLink;
    [JsonProperty("Дневное отыгранное время на сервере")]
    public int DayTime;
    [JsonProperty("Дневное количество выданных мутов")]
    public int DayMute;
    [JsonProperty("Дневное количество проверок")]
    public int DayCheck;
    [JsonProperty("Дневное количество выданных блокировок")]
    public int DayBans;
    [JsonProperty("Очки репутации игрока")]
    public int ScorePlayer;
    [JsonProperty("Статус дневной нормы(true - выполнил/false - нет)")]
    public bool StatusDayNormal;
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

## IQPlagueSkill

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
[Info("IQPlagueSkill", "Mercury", "0.0.9")]
[Description("Скилл система новых веков")]
 class IQPlagueSkill : RustPlugin
{
    public static string PermissionsPatogenArmor;
    [PluginReference]
     Plugin ImageLibrary;
     Plugin IQChat;
     Plugin Friends;
     Plugin Clans;
     Plugin Battles;
     Plugin Duel;
     Plugin IQEconomic;
     Plugin IQHeadReward;
     Plugin XDNotifications;
     Plugin IQCraftSystem;
     Plugin IQKits;
    private void AddNotify(BasePlayer player, string title, string description, string command, string cmdyes, string cmdno);
    public string GetImage(string shortname, ulong skin);
    public bool AddImage(string url, string shortname, ulong skin);
    public void SendImage(BasePlayer player, string imageName, ulong imageId);
    public bool HasImage(string imageName);
     void LoadedImage();
     void CahedImages(BasePlayer player);
    public void SendChat(BasePlayer player, string Message, Chat.ChatChannel channel);
    public bool IsFriends(ulong userID, ulong targetID);
    public bool IsClans(ulong userID, ulong targetID);
    public bool IsDuel(ulong userID);
     int GetBalanceUser(ulong UserID);
     void RemoveBalanceUser(ulong UserID, int Balance);
    [JsonProperty("Информация пользователях и их ДНК")]
    public Dictionary<ulong, int> DataInformation;
    [JsonProperty("Информация о скиллах пользователей")]
    public Dictionary<ulong, InformationSkills> DataSkills;
    public class InformationSkills
    {
        [JsonProperty("Шахтер")]
        public bool Miner;
        [JsonProperty("Регенерация")]
        public bool Regeneration;
        [JsonProperty("Военный")]
        public bool Military;
        [JsonProperty("Кожа")]
        public bool ThickSkin;
        [JsonProperty("Дух")]
        public bool WoundedShake;
        [JsonProperty("Метаболизм")]
        public bool Metabolism;
        [JsonProperty("Защита от патогена")]
        public bool PatogenAmrory;
        [JsonProperty("Генезиз ген")]
        public bool GenesisGens;
        [JsonProperty("Единство с животными")]
        public bool AnimalFriends;
        [JsonProperty("Единство с природой")]
        public bool GatherFriends;
        [JsonProperty("Анабиотики")]
        public bool Anabiotics;
        [JsonProperty("Крафтер")]
        public bool Crafter;
        [JsonProperty("IQHeadReward")]
        public bool IQHeadReward;
        [JsonProperty("IQCraftSystem : Продвинутый крафтер")]
        public bool IQCraftSystemAdvanced;
        [JsonProperty("IQKits : Уменьшенная перезарядка")]
        public bool IQKitsCooldownPercent;
        [JsonProperty("IQKits : Увеличенный шанс выпадения")]
        public bool IQKitsRareup;
        [JsonProperty("Имеется ли патоген")]
        public bool PatogenAttack;
    }

     void ReadData();
     void WriteData();
     void RegisteredDataUser(BasePlayer player);
    private static Configuration config;
    private class Configuration
    {
        [JsonProperty("Настройка плагина")]
        public GeneralSetting GeneralSettings;
        [JsonProperty("Настройка всех скиллов")]
        public Skills SkillSettings;
        [JsonProperty("Настройка интерфейса")]
        public InterfaceSetting InterfaceSettings;
        [JsonProperty("Настройка совместимостей с другими плагинами")]
        public ReferenceSetting ReferenceSettings;
        [JsonProperty("Настройка получения ДНК")]
        public FarmingDNK FarmingDNKS;
        internal class Skills
        {
            [JsonProperty("Разрешить просмотр навыка без нужного кол-во ДНК")]
            public bool ShowSkillNotDNK;
            [JsonProperty("Настройка скилла шахтер(Увеличивает рейт добычи)")]
            public Miner MinerSettings;
            [JsonProperty("Настройка скилла регенерации(Регенирация ХП после боя)")]
            public Regeneration RegenerationSettings;
            [JsonProperty("Настройка скилла военный(Уменьшает изнашивания оружия)")]
            public Military MilitarySettings;
            [JsonProperty("Настройка скилла Твердая кожа(Защищает от холода в зимних биомах)")]
            public ThickSkin ThickSkinSettings;
            [JsonProperty("Настройка скилла Не поколебим(Дает шанс встать после падения)")]
            public WoundedShake WoundedShakeSettings;
            [JsonProperty("Настройка скилла Метаболизм(При возраждении дает N значения сытности,хп,жажды)")]
            public Metabolism MetabolismSettings;
            [JsonProperty("Настройка скилла Защита от патогена(Одноразовая защита от грибка Патоген(При заражаении патогеном , вирус - со временем разрушает скиллы игрока))")]
            public PatogenAmrory PatogenAmrorySettings;
            [JsonProperty("Настройка скилла Убийство патогена(Одноразовое убийство грибка,если игрок заразился Патогеном)")]
            public PatogenKill PatogenKillSettings;
            [JsonProperty("Настройка скилла Генезиз ген(Сохраняет % ДНК в конце вайпа)")]
            public GenesisGens GenesisGensSettings;
            [JsonProperty("Настройка скилла Единство с природой(Добавляет возможность получать больше ДНК за добычу животных)")]
            public AnimalFriends AnimalFriendsSettings;
            [JsonProperty("Настройка скилла Единство с землей(Добавляет возможность получать больше ДНК за полную добычу ресурса)")]
            public GatherFriends GatherFriendsSettings;
            [JsonProperty("Настройка скилла Анабиотики(Игроки будут получать больше лечения от препаратов)")]
            public Anabiotics AnabioticsSettings;
            [JsonProperty("Настройка скилла Крафтер(Скорость крафта увеличивается)")]
            public Crafter CrafterSettings;
            [JsonProperty("Настройка НЕЙТРАЛЬНЫХ навыков")]
            public NeutralSkill NeutralSkills;
            internal class NeutralSkill
            {
                [JsonProperty("Увеличивает уровень крафта до продвинутого открывая возможность крафтить предметы с условием продвинутого крафта")]
                public IQCraftSystemAdvancedCraft IQCraftSystemAdvancedCrafts;
                [JsonProperty("Увеличивает шанс выпадения предметов в наборе")]
                public IQKitsRareSkill IQKitsRare;
                [JsonProperty("Уменьшает перезарядку навыков")]
                public IQKitsCooldownPercenct IQKitsCooldown;
                [JsonProperty("Настройка IQHeadReward (Описание и настройка в плагине IQHeadReward)")]
                public IQHeadReward SkillIQHeadRewards;
                internal class IQHeadReward
                {
                    [JsonProperty("Включить умение?(true - включено/false - выключено)")]
                    public bool SkillTurn;
                    [JsonProperty("Общая настройка")]
                    public GeneralSettingsSkill GeneralSettings;
                }

                internal class IQCraftSystemAdvancedCraft
                {
                    [JsonProperty("Включить умение?(true - включено/false - выключено)")]
                    public bool SkillTurn;
                    [JsonProperty("Общая настройка")]
                    public GeneralSettingsSkill GeneralSettings;
                }

                internal class IQKitsRareSkill
                {
                    [JsonProperty("Включить умение?(true - включено/false - выключено)")]
                    public bool SkillTurn;
                    [JsonProperty("Общая настройка")]
                    public GeneralSettingsSkill GeneralSettings;
                    [JsonProperty("На сколько % увеличивать шанс выпадения предметов?")]
                    public int RareUP;
                }

                internal class IQKitsCooldownPercenct
                {
                    [JsonProperty("Включить умение?(true - включено/false - выключено)")]
                    public bool SkillTurn;
                    [JsonProperty("Общая настройка")]
                    public GeneralSettingsSkill GeneralSettings;
                    [JsonProperty("На сколько % уменьшать перезарядку набора?")]
                    public int PercentDrop;
                }

            }

            internal class Miner
            {
                [JsonProperty("Включить умение?(true - включено/false - выключено)")]
                public bool SkillTurn;
                [JsonProperty("Общая настройка")]
                public GeneralSettingsSkill GeneralSettings;
                [JsonProperty("На сколько умножать рейты(все сразу)")]
                public float Rate;
                [JsonProperty("Использовать кастомные множители(true - да/false - нет)")]
                public bool UseLists;
                [JsonProperty("Использовать кастомные множители([Shortname] = множитель)")]
                public Dictionary<string, float> CustomRate;
            }

            internal class Regeneration
            {
                [JsonProperty("Включить умение?(true - включено/false - выключено)")]
                public bool SkillTurn;
                [JsonProperty("Общая настройка")]
                public GeneralSettingsSkill GeneralSettings;
                [JsonProperty("Сколько жизней регенерировать в промежуток времени")]
                public int HealtRegeneration;
                [JsonProperty("Раз в сколько секунд регенерировать игрока")]
                public int RegenerationTimer;
            }

            internal class Military
            {
                [JsonProperty("Включить умение?(true - включено/false - выключено)")]
                public bool SkillTurn;
                [JsonProperty("Общая настройка")]
                public GeneralSettingsSkill GeneralSettings;
                [JsonProperty("На сколько процентов снижать изнашивания оружий(0-100%)")]
                public int PercentNoBroken;
            }

            internal class ThickSkin
            {
                [JsonProperty("Включить умение?(true - включено/false - выключено)")]
                public bool SkillTurn;
                [JsonProperty("Общая настройка")]
                public GeneralSettingsSkill GeneralSettings;
            }

            internal class WoundedShake
            {
                [JsonProperty("Включить умение?(true - включено/false - выключено)")]
                public bool SkillTurn;
                [JsonProperty("Общая настройка")]
                public GeneralSettingsSkill GeneralSettings;
                [JsonProperty("Шанс встать в момент когда игрока положат")]
                public int Rare;
                [JsonProperty("Через сколько секунд поднимать игрока при падаение,если шанс успешен")]
                public int RareStartTime;
                [JsonProperty("После успешного срабатывания - забирать скилл(true - да/false - нет)")]
                public bool DropSkill;
            }

            internal class Metabolism
            {
                [JsonProperty("Включить умение?(true - включено/false - выключено)")]
                public bool SkillTurn;
                [JsonProperty("Общая настройка")]
                public GeneralSettingsSkill GeneralSettings;
                [JsonProperty("Сколько ХП будет при возрождениие")]
                public int Health;
                [JsonProperty("Сколько Сытности будет при возрождении")]
                public int Calories;
                [JsonProperty("Сколько Жажды будет при возрождении")]
                public int Hydration;
                [JsonProperty("Шанс проснуться с данными показателями")]
                public int RareMetabolisme;
                [JsonProperty("После успешного срабатывания - забирать скилл(true - да/false - нет)")]
                public bool DropSkill;
            }

            internal class PatogenAmrory
            {
                [JsonProperty("Включить умение?(true - включено/false - выключено)")]
                public bool SkillTurn;
                [JsonProperty("Общая настройка")]
                public GeneralSettingsSkill GeneralSettings;
            }

            internal class PatogenKill
            {
                [JsonProperty("Включить умение?(true - включено/false - выключено)")]
                public bool SkillTurn;
                [JsonProperty("Общая настройка")]
                public GeneralSettingsSkill GeneralSettings;
            }

            internal class GenesisGens
            {
                [JsonProperty("Включить умение?(true - включено/false - выключено)")]
                public bool SkillTurn;
                [JsonProperty("Общая настройка")]
                public GeneralSettingsSkill GeneralSettings;
                [JsonProperty("Сколько % ДНК оставлять в конце вайпа(будет отсчитываться от затраченного количества на скиллы)0 - 100")]
                public int PercentSave;
            }

            internal class AnimalFriends
            {
                [JsonProperty("Включить умение?(true - включено/false - выключено)")]
                public bool SkillTurn;
                [JsonProperty("Общая настройка")]
                public GeneralSettingsSkill GeneralSettings;
                [JsonProperty("Min : Сколько выдавать ДНК")]
                public int MinDNKAnimal;
                [JsonProperty("Max : Сколько выдавать ДНК")]
                public int MaxDNKAnimal;
                [JsonProperty("Использовать кастомный лист(true - да/false - нет)")]
                public bool UseLists;
                [JsonProperty("Кастомный лист ,на сколько увеличивать количество ДНК за убийство животного [Animal] = Настройка")]
                public Dictionary<string, CustomSettings> AnimalsList;
                [JsonProperty("Шанс получения дополнительного ДНК")]
                public int RareAll;
                [JsonProperty("Животные с которых будет падать ДНК")]
                public List<string> AnimalDetected;
                internal class CustomSettings
                {
                    [JsonProperty("Шанс получения дополнительного ДНК")]
                    public int Rare;
                    [JsonProperty("Min : Сколько выдавать ДНК")]
                    public int MinDNKCustom;
                    [JsonProperty("Max : Сколько выдавать ДНК")]
                    public int MaxDNKCustom;
                }

            }

            internal class GatherFriends
            {
                [JsonProperty("Включить умение?(true - включено/false - выключено)")]
                public bool SkillTurn;
                [JsonProperty("Общая настройка")]
                public GeneralSettingsSkill GeneralSettings;
                [JsonProperty("Кастомный лист ,на сколько увеличивать количество ДНК за полную добычу ресурса [Shortname] = Настройка")]
                public Dictionary<string, CustomSettings> GatherList;
                internal class CustomSettings
                {
                    [JsonProperty("Шанс получения дополнительного ДНК")]
                    public int Rare;
                    [JsonProperty("Min : Сколько выдавать ДНК")]
                    public int MinDNKCustom;
                    [JsonProperty("Max : Сколько выдавать ДНК")]
                    public int MaxDNKCustom;
                }

            }

            internal class Anabiotics
            {
                [JsonProperty("Включить умение?(true - включено/false - выключено)")]
                public bool SkillTurn;
                [JsonProperty("Общая настройка")]
                public GeneralSettingsSkill GeneralSettings;
                [JsonProperty("На сколько увеличивать количество ХП за использование медицины[Shortname] = Amount")]
                public Dictionary<string, int> AnabioticsList;
            }

            internal class Crafter
            {
                [JsonProperty("Включить умение?(true - включено/false - выключено)")]
                public bool SkillTurn;
                [JsonProperty("Общая настройка")]
                public GeneralSettingsSkill GeneralSettings;
                [JsonProperty("В сколько раз увеличивать скорость крафта? (Пример : изначально 60 секунд, увеличение в 3 раза = 60/3 = 20)")]
                public int CraftBoost;
            }

            internal class GeneralSettingsSkill
            {
                [JsonProperty("Отображаемое имя")]
                public string DisplayName;
                [JsonProperty("Описание скилла")]
                public string Description;
                [JsonProperty("Sprite для иконки")]
                public string Sprite;
                [JsonProperty("PNG-ссылка для иконки(64х64).Если используете это,спрайт будет игнорироваться")]
                public string PNG;
                [JsonProperty("Цена за изучение")]
                public int PriceDNK;
            }

        }

        internal class InterfaceSetting
        {
            [JsonProperty("Настройка элементов дизайна")]
            public Icons IconsPNG;
            [JsonProperty("Настройка основных элементов")]
            public General GeneralSettings;
            internal class Icons
            {
                [JsonProperty("Ссылка PNG на задний фон")]
                public string BackgroundPNG;
                [JsonProperty("Ссылка ссылка на иконку заблокированного навыка PNG")]
                public string BlockSkill;
                [JsonProperty("Ссылка ссылка на иконку доступного навыка PNG")]
                public string AvailableSkill;
                [JsonProperty("Ссылка ссылка на иконку изученного навыка PNG")]
                public string ReceivedSkill;
                [JsonProperty("Ссылка на кнопку для получения данного скилла(развить)")]
                public string ButtonTakeSkill;
                [JsonProperty("Ссылка панель для изучения скилла")]
                public string BackgroundTakePanel;
                [JsonProperty("Ссылка ссылка на иконку НЕЙТРАЛЬНОГО заблокированного навыка PNG")]
                public string NeutralBlockSkill;
                [JsonProperty("Ссылка ссылка на иконку НЕЙТРАЛЬНОГО доступного навыка PNG")]
                public string NeutralAvailableSkill;
                [JsonProperty("Ссылка ссылка на иконку НЕЙТРАЛЬНОГО изученного навыка PNG")]
                public string NeutralReceivedSkill;
            }

            internal class General
            {
                [JsonProperty("Цвет текста")]
                public string HexLabels;
                [JsonProperty("Цвет текста навыков")]
                public string HexLabelsSkill;
                [JsonProperty("Цвет текста описания навыков")]
                public string HexLabelTakePanel;
            }

        }

        internal class GeneralSetting
        {
            [JsonProperty("Настройки автоматической очисти даты после вайпа")]
            public WipeContoller WipeContollers;
            [JsonProperty("Управление вирусом ПАТОГЕН")]
            public VirusPatogen VirusPatogens;
            internal class WipeContoller
            {
                [JsonProperty("Включить автоматическую очистку даты после вайпа сервера")]
                public bool WipeDataUse;
                [JsonProperty("Очищать скиллы игроков после вайпа")]
                public bool WipeDataSkill;
            }

            internal class VirusPatogen
            {
                [JsonProperty("Включить вирус-патоген(Данный вирус будет удалять у игрока 1 случайный навык через N количество времени,у них так же будут навыки на излечение и защиты от заражаения)")]
                public bool UsePatogen;
                [JsonProperty("Шанс заражения вирусом")]
                public int RareInfected;
                [JsonProperty("Раз в сколько времени начинать инфекцию игроков(Секунды)")]
                public int TimerInfectedVirus;
                [JsonProperty("Через сколько удалять 1 случайный навык игроку(секунды)")]
                public int TimerRemoveSkill;
            }

        }

        internal class ReferenceSetting
        {
            [JsonProperty("Настройка IQChat")]
            public IQChat IQChatSettings;
            [JsonProperty("Включить поддержку IQEconomic(ДНК заменится на валюту экономики)")]
            public bool IQEconomicUse;
            [JsonProperty("Включить поддержку IQHeadReward")]
            public bool IQHeadRewardUse;
            [JsonProperty("Включить поддержку IQCraftSystem")]
            public bool IQCraftSystem;
            [JsonProperty("Включить поддержку IQKits")]
            public bool IQKits;
            [JsonProperty("Настройка XDNotifications")]
            public XDNotifications XDNotificationsSettings;
            internal class XDNotifications
            {
                [JsonProperty("Включить поддержку XDNotifications(Некоторые уведомления будут приходить в XDNotifications)")]
                public bool UseXDNotifications;
                [JsonProperty("Цвет заднего фона уведомления(HEX)")]
                public string Color;
                [JsonProperty("Через сколько удалиться уведомление")]
                public int AlertDelete;
                [JsonProperty("Звуковой эффект")]
                public string SoundEffect;
                [JsonProperty("Оглавление")]
                public string Title;
            }

            internal class IQChat
            {
                [JsonProperty("IQChat : Кастомный префикс в чате")]
                public string CustomPrefix;
                [JsonProperty("IQChat : Кастомный аватар в чате(Если требуется)")]
                public string CustomAvatar;
            }

        }

        internal class FarmingDNK
        {
            [JsonProperty("Настройка получения ДНК за убийство игроков")]
            public PlayerKill PlayerKills;
            [JsonProperty("Настройка получения ДНК за убийство NPC")]
            public NPCKill NPCKills;
            [JsonProperty("Настройка получения ДНК за убийство животных")]
            public AnimalKill AnimalKills;
            internal class PlayerKill
            {
                [JsonProperty("Получение ДНК за убийство игроков")]
                public bool DNKKillUser;
                [JsonProperty("Параметры")]
                public GeneralSettingsFarming GeneralSettingsFarmings;
            }

            internal class NPCKill
            {
                [JsonProperty("Получение валюты за убийство NPC")]
                public bool DNKKillNPC;
                [JsonProperty("Параметры")]
                public GeneralSettingsFarming GeneralSettingsFarmings;
            }

            internal class AnimalKill
            {
                [JsonProperty("Получение валюты за убийство животных")]
                public bool DNKKillAnimal;
                [JsonProperty("Параметры")]
                public GeneralSettingsFarming GeneralSettingsFarmings;
            }

            internal class GeneralSettingsFarming
            {
                [JsonProperty("Шанс получения ДНК")]
                public int RareGiveDNK;
                [JsonProperty("Минимальное количество ДНК")]
                public int MinimumDNK;
                [JsonProperty("Максимальное количество ДНК")]
                public int MaximumDNK;
            }

        }

        public static Configuration GetNewConfiguration();
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private void Init();
    private void OnServerInitialized();
     void OnPlayerConnected(BasePlayer player);
     void Unload();
     void OnEntityDeath(BaseCombatEntity entity, HitInfo info);
     Dictionary<ulong, double> Cd;
     object OnHealingItemUse(MedicalTool tool, BasePlayer player);
     object OnItemAction(Item item, string action, BasePlayer player);
     object OnDispenserBonus(ResourceDispenser dispenser, BasePlayer player, Item item);
     object OnDispenserGather(ResourceDispenser dispenser, BaseEntity entity, Item item);
    private void OnPlayerRespawned(BasePlayer player);
    private void OnPlayerWound(BasePlayer player);
     void OnNewSave(string filename);
     void OnRunPlayerMetabolism(PlayerMetabolism metabolism, BaseCombatEntity combat);
     void OnLoseCondition(Item item, float amount);
    public Dictionary<string, float> Blueprints { get; set; }
    private object OnItemCraft(ItemCraftTask item, BasePlayer crafter);
    private object OnCraft(ItemCraftTask task, BasePlayer crafter);
    public float GetTime(BasePlayer crafter, string CraftItem, float Time);
    [ChatCommand("skill")]
     void ChatCommandSkill(BasePlayer player);
    [ConsoleCommand("iqps")]
     void IQPlagueSkillCommands(ConsoleSystem.Arg arg);
    public void Anabiotics(BasePlayer player, string Shortname);
     void AnimalFriends(BasePlayer player, string Animal);
     void GatherFriends(BasePlayer player, string Shortname);
    public void Metabolism(BasePlayer player);
     int Miner(BasePlayer player, string Shortname, int Amount);
    public void Wounded(BasePlayer player);
     void GenesisGens();
     void ThicksSkin(BasePlayer player);
    public float Military(BasePlayer player, Item item, float amount);
    public void StartRegeneration();
    public void Regeneration(BasePlayer player);
     void RedistributionHooks();
     void WipeController();
    public void StudySkill(BasePlayer player, string Skill, int Price, bool IsInfo);
     void RemoveBalance(BasePlayer player, int Price, bool SkillEconomic);
    public void PatogenInfected();
    public void PatogenAttack();
    public void PatogenRecovered(BasePlayer player);
    public List<ulong> IsOpenSkill;
    static DateTime epoch;
    static double CurrentTime();
    public bool IsAvailable(int Balance, int Price);
    public bool IsRare(int Rare);
    public void GiveDNK(BasePlayer player, int Min, int Max);
    public void GiveDNK(BasePlayer player, int Amount);
    static string PLAGUE_PARENT_MAIN;
    static string PLAGUE_PARENT_PANEL_INFORMATION;
    public void Interface_Panel(BasePlayer player);
    public void LoadedSkills(BasePlayer player);
     void Interface_Skill_Crafter(BasePlayer player);
     void Interface_Skill_Anabiotics(BasePlayer player);
     void Interface_Skill_AnimalFriends(BasePlayer player);
     void Interface_Skill_GatherFriends(BasePlayer player);
     void Interface_Skill_GenesisGen(BasePlayer player);
     void Interface_Skill_Metabolism(BasePlayer player);
     void Interface_Skill_Military(BasePlayer player);
     void Interface_Skill_Miner(BasePlayer player);
     void Interface_Skill_PatogenArmory(BasePlayer player);
     void Interface_Skill_PatogenKill(BasePlayer player);
     void Interface_Skill_Regeneration(BasePlayer player);
     void Interface_Skill_ThicksSkin(BasePlayer player);
     void Interface_Skill_Wounded(BasePlayer player);
     void Interface_Skill_IQHeadReward(BasePlayer player);
     void Interface_Skill_Advanced_IQCraftSystem(BasePlayer player);
     void Interface_Skill_DropPercent_IQKits(BasePlayer player);
     void Interface_Skill_Rare_IQKits(BasePlayer player);
     void Interface_Balance(BasePlayer player);
     void Interface_Show_Information(BasePlayer player, Configuration.Skills.GeneralSettingsSkill Information, string Skill, TypeSkill typeSkill);
    private static string HexToRustFormat(string hex);
    private new void LoadDefaultMessages();
     bool API_HEAD_REWARD_SKILL(BasePlayer player);
     bool API_IS_RARE_SKILL_KITS(BasePlayer player);
     bool API_IS_COOLDOWN_SKILL_KITS(BasePlayer player);
     bool API_IS_ADVANCED_CRAFT(BasePlayer player);
     int API_GET_RARE_IQKITS();
     int API_GET_COOLDOWN_IQKITS();
}

public class InformationSkills
{
    [JsonProperty("Шахтер")]
    public bool Miner;
    [JsonProperty("Регенерация")]
    public bool Regeneration;
    [JsonProperty("Военный")]
    public bool Military;
    [JsonProperty("Кожа")]
    public bool ThickSkin;
    [JsonProperty("Дух")]
    public bool WoundedShake;
    [JsonProperty("Метаболизм")]
    public bool Metabolism;
    [JsonProperty("Защита от патогена")]
    public bool PatogenAmrory;
    [JsonProperty("Генезиз ген")]
    public bool GenesisGens;
    [JsonProperty("Единство с животными")]
    public bool AnimalFriends;
    [JsonProperty("Единство с природой")]
    public bool GatherFriends;
    [JsonProperty("Анабиотики")]
    public bool Anabiotics;
    [JsonProperty("Крафтер")]
    public bool Crafter;
    [JsonProperty("IQHeadReward")]
    public bool IQHeadReward;
    [JsonProperty("IQCraftSystem : Продвинутый крафтер")]
    public bool IQCraftSystemAdvanced;
    [JsonProperty("IQKits : Уменьшенная перезарядка")]
    public bool IQKitsCooldownPercent;
    [JsonProperty("IQKits : Увеличенный шанс выпадения")]
    public bool IQKitsRareup;
    [JsonProperty("Имеется ли патоген")]
    public bool PatogenAttack;
}

private class Configuration
{
    [JsonProperty("Настройка плагина")]
    public GeneralSetting GeneralSettings;
    [JsonProperty("Настройка всех скиллов")]
    public Skills SkillSettings;
    [JsonProperty("Настройка интерфейса")]
    public InterfaceSetting InterfaceSettings;
    [JsonProperty("Настройка совместимостей с другими плагинами")]
    public ReferenceSetting ReferenceSettings;
    [JsonProperty("Настройка получения ДНК")]
    public FarmingDNK FarmingDNKS;
    internal class Skills
    {
        [JsonProperty("Разрешить просмотр навыка без нужного кол-во ДНК")]
        public bool ShowSkillNotDNK;
        [JsonProperty("Настройка скилла шахтер(Увеличивает рейт добычи)")]
        public Miner MinerSettings;
        [JsonProperty("Настройка скилла регенерации(Регенирация ХП после боя)")]
        public Regeneration RegenerationSettings;
        [JsonProperty("Настройка скилла военный(Уменьшает изнашивания оружия)")]
        public Military MilitarySettings;
        [JsonProperty("Настройка скилла Твердая кожа(Защищает от холода в зимних биомах)")]
        public ThickSkin ThickSkinSettings;
        [JsonProperty("Настройка скилла Не поколебим(Дает шанс встать после падения)")]
        public WoundedShake WoundedShakeSettings;
        [JsonProperty("Настройка скилла Метаболизм(При возраждении дает N значения сытности,хп,жажды)")]
        public Metabolism MetabolismSettings;
        [JsonProperty("Настройка скилла Защита от патогена(Одноразовая защита от грибка Патоген(При заражаении патогеном , вирус - со временем разрушает скиллы игрока))")]
        public PatogenAmrory PatogenAmrorySettings;
        [JsonProperty("Настройка скилла Убийство патогена(Одноразовое убийство грибка,если игрок заразился Патогеном)")]
        public PatogenKill PatogenKillSettings;
        [JsonProperty("Настройка скилла Генезиз ген(Сохраняет % ДНК в конце вайпа)")]
        public GenesisGens GenesisGensSettings;
        [JsonProperty("Настройка скилла Единство с природой(Добавляет возможность получать больше ДНК за добычу животных)")]
        public AnimalFriends AnimalFriendsSettings;
        [JsonProperty("Настройка скилла Единство с землей(Добавляет возможность получать больше ДНК за полную добычу ресурса)")]
        public GatherFriends GatherFriendsSettings;
        [JsonProperty("Настройка скилла Анабиотики(Игроки будут получать больше лечения от препаратов)")]
        public Anabiotics AnabioticsSettings;
        [JsonProperty("Настройка скилла Крафтер(Скорость крафта увеличивается)")]
        public Crafter CrafterSettings;
        [JsonProperty("Настройка НЕЙТРАЛЬНЫХ навыков")]
        public NeutralSkill NeutralSkills;
        internal class NeutralSkill
        {
            [JsonProperty("Увеличивает уровень крафта до продвинутого открывая возможность крафтить предметы с условием продвинутого крафта")]
            public IQCraftSystemAdvancedCraft IQCraftSystemAdvancedCrafts;
            [JsonProperty("Увеличивает шанс выпадения предметов в наборе")]
            public IQKitsRareSkill IQKitsRare;
            [JsonProperty("Уменьшает перезарядку навыков")]
            public IQKitsCooldownPercenct IQKitsCooldown;
            [JsonProperty("Настройка IQHeadReward (Описание и настройка в плагине IQHeadReward)")]
            public IQHeadReward SkillIQHeadRewards;
            internal class IQHeadReward
            {
                [JsonProperty("Включить умение?(true - включено/false - выключено)")]
                public bool SkillTurn;
                [JsonProperty("Общая настройка")]
                public GeneralSettingsSkill GeneralSettings;
            }

            internal class IQCraftSystemAdvancedCraft
            {
                [JsonProperty("Включить умение?(true - включено/false - выключено)")]
                public bool SkillTurn;
                [JsonProperty("Общая настройка")]
                public GeneralSettingsSkill GeneralSettings;
            }

            internal class IQKitsRareSkill
            {
                [JsonProperty("Включить умение?(true - включено/false - выключено)")]
                public bool SkillTurn;
                [JsonProperty("Общая настройка")]
                public GeneralSettingsSkill GeneralSettings;
                [JsonProperty("На сколько % увеличивать шанс выпадения предметов?")]
                public int RareUP;
            }

            internal class IQKitsCooldownPercenct
            {
                [JsonProperty("Включить умение?(true - включено/false - выключено)")]
                public bool SkillTurn;
                [JsonProperty("Общая настройка")]
                public GeneralSettingsSkill GeneralSettings;
                [JsonProperty("На сколько % уменьшать перезарядку набора?")]
                public int PercentDrop;
            }

        }

        internal class Miner
        {
            [JsonProperty("Включить умение?(true - включено/false - выключено)")]
            public bool SkillTurn;
            [JsonProperty("Общая настройка")]
            public GeneralSettingsSkill GeneralSettings;
            [JsonProperty("На сколько умножать рейты(все сразу)")]
            public float Rate;
            [JsonProperty("Использовать кастомные множители(true - да/false - нет)")]
            public bool UseLists;
            [JsonProperty("Использовать кастомные множители([Shortname] = множитель)")]
            public Dictionary<string, float> CustomRate;
        }

        internal class Regeneration
        {
            [JsonProperty("Включить умение?(true - включено/false - выключено)")]
            public bool SkillTurn;
            [JsonProperty("Общая настройка")]
            public GeneralSettingsSkill GeneralSettings;
            [JsonProperty("Сколько жизней регенерировать в промежуток времени")]
            public int HealtRegeneration;
            [JsonProperty("Раз в сколько секунд регенерировать игрока")]
            public int RegenerationTimer;
        }

        internal class Military
        {
            [JsonProperty("Включить умение?(true - включено/false - выключено)")]
            public bool SkillTurn;
            [JsonProperty("Общая настройка")]
            public GeneralSettingsSkill GeneralSettings;
            [JsonProperty("На сколько процентов снижать изнашивания оружий(0-100%)")]
            public int PercentNoBroken;
        }

        internal class ThickSkin
        {
            [JsonProperty("Включить умение?(true - включено/false - выключено)")]
            public bool SkillTurn;
            [JsonProperty("Общая настройка")]
            public GeneralSettingsSkill GeneralSettings;
        }

        internal class WoundedShake
        {
            [JsonProperty("Включить умение?(true - включено/false - выключено)")]
            public bool SkillTurn;
            [JsonProperty("Общая настройка")]
            public GeneralSettingsSkill GeneralSettings;
            [JsonProperty("Шанс встать в момент когда игрока положат")]
            public int Rare;
            [JsonProperty("Через сколько секунд поднимать игрока при падаение,если шанс успешен")]
            public int RareStartTime;
            [JsonProperty("После успешного срабатывания - забирать скилл(true - да/false - нет)")]
            public bool DropSkill;
        }

        internal class Metabolism
        {
            [JsonProperty("Включить умение?(true - включено/false - выключено)")]
            public bool SkillTurn;
            [JsonProperty("Общая настройка")]
            public GeneralSettingsSkill GeneralSettings;
            [JsonProperty("Сколько ХП будет при возрождениие")]
            public int Health;
            [JsonProperty("Сколько Сытности будет при возрождении")]
            public int Calories;
            [JsonProperty("Сколько Жажды будет при возрождении")]
            public int Hydration;
            [JsonProperty("Шанс проснуться с данными показателями")]
            public int RareMetabolisme;
            [JsonProperty("После успешного срабатывания - забирать скилл(true - да/false - нет)")]
            public bool DropSkill;
        }

        internal class PatogenAmrory
        {
            [JsonProperty("Включить умение?(true - включено/false - выключено)")]
            public bool SkillTurn;
            [JsonProperty("Общая настройка")]
            public GeneralSettingsSkill GeneralSettings;
        }

        internal class PatogenKill
        {
            [JsonProperty("Включить умение?(true - включено/false - выключено)")]
            public bool SkillTurn;
            [JsonProperty("Общая настройка")]
            public GeneralSettingsSkill GeneralSettings;
        }

        internal class GenesisGens
        {
            [JsonProperty("Включить умение?(true - включено/false - выключено)")]
            public bool SkillTurn;
            [JsonProperty("Общая настройка")]
            public GeneralSettingsSkill GeneralSettings;
            [JsonProperty("Сколько % ДНК оставлять в конце вайпа(будет отсчитываться от затраченного количества на скиллы)0 - 100")]
            public int PercentSave;
        }

        internal class AnimalFriends
        {
            [JsonProperty("Включить умение?(true - включено/false - выключено)")]
            public bool SkillTurn;
            [JsonProperty("Общая настройка")]
            public GeneralSettingsSkill GeneralSettings;
            [JsonProperty("Min : Сколько выдавать ДНК")]
            public int MinDNKAnimal;
            [JsonProperty("Max : Сколько выдавать ДНК")]
            public int MaxDNKAnimal;
            [JsonProperty("Использовать кастомный лист(true - да/false - нет)")]
            public bool UseLists;
            [JsonProperty("Кастомный лист ,на сколько увеличивать количество ДНК за убийство животного [Animal] = Настройка")]
            public Dictionary<string, CustomSettings> AnimalsList;
            [JsonProperty("Шанс получения дополнительного ДНК")]
            public int RareAll;
            [JsonProperty("Животные с которых будет падать ДНК")]
            public List<string> AnimalDetected;
            internal class CustomSettings
            {
                [JsonProperty("Шанс получения дополнительного ДНК")]
                public int Rare;
                [JsonProperty("Min : Сколько выдавать ДНК")]
                public int MinDNKCustom;
                [JsonProperty("Max : Сколько выдавать ДНК")]
                public int MaxDNKCustom;
            }

        }

        internal class GatherFriends
        {
            [JsonProperty("Включить умение?(true - включено/false - выключено)")]
            public bool SkillTurn;
            [JsonProperty("Общая настройка")]
            public GeneralSettingsSkill GeneralSettings;
            [JsonProperty("Кастомный лист ,на сколько увеличивать количество ДНК за полную добычу ресурса [Shortname] = Настройка")]
            public Dictionary<string, CustomSettings> GatherList;
            internal class CustomSettings
            {
                [JsonProperty("Шанс получения дополнительного ДНК")]
                public int Rare;
                [JsonProperty("Min : Сколько выдавать ДНК")]
                public int MinDNKCustom;
                [JsonProperty("Max : Сколько выдавать ДНК")]
                public int MaxDNKCustom;
            }

        }

        internal class Anabiotics
        {
            [JsonProperty("Включить умение?(true - включено/false - выключено)")]
            public bool SkillTurn;
            [JsonProperty("Общая настройка")]
            public GeneralSettingsSkill GeneralSettings;
            [JsonProperty("На сколько увеличивать количество ХП за использование медицины[Shortname] = Amount")]
            public Dictionary<string, int> AnabioticsList;
        }

        internal class Crafter
        {
            [JsonProperty("Включить умение?(true - включено/false - выключено)")]
            public bool SkillTurn;
            [JsonProperty("Общая настройка")]
            public GeneralSettingsSkill GeneralSettings;
            [JsonProperty("В сколько раз увеличивать скорость крафта? (Пример : изначально 60 секунд, увеличение в 3 раза = 60/3 = 20)")]
            public int CraftBoost;
        }

        internal class GeneralSettingsSkill
        {
            [JsonProperty("Отображаемое имя")]
            public string DisplayName;
            [JsonProperty("Описание скилла")]
            public string Description;
            [JsonProperty("Sprite для иконки")]
            public string Sprite;
            [JsonProperty("PNG-ссылка для иконки(64х64).Если используете это,спрайт будет игнорироваться")]
            public string PNG;
            [JsonProperty("Цена за изучение")]
            public int PriceDNK;
        }

    }

    internal class InterfaceSetting
    {
        [JsonProperty("Настройка элементов дизайна")]
        public Icons IconsPNG;
        [JsonProperty("Настройка основных элементов")]
        public General GeneralSettings;
        internal class Icons
        {
            [JsonProperty("Ссылка PNG на задний фон")]
            public string BackgroundPNG;
            [JsonProperty("Ссылка ссылка на иконку заблокированного навыка PNG")]
            public string BlockSkill;
            [JsonProperty("Ссылка ссылка на иконку доступного навыка PNG")]
            public string AvailableSkill;
            [JsonProperty("Ссылка ссылка на иконку изученного навыка PNG")]
            public string ReceivedSkill;
            [JsonProperty("Ссылка на кнопку для получения данного скилла(развить)")]
            public string ButtonTakeSkill;
            [JsonProperty("Ссылка панель для изучения скилла")]
            public string BackgroundTakePanel;
            [JsonProperty("Ссылка ссылка на иконку НЕЙТРАЛЬНОГО заблокированного навыка PNG")]
            public string NeutralBlockSkill;
            [JsonProperty("Ссылка ссылка на иконку НЕЙТРАЛЬНОГО доступного навыка PNG")]
            public string NeutralAvailableSkill;
            [JsonProperty("Ссылка ссылка на иконку НЕЙТРАЛЬНОГО изученного навыка PNG")]
            public string NeutralReceivedSkill;
        }

        internal class General
        {
            [JsonProperty("Цвет текста")]
            public string HexLabels;
            [JsonProperty("Цвет текста навыков")]
            public string HexLabelsSkill;
            [JsonProperty("Цвет текста описания навыков")]
            public string HexLabelTakePanel;
        }

    }

    internal class GeneralSetting
    {
        [JsonProperty("Настройки автоматической очисти даты после вайпа")]
        public WipeContoller WipeContollers;
        [JsonProperty("Управление вирусом ПАТОГЕН")]
        public VirusPatogen VirusPatogens;
        internal class WipeContoller
        {
            [JsonProperty("Включить автоматическую очистку даты после вайпа сервера")]
            public bool WipeDataUse;
            [JsonProperty("Очищать скиллы игроков после вайпа")]
            public bool WipeDataSkill;
        }

        internal class VirusPatogen
        {
            [JsonProperty("Включить вирус-патоген(Данный вирус будет удалять у игрока 1 случайный навык через N количество времени,у них так же будут навыки на излечение и защиты от заражаения)")]
            public bool UsePatogen;
            [JsonProperty("Шанс заражения вирусом")]
            public int RareInfected;
            [JsonProperty("Раз в сколько времени начинать инфекцию игроков(Секунды)")]
            public int TimerInfectedVirus;
            [JsonProperty("Через сколько удалять 1 случайный навык игроку(секунды)")]
            public int TimerRemoveSkill;
        }

    }

    internal class ReferenceSetting
    {
        [JsonProperty("Настройка IQChat")]
        public IQChat IQChatSettings;
        [JsonProperty("Включить поддержку IQEconomic(ДНК заменится на валюту экономики)")]
        public bool IQEconomicUse;
        [JsonProperty("Включить поддержку IQHeadReward")]
        public bool IQHeadRewardUse;
        [JsonProperty("Включить поддержку IQCraftSystem")]
        public bool IQCraftSystem;
        [JsonProperty("Включить поддержку IQKits")]
        public bool IQKits;
        [JsonProperty("Настройка XDNotifications")]
        public XDNotifications XDNotificationsSettings;
        internal class XDNotifications
        {
            [JsonProperty("Включить поддержку XDNotifications(Некоторые уведомления будут приходить в XDNotifications)")]
            public bool UseXDNotifications;
            [JsonProperty("Цвет заднего фона уведомления(HEX)")]
            public string Color;
            [JsonProperty("Через сколько удалиться уведомление")]
            public int AlertDelete;
            [JsonProperty("Звуковой эффект")]
            public string SoundEffect;
            [JsonProperty("Оглавление")]
            public string Title;
        }

        internal class IQChat
        {
            [JsonProperty("IQChat : Кастомный префикс в чате")]
            public string CustomPrefix;
            [JsonProperty("IQChat : Кастомный аватар в чате(Если требуется)")]
            public string CustomAvatar;
        }

    }

    internal class FarmingDNK
    {
        [JsonProperty("Настройка получения ДНК за убийство игроков")]
        public PlayerKill PlayerKills;
        [JsonProperty("Настройка получения ДНК за убийство NPC")]
        public NPCKill NPCKills;
        [JsonProperty("Настройка получения ДНК за убийство животных")]
        public AnimalKill AnimalKills;
        internal class PlayerKill
        {
            [JsonProperty("Получение ДНК за убийство игроков")]
            public bool DNKKillUser;
            [JsonProperty("Параметры")]
            public GeneralSettingsFarming GeneralSettingsFarmings;
        }

        internal class NPCKill
        {
            [JsonProperty("Получение валюты за убийство NPC")]
            public bool DNKKillNPC;
            [JsonProperty("Параметры")]
            public GeneralSettingsFarming GeneralSettingsFarmings;
        }

        internal class AnimalKill
        {
            [JsonProperty("Получение валюты за убийство животных")]
            public bool DNKKillAnimal;
            [JsonProperty("Параметры")]
            public GeneralSettingsFarming GeneralSettingsFarmings;
        }

        internal class GeneralSettingsFarming
        {
            [JsonProperty("Шанс получения ДНК")]
            public int RareGiveDNK;
            [JsonProperty("Минимальное количество ДНК")]
            public int MinimumDNK;
            [JsonProperty("Максимальное количество ДНК")]
            public int MaximumDNK;
        }

    }

    public static Configuration GetNewConfiguration();
}

internal class Skills
{
    [JsonProperty("Разрешить просмотр навыка без нужного кол-во ДНК")]
    public bool ShowSkillNotDNK;
    [JsonProperty("Настройка скилла шахтер(Увеличивает рейт добычи)")]
    public Miner MinerSettings;
    [JsonProperty("Настройка скилла регенерации(Регенирация ХП после боя)")]
    public Regeneration RegenerationSettings;
    [JsonProperty("Настройка скилла военный(Уменьшает изнашивания оружия)")]
    public Military MilitarySettings;
    [JsonProperty("Настройка скилла Твердая кожа(Защищает от холода в зимних биомах)")]
    public ThickSkin ThickSkinSettings;
    [JsonProperty("Настройка скилла Не поколебим(Дает шанс встать после падения)")]
    public WoundedShake WoundedShakeSettings;
    [JsonProperty("Настройка скилла Метаболизм(При возраждении дает N значения сытности,хп,жажды)")]
    public Metabolism MetabolismSettings;
    [JsonProperty("Настройка скилла Защита от патогена(Одноразовая защита от грибка Патоген(При заражаении патогеном , вирус - со временем разрушает скиллы игрока))")]
    public PatogenAmrory PatogenAmrorySettings;
    [JsonProperty("Настройка скилла Убийство патогена(Одноразовое убийство грибка,если игрок заразился Патогеном)")]
    public PatogenKill PatogenKillSettings;
    [JsonProperty("Настройка скилла Генезиз ген(Сохраняет % ДНК в конце вайпа)")]
    public GenesisGens GenesisGensSettings;
    [JsonProperty("Настройка скилла Единство с природой(Добавляет возможность получать больше ДНК за добычу животных)")]
    public AnimalFriends AnimalFriendsSettings;
    [JsonProperty("Настройка скилла Единство с землей(Добавляет возможность получать больше ДНК за полную добычу ресурса)")]
    public GatherFriends GatherFriendsSettings;
    [JsonProperty("Настройка скилла Анабиотики(Игроки будут получать больше лечения от препаратов)")]
    public Anabiotics AnabioticsSettings;
    [JsonProperty("Настройка скилла Крафтер(Скорость крафта увеличивается)")]
    public Crafter CrafterSettings;
    [JsonProperty("Настройка НЕЙТРАЛЬНЫХ навыков")]
    public NeutralSkill NeutralSkills;
    internal class NeutralSkill
    {
        [JsonProperty("Увеличивает уровень крафта до продвинутого открывая возможность крафтить предметы с условием продвинутого крафта")]
        public IQCraftSystemAdvancedCraft IQCraftSystemAdvancedCrafts;
        [JsonProperty("Увеличивает шанс выпадения предметов в наборе")]
        public IQKitsRareSkill IQKitsRare;
        [JsonProperty("Уменьшает перезарядку навыков")]
        public IQKitsCooldownPercenct IQKitsCooldown;
        [JsonProperty("Настройка IQHeadReward (Описание и настройка в плагине IQHeadReward)")]
        public IQHeadReward SkillIQHeadRewards;
        internal class IQHeadReward
        {
            [JsonProperty("Включить умение?(true - включено/false - выключено)")]
            public bool SkillTurn;
            [JsonProperty("Общая настройка")]
            public GeneralSettingsSkill GeneralSettings;
        }

        internal class IQCraftSystemAdvancedCraft
        {
            [JsonProperty("Включить умение?(true - включено/false - выключено)")]
            public bool SkillTurn;
            [JsonProperty("Общая настройка")]
            public GeneralSettingsSkill GeneralSettings;
        }

        internal class IQKitsRareSkill
        {
            [JsonProperty("Включить умение?(true - включено/false - выключено)")]
            public bool SkillTurn;
            [JsonProperty("Общая настройка")]
            public GeneralSettingsSkill GeneralSettings;
            [JsonProperty("На сколько % увеличивать шанс выпадения предметов?")]
            public int RareUP;
        }

        internal class IQKitsCooldownPercenct
        {
            [JsonProperty("Включить умение?(true - включено/false - выключено)")]
            public bool SkillTurn;
            [JsonProperty("Общая настройка")]
            public GeneralSettingsSkill GeneralSettings;
            [JsonProperty("На сколько % уменьшать перезарядку набора?")]
            public int PercentDrop;
        }

    }

    internal class Miner
    {
        [JsonProperty("Включить умение?(true - включено/false - выключено)")]
        public bool SkillTurn;
        [JsonProperty("Общая настройка")]
        public GeneralSettingsSkill GeneralSettings;
        [JsonProperty("На сколько умножать рейты(все сразу)")]
        public float Rate;
        [JsonProperty("Использовать кастомные множители(true - да/false - нет)")]
        public bool UseLists;
        [JsonProperty("Использовать кастомные множители([Shortname] = множитель)")]
        public Dictionary<string, float> CustomRate;
    }

    internal class Regeneration
    {
        [JsonProperty("Включить умение?(true - включено/false - выключено)")]
        public bool SkillTurn;
        [JsonProperty("Общая настройка")]
        public GeneralSettingsSkill GeneralSettings;
        [JsonProperty("Сколько жизней регенерировать в промежуток времени")]
        public int HealtRegeneration;
        [JsonProperty("Раз в сколько секунд регенерировать игрока")]
        public int RegenerationTimer;
    }

    internal class Military
    {
        [JsonProperty("Включить умение?(true - включено/false - выключено)")]
        public bool SkillTurn;
        [JsonProperty("Общая настройка")]
        public GeneralSettingsSkill GeneralSettings;
        [JsonProperty("На сколько процентов снижать изнашивания оружий(0-100%)")]
        public int PercentNoBroken;
    }

    internal class ThickSkin
    {
        [JsonProperty("Включить умение?(true - включено/false - выключено)")]
        public bool SkillTurn;
        [JsonProperty("Общая настройка")]
        public GeneralSettingsSkill GeneralSettings;
    }

    internal class WoundedShake
    {
        [JsonProperty("Включить умение?(true - включено/false - выключено)")]
        public bool SkillTurn;
        [JsonProperty("Общая настройка")]
        public GeneralSettingsSkill GeneralSettings;
        [JsonProperty("Шанс встать в момент когда игрока положат")]
        public int Rare;
        [JsonProperty("Через сколько секунд поднимать игрока при падаение,если шанс успешен")]
        public int RareStartTime;
        [JsonProperty("После успешного срабатывания - забирать скилл(true - да/false - нет)")]
        public bool DropSkill;
    }

    internal class Metabolism
    {
        [JsonProperty("Включить умение?(true - включено/false - выключено)")]
        public bool SkillTurn;
        [JsonProperty("Общая настройка")]
        public GeneralSettingsSkill GeneralSettings;
        [JsonProperty("Сколько ХП будет при возрождениие")]
        public int Health;
        [JsonProperty("Сколько Сытности будет при возрождении")]
        public int Calories;
        [JsonProperty("Сколько Жажды будет при возрождении")]
        public int Hydration;
        [JsonProperty("Шанс проснуться с данными показателями")]
        public int RareMetabolisme;
        [JsonProperty("После успешного срабатывания - забирать скилл(true - да/false - нет)")]
        public bool DropSkill;
    }

    internal class PatogenAmrory
    {
        [JsonProperty("Включить умение?(true - включено/false - выключено)")]
        public bool SkillTurn;
        [JsonProperty("Общая настройка")]
        public GeneralSettingsSkill GeneralSettings;
    }

    internal class PatogenKill
    {
        [JsonProperty("Включить умение?(true - включено/false - выключено)")]
        public bool SkillTurn;
        [JsonProperty("Общая настройка")]
        public GeneralSettingsSkill GeneralSettings;
    }

    internal class GenesisGens
    {
        [JsonProperty("Включить умение?(true - включено/false - выключено)")]
        public bool SkillTurn;
        [JsonProperty("Общая настройка")]
        public GeneralSettingsSkill GeneralSettings;
        [JsonProperty("Сколько % ДНК оставлять в конце вайпа(будет отсчитываться от затраченного количества на скиллы)0 - 100")]
        public int PercentSave;
    }

    internal class AnimalFriends
    {
        [JsonProperty("Включить умение?(true - включено/false - выключено)")]
        public bool SkillTurn;
        [JsonProperty("Общая настройка")]
        public GeneralSettingsSkill GeneralSettings;
        [JsonProperty("Min : Сколько выдавать ДНК")]
        public int MinDNKAnimal;
        [JsonProperty("Max : Сколько выдавать ДНК")]
        public int MaxDNKAnimal;
        [JsonProperty("Использовать кастомный лист(true - да/false - нет)")]
        public bool UseLists;
        [JsonProperty("Кастомный лист ,на сколько увеличивать количество ДНК за убийство животного [Animal] = Настройка")]
        public Dictionary<string, CustomSettings> AnimalsList;
        [JsonProperty("Шанс получения дополнительного ДНК")]
        public int RareAll;
        [JsonProperty("Животные с которых будет падать ДНК")]
        public List<string> AnimalDetected;
        internal class CustomSettings
        {
            [JsonProperty("Шанс получения дополнительного ДНК")]
            public int Rare;
            [JsonProperty("Min : Сколько выдавать ДНК")]
            public int MinDNKCustom;
            [JsonProperty("Max : Сколько выдавать ДНК")]
            public int MaxDNKCustom;
        }

    }

    internal class GatherFriends
    {
        [JsonProperty("Включить умение?(true - включено/false - выключено)")]
        public bool SkillTurn;
        [JsonProperty("Общая настройка")]
        public GeneralSettingsSkill GeneralSettings;
        [JsonProperty("Кастомный лист ,на сколько увеличивать количество ДНК за полную добычу ресурса [Shortname] = Настройка")]
        public Dictionary<string, CustomSettings> GatherList;
        internal class CustomSettings
        {
            [JsonProperty("Шанс получения дополнительного ДНК")]
            public int Rare;
            [JsonProperty("Min : Сколько выдавать ДНК")]
            public int MinDNKCustom;
            [JsonProperty("Max : Сколько выдавать ДНК")]
            public int MaxDNKCustom;
        }

    }

    internal class Anabiotics
    {
        [JsonProperty("Включить умение?(true - включено/false - выключено)")]
        public bool SkillTurn;
        [JsonProperty("Общая настройка")]
        public GeneralSettingsSkill GeneralSettings;
        [JsonProperty("На сколько увеличивать количество ХП за использование медицины[Shortname] = Amount")]
        public Dictionary<string, int> AnabioticsList;
    }

    internal class Crafter
    {
        [JsonProperty("Включить умение?(true - включено/false - выключено)")]
        public bool SkillTurn;
        [JsonProperty("Общая настройка")]
        public GeneralSettingsSkill GeneralSettings;
        [JsonProperty("В сколько раз увеличивать скорость крафта? (Пример : изначально 60 секунд, увеличение в 3 раза = 60/3 = 20)")]
        public int CraftBoost;
    }

    internal class GeneralSettingsSkill
    {
        [JsonProperty("Отображаемое имя")]
        public string DisplayName;
        [JsonProperty("Описание скилла")]
        public string Description;
        [JsonProperty("Sprite для иконки")]
        public string Sprite;
        [JsonProperty("PNG-ссылка для иконки(64х64).Если используете это,спрайт будет игнорироваться")]
        public string PNG;
        [JsonProperty("Цена за изучение")]
        public int PriceDNK;
    }

}

internal class NeutralSkill
{
    [JsonProperty("Увеличивает уровень крафта до продвинутого открывая возможность крафтить предметы с условием продвинутого крафта")]
    public IQCraftSystemAdvancedCraft IQCraftSystemAdvancedCrafts;
    [JsonProperty("Увеличивает шанс выпадения предметов в наборе")]
    public IQKitsRareSkill IQKitsRare;
    [JsonProperty("Уменьшает перезарядку навыков")]
    public IQKitsCooldownPercenct IQKitsCooldown;
    [JsonProperty("Настройка IQHeadReward (Описание и настройка в плагине IQHeadReward)")]
    public IQHeadReward SkillIQHeadRewards;
    internal class IQHeadReward
    {
        [JsonProperty("Включить умение?(true - включено/false - выключено)")]
        public bool SkillTurn;
        [JsonProperty("Общая настройка")]
        public GeneralSettingsSkill GeneralSettings;
    }

    internal class IQCraftSystemAdvancedCraft
    {
        [JsonProperty("Включить умение?(true - включено/false - выключено)")]
        public bool SkillTurn;
        [JsonProperty("Общая настройка")]
        public GeneralSettingsSkill GeneralSettings;
    }

    internal class IQKitsRareSkill
    {
        [JsonProperty("Включить умение?(true - включено/false - выключено)")]
        public bool SkillTurn;
        [JsonProperty("Общая настройка")]
        public GeneralSettingsSkill GeneralSettings;
        [JsonProperty("На сколько % увеличивать шанс выпадения предметов?")]
        public int RareUP;
    }

    internal class IQKitsCooldownPercenct
    {
        [JsonProperty("Включить умение?(true - включено/false - выключено)")]
        public bool SkillTurn;
        [JsonProperty("Общая настройка")]
        public GeneralSettingsSkill GeneralSettings;
        [JsonProperty("На сколько % уменьшать перезарядку набора?")]
        public int PercentDrop;
    }

}

internal class IQHeadReward
{
    [JsonProperty("Включить умение?(true - включено/false - выключено)")]
    public bool SkillTurn;
    [JsonProperty("Общая настройка")]
    public GeneralSettingsSkill GeneralSettings;
}

internal class IQCraftSystemAdvancedCraft
{
    [JsonProperty("Включить умение?(true - включено/false - выключено)")]
    public bool SkillTurn;
    [JsonProperty("Общая настройка")]
    public GeneralSettingsSkill GeneralSettings;
}

internal class IQKitsRareSkill
{
    [JsonProperty("Включить умение?(true - включено/false - выключено)")]
    public bool SkillTurn;
    [JsonProperty("Общая настройка")]
    public GeneralSettingsSkill GeneralSettings;
    [JsonProperty("На сколько % увеличивать шанс выпадения предметов?")]
    public int RareUP;
}

internal class IQKitsCooldownPercenct
{
    [JsonProperty("Включить умение?(true - включено/false - выключено)")]
    public bool SkillTurn;
    [JsonProperty("Общая настройка")]
    public GeneralSettingsSkill GeneralSettings;
    [JsonProperty("На сколько % уменьшать перезарядку набора?")]
    public int PercentDrop;
}

internal class Miner
{
    [JsonProperty("Включить умение?(true - включено/false - выключено)")]
    public bool SkillTurn;
    [JsonProperty("Общая настройка")]
    public GeneralSettingsSkill GeneralSettings;
    [JsonProperty("На сколько умножать рейты(все сразу)")]
    public float Rate;
    [JsonProperty("Использовать кастомные множители(true - да/false - нет)")]
    public bool UseLists;
    [JsonProperty("Использовать кастомные множители([Shortname] = множитель)")]
    public Dictionary<string, float> CustomRate;
}

internal class Regeneration
{
    [JsonProperty("Включить умение?(true - включено/false - выключено)")]
    public bool SkillTurn;
    [JsonProperty("Общая настройка")]
    public GeneralSettingsSkill GeneralSettings;
    [JsonProperty("Сколько жизней регенерировать в промежуток времени")]
    public int HealtRegeneration;
    [JsonProperty("Раз в сколько секунд регенерировать игрока")]
    public int RegenerationTimer;
}

internal class Military
{
    [JsonProperty("Включить умение?(true - включено/false - выключено)")]
    public bool SkillTurn;
    [JsonProperty("Общая настройка")]
    public GeneralSettingsSkill GeneralSettings;
    [JsonProperty("На сколько процентов снижать изнашивания оружий(0-100%)")]
    public int PercentNoBroken;
}

internal class ThickSkin
{
    [JsonProperty("Включить умение?(true - включено/false - выключено)")]
    public bool SkillTurn;
    [JsonProperty("Общая настройка")]
    public GeneralSettingsSkill GeneralSettings;
}

internal class WoundedShake
{
    [JsonProperty("Включить умение?(true - включено/false - выключено)")]
    public bool SkillTurn;
    [JsonProperty("Общая настройка")]
    public GeneralSettingsSkill GeneralSettings;
    [JsonProperty("Шанс встать в момент когда игрока положат")]
    public int Rare;
    [JsonProperty("Через сколько секунд поднимать игрока при падаение,если шанс успешен")]
    public int RareStartTime;
    [JsonProperty("После успешного срабатывания - забирать скилл(true - да/false - нет)")]
    public bool DropSkill;
}

internal class Metabolism
{
    [JsonProperty("Включить умение?(true - включено/false - выключено)")]
    public bool SkillTurn;
    [JsonProperty("Общая настройка")]
    public GeneralSettingsSkill GeneralSettings;
    [JsonProperty("Сколько ХП будет при возрождениие")]
    public int Health;
    [JsonProperty("Сколько Сытности будет при возрождении")]
    public int Calories;
    [JsonProperty("Сколько Жажды будет при возрождении")]
    public int Hydration;
    [JsonProperty("Шанс проснуться с данными показателями")]
    public int RareMetabolisme;
    [JsonProperty("После успешного срабатывания - забирать скилл(true - да/false - нет)")]
    public bool DropSkill;
}

internal class PatogenAmrory
{
    [JsonProperty("Включить умение?(true - включено/false - выключено)")]
    public bool SkillTurn;
    [JsonProperty("Общая настройка")]
    public GeneralSettingsSkill GeneralSettings;
}

internal class PatogenKill
{
    [JsonProperty("Включить умение?(true - включено/false - выключено)")]
    public bool SkillTurn;
    [JsonProperty("Общая настройка")]
    public GeneralSettingsSkill GeneralSettings;
}

internal class GenesisGens
{
    [JsonProperty("Включить умение?(true - включено/false - выключено)")]
    public bool SkillTurn;
    [JsonProperty("Общая настройка")]
    public GeneralSettingsSkill GeneralSettings;
    [JsonProperty("Сколько % ДНК оставлять в конце вайпа(будет отсчитываться от затраченного количества на скиллы)0 - 100")]
    public int PercentSave;
}

internal class AnimalFriends
{
    [JsonProperty("Включить умение?(true - включено/false - выключено)")]
    public bool SkillTurn;
    [JsonProperty("Общая настройка")]
    public GeneralSettingsSkill GeneralSettings;
    [JsonProperty("Min : Сколько выдавать ДНК")]
    public int MinDNKAnimal;
    [JsonProperty("Max : Сколько выдавать ДНК")]
    public int MaxDNKAnimal;
    [JsonProperty("Использовать кастомный лист(true - да/false - нет)")]
    public bool UseLists;
    [JsonProperty("Кастомный лист ,на сколько увеличивать количество ДНК за убийство животного [Animal] = Настройка")]
    public Dictionary<string, CustomSettings> AnimalsList;
    [JsonProperty("Шанс получения дополнительного ДНК")]
    public int RareAll;
    [JsonProperty("Животные с которых будет падать ДНК")]
    public List<string> AnimalDetected;
    internal class CustomSettings
    {
        [JsonProperty("Шанс получения дополнительного ДНК")]
        public int Rare;
        [JsonProperty("Min : Сколько выдавать ДНК")]
        public int MinDNKCustom;
        [JsonProperty("Max : Сколько выдавать ДНК")]
        public int MaxDNKCustom;
    }

}

internal class CustomSettings
{
    [JsonProperty("Шанс получения дополнительного ДНК")]
    public int Rare;
    [JsonProperty("Min : Сколько выдавать ДНК")]
    public int MinDNKCustom;
    [JsonProperty("Max : Сколько выдавать ДНК")]
    public int MaxDNKCustom;
}

internal class GatherFriends
{
    [JsonProperty("Включить умение?(true - включено/false - выключено)")]
    public bool SkillTurn;
    [JsonProperty("Общая настройка")]
    public GeneralSettingsSkill GeneralSettings;
    [JsonProperty("Кастомный лист ,на сколько увеличивать количество ДНК за полную добычу ресурса [Shortname] = Настройка")]
    public Dictionary<string, CustomSettings> GatherList;
    internal class CustomSettings
    {
        [JsonProperty("Шанс получения дополнительного ДНК")]
        public int Rare;
        [JsonProperty("Min : Сколько выдавать ДНК")]
        public int MinDNKCustom;
        [JsonProperty("Max : Сколько выдавать ДНК")]
        public int MaxDNKCustom;
    }

}

internal class CustomSettings
{
    [JsonProperty("Шанс получения дополнительного ДНК")]
    public int Rare;
    [JsonProperty("Min : Сколько выдавать ДНК")]
    public int MinDNKCustom;
    [JsonProperty("Max : Сколько выдавать ДНК")]
    public int MaxDNKCustom;
}

internal class Anabiotics
{
    [JsonProperty("Включить умение?(true - включено/false - выключено)")]
    public bool SkillTurn;
    [JsonProperty("Общая настройка")]
    public GeneralSettingsSkill GeneralSettings;
    [JsonProperty("На сколько увеличивать количество ХП за использование медицины[Shortname] = Amount")]
    public Dictionary<string, int> AnabioticsList;
}

internal class Crafter
{
    [JsonProperty("Включить умение?(true - включено/false - выключено)")]
    public bool SkillTurn;
    [JsonProperty("Общая настройка")]
    public GeneralSettingsSkill GeneralSettings;
    [JsonProperty("В сколько раз увеличивать скорость крафта? (Пример : изначально 60 секунд, увеличение в 3 раза = 60/3 = 20)")]
    public int CraftBoost;
}

internal class GeneralSettingsSkill
{
    [JsonProperty("Отображаемое имя")]
    public string DisplayName;
    [JsonProperty("Описание скилла")]
    public string Description;
    [JsonProperty("Sprite для иконки")]
    public string Sprite;
    [JsonProperty("PNG-ссылка для иконки(64х64).Если используете это,спрайт будет игнорироваться")]
    public string PNG;
    [JsonProperty("Цена за изучение")]
    public int PriceDNK;
}

internal class InterfaceSetting
{
    [JsonProperty("Настройка элементов дизайна")]
    public Icons IconsPNG;
    [JsonProperty("Настройка основных элементов")]
    public General GeneralSettings;
    internal class Icons
    {
        [JsonProperty("Ссылка PNG на задний фон")]
        public string BackgroundPNG;
        [JsonProperty("Ссылка ссылка на иконку заблокированного навыка PNG")]
        public string BlockSkill;
        [JsonProperty("Ссылка ссылка на иконку доступного навыка PNG")]
        public string AvailableSkill;
        [JsonProperty("Ссылка ссылка на иконку изученного навыка PNG")]
        public string ReceivedSkill;
        [JsonProperty("Ссылка на кнопку для получения данного скилла(развить)")]
        public string ButtonTakeSkill;
        [JsonProperty("Ссылка панель для изучения скилла")]
        public string BackgroundTakePanel;
        [JsonProperty("Ссылка ссылка на иконку НЕЙТРАЛЬНОГО заблокированного навыка PNG")]
        public string NeutralBlockSkill;
        [JsonProperty("Ссылка ссылка на иконку НЕЙТРАЛЬНОГО доступного навыка PNG")]
        public string NeutralAvailableSkill;
        [JsonProperty("Ссылка ссылка на иконку НЕЙТРАЛЬНОГО изученного навыка PNG")]
        public string NeutralReceivedSkill;
    }

    internal class General
    {
        [JsonProperty("Цвет текста")]
        public string HexLabels;
        [JsonProperty("Цвет текста навыков")]
        public string HexLabelsSkill;
        [JsonProperty("Цвет текста описания навыков")]
        public string HexLabelTakePanel;
    }

}

internal class Icons
{
    [JsonProperty("Ссылка PNG на задний фон")]
    public string BackgroundPNG;
    [JsonProperty("Ссылка ссылка на иконку заблокированного навыка PNG")]
    public string BlockSkill;
    [JsonProperty("Ссылка ссылка на иконку доступного навыка PNG")]
    public string AvailableSkill;
    [JsonProperty("Ссылка ссылка на иконку изученного навыка PNG")]
    public string ReceivedSkill;
    [JsonProperty("Ссылка на кнопку для получения данного скилла(развить)")]
    public string ButtonTakeSkill;
    [JsonProperty("Ссылка панель для изучения скилла")]
    public string BackgroundTakePanel;
    [JsonProperty("Ссылка ссылка на иконку НЕЙТРАЛЬНОГО заблокированного навыка PNG")]
    public string NeutralBlockSkill;
    [JsonProperty("Ссылка ссылка на иконку НЕЙТРАЛЬНОГО доступного навыка PNG")]
    public string NeutralAvailableSkill;
    [JsonProperty("Ссылка ссылка на иконку НЕЙТРАЛЬНОГО изученного навыка PNG")]
    public string NeutralReceivedSkill;
}

internal class General
{
    [JsonProperty("Цвет текста")]
    public string HexLabels;
    [JsonProperty("Цвет текста навыков")]
    public string HexLabelsSkill;
    [JsonProperty("Цвет текста описания навыков")]
    public string HexLabelTakePanel;
}

internal class GeneralSetting
{
    [JsonProperty("Настройки автоматической очисти даты после вайпа")]
    public WipeContoller WipeContollers;
    [JsonProperty("Управление вирусом ПАТОГЕН")]
    public VirusPatogen VirusPatogens;
    internal class WipeContoller
    {
        [JsonProperty("Включить автоматическую очистку даты после вайпа сервера")]
        public bool WipeDataUse;
        [JsonProperty("Очищать скиллы игроков после вайпа")]
        public bool WipeDataSkill;
    }

    internal class VirusPatogen
    {
        [JsonProperty("Включить вирус-патоген(Данный вирус будет удалять у игрока 1 случайный навык через N количество времени,у них так же будут навыки на излечение и защиты от заражаения)")]
        public bool UsePatogen;
        [JsonProperty("Шанс заражения вирусом")]
        public int RareInfected;
        [JsonProperty("Раз в сколько времени начинать инфекцию игроков(Секунды)")]
        public int TimerInfectedVirus;
        [JsonProperty("Через сколько удалять 1 случайный навык игроку(секунды)")]
        public int TimerRemoveSkill;
    }

}

internal class WipeContoller
{
    [JsonProperty("Включить автоматическую очистку даты после вайпа сервера")]
    public bool WipeDataUse;
    [JsonProperty("Очищать скиллы игроков после вайпа")]
    public bool WipeDataSkill;
}

internal class VirusPatogen
{
    [JsonProperty("Включить вирус-патоген(Данный вирус будет удалять у игрока 1 случайный навык через N количество времени,у них так же будут навыки на излечение и защиты от заражаения)")]
    public bool UsePatogen;
    [JsonProperty("Шанс заражения вирусом")]
    public int RareInfected;
    [JsonProperty("Раз в сколько времени начинать инфекцию игроков(Секунды)")]
    public int TimerInfectedVirus;
    [JsonProperty("Через сколько удалять 1 случайный навык игроку(секунды)")]
    public int TimerRemoveSkill;
}

internal class ReferenceSetting
{
    [JsonProperty("Настройка IQChat")]
    public IQChat IQChatSettings;
    [JsonProperty("Включить поддержку IQEconomic(ДНК заменится на валюту экономики)")]
    public bool IQEconomicUse;
    [JsonProperty("Включить поддержку IQHeadReward")]
    public bool IQHeadRewardUse;
    [JsonProperty("Включить поддержку IQCraftSystem")]
    public bool IQCraftSystem;
    [JsonProperty("Включить поддержку IQKits")]
    public bool IQKits;
    [JsonProperty("Настройка XDNotifications")]
    public XDNotifications XDNotificationsSettings;
    internal class XDNotifications
    {
        [JsonProperty("Включить поддержку XDNotifications(Некоторые уведомления будут приходить в XDNotifications)")]
        public bool UseXDNotifications;
        [JsonProperty("Цвет заднего фона уведомления(HEX)")]
        public string Color;
        [JsonProperty("Через сколько удалиться уведомление")]
        public int AlertDelete;
        [JsonProperty("Звуковой эффект")]
        public string SoundEffect;
        [JsonProperty("Оглавление")]
        public string Title;
    }

    internal class IQChat
    {
        [JsonProperty("IQChat : Кастомный префикс в чате")]
        public string CustomPrefix;
        [JsonProperty("IQChat : Кастомный аватар в чате(Если требуется)")]
        public string CustomAvatar;
    }

}

internal class XDNotifications
{
    [JsonProperty("Включить поддержку XDNotifications(Некоторые уведомления будут приходить в XDNotifications)")]
    public bool UseXDNotifications;
    [JsonProperty("Цвет заднего фона уведомления(HEX)")]
    public string Color;
    [JsonProperty("Через сколько удалиться уведомление")]
    public int AlertDelete;
    [JsonProperty("Звуковой эффект")]
    public string SoundEffect;
    [JsonProperty("Оглавление")]
    public string Title;
}

internal class IQChat
{
    [JsonProperty("IQChat : Кастомный префикс в чате")]
    public string CustomPrefix;
    [JsonProperty("IQChat : Кастомный аватар в чате(Если требуется)")]
    public string CustomAvatar;
}

internal class FarmingDNK
{
    [JsonProperty("Настройка получения ДНК за убийство игроков")]
    public PlayerKill PlayerKills;
    [JsonProperty("Настройка получения ДНК за убийство NPC")]
    public NPCKill NPCKills;
    [JsonProperty("Настройка получения ДНК за убийство животных")]
    public AnimalKill AnimalKills;
    internal class PlayerKill
    {
        [JsonProperty("Получение ДНК за убийство игроков")]
        public bool DNKKillUser;
        [JsonProperty("Параметры")]
        public GeneralSettingsFarming GeneralSettingsFarmings;
    }

    internal class NPCKill
    {
        [JsonProperty("Получение валюты за убийство NPC")]
        public bool DNKKillNPC;
        [JsonProperty("Параметры")]
        public GeneralSettingsFarming GeneralSettingsFarmings;
    }

    internal class AnimalKill
    {
        [JsonProperty("Получение валюты за убийство животных")]
        public bool DNKKillAnimal;
        [JsonProperty("Параметры")]
        public GeneralSettingsFarming GeneralSettingsFarmings;
    }

    internal class GeneralSettingsFarming
    {
        [JsonProperty("Шанс получения ДНК")]
        public int RareGiveDNK;
        [JsonProperty("Минимальное количество ДНК")]
        public int MinimumDNK;
        [JsonProperty("Максимальное количество ДНК")]
        public int MaximumDNK;
    }

}

internal class PlayerKill
{
    [JsonProperty("Получение ДНК за убийство игроков")]
    public bool DNKKillUser;
    [JsonProperty("Параметры")]
    public GeneralSettingsFarming GeneralSettingsFarmings;
}

internal class NPCKill
{
    [JsonProperty("Получение валюты за убийство NPC")]
    public bool DNKKillNPC;
    [JsonProperty("Параметры")]
    public GeneralSettingsFarming GeneralSettingsFarmings;
}

internal class AnimalKill
{
    [JsonProperty("Получение валюты за убийство животных")]
    public bool DNKKillAnimal;
    [JsonProperty("Параметры")]
    public GeneralSettingsFarming GeneralSettingsFarmings;
}

internal class GeneralSettingsFarming
{
    [JsonProperty("Шанс получения ДНК")]
    public int RareGiveDNK;
    [JsonProperty("Минимальное количество ДНК")]
    public int MinimumDNK;
    [JsonProperty("Максимальное количество ДНК")]
    public int MaximumDNK;
}


```

---

## IQRankSystem

```csharp
using Newtonsoft.Json;
using System.Collections.Generic;
using Oxide.Core.Plugins;
using System.Linq;
using System;
using ConVar;
using System.Text;
using Oxide.Core;

Oxide.Plugins
[Info("IQRankSystem", "SkuliDropek", "0.0.3")]
[Description("Ваши ранги для сенрвера")]
 class IQRankSystem : RustPlugin
{
    [PluginReference]
     Plugin IQChat;
     Plugin IQCases;
     Plugin IQHeadReward;
     Plugin IQEconomic;
    public void SendChat(BasePlayer player, string Message, Chat.ChatChannel channel);
     void OpenCase(BasePlayer player, string DisplayNameCase);
     void KilledTask(BasePlayer player);
     void SET_BALANCE_USER(ulong userID, int SetBalance);
     StringBuilder sb;
    private string GetLang(string LangKey, string userID, object[] args);
     void RegisteredPermissions();
    private static Configuration config;
    private class Configuration
    {
        [JsonProperty("Формат для отображения рангов в чате (В IQChat настраивается отдельно) [Не добавляйте значения в {} и не меняйте их порядок, если не уверены в своих возможностях или не знаете что это!!].Все что не внутри {} - можете смело вертеть")]
        public String FormatRanks;
        [JsonProperty("Список рангов и их настройка")]
        public Dictionary<string, RankSettings> RankList;
        [JsonProperty("Настройки плагина")]
        public Settings Setting;
        internal class RankSettings
        {
            [JsonProperty("Название ранга")]
            public String DisplayNameRank;
            [JsonProperty("Права с которыми доступен данный ранг")]
            public String PermissionRank;
            [JsonProperty("Настройки получения ранга")]
            public Obtainings Obtaining;
            internal class Obtainings
            {
                [JsonProperty("Выберите за что возможно получить доступ к данному рангу" +
                                  "(0 - Добыча, " +
                                  "1 - Время игры на сервере, " +
                                  "2 - Вермя игры на сервере и добыча вместе," +
                                  "3 - IQCases , открыть N количество кейсов," +
                                  "4 - IQHeadReward , убить N количество игроков в розыске" +
                                  "5 - IQEconomic, собрать за все время N количество валюты")]
                public Obtaining ObtainingType;
                [JsonProperty("Настройки получения ранга за время")]
                public ObtainingsTime ObtainingsTimes;
                [JsonProperty("Настройки получения ранга за добычу")]
                public ObtainingsGather ObtainingsGathers;
                [JsonProperty("Настройки получения ранга за открытие кейсов IQCases")]
                public ObtainingsIQCases ObtainingsIQCase;
                [JsonProperty("Настройки получения ранга за убийство разыскиваемых IQHeadReward")]
                public ObtainingsIQHeadReward ObtainingsIQHeadRewards;
                [JsonProperty("Настройки получения ранга за собранную валюту IQEconomic")]
                public ObtainingsIQEconomic ObtainingsIQEconomics;
                internal class ObtainingsIQEconomic
                {
                    [JsonProperty("Сколько собрать валюты для получения этого ранга")]
                    public Int32 IQEconomicBalance;
                }

                internal class ObtainingsIQCases
                {
                    [JsonProperty("Сколько кейсов открыть для получения этого ранга")]
                    public Int32 OpenCaseAmount;
                }

                internal class ObtainingsIQHeadReward
                {
                    [JsonProperty("Сколько нужно убить разыскиваемых для получения этого ранга")]
                    public Int32 KillHeadAmount;
                }

                internal class ObtainingsTime
                {
                    [JsonProperty("Время, которое нужно отыграть для получения этого ранга")]
                    public Int32 TimeGame;
                }

                internal class ObtainingsGather
                {
                    [JsonProperty("Сколько нужно добыть всего ресурсов для ранга")]
                    public Int32 Amount;
                    [JsonProperty("Использовать детальную добычу true - да(из списка, по критериям)/false - нет(учитывается на все ресурсы)")]
                    public Boolean UseDetalisGather;
                    [JsonProperty("Настройки детальной добычи : Shortname = Amount")]
                    public Dictionary<String, Int32> GatherDetalis;
                }

            }

        }

        internal class Settings
        {
            [JsonProperty("Настройки плагинов совместимости")]
            public ReferenceSettings ReferenceSetting;
            [JsonProperty("Общие настройки")]
            public GeneralSettings GeneralSetting;
            internal class GeneralSettings
            {
                [JsonProperty("Отображать время игры на сервере перед рангом")]
                public Boolean ShowTimeGame;
                [JsonProperty("При получении нового ранга сразу устанавливать его(true - да/false - нет)")]
                public Boolean RankSetupNew;
            }

            internal class ReferenceSettings
            {
                [JsonProperty("Настройки IQChat")]
                public IQChatSettings IQChatSetting;
                internal class IQChatSettings
                {
                    [JsonProperty("IQChat : Кастомный префикс в чате")]
                    public String CustomPrefix;
                    [JsonProperty("IQChat : Кастомный аватар в чате(Если требуется)")]
                    public String CustomAvatar;
                }

            }

        }

        public static Configuration GetNewConfiguration();
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    [JsonProperty("Информация о пользователях")]
    public Dictionary<UInt64, DataClass> DataInformation;
    public class DataClass
    {
        [JsonProperty("Активный ранг")]
        public String RankActive;
        [JsonProperty("Доступные ранги")]
        public List<String> RankAccessList;
        [JsonProperty("Информация о прогрессе игроков")]
        public Infromation InformationUser;
        internal class Infromation
        {
            [JsonProperty("Время на сервере")]
            public Int32 TimeGame;
            [JsonProperty("Добыто всего")]
            public Int32 GatherAll;
            [JsonProperty("Добыто всего : детально")]
            public Dictionary<String, Int32> GatherDetalis;
            [JsonProperty("IQCases : Открыто кейсов")]
            public Int32 IQCasesOpenCase;
            [JsonProperty("IQHeadReward : Убито разыскиваемых")]
            public Int32 IQHeadRewardKillAmount;
            [JsonProperty("IQEconomic : Собрано валюты")]
            public Int32 IQEconomicAmountBalance;
        }

    }

     void ReadData();
     void WriteData();
     void RegisteredDataUser(UInt64 userID);
    private void Init();
    private void OnServerInitialized();
     void OnPlayerConnected(BasePlayer player);
     object OnDispenserGather(ResourceDispenser dispenser, BaseEntity entity, Item item);
     void OnDispenserBonus(ResourceDispenser dispenser, BasePlayer player, Item item);
    private void Unload();
    private bool OnPlayerChat(BasePlayer player, string message, Chat.ChatChannel channel);
    private void SeparatorChat(Chat.ChatChannel channel, BasePlayer player, string Message);
    public void RankAccess(BasePlayer player);
     void SetupRank(BasePlayer player, string RankKey);
    public void TrackerTime();
     void RankSetUp(BasePlayer player, string RankName);
    public static string FormatTime(TimeSpan time);
    private static string Format(int units, string form1, string form2, string form3);
    [ChatCommand("rank")]
     void ChatRankCommand(BasePlayer player, string cmd, string[] arg);
    private new void LoadDefaultMessages();
     string API_GET_RANK_NAME(ulong userID);
     string API_GET_RANK_NAME(string Key);
     void API_ADD_RANK(UInt64 userID, String Key);
     bool API_IS_RANK_REALITY(string Key);
     string API_GET_RANK_PERM(string Key);
     bool API_GET_RANK_ACCESS(ulong userID, string Key);
     bool API_GET_AVAILABILITY_RANK_USER(ulong userID, string Key);
     string API_GET_TIME_GAME(ulong userID);
     int API_GET_SECONDGAME(ulong userID);
     List<string> API_RANK_USER_KEYS(ulong userID);
     void API_SET_ACTIVE_RANK(ulong userID, string RankKey);
}

private class Configuration
{
    [JsonProperty("Формат для отображения рангов в чате (В IQChat настраивается отдельно) [Не добавляйте значения в {} и не меняйте их порядок, если не уверены в своих возможностях или не знаете что это!!].Все что не внутри {} - можете смело вертеть")]
    public String FormatRanks;
    [JsonProperty("Список рангов и их настройка")]
    public Dictionary<string, RankSettings> RankList;
    [JsonProperty("Настройки плагина")]
    public Settings Setting;
    internal class RankSettings
    {
        [JsonProperty("Название ранга")]
        public String DisplayNameRank;
        [JsonProperty("Права с которыми доступен данный ранг")]
        public String PermissionRank;
        [JsonProperty("Настройки получения ранга")]
        public Obtainings Obtaining;
        internal class Obtainings
        {
            [JsonProperty("Выберите за что возможно получить доступ к данному рангу" +
                                  "(0 - Добыча, " +
                                  "1 - Время игры на сервере, " +
                                  "2 - Вермя игры на сервере и добыча вместе," +
                                  "3 - IQCases , открыть N количество кейсов," +
                                  "4 - IQHeadReward , убить N количество игроков в розыске" +
                                  "5 - IQEconomic, собрать за все время N количество валюты")]
            public Obtaining ObtainingType;
            [JsonProperty("Настройки получения ранга за время")]
            public ObtainingsTime ObtainingsTimes;
            [JsonProperty("Настройки получения ранга за добычу")]
            public ObtainingsGather ObtainingsGathers;
            [JsonProperty("Настройки получения ранга за открытие кейсов IQCases")]
            public ObtainingsIQCases ObtainingsIQCase;
            [JsonProperty("Настройки получения ранга за убийство разыскиваемых IQHeadReward")]
            public ObtainingsIQHeadReward ObtainingsIQHeadRewards;
            [JsonProperty("Настройки получения ранга за собранную валюту IQEconomic")]
            public ObtainingsIQEconomic ObtainingsIQEconomics;
            internal class ObtainingsIQEconomic
            {
                [JsonProperty("Сколько собрать валюты для получения этого ранга")]
                public Int32 IQEconomicBalance;
            }

            internal class ObtainingsIQCases
            {
                [JsonProperty("Сколько кейсов открыть для получения этого ранга")]
                public Int32 OpenCaseAmount;
            }

            internal class ObtainingsIQHeadReward
            {
                [JsonProperty("Сколько нужно убить разыскиваемых для получения этого ранга")]
                public Int32 KillHeadAmount;
            }

            internal class ObtainingsTime
            {
                [JsonProperty("Время, которое нужно отыграть для получения этого ранга")]
                public Int32 TimeGame;
            }

            internal class ObtainingsGather
            {
                [JsonProperty("Сколько нужно добыть всего ресурсов для ранга")]
                public Int32 Amount;
                [JsonProperty("Использовать детальную добычу true - да(из списка, по критериям)/false - нет(учитывается на все ресурсы)")]
                public Boolean UseDetalisGather;
                [JsonProperty("Настройки детальной добычи : Shortname = Amount")]
                public Dictionary<String, Int32> GatherDetalis;
            }

        }

    }

    internal class Settings
    {
        [JsonProperty("Настройки плагинов совместимости")]
        public ReferenceSettings ReferenceSetting;
        [JsonProperty("Общие настройки")]
        public GeneralSettings GeneralSetting;
        internal class GeneralSettings
        {
            [JsonProperty("Отображать время игры на сервере перед рангом")]
            public Boolean ShowTimeGame;
            [JsonProperty("При получении нового ранга сразу устанавливать его(true - да/false - нет)")]
            public Boolean RankSetupNew;
        }

        internal class ReferenceSettings
        {
            [JsonProperty("Настройки IQChat")]
            public IQChatSettings IQChatSetting;
            internal class IQChatSettings
            {
                [JsonProperty("IQChat : Кастомный префикс в чате")]
                public String CustomPrefix;
                [JsonProperty("IQChat : Кастомный аватар в чате(Если требуется)")]
                public String CustomAvatar;
            }

        }

    }

    public static Configuration GetNewConfiguration();
}

internal class RankSettings
{
    [JsonProperty("Название ранга")]
    public String DisplayNameRank;
    [JsonProperty("Права с которыми доступен данный ранг")]
    public String PermissionRank;
    [JsonProperty("Настройки получения ранга")]
    public Obtainings Obtaining;
    internal class Obtainings
    {
        [JsonProperty("Выберите за что возможно получить доступ к данному рангу" +
                                  "(0 - Добыча, " +
                                  "1 - Время игры на сервере, " +
                                  "2 - Вермя игры на сервере и добыча вместе," +
                                  "3 - IQCases , открыть N количество кейсов," +
                                  "4 - IQHeadReward , убить N количество игроков в розыске" +
                                  "5 - IQEconomic, собрать за все время N количество валюты")]
        public Obtaining ObtainingType;
        [JsonProperty("Настройки получения ранга за время")]
        public ObtainingsTime ObtainingsTimes;
        [JsonProperty("Настройки получения ранга за добычу")]
        public ObtainingsGather ObtainingsGathers;
        [JsonProperty("Настройки получения ранга за открытие кейсов IQCases")]
        public ObtainingsIQCases ObtainingsIQCase;
        [JsonProperty("Настройки получения ранга за убийство разыскиваемых IQHeadReward")]
        public ObtainingsIQHeadReward ObtainingsIQHeadRewards;
        [JsonProperty("Настройки получения ранга за собранную валюту IQEconomic")]
        public ObtainingsIQEconomic ObtainingsIQEconomics;
        internal class ObtainingsIQEconomic
        {
            [JsonProperty("Сколько собрать валюты для получения этого ранга")]
            public Int32 IQEconomicBalance;
        }

        internal class ObtainingsIQCases
        {
            [JsonProperty("Сколько кейсов открыть для получения этого ранга")]
            public Int32 OpenCaseAmount;
        }

        internal class ObtainingsIQHeadReward
        {
            [JsonProperty("Сколько нужно убить разыскиваемых для получения этого ранга")]
            public Int32 KillHeadAmount;
        }

        internal class ObtainingsTime
        {
            [JsonProperty("Время, которое нужно отыграть для получения этого ранга")]
            public Int32 TimeGame;
        }

        internal class ObtainingsGather
        {
            [JsonProperty("Сколько нужно добыть всего ресурсов для ранга")]
            public Int32 Amount;
            [JsonProperty("Использовать детальную добычу true - да(из списка, по критериям)/false - нет(учитывается на все ресурсы)")]
            public Boolean UseDetalisGather;
            [JsonProperty("Настройки детальной добычи : Shortname = Amount")]
            public Dictionary<String, Int32> GatherDetalis;
        }

    }

}

internal class Obtainings
{
    [JsonProperty("Выберите за что возможно получить доступ к данному рангу" +
                                  "(0 - Добыча, " +
                                  "1 - Время игры на сервере, " +
                                  "2 - Вермя игры на сервере и добыча вместе," +
                                  "3 - IQCases , открыть N количество кейсов," +
                                  "4 - IQHeadReward , убить N количество игроков в розыске" +
                                  "5 - IQEconomic, собрать за все время N количество валюты")]
    public Obtaining ObtainingType;
    [JsonProperty("Настройки получения ранга за время")]
    public ObtainingsTime ObtainingsTimes;
    [JsonProperty("Настройки получения ранга за добычу")]
    public ObtainingsGather ObtainingsGathers;
    [JsonProperty("Настройки получения ранга за открытие кейсов IQCases")]
    public ObtainingsIQCases ObtainingsIQCase;
    [JsonProperty("Настройки получения ранга за убийство разыскиваемых IQHeadReward")]
    public ObtainingsIQHeadReward ObtainingsIQHeadRewards;
    [JsonProperty("Настройки получения ранга за собранную валюту IQEconomic")]
    public ObtainingsIQEconomic ObtainingsIQEconomics;
    internal class ObtainingsIQEconomic
    {
        [JsonProperty("Сколько собрать валюты для получения этого ранга")]
        public Int32 IQEconomicBalance;
    }

    internal class ObtainingsIQCases
    {
        [JsonProperty("Сколько кейсов открыть для получения этого ранга")]
        public Int32 OpenCaseAmount;
    }

    internal class ObtainingsIQHeadReward
    {
        [JsonProperty("Сколько нужно убить разыскиваемых для получения этого ранга")]
        public Int32 KillHeadAmount;
    }

    internal class ObtainingsTime
    {
        [JsonProperty("Время, которое нужно отыграть для получения этого ранга")]
        public Int32 TimeGame;
    }

    internal class ObtainingsGather
    {
        [JsonProperty("Сколько нужно добыть всего ресурсов для ранга")]
        public Int32 Amount;
        [JsonProperty("Использовать детальную добычу true - да(из списка, по критериям)/false - нет(учитывается на все ресурсы)")]
        public Boolean UseDetalisGather;
        [JsonProperty("Настройки детальной добычи : Shortname = Amount")]
        public Dictionary<String, Int32> GatherDetalis;
    }

}

internal class ObtainingsIQEconomic
{
    [JsonProperty("Сколько собрать валюты для получения этого ранга")]
    public Int32 IQEconomicBalance;
}

internal class ObtainingsIQCases
{
    [JsonProperty("Сколько кейсов открыть для получения этого ранга")]
    public Int32 OpenCaseAmount;
}

internal class ObtainingsIQHeadReward
{
    [JsonProperty("Сколько нужно убить разыскиваемых для получения этого ранга")]
    public Int32 KillHeadAmount;
}

internal class ObtainingsTime
{
    [JsonProperty("Время, которое нужно отыграть для получения этого ранга")]
    public Int32 TimeGame;
}

internal class ObtainingsGather
{
    [JsonProperty("Сколько нужно добыть всего ресурсов для ранга")]
    public Int32 Amount;
    [JsonProperty("Использовать детальную добычу true - да(из списка, по критериям)/false - нет(учитывается на все ресурсы)")]
    public Boolean UseDetalisGather;
    [JsonProperty("Настройки детальной добычи : Shortname = Amount")]
    public Dictionary<String, Int32> GatherDetalis;
}

internal class Settings
{
    [JsonProperty("Настройки плагинов совместимости")]
    public ReferenceSettings ReferenceSetting;
    [JsonProperty("Общие настройки")]
    public GeneralSettings GeneralSetting;
    internal class GeneralSettings
    {
        [JsonProperty("Отображать время игры на сервере перед рангом")]
        public Boolean ShowTimeGame;
        [JsonProperty("При получении нового ранга сразу устанавливать его(true - да/false - нет)")]
        public Boolean RankSetupNew;
    }

    internal class ReferenceSettings
    {
        [JsonProperty("Настройки IQChat")]
        public IQChatSettings IQChatSetting;
        internal class IQChatSettings
        {
            [JsonProperty("IQChat : Кастомный префикс в чате")]
            public String CustomPrefix;
            [JsonProperty("IQChat : Кастомный аватар в чате(Если требуется)")]
            public String CustomAvatar;
        }

    }

}

internal class GeneralSettings
{
    [JsonProperty("Отображать время игры на сервере перед рангом")]
    public Boolean ShowTimeGame;
    [JsonProperty("При получении нового ранга сразу устанавливать его(true - да/false - нет)")]
    public Boolean RankSetupNew;
}

internal class ReferenceSettings
{
    [JsonProperty("Настройки IQChat")]
    public IQChatSettings IQChatSetting;
    internal class IQChatSettings
    {
        [JsonProperty("IQChat : Кастомный префикс в чате")]
        public String CustomPrefix;
        [JsonProperty("IQChat : Кастомный аватар в чате(Если требуется)")]
        public String CustomAvatar;
    }

}

internal class IQChatSettings
{
    [JsonProperty("IQChat : Кастомный префикс в чате")]
    public String CustomPrefix;
    [JsonProperty("IQChat : Кастомный аватар в чате(Если требуется)")]
    public String CustomAvatar;
}

public class DataClass
{
    [JsonProperty("Активный ранг")]
    public String RankActive;
    [JsonProperty("Доступные ранги")]
    public List<String> RankAccessList;
    [JsonProperty("Информация о прогрессе игроков")]
    public Infromation InformationUser;
    internal class Infromation
    {
        [JsonProperty("Время на сервере")]
        public Int32 TimeGame;
        [JsonProperty("Добыто всего")]
        public Int32 GatherAll;
        [JsonProperty("Добыто всего : детально")]
        public Dictionary<String, Int32> GatherDetalis;
        [JsonProperty("IQCases : Открыто кейсов")]
        public Int32 IQCasesOpenCase;
        [JsonProperty("IQHeadReward : Убито разыскиваемых")]
        public Int32 IQHeadRewardKillAmount;
        [JsonProperty("IQEconomic : Собрано валюты")]
        public Int32 IQEconomicAmountBalance;
    }

}

internal class Infromation
{
    [JsonProperty("Время на сервере")]
    public Int32 TimeGame;
    [JsonProperty("Добыто всего")]
    public Int32 GatherAll;
    [JsonProperty("Добыто всего : детально")]
    public Dictionary<String, Int32> GatherDetalis;
    [JsonProperty("IQCases : Открыто кейсов")]
    public Int32 IQCasesOpenCase;
    [JsonProperty("IQHeadReward : Убито разыскиваемых")]
    public Int32 IQHeadRewardKillAmount;
    [JsonProperty("IQEconomic : Собрано валюты")]
    public Int32 IQEconomicAmountBalance;
}


```

---

## IQRates

```csharp
using Newtonsoft.Json;
using Oxide.Core.Plugins;
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using UnityEngine;

Oxide.Plugins
[Info("IQRates", "SkuliDropek", "1.3.67")]
[Description("Настройка рейтинга на сервере")]
 class IQRates : RustPlugin
{
    [PluginReference]
     Plugin IQChat;
    public void SendChat(String Message, BasePlayer player, ConVar.Chat.ChatChannel channel);
    private const Boolean LanguageEn;
    private MonumentInfo SpacePort;
    public List<UInt64> LootersListCrateID;
    public static IQRates _;
    private static Configuration config;
    private class Configuration
    {
        [JsonProperty(LanguageEn ? "Plugin setup" : "Настройка плагина")]
        public PluginSettings pluginSettings;
        internal class PluginSettings
        {
            [JsonProperty(LanguageEn ? "Rating settings" : "Настройка рейтингов")]
            public Rates RateSetting;
            [JsonProperty(LanguageEn ? "Additional plugin settings" : "Дополнительная настройка плагина")]
            public OtherSettings OtherSetting;
            [JsonProperty(LanguageEn ? "Configuring supported plugins" : "Настройка поддерживаемых плагинов")]
            public ReferencePlugin ReferenceSettings;
            internal class ReferencePlugin
            {
                [JsonProperty(LanguageEn ? "Setting up IQChat" : "Настройка IQChat")]
                public IQChatReference IQChatSetting;
                internal class IQChatReference
                {
                    [JsonProperty(LanguageEn ? "IQChat : Custom prefix in chat" : "IQChat : Кастомный префикс в чате")]
                    public String CustomPrefix;
                    [JsonProperty(LanguageEn ? "IQChat : Custom chat avatar (If required)" : "IQChat : Кастомный аватар в чате(Если требуется)")]
                    public String CustomAvatar;
                    [JsonProperty(LanguageEn ? "IQChat : Use UI Notifications" : "IQChat : Использовать UI уведомления")]
                    public Boolean UIAlertUse;
                }

            }

            internal class Rates
            {
                [JsonProperty(LanguageEn ? "Ranking setting during the day" : "Настройка рейтинга днем")]
                public AllRates DayRates;
                [JsonProperty(LanguageEn ? "Setting the rating at night" : "Настройка рейтинга ночью")]
                public AllRates NightRates;
                [JsonProperty(LanguageEn ? "Setting privileges and ratings specifically for them [iqrates.vip] = { Setting } (Descending)" : "Настройка привилегий и рейтингов конкретно для них [iqrates.vip] = { Настройка } (По убыванию)")]
                public Dictionary<String, DayAnNightRate> PrivilegyRates;
                [JsonProperty(LanguageEn ? "Setting custom rates (items) by permission - setting (Descending)" : "Настройка кастомных рейтов(предметов) по пермишенсу - настройка (По убыванию)")]
                public PermissionsRate CustomRatesPermissions;
                [JsonProperty(LanguageEn ? "Black list of items that will not be categorically affected by the rating" : "Черный лист предметов,на которые катигорично не будут действовать рейтинг")]
                public List<String> BlackList;
                [JsonProperty(LanguageEn ? "Enable melting speed in furnaces (true - yes/false - no)" : "Включить скорость плавки в печах(true - да/false - нет)")]
                public Boolean UseSpeedBurnable;
                [JsonProperty(LanguageEn ? "A black list of items for the stove, which will not be categorically affected by melting" : "Черный лист предметов для печки,на которые катигорично не будут действовать плавка")]
                public List<String> BlackListBurnable;
                [JsonProperty(LanguageEn ? "Furnace smelting speed (If the list is enabled, this value will be the default for everyone who does not have rights)" : "Скорость плавки печей(Если включен список - это значение будет стандартное для всех у кого нет прав)")]
                public Single SpeedBurnable;
                [JsonProperty(LanguageEn ? "Smelting Fuel Usage Rating (If the list is enabled, this value will be the default value for all non-licensed)" : "Рейтинг использования топлива при переплавки(Если включен список - это значение будет стандартное для всех у кого нет прав)")]
                public Int32 SpeedFuelBurnable;
                [JsonProperty(LanguageEn ? "Enable list of melting speed in furnaces (true - yes/false - no)" : "Включить список скорости плавки в печах(true - да/false - нет)")]
                public Boolean UseSpeedBurnableList;
                [JsonProperty(LanguageEn ? "Setting the melting speed in furnaces by privileges" : "Настройка скорости плавки в печах по привилегиям")]
                public List<SpeedBurnablePreset> SpeedBurableList;
                internal class DayAnNightRate
                {
                    [JsonProperty(LanguageEn ? "Ranking setting during the day" : "Настройка рейтинга днем")]
                    public AllRates DayRates;
                    [JsonProperty(LanguageEn ? "Setting the rating at night" : "Настройка рейтинга ночью")]
                    public AllRates NightRates;
                }

                internal class SpeedBurnablePreset
                {
                    [JsonProperty(LanguageEn ? "Permissions" : "Права")]
                    public String Permissions;
                    [JsonProperty(LanguageEn ? "Furnace melting speed" : "Скорость плавки печей")]
                    public Single SpeedBurnable;
                    [JsonProperty(LanguageEn ? "Smelting Fuel Use Rating" : "Рейтинг использования топлива при переплавки")]
                    public Int32 SpeedFuelBurnable;
                }

                internal class PermissionsRate
                {
                    [JsonProperty(LanguageEn ? "Ranking setting during the day" : "Настройка рейтинга днем")]
                    public Dictionary<String, List<PermissionsRateDetalis>> DayRates;
                    [JsonProperty(LanguageEn ? "Setting the rating at night" : "Настройка рейтинга ночью")]
                    public Dictionary<String, List<PermissionsRateDetalis>> NightRates;
                    public class PermissionsRateDetalis
                    {
                        [JsonProperty(LanguageEn ? "Shortname" : "Shortname")]
                        public String Shortname;
                        [JsonProperty(LanguageEn ? "Rate" : "Рейтинг")]
                        public Single Rate;
                    }

                }

                internal class AllRates
                {
                    [JsonProperty(LanguageEn ? "Rating of extracted resources" : "Рейтинг добываемых ресурсов")]
                    public Single GatherRate;
                    [JsonProperty(LanguageEn ? "Rating of found items" : "Рейтинг найденных предметов")]
                    public Single LootRate;
                    [JsonProperty(LanguageEn ? "Pickup Rating" : "Рейтинг поднимаемых предметов")]
                    public Single PickUpRate;
                    [JsonProperty(LanguageEn ? "Rating of plants raised from the beds" : "Рейтинг поднимаемых растений с грядок")]
                    public Single GrowableRate;
                    [JsonProperty(LanguageEn ? "Quarry rating" : "Рейтинг карьеров")]
                    public Single QuarryRate;
                    [JsonProperty(LanguageEn ? "Excavator Rating" : "Рейтинг экскаватора")]
                    public Single ExcavatorRate;
                    [JsonProperty(LanguageEn ? "Coal drop chance" : "Шанс выпадения угля")]
                    public Single CoalRare;
                }

            }

            internal class OtherSettings
            {
                [JsonProperty(LanguageEn ? "Event settings on the server" : "Настройки ивентов на сервере")]
                public EventSettings EventSetting;
                [JsonProperty(LanguageEn ? "Fuel settings when buying vehicles from NPCs" : "Настройки топлива при покупке транспорта у NPC")]
                public FuelSettings FuelSetting;
                internal class FuelSettings
                {
                    [JsonProperty(LanguageEn ? "Amount of fuel for boats" : "Кол-во топлива у лодок")]
                    public Int32 AmountBoat;
                    [JsonProperty(LanguageEn ? "The amount of fuel in submarines" : "Кол-во топлива у подводных лодок")]
                    public Int32 AmountSubmarine;
                    [JsonProperty(LanguageEn ? "Minicopter fuel quantity" : "Кол-во топлива у миникоптера")]
                    public Int32 AmountMinicopter;
                    [JsonProperty(LanguageEn ? "Helicopter fuel quantity" : "Кол-во топлива у вертолета")]
                    public Int32 AmountScrapTransport;
                }

                [JsonProperty(LanguageEn ? "Use Time Acceleration" : "Использовать ускорение времени")]
                public Boolean UseTime;
                [JsonProperty(LanguageEn ? "Use time freeze (the time will be the one you set in the item &lt;Frozen time on the server&gt;)" : "Использовать заморозку времени(время будет такое, какое вы установите в пунке <Замороженное время на сервере>)")]
                public Boolean UseFreezeTime;
                [JsonProperty(LanguageEn ? "Frozen time on the server (Set time that will not change and be forever on the server, must be true on &lt;Use time freeze&gt;" : "Замороженное время на сервере (Установите время, которое не будет изменяться и будет вечно на сервере, должен быть true на <Использовать заморозку времени>")]
                public Int32 FreezeTime;
                [JsonProperty(LanguageEn ? "What time will the day start?" : "Укажите во сколько будет начинаться день")]
                public Int32 DayStart;
                [JsonProperty(LanguageEn ? "What time will the night start?" : "Укажите во сколько будет начинаться ночь")]
                public Int32 NightStart;
                [JsonProperty(LanguageEn ? "Specify how long the day will be in minutes" : "Укажите сколько будет длится день в минутах")]
                public Int32 DayTime;
                [JsonProperty(LanguageEn ? "Specify how long the night will last in minutes" : "Укажите сколько будет длится ночь в минутах")]
                public Int32 NightTime;
                [JsonProperty(LanguageEn ? "Use notification of players about the change of day and night (switching rates. The message is configured in the lang)" : "Использовать уведомление игроков о смене дня и ночи (переключение рейтов. Сообщение настраивается в лэнге)")]
                public Boolean UseAlertDayNight;
                [JsonProperty(LanguageEn ? "Enable the ability to completely skip the time of day (selected in the paragraph below)" : "Включить возможность полного пропуска времени суток(выбирается в пункте ниже)")]
                public Boolean UseSkipTime;
                [JsonProperty(LanguageEn ? "Select the type of time-of-day skip (0 - Skip day, 1 - Skip night)" : "Выберите тип пропуска времени суток (0 - Пропускать день, 1 - Пропускать ночь)(Не забудьте включить возможность полного пропуска времени суток)")]
                public SkipType TypeSkipped;
                internal class EventSettings
                {
                    [JsonProperty(LanguageEn ? "Helicopter spawn custom settings" : "Кастомные настройки спавна вертолета")]
                    public Setting HelicopterSetting;
                    [JsonProperty(LanguageEn ? "Custom tank spawn settings" : "Кастомные настройки спавна танка")]
                    public Setting BreadlaySetting;
                    [JsonProperty(LanguageEn ? "Custom ship spawn settings" : "Кастомные настройки спавна корабля")]
                    public Setting CargoShipSetting;
                    [JsonProperty(LanguageEn ? "Airdrop spawn custom settings" : "Кастомные настройки спавна аирдропа")]
                    public Setting CargoPlaneSetting;
                    [JsonProperty(LanguageEn ? "Chinook custom spawn settings" : "Кастомные настройки спавна чинука")]
                    public Setting ChinoockSetting;
                    internal class Setting
                    {
                        [JsonProperty(LanguageEn ? "Completely disable event spawning on the server (true - yes/false - no)" : "Полностью отключить спавн ивента на сервере(true - да/false - нет)")]
                        public Boolean FullOff;
                        [JsonProperty(LanguageEn ? "Enable custom spawn event (true - yes/false - no)" : "Включить кастомный спавн ивент(true - да/false - нет)")]
                        public Boolean UseEventCustom;
                        [JsonProperty(LanguageEn ? "Static event spawn time" : "Статическое время спавна ивента")]
                        public Int32 EventSpawnTime;
                        [JsonProperty(LanguageEn ? "Random spawn time settings" : "Настройки случайного времени спавна")]
                        public RandomingTime RandomTimeSpawn;
                        internal class RandomingTime
                        {
                            [JsonProperty(LanguageEn ? "Use random event spawn time (static time will not be taken into account) (true - yes/false - no)" : "Использовать случайное время спавно ивента(статическое время не будет учитываться)(true - да/false - нет)")]
                            public Boolean UseRandomTime;
                            [JsonProperty(LanguageEn ? "Minimum event spawn value" : "Минимальное значение спавна ивента")]
                            public Int32 MinEventSpawnTime;
                            [JsonProperty(LanguageEn ? "Max event spawn value" : "Максимальное значении спавна ивента")]
                            public Int32 MaxEventSpawnTime;
                        }

                    }

                }

            }

        }

        public static Configuration GetNewConfiguration();
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    public void Register(string Permissions);
    private const string prefabCH47;
    private const string prefabPlane;
    private const string prefabShip;
    private const string prefabPatrol;
    private Int32 GetRandomTime(Int32 Min, Int32 Max);
     void StartEvent();
    private void StartCargoShip(Configuration.PluginSettings.OtherSettings.EventSettings EventSettings);
    private void StartCargoPlane(Configuration.PluginSettings.OtherSettings.EventSettings EventSettings);
    private void StartBreadley(Configuration.PluginSettings.OtherSettings.EventSettings EventSettings);
    private void StartChinoock(Configuration.PluginSettings.OtherSettings.EventSettings EventSettings);
    private void StartHelicopter(Configuration.PluginSettings.OtherSettings.EventSettings EventSettings);
    private void UnSubProSub(int time);
     void SpawnCH47();
     void SpawnCargo();
     void SpawnHeli();
     void SpawnPlane();
    private void SpawnTank();
     int Converted(Types RateType, string Shortname, float Amount, BasePlayer player);
     float GetRareCoal(BasePlayer player);
    private void FuelSystemRating(EntityFuelSystem FuelSystem, Int32 Amount);
     bool IsBlackList(string Shortname);
     bool IsBlackListBurnable(string Shortname);
     bool IsTime();
     bool IsPermission(string userID, string Permission);
    [ChatCommand("rates")]
    private void GetInfoMyRates(BasePlayer player);
     object OnDispenserGather(ResourceDispenser dispenser, BaseEntity entity, Item item);
     void OnDispenserBonus(ResourceDispenser disp, BasePlayer player, Item item);
     void OnCollectiblePickup(Item item, BasePlayer player);
     void OnGrowableGathered(GrowableEntity plant, Item item, BasePlayer player);
     void OnContainerDropItems(ItemContainer container);
     void OnLootEntity(BasePlayer player, BaseEntity entity);
     void OnEntityKill(BaseNetworkable entity);
     void OnQuarryGather(MiningQuarry quarry, Item item);
    private BasePlayer ExcavatorPlayer;
     void OnExcavatorResourceSet(ExcavatorArm arm, string resourceName, BasePlayer player);
    private object OnExcavatorGather(ExcavatorArm arm, Item item);
     void OnFuelConsume(BaseOven oven, Item fuel, ItemModBurnable burnable);
     TOD_Time timeComponent;
     Boolean activatedDay;
    private void GetTimeComponent();
     void SetTimeComponent();
     void OnHour();
     void OnSunrise();
     void OnSunset();
     void StartupFreeze();
    private void OnServerInitialized();
    public Single GetMultiplaceBurnableSpeed(String ownerid);
    public Int32 GetMultiplaceBurnableFuelSpeed(String ownerid);
    private object OnOvenToggle(BaseOven oven, BasePlayer player);
    private class OvenController : FacepunchBehaviour
    {
        private static readonly Dictionary<BaseOven, OvenController> Controllers;
        private BaseOven _oven;
        private float _speed;
        private Int32 _ticks;
        private string _ownerId;
        private Int32 _speedFuel;
        private bool IsFurnace { get; set; }
        private void Awake();
        public object Switch(BasePlayer player);
        public void TryRestart();
        private void Kill();
        public static OvenController GetOrAdd(BaseOven oven);
        public static void TryRestartAll();
        public static void KillAll();
        private void StartCooking();
        private void StopCooking();
        public void Cook();
        private void SmeltItems();
    }

    private void Unload();
    private void OnEntitySpawned(BaseBoat boat);
    private void OnEntitySpawned(BaseSubmarine submarine);
    private void OnEntitySpawned(MiniCopter copter);
    private void OnEntitySpawned(ScrapTransportHelicopter helicopter);
    private void OnEntitySpawned(SupplySignal entity);
    private void OnEntitySpawned(CargoPlane entity);
    private void OnEntitySpawned(CargoShip entity);
    private void OnEntitySpawned(BradleyAPC entity);
    private void OnEntitySpawned(BaseHelicopter entity);
    private void OnEntitySpawned(CH47Helicopter entity);
    private static StringBuilder sb;
    public string GetLang(string LangKey, string userID, object[] args);
    private new void LoadDefaultMessages();
     int API_CONVERT(Types RateType, string Shortname, float Amount, BasePlayer player);
     int API_CONVERT_GATHER(string Shortname, float Amount, BasePlayer player);
}

private class Configuration
{
    [JsonProperty(LanguageEn ? "Plugin setup" : "Настройка плагина")]
    public PluginSettings pluginSettings;
    internal class PluginSettings
    {
        [JsonProperty(LanguageEn ? "Rating settings" : "Настройка рейтингов")]
        public Rates RateSetting;
        [JsonProperty(LanguageEn ? "Additional plugin settings" : "Дополнительная настройка плагина")]
        public OtherSettings OtherSetting;
        [JsonProperty(LanguageEn ? "Configuring supported plugins" : "Настройка поддерживаемых плагинов")]
        public ReferencePlugin ReferenceSettings;
        internal class ReferencePlugin
        {
            [JsonProperty(LanguageEn ? "Setting up IQChat" : "Настройка IQChat")]
            public IQChatReference IQChatSetting;
            internal class IQChatReference
            {
                [JsonProperty(LanguageEn ? "IQChat : Custom prefix in chat" : "IQChat : Кастомный префикс в чате")]
                public String CustomPrefix;
                [JsonProperty(LanguageEn ? "IQChat : Custom chat avatar (If required)" : "IQChat : Кастомный аватар в чате(Если требуется)")]
                public String CustomAvatar;
                [JsonProperty(LanguageEn ? "IQChat : Use UI Notifications" : "IQChat : Использовать UI уведомления")]
                public Boolean UIAlertUse;
            }

        }

        internal class Rates
        {
            [JsonProperty(LanguageEn ? "Ranking setting during the day" : "Настройка рейтинга днем")]
            public AllRates DayRates;
            [JsonProperty(LanguageEn ? "Setting the rating at night" : "Настройка рейтинга ночью")]
            public AllRates NightRates;
            [JsonProperty(LanguageEn ? "Setting privileges and ratings specifically for them [iqrates.vip] = { Setting } (Descending)" : "Настройка привилегий и рейтингов конкретно для них [iqrates.vip] = { Настройка } (По убыванию)")]
            public Dictionary<String, DayAnNightRate> PrivilegyRates;
            [JsonProperty(LanguageEn ? "Setting custom rates (items) by permission - setting (Descending)" : "Настройка кастомных рейтов(предметов) по пермишенсу - настройка (По убыванию)")]
            public PermissionsRate CustomRatesPermissions;
            [JsonProperty(LanguageEn ? "Black list of items that will not be categorically affected by the rating" : "Черный лист предметов,на которые катигорично не будут действовать рейтинг")]
            public List<String> BlackList;
            [JsonProperty(LanguageEn ? "Enable melting speed in furnaces (true - yes/false - no)" : "Включить скорость плавки в печах(true - да/false - нет)")]
            public Boolean UseSpeedBurnable;
            [JsonProperty(LanguageEn ? "A black list of items for the stove, which will not be categorically affected by melting" : "Черный лист предметов для печки,на которые катигорично не будут действовать плавка")]
            public List<String> BlackListBurnable;
            [JsonProperty(LanguageEn ? "Furnace smelting speed (If the list is enabled, this value will be the default for everyone who does not have rights)" : "Скорость плавки печей(Если включен список - это значение будет стандартное для всех у кого нет прав)")]
            public Single SpeedBurnable;
            [JsonProperty(LanguageEn ? "Smelting Fuel Usage Rating (If the list is enabled, this value will be the default value for all non-licensed)" : "Рейтинг использования топлива при переплавки(Если включен список - это значение будет стандартное для всех у кого нет прав)")]
            public Int32 SpeedFuelBurnable;
            [JsonProperty(LanguageEn ? "Enable list of melting speed in furnaces (true - yes/false - no)" : "Включить список скорости плавки в печах(true - да/false - нет)")]
            public Boolean UseSpeedBurnableList;
            [JsonProperty(LanguageEn ? "Setting the melting speed in furnaces by privileges" : "Настройка скорости плавки в печах по привилегиям")]
            public List<SpeedBurnablePreset> SpeedBurableList;
            internal class DayAnNightRate
            {
                [JsonProperty(LanguageEn ? "Ranking setting during the day" : "Настройка рейтинга днем")]
                public AllRates DayRates;
                [JsonProperty(LanguageEn ? "Setting the rating at night" : "Настройка рейтинга ночью")]
                public AllRates NightRates;
            }

            internal class SpeedBurnablePreset
            {
                [JsonProperty(LanguageEn ? "Permissions" : "Права")]
                public String Permissions;
                [JsonProperty(LanguageEn ? "Furnace melting speed" : "Скорость плавки печей")]
                public Single SpeedBurnable;
                [JsonProperty(LanguageEn ? "Smelting Fuel Use Rating" : "Рейтинг использования топлива при переплавки")]
                public Int32 SpeedFuelBurnable;
            }

            internal class PermissionsRate
            {
                [JsonProperty(LanguageEn ? "Ranking setting during the day" : "Настройка рейтинга днем")]
                public Dictionary<String, List<PermissionsRateDetalis>> DayRates;
                [JsonProperty(LanguageEn ? "Setting the rating at night" : "Настройка рейтинга ночью")]
                public Dictionary<String, List<PermissionsRateDetalis>> NightRates;
                public class PermissionsRateDetalis
                {
                    [JsonProperty(LanguageEn ? "Shortname" : "Shortname")]
                    public String Shortname;
                    [JsonProperty(LanguageEn ? "Rate" : "Рейтинг")]
                    public Single Rate;
                }

            }

            internal class AllRates
            {
                [JsonProperty(LanguageEn ? "Rating of extracted resources" : "Рейтинг добываемых ресурсов")]
                public Single GatherRate;
                [JsonProperty(LanguageEn ? "Rating of found items" : "Рейтинг найденных предметов")]
                public Single LootRate;
                [JsonProperty(LanguageEn ? "Pickup Rating" : "Рейтинг поднимаемых предметов")]
                public Single PickUpRate;
                [JsonProperty(LanguageEn ? "Rating of plants raised from the beds" : "Рейтинг поднимаемых растений с грядок")]
                public Single GrowableRate;
                [JsonProperty(LanguageEn ? "Quarry rating" : "Рейтинг карьеров")]
                public Single QuarryRate;
                [JsonProperty(LanguageEn ? "Excavator Rating" : "Рейтинг экскаватора")]
                public Single ExcavatorRate;
                [JsonProperty(LanguageEn ? "Coal drop chance" : "Шанс выпадения угля")]
                public Single CoalRare;
            }

        }

        internal class OtherSettings
        {
            [JsonProperty(LanguageEn ? "Event settings on the server" : "Настройки ивентов на сервере")]
            public EventSettings EventSetting;
            [JsonProperty(LanguageEn ? "Fuel settings when buying vehicles from NPCs" : "Настройки топлива при покупке транспорта у NPC")]
            public FuelSettings FuelSetting;
            internal class FuelSettings
            {
                [JsonProperty(LanguageEn ? "Amount of fuel for boats" : "Кол-во топлива у лодок")]
                public Int32 AmountBoat;
                [JsonProperty(LanguageEn ? "The amount of fuel in submarines" : "Кол-во топлива у подводных лодок")]
                public Int32 AmountSubmarine;
                [JsonProperty(LanguageEn ? "Minicopter fuel quantity" : "Кол-во топлива у миникоптера")]
                public Int32 AmountMinicopter;
                [JsonProperty(LanguageEn ? "Helicopter fuel quantity" : "Кол-во топлива у вертолета")]
                public Int32 AmountScrapTransport;
            }

            [JsonProperty(LanguageEn ? "Use Time Acceleration" : "Использовать ускорение времени")]
            public Boolean UseTime;
            [JsonProperty(LanguageEn ? "Use time freeze (the time will be the one you set in the item &lt;Frozen time on the server&gt;)" : "Использовать заморозку времени(время будет такое, какое вы установите в пунке <Замороженное время на сервере>)")]
            public Boolean UseFreezeTime;
            [JsonProperty(LanguageEn ? "Frozen time on the server (Set time that will not change and be forever on the server, must be true on &lt;Use time freeze&gt;" : "Замороженное время на сервере (Установите время, которое не будет изменяться и будет вечно на сервере, должен быть true на <Использовать заморозку времени>")]
            public Int32 FreezeTime;
            [JsonProperty(LanguageEn ? "What time will the day start?" : "Укажите во сколько будет начинаться день")]
            public Int32 DayStart;
            [JsonProperty(LanguageEn ? "What time will the night start?" : "Укажите во сколько будет начинаться ночь")]
            public Int32 NightStart;
            [JsonProperty(LanguageEn ? "Specify how long the day will be in minutes" : "Укажите сколько будет длится день в минутах")]
            public Int32 DayTime;
            [JsonProperty(LanguageEn ? "Specify how long the night will last in minutes" : "Укажите сколько будет длится ночь в минутах")]
            public Int32 NightTime;
            [JsonProperty(LanguageEn ? "Use notification of players about the change of day and night (switching rates. The message is configured in the lang)" : "Использовать уведомление игроков о смене дня и ночи (переключение рейтов. Сообщение настраивается в лэнге)")]
            public Boolean UseAlertDayNight;
            [JsonProperty(LanguageEn ? "Enable the ability to completely skip the time of day (selected in the paragraph below)" : "Включить возможность полного пропуска времени суток(выбирается в пункте ниже)")]
            public Boolean UseSkipTime;
            [JsonProperty(LanguageEn ? "Select the type of time-of-day skip (0 - Skip day, 1 - Skip night)" : "Выберите тип пропуска времени суток (0 - Пропускать день, 1 - Пропускать ночь)(Не забудьте включить возможность полного пропуска времени суток)")]
            public SkipType TypeSkipped;
            internal class EventSettings
            {
                [JsonProperty(LanguageEn ? "Helicopter spawn custom settings" : "Кастомные настройки спавна вертолета")]
                public Setting HelicopterSetting;
                [JsonProperty(LanguageEn ? "Custom tank spawn settings" : "Кастомные настройки спавна танка")]
                public Setting BreadlaySetting;
                [JsonProperty(LanguageEn ? "Custom ship spawn settings" : "Кастомные настройки спавна корабля")]
                public Setting CargoShipSetting;
                [JsonProperty(LanguageEn ? "Airdrop spawn custom settings" : "Кастомные настройки спавна аирдропа")]
                public Setting CargoPlaneSetting;
                [JsonProperty(LanguageEn ? "Chinook custom spawn settings" : "Кастомные настройки спавна чинука")]
                public Setting ChinoockSetting;
                internal class Setting
                {
                    [JsonProperty(LanguageEn ? "Completely disable event spawning on the server (true - yes/false - no)" : "Полностью отключить спавн ивента на сервере(true - да/false - нет)")]
                    public Boolean FullOff;
                    [JsonProperty(LanguageEn ? "Enable custom spawn event (true - yes/false - no)" : "Включить кастомный спавн ивент(true - да/false - нет)")]
                    public Boolean UseEventCustom;
                    [JsonProperty(LanguageEn ? "Static event spawn time" : "Статическое время спавна ивента")]
                    public Int32 EventSpawnTime;
                    [JsonProperty(LanguageEn ? "Random spawn time settings" : "Настройки случайного времени спавна")]
                    public RandomingTime RandomTimeSpawn;
                    internal class RandomingTime
                    {
                        [JsonProperty(LanguageEn ? "Use random event spawn time (static time will not be taken into account) (true - yes/false - no)" : "Использовать случайное время спавно ивента(статическое время не будет учитываться)(true - да/false - нет)")]
                        public Boolean UseRandomTime;
                        [JsonProperty(LanguageEn ? "Minimum event spawn value" : "Минимальное значение спавна ивента")]
                        public Int32 MinEventSpawnTime;
                        [JsonProperty(LanguageEn ? "Max event spawn value" : "Максимальное значении спавна ивента")]
                        public Int32 MaxEventSpawnTime;
                    }

                }

            }

        }

    }

    public static Configuration GetNewConfiguration();
}

internal class PluginSettings
{
    [JsonProperty(LanguageEn ? "Rating settings" : "Настройка рейтингов")]
    public Rates RateSetting;
    [JsonProperty(LanguageEn ? "Additional plugin settings" : "Дополнительная настройка плагина")]
    public OtherSettings OtherSetting;
    [JsonProperty(LanguageEn ? "Configuring supported plugins" : "Настройка поддерживаемых плагинов")]
    public ReferencePlugin ReferenceSettings;
    internal class ReferencePlugin
    {
        [JsonProperty(LanguageEn ? "Setting up IQChat" : "Настройка IQChat")]
        public IQChatReference IQChatSetting;
        internal class IQChatReference
        {
            [JsonProperty(LanguageEn ? "IQChat : Custom prefix in chat" : "IQChat : Кастомный префикс в чате")]
            public String CustomPrefix;
            [JsonProperty(LanguageEn ? "IQChat : Custom chat avatar (If required)" : "IQChat : Кастомный аватар в чате(Если требуется)")]
            public String CustomAvatar;
            [JsonProperty(LanguageEn ? "IQChat : Use UI Notifications" : "IQChat : Использовать UI уведомления")]
            public Boolean UIAlertUse;
        }

    }

    internal class Rates
    {
        [JsonProperty(LanguageEn ? "Ranking setting during the day" : "Настройка рейтинга днем")]
        public AllRates DayRates;
        [JsonProperty(LanguageEn ? "Setting the rating at night" : "Настройка рейтинга ночью")]
        public AllRates NightRates;
        [JsonProperty(LanguageEn ? "Setting privileges and ratings specifically for them [iqrates.vip] = { Setting } (Descending)" : "Настройка привилегий и рейтингов конкретно для них [iqrates.vip] = { Настройка } (По убыванию)")]
        public Dictionary<String, DayAnNightRate> PrivilegyRates;
        [JsonProperty(LanguageEn ? "Setting custom rates (items) by permission - setting (Descending)" : "Настройка кастомных рейтов(предметов) по пермишенсу - настройка (По убыванию)")]
        public PermissionsRate CustomRatesPermissions;
        [JsonProperty(LanguageEn ? "Black list of items that will not be categorically affected by the rating" : "Черный лист предметов,на которые катигорично не будут действовать рейтинг")]
        public List<String> BlackList;
        [JsonProperty(LanguageEn ? "Enable melting speed in furnaces (true - yes/false - no)" : "Включить скорость плавки в печах(true - да/false - нет)")]
        public Boolean UseSpeedBurnable;
        [JsonProperty(LanguageEn ? "A black list of items for the stove, which will not be categorically affected by melting" : "Черный лист предметов для печки,на которые катигорично не будут действовать плавка")]
        public List<String> BlackListBurnable;
        [JsonProperty(LanguageEn ? "Furnace smelting speed (If the list is enabled, this value will be the default for everyone who does not have rights)" : "Скорость плавки печей(Если включен список - это значение будет стандартное для всех у кого нет прав)")]
        public Single SpeedBurnable;
        [JsonProperty(LanguageEn ? "Smelting Fuel Usage Rating (If the list is enabled, this value will be the default value for all non-licensed)" : "Рейтинг использования топлива при переплавки(Если включен список - это значение будет стандартное для всех у кого нет прав)")]
        public Int32 SpeedFuelBurnable;
        [JsonProperty(LanguageEn ? "Enable list of melting speed in furnaces (true - yes/false - no)" : "Включить список скорости плавки в печах(true - да/false - нет)")]
        public Boolean UseSpeedBurnableList;
        [JsonProperty(LanguageEn ? "Setting the melting speed in furnaces by privileges" : "Настройка скорости плавки в печах по привилегиям")]
        public List<SpeedBurnablePreset> SpeedBurableList;
        internal class DayAnNightRate
        {
            [JsonProperty(LanguageEn ? "Ranking setting during the day" : "Настройка рейтинга днем")]
            public AllRates DayRates;
            [JsonProperty(LanguageEn ? "Setting the rating at night" : "Настройка рейтинга ночью")]
            public AllRates NightRates;
        }

        internal class SpeedBurnablePreset
        {
            [JsonProperty(LanguageEn ? "Permissions" : "Права")]
            public String Permissions;
            [JsonProperty(LanguageEn ? "Furnace melting speed" : "Скорость плавки печей")]
            public Single SpeedBurnable;
            [JsonProperty(LanguageEn ? "Smelting Fuel Use Rating" : "Рейтинг использования топлива при переплавки")]
            public Int32 SpeedFuelBurnable;
        }

        internal class PermissionsRate
        {
            [JsonProperty(LanguageEn ? "Ranking setting during the day" : "Настройка рейтинга днем")]
            public Dictionary<String, List<PermissionsRateDetalis>> DayRates;
            [JsonProperty(LanguageEn ? "Setting the rating at night" : "Настройка рейтинга ночью")]
            public Dictionary<String, List<PermissionsRateDetalis>> NightRates;
            public class PermissionsRateDetalis
            {
                [JsonProperty(LanguageEn ? "Shortname" : "Shortname")]
                public String Shortname;
                [JsonProperty(LanguageEn ? "Rate" : "Рейтинг")]
                public Single Rate;
            }

        }

        internal class AllRates
        {
            [JsonProperty(LanguageEn ? "Rating of extracted resources" : "Рейтинг добываемых ресурсов")]
            public Single GatherRate;
            [JsonProperty(LanguageEn ? "Rating of found items" : "Рейтинг найденных предметов")]
            public Single LootRate;
            [JsonProperty(LanguageEn ? "Pickup Rating" : "Рейтинг поднимаемых предметов")]
            public Single PickUpRate;
            [JsonProperty(LanguageEn ? "Rating of plants raised from the beds" : "Рейтинг поднимаемых растений с грядок")]
            public Single GrowableRate;
            [JsonProperty(LanguageEn ? "Quarry rating" : "Рейтинг карьеров")]
            public Single QuarryRate;
            [JsonProperty(LanguageEn ? "Excavator Rating" : "Рейтинг экскаватора")]
            public Single ExcavatorRate;
            [JsonProperty(LanguageEn ? "Coal drop chance" : "Шанс выпадения угля")]
            public Single CoalRare;
        }

    }

    internal class OtherSettings
    {
        [JsonProperty(LanguageEn ? "Event settings on the server" : "Настройки ивентов на сервере")]
        public EventSettings EventSetting;
        [JsonProperty(LanguageEn ? "Fuel settings when buying vehicles from NPCs" : "Настройки топлива при покупке транспорта у NPC")]
        public FuelSettings FuelSetting;
        internal class FuelSettings
        {
            [JsonProperty(LanguageEn ? "Amount of fuel for boats" : "Кол-во топлива у лодок")]
            public Int32 AmountBoat;
            [JsonProperty(LanguageEn ? "The amount of fuel in submarines" : "Кол-во топлива у подводных лодок")]
            public Int32 AmountSubmarine;
            [JsonProperty(LanguageEn ? "Minicopter fuel quantity" : "Кол-во топлива у миникоптера")]
            public Int32 AmountMinicopter;
            [JsonProperty(LanguageEn ? "Helicopter fuel quantity" : "Кол-во топлива у вертолета")]
            public Int32 AmountScrapTransport;
        }

        [JsonProperty(LanguageEn ? "Use Time Acceleration" : "Использовать ускорение времени")]
        public Boolean UseTime;
        [JsonProperty(LanguageEn ? "Use time freeze (the time will be the one you set in the item &lt;Frozen time on the server&gt;)" : "Использовать заморозку времени(время будет такое, какое вы установите в пунке <Замороженное время на сервере>)")]
        public Boolean UseFreezeTime;
        [JsonProperty(LanguageEn ? "Frozen time on the server (Set time that will not change and be forever on the server, must be true on &lt;Use time freeze&gt;" : "Замороженное время на сервере (Установите время, которое не будет изменяться и будет вечно на сервере, должен быть true на <Использовать заморозку времени>")]
        public Int32 FreezeTime;
        [JsonProperty(LanguageEn ? "What time will the day start?" : "Укажите во сколько будет начинаться день")]
        public Int32 DayStart;
        [JsonProperty(LanguageEn ? "What time will the night start?" : "Укажите во сколько будет начинаться ночь")]
        public Int32 NightStart;
        [JsonProperty(LanguageEn ? "Specify how long the day will be in minutes" : "Укажите сколько будет длится день в минутах")]
        public Int32 DayTime;
        [JsonProperty(LanguageEn ? "Specify how long the night will last in minutes" : "Укажите сколько будет длится ночь в минутах")]
        public Int32 NightTime;
        [JsonProperty(LanguageEn ? "Use notification of players about the change of day and night (switching rates. The message is configured in the lang)" : "Использовать уведомление игроков о смене дня и ночи (переключение рейтов. Сообщение настраивается в лэнге)")]
        public Boolean UseAlertDayNight;
        [JsonProperty(LanguageEn ? "Enable the ability to completely skip the time of day (selected in the paragraph below)" : "Включить возможность полного пропуска времени суток(выбирается в пункте ниже)")]
        public Boolean UseSkipTime;
        [JsonProperty(LanguageEn ? "Select the type of time-of-day skip (0 - Skip day, 1 - Skip night)" : "Выберите тип пропуска времени суток (0 - Пропускать день, 1 - Пропускать ночь)(Не забудьте включить возможность полного пропуска времени суток)")]
        public SkipType TypeSkipped;
        internal class EventSettings
        {
            [JsonProperty(LanguageEn ? "Helicopter spawn custom settings" : "Кастомные настройки спавна вертолета")]
            public Setting HelicopterSetting;
            [JsonProperty(LanguageEn ? "Custom tank spawn settings" : "Кастомные настройки спавна танка")]
            public Setting BreadlaySetting;
            [JsonProperty(LanguageEn ? "Custom ship spawn settings" : "Кастомные настройки спавна корабля")]
            public Setting CargoShipSetting;
            [JsonProperty(LanguageEn ? "Airdrop spawn custom settings" : "Кастомные настройки спавна аирдропа")]
            public Setting CargoPlaneSetting;
            [JsonProperty(LanguageEn ? "Chinook custom spawn settings" : "Кастомные настройки спавна чинука")]
            public Setting ChinoockSetting;
            internal class Setting
            {
                [JsonProperty(LanguageEn ? "Completely disable event spawning on the server (true - yes/false - no)" : "Полностью отключить спавн ивента на сервере(true - да/false - нет)")]
                public Boolean FullOff;
                [JsonProperty(LanguageEn ? "Enable custom spawn event (true - yes/false - no)" : "Включить кастомный спавн ивент(true - да/false - нет)")]
                public Boolean UseEventCustom;
                [JsonProperty(LanguageEn ? "Static event spawn time" : "Статическое время спавна ивента")]
                public Int32 EventSpawnTime;
                [JsonProperty(LanguageEn ? "Random spawn time settings" : "Настройки случайного времени спавна")]
                public RandomingTime RandomTimeSpawn;
                internal class RandomingTime
                {
                    [JsonProperty(LanguageEn ? "Use random event spawn time (static time will not be taken into account) (true - yes/false - no)" : "Использовать случайное время спавно ивента(статическое время не будет учитываться)(true - да/false - нет)")]
                    public Boolean UseRandomTime;
                    [JsonProperty(LanguageEn ? "Minimum event spawn value" : "Минимальное значение спавна ивента")]
                    public Int32 MinEventSpawnTime;
                    [JsonProperty(LanguageEn ? "Max event spawn value" : "Максимальное значении спавна ивента")]
                    public Int32 MaxEventSpawnTime;
                }

            }

        }

    }

}

internal class ReferencePlugin
{
    [JsonProperty(LanguageEn ? "Setting up IQChat" : "Настройка IQChat")]
    public IQChatReference IQChatSetting;
    internal class IQChatReference
    {
        [JsonProperty(LanguageEn ? "IQChat : Custom prefix in chat" : "IQChat : Кастомный префикс в чате")]
        public String CustomPrefix;
        [JsonProperty(LanguageEn ? "IQChat : Custom chat avatar (If required)" : "IQChat : Кастомный аватар в чате(Если требуется)")]
        public String CustomAvatar;
        [JsonProperty(LanguageEn ? "IQChat : Use UI Notifications" : "IQChat : Использовать UI уведомления")]
        public Boolean UIAlertUse;
    }

}

internal class IQChatReference
{
    [JsonProperty(LanguageEn ? "IQChat : Custom prefix in chat" : "IQChat : Кастомный префикс в чате")]
    public String CustomPrefix;
    [JsonProperty(LanguageEn ? "IQChat : Custom chat avatar (If required)" : "IQChat : Кастомный аватар в чате(Если требуется)")]
    public String CustomAvatar;
    [JsonProperty(LanguageEn ? "IQChat : Use UI Notifications" : "IQChat : Использовать UI уведомления")]
    public Boolean UIAlertUse;
}

internal class Rates
{
    [JsonProperty(LanguageEn ? "Ranking setting during the day" : "Настройка рейтинга днем")]
    public AllRates DayRates;
    [JsonProperty(LanguageEn ? "Setting the rating at night" : "Настройка рейтинга ночью")]
    public AllRates NightRates;
    [JsonProperty(LanguageEn ? "Setting privileges and ratings specifically for them [iqrates.vip] = { Setting } (Descending)" : "Настройка привилегий и рейтингов конкретно для них [iqrates.vip] = { Настройка } (По убыванию)")]
    public Dictionary<String, DayAnNightRate> PrivilegyRates;
    [JsonProperty(LanguageEn ? "Setting custom rates (items) by permission - setting (Descending)" : "Настройка кастомных рейтов(предметов) по пермишенсу - настройка (По убыванию)")]
    public PermissionsRate CustomRatesPermissions;
    [JsonProperty(LanguageEn ? "Black list of items that will not be categorically affected by the rating" : "Черный лист предметов,на которые катигорично не будут действовать рейтинг")]
    public List<String> BlackList;
    [JsonProperty(LanguageEn ? "Enable melting speed in furnaces (true - yes/false - no)" : "Включить скорость плавки в печах(true - да/false - нет)")]
    public Boolean UseSpeedBurnable;
    [JsonProperty(LanguageEn ? "A black list of items for the stove, which will not be categorically affected by melting" : "Черный лист предметов для печки,на которые катигорично не будут действовать плавка")]
    public List<String> BlackListBurnable;
    [JsonProperty(LanguageEn ? "Furnace smelting speed (If the list is enabled, this value will be the default for everyone who does not have rights)" : "Скорость плавки печей(Если включен список - это значение будет стандартное для всех у кого нет прав)")]
    public Single SpeedBurnable;
    [JsonProperty(LanguageEn ? "Smelting Fuel Usage Rating (If the list is enabled, this value will be the default value for all non-licensed)" : "Рейтинг использования топлива при переплавки(Если включен список - это значение будет стандартное для всех у кого нет прав)")]
    public Int32 SpeedFuelBurnable;
    [JsonProperty(LanguageEn ? "Enable list of melting speed in furnaces (true - yes/false - no)" : "Включить список скорости плавки в печах(true - да/false - нет)")]
    public Boolean UseSpeedBurnableList;
    [JsonProperty(LanguageEn ? "Setting the melting speed in furnaces by privileges" : "Настройка скорости плавки в печах по привилегиям")]
    public List<SpeedBurnablePreset> SpeedBurableList;
    internal class DayAnNightRate
    {
        [JsonProperty(LanguageEn ? "Ranking setting during the day" : "Настройка рейтинга днем")]
        public AllRates DayRates;
        [JsonProperty(LanguageEn ? "Setting the rating at night" : "Настройка рейтинга ночью")]
        public AllRates NightRates;
    }

    internal class SpeedBurnablePreset
    {
        [JsonProperty(LanguageEn ? "Permissions" : "Права")]
        public String Permissions;
        [JsonProperty(LanguageEn ? "Furnace melting speed" : "Скорость плавки печей")]
        public Single SpeedBurnable;
        [JsonProperty(LanguageEn ? "Smelting Fuel Use Rating" : "Рейтинг использования топлива при переплавки")]
        public Int32 SpeedFuelBurnable;
    }

    internal class PermissionsRate
    {
        [JsonProperty(LanguageEn ? "Ranking setting during the day" : "Настройка рейтинга днем")]
        public Dictionary<String, List<PermissionsRateDetalis>> DayRates;
        [JsonProperty(LanguageEn ? "Setting the rating at night" : "Настройка рейтинга ночью")]
        public Dictionary<String, List<PermissionsRateDetalis>> NightRates;
        public class PermissionsRateDetalis
        {
            [JsonProperty(LanguageEn ? "Shortname" : "Shortname")]
            public String Shortname;
            [JsonProperty(LanguageEn ? "Rate" : "Рейтинг")]
            public Single Rate;
        }

    }

    internal class AllRates
    {
        [JsonProperty(LanguageEn ? "Rating of extracted resources" : "Рейтинг добываемых ресурсов")]
        public Single GatherRate;
        [JsonProperty(LanguageEn ? "Rating of found items" : "Рейтинг найденных предметов")]
        public Single LootRate;
        [JsonProperty(LanguageEn ? "Pickup Rating" : "Рейтинг поднимаемых предметов")]
        public Single PickUpRate;
        [JsonProperty(LanguageEn ? "Rating of plants raised from the beds" : "Рейтинг поднимаемых растений с грядок")]
        public Single GrowableRate;
        [JsonProperty(LanguageEn ? "Quarry rating" : "Рейтинг карьеров")]
        public Single QuarryRate;
        [JsonProperty(LanguageEn ? "Excavator Rating" : "Рейтинг экскаватора")]
        public Single ExcavatorRate;
        [JsonProperty(LanguageEn ? "Coal drop chance" : "Шанс выпадения угля")]
        public Single CoalRare;
    }

}

internal class DayAnNightRate
{
    [JsonProperty(LanguageEn ? "Ranking setting during the day" : "Настройка рейтинга днем")]
    public AllRates DayRates;
    [JsonProperty(LanguageEn ? "Setting the rating at night" : "Настройка рейтинга ночью")]
    public AllRates NightRates;
}

internal class SpeedBurnablePreset
{
    [JsonProperty(LanguageEn ? "Permissions" : "Права")]
    public String Permissions;
    [JsonProperty(LanguageEn ? "Furnace melting speed" : "Скорость плавки печей")]
    public Single SpeedBurnable;
    [JsonProperty(LanguageEn ? "Smelting Fuel Use Rating" : "Рейтинг использования топлива при переплавки")]
    public Int32 SpeedFuelBurnable;
}

internal class PermissionsRate
{
    [JsonProperty(LanguageEn ? "Ranking setting during the day" : "Настройка рейтинга днем")]
    public Dictionary<String, List<PermissionsRateDetalis>> DayRates;
    [JsonProperty(LanguageEn ? "Setting the rating at night" : "Настройка рейтинга ночью")]
    public Dictionary<String, List<PermissionsRateDetalis>> NightRates;
    public class PermissionsRateDetalis
    {
        [JsonProperty(LanguageEn ? "Shortname" : "Shortname")]
        public String Shortname;
        [JsonProperty(LanguageEn ? "Rate" : "Рейтинг")]
        public Single Rate;
    }

}

public class PermissionsRateDetalis
{
    [JsonProperty(LanguageEn ? "Shortname" : "Shortname")]
    public String Shortname;
    [JsonProperty(LanguageEn ? "Rate" : "Рейтинг")]
    public Single Rate;
}

internal class AllRates
{
    [JsonProperty(LanguageEn ? "Rating of extracted resources" : "Рейтинг добываемых ресурсов")]
    public Single GatherRate;
    [JsonProperty(LanguageEn ? "Rating of found items" : "Рейтинг найденных предметов")]
    public Single LootRate;
    [JsonProperty(LanguageEn ? "Pickup Rating" : "Рейтинг поднимаемых предметов")]
    public Single PickUpRate;
    [JsonProperty(LanguageEn ? "Rating of plants raised from the beds" : "Рейтинг поднимаемых растений с грядок")]
    public Single GrowableRate;
    [JsonProperty(LanguageEn ? "Quarry rating" : "Рейтинг карьеров")]
    public Single QuarryRate;
    [JsonProperty(LanguageEn ? "Excavator Rating" : "Рейтинг экскаватора")]
    public Single ExcavatorRate;
    [JsonProperty(LanguageEn ? "Coal drop chance" : "Шанс выпадения угля")]
    public Single CoalRare;
}

internal class OtherSettings
{
    [JsonProperty(LanguageEn ? "Event settings on the server" : "Настройки ивентов на сервере")]
    public EventSettings EventSetting;
    [JsonProperty(LanguageEn ? "Fuel settings when buying vehicles from NPCs" : "Настройки топлива при покупке транспорта у NPC")]
    public FuelSettings FuelSetting;
    internal class FuelSettings
    {
        [JsonProperty(LanguageEn ? "Amount of fuel for boats" : "Кол-во топлива у лодок")]
        public Int32 AmountBoat;
        [JsonProperty(LanguageEn ? "The amount of fuel in submarines" : "Кол-во топлива у подводных лодок")]
        public Int32 AmountSubmarine;
        [JsonProperty(LanguageEn ? "Minicopter fuel quantity" : "Кол-во топлива у миникоптера")]
        public Int32 AmountMinicopter;
        [JsonProperty(LanguageEn ? "Helicopter fuel quantity" : "Кол-во топлива у вертолета")]
        public Int32 AmountScrapTransport;
    }

    [JsonProperty(LanguageEn ? "Use Time Acceleration" : "Использовать ускорение времени")]
    public Boolean UseTime;
    [JsonProperty(LanguageEn ? "Use time freeze (the time will be the one you set in the item &lt;Frozen time on the server&gt;)" : "Использовать заморозку времени(время будет такое, какое вы установите в пунке <Замороженное время на сервере>)")]
    public Boolean UseFreezeTime;
    [JsonProperty(LanguageEn ? "Frozen time on the server (Set time that will not change and be forever on the server, must be true on &lt;Use time freeze&gt;" : "Замороженное время на сервере (Установите время, которое не будет изменяться и будет вечно на сервере, должен быть true на <Использовать заморозку времени>")]
    public Int32 FreezeTime;
    [JsonProperty(LanguageEn ? "What time will the day start?" : "Укажите во сколько будет начинаться день")]
    public Int32 DayStart;
    [JsonProperty(LanguageEn ? "What time will the night start?" : "Укажите во сколько будет начинаться ночь")]
    public Int32 NightStart;
    [JsonProperty(LanguageEn ? "Specify how long the day will be in minutes" : "Укажите сколько будет длится день в минутах")]
    public Int32 DayTime;
    [JsonProperty(LanguageEn ? "Specify how long the night will last in minutes" : "Укажите сколько будет длится ночь в минутах")]
    public Int32 NightTime;
    [JsonProperty(LanguageEn ? "Use notification of players about the change of day and night (switching rates. The message is configured in the lang)" : "Использовать уведомление игроков о смене дня и ночи (переключение рейтов. Сообщение настраивается в лэнге)")]
    public Boolean UseAlertDayNight;
    [JsonProperty(LanguageEn ? "Enable the ability to completely skip the time of day (selected in the paragraph below)" : "Включить возможность полного пропуска времени суток(выбирается в пункте ниже)")]
    public Boolean UseSkipTime;
    [JsonProperty(LanguageEn ? "Select the type of time-of-day skip (0 - Skip day, 1 - Skip night)" : "Выберите тип пропуска времени суток (0 - Пропускать день, 1 - Пропускать ночь)(Не забудьте включить возможность полного пропуска времени суток)")]
    public SkipType TypeSkipped;
    internal class EventSettings
    {
        [JsonProperty(LanguageEn ? "Helicopter spawn custom settings" : "Кастомные настройки спавна вертолета")]
        public Setting HelicopterSetting;
        [JsonProperty(LanguageEn ? "Custom tank spawn settings" : "Кастомные настройки спавна танка")]
        public Setting BreadlaySetting;
        [JsonProperty(LanguageEn ? "Custom ship spawn settings" : "Кастомные настройки спавна корабля")]
        public Setting CargoShipSetting;
        [JsonProperty(LanguageEn ? "Airdrop spawn custom settings" : "Кастомные настройки спавна аирдропа")]
        public Setting CargoPlaneSetting;
        [JsonProperty(LanguageEn ? "Chinook custom spawn settings" : "Кастомные настройки спавна чинука")]
        public Setting ChinoockSetting;
        internal class Setting
        {
            [JsonProperty(LanguageEn ? "Completely disable event spawning on the server (true - yes/false - no)" : "Полностью отключить спавн ивента на сервере(true - да/false - нет)")]
            public Boolean FullOff;
            [JsonProperty(LanguageEn ? "Enable custom spawn event (true - yes/false - no)" : "Включить кастомный спавн ивент(true - да/false - нет)")]
            public Boolean UseEventCustom;
            [JsonProperty(LanguageEn ? "Static event spawn time" : "Статическое время спавна ивента")]
            public Int32 EventSpawnTime;
            [JsonProperty(LanguageEn ? "Random spawn time settings" : "Настройки случайного времени спавна")]
            public RandomingTime RandomTimeSpawn;
            internal class RandomingTime
            {
                [JsonProperty(LanguageEn ? "Use random event spawn time (static time will not be taken into account) (true - yes/false - no)" : "Использовать случайное время спавно ивента(статическое время не будет учитываться)(true - да/false - нет)")]
                public Boolean UseRandomTime;
                [JsonProperty(LanguageEn ? "Minimum event spawn value" : "Минимальное значение спавна ивента")]
                public Int32 MinEventSpawnTime;
                [JsonProperty(LanguageEn ? "Max event spawn value" : "Максимальное значении спавна ивента")]
                public Int32 MaxEventSpawnTime;
            }

        }

    }

}

internal class FuelSettings
{
    [JsonProperty(LanguageEn ? "Amount of fuel for boats" : "Кол-во топлива у лодок")]
    public Int32 AmountBoat;
    [JsonProperty(LanguageEn ? "The amount of fuel in submarines" : "Кол-во топлива у подводных лодок")]
    public Int32 AmountSubmarine;
    [JsonProperty(LanguageEn ? "Minicopter fuel quantity" : "Кол-во топлива у миникоптера")]
    public Int32 AmountMinicopter;
    [JsonProperty(LanguageEn ? "Helicopter fuel quantity" : "Кол-во топлива у вертолета")]
    public Int32 AmountScrapTransport;
}

internal class EventSettings
{
    [JsonProperty(LanguageEn ? "Helicopter spawn custom settings" : "Кастомные настройки спавна вертолета")]
    public Setting HelicopterSetting;
    [JsonProperty(LanguageEn ? "Custom tank spawn settings" : "Кастомные настройки спавна танка")]
    public Setting BreadlaySetting;
    [JsonProperty(LanguageEn ? "Custom ship spawn settings" : "Кастомные настройки спавна корабля")]
    public Setting CargoShipSetting;
    [JsonProperty(LanguageEn ? "Airdrop spawn custom settings" : "Кастомные настройки спавна аирдропа")]
    public Setting CargoPlaneSetting;
    [JsonProperty(LanguageEn ? "Chinook custom spawn settings" : "Кастомные настройки спавна чинука")]
    public Setting ChinoockSetting;
    internal class Setting
    {
        [JsonProperty(LanguageEn ? "Completely disable event spawning on the server (true - yes/false - no)" : "Полностью отключить спавн ивента на сервере(true - да/false - нет)")]
        public Boolean FullOff;
        [JsonProperty(LanguageEn ? "Enable custom spawn event (true - yes/false - no)" : "Включить кастомный спавн ивент(true - да/false - нет)")]
        public Boolean UseEventCustom;
        [JsonProperty(LanguageEn ? "Static event spawn time" : "Статическое время спавна ивента")]
        public Int32 EventSpawnTime;
        [JsonProperty(LanguageEn ? "Random spawn time settings" : "Настройки случайного времени спавна")]
        public RandomingTime RandomTimeSpawn;
        internal class RandomingTime
        {
            [JsonProperty(LanguageEn ? "Use random event spawn time (static time will not be taken into account) (true - yes/false - no)" : "Использовать случайное время спавно ивента(статическое время не будет учитываться)(true - да/false - нет)")]
            public Boolean UseRandomTime;
            [JsonProperty(LanguageEn ? "Minimum event spawn value" : "Минимальное значение спавна ивента")]
            public Int32 MinEventSpawnTime;
            [JsonProperty(LanguageEn ? "Max event spawn value" : "Максимальное значении спавна ивента")]
            public Int32 MaxEventSpawnTime;
        }

    }

}

internal class Setting
{
    [JsonProperty(LanguageEn ? "Completely disable event spawning on the server (true - yes/false - no)" : "Полностью отключить спавн ивента на сервере(true - да/false - нет)")]
    public Boolean FullOff;
    [JsonProperty(LanguageEn ? "Enable custom spawn event (true - yes/false - no)" : "Включить кастомный спавн ивент(true - да/false - нет)")]
    public Boolean UseEventCustom;
    [JsonProperty(LanguageEn ? "Static event spawn time" : "Статическое время спавна ивента")]
    public Int32 EventSpawnTime;
    [JsonProperty(LanguageEn ? "Random spawn time settings" : "Настройки случайного времени спавна")]
    public RandomingTime RandomTimeSpawn;
    internal class RandomingTime
    {
        [JsonProperty(LanguageEn ? "Use random event spawn time (static time will not be taken into account) (true - yes/false - no)" : "Использовать случайное время спавно ивента(статическое время не будет учитываться)(true - да/false - нет)")]
        public Boolean UseRandomTime;
        [JsonProperty(LanguageEn ? "Minimum event spawn value" : "Минимальное значение спавна ивента")]
        public Int32 MinEventSpawnTime;
        [JsonProperty(LanguageEn ? "Max event spawn value" : "Максимальное значении спавна ивента")]
        public Int32 MaxEventSpawnTime;
    }

}

internal class RandomingTime
{
    [JsonProperty(LanguageEn ? "Use random event spawn time (static time will not be taken into account) (true - yes/false - no)" : "Использовать случайное время спавно ивента(статическое время не будет учитываться)(true - да/false - нет)")]
    public Boolean UseRandomTime;
    [JsonProperty(LanguageEn ? "Minimum event spawn value" : "Минимальное значение спавна ивента")]
    public Int32 MinEventSpawnTime;
    [JsonProperty(LanguageEn ? "Max event spawn value" : "Максимальное значении спавна ивента")]
    public Int32 MaxEventSpawnTime;
}

private class OvenController : FacepunchBehaviour
{
    private static readonly Dictionary<BaseOven, OvenController> Controllers;
    private BaseOven _oven;
    private float _speed;
    private Int32 _ticks;
    private string _ownerId;
    private Int32 _speedFuel;
    private bool IsFurnace { get; set; }
    private void Awake();
    public object Switch(BasePlayer player);
    public void TryRestart();
    private void Kill();
    public static OvenController GetOrAdd(BaseOven oven);
    public static void TryRestartAll();
    public static void KillAll();
    private void StartCooking();
    private void StopCooking();
    public void Cook();
    private void SmeltItems();
}


```

---

## IQRates-1.99.37

```csharp
using UnityEngine;
using Newtonsoft.Json;
using Oxide.Core.Plugins;
using System.Collections.Generic;
using System.Linq;
using Oxide.Core;
using System;
using Object = System.Object;
using System.Text;
using Facepunch;
using System.Collections;

Oxide.Plugins
[Info("IQRates", "Mercury", "1.99.37")]
[Description("Настройка рейтинга на сервере")]
 class IQRates : RustPlugin
{
    private const Boolean LanguageEn;
     bool IsPermission(string userID, string Permission);
    private void StartCargoPlane(Configuration.PluginSettings.OtherSettings.EventSettings EventSettings);
     UInt64? GetQuarryPlayer(UInt64 NetID);
     void StartupFreeze();
     void OnQuarryToggled(MiningQuarry quarry, BasePlayer player);
    private class OvenController : FacepunchBehaviour
    {
        private static readonly Dictionary<BaseOven, OvenController> Controllers;
        private BaseOven _oven;
        private float _speed;
        private string _ownerId;
        private Int32 _ticks;
        private Int32 _speedFuel;
        private bool IsFurnace { get; set; }
        private void Awake();
        public object Switch(BasePlayer player);
        public void TryRestart();
        private void Kill();
        public static OvenController GetOrAdd(BaseOven oven);
        public static void TryRestartAll();
        public static void KillAll();
        public void OnDestroy();
        private void StartCooking();
        private void StopCooking();
        public void Cook();
        private void SmeltItems();
    }

    private static StringBuilder sb;
    private const string prefabCH47;
    public string GetLang(string LangKey, string userID, object[] args);
    [ChatCommand("rates")]
    private void GetInfoMyRates(BasePlayer player);
    private void OnMixingTableToggle(MixingTable table, BasePlayer player);
    public Int32 GetMultiplaceBurnableFuelSpeed(String ownerid);
    private object OnPlayerAddModifiers(BasePlayer player, Item item, ItemModConsumable consumable);
    private TriggeredEventPrefab[] defaultEvents;
     void SpawnCH47();
    private void UnSubProSub(int time);
    private void OnEntitySpawned(Minicopter copter);
    private void StartBreadley(Configuration.PluginSettings.OtherSettings.EventSettings EventSettings);
    private void Init();
     int Converted(Types RateType, string Shortname, float Amount, BasePlayer player, String UserID);
     void OnQuarryGather(MiningQuarry quarry, Item item);
    public void SendChat(String Message, BasePlayer player, ConVar.Chat.ChatChannel channel);
    private const string prefabPatrol;
    [PluginReference]
     Plugin IQChat;
     Item OnFishCatch(Item item, BaseFishingRod rod, BasePlayer player);
    private void OnEntitySpawned(BradleyAPC entity);
     void OnEntityKill(BaseNetworkable entity);
    public List<UInt64> LootersSaveListCrateID;
    private TOD_Time timeComponent;
    public class ModiferTea
    {
        public Modifier.ModifierType Type;
        public Single Duration;
        public Single Value;
    }

    private void OnEntitySpawned(ScrapTransportHelicopter helicopter);
     Single GetBonusRate(BasePlayer player);
     void OnSunset();
    private void OnEntitySpawned(TrainCar trainCar);
    private void OnServerInitialized();
    public List<UInt64> LootersListCrateID;
     void OnDispenserBonus(ResourceDispenser disp, BasePlayer player, Item item);
     void OnWireConnect(BasePlayer player, IOEntity entity1, int inputs, IOEntity entity2, int outputs);
    private void FuelSystemRating(IFuelSystem fuelSystem, Int32 Amount);
    private void OnNewSave(String filename);
     void SetTimeComponent();
    public Dictionary<UInt64, UInt64> DataQuarryPlayer;
    private static Configuration config;
     void OnContainerDropItems(ItemContainer container);
     void API_BONUS_RATE_ADDPLAYER(BasePlayer player, Single Rate);
     void SpawnHeli();
    private void StartHelicopter(Configuration.PluginSettings.OtherSettings.EventSettings EventSettings);
     void AddQuarryPlayer(UInt64 NetID, UInt64 userID);
    private void OnEntitySpawned(Snowmobile snowmobiles);
    private BasePlayer ExcavatorPlayer;
     void OnGrowableGathered(GrowableEntity plant, Item item, BasePlayer player);
    private void UpdateBonusStatus(Boolean isInit);
     bool IsTime();
     float GetRareCoal(BasePlayer player);
    protected override void LoadDefaultConfig();
     void SpawnCargo();
    private class Configuration
    {
        [JsonProperty(LanguageEn ? "Plugin setup" : "Настройка плагина")]
        public PluginSettings pluginSettings;
        public static Configuration GetNewConfiguration();
        internal class PluginSettings
        {
            internal class ReferencePlugin
            {
                internal class IQChatReference
                {
                    [JsonProperty(LanguageEn ? "IQChat : Custom chat avatar (If required)" : "IQChat : Кастомный аватар в чате(Если требуется)")]
                    public String CustomAvatar;
                    [JsonProperty(LanguageEn ? "IQChat : Use UI Notifications" : "IQChat : Использовать UI уведомления")]
                    public Boolean UIAlertUse;
                    [JsonProperty(LanguageEn ? "IQChat : Custom prefix in chat" : "IQChat : Кастомный префикс в чате")]
                    public String CustomPrefix;
                }

                [JsonProperty(LanguageEn ? "Setting up IQChat" : "Настройка IQChat")]
                public IQChatReference IQChatSetting;
            }

            [JsonProperty(LanguageEn ? "Rating settings" : "Настройка рейтингов")]
            public Rates RateSetting;
            [JsonProperty(LanguageEn ? "Additional plugin settings" : "Дополнительная настройка плагина")]
            public OtherSettings OtherSetting;
            internal class OtherSettings
            {
                [JsonProperty(LanguageEn ? "Event settings on the server" : "Настройки ивентов на сервере")]
                public EventSettings EventSetting;
                [JsonProperty(LanguageEn ? "Fuel settings when buying vehicles from NPCs" : "Настройки топлива при покупке транспорта у NPC")]
                public FuelSettings FuelSetting;
                [JsonProperty(LanguageEn ? "Fuel Consumption Rating Settings" : "Настройки рейтинга потребления топлива")]
                public FuelConsumedTransport FuelConsumedTransportSetting;
                internal class FuelConsumedTransport
                {
                    [JsonProperty(LanguageEn ? "Use the fuel consumption rating in transport (true - yes/false - no)" : "Использовать рейтинг потребления топлива в транспорте (true - да/false - нет)")]
                    public Boolean useConsumedFuel;
                    [JsonProperty(LanguageEn ? "Hotairballoon fuel consumption rating (Default = 0.25)" : "Рейтинг потребление топлива у воздушного шара (Стандартно = 0.25)")]
                    public Single ConsumedHotAirBalloon;
                    [JsonProperty(LanguageEn ? "Snowmobile fuel consumption rating (Default = 0.15)" : "Рейтинг потребление топлива снегоходов (Стандартно = 0.15)")]
                    public Single ConsumedSnowmobile;
                    [JsonProperty(LanguageEn ? "Train fuel consumption rating (Default = 0.15)" : "Рейтинг потребление топлива поездов (Стандартно = 0.15)")]
                    public Single ConsumedTrain;
                    [JsonProperty(LanguageEn ? "Rowboat fuel consumption rating (Default = 0.25)" : "Рейтинг потребление топлива у лодок (Стандартно = 0.25)")]
                    public Single ConsumedBoat;
                    [JsonProperty(LanguageEn ? "Submarine fuel consumption rating (Default = 0.15)" : "Рейтинг потребление топлива у субмарин (Стандартно = 0.15)")]
                    public Single ConsumedSubmarine;
                    [JsonProperty(LanguageEn ? "Minicopter fuel consumption rating (Default = 0.25)" : "Рейтинг потребление топлива у миникоптера (Стандартно = 0.25)")]
                    public Single ConsumedCopter;
                    [JsonProperty(LanguageEn ? "ScrapTransportHelicopter fuel consumption rating (Default = 0.25)" : "Рейтинг потребление топлива у коровы (Стандартно = 0.25)")]
                    public Single ConsumedScrapTransport;
                    [JsonProperty(LanguageEn ? "Attack-Helicopter fuel consumption rating (Default = 0.25)" : "Рейтинг потребление топлива у боевого-вертолета (Стандартно = 0.25)")]
                    public Single ConsumedAttackHelicopter;
                }

                internal class FuelSettings
                {
                    [JsonProperty(LanguageEn ? "Use fuel replenishment when buying vehicles from NPC (true - yes/false - no)" : "Использовать пополнение топлива при покупке транспорта у NPC (true - да/false - нет)")]
                    public Boolean useAutoFillFuel;
                    [JsonProperty(LanguageEn ? "Amount of fuel for boats" : "Кол-во топлива у лодок")]
                    public Int32 AmountBoat;
                    [JsonProperty(LanguageEn ? "The amount of fuel in submarines" : "Кол-во топлива у подводных лодок")]
                    public Int32 AmountSubmarine;
                    [JsonProperty(LanguageEn ? "Minicopter fuel quantity" : "Кол-во топлива у миникоптера")]
                    public Int32 AmountMinicopter;
                    [JsonProperty(LanguageEn ? "Helicopter fuel quantity" : "Кол-во топлива у вертолета")]
                    public Int32 AmountScrapTransport;
                    [JsonProperty(LanguageEn ? "Attack-Helicopter fuel quantity" : "Кол-во топлива у боевого вертолета")]
                    public Int32 AmountAttackHelicopter;
                }

                [JsonProperty(LanguageEn ? "Use Time Acceleration" : "Использовать ускорение времени")]
                public Boolean UseTime;
                [JsonProperty(LanguageEn ? "Use time freeze (the time will be the one you set in the item &lt;Frozen time on the server&gt;)" : "Использовать заморозку времени(время будет такое, какое вы установите в пунке <Замороженное время на сервере>)")]
                public Boolean UseFreezeTime;
                [JsonProperty(LanguageEn ? "Frozen time on the server (Set time that will not change and be forever on the server, must be true on &lt;Use time freeze&gt;" : "Замороженное время на сервере (Установите время, которое не будет изменяться и будет вечно на сервере, должен быть true на <Использовать заморозку времени>")]
                public Int32 FreezeTime;
                [JsonProperty(LanguageEn ? "What time will the day start?" : "Укажите во сколько будет начинаться день")]
                public Int32 DayStart;
                [JsonProperty(LanguageEn ? "What time will the night start?" : "Укажите во сколько будет начинаться ночь")]
                public Int32 NightStart;
                [JsonProperty(LanguageEn ? "Specify how long the day will be in minutes" : "Укажите сколько будет длится день в минутах")]
                public Int32 DayTime;
                [JsonProperty(LanguageEn ? "Specify how long the night will last in minutes" : "Укажите сколько будет длится ночь в минутах")]
                public Int32 NightTime;
                [JsonProperty(LanguageEn ? "Use notification of players about the change of day and night (switching rates. The message is configured in the lang)" : "Использовать уведомление игроков о смене дня и ночи (переключение рейтов. Сообщение настраивается в лэнге)")]
                public Boolean UseAlertDayNight;
                [JsonProperty(LanguageEn ? "Enable the ability to completely skip the time of day (selected in the paragraph below)" : "Включить возможность полного пропуска времени суток(выбирается в пункте ниже)")]
                public Boolean UseSkipTime;
                [JsonProperty(LanguageEn ? "Select the type of time-of-day skip (0 - Skip day, 1 - Skip night)" : "Выберите тип пропуска времени суток (0 - Пропускать день, 1 - Пропускать ночь)(Не забудьте включить возможность полного пропуска времени суток)")]
                public SkipType TypeSkipped;
                internal class EventSettings
                {
                    [JsonProperty(LanguageEn ? "Helicopter spawn custom settings" : "Кастомные настройки спавна вертолета")]
                    public Setting HelicopterSetting;
                    [JsonProperty(LanguageEn ? "Custom tank spawn settings" : "Кастомные настройки спавна танка")]
                    public Setting BreadlaySetting;
                    [JsonProperty(LanguageEn ? "Custom ship spawn settings" : "Кастомные настройки спавна корабля")]
                    public Setting CargoShipSetting;
                    [JsonProperty(LanguageEn ? "Airdrop spawn custom settings" : "Кастомные настройки спавна аирдропа")]
                    public Setting CargoPlaneSetting;
                    [JsonProperty(LanguageEn ? "Chinook custom spawn settings" : "Кастомные настройки спавна чинука")]
                    public Setting ChinoockSetting;
                    internal class Setting
                    {
                        [JsonProperty(LanguageEn ? "Use notifications for running events (configurable in lang)" : "Использовать уведомления у запущенных ивентах (настраивается в lang)")]
                        public Boolean useGameTip;
                        [JsonProperty(LanguageEn ? "Completely disable event spawning on the server (true - yes/false - no)" : "Полностью отключить спавн ивента на сервере(true - да/false - нет)")]
                        public Boolean FullOff;
                        [JsonProperty(LanguageEn ? "Enable custom spawn event (true - yes/false - no)" : "Включить кастомный спавн ивент(true - да/false - нет)")]
                        public Boolean UseEventCustom;
                        [JsonProperty(LanguageEn ? "Static event spawn time" : "Статическое время спавна ивента")]
                        public Int32 EventSpawnTime;
                        [JsonProperty(LanguageEn ? "Random spawn time settings" : "Настройки случайного времени спавна")]
                        public RandomingTime RandomTimeSpawn;
                        internal class RandomingTime
                        {
                            [JsonProperty(LanguageEn ? "Use random event spawn time (static time will not be taken into account) (true - yes/false - no)" : "Использовать случайное время спавно ивента(статическое время не будет учитываться)(true - да/false - нет)")]
                            public Boolean UseRandomTime;
                            [JsonProperty(LanguageEn ? "Minimum event spawn value" : "Минимальное значение спавна ивента")]
                            public Int32 MinEventSpawnTime;
                            [JsonProperty(LanguageEn ? "Max event spawn value" : "Максимальное значении спавна ивента")]
                            public Int32 MaxEventSpawnTime;
                        }

                    }

                }

            }

            internal class Rates
            {
                [JsonProperty(LanguageEn ? "Ranking setting during the day" : "Настройка рейтинга днем")]
                public AllRates DayRates;
                [JsonProperty(LanguageEn ? "Setting the rating at night" : "Настройка рейтинга ночью")]
                public AllRates NightRates;
                [JsonProperty(LanguageEn ? "Setting privileges and ratings specifically for them [iqrates.vip] = { Setting } (Descending)" : "Настройка привилегий и рейтингов конкретно для них [iqrates.vip] = { Настройка } (По убыванию)")]
                public Dictionary<String, DayAnNightRate> PrivilegyRates;
                [JsonProperty(LanguageEn ? "Setting up rating increase by days of the week" : "Настройка увеличения рейтинга по дням недели")]
                public RateControllerDayOfWeek rateControllerDayOfWeek;
                internal class RateControllerDayOfWeek
                {
                    [JsonProperty(LanguageEn ? "Using the function to increase ratings based on the days of the week (time and days are taken from your server machine)" : "Использовать функцию увеличения рейтинга по дням недели (время и дни берутся с вашей серверной машины)")]
                    public Boolean useRateBonusDayOfWeek;
                    [JsonProperty(LanguageEn ? "Setting the days of the week and time intervals (make sure that different intervals do not overlap with each other, otherwise it may lead to conflicts)" : "Настройка дней недели и промежутка времени (учтите, чтобы разные промежутка не пересекались друг с другом, иначе это может привести к конфликту)")]
                    public List<RateBonusDays> rateBonusDayOfWeek;
                    internal class RateBonusDays
                    {
                        [JsonProperty(LanguageEn ? "How much additional rating will be added to the xN player? For example, if a player has x4, and this parameter is 1.5, the player's rating will be x5.5" : "Сколько будет добавлено дополнительного рейтинга к xN игрока (Например : у игрока х4, данный параметр равен 1.5, в результате у игрока будет x5.5)")]
                        public Single upBonusRate;
                        [JsonProperty(LanguageEn ? "How much additional rating will be added to the xN player, based on permissions (Permission = xN)" : "Сколько будет добавлено дополнительного рейтинга к xN игрока, по привилегиям (Permission = xN)")]
                        public Dictionary<String, Single> privilageUpRates;
                        [JsonProperty(LanguageEn ? "Configuration of the day and time for starting the bonus rating" : "Настройка дня и времени запуска бонусного рейтинга")]
                        public TimeController timeStartBonus;
                        [JsonProperty(LanguageEn ? "Configuration of the day and time for the end of the bonus rating" : "Настройка дня и времени заверешения бонусного рейтинга")]
                        public TimeController timeStopBonus;
                        internal class TimeController
                        {
                            [JsonProperty(LanguageEn ? "Specify the hours (24-hour format)" : "Укажите часы (Формат 24 часа)")]
                            public Int32 timeHours;
                            [JsonProperty(LanguageEn ? "Day of the week: Monday, Tuesday, Wednesday, Thursday, Friday, Saturday, Sunday" : "День недели : Monday, Tuesday, Wednesday, Thursday, Friday, Saturday, Sunday")]
                            public String dayOfWeek;
                        }

                    }

                }

                [JsonProperty(LanguageEn ? "Setting custom rates (items) by permission - setting (Descending)" : "Настройка кастомных рейтов(предметов) по пермишенсу - настройка (По убыванию)")]
                public PermissionsRate CustomRatesPermissions;
                [JsonProperty(LanguageEn ? "Select the type of sheet to use: 0 - White sheet, 1 - Black sheet (White sheet - the ratings of only those items that are in it increase, the rest are ignored | The Black sheet is completely the opposite)"
                                             : "Выберите тип используемого листа : 0 - Белый лист, 1 - Черный лист (Белый лист - увеличиваются рейтинги только тех предметов - которые в нем, остальные игнорируются | Черный лист полностью наоборот)")]
                public TypeListUsed TypeList;
                [JsonProperty(LanguageEn ? "Black list of items that will not be categorically affected by the rating" : "Черный лист предметов,на которые катигорично не будут действовать рейтинг")]
                public List<String> BlackList;
                [JsonProperty(LanguageEn ? "A white list of items that will ONLY be affected by ratings - the rest is ignored" : "Белый лист предметов, на которые ТОЛЬКО будут действовать рейтинги - остальное игнорируются")]
                public List<String> WhiteList;
                [JsonProperty(LanguageEn ? "A blacklist of categories that will NOT be affected by ratings" : "Черный список категорий на которые НЕ БУДУТ действовать рейтинги")]
                public List<String> BlackListCategory;
                [JsonProperty(LanguageEn ? "Use a tea controller? (Works on scrap, ore and wood tea). If enabled, it will set % to production due to the difference between rates (standard / privileges)" : "Использовать контроллер чая? (Работае на скрап, рудный и древесный чай). Если включено - будет устанавливать % к добычи за счет разницы между рейтами (стандартный / привилегии)")]
                public Boolean UseTeaController;
                [JsonProperty(LanguageEn ? "Use a prefabs blacklist?" : "Использовать черный лист префабов?")]
                public Boolean UseBlackListPrefabs;
                [JsonProperty(LanguageEn ? "Black list of prefabs(ShortPrefabName) - which will not be affected by ratings" : "Черный лист префабов(ShortPrefabName) - на которые не будут действовать рейтинги")]
                public List<String> BlackListPrefabs;
                [JsonProperty(LanguageEn ? "Enable melting speed in furnaces (true - yes/false - no)" : "Включить скорость плавки в печах(true - да/false - нет)")]
                public Boolean UseSpeedBurnable;
                [JsonProperty(LanguageEn ? "Smelting Fuel Usage Rating (If the list is enabled, this value will be the default value for all non-licensed)" : "Рейтинг использования топлива при переплавки(Если включен список - это значение будет стандартное для всех у кого нет прав)")]
                public Int32 SpeedFuelBurnable;
                [JsonProperty(LanguageEn ? "Use a blacklist of items for melting?" : "Использовать черный список предметов для плавки?")]
                public Boolean UseBlackListBurnable;
                [JsonProperty(LanguageEn ? "A black list of items for the stove, which will not be categorically affected by melting" : "Черный лист предметов для печки,на которые катигорично не будут действовать плавка")]
                public List<String> BlackListBurnable;
                [JsonProperty(LanguageEn ? "Furnace smelting speed (If the list is enabled, this value will be the default for everyone who does not have rights)" : "Скорость плавки печей(Если включен список - это значение будет стандартное для всех у кого нет прав)")]
                public Single SpeedBurnable;
                [JsonProperty(LanguageEn ? "Enable list of melting speed in furnaces (true - yes/false - no)" : "Включить список скорости плавки в печах(true - да/false - нет)")]
                public Boolean UseSpeedBurnableList;
                [JsonProperty(LanguageEn ? "Setting the melting speed in furnaces by privileges" : "Настройка скорости плавки в печах по привилегиям")]
                public List<SpeedBurnablePreset> SpeedBurableList;
                [JsonProperty(LanguageEn ? "Enable tea mixing acceleration in the mixing table (true - yes/false - no)" : "Включить ускорение смешивания чая в столе смешивания (true - да/false - нет)")]
                public Boolean useSpeedMixingTable;
                [JsonProperty(LanguageEn ? "Setting the tea mixing speed" : "Настройка скорости смешивания чая")]
                public List<SpeedMixingTable> speedMixingTables;
                [JsonProperty(LanguageEn ? "A blacklist of prefabs (ShortPrefabName) that will not be affected by acceleration (example: campfire) [If you don't need it, leave it empty]" : "Черный список префабов(ShortPrefabName), на которые не будет действовать ускорение (пример : campfire) [Если вам не нужно - оставьте пустым]")]
                public List<String> IgnoreSpeedBurnablePrefabList;
                internal class DayAnNightRate
                {
                    [JsonProperty(LanguageEn ? "Ranking setting during the day" : "Настройка рейтинга днем")]
                    public AllRates DayRates;
                    [JsonProperty(LanguageEn ? "Setting the rating at night" : "Настройка рейтинга ночью")]
                    public AllRates NightRates;
                }

                [JsonProperty(LanguageEn ? "Setting up a recycler" : "Настройка переработчика")]
                public RecyclerController RecyclersController;
                internal class RecyclerController
                {
                    [JsonProperty(LanguageEn ? "Use the processing time editing functions" : "Использовать функции редактирования времени переработки")]
                    public Boolean UseRecyclerSpeed;
                    [JsonProperty(LanguageEn ? "Static processing time (in seconds) (Will be set according to the standard or if the player does not have the privilege) (Default = 5s)" : "Статичное время переработки (в секундах) (Будет установлено по стандарту или если у игрока нет привилеии) (По умолчанию = 5с)")]
                    public Single DefaultSpeedRecycler;
                    [JsonProperty(LanguageEn ? "Setting the processing time for privileges (adjust from greater privilege to lesser privilege)" : "Настройка времени переработки для привилегий (настраивать от большей привилегии к меньшей)")]
                    public List<PresetRecycler> PrivilageSpeedRecycler;
                    internal class PresetRecycler
                    {
                        [JsonProperty(LanguageEn ? "Permissions" : "Права")]
                        public String Permissions;
                        [JsonProperty(LanguageEn ? "Standard processing time (in seconds)" : "Стандартное время переработки (в секундах)")]
                        public Single SpeedRecyclers;
                    }

                }

                internal class SpeedBurnablePreset
                {
                    [JsonProperty(LanguageEn ? "Permissions" : "Права")]
                    public String Permissions;
                    [JsonProperty(LanguageEn ? "Furnace melting speed" : "Скорость плавки печей")]
                    public Single SpeedBurnable;
                    [JsonProperty(LanguageEn ? "Smelting Fuel Use Rating" : "Рейтинг использования топлива при переплавки")]
                    public Int32 SpeedFuelBurnable;
                }

                internal class SpeedMixingTable
                {
                    [JsonProperty(LanguageEn ? "Permissions" : "Права")]
                    public String Permissions;
                    [JsonProperty(LanguageEn ? "Mixing speed (0-100%)" : "Скорость смешивания (0-100%)")]
                    public Int32 SpeedMixing;
                }

                internal class PermissionsRate
                {
                    [JsonProperty(LanguageEn ? "Ranking setting during the day" : "Настройка рейтинга днем")]
                    public Dictionary<String, List<PermissionsRateDetalis>> DayRates;
                    [JsonProperty(LanguageEn ? "Setting the rating at night" : "Настройка рейтинга ночью")]
                    public Dictionary<String, List<PermissionsRateDetalis>> NightRates;
                    public class PermissionsRateDetalis
                    {
                        [JsonProperty(LanguageEn ? "Shortname" : "Shortname")]
                        public String Shortname;
                        [JsonProperty(LanguageEn ? "Rate" : "Рейтинг")]
                        public Single Rate;
                    }

                }

                internal class AllRates
                {
                    [JsonProperty(LanguageEn ? "Rating of extracted resources" : "Рейтинг добываемых ресурсов")]
                    public Single GatherRate;
                    [JsonProperty(LanguageEn ? "Rating of found items" : "Рейтинг найденных предметов")]
                    public Single LootRate;
                    [JsonProperty(LanguageEn ? "Pickup Rating" : "Рейтинг поднимаемых предметов")]
                    public Single PickUpRate;
                    [JsonProperty(LanguageEn ? "Rating of plants raised from the beds" : "Рейтинг поднимаемых растений с грядок")]
                    public Single GrowableRate;
                    [JsonProperty(LanguageEn ? "Quarry rating" : "Рейтинг карьеров")]
                    public Single QuarryRate;
                    [JsonProperty(LanguageEn ? "Detailed rating settings in the career" : "Детальная настройка рейтинга в карьере")]
                    public QuarryRateDetalis QuarryDetalis;
                    [JsonProperty(LanguageEn ? "Excavator Rating" : "Рейтинг экскаватора")]
                    public Single ExcavatorRate;
                    [JsonProperty(LanguageEn ? "Coal drop chance" : "Шанс выпадения угля")]
                    public Single CoalRare;
                    [JsonProperty(LanguageEn ? "Rating of items caught from the sea (fishing)" : "Рейтинг предметов выловленных с моря (рыбалки)")]
                    public Single FishRate;
                    internal class QuarryRateDetalis
                    {
                        [JsonProperty(LanguageEn ? "Use the detailed setting of the career rating (otherwise the 'Career Rating' will be taken for all subjects)" : "Использовать детальную настройку рейтинга карьеров (иначе будет браться 'Рейтинг карьеров' для всех предметов)")]
                        public Boolean UseDetalisRateQuarry;
                        [JsonProperty(LanguageEn ? "The item dropped out of the career - rating" : "Предмет выпадаемый из карьера - рейтинг")]
                        public Dictionary<String, Single> ShortnameListQuarry;
                    }

                }

            }

            [JsonProperty(LanguageEn ? "Configuring supported plugins" : "Настройка поддерживаемых плагинов")]
            public ReferencePlugin ReferenceSettings;
        }

    }

     object OnContainerDropGrowable(ItemContainer container, Item item);
    private Configuration.PluginSettings.Rates.RateControllerDayOfWeek.RateBonusDays rateDayOfWeek;
    protected override void LoadConfig();
    private void GetTimeComponent();
     bool IsBlackList(string Shortname);
    private MonumentInfo SpacePort;
    private object OnOvenToggle(BaseOven oven, BasePlayer player);
     bool IsWhiteList(string Shortname);
    private void OnEntitySpawned(BaseSubmarine submarine);
    private void OnEntitySpawned(MotorRowboat boat);
     Single GetRateQuarryDetalis(Configuration.PluginSettings.Rates.AllRates Rates, String Shortname);
     void OnHour();
    private void SpawnTank();
    private Single GetBonusRateDayOfWeek(BasePlayer player);
    private Boolean sendMessageNight;
    private void OnEntitySpawned(HotAirBalloon hotAirBalloon);
    private IEnumerator InitializeTransport();
     int API_CONVERT(Int32 RateType, string Shortname, float Amount, BasePlayer player);
     void SpawnPlane();
    private void StartCargoShip(Configuration.PluginSettings.OtherSettings.EventSettings EventSettings);
    private void OnRecyclerToggle(Recycler recycler, BasePlayer player);
    private readonly Dictionary<String, ModiferTea> TeaModifers;
     void OnLootEntity(BasePlayer player, BaseEntity entity);
     object OnDispenserGather(ResourceDispenser dispenser, BaseEntity entity, Item item);
    private Boolean sendMessageDay;
    private void MessageGameTipsError(String langKey);
     void ReadData();
     void StartEvent();
     int API_CONVERT_GATHER(string Shortname, float Amount, BasePlayer player);
    private void OnEntitySpawned(AttackHelicopter helicopter);
     void OnSunrise();
    private Int32 GetRandomTime(Int32 Min, Int32 Max);
    private Single GetSpeedRecycler(BasePlayer player);
    private object OnExcavatorGather(ExcavatorArm arm, Item item);
    private object OnOvenStart(BaseOven oven);
     void WriteData();
     void OnFuelConsume(BaseOven oven, Item fuel, ItemModBurnable burnable);
     void OnCollectiblePickup(CollectibleEntity collectible, BasePlayer player);
    private Coroutine initializeTransport;
    private Configuration.PluginSettings.Rates.RateControllerDayOfWeek.RateBonusDays GetRateBonusForCurrentTime();
    private void StartChinoock(Configuration.PluginSettings.OtherSettings.EventSettings EventSettings);
    private String IsValidConfigDayOfWeek();
    protected override void SaveConfig();
    private ModifierDefintion GetDefintionModifer(Modifier.ModifierType Type, Single Duration, Single Value);
    private const string prefabPlane;
    private bool IsOverlapping(int start1, int end1, int start2, int end2);
     void OnExcavatorResourceSet(ExcavatorArm arm, string resourceName, BasePlayer player);
    private Single GetSpeeedMixingTable(BasePlayer player);
     void API_BONUS_RATE_ADDPLAYER(UInt64 userID, Single Rate);
    public void Register(string Permissions);
    private Boolean activatedDay;
     bool IsBlackListBurnable(string Shortname);
    private int GetTotalHoursFromStartOfWeek(string dayOfWeek, int hour);
    private new void LoadDefaultMessages();
    private Object OnEventTrigger(TriggeredEventPrefab info);
    public Single GetMultiplaceBurnableSpeed(String ownerid);
    private void Unload();
    public Dictionary<BasePlayer, Single> BonusRates;
    private void OnEntitySpawned(RHIB boat);
     void OnSwitchToggled(IOEntity entity, BasePlayer player);
    public static IQRates _;
    private const string prefabShip;
}

private class OvenController : FacepunchBehaviour
{
    private static readonly Dictionary<BaseOven, OvenController> Controllers;
    private BaseOven _oven;
    private float _speed;
    private string _ownerId;
    private Int32 _ticks;
    private Int32 _speedFuel;
    private bool IsFurnace { get; set; }
    private void Awake();
    public object Switch(BasePlayer player);
    public void TryRestart();
    private void Kill();
    public static OvenController GetOrAdd(BaseOven oven);
    public static void TryRestartAll();
    public static void KillAll();
    public void OnDestroy();
    private void StartCooking();
    private void StopCooking();
    public void Cook();
    private void SmeltItems();
}

public class ModiferTea
{
    public Modifier.ModifierType Type;
    public Single Duration;
    public Single Value;
}

private class Configuration
{
    [JsonProperty(LanguageEn ? "Plugin setup" : "Настройка плагина")]
    public PluginSettings pluginSettings;
    public static Configuration GetNewConfiguration();
    internal class PluginSettings
    {
        internal class ReferencePlugin
        {
            internal class IQChatReference
            {
                [JsonProperty(LanguageEn ? "IQChat : Custom chat avatar (If required)" : "IQChat : Кастомный аватар в чате(Если требуется)")]
                public String CustomAvatar;
                [JsonProperty(LanguageEn ? "IQChat : Use UI Notifications" : "IQChat : Использовать UI уведомления")]
                public Boolean UIAlertUse;
                [JsonProperty(LanguageEn ? "IQChat : Custom prefix in chat" : "IQChat : Кастомный префикс в чате")]
                public String CustomPrefix;
            }

            [JsonProperty(LanguageEn ? "Setting up IQChat" : "Настройка IQChat")]
            public IQChatReference IQChatSetting;
        }

        [JsonProperty(LanguageEn ? "Rating settings" : "Настройка рейтингов")]
        public Rates RateSetting;
        [JsonProperty(LanguageEn ? "Additional plugin settings" : "Дополнительная настройка плагина")]
        public OtherSettings OtherSetting;
        internal class OtherSettings
        {
            [JsonProperty(LanguageEn ? "Event settings on the server" : "Настройки ивентов на сервере")]
            public EventSettings EventSetting;
            [JsonProperty(LanguageEn ? "Fuel settings when buying vehicles from NPCs" : "Настройки топлива при покупке транспорта у NPC")]
            public FuelSettings FuelSetting;
            [JsonProperty(LanguageEn ? "Fuel Consumption Rating Settings" : "Настройки рейтинга потребления топлива")]
            public FuelConsumedTransport FuelConsumedTransportSetting;
            internal class FuelConsumedTransport
            {
                [JsonProperty(LanguageEn ? "Use the fuel consumption rating in transport (true - yes/false - no)" : "Использовать рейтинг потребления топлива в транспорте (true - да/false - нет)")]
                public Boolean useConsumedFuel;
                [JsonProperty(LanguageEn ? "Hotairballoon fuel consumption rating (Default = 0.25)" : "Рейтинг потребление топлива у воздушного шара (Стандартно = 0.25)")]
                public Single ConsumedHotAirBalloon;
                [JsonProperty(LanguageEn ? "Snowmobile fuel consumption rating (Default = 0.15)" : "Рейтинг потребление топлива снегоходов (Стандартно = 0.15)")]
                public Single ConsumedSnowmobile;
                [JsonProperty(LanguageEn ? "Train fuel consumption rating (Default = 0.15)" : "Рейтинг потребление топлива поездов (Стандартно = 0.15)")]
                public Single ConsumedTrain;
                [JsonProperty(LanguageEn ? "Rowboat fuel consumption rating (Default = 0.25)" : "Рейтинг потребление топлива у лодок (Стандартно = 0.25)")]
                public Single ConsumedBoat;
                [JsonProperty(LanguageEn ? "Submarine fuel consumption rating (Default = 0.15)" : "Рейтинг потребление топлива у субмарин (Стандартно = 0.15)")]
                public Single ConsumedSubmarine;
                [JsonProperty(LanguageEn ? "Minicopter fuel consumption rating (Default = 0.25)" : "Рейтинг потребление топлива у миникоптера (Стандартно = 0.25)")]
                public Single ConsumedCopter;
                [JsonProperty(LanguageEn ? "ScrapTransportHelicopter fuel consumption rating (Default = 0.25)" : "Рейтинг потребление топлива у коровы (Стандартно = 0.25)")]
                public Single ConsumedScrapTransport;
                [JsonProperty(LanguageEn ? "Attack-Helicopter fuel consumption rating (Default = 0.25)" : "Рейтинг потребление топлива у боевого-вертолета (Стандартно = 0.25)")]
                public Single ConsumedAttackHelicopter;
            }

            internal class FuelSettings
            {
                [JsonProperty(LanguageEn ? "Use fuel replenishment when buying vehicles from NPC (true - yes/false - no)" : "Использовать пополнение топлива при покупке транспорта у NPC (true - да/false - нет)")]
                public Boolean useAutoFillFuel;
                [JsonProperty(LanguageEn ? "Amount of fuel for boats" : "Кол-во топлива у лодок")]
                public Int32 AmountBoat;
                [JsonProperty(LanguageEn ? "The amount of fuel in submarines" : "Кол-во топлива у подводных лодок")]
                public Int32 AmountSubmarine;
                [JsonProperty(LanguageEn ? "Minicopter fuel quantity" : "Кол-во топлива у миникоптера")]
                public Int32 AmountMinicopter;
                [JsonProperty(LanguageEn ? "Helicopter fuel quantity" : "Кол-во топлива у вертолета")]
                public Int32 AmountScrapTransport;
                [JsonProperty(LanguageEn ? "Attack-Helicopter fuel quantity" : "Кол-во топлива у боевого вертолета")]
                public Int32 AmountAttackHelicopter;
            }

            [JsonProperty(LanguageEn ? "Use Time Acceleration" : "Использовать ускорение времени")]
            public Boolean UseTime;
            [JsonProperty(LanguageEn ? "Use time freeze (the time will be the one you set in the item &lt;Frozen time on the server&gt;)" : "Использовать заморозку времени(время будет такое, какое вы установите в пунке <Замороженное время на сервере>)")]
            public Boolean UseFreezeTime;
            [JsonProperty(LanguageEn ? "Frozen time on the server (Set time that will not change and be forever on the server, must be true on &lt;Use time freeze&gt;" : "Замороженное время на сервере (Установите время, которое не будет изменяться и будет вечно на сервере, должен быть true на <Использовать заморозку времени>")]
            public Int32 FreezeTime;
            [JsonProperty(LanguageEn ? "What time will the day start?" : "Укажите во сколько будет начинаться день")]
            public Int32 DayStart;
            [JsonProperty(LanguageEn ? "What time will the night start?" : "Укажите во сколько будет начинаться ночь")]
            public Int32 NightStart;
            [JsonProperty(LanguageEn ? "Specify how long the day will be in minutes" : "Укажите сколько будет длится день в минутах")]
            public Int32 DayTime;
            [JsonProperty(LanguageEn ? "Specify how long the night will last in minutes" : "Укажите сколько будет длится ночь в минутах")]
            public Int32 NightTime;
            [JsonProperty(LanguageEn ? "Use notification of players about the change of day and night (switching rates. The message is configured in the lang)" : "Использовать уведомление игроков о смене дня и ночи (переключение рейтов. Сообщение настраивается в лэнге)")]
            public Boolean UseAlertDayNight;
            [JsonProperty(LanguageEn ? "Enable the ability to completely skip the time of day (selected in the paragraph below)" : "Включить возможность полного пропуска времени суток(выбирается в пункте ниже)")]
            public Boolean UseSkipTime;
            [JsonProperty(LanguageEn ? "Select the type of time-of-day skip (0 - Skip day, 1 - Skip night)" : "Выберите тип пропуска времени суток (0 - Пропускать день, 1 - Пропускать ночь)(Не забудьте включить возможность полного пропуска времени суток)")]
            public SkipType TypeSkipped;
            internal class EventSettings
            {
                [JsonProperty(LanguageEn ? "Helicopter spawn custom settings" : "Кастомные настройки спавна вертолета")]
                public Setting HelicopterSetting;
                [JsonProperty(LanguageEn ? "Custom tank spawn settings" : "Кастомные настройки спавна танка")]
                public Setting BreadlaySetting;
                [JsonProperty(LanguageEn ? "Custom ship spawn settings" : "Кастомные настройки спавна корабля")]
                public Setting CargoShipSetting;
                [JsonProperty(LanguageEn ? "Airdrop spawn custom settings" : "Кастомные настройки спавна аирдропа")]
                public Setting CargoPlaneSetting;
                [JsonProperty(LanguageEn ? "Chinook custom spawn settings" : "Кастомные настройки спавна чинука")]
                public Setting ChinoockSetting;
                internal class Setting
                {
                    [JsonProperty(LanguageEn ? "Use notifications for running events (configurable in lang)" : "Использовать уведомления у запущенных ивентах (настраивается в lang)")]
                    public Boolean useGameTip;
                    [JsonProperty(LanguageEn ? "Completely disable event spawning on the server (true - yes/false - no)" : "Полностью отключить спавн ивента на сервере(true - да/false - нет)")]
                    public Boolean FullOff;
                    [JsonProperty(LanguageEn ? "Enable custom spawn event (true - yes/false - no)" : "Включить кастомный спавн ивент(true - да/false - нет)")]
                    public Boolean UseEventCustom;
                    [JsonProperty(LanguageEn ? "Static event spawn time" : "Статическое время спавна ивента")]
                    public Int32 EventSpawnTime;
                    [JsonProperty(LanguageEn ? "Random spawn time settings" : "Настройки случайного времени спавна")]
                    public RandomingTime RandomTimeSpawn;
                    internal class RandomingTime
                    {
                        [JsonProperty(LanguageEn ? "Use random event spawn time (static time will not be taken into account) (true - yes/false - no)" : "Использовать случайное время спавно ивента(статическое время не будет учитываться)(true - да/false - нет)")]
                        public Boolean UseRandomTime;
                        [JsonProperty(LanguageEn ? "Minimum event spawn value" : "Минимальное значение спавна ивента")]
                        public Int32 MinEventSpawnTime;
                        [JsonProperty(LanguageEn ? "Max event spawn value" : "Максимальное значении спавна ивента")]
                        public Int32 MaxEventSpawnTime;
                    }

                }

            }

        }

        internal class Rates
        {
            [JsonProperty(LanguageEn ? "Ranking setting during the day" : "Настройка рейтинга днем")]
            public AllRates DayRates;
            [JsonProperty(LanguageEn ? "Setting the rating at night" : "Настройка рейтинга ночью")]
            public AllRates NightRates;
            [JsonProperty(LanguageEn ? "Setting privileges and ratings specifically for them [iqrates.vip] = { Setting } (Descending)" : "Настройка привилегий и рейтингов конкретно для них [iqrates.vip] = { Настройка } (По убыванию)")]
            public Dictionary<String, DayAnNightRate> PrivilegyRates;
            [JsonProperty(LanguageEn ? "Setting up rating increase by days of the week" : "Настройка увеличения рейтинга по дням недели")]
            public RateControllerDayOfWeek rateControllerDayOfWeek;
            internal class RateControllerDayOfWeek
            {
                [JsonProperty(LanguageEn ? "Using the function to increase ratings based on the days of the week (time and days are taken from your server machine)" : "Использовать функцию увеличения рейтинга по дням недели (время и дни берутся с вашей серверной машины)")]
                public Boolean useRateBonusDayOfWeek;
                [JsonProperty(LanguageEn ? "Setting the days of the week and time intervals (make sure that different intervals do not overlap with each other, otherwise it may lead to conflicts)" : "Настройка дней недели и промежутка времени (учтите, чтобы разные промежутка не пересекались друг с другом, иначе это может привести к конфликту)")]
                public List<RateBonusDays> rateBonusDayOfWeek;
                internal class RateBonusDays
                {
                    [JsonProperty(LanguageEn ? "How much additional rating will be added to the xN player? For example, if a player has x4, and this parameter is 1.5, the player's rating will be x5.5" : "Сколько будет добавлено дополнительного рейтинга к xN игрока (Например : у игрока х4, данный параметр равен 1.5, в результате у игрока будет x5.5)")]
                    public Single upBonusRate;
                    [JsonProperty(LanguageEn ? "How much additional rating will be added to the xN player, based on permissions (Permission = xN)" : "Сколько будет добавлено дополнительного рейтинга к xN игрока, по привилегиям (Permission = xN)")]
                    public Dictionary<String, Single> privilageUpRates;
                    [JsonProperty(LanguageEn ? "Configuration of the day and time for starting the bonus rating" : "Настройка дня и времени запуска бонусного рейтинга")]
                    public TimeController timeStartBonus;
                    [JsonProperty(LanguageEn ? "Configuration of the day and time for the end of the bonus rating" : "Настройка дня и времени заверешения бонусного рейтинга")]
                    public TimeController timeStopBonus;
                    internal class TimeController
                    {
                        [JsonProperty(LanguageEn ? "Specify the hours (24-hour format)" : "Укажите часы (Формат 24 часа)")]
                        public Int32 timeHours;
                        [JsonProperty(LanguageEn ? "Day of the week: Monday, Tuesday, Wednesday, Thursday, Friday, Saturday, Sunday" : "День недели : Monday, Tuesday, Wednesday, Thursday, Friday, Saturday, Sunday")]
                        public String dayOfWeek;
                    }

                }

            }

            [JsonProperty(LanguageEn ? "Setting custom rates (items) by permission - setting (Descending)" : "Настройка кастомных рейтов(предметов) по пермишенсу - настройка (По убыванию)")]
            public PermissionsRate CustomRatesPermissions;
            [JsonProperty(LanguageEn ? "Select the type of sheet to use: 0 - White sheet, 1 - Black sheet (White sheet - the ratings of only those items that are in it increase, the rest are ignored | The Black sheet is completely the opposite)"
                                             : "Выберите тип используемого листа : 0 - Белый лист, 1 - Черный лист (Белый лист - увеличиваются рейтинги только тех предметов - которые в нем, остальные игнорируются | Черный лист полностью наоборот)")]
            public TypeListUsed TypeList;
            [JsonProperty(LanguageEn ? "Black list of items that will not be categorically affected by the rating" : "Черный лист предметов,на которые катигорично не будут действовать рейтинг")]
            public List<String> BlackList;
            [JsonProperty(LanguageEn ? "A white list of items that will ONLY be affected by ratings - the rest is ignored" : "Белый лист предметов, на которые ТОЛЬКО будут действовать рейтинги - остальное игнорируются")]
            public List<String> WhiteList;
            [JsonProperty(LanguageEn ? "A blacklist of categories that will NOT be affected by ratings" : "Черный список категорий на которые НЕ БУДУТ действовать рейтинги")]
            public List<String> BlackListCategory;
            [JsonProperty(LanguageEn ? "Use a tea controller? (Works on scrap, ore and wood tea). If enabled, it will set % to production due to the difference between rates (standard / privileges)" : "Использовать контроллер чая? (Работае на скрап, рудный и древесный чай). Если включено - будет устанавливать % к добычи за счет разницы между рейтами (стандартный / привилегии)")]
            public Boolean UseTeaController;
            [JsonProperty(LanguageEn ? "Use a prefabs blacklist?" : "Использовать черный лист префабов?")]
            public Boolean UseBlackListPrefabs;
            [JsonProperty(LanguageEn ? "Black list of prefabs(ShortPrefabName) - which will not be affected by ratings" : "Черный лист префабов(ShortPrefabName) - на которые не будут действовать рейтинги")]
            public List<String> BlackListPrefabs;
            [JsonProperty(LanguageEn ? "Enable melting speed in furnaces (true - yes/false - no)" : "Включить скорость плавки в печах(true - да/false - нет)")]
            public Boolean UseSpeedBurnable;
            [JsonProperty(LanguageEn ? "Smelting Fuel Usage Rating (If the list is enabled, this value will be the default value for all non-licensed)" : "Рейтинг использования топлива при переплавки(Если включен список - это значение будет стандартное для всех у кого нет прав)")]
            public Int32 SpeedFuelBurnable;
            [JsonProperty(LanguageEn ? "Use a blacklist of items for melting?" : "Использовать черный список предметов для плавки?")]
            public Boolean UseBlackListBurnable;
            [JsonProperty(LanguageEn ? "A black list of items for the stove, which will not be categorically affected by melting" : "Черный лист предметов для печки,на которые катигорично не будут действовать плавка")]
            public List<String> BlackListBurnable;
            [JsonProperty(LanguageEn ? "Furnace smelting speed (If the list is enabled, this value will be the default for everyone who does not have rights)" : "Скорость плавки печей(Если включен список - это значение будет стандартное для всех у кого нет прав)")]
            public Single SpeedBurnable;
            [JsonProperty(LanguageEn ? "Enable list of melting speed in furnaces (true - yes/false - no)" : "Включить список скорости плавки в печах(true - да/false - нет)")]
            public Boolean UseSpeedBurnableList;
            [JsonProperty(LanguageEn ? "Setting the melting speed in furnaces by privileges" : "Настройка скорости плавки в печах по привилегиям")]
            public List<SpeedBurnablePreset> SpeedBurableList;
            [JsonProperty(LanguageEn ? "Enable tea mixing acceleration in the mixing table (true - yes/false - no)" : "Включить ускорение смешивания чая в столе смешивания (true - да/false - нет)")]
            public Boolean useSpeedMixingTable;
            [JsonProperty(LanguageEn ? "Setting the tea mixing speed" : "Настройка скорости смешивания чая")]
            public List<SpeedMixingTable> speedMixingTables;
            [JsonProperty(LanguageEn ? "A blacklist of prefabs (ShortPrefabName) that will not be affected by acceleration (example: campfire) [If you don't need it, leave it empty]" : "Черный список префабов(ShortPrefabName), на которые не будет действовать ускорение (пример : campfire) [Если вам не нужно - оставьте пустым]")]
            public List<String> IgnoreSpeedBurnablePrefabList;
            internal class DayAnNightRate
            {
                [JsonProperty(LanguageEn ? "Ranking setting during the day" : "Настройка рейтинга днем")]
                public AllRates DayRates;
                [JsonProperty(LanguageEn ? "Setting the rating at night" : "Настройка рейтинга ночью")]
                public AllRates NightRates;
            }

            [JsonProperty(LanguageEn ? "Setting up a recycler" : "Настройка переработчика")]
            public RecyclerController RecyclersController;
            internal class RecyclerController
            {
                [JsonProperty(LanguageEn ? "Use the processing time editing functions" : "Использовать функции редактирования времени переработки")]
                public Boolean UseRecyclerSpeed;
                [JsonProperty(LanguageEn ? "Static processing time (in seconds) (Will be set according to the standard or if the player does not have the privilege) (Default = 5s)" : "Статичное время переработки (в секундах) (Будет установлено по стандарту или если у игрока нет привилеии) (По умолчанию = 5с)")]
                public Single DefaultSpeedRecycler;
                [JsonProperty(LanguageEn ? "Setting the processing time for privileges (adjust from greater privilege to lesser privilege)" : "Настройка времени переработки для привилегий (настраивать от большей привилегии к меньшей)")]
                public List<PresetRecycler> PrivilageSpeedRecycler;
                internal class PresetRecycler
                {
                    [JsonProperty(LanguageEn ? "Permissions" : "Права")]
                    public String Permissions;
                    [JsonProperty(LanguageEn ? "Standard processing time (in seconds)" : "Стандартное время переработки (в секундах)")]
                    public Single SpeedRecyclers;
                }

            }

            internal class SpeedBurnablePreset
            {
                [JsonProperty(LanguageEn ? "Permissions" : "Права")]
                public String Permissions;
                [JsonProperty(LanguageEn ? "Furnace melting speed" : "Скорость плавки печей")]
                public Single SpeedBurnable;
                [JsonProperty(LanguageEn ? "Smelting Fuel Use Rating" : "Рейтинг использования топлива при переплавки")]
                public Int32 SpeedFuelBurnable;
            }

            internal class SpeedMixingTable
            {
                [JsonProperty(LanguageEn ? "Permissions" : "Права")]
                public String Permissions;
                [JsonProperty(LanguageEn ? "Mixing speed (0-100%)" : "Скорость смешивания (0-100%)")]
                public Int32 SpeedMixing;
            }

            internal class PermissionsRate
            {
                [JsonProperty(LanguageEn ? "Ranking setting during the day" : "Настройка рейтинга днем")]
                public Dictionary<String, List<PermissionsRateDetalis>> DayRates;
                [JsonProperty(LanguageEn ? "Setting the rating at night" : "Настройка рейтинга ночью")]
                public Dictionary<String, List<PermissionsRateDetalis>> NightRates;
                public class PermissionsRateDetalis
                {
                    [JsonProperty(LanguageEn ? "Shortname" : "Shortname")]
                    public String Shortname;
                    [JsonProperty(LanguageEn ? "Rate" : "Рейтинг")]
                    public Single Rate;
                }

            }

            internal class AllRates
            {
                [JsonProperty(LanguageEn ? "Rating of extracted resources" : "Рейтинг добываемых ресурсов")]
                public Single GatherRate;
                [JsonProperty(LanguageEn ? "Rating of found items" : "Рейтинг найденных предметов")]
                public Single LootRate;
                [JsonProperty(LanguageEn ? "Pickup Rating" : "Рейтинг поднимаемых предметов")]
                public Single PickUpRate;
                [JsonProperty(LanguageEn ? "Rating of plants raised from the beds" : "Рейтинг поднимаемых растений с грядок")]
                public Single GrowableRate;
                [JsonProperty(LanguageEn ? "Quarry rating" : "Рейтинг карьеров")]
                public Single QuarryRate;
                [JsonProperty(LanguageEn ? "Detailed rating settings in the career" : "Детальная настройка рейтинга в карьере")]
                public QuarryRateDetalis QuarryDetalis;
                [JsonProperty(LanguageEn ? "Excavator Rating" : "Рейтинг экскаватора")]
                public Single ExcavatorRate;
                [JsonProperty(LanguageEn ? "Coal drop chance" : "Шанс выпадения угля")]
                public Single CoalRare;
                [JsonProperty(LanguageEn ? "Rating of items caught from the sea (fishing)" : "Рейтинг предметов выловленных с моря (рыбалки)")]
                public Single FishRate;
                internal class QuarryRateDetalis
                {
                    [JsonProperty(LanguageEn ? "Use the detailed setting of the career rating (otherwise the 'Career Rating' will be taken for all subjects)" : "Использовать детальную настройку рейтинга карьеров (иначе будет браться 'Рейтинг карьеров' для всех предметов)")]
                    public Boolean UseDetalisRateQuarry;
                    [JsonProperty(LanguageEn ? "The item dropped out of the career - rating" : "Предмет выпадаемый из карьера - рейтинг")]
                    public Dictionary<String, Single> ShortnameListQuarry;
                }

            }

        }

        [JsonProperty(LanguageEn ? "Configuring supported plugins" : "Настройка поддерживаемых плагинов")]
        public ReferencePlugin ReferenceSettings;
    }

}

internal class PluginSettings
{
    internal class ReferencePlugin
    {
        internal class IQChatReference
        {
            [JsonProperty(LanguageEn ? "IQChat : Custom chat avatar (If required)" : "IQChat : Кастомный аватар в чате(Если требуется)")]
            public String CustomAvatar;
            [JsonProperty(LanguageEn ? "IQChat : Use UI Notifications" : "IQChat : Использовать UI уведомления")]
            public Boolean UIAlertUse;
            [JsonProperty(LanguageEn ? "IQChat : Custom prefix in chat" : "IQChat : Кастомный префикс в чате")]
            public String CustomPrefix;
        }

        [JsonProperty(LanguageEn ? "Setting up IQChat" : "Настройка IQChat")]
        public IQChatReference IQChatSetting;
    }

    [JsonProperty(LanguageEn ? "Rating settings" : "Настройка рейтингов")]
    public Rates RateSetting;
    [JsonProperty(LanguageEn ? "Additional plugin settings" : "Дополнительная настройка плагина")]
    public OtherSettings OtherSetting;
    internal class OtherSettings
    {
        [JsonProperty(LanguageEn ? "Event settings on the server" : "Настройки ивентов на сервере")]
        public EventSettings EventSetting;
        [JsonProperty(LanguageEn ? "Fuel settings when buying vehicles from NPCs" : "Настройки топлива при покупке транспорта у NPC")]
        public FuelSettings FuelSetting;
        [JsonProperty(LanguageEn ? "Fuel Consumption Rating Settings" : "Настройки рейтинга потребления топлива")]
        public FuelConsumedTransport FuelConsumedTransportSetting;
        internal class FuelConsumedTransport
        {
            [JsonProperty(LanguageEn ? "Use the fuel consumption rating in transport (true - yes/false - no)" : "Использовать рейтинг потребления топлива в транспорте (true - да/false - нет)")]
            public Boolean useConsumedFuel;
            [JsonProperty(LanguageEn ? "Hotairballoon fuel consumption rating (Default = 0.25)" : "Рейтинг потребление топлива у воздушного шара (Стандартно = 0.25)")]
            public Single ConsumedHotAirBalloon;
            [JsonProperty(LanguageEn ? "Snowmobile fuel consumption rating (Default = 0.15)" : "Рейтинг потребление топлива снегоходов (Стандартно = 0.15)")]
            public Single ConsumedSnowmobile;
            [JsonProperty(LanguageEn ? "Train fuel consumption rating (Default = 0.15)" : "Рейтинг потребление топлива поездов (Стандартно = 0.15)")]
            public Single ConsumedTrain;
            [JsonProperty(LanguageEn ? "Rowboat fuel consumption rating (Default = 0.25)" : "Рейтинг потребление топлива у лодок (Стандартно = 0.25)")]
            public Single ConsumedBoat;
            [JsonProperty(LanguageEn ? "Submarine fuel consumption rating (Default = 0.15)" : "Рейтинг потребление топлива у субмарин (Стандартно = 0.15)")]
            public Single ConsumedSubmarine;
            [JsonProperty(LanguageEn ? "Minicopter fuel consumption rating (Default = 0.25)" : "Рейтинг потребление топлива у миникоптера (Стандартно = 0.25)")]
            public Single ConsumedCopter;
            [JsonProperty(LanguageEn ? "ScrapTransportHelicopter fuel consumption rating (Default = 0.25)" : "Рейтинг потребление топлива у коровы (Стандартно = 0.25)")]
            public Single ConsumedScrapTransport;
            [JsonProperty(LanguageEn ? "Attack-Helicopter fuel consumption rating (Default = 0.25)" : "Рейтинг потребление топлива у боевого-вертолета (Стандартно = 0.25)")]
            public Single ConsumedAttackHelicopter;
        }

        internal class FuelSettings
        {
            [JsonProperty(LanguageEn ? "Use fuel replenishment when buying vehicles from NPC (true - yes/false - no)" : "Использовать пополнение топлива при покупке транспорта у NPC (true - да/false - нет)")]
            public Boolean useAutoFillFuel;
            [JsonProperty(LanguageEn ? "Amount of fuel for boats" : "Кол-во топлива у лодок")]
            public Int32 AmountBoat;
            [JsonProperty(LanguageEn ? "The amount of fuel in submarines" : "Кол-во топлива у подводных лодок")]
            public Int32 AmountSubmarine;
            [JsonProperty(LanguageEn ? "Minicopter fuel quantity" : "Кол-во топлива у миникоптера")]
            public Int32 AmountMinicopter;
            [JsonProperty(LanguageEn ? "Helicopter fuel quantity" : "Кол-во топлива у вертолета")]
            public Int32 AmountScrapTransport;
            [JsonProperty(LanguageEn ? "Attack-Helicopter fuel quantity" : "Кол-во топлива у боевого вертолета")]
            public Int32 AmountAttackHelicopter;
        }

        [JsonProperty(LanguageEn ? "Use Time Acceleration" : "Использовать ускорение времени")]
        public Boolean UseTime;
        [JsonProperty(LanguageEn ? "Use time freeze (the time will be the one you set in the item &lt;Frozen time on the server&gt;)" : "Использовать заморозку времени(время будет такое, какое вы установите в пунке <Замороженное время на сервере>)")]
        public Boolean UseFreezeTime;
        [JsonProperty(LanguageEn ? "Frozen time on the server (Set time that will not change and be forever on the server, must be true on &lt;Use time freeze&gt;" : "Замороженное время на сервере (Установите время, которое не будет изменяться и будет вечно на сервере, должен быть true на <Использовать заморозку времени>")]
        public Int32 FreezeTime;
        [JsonProperty(LanguageEn ? "What time will the day start?" : "Укажите во сколько будет начинаться день")]
        public Int32 DayStart;
        [JsonProperty(LanguageEn ? "What time will the night start?" : "Укажите во сколько будет начинаться ночь")]
        public Int32 NightStart;
        [JsonProperty(LanguageEn ? "Specify how long the day will be in minutes" : "Укажите сколько будет длится день в минутах")]
        public Int32 DayTime;
        [JsonProperty(LanguageEn ? "Specify how long the night will last in minutes" : "Укажите сколько будет длится ночь в минутах")]
        public Int32 NightTime;
        [JsonProperty(LanguageEn ? "Use notification of players about the change of day and night (switching rates. The message is configured in the lang)" : "Использовать уведомление игроков о смене дня и ночи (переключение рейтов. Сообщение настраивается в лэнге)")]
        public Boolean UseAlertDayNight;
        [JsonProperty(LanguageEn ? "Enable the ability to completely skip the time of day (selected in the paragraph below)" : "Включить возможность полного пропуска времени суток(выбирается в пункте ниже)")]
        public Boolean UseSkipTime;
        [JsonProperty(LanguageEn ? "Select the type of time-of-day skip (0 - Skip day, 1 - Skip night)" : "Выберите тип пропуска времени суток (0 - Пропускать день, 1 - Пропускать ночь)(Не забудьте включить возможность полного пропуска времени суток)")]
        public SkipType TypeSkipped;
        internal class EventSettings
        {
            [JsonProperty(LanguageEn ? "Helicopter spawn custom settings" : "Кастомные настройки спавна вертолета")]
            public Setting HelicopterSetting;
            [JsonProperty(LanguageEn ? "Custom tank spawn settings" : "Кастомные настройки спавна танка")]
            public Setting BreadlaySetting;
            [JsonProperty(LanguageEn ? "Custom ship spawn settings" : "Кастомные настройки спавна корабля")]
            public Setting CargoShipSetting;
            [JsonProperty(LanguageEn ? "Airdrop spawn custom settings" : "Кастомные настройки спавна аирдропа")]
            public Setting CargoPlaneSetting;
            [JsonProperty(LanguageEn ? "Chinook custom spawn settings" : "Кастомные настройки спавна чинука")]
            public Setting ChinoockSetting;
            internal class Setting
            {
                [JsonProperty(LanguageEn ? "Use notifications for running events (configurable in lang)" : "Использовать уведомления у запущенных ивентах (настраивается в lang)")]
                public Boolean useGameTip;
                [JsonProperty(LanguageEn ? "Completely disable event spawning on the server (true - yes/false - no)" : "Полностью отключить спавн ивента на сервере(true - да/false - нет)")]
                public Boolean FullOff;
                [JsonProperty(LanguageEn ? "Enable custom spawn event (true - yes/false - no)" : "Включить кастомный спавн ивент(true - да/false - нет)")]
                public Boolean UseEventCustom;
                [JsonProperty(LanguageEn ? "Static event spawn time" : "Статическое время спавна ивента")]
                public Int32 EventSpawnTime;
                [JsonProperty(LanguageEn ? "Random spawn time settings" : "Настройки случайного времени спавна")]
                public RandomingTime RandomTimeSpawn;
                internal class RandomingTime
                {
                    [JsonProperty(LanguageEn ? "Use random event spawn time (static time will not be taken into account) (true - yes/false - no)" : "Использовать случайное время спавно ивента(статическое время не будет учитываться)(true - да/false - нет)")]
                    public Boolean UseRandomTime;
                    [JsonProperty(LanguageEn ? "Minimum event spawn value" : "Минимальное значение спавна ивента")]
                    public Int32 MinEventSpawnTime;
                    [JsonProperty(LanguageEn ? "Max event spawn value" : "Максимальное значении спавна ивента")]
                    public Int32 MaxEventSpawnTime;
                }

            }

        }

    }

    internal class Rates
    {
        [JsonProperty(LanguageEn ? "Ranking setting during the day" : "Настройка рейтинга днем")]
        public AllRates DayRates;
        [JsonProperty(LanguageEn ? "Setting the rating at night" : "Настройка рейтинга ночью")]
        public AllRates NightRates;
        [JsonProperty(LanguageEn ? "Setting privileges and ratings specifically for them [iqrates.vip] = { Setting } (Descending)" : "Настройка привилегий и рейтингов конкретно для них [iqrates.vip] = { Настройка } (По убыванию)")]
        public Dictionary<String, DayAnNightRate> PrivilegyRates;
        [JsonProperty(LanguageEn ? "Setting up rating increase by days of the week" : "Настройка увеличения рейтинга по дням недели")]
        public RateControllerDayOfWeek rateControllerDayOfWeek;
        internal class RateControllerDayOfWeek
        {
            [JsonProperty(LanguageEn ? "Using the function to increase ratings based on the days of the week (time and days are taken from your server machine)" : "Использовать функцию увеличения рейтинга по дням недели (время и дни берутся с вашей серверной машины)")]
            public Boolean useRateBonusDayOfWeek;
            [JsonProperty(LanguageEn ? "Setting the days of the week and time intervals (make sure that different intervals do not overlap with each other, otherwise it may lead to conflicts)" : "Настройка дней недели и промежутка времени (учтите, чтобы разные промежутка не пересекались друг с другом, иначе это может привести к конфликту)")]
            public List<RateBonusDays> rateBonusDayOfWeek;
            internal class RateBonusDays
            {
                [JsonProperty(LanguageEn ? "How much additional rating will be added to the xN player? For example, if a player has x4, and this parameter is 1.5, the player's rating will be x5.5" : "Сколько будет добавлено дополнительного рейтинга к xN игрока (Например : у игрока х4, данный параметр равен 1.5, в результате у игрока будет x5.5)")]
                public Single upBonusRate;
                [JsonProperty(LanguageEn ? "How much additional rating will be added to the xN player, based on permissions (Permission = xN)" : "Сколько будет добавлено дополнительного рейтинга к xN игрока, по привилегиям (Permission = xN)")]
                public Dictionary<String, Single> privilageUpRates;
                [JsonProperty(LanguageEn ? "Configuration of the day and time for starting the bonus rating" : "Настройка дня и времени запуска бонусного рейтинга")]
                public TimeController timeStartBonus;
                [JsonProperty(LanguageEn ? "Configuration of the day and time for the end of the bonus rating" : "Настройка дня и времени заверешения бонусного рейтинга")]
                public TimeController timeStopBonus;
                internal class TimeController
                {
                    [JsonProperty(LanguageEn ? "Specify the hours (24-hour format)" : "Укажите часы (Формат 24 часа)")]
                    public Int32 timeHours;
                    [JsonProperty(LanguageEn ? "Day of the week: Monday, Tuesday, Wednesday, Thursday, Friday, Saturday, Sunday" : "День недели : Monday, Tuesday, Wednesday, Thursday, Friday, Saturday, Sunday")]
                    public String dayOfWeek;
                }

            }

        }

        [JsonProperty(LanguageEn ? "Setting custom rates (items) by permission - setting (Descending)" : "Настройка кастомных рейтов(предметов) по пермишенсу - настройка (По убыванию)")]
        public PermissionsRate CustomRatesPermissions;
        [JsonProperty(LanguageEn ? "Select the type of sheet to use: 0 - White sheet, 1 - Black sheet (White sheet - the ratings of only those items that are in it increase, the rest are ignored | The Black sheet is completely the opposite)"
                                             : "Выберите тип используемого листа : 0 - Белый лист, 1 - Черный лист (Белый лист - увеличиваются рейтинги только тех предметов - которые в нем, остальные игнорируются | Черный лист полностью наоборот)")]
        public TypeListUsed TypeList;
        [JsonProperty(LanguageEn ? "Black list of items that will not be categorically affected by the rating" : "Черный лист предметов,на которые катигорично не будут действовать рейтинг")]
        public List<String> BlackList;
        [JsonProperty(LanguageEn ? "A white list of items that will ONLY be affected by ratings - the rest is ignored" : "Белый лист предметов, на которые ТОЛЬКО будут действовать рейтинги - остальное игнорируются")]
        public List<String> WhiteList;
        [JsonProperty(LanguageEn ? "A blacklist of categories that will NOT be affected by ratings" : "Черный список категорий на которые НЕ БУДУТ действовать рейтинги")]
        public List<String> BlackListCategory;
        [JsonProperty(LanguageEn ? "Use a tea controller? (Works on scrap, ore and wood tea). If enabled, it will set % to production due to the difference between rates (standard / privileges)" : "Использовать контроллер чая? (Работае на скрап, рудный и древесный чай). Если включено - будет устанавливать % к добычи за счет разницы между рейтами (стандартный / привилегии)")]
        public Boolean UseTeaController;
        [JsonProperty(LanguageEn ? "Use a prefabs blacklist?" : "Использовать черный лист префабов?")]
        public Boolean UseBlackListPrefabs;
        [JsonProperty(LanguageEn ? "Black list of prefabs(ShortPrefabName) - which will not be affected by ratings" : "Черный лист префабов(ShortPrefabName) - на которые не будут действовать рейтинги")]
        public List<String> BlackListPrefabs;
        [JsonProperty(LanguageEn ? "Enable melting speed in furnaces (true - yes/false - no)" : "Включить скорость плавки в печах(true - да/false - нет)")]
        public Boolean UseSpeedBurnable;
        [JsonProperty(LanguageEn ? "Smelting Fuel Usage Rating (If the list is enabled, this value will be the default value for all non-licensed)" : "Рейтинг использования топлива при переплавки(Если включен список - это значение будет стандартное для всех у кого нет прав)")]
        public Int32 SpeedFuelBurnable;
        [JsonProperty(LanguageEn ? "Use a blacklist of items for melting?" : "Использовать черный список предметов для плавки?")]
        public Boolean UseBlackListBurnable;
        [JsonProperty(LanguageEn ? "A black list of items for the stove, which will not be categorically affected by melting" : "Черный лист предметов для печки,на которые катигорично не будут действовать плавка")]
        public List<String> BlackListBurnable;
        [JsonProperty(LanguageEn ? "Furnace smelting speed (If the list is enabled, this value will be the default for everyone who does not have rights)" : "Скорость плавки печей(Если включен список - это значение будет стандартное для всех у кого нет прав)")]
        public Single SpeedBurnable;
        [JsonProperty(LanguageEn ? "Enable list of melting speed in furnaces (true - yes/false - no)" : "Включить список скорости плавки в печах(true - да/false - нет)")]
        public Boolean UseSpeedBurnableList;
        [JsonProperty(LanguageEn ? "Setting the melting speed in furnaces by privileges" : "Настройка скорости плавки в печах по привилегиям")]
        public List<SpeedBurnablePreset> SpeedBurableList;
        [JsonProperty(LanguageEn ? "Enable tea mixing acceleration in the mixing table (true - yes/false - no)" : "Включить ускорение смешивания чая в столе смешивания (true - да/false - нет)")]
        public Boolean useSpeedMixingTable;
        [JsonProperty(LanguageEn ? "Setting the tea mixing speed" : "Настройка скорости смешивания чая")]
        public List<SpeedMixingTable> speedMixingTables;
        [JsonProperty(LanguageEn ? "A blacklist of prefabs (ShortPrefabName) that will not be affected by acceleration (example: campfire) [If you don't need it, leave it empty]" : "Черный список префабов(ShortPrefabName), на которые не будет действовать ускорение (пример : campfire) [Если вам не нужно - оставьте пустым]")]
        public List<String> IgnoreSpeedBurnablePrefabList;
        internal class DayAnNightRate
        {
            [JsonProperty(LanguageEn ? "Ranking setting during the day" : "Настройка рейтинга днем")]
            public AllRates DayRates;
            [JsonProperty(LanguageEn ? "Setting the rating at night" : "Настройка рейтинга ночью")]
            public AllRates NightRates;
        }

        [JsonProperty(LanguageEn ? "Setting up a recycler" : "Настройка переработчика")]
        public RecyclerController RecyclersController;
        internal class RecyclerController
        {
            [JsonProperty(LanguageEn ? "Use the processing time editing functions" : "Использовать функции редактирования времени переработки")]
            public Boolean UseRecyclerSpeed;
            [JsonProperty(LanguageEn ? "Static processing time (in seconds) (Will be set according to the standard or if the player does not have the privilege) (Default = 5s)" : "Статичное время переработки (в секундах) (Будет установлено по стандарту или если у игрока нет привилеии) (По умолчанию = 5с)")]
            public Single DefaultSpeedRecycler;
            [JsonProperty(LanguageEn ? "Setting the processing time for privileges (adjust from greater privilege to lesser privilege)" : "Настройка времени переработки для привилегий (настраивать от большей привилегии к меньшей)")]
            public List<PresetRecycler> PrivilageSpeedRecycler;
            internal class PresetRecycler
            {
                [JsonProperty(LanguageEn ? "Permissions" : "Права")]
                public String Permissions;
                [JsonProperty(LanguageEn ? "Standard processing time (in seconds)" : "Стандартное время переработки (в секундах)")]
                public Single SpeedRecyclers;
            }

        }

        internal class SpeedBurnablePreset
        {
            [JsonProperty(LanguageEn ? "Permissions" : "Права")]
            public String Permissions;
            [JsonProperty(LanguageEn ? "Furnace melting speed" : "Скорость плавки печей")]
            public Single SpeedBurnable;
            [JsonProperty(LanguageEn ? "Smelting Fuel Use Rating" : "Рейтинг использования топлива при переплавки")]
            public Int32 SpeedFuelBurnable;
        }

        internal class SpeedMixingTable
        {
            [JsonProperty(LanguageEn ? "Permissions" : "Права")]
            public String Permissions;
            [JsonProperty(LanguageEn ? "Mixing speed (0-100%)" : "Скорость смешивания (0-100%)")]
            public Int32 SpeedMixing;
        }

        internal class PermissionsRate
        {
            [JsonProperty(LanguageEn ? "Ranking setting during the day" : "Настройка рейтинга днем")]
            public Dictionary<String, List<PermissionsRateDetalis>> DayRates;
            [JsonProperty(LanguageEn ? "Setting the rating at night" : "Настройка рейтинга ночью")]
            public Dictionary<String, List<PermissionsRateDetalis>> NightRates;
            public class PermissionsRateDetalis
            {
                [JsonProperty(LanguageEn ? "Shortname" : "Shortname")]
                public String Shortname;
                [JsonProperty(LanguageEn ? "Rate" : "Рейтинг")]
                public Single Rate;
            }

        }

        internal class AllRates
        {
            [JsonProperty(LanguageEn ? "Rating of extracted resources" : "Рейтинг добываемых ресурсов")]
            public Single GatherRate;
            [JsonProperty(LanguageEn ? "Rating of found items" : "Рейтинг найденных предметов")]
            public Single LootRate;
            [JsonProperty(LanguageEn ? "Pickup Rating" : "Рейтинг поднимаемых предметов")]
            public Single PickUpRate;
            [JsonProperty(LanguageEn ? "Rating of plants raised from the beds" : "Рейтинг поднимаемых растений с грядок")]
            public Single GrowableRate;
            [JsonProperty(LanguageEn ? "Quarry rating" : "Рейтинг карьеров")]
            public Single QuarryRate;
            [JsonProperty(LanguageEn ? "Detailed rating settings in the career" : "Детальная настройка рейтинга в карьере")]
            public QuarryRateDetalis QuarryDetalis;
            [JsonProperty(LanguageEn ? "Excavator Rating" : "Рейтинг экскаватора")]
            public Single ExcavatorRate;
            [JsonProperty(LanguageEn ? "Coal drop chance" : "Шанс выпадения угля")]
            public Single CoalRare;
            [JsonProperty(LanguageEn ? "Rating of items caught from the sea (fishing)" : "Рейтинг предметов выловленных с моря (рыбалки)")]
            public Single FishRate;
            internal class QuarryRateDetalis
            {
                [JsonProperty(LanguageEn ? "Use the detailed setting of the career rating (otherwise the 'Career Rating' will be taken for all subjects)" : "Использовать детальную настройку рейтинга карьеров (иначе будет браться 'Рейтинг карьеров' для всех предметов)")]
                public Boolean UseDetalisRateQuarry;
                [JsonProperty(LanguageEn ? "The item dropped out of the career - rating" : "Предмет выпадаемый из карьера - рейтинг")]
                public Dictionary<String, Single> ShortnameListQuarry;
            }

        }

    }

    [JsonProperty(LanguageEn ? "Configuring supported plugins" : "Настройка поддерживаемых плагинов")]
    public ReferencePlugin ReferenceSettings;
}

internal class ReferencePlugin
{
    internal class IQChatReference
    {
        [JsonProperty(LanguageEn ? "IQChat : Custom chat avatar (If required)" : "IQChat : Кастомный аватар в чате(Если требуется)")]
        public String CustomAvatar;
        [JsonProperty(LanguageEn ? "IQChat : Use UI Notifications" : "IQChat : Использовать UI уведомления")]
        public Boolean UIAlertUse;
        [JsonProperty(LanguageEn ? "IQChat : Custom prefix in chat" : "IQChat : Кастомный префикс в чате")]
        public String CustomPrefix;
    }

    [JsonProperty(LanguageEn ? "Setting up IQChat" : "Настройка IQChat")]
    public IQChatReference IQChatSetting;
}

internal class IQChatReference
{
    [JsonProperty(LanguageEn ? "IQChat : Custom chat avatar (If required)" : "IQChat : Кастомный аватар в чате(Если требуется)")]
    public String CustomAvatar;
    [JsonProperty(LanguageEn ? "IQChat : Use UI Notifications" : "IQChat : Использовать UI уведомления")]
    public Boolean UIAlertUse;
    [JsonProperty(LanguageEn ? "IQChat : Custom prefix in chat" : "IQChat : Кастомный префикс в чате")]
    public String CustomPrefix;
}

internal class OtherSettings
{
    [JsonProperty(LanguageEn ? "Event settings on the server" : "Настройки ивентов на сервере")]
    public EventSettings EventSetting;
    [JsonProperty(LanguageEn ? "Fuel settings when buying vehicles from NPCs" : "Настройки топлива при покупке транспорта у NPC")]
    public FuelSettings FuelSetting;
    [JsonProperty(LanguageEn ? "Fuel Consumption Rating Settings" : "Настройки рейтинга потребления топлива")]
    public FuelConsumedTransport FuelConsumedTransportSetting;
    internal class FuelConsumedTransport
    {
        [JsonProperty(LanguageEn ? "Use the fuel consumption rating in transport (true - yes/false - no)" : "Использовать рейтинг потребления топлива в транспорте (true - да/false - нет)")]
        public Boolean useConsumedFuel;
        [JsonProperty(LanguageEn ? "Hotairballoon fuel consumption rating (Default = 0.25)" : "Рейтинг потребление топлива у воздушного шара (Стандартно = 0.25)")]
        public Single ConsumedHotAirBalloon;
        [JsonProperty(LanguageEn ? "Snowmobile fuel consumption rating (Default = 0.15)" : "Рейтинг потребление топлива снегоходов (Стандартно = 0.15)")]
        public Single ConsumedSnowmobile;
        [JsonProperty(LanguageEn ? "Train fuel consumption rating (Default = 0.15)" : "Рейтинг потребление топлива поездов (Стандартно = 0.15)")]
        public Single ConsumedTrain;
        [JsonProperty(LanguageEn ? "Rowboat fuel consumption rating (Default = 0.25)" : "Рейтинг потребление топлива у лодок (Стандартно = 0.25)")]
        public Single ConsumedBoat;
        [JsonProperty(LanguageEn ? "Submarine fuel consumption rating (Default = 0.15)" : "Рейтинг потребление топлива у субмарин (Стандартно = 0.15)")]
        public Single ConsumedSubmarine;
        [JsonProperty(LanguageEn ? "Minicopter fuel consumption rating (Default = 0.25)" : "Рейтинг потребление топлива у миникоптера (Стандартно = 0.25)")]
        public Single ConsumedCopter;
        [JsonProperty(LanguageEn ? "ScrapTransportHelicopter fuel consumption rating (Default = 0.25)" : "Рейтинг потребление топлива у коровы (Стандартно = 0.25)")]
        public Single ConsumedScrapTransport;
        [JsonProperty(LanguageEn ? "Attack-Helicopter fuel consumption rating (Default = 0.25)" : "Рейтинг потребление топлива у боевого-вертолета (Стандартно = 0.25)")]
        public Single ConsumedAttackHelicopter;
    }

    internal class FuelSettings
    {
        [JsonProperty(LanguageEn ? "Use fuel replenishment when buying vehicles from NPC (true - yes/false - no)" : "Использовать пополнение топлива при покупке транспорта у NPC (true - да/false - нет)")]
        public Boolean useAutoFillFuel;
        [JsonProperty(LanguageEn ? "Amount of fuel for boats" : "Кол-во топлива у лодок")]
        public Int32 AmountBoat;
        [JsonProperty(LanguageEn ? "The amount of fuel in submarines" : "Кол-во топлива у подводных лодок")]
        public Int32 AmountSubmarine;
        [JsonProperty(LanguageEn ? "Minicopter fuel quantity" : "Кол-во топлива у миникоптера")]
        public Int32 AmountMinicopter;
        [JsonProperty(LanguageEn ? "Helicopter fuel quantity" : "Кол-во топлива у вертолета")]
        public Int32 AmountScrapTransport;
        [JsonProperty(LanguageEn ? "Attack-Helicopter fuel quantity" : "Кол-во топлива у боевого вертолета")]
        public Int32 AmountAttackHelicopter;
    }

    [JsonProperty(LanguageEn ? "Use Time Acceleration" : "Использовать ускорение времени")]
    public Boolean UseTime;
    [JsonProperty(LanguageEn ? "Use time freeze (the time will be the one you set in the item &lt;Frozen time on the server&gt;)" : "Использовать заморозку времени(время будет такое, какое вы установите в пунке <Замороженное время на сервере>)")]
    public Boolean UseFreezeTime;
    [JsonProperty(LanguageEn ? "Frozen time on the server (Set time that will not change and be forever on the server, must be true on &lt;Use time freeze&gt;" : "Замороженное время на сервере (Установите время, которое не будет изменяться и будет вечно на сервере, должен быть true на <Использовать заморозку времени>")]
    public Int32 FreezeTime;
    [JsonProperty(LanguageEn ? "What time will the day start?" : "Укажите во сколько будет начинаться день")]
    public Int32 DayStart;
    [JsonProperty(LanguageEn ? "What time will the night start?" : "Укажите во сколько будет начинаться ночь")]
    public Int32 NightStart;
    [JsonProperty(LanguageEn ? "Specify how long the day will be in minutes" : "Укажите сколько будет длится день в минутах")]
    public Int32 DayTime;
    [JsonProperty(LanguageEn ? "Specify how long the night will last in minutes" : "Укажите сколько будет длится ночь в минутах")]
    public Int32 NightTime;
    [JsonProperty(LanguageEn ? "Use notification of players about the change of day and night (switching rates. The message is configured in the lang)" : "Использовать уведомление игроков о смене дня и ночи (переключение рейтов. Сообщение настраивается в лэнге)")]
    public Boolean UseAlertDayNight;
    [JsonProperty(LanguageEn ? "Enable the ability to completely skip the time of day (selected in the paragraph below)" : "Включить возможность полного пропуска времени суток(выбирается в пункте ниже)")]
    public Boolean UseSkipTime;
    [JsonProperty(LanguageEn ? "Select the type of time-of-day skip (0 - Skip day, 1 - Skip night)" : "Выберите тип пропуска времени суток (0 - Пропускать день, 1 - Пропускать ночь)(Не забудьте включить возможность полного пропуска времени суток)")]
    public SkipType TypeSkipped;
    internal class EventSettings
    {
        [JsonProperty(LanguageEn ? "Helicopter spawn custom settings" : "Кастомные настройки спавна вертолета")]
        public Setting HelicopterSetting;
        [JsonProperty(LanguageEn ? "Custom tank spawn settings" : "Кастомные настройки спавна танка")]
        public Setting BreadlaySetting;
        [JsonProperty(LanguageEn ? "Custom ship spawn settings" : "Кастомные настройки спавна корабля")]
        public Setting CargoShipSetting;
        [JsonProperty(LanguageEn ? "Airdrop spawn custom settings" : "Кастомные настройки спавна аирдропа")]
        public Setting CargoPlaneSetting;
        [JsonProperty(LanguageEn ? "Chinook custom spawn settings" : "Кастомные настройки спавна чинука")]
        public Setting ChinoockSetting;
        internal class Setting
        {
            [JsonProperty(LanguageEn ? "Use notifications for running events (configurable in lang)" : "Использовать уведомления у запущенных ивентах (настраивается в lang)")]
            public Boolean useGameTip;
            [JsonProperty(LanguageEn ? "Completely disable event spawning on the server (true - yes/false - no)" : "Полностью отключить спавн ивента на сервере(true - да/false - нет)")]
            public Boolean FullOff;
            [JsonProperty(LanguageEn ? "Enable custom spawn event (true - yes/false - no)" : "Включить кастомный спавн ивент(true - да/false - нет)")]
            public Boolean UseEventCustom;
            [JsonProperty(LanguageEn ? "Static event spawn time" : "Статическое время спавна ивента")]
            public Int32 EventSpawnTime;
            [JsonProperty(LanguageEn ? "Random spawn time settings" : "Настройки случайного времени спавна")]
            public RandomingTime RandomTimeSpawn;
            internal class RandomingTime
            {
                [JsonProperty(LanguageEn ? "Use random event spawn time (static time will not be taken into account) (true - yes/false - no)" : "Использовать случайное время спавно ивента(статическое время не будет учитываться)(true - да/false - нет)")]
                public Boolean UseRandomTime;
                [JsonProperty(LanguageEn ? "Minimum event spawn value" : "Минимальное значение спавна ивента")]
                public Int32 MinEventSpawnTime;
                [JsonProperty(LanguageEn ? "Max event spawn value" : "Максимальное значении спавна ивента")]
                public Int32 MaxEventSpawnTime;
            }

        }

    }

}

internal class FuelConsumedTransport
{
    [JsonProperty(LanguageEn ? "Use the fuel consumption rating in transport (true - yes/false - no)" : "Использовать рейтинг потребления топлива в транспорте (true - да/false - нет)")]
    public Boolean useConsumedFuel;
    [JsonProperty(LanguageEn ? "Hotairballoon fuel consumption rating (Default = 0.25)" : "Рейтинг потребление топлива у воздушного шара (Стандартно = 0.25)")]
    public Single ConsumedHotAirBalloon;
    [JsonProperty(LanguageEn ? "Snowmobile fuel consumption rating (Default = 0.15)" : "Рейтинг потребление топлива снегоходов (Стандартно = 0.15)")]
    public Single ConsumedSnowmobile;
    [JsonProperty(LanguageEn ? "Train fuel consumption rating (Default = 0.15)" : "Рейтинг потребление топлива поездов (Стандартно = 0.15)")]
    public Single ConsumedTrain;
    [JsonProperty(LanguageEn ? "Rowboat fuel consumption rating (Default = 0.25)" : "Рейтинг потребление топлива у лодок (Стандартно = 0.25)")]
    public Single ConsumedBoat;
    [JsonProperty(LanguageEn ? "Submarine fuel consumption rating (Default = 0.15)" : "Рейтинг потребление топлива у субмарин (Стандартно = 0.15)")]
    public Single ConsumedSubmarine;
    [JsonProperty(LanguageEn ? "Minicopter fuel consumption rating (Default = 0.25)" : "Рейтинг потребление топлива у миникоптера (Стандартно = 0.25)")]
    public Single ConsumedCopter;
    [JsonProperty(LanguageEn ? "ScrapTransportHelicopter fuel consumption rating (Default = 0.25)" : "Рейтинг потребление топлива у коровы (Стандартно = 0.25)")]
    public Single ConsumedScrapTransport;
    [JsonProperty(LanguageEn ? "Attack-Helicopter fuel consumption rating (Default = 0.25)" : "Рейтинг потребление топлива у боевого-вертолета (Стандартно = 0.25)")]
    public Single ConsumedAttackHelicopter;
}

internal class FuelSettings
{
    [JsonProperty(LanguageEn ? "Use fuel replenishment when buying vehicles from NPC (true - yes/false - no)" : "Использовать пополнение топлива при покупке транспорта у NPC (true - да/false - нет)")]
    public Boolean useAutoFillFuel;
    [JsonProperty(LanguageEn ? "Amount of fuel for boats" : "Кол-во топлива у лодок")]
    public Int32 AmountBoat;
    [JsonProperty(LanguageEn ? "The amount of fuel in submarines" : "Кол-во топлива у подводных лодок")]
    public Int32 AmountSubmarine;
    [JsonProperty(LanguageEn ? "Minicopter fuel quantity" : "Кол-во топлива у миникоптера")]
    public Int32 AmountMinicopter;
    [JsonProperty(LanguageEn ? "Helicopter fuel quantity" : "Кол-во топлива у вертолета")]
    public Int32 AmountScrapTransport;
    [JsonProperty(LanguageEn ? "Attack-Helicopter fuel quantity" : "Кол-во топлива у боевого вертолета")]
    public Int32 AmountAttackHelicopter;
}

internal class EventSettings
{
    [JsonProperty(LanguageEn ? "Helicopter spawn custom settings" : "Кастомные настройки спавна вертолета")]
    public Setting HelicopterSetting;
    [JsonProperty(LanguageEn ? "Custom tank spawn settings" : "Кастомные настройки спавна танка")]
    public Setting BreadlaySetting;
    [JsonProperty(LanguageEn ? "Custom ship spawn settings" : "Кастомные настройки спавна корабля")]
    public Setting CargoShipSetting;
    [JsonProperty(LanguageEn ? "Airdrop spawn custom settings" : "Кастомные настройки спавна аирдропа")]
    public Setting CargoPlaneSetting;
    [JsonProperty(LanguageEn ? "Chinook custom spawn settings" : "Кастомные настройки спавна чинука")]
    public Setting ChinoockSetting;
    internal class Setting
    {
        [JsonProperty(LanguageEn ? "Use notifications for running events (configurable in lang)" : "Использовать уведомления у запущенных ивентах (настраивается в lang)")]
        public Boolean useGameTip;
        [JsonProperty(LanguageEn ? "Completely disable event spawning on the server (true - yes/false - no)" : "Полностью отключить спавн ивента на сервере(true - да/false - нет)")]
        public Boolean FullOff;
        [JsonProperty(LanguageEn ? "Enable custom spawn event (true - yes/false - no)" : "Включить кастомный спавн ивент(true - да/false - нет)")]
        public Boolean UseEventCustom;
        [JsonProperty(LanguageEn ? "Static event spawn time" : "Статическое время спавна ивента")]
        public Int32 EventSpawnTime;
        [JsonProperty(LanguageEn ? "Random spawn time settings" : "Настройки случайного времени спавна")]
        public RandomingTime RandomTimeSpawn;
        internal class RandomingTime
        {
            [JsonProperty(LanguageEn ? "Use random event spawn time (static time will not be taken into account) (true - yes/false - no)" : "Использовать случайное время спавно ивента(статическое время не будет учитываться)(true - да/false - нет)")]
            public Boolean UseRandomTime;
            [JsonProperty(LanguageEn ? "Minimum event spawn value" : "Минимальное значение спавна ивента")]
            public Int32 MinEventSpawnTime;
            [JsonProperty(LanguageEn ? "Max event spawn value" : "Максимальное значении спавна ивента")]
            public Int32 MaxEventSpawnTime;
        }

    }

}

internal class Setting
{
    [JsonProperty(LanguageEn ? "Use notifications for running events (configurable in lang)" : "Использовать уведомления у запущенных ивентах (настраивается в lang)")]
    public Boolean useGameTip;
    [JsonProperty(LanguageEn ? "Completely disable event spawning on the server (true - yes/false - no)" : "Полностью отключить спавн ивента на сервере(true - да/false - нет)")]
    public Boolean FullOff;
    [JsonProperty(LanguageEn ? "Enable custom spawn event (true - yes/false - no)" : "Включить кастомный спавн ивент(true - да/false - нет)")]
    public Boolean UseEventCustom;
    [JsonProperty(LanguageEn ? "Static event spawn time" : "Статическое время спавна ивента")]
    public Int32 EventSpawnTime;
    [JsonProperty(LanguageEn ? "Random spawn time settings" : "Настройки случайного времени спавна")]
    public RandomingTime RandomTimeSpawn;
    internal class RandomingTime
    {
        [JsonProperty(LanguageEn ? "Use random event spawn time (static time will not be taken into account) (true - yes/false - no)" : "Использовать случайное время спавно ивента(статическое время не будет учитываться)(true - да/false - нет)")]
        public Boolean UseRandomTime;
        [JsonProperty(LanguageEn ? "Minimum event spawn value" : "Минимальное значение спавна ивента")]
        public Int32 MinEventSpawnTime;
        [JsonProperty(LanguageEn ? "Max event spawn value" : "Максимальное значении спавна ивента")]
        public Int32 MaxEventSpawnTime;
    }

}

internal class RandomingTime
{
    [JsonProperty(LanguageEn ? "Use random event spawn time (static time will not be taken into account) (true - yes/false - no)" : "Использовать случайное время спавно ивента(статическое время не будет учитываться)(true - да/false - нет)")]
    public Boolean UseRandomTime;
    [JsonProperty(LanguageEn ? "Minimum event spawn value" : "Минимальное значение спавна ивента")]
    public Int32 MinEventSpawnTime;
    [JsonProperty(LanguageEn ? "Max event spawn value" : "Максимальное значении спавна ивента")]
    public Int32 MaxEventSpawnTime;
}

internal class Rates
{
    [JsonProperty(LanguageEn ? "Ranking setting during the day" : "Настройка рейтинга днем")]
    public AllRates DayRates;
    [JsonProperty(LanguageEn ? "Setting the rating at night" : "Настройка рейтинга ночью")]
    public AllRates NightRates;
    [JsonProperty(LanguageEn ? "Setting privileges and ratings specifically for them [iqrates.vip] = { Setting } (Descending)" : "Настройка привилегий и рейтингов конкретно для них [iqrates.vip] = { Настройка } (По убыванию)")]
    public Dictionary<String, DayAnNightRate> PrivilegyRates;
    [JsonProperty(LanguageEn ? "Setting up rating increase by days of the week" : "Настройка увеличения рейтинга по дням недели")]
    public RateControllerDayOfWeek rateControllerDayOfWeek;
    internal class RateControllerDayOfWeek
    {
        [JsonProperty(LanguageEn ? "Using the function to increase ratings based on the days of the week (time and days are taken from your server machine)" : "Использовать функцию увеличения рейтинга по дням недели (время и дни берутся с вашей серверной машины)")]
        public Boolean useRateBonusDayOfWeek;
        [JsonProperty(LanguageEn ? "Setting the days of the week and time intervals (make sure that different intervals do not overlap with each other, otherwise it may lead to conflicts)" : "Настройка дней недели и промежутка времени (учтите, чтобы разные промежутка не пересекались друг с другом, иначе это может привести к конфликту)")]
        public List<RateBonusDays> rateBonusDayOfWeek;
        internal class RateBonusDays
        {
            [JsonProperty(LanguageEn ? "How much additional rating will be added to the xN player? For example, if a player has x4, and this parameter is 1.5, the player's rating will be x5.5" : "Сколько будет добавлено дополнительного рейтинга к xN игрока (Например : у игрока х4, данный параметр равен 1.5, в результате у игрока будет x5.5)")]
            public Single upBonusRate;
            [JsonProperty(LanguageEn ? "How much additional rating will be added to the xN player, based on permissions (Permission = xN)" : "Сколько будет добавлено дополнительного рейтинга к xN игрока, по привилегиям (Permission = xN)")]
            public Dictionary<String, Single> privilageUpRates;
            [JsonProperty(LanguageEn ? "Configuration of the day and time for starting the bonus rating" : "Настройка дня и времени запуска бонусного рейтинга")]
            public TimeController timeStartBonus;
            [JsonProperty(LanguageEn ? "Configuration of the day and time for the end of the bonus rating" : "Настройка дня и времени заверешения бонусного рейтинга")]
            public TimeController timeStopBonus;
            internal class TimeController
            {
                [JsonProperty(LanguageEn ? "Specify the hours (24-hour format)" : "Укажите часы (Формат 24 часа)")]
                public Int32 timeHours;
                [JsonProperty(LanguageEn ? "Day of the week: Monday, Tuesday, Wednesday, Thursday, Friday, Saturday, Sunday" : "День недели : Monday, Tuesday, Wednesday, Thursday, Friday, Saturday, Sunday")]
                public String dayOfWeek;
            }

        }

    }

    [JsonProperty(LanguageEn ? "Setting custom rates (items) by permission - setting (Descending)" : "Настройка кастомных рейтов(предметов) по пермишенсу - настройка (По убыванию)")]
    public PermissionsRate CustomRatesPermissions;
    [JsonProperty(LanguageEn ? "Select the type of sheet to use: 0 - White sheet, 1 - Black sheet (White sheet - the ratings of only those items that are in it increase, the rest are ignored | The Black sheet is completely the opposite)"
                                             : "Выберите тип используемого листа : 0 - Белый лист, 1 - Черный лист (Белый лист - увеличиваются рейтинги только тех предметов - которые в нем, остальные игнорируются | Черный лист полностью наоборот)")]
    public TypeListUsed TypeList;
    [JsonProperty(LanguageEn ? "Black list of items that will not be categorically affected by the rating" : "Черный лист предметов,на которые катигорично не будут действовать рейтинг")]
    public List<String> BlackList;
    [JsonProperty(LanguageEn ? "A white list of items that will ONLY be affected by ratings - the rest is ignored" : "Белый лист предметов, на которые ТОЛЬКО будут действовать рейтинги - остальное игнорируются")]
    public List<String> WhiteList;
    [JsonProperty(LanguageEn ? "A blacklist of categories that will NOT be affected by ratings" : "Черный список категорий на которые НЕ БУДУТ действовать рейтинги")]
    public List<String> BlackListCategory;
    [JsonProperty(LanguageEn ? "Use a tea controller? (Works on scrap, ore and wood tea). If enabled, it will set % to production due to the difference between rates (standard / privileges)" : "Использовать контроллер чая? (Работае на скрап, рудный и древесный чай). Если включено - будет устанавливать % к добычи за счет разницы между рейтами (стандартный / привилегии)")]
    public Boolean UseTeaController;
    [JsonProperty(LanguageEn ? "Use a prefabs blacklist?" : "Использовать черный лист префабов?")]
    public Boolean UseBlackListPrefabs;
    [JsonProperty(LanguageEn ? "Black list of prefabs(ShortPrefabName) - which will not be affected by ratings" : "Черный лист префабов(ShortPrefabName) - на которые не будут действовать рейтинги")]
    public List<String> BlackListPrefabs;
    [JsonProperty(LanguageEn ? "Enable melting speed in furnaces (true - yes/false - no)" : "Включить скорость плавки в печах(true - да/false - нет)")]
    public Boolean UseSpeedBurnable;
    [JsonProperty(LanguageEn ? "Smelting Fuel Usage Rating (If the list is enabled, this value will be the default value for all non-licensed)" : "Рейтинг использования топлива при переплавки(Если включен список - это значение будет стандартное для всех у кого нет прав)")]
    public Int32 SpeedFuelBurnable;
    [JsonProperty(LanguageEn ? "Use a blacklist of items for melting?" : "Использовать черный список предметов для плавки?")]
    public Boolean UseBlackListBurnable;
    [JsonProperty(LanguageEn ? "A black list of items for the stove, which will not be categorically affected by melting" : "Черный лист предметов для печки,на которые катигорично не будут действовать плавка")]
    public List<String> BlackListBurnable;
    [JsonProperty(LanguageEn ? "Furnace smelting speed (If the list is enabled, this value will be the default for everyone who does not have rights)" : "Скорость плавки печей(Если включен список - это значение будет стандартное для всех у кого нет прав)")]
    public Single SpeedBurnable;
    [JsonProperty(LanguageEn ? "Enable list of melting speed in furnaces (true - yes/false - no)" : "Включить список скорости плавки в печах(true - да/false - нет)")]
    public Boolean UseSpeedBurnableList;
    [JsonProperty(LanguageEn ? "Setting the melting speed in furnaces by privileges" : "Настройка скорости плавки в печах по привилегиям")]
    public List<SpeedBurnablePreset> SpeedBurableList;
    [JsonProperty(LanguageEn ? "Enable tea mixing acceleration in the mixing table (true - yes/false - no)" : "Включить ускорение смешивания чая в столе смешивания (true - да/false - нет)")]
    public Boolean useSpeedMixingTable;
    [JsonProperty(LanguageEn ? "Setting the tea mixing speed" : "Настройка скорости смешивания чая")]
    public List<SpeedMixingTable> speedMixingTables;
    [JsonProperty(LanguageEn ? "A blacklist of prefabs (ShortPrefabName) that will not be affected by acceleration (example: campfire) [If you don't need it, leave it empty]" : "Черный список префабов(ShortPrefabName), на которые не будет действовать ускорение (пример : campfire) [Если вам не нужно - оставьте пустым]")]
    public List<String> IgnoreSpeedBurnablePrefabList;
    internal class DayAnNightRate
    {
        [JsonProperty(LanguageEn ? "Ranking setting during the day" : "Настройка рейтинга днем")]
        public AllRates DayRates;
        [JsonProperty(LanguageEn ? "Setting the rating at night" : "Настройка рейтинга ночью")]
        public AllRates NightRates;
    }

    [JsonProperty(LanguageEn ? "Setting up a recycler" : "Настройка переработчика")]
    public RecyclerController RecyclersController;
    internal class RecyclerController
    {
        [JsonProperty(LanguageEn ? "Use the processing time editing functions" : "Использовать функции редактирования времени переработки")]
        public Boolean UseRecyclerSpeed;
        [JsonProperty(LanguageEn ? "Static processing time (in seconds) (Will be set according to the standard or if the player does not have the privilege) (Default = 5s)" : "Статичное время переработки (в секундах) (Будет установлено по стандарту или если у игрока нет привилеии) (По умолчанию = 5с)")]
        public Single DefaultSpeedRecycler;
        [JsonProperty(LanguageEn ? "Setting the processing time for privileges (adjust from greater privilege to lesser privilege)" : "Настройка времени переработки для привилегий (настраивать от большей привилегии к меньшей)")]
        public List<PresetRecycler> PrivilageSpeedRecycler;
        internal class PresetRecycler
        {
            [JsonProperty(LanguageEn ? "Permissions" : "Права")]
            public String Permissions;
            [JsonProperty(LanguageEn ? "Standard processing time (in seconds)" : "Стандартное время переработки (в секундах)")]
            public Single SpeedRecyclers;
        }

    }

    internal class SpeedBurnablePreset
    {
        [JsonProperty(LanguageEn ? "Permissions" : "Права")]
        public String Permissions;
        [JsonProperty(LanguageEn ? "Furnace melting speed" : "Скорость плавки печей")]
        public Single SpeedBurnable;
        [JsonProperty(LanguageEn ? "Smelting Fuel Use Rating" : "Рейтинг использования топлива при переплавки")]
        public Int32 SpeedFuelBurnable;
    }

    internal class SpeedMixingTable
    {
        [JsonProperty(LanguageEn ? "Permissions" : "Права")]
        public String Permissions;
        [JsonProperty(LanguageEn ? "Mixing speed (0-100%)" : "Скорость смешивания (0-100%)")]
        public Int32 SpeedMixing;
    }

    internal class PermissionsRate
    {
        [JsonProperty(LanguageEn ? "Ranking setting during the day" : "Настройка рейтинга днем")]
        public Dictionary<String, List<PermissionsRateDetalis>> DayRates;
        [JsonProperty(LanguageEn ? "Setting the rating at night" : "Настройка рейтинга ночью")]
        public Dictionary<String, List<PermissionsRateDetalis>> NightRates;
        public class PermissionsRateDetalis
        {
            [JsonProperty(LanguageEn ? "Shortname" : "Shortname")]
            public String Shortname;
            [JsonProperty(LanguageEn ? "Rate" : "Рейтинг")]
            public Single Rate;
        }

    }

    internal class AllRates
    {
        [JsonProperty(LanguageEn ? "Rating of extracted resources" : "Рейтинг добываемых ресурсов")]
        public Single GatherRate;
        [JsonProperty(LanguageEn ? "Rating of found items" : "Рейтинг найденных предметов")]
        public Single LootRate;
        [JsonProperty(LanguageEn ? "Pickup Rating" : "Рейтинг поднимаемых предметов")]
        public Single PickUpRate;
        [JsonProperty(LanguageEn ? "Rating of plants raised from the beds" : "Рейтинг поднимаемых растений с грядок")]
        public Single GrowableRate;
        [JsonProperty(LanguageEn ? "Quarry rating" : "Рейтинг карьеров")]
        public Single QuarryRate;
        [JsonProperty(LanguageEn ? "Detailed rating settings in the career" : "Детальная настройка рейтинга в карьере")]
        public QuarryRateDetalis QuarryDetalis;
        [JsonProperty(LanguageEn ? "Excavator Rating" : "Рейтинг экскаватора")]
        public Single ExcavatorRate;
        [JsonProperty(LanguageEn ? "Coal drop chance" : "Шанс выпадения угля")]
        public Single CoalRare;
        [JsonProperty(LanguageEn ? "Rating of items caught from the sea (fishing)" : "Рейтинг предметов выловленных с моря (рыбалки)")]
        public Single FishRate;
        internal class QuarryRateDetalis
        {
            [JsonProperty(LanguageEn ? "Use the detailed setting of the career rating (otherwise the 'Career Rating' will be taken for all subjects)" : "Использовать детальную настройку рейтинга карьеров (иначе будет браться 'Рейтинг карьеров' для всех предметов)")]
            public Boolean UseDetalisRateQuarry;
            [JsonProperty(LanguageEn ? "The item dropped out of the career - rating" : "Предмет выпадаемый из карьера - рейтинг")]
            public Dictionary<String, Single> ShortnameListQuarry;
        }

    }

}

internal class RateControllerDayOfWeek
{
    [JsonProperty(LanguageEn ? "Using the function to increase ratings based on the days of the week (time and days are taken from your server machine)" : "Использовать функцию увеличения рейтинга по дням недели (время и дни берутся с вашей серверной машины)")]
    public Boolean useRateBonusDayOfWeek;
    [JsonProperty(LanguageEn ? "Setting the days of the week and time intervals (make sure that different intervals do not overlap with each other, otherwise it may lead to conflicts)" : "Настройка дней недели и промежутка времени (учтите, чтобы разные промежутка не пересекались друг с другом, иначе это может привести к конфликту)")]
    public List<RateBonusDays> rateBonusDayOfWeek;
    internal class RateBonusDays
    {
        [JsonProperty(LanguageEn ? "How much additional rating will be added to the xN player? For example, if a player has x4, and this parameter is 1.5, the player's rating will be x5.5" : "Сколько будет добавлено дополнительного рейтинга к xN игрока (Например : у игрока х4, данный параметр равен 1.5, в результате у игрока будет x5.5)")]
        public Single upBonusRate;
        [JsonProperty(LanguageEn ? "How much additional rating will be added to the xN player, based on permissions (Permission = xN)" : "Сколько будет добавлено дополнительного рейтинга к xN игрока, по привилегиям (Permission = xN)")]
        public Dictionary<String, Single> privilageUpRates;
        [JsonProperty(LanguageEn ? "Configuration of the day and time for starting the bonus rating" : "Настройка дня и времени запуска бонусного рейтинга")]
        public TimeController timeStartBonus;
        [JsonProperty(LanguageEn ? "Configuration of the day and time for the end of the bonus rating" : "Настройка дня и времени заверешения бонусного рейтинга")]
        public TimeController timeStopBonus;
        internal class TimeController
        {
            [JsonProperty(LanguageEn ? "Specify the hours (24-hour format)" : "Укажите часы (Формат 24 часа)")]
            public Int32 timeHours;
            [JsonProperty(LanguageEn ? "Day of the week: Monday, Tuesday, Wednesday, Thursday, Friday, Saturday, Sunday" : "День недели : Monday, Tuesday, Wednesday, Thursday, Friday, Saturday, Sunday")]
            public String dayOfWeek;
        }

    }

}

internal class RateBonusDays
{
    [JsonProperty(LanguageEn ? "How much additional rating will be added to the xN player? For example, if a player has x4, and this parameter is 1.5, the player's rating will be x5.5" : "Сколько будет добавлено дополнительного рейтинга к xN игрока (Например : у игрока х4, данный параметр равен 1.5, в результате у игрока будет x5.5)")]
    public Single upBonusRate;
    [JsonProperty(LanguageEn ? "How much additional rating will be added to the xN player, based on permissions (Permission = xN)" : "Сколько будет добавлено дополнительного рейтинга к xN игрока, по привилегиям (Permission = xN)")]
    public Dictionary<String, Single> privilageUpRates;
    [JsonProperty(LanguageEn ? "Configuration of the day and time for starting the bonus rating" : "Настройка дня и времени запуска бонусного рейтинга")]
    public TimeController timeStartBonus;
    [JsonProperty(LanguageEn ? "Configuration of the day and time for the end of the bonus rating" : "Настройка дня и времени заверешения бонусного рейтинга")]
    public TimeController timeStopBonus;
    internal class TimeController
    {
        [JsonProperty(LanguageEn ? "Specify the hours (24-hour format)" : "Укажите часы (Формат 24 часа)")]
        public Int32 timeHours;
        [JsonProperty(LanguageEn ? "Day of the week: Monday, Tuesday, Wednesday, Thursday, Friday, Saturday, Sunday" : "День недели : Monday, Tuesday, Wednesday, Thursday, Friday, Saturday, Sunday")]
        public String dayOfWeek;
    }

}

internal class TimeController
{
    [JsonProperty(LanguageEn ? "Specify the hours (24-hour format)" : "Укажите часы (Формат 24 часа)")]
    public Int32 timeHours;
    [JsonProperty(LanguageEn ? "Day of the week: Monday, Tuesday, Wednesday, Thursday, Friday, Saturday, Sunday" : "День недели : Monday, Tuesday, Wednesday, Thursday, Friday, Saturday, Sunday")]
    public String dayOfWeek;
}

internal class DayAnNightRate
{
    [JsonProperty(LanguageEn ? "Ranking setting during the day" : "Настройка рейтинга днем")]
    public AllRates DayRates;
    [JsonProperty(LanguageEn ? "Setting the rating at night" : "Настройка рейтинга ночью")]
    public AllRates NightRates;
}

internal class RecyclerController
{
    [JsonProperty(LanguageEn ? "Use the processing time editing functions" : "Использовать функции редактирования времени переработки")]
    public Boolean UseRecyclerSpeed;
    [JsonProperty(LanguageEn ? "Static processing time (in seconds) (Will be set according to the standard or if the player does not have the privilege) (Default = 5s)" : "Статичное время переработки (в секундах) (Будет установлено по стандарту или если у игрока нет привилеии) (По умолчанию = 5с)")]
    public Single DefaultSpeedRecycler;
    [JsonProperty(LanguageEn ? "Setting the processing time for privileges (adjust from greater privilege to lesser privilege)" : "Настройка времени переработки для привилегий (настраивать от большей привилегии к меньшей)")]
    public List<PresetRecycler> PrivilageSpeedRecycler;
    internal class PresetRecycler
    {
        [JsonProperty(LanguageEn ? "Permissions" : "Права")]
        public String Permissions;
        [JsonProperty(LanguageEn ? "Standard processing time (in seconds)" : "Стандартное время переработки (в секундах)")]
        public Single SpeedRecyclers;
    }

}

internal class PresetRecycler
{
    [JsonProperty(LanguageEn ? "Permissions" : "Права")]
    public String Permissions;
    [JsonProperty(LanguageEn ? "Standard processing time (in seconds)" : "Стандартное время переработки (в секундах)")]
    public Single SpeedRecyclers;
}

internal class SpeedBurnablePreset
{
    [JsonProperty(LanguageEn ? "Permissions" : "Права")]
    public String Permissions;
    [JsonProperty(LanguageEn ? "Furnace melting speed" : "Скорость плавки печей")]
    public Single SpeedBurnable;
    [JsonProperty(LanguageEn ? "Smelting Fuel Use Rating" : "Рейтинг использования топлива при переплавки")]
    public Int32 SpeedFuelBurnable;
}

internal class SpeedMixingTable
{
    [JsonProperty(LanguageEn ? "Permissions" : "Права")]
    public String Permissions;
    [JsonProperty(LanguageEn ? "Mixing speed (0-100%)" : "Скорость смешивания (0-100%)")]
    public Int32 SpeedMixing;
}

internal class PermissionsRate
{
    [JsonProperty(LanguageEn ? "Ranking setting during the day" : "Настройка рейтинга днем")]
    public Dictionary<String, List<PermissionsRateDetalis>> DayRates;
    [JsonProperty(LanguageEn ? "Setting the rating at night" : "Настройка рейтинга ночью")]
    public Dictionary<String, List<PermissionsRateDetalis>> NightRates;
    public class PermissionsRateDetalis
    {
        [JsonProperty(LanguageEn ? "Shortname" : "Shortname")]
        public String Shortname;
        [JsonProperty(LanguageEn ? "Rate" : "Рейтинг")]
        public Single Rate;
    }

}

public class PermissionsRateDetalis
{
    [JsonProperty(LanguageEn ? "Shortname" : "Shortname")]
    public String Shortname;
    [JsonProperty(LanguageEn ? "Rate" : "Рейтинг")]
    public Single Rate;
}

internal class AllRates
{
    [JsonProperty(LanguageEn ? "Rating of extracted resources" : "Рейтинг добываемых ресурсов")]
    public Single GatherRate;
    [JsonProperty(LanguageEn ? "Rating of found items" : "Рейтинг найденных предметов")]
    public Single LootRate;
    [JsonProperty(LanguageEn ? "Pickup Rating" : "Рейтинг поднимаемых предметов")]
    public Single PickUpRate;
    [JsonProperty(LanguageEn ? "Rating of plants raised from the beds" : "Рейтинг поднимаемых растений с грядок")]
    public Single GrowableRate;
    [JsonProperty(LanguageEn ? "Quarry rating" : "Рейтинг карьеров")]
    public Single QuarryRate;
    [JsonProperty(LanguageEn ? "Detailed rating settings in the career" : "Детальная настройка рейтинга в карьере")]
    public QuarryRateDetalis QuarryDetalis;
    [JsonProperty(LanguageEn ? "Excavator Rating" : "Рейтинг экскаватора")]
    public Single ExcavatorRate;
    [JsonProperty(LanguageEn ? "Coal drop chance" : "Шанс выпадения угля")]
    public Single CoalRare;
    [JsonProperty(LanguageEn ? "Rating of items caught from the sea (fishing)" : "Рейтинг предметов выловленных с моря (рыбалки)")]
    public Single FishRate;
    internal class QuarryRateDetalis
    {
        [JsonProperty(LanguageEn ? "Use the detailed setting of the career rating (otherwise the 'Career Rating' will be taken for all subjects)" : "Использовать детальную настройку рейтинга карьеров (иначе будет браться 'Рейтинг карьеров' для всех предметов)")]
        public Boolean UseDetalisRateQuarry;
        [JsonProperty(LanguageEn ? "The item dropped out of the career - rating" : "Предмет выпадаемый из карьера - рейтинг")]
        public Dictionary<String, Single> ShortnameListQuarry;
    }

}

internal class QuarryRateDetalis
{
    [JsonProperty(LanguageEn ? "Use the detailed setting of the career rating (otherwise the 'Career Rating' will be taken for all subjects)" : "Использовать детальную настройку рейтинга карьеров (иначе будет браться 'Рейтинг карьеров' для всех предметов)")]
    public Boolean UseDetalisRateQuarry;
    [JsonProperty(LanguageEn ? "The item dropped out of the career - rating" : "Предмет выпадаемый из карьера - рейтинг")]
    public Dictionary<String, Single> ShortnameListQuarry;
}


```

---

## IQReportSystem

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
using Oxide.Core;

Oxide.Plugins
[Info("IQReportSystem", "", "0.1.0")]
 class IQReportSystem : RustPlugin
{
    [PluginReference]
     Plugin GameWerAC;
     Plugin ImageLibrary;
     Plugin MultiFighting;
     Plugin IQChat;
     Plugin Friends;
     Plugin IQPersonal;
    public string GetImage(string shortname, ulong skin);
    public bool AddImage(string url, string shortname, ulong skin);
    public void SetCheck(BasePlayer player);
    public void SetBans(BasePlayer player);
    public void SetScore(ulong UserID, int Amount);
    public void RemoveScore(ulong UserID, int Amount);
     string PermissionModeration;
     string PermissionAdmin;
    public Dictionary<ulong, int> CooldownPC;
    public Dictionary<ulong, PlayerSaveCheckClass> PlayerSaveCheck;
    public class PlayerSaveCheckClass
    {
        public string Discord;
        public string NickName;
        public string StatusNetwork;
        public ulong ModeratorID;
    }

    private class Response
    {
        public List<string> last_ip;
        public string last_nick;
        public List<ulong> another_accs;
        public List<last_checks> last_check;
        public class last_checks
        {
            public ulong moderSteamID;
            public string serverName;
            public int time;
        }

        public List<RustCCBans> bans;
        public class RustCCBans
        {
            public int banID;
            public string reason;
            public string serverName;
            public int OVHserverID;
            public int banDate;
        }

    }

    private static Configuration config;
    public class Configuration
    {
        [JsonProperty("Основные настройки")]
        public Settings Setting;
        [JsonProperty("Причины репорта")]
        public List<string> ReasonReport;
        [JsonProperty("Причины блокировки")]
        public List<BanReason> ReasonBan;
        [JsonProperty("Настройки RustCheatCheck(Будет при проверке выдавать доступ в чекер и выводить информацию модератору)")]
        public RCCSettings RCCSetting;
        [JsonProperty("Настройка репутации для проверяющих")]
        public RaitingSettings RaitingSetting;
        internal class BanReason
        {
            [JsonProperty("Название")]
            public string DisplayName;
            [JsonProperty("Команда")]
            public string Command;
        }

        internal class RCCSettings
        {
            [JsonProperty("Включить поддержку RCC")]
            public bool RCCUse;
            [JsonProperty("Ключ от RCC")]
            public string Key;
        }

        internal class RaitingSettings
        {
            [JsonProperty("Сколько репутации снимать за 1-2 звезды(IQPersonal)")]
            public int RemoveAmountOneTwo;
            [JsonProperty("Сколько репутации давать за 3-4 звезды(IQPersonal)")]
            public int GiveAmountThreeFour;
            [JsonProperty("Сколько репутации давать за 5 звезд(IQPersonal)")]
            public int GiveAmountFive;
        }

        internal class Settings
        {
            [JsonProperty("Настройки IQChat")]
            public ChatSettings ChatSetting;
            [JsonProperty("Настройки интерфейса")]
            public InterfaceSetting Interface;
            [JsonProperty("Включить/отключить оповещение о максимальном кол-во репортов")]
            public bool MaxReportAlert;
            [JsonProperty("Максимальное количество репортов")]
            public int MaxReport;
            [JsonProperty("Перезарядка для отправки репорта(секунды)")]
            public int CooldownTime;
            [JsonProperty("Запретить друзьям репортить друг друга")]
            public bool FriendNoReport;
            [JsonProperty("Включить логирование в беседу ВК")]
            public bool VKMessage;
            [JsonProperty("Включить логирование в Discord")]
            public bool DiscrodMessage;
            [JsonProperty("Webhooks для дискорда")]
            public string WebHook;
            [JsonProperty("Настройки ВК")]
            public VKSetting VKSettings;
            internal class ChatSettings
            {
                [JsonProperty("IQChat : Кастомный префикс в чате")]
                public string CustomPrefix;
                [JsonProperty("IQChat : Кастомный аватар в чате(Если требуется)")]
                public string CustomAvatar;
            }

            internal class VKSetting
            {
                [JsonProperty("Токен от группы ВК(От группы будут идти сообщения в беседу.Вам нужно добавить свою группу в беседу!)")]
                public string Token;
                [JsonProperty("ID беседы для группы")]
                public string ChatID;
            }

            internal class InterfaceSetting
            {
                [JsonProperty("Настройка интерфейса для отправки жалобы")]
                public ReasonInterfaceSetting ReasonInterface;
                [JsonProperty("Настройка интерфейса для уведомления")]
                public AlertInterfaceSettings AlertInterface;
                [JsonProperty("Настройка интерфейса для мини-панели модератора")]
                public ModeratorPanelInterfaceSettings ModderatorPanel;
                [JsonProperty("Настройка интерфейса для рейтинга")]
                public RaitingInterfaceSettings RaitingInterface;
                [JsonProperty("Sprite для рейтинга(звезды)")]
                public string SpriteRaiting;
                [JsonProperty("Sprite для кнопки жалоб в панели модератора")]
                public string SpriteReportModeration;
                [JsonProperty("Sprite для иконки жалоб")]
                public string SpriteReport;
                [JsonProperty("Цвет текста в плагине")]
                public string HexLabels;
                [JsonProperty("Цвет боковой панели")]
                public string HexRightMenu;
                [JsonProperty("Цвет кнопок боковой панели")]
                public string HexButtonRightMenu;
                [JsonProperty("Цвет основной панели")]
                public string HexMainPanel;
                [JsonProperty("Цвет панели поиска и заднего фона игроков")]
                public string HexSearchPanel;
                [JsonProperty("Цвет кнопки у игрока для перехода к действию")]
                public string HexPlayerButton;
                [JsonProperty("Sprite кнопки у игрока для преехода к действию")]
                public string SpritePlayerButton;
                internal class ReasonInterfaceSetting
                {
                    [JsonProperty("Цвет основной панели")]
                    public string HexMain;
                    [JsonProperty("Цвет панели с заголовком")]
                    public string HexTitlePanel;
                    [JsonProperty("Цвет жалоб")]
                    public string HexButton;
                    [JsonProperty("Цвет текста с жалобами")]
                    public string HexLabel;
                    [JsonProperty("Sprite кнопки закрыть")]
                    public string SpriteClose;
                    [JsonProperty("Sprite панели с заголовком")]
                    public string SpriteTitlePanel;
                    [JsonProperty("Цвет кнопки закрыть")]
                    public string HexClose;
                }

                internal class AlertInterfaceSettings
                {
                    [JsonProperty("Цвет основной панели")]
                    public string HexMain;
                    [JsonProperty("Цвет заголовка и полоски")]
                    public string HexTitle;
                    [JsonProperty("Цвет текста")]
                    public string HexLabel;
                }

                internal class ModeratorPanelInterfaceSettings
                {
                    [JsonProperty("Цвет основной панели")]
                    public string HexMain;
                    [JsonProperty("Цвет панели с заголовком")]
                    public string HexTitlePanel;
                    [JsonProperty("Sprite панели с заголовком")]
                    public string SpriteTitlePanel;
                    [JsonProperty("Цвет текста")]
                    public string HexLabel;
                    [JsonProperty("Цвет кнопки вердикт и задний фон причин")]
                    public string HexBanButton;
                    [JsonProperty("Цвет кнопки окончания проверки")]
                    public string HexStopButton;
                }

                internal class RaitingInterfaceSettings
                {
                    [JsonProperty("Цвет основной панели")]
                    public string HexMain;
                    [JsonProperty("Цвет панели с заголовком")]
                    public string HexTitlePanel;
                    [JsonProperty("Sprite панели с заголовком")]
                    public string SpriteTitlePanel;
                    [JsonProperty("Цвет текста")]
                    public string HexLabel;
                    [JsonProperty("Цвет иконок с рейтингом")]
                    public string HexRaitingButton;
                    [JsonProperty("Sprite рейтинга")]
                    public string SpriteRaiting;
                }

            }

        }

        public static Configuration GetNewConfiguration();
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    public Dictionary<ulong, PlayerInfo> ReportInformation;
    public Dictionary<ulong, ModeratorInfo> ModeratorInformation;
    public class PlayerInfo
    {
        [JsonProperty("Отображаемое имя")]
        public string DisplayName;
        [JsonProperty("IP Адреса")]
        public List<string> IP;
        [JsonProperty("Последняя жалоба")]
        public string LastReport;
        [JsonProperty("Последний проверяющий модератор")]
        public string LastCheckModerator;
        [JsonProperty("Количество проверок")]
        public int CheckCount;
        [JsonProperty("История жалоб")]
        public List<string> ReportHistory;
        [JsonProperty("Количество жалоб")]
        public int ReportCount;
        [JsonProperty("Игровой статус")]
        public string GameStatus;
    }

    public class ModeratorInfo
    {
        [JsonProperty("Проверки игроков с вердиктами")]
        public Dictionary<string, string> CheckPlayerModerator;
        [JsonProperty("Блокировки игроков с вердиктом")]
        public Dictionary<string, string> BanPlayerModerator;
        [JsonProperty("Общее количество проверок")]
        public int CheckCount;
        [JsonProperty("История оценок модератора")]
        public List<int> Arrayrating;
        [JsonProperty("Средняя оценка качества")]
        public float AverageRating;
    }

     void Metods_PlayerConnected(BasePlayer player);
     void Metods_Report(BasePlayer target, int ReasonIndex);
     void Metods_GiveCooldown(ulong ID, int cooldown);
     bool Metods_GetCooldown(ulong ID);
     void Metods_CheckModeration(BasePlayer Suspect, BasePlayer Moderator);
     void Metods_CheckModerationFinish(BasePlayer moderator, ulong SuspectID);
     void Metods_StatusNetwork(BasePlayer Suspect, string Reason);
    public Timer ModerTimeOutTimer;
     void Metods_ModeratorExitCheck(BasePlayer Moderator);
     void Metods_ModeratorBanned(BasePlayer Moderator, ulong SuspectID, int i);
     void Metods_CheckStopInAFK(BasePlayer moderator, string ID);
    public Dictionary<ulong,int> AFKCheck;
     void Metods_AFK(ulong SuspectID, BasePlayer moderator);
    readonly Hash<string, GenericPosition> lastPosition;
     void SavePositionAFK(IPlayer Suspect, BasePlayer moderator, int num);
    [ChatCommand("report")]
     void ReportChatCommand(BasePlayer player, string command, string[] args);
    [ConsoleCommand("report.list")]
     void ReportList(ConsoleSystem.Arg arg);
    [ChatCommand("discord")]
     void SendDiscord(BasePlayer Suspect, string command, string[] args);
    [ConsoleCommand("call")]
     void CallAdminCheck(ConsoleSystem.Arg arg);
    [ConsoleCommand("report")]
     void ReportCommand(ConsoleSystem.Arg arg);
    [ConsoleCommand("iqreport")]
     void IQReportSystemCommands(ConsoleSystem.Arg arg);
    public float GetAverageRaiting(ulong userID);
    private new void LoadDefaultMessages();
    public static string PARENT_UI;
    public static string PARENT_UI_REPORT_MENU;
    public static string PARENT_UI_PLAYER_PANEL;
    public static string PARENT_UI_PLAYER_REPORT;
    public static string PARENT_UI_MODER_REPORT;
    public static string PARENT_UI_ALERT_SEND;
    public static string PARENT_UI_MODERATOR_MINI_PANEL;
    private static string UI_MODERATION_CHECK_MENU_DISCORD;
    private static string UI_MODERATION_CHECK_MENU_NETWORK;
    private static string UI_MODERATION_RAITING;
     void UI_Interface(BasePlayer player);
     void UI_PanelReportsPlayer(BasePlayer player, bool Moderation);
     void UI_Player_Loaded(BasePlayer player, bool Moderation, int Page, string TargetName);
     void UI_SendReport(BasePlayer player, BasePlayer Suspect);
     void UI_ModerReport(BasePlayer player, BasePlayer Suspect);
     void UI_AlertSendPlayer(BasePlayer Suspect);
     void UI_MiniPanelModerator(BasePlayer player, ulong SuspectID);
     void UI_OpenReasonsBan(BasePlayer player, ulong SuspectID);
     void UI_RaitingSend(BasePlayer player, BasePlayer Moderator);
    private void OnServerInitialized();
     void OnPlayerInit(BasePlayer player);
    private void Unload();
     void OnPlayerDisconnected(BasePlayer player, string reason);
     void DestroyAll(BasePlayer player);
     void VKSendMessage(string Message);
     void DiscordSendMessage(string key, ulong userID, object[] args);
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
     int API_GET_REPORT_COUNT(ulong UserID);
     int API_GET_CHECK_COUNT(ulong UserID);
     List<string> API_GET_LIST_API(ulong UserID);
     string API_GET_GAME_STATUS(ulong UserID);
     string API_GET_LAST_CHECK_MODERATOR(ulong UserID);
     string API_GET_LAST_REPORT(ulong UserID);
     List<string> API_GET_REPORT_HISTORY(ulong UserID);
    public void SendChat(BasePlayer player, string Message);
    private static string HexToRustFormat(string hex);
     string IsSteam(string id);
    static DateTime epoch;
    static double CurrentTime();
    public static string FormatTime(TimeSpan time);
    private static string Format(int units, string form1, string form2, string form3);
}

public class PlayerSaveCheckClass
{
    public string Discord;
    public string NickName;
    public string StatusNetwork;
    public ulong ModeratorID;
}

private class Response
{
    public List<string> last_ip;
    public string last_nick;
    public List<ulong> another_accs;
    public List<last_checks> last_check;
    public class last_checks
    {
        public ulong moderSteamID;
        public string serverName;
        public int time;
    }

    public List<RustCCBans> bans;
    public class RustCCBans
    {
        public int banID;
        public string reason;
        public string serverName;
        public int OVHserverID;
        public int banDate;
    }

}

public class last_checks
{
    public ulong moderSteamID;
    public string serverName;
    public int time;
}

public class RustCCBans
{
    public int banID;
    public string reason;
    public string serverName;
    public int OVHserverID;
    public int banDate;
}

public class Configuration
{
    [JsonProperty("Основные настройки")]
    public Settings Setting;
    [JsonProperty("Причины репорта")]
    public List<string> ReasonReport;
    [JsonProperty("Причины блокировки")]
    public List<BanReason> ReasonBan;
    [JsonProperty("Настройки RustCheatCheck(Будет при проверке выдавать доступ в чекер и выводить информацию модератору)")]
    public RCCSettings RCCSetting;
    [JsonProperty("Настройка репутации для проверяющих")]
    public RaitingSettings RaitingSetting;
    internal class BanReason
    {
        [JsonProperty("Название")]
        public string DisplayName;
        [JsonProperty("Команда")]
        public string Command;
    }

    internal class RCCSettings
    {
        [JsonProperty("Включить поддержку RCC")]
        public bool RCCUse;
        [JsonProperty("Ключ от RCC")]
        public string Key;
    }

    internal class RaitingSettings
    {
        [JsonProperty("Сколько репутации снимать за 1-2 звезды(IQPersonal)")]
        public int RemoveAmountOneTwo;
        [JsonProperty("Сколько репутации давать за 3-4 звезды(IQPersonal)")]
        public int GiveAmountThreeFour;
        [JsonProperty("Сколько репутации давать за 5 звезд(IQPersonal)")]
        public int GiveAmountFive;
    }

    internal class Settings
    {
        [JsonProperty("Настройки IQChat")]
        public ChatSettings ChatSetting;
        [JsonProperty("Настройки интерфейса")]
        public InterfaceSetting Interface;
        [JsonProperty("Включить/отключить оповещение о максимальном кол-во репортов")]
        public bool MaxReportAlert;
        [JsonProperty("Максимальное количество репортов")]
        public int MaxReport;
        [JsonProperty("Перезарядка для отправки репорта(секунды)")]
        public int CooldownTime;
        [JsonProperty("Запретить друзьям репортить друг друга")]
        public bool FriendNoReport;
        [JsonProperty("Включить логирование в беседу ВК")]
        public bool VKMessage;
        [JsonProperty("Включить логирование в Discord")]
        public bool DiscrodMessage;
        [JsonProperty("Webhooks для дискорда")]
        public string WebHook;
        [JsonProperty("Настройки ВК")]
        public VKSetting VKSettings;
        internal class ChatSettings
        {
            [JsonProperty("IQChat : Кастомный префикс в чате")]
            public string CustomPrefix;
            [JsonProperty("IQChat : Кастомный аватар в чате(Если требуется)")]
            public string CustomAvatar;
        }

        internal class VKSetting
        {
            [JsonProperty("Токен от группы ВК(От группы будут идти сообщения в беседу.Вам нужно добавить свою группу в беседу!)")]
            public string Token;
            [JsonProperty("ID беседы для группы")]
            public string ChatID;
        }

        internal class InterfaceSetting
        {
            [JsonProperty("Настройка интерфейса для отправки жалобы")]
            public ReasonInterfaceSetting ReasonInterface;
            [JsonProperty("Настройка интерфейса для уведомления")]
            public AlertInterfaceSettings AlertInterface;
            [JsonProperty("Настройка интерфейса для мини-панели модератора")]
            public ModeratorPanelInterfaceSettings ModderatorPanel;
            [JsonProperty("Настройка интерфейса для рейтинга")]
            public RaitingInterfaceSettings RaitingInterface;
            [JsonProperty("Sprite для рейтинга(звезды)")]
            public string SpriteRaiting;
            [JsonProperty("Sprite для кнопки жалоб в панели модератора")]
            public string SpriteReportModeration;
            [JsonProperty("Sprite для иконки жалоб")]
            public string SpriteReport;
            [JsonProperty("Цвет текста в плагине")]
            public string HexLabels;
            [JsonProperty("Цвет боковой панели")]
            public string HexRightMenu;
            [JsonProperty("Цвет кнопок боковой панели")]
            public string HexButtonRightMenu;
            [JsonProperty("Цвет основной панели")]
            public string HexMainPanel;
            [JsonProperty("Цвет панели поиска и заднего фона игроков")]
            public string HexSearchPanel;
            [JsonProperty("Цвет кнопки у игрока для перехода к действию")]
            public string HexPlayerButton;
            [JsonProperty("Sprite кнопки у игрока для преехода к действию")]
            public string SpritePlayerButton;
            internal class ReasonInterfaceSetting
            {
                [JsonProperty("Цвет основной панели")]
                public string HexMain;
                [JsonProperty("Цвет панели с заголовком")]
                public string HexTitlePanel;
                [JsonProperty("Цвет жалоб")]
                public string HexButton;
                [JsonProperty("Цвет текста с жалобами")]
                public string HexLabel;
                [JsonProperty("Sprite кнопки закрыть")]
                public string SpriteClose;
                [JsonProperty("Sprite панели с заголовком")]
                public string SpriteTitlePanel;
                [JsonProperty("Цвет кнопки закрыть")]
                public string HexClose;
            }

            internal class AlertInterfaceSettings
            {
                [JsonProperty("Цвет основной панели")]
                public string HexMain;
                [JsonProperty("Цвет заголовка и полоски")]
                public string HexTitle;
                [JsonProperty("Цвет текста")]
                public string HexLabel;
            }

            internal class ModeratorPanelInterfaceSettings
            {
                [JsonProperty("Цвет основной панели")]
                public string HexMain;
                [JsonProperty("Цвет панели с заголовком")]
                public string HexTitlePanel;
                [JsonProperty("Sprite панели с заголовком")]
                public string SpriteTitlePanel;
                [JsonProperty("Цвет текста")]
                public string HexLabel;
                [JsonProperty("Цвет кнопки вердикт и задний фон причин")]
                public string HexBanButton;
                [JsonProperty("Цвет кнопки окончания проверки")]
                public string HexStopButton;
            }

            internal class RaitingInterfaceSettings
            {
                [JsonProperty("Цвет основной панели")]
                public string HexMain;
                [JsonProperty("Цвет панели с заголовком")]
                public string HexTitlePanel;
                [JsonProperty("Sprite панели с заголовком")]
                public string SpriteTitlePanel;
                [JsonProperty("Цвет текста")]
                public string HexLabel;
                [JsonProperty("Цвет иконок с рейтингом")]
                public string HexRaitingButton;
                [JsonProperty("Sprite рейтинга")]
                public string SpriteRaiting;
            }

        }

    }

    public static Configuration GetNewConfiguration();
}

internal class BanReason
{
    [JsonProperty("Название")]
    public string DisplayName;
    [JsonProperty("Команда")]
    public string Command;
}

internal class RCCSettings
{
    [JsonProperty("Включить поддержку RCC")]
    public bool RCCUse;
    [JsonProperty("Ключ от RCC")]
    public string Key;
}

internal class RaitingSettings
{
    [JsonProperty("Сколько репутации снимать за 1-2 звезды(IQPersonal)")]
    public int RemoveAmountOneTwo;
    [JsonProperty("Сколько репутации давать за 3-4 звезды(IQPersonal)")]
    public int GiveAmountThreeFour;
    [JsonProperty("Сколько репутации давать за 5 звезд(IQPersonal)")]
    public int GiveAmountFive;
}

internal class Settings
{
    [JsonProperty("Настройки IQChat")]
    public ChatSettings ChatSetting;
    [JsonProperty("Настройки интерфейса")]
    public InterfaceSetting Interface;
    [JsonProperty("Включить/отключить оповещение о максимальном кол-во репортов")]
    public bool MaxReportAlert;
    [JsonProperty("Максимальное количество репортов")]
    public int MaxReport;
    [JsonProperty("Перезарядка для отправки репорта(секунды)")]
    public int CooldownTime;
    [JsonProperty("Запретить друзьям репортить друг друга")]
    public bool FriendNoReport;
    [JsonProperty("Включить логирование в беседу ВК")]
    public bool VKMessage;
    [JsonProperty("Включить логирование в Discord")]
    public bool DiscrodMessage;
    [JsonProperty("Webhooks для дискорда")]
    public string WebHook;
    [JsonProperty("Настройки ВК")]
    public VKSetting VKSettings;
    internal class ChatSettings
    {
        [JsonProperty("IQChat : Кастомный префикс в чате")]
        public string CustomPrefix;
        [JsonProperty("IQChat : Кастомный аватар в чате(Если требуется)")]
        public string CustomAvatar;
    }

    internal class VKSetting
    {
        [JsonProperty("Токен от группы ВК(От группы будут идти сообщения в беседу.Вам нужно добавить свою группу в беседу!)")]
        public string Token;
        [JsonProperty("ID беседы для группы")]
        public string ChatID;
    }

    internal class InterfaceSetting
    {
        [JsonProperty("Настройка интерфейса для отправки жалобы")]
        public ReasonInterfaceSetting ReasonInterface;
        [JsonProperty("Настройка интерфейса для уведомления")]
        public AlertInterfaceSettings AlertInterface;
        [JsonProperty("Настройка интерфейса для мини-панели модератора")]
        public ModeratorPanelInterfaceSettings ModderatorPanel;
        [JsonProperty("Настройка интерфейса для рейтинга")]
        public RaitingInterfaceSettings RaitingInterface;
        [JsonProperty("Sprite для рейтинга(звезды)")]
        public string SpriteRaiting;
        [JsonProperty("Sprite для кнопки жалоб в панели модератора")]
        public string SpriteReportModeration;
        [JsonProperty("Sprite для иконки жалоб")]
        public string SpriteReport;
        [JsonProperty("Цвет текста в плагине")]
        public string HexLabels;
        [JsonProperty("Цвет боковой панели")]
        public string HexRightMenu;
        [JsonProperty("Цвет кнопок боковой панели")]
        public string HexButtonRightMenu;
        [JsonProperty("Цвет основной панели")]
        public string HexMainPanel;
        [JsonProperty("Цвет панели поиска и заднего фона игроков")]
        public string HexSearchPanel;
        [JsonProperty("Цвет кнопки у игрока для перехода к действию")]
        public string HexPlayerButton;
        [JsonProperty("Sprite кнопки у игрока для преехода к действию")]
        public string SpritePlayerButton;
        internal class ReasonInterfaceSetting
        {
            [JsonProperty("Цвет основной панели")]
            public string HexMain;
            [JsonProperty("Цвет панели с заголовком")]
            public string HexTitlePanel;
            [JsonProperty("Цвет жалоб")]
            public string HexButton;
            [JsonProperty("Цвет текста с жалобами")]
            public string HexLabel;
            [JsonProperty("Sprite кнопки закрыть")]
            public string SpriteClose;
            [JsonProperty("Sprite панели с заголовком")]
            public string SpriteTitlePanel;
            [JsonProperty("Цвет кнопки закрыть")]
            public string HexClose;
        }

        internal class AlertInterfaceSettings
        {
            [JsonProperty("Цвет основной панели")]
            public string HexMain;
            [JsonProperty("Цвет заголовка и полоски")]
            public string HexTitle;
            [JsonProperty("Цвет текста")]
            public string HexLabel;
        }

        internal class ModeratorPanelInterfaceSettings
        {
            [JsonProperty("Цвет основной панели")]
            public string HexMain;
            [JsonProperty("Цвет панели с заголовком")]
            public string HexTitlePanel;
            [JsonProperty("Sprite панели с заголовком")]
            public string SpriteTitlePanel;
            [JsonProperty("Цвет текста")]
            public string HexLabel;
            [JsonProperty("Цвет кнопки вердикт и задний фон причин")]
            public string HexBanButton;
            [JsonProperty("Цвет кнопки окончания проверки")]
            public string HexStopButton;
        }

        internal class RaitingInterfaceSettings
        {
            [JsonProperty("Цвет основной панели")]
            public string HexMain;
            [JsonProperty("Цвет панели с заголовком")]
            public string HexTitlePanel;
            [JsonProperty("Sprite панели с заголовком")]
            public string SpriteTitlePanel;
            [JsonProperty("Цвет текста")]
            public string HexLabel;
            [JsonProperty("Цвет иконок с рейтингом")]
            public string HexRaitingButton;
            [JsonProperty("Sprite рейтинга")]
            public string SpriteRaiting;
        }

    }

}

internal class ChatSettings
{
    [JsonProperty("IQChat : Кастомный префикс в чате")]
    public string CustomPrefix;
    [JsonProperty("IQChat : Кастомный аватар в чате(Если требуется)")]
    public string CustomAvatar;
}

internal class VKSetting
{
    [JsonProperty("Токен от группы ВК(От группы будут идти сообщения в беседу.Вам нужно добавить свою группу в беседу!)")]
    public string Token;
    [JsonProperty("ID беседы для группы")]
    public string ChatID;
}

internal class InterfaceSetting
{
    [JsonProperty("Настройка интерфейса для отправки жалобы")]
    public ReasonInterfaceSetting ReasonInterface;
    [JsonProperty("Настройка интерфейса для уведомления")]
    public AlertInterfaceSettings AlertInterface;
    [JsonProperty("Настройка интерфейса для мини-панели модератора")]
    public ModeratorPanelInterfaceSettings ModderatorPanel;
    [JsonProperty("Настройка интерфейса для рейтинга")]
    public RaitingInterfaceSettings RaitingInterface;
    [JsonProperty("Sprite для рейтинга(звезды)")]
    public string SpriteRaiting;
    [JsonProperty("Sprite для кнопки жалоб в панели модератора")]
    public string SpriteReportModeration;
    [JsonProperty("Sprite для иконки жалоб")]
    public string SpriteReport;
    [JsonProperty("Цвет текста в плагине")]
    public string HexLabels;
    [JsonProperty("Цвет боковой панели")]
    public string HexRightMenu;
    [JsonProperty("Цвет кнопок боковой панели")]
    public string HexButtonRightMenu;
    [JsonProperty("Цвет основной панели")]
    public string HexMainPanel;
    [JsonProperty("Цвет панели поиска и заднего фона игроков")]
    public string HexSearchPanel;
    [JsonProperty("Цвет кнопки у игрока для перехода к действию")]
    public string HexPlayerButton;
    [JsonProperty("Sprite кнопки у игрока для преехода к действию")]
    public string SpritePlayerButton;
    internal class ReasonInterfaceSetting
    {
        [JsonProperty("Цвет основной панели")]
        public string HexMain;
        [JsonProperty("Цвет панели с заголовком")]
        public string HexTitlePanel;
        [JsonProperty("Цвет жалоб")]
        public string HexButton;
        [JsonProperty("Цвет текста с жалобами")]
        public string HexLabel;
        [JsonProperty("Sprite кнопки закрыть")]
        public string SpriteClose;
        [JsonProperty("Sprite панели с заголовком")]
        public string SpriteTitlePanel;
        [JsonProperty("Цвет кнопки закрыть")]
        public string HexClose;
    }

    internal class AlertInterfaceSettings
    {
        [JsonProperty("Цвет основной панели")]
        public string HexMain;
        [JsonProperty("Цвет заголовка и полоски")]
        public string HexTitle;
        [JsonProperty("Цвет текста")]
        public string HexLabel;
    }

    internal class ModeratorPanelInterfaceSettings
    {
        [JsonProperty("Цвет основной панели")]
        public string HexMain;
        [JsonProperty("Цвет панели с заголовком")]
        public string HexTitlePanel;
        [JsonProperty("Sprite панели с заголовком")]
        public string SpriteTitlePanel;
        [JsonProperty("Цвет текста")]
        public string HexLabel;
        [JsonProperty("Цвет кнопки вердикт и задний фон причин")]
        public string HexBanButton;
        [JsonProperty("Цвет кнопки окончания проверки")]
        public string HexStopButton;
    }

    internal class RaitingInterfaceSettings
    {
        [JsonProperty("Цвет основной панели")]
        public string HexMain;
        [JsonProperty("Цвет панели с заголовком")]
        public string HexTitlePanel;
        [JsonProperty("Sprite панели с заголовком")]
        public string SpriteTitlePanel;
        [JsonProperty("Цвет текста")]
        public string HexLabel;
        [JsonProperty("Цвет иконок с рейтингом")]
        public string HexRaitingButton;
        [JsonProperty("Sprite рейтинга")]
        public string SpriteRaiting;
    }

}

internal class ReasonInterfaceSetting
{
    [JsonProperty("Цвет основной панели")]
    public string HexMain;
    [JsonProperty("Цвет панели с заголовком")]
    public string HexTitlePanel;
    [JsonProperty("Цвет жалоб")]
    public string HexButton;
    [JsonProperty("Цвет текста с жалобами")]
    public string HexLabel;
    [JsonProperty("Sprite кнопки закрыть")]
    public string SpriteClose;
    [JsonProperty("Sprite панели с заголовком")]
    public string SpriteTitlePanel;
    [JsonProperty("Цвет кнопки закрыть")]
    public string HexClose;
}

internal class AlertInterfaceSettings
{
    [JsonProperty("Цвет основной панели")]
    public string HexMain;
    [JsonProperty("Цвет заголовка и полоски")]
    public string HexTitle;
    [JsonProperty("Цвет текста")]
    public string HexLabel;
}

internal class ModeratorPanelInterfaceSettings
{
    [JsonProperty("Цвет основной панели")]
    public string HexMain;
    [JsonProperty("Цвет панели с заголовком")]
    public string HexTitlePanel;
    [JsonProperty("Sprite панели с заголовком")]
    public string SpriteTitlePanel;
    [JsonProperty("Цвет текста")]
    public string HexLabel;
    [JsonProperty("Цвет кнопки вердикт и задний фон причин")]
    public string HexBanButton;
    [JsonProperty("Цвет кнопки окончания проверки")]
    public string HexStopButton;
}

internal class RaitingInterfaceSettings
{
    [JsonProperty("Цвет основной панели")]
    public string HexMain;
    [JsonProperty("Цвет панели с заголовком")]
    public string HexTitlePanel;
    [JsonProperty("Sprite панели с заголовком")]
    public string SpriteTitlePanel;
    [JsonProperty("Цвет текста")]
    public string HexLabel;
    [JsonProperty("Цвет иконок с рейтингом")]
    public string HexRaitingButton;
    [JsonProperty("Sprite рейтинга")]
    public string SpriteRaiting;
}

public class PlayerInfo
{
    [JsonProperty("Отображаемое имя")]
    public string DisplayName;
    [JsonProperty("IP Адреса")]
    public List<string> IP;
    [JsonProperty("Последняя жалоба")]
    public string LastReport;
    [JsonProperty("Последний проверяющий модератор")]
    public string LastCheckModerator;
    [JsonProperty("Количество проверок")]
    public int CheckCount;
    [JsonProperty("История жалоб")]
    public List<string> ReportHistory;
    [JsonProperty("Количество жалоб")]
    public int ReportCount;
    [JsonProperty("Игровой статус")]
    public string GameStatus;
}

public class ModeratorInfo
{
    [JsonProperty("Проверки игроков с вердиктами")]
    public Dictionary<string, string> CheckPlayerModerator;
    [JsonProperty("Блокировки игроков с вердиктом")]
    public Dictionary<string, string> BanPlayerModerator;
    [JsonProperty("Общее количество проверок")]
    public int CheckCount;
    [JsonProperty("История оценок модератора")]
    public List<int> Arrayrating;
    [JsonProperty("Средняя оценка качества")]
    public float AverageRating;
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

