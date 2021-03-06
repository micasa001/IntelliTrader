<img src="http://intellitrader.io/img/logo.png" alt="logo" width="40" height="40"> IntelliTrader
===========

Overview
-------------

IntelliTrader is a fully featured, highly configurable trading bot for cryptocurrency exchanges. It is completely free, so please use it at your own risk. Always start by [virtual trading](#virtual-trading) first. Join our [Discord channel](https://discord.gg/rqfpn5a) to get additional help, support and participate in discussions.

##### Main Features

* Cross-platform
* Highly configurable
* Easy to use, fast and lightweight
* Ability to adjust to different market conditions
* Virtual/paper trading support
* Backtesting support

##### Additional Resources
* <img src="http://intellitrader.io/img/logo.png" alt="logo" width="20" height="22"> [Official Website](http://intellitrader.io)
* <img src="http://intellitrader.io/img/discord_icon.png" alt="logo" width="20" height="22"> [Discord Channel](https://discord.gg/rqfpn5a)
* <img src="http://intellitrader.io/img/youtube_icon.png" alt="logo" width="20" height="22"> [Youtube Channel](https://www.youtube.com/channel/UC8Gvv0ArdF9a2CHUPTdqkjg)
* <img src="http://intellitrader.io/img/medium_icon.png" alt="logo" width="20" height="22"> [Medium](https://medium.com/@intellitrader.io/)

Getting Started
-------------

#### Prerequisites

###### Windows, Linux & MacOS
Download and install .NET Core Runtime 2.1 from [Microsoft](https://www.microsoft.com/net/download/all).

#### Building

You don't usually need to build the bot yourself, in most cases it's easier to grab the latest published released from [the releases page](https://github.com/jazzonaut/IntelliTrader/releases). But if you are feeling particularly adventurous, here are the steps:

1. Clone the IntelliTrader repository locally
2. From Git terminal, run 'git submodule update --init --recursive' to pull all the necessary submodules
3. Run Publish.bat (or Publish.sh if you are on Linux) and it should generate the bin folder in the Publish directory.
4. That should be it, try running the bot with Start-IntelliTrader to see if it worked.

#### Setting Up
The bot should just run with out of the box settings. By default, it is configured for virtual trading, so there is no need to provide any API keys at the start. The only thing you might want to change is the default port for the web interface (7000).
Refer to [web configuration](#web-configuration) section for information on how to do this.

#### Running

###### Windows
Simply run Start-IntelliTrader.bat to start the bot.
###### Linux & MacOS
Copy the *.sh files from the linux directory supplied with the package to the root directory of the package and run them.

#### Supported Exchanges
Currenly only Binance Exchange is supported.

Configuration
-------------

All changes to the configuration files will take effect immediately.

Configuration quick links:
[Value Types](#value-types), [Core](#core-configuration), [Web](#web-configuration), [Signals](#signals-configuration), [Trading](#trading-configuration), [Rules](#rules-configuration), [Notification](#notification-configuration), [Backtesting](#backtesting-configuration), [Other](#other-configuration)

#### Value Types

|Type|Example|Description|
|-|:-:|-|
|Number|0.40|Numeric value|
|Boolean|true|Boolean value (true/false)|
|String|Market|String value|
|Array|[ "BNBBTC", "ETHBTC" ] |Array of numbers, booleans or strings|
|Object|{ "Key": "Value" }|Json object|

#### Core Configuration

|Setting|Type|Default Value|Description|
|-|:-:|:-:|-|
|DebugMode|Boolean|true|Enable / disable debug mode|
|PasswordProtected|Boolean|true|Use password authentication|
|Password|String|MD5 hash|MD5-encrypted password|
|InstanceName|String|Main|Must be alphanumeric and should not contain spaces. Used by the web interface & notification service to distinguish between different bot instances (when running multiple)|
|TimezoneOffset|Number|1|Timezone offset from UTC (in hours), used by stats|
|HealthCheckEnabled|Boolean|true|Enable / disable health check|
|HealthCheckInterval|Number|180|Interval to check that all the data is up to date and services are running correctly (in seconds)|
|HealthCheckSuspendTradingTimeout|Number|900|Suspend trading (disable sells and buys) if any of the health checks did not pass in a specified period of time (in seconds). Set to 0 to continue trading even after health check fails
|HealthCheckFailuresToRestartServices|Number|3|Restart all services when health check fails for a specified number of times in a row. Set to 0 to disable restart|

#### Web Configuration

Read more about how web interface works in the [web](#web-interface) section.

|Setting|Type|Default Value|Description|
|-|:-:|:-:|-|
|DebugMode|Boolean|true|Enable / disable debug mode|
|Enabled|Boolean|true|Enable / disable web interface|
|Port|Number|7000|Port on which to host the web interface|

#### Signals Configuration

Read more about how signals work in the [signals](#signals) section.

|Setting|Type|Default Value|Description|
|-|:-:|:-:|-|
|Enabled|Boolean|true|Enable / disable signals|
|GlobalRatingSignals|Array|"TV-5mins","TV-15mins","TV-60mins"|Signals to calculate the Global Rating from|
|Definitions|Array|[Signal Definitions](#signal-definitions)|Signal source definitions|

###### Signal Definitions

|Setting|Type|Default Value|Description|
|-|:-:|:-:|-|
|Name|String|"TV-15mins"|Signal name|
|Receiver|String|"TradingViewCryptoSignalReceiver"|Signal receiver name|
|Configuration|Object|[Signal Receiver Configuration](#signal-receiver-configuration)|Signal receiver configuration|

###### Signal Receiver Configuration

|Setting|Type|Default Value|Description|
|-|:-:|-|-|
|PollingInterval|Number|7|How often to check for latest signals (in seconds)|
|SignalPeriod|Number|15|Signal period (in minutes). Possible values: 5, 15, 60, 240, 1440|
|VolatilityPeriod|String|"Day"|Volatility period. Possible values: Day, Week, Month|
|RequestUrl|String|"https://scanner.tradingview.com/crypto/scan"|Request url|
|RequestData|String|"{\"filter\":[{\"left\":\"exchange\",\"operation\":\"equal\",\"right\":\"%EXCHANGE%\"},{\"left\":\"name\",\"operation\":\"match\",\"right\":\"%MARKET%\"}],\"columns\":[\"name\",\"close%PERIOD%\",\"change%PERIOD%\",\"volume%PERIOD%\",\"Recommend.All%PERIOD%\",\"Volatility%VOLATILITY%\"],\"options\":{\"lang\":\"en\"},\"range\":[0,500]}"|Request data|

#### Trading Configuration

Read more about how trading works in the [trading](#trading) section.

###### General

|Setting|Type|Default Value|Description|
|-|:-:|:-:|-|
|Enabled|Boolean|true|Enable / disable trading|
|Market|String|"BTC"|Market to trade on - BTC, ETH, USDT, etc.|
|Exchange|String|"Binance"|Exchange to trade on|
|MaxPairs|Number|16|Maximum pairs to trade with|
|MinCost|Number|0.000999|Ignore pairs with the specified market value or lower (dust)|
|ExcludedPairs|Array|[ "BNBBTC" ]|Pairs excluded from trading|

###### Buying

|Setting|Type|Default Value|Description|
|-|:-:|:-:|-|
|BuyEnabled|Boolean|true|Enable / disable buying|
|BuyType|String|"Market"|Supported types: Market. Limit is not currently supported|
|BuyMaxCost|Number|0.0012|Maximum cost when buying a new pair|
|BuyMultiplier|Number|1|Maximum cost multiplier|
|BuyMinBalance|Number|0|Minimum account balance to buy new pairs|
|BuySamePairTimeout|Number|900|Rebuy same pair timeout (in seconds)|
|BuyTrailing|Number|-0.15|Buy trailing percentage (should be a negative number, set to 0 to disable trailing)|
|BuyTrailingStopMargin|Number|0.05|Stop trailing and place buy order immediately when margin hits the specified value|
|BuyTrailingStopAction|String|"Buy"|Action to take after hitting the StopMargin. Possible values: Buy, Cancel|

###### Buying DCA

|Setting|Type|Default Value|Description|
|-|:-:|:-:|-|
|BuyDCAEnabled|Boolean|true|Enable / disable buying DCA|
|BuyDCAMultiplier|Number|1|Current cost multiplier. To double down set to 1|
|BuyDCAMinBalance|Number|0|Minimum account balance to DCA|
|BuyDCASamePairTimeout|Number|4200|Rebuy same pair timeout (in seconds)|
|BuyDCATrailing|Number|-1.50|Buy trailing percentage (should be a negative number, set to 0 to disable trailing)|
|BuyDCATrailingStopMargin|Number|0.40|Stop trailing and place buy order immediately when margin hits the specified value|
|BuyDCATrailingStopAction|String|"Cancel"|Action to take after hitting the StopMargin. Possible values: Buy, Cancel|

###### Selling

|Setting|Type|Default Value|Description|
|-|:-:|:-:|-|
|SellEnabled|Boolean|true|Enable / disable selling|
|SellType|String|"Market"|Supported types: Market. Limit is not currently supported|
|SellMargin|Number|2.00|Minimum percentage increase to start trailing|
|SellTrailing|Number|0.70|Sell trailing percentage (should be a positive number, set to 0 to disable trailing)|
|SellTrailingStopMargin|Number|1.70|Stop trailing and place sell order immediately when margin hits the specified value|
|SellTrailingStopAction|String|"Sell"|Action to take after hitting the StopMargin. Possible values: Sell, Cancel|
|SellStopLossEnabled|Boolean|false|Enable / disable stop loss trigger|
|SellStopLossAfterDCA|Boolean|true|Trigger stop loss only after all DCA levels has been reached|
|SellStopLossMinAge|Number|3.0|Minimum number of days needed before triggering the stop loss|
|SellStopLossMargin|Number|-20|Trigger stop loss and immediately place sell order at the specified percentage decrease|

###### Selling DCA

|Setting|Type|Default Value|Description|
|-|:-:|:-:|-|
|SellDCAMargin|Number|1.50|Minimum percentage increase to start trailing|
|SellDCATrailing|Number|0.50|Sell trailing percentage (set to 0 to disable trailing)
|SellDCATrailingStopMargin|Number|1.25|Stop trailing and place sell order immediately when margin hits the specified value|
|SellDCATrailingStopAction|String|"Sell"|Action to take after hitting the StopMargin. Possible values: Sell, Cancel|
|RepeatLastDCALevel|Boolean|fasle|Repeat the last DCA Level indefinitely, essentially making the DCA level number unlimited|
|DCALevels|Array|[DCA Levels](#dca-levels)|Action to take after hitting the StopMargin. Possible values: Sell, Cancel|

###### DCA Levels

Read more about how DCA works in the [DCA](#dca) section.

DCALevels setting is an array of DCA levels. There is no limit to the number of levels. The only mandatory setting for each level is Margin, which specifies when to trigger the DCA. All other settings are optional and when omitted will use the default values as defined above.

|Setting|Type|Default Value|Description|
|-|:-:|:-:|-|
|Margin|Number|-1.50|When to trigger the DCA|
|BuyMultiplier|Number|BuyDCAMultiplier|Current cost multiplier. To double down set to 1 (Optional)|
|BuySamePairTimeout|Number|BuyDCASamePairTimeout|Rebuy same pair timeout (in seconds) (Optional)|
|BuyTrailing|Number|BuyDCATrailing|Buy trailing percentage (should be a negative number, set to 0 to disable trailing) (Optional)|
|BuyTrailingStopMargin|Number|BuyDCATrailingStopMargin|Stop trailing and place buy order immediately when margin hits the specified value. (Optional)|
|BuyTrailingStopAction|String|BuyDCATrailingStopAction|Action to take after hitting the StopMargin. Possible values: Buy, Cancel (Optional)|
|SellMargin|Number|SellDCAMargin|Minimum percentage increase to start trailing (Optional)|
|SellTrailing|Number|SellDCATrailing|Sell trailing percentage (set to 0 to disable trailing) (Optional)|
|SellTrailingStopMargin|Number|SellDCATrailingStopMargin|Stop trailing and place buy order immediately when margin hits the specified value. (Optional)|
|SellTrailingStopAction|String|SellDCATrailingStopAction|Action to take after hitting the StopMargin. Possible values: Sell, Cancel (Optional)|

###### Accounts
|Setting|Type|Default Value|Description|
|-|:-:|:-:|-|
|TradingCheckInterval|Number|1|Tickers check frequency (in seconds)|
|AccountRefreshInterval|Number|360|Exchange account refresh interval (in seconds)|
|AccountInitialBalance|Number|0.12|Initial balance on the account, used for stats calculations|
|AccountInitialBalanceDate|String|"2018-04-08T00:00:00+00:00"|Date of the initial balance snapshot|
|AccountFilePath|String|"data/exchange-account.json"|Path to the account file|
|VirtualTrading|Boolean|true|Enable / disable virtual trading|
|VirtualAccountInitialBalance|Number|0.12|Initial balance on the virtual account, used for stats calculations|
|VirtualAccountFilePath|String|"data/virtual-account.json"|Path to the virtual account file|

#### Rules Configuration

Read more about how rules work in the [rules](#rules) section.

|Setting|Type|Default Value|Description|
|-|:-:|:-:|-|
|Modules|Array|[Module](#module)|Rule Modules|

###### Module

|Setting|Type|Default Value|Description|
|-|:-:|:-:|-|
|Module|String|"Module"|Module name|
|Configuration|Object|[Module Configuration](#module-configuration)|Module configuration|
|Entries|Array|[Rules Configuration](#rules-configuration)|Rules configuration|

###### Module Configuration

|Setting|Type|Default Value|Description|
|-|:-:|:-:|-|
|ProcessingMode|String|"AllMatches"|Available modes: AllMatches (all enabled rules will get processed) or FirstMatch (stop processing at the first rule that is satisfied)|
|CheckInterval|Number|3|Rules check frequency (in seconds)|

###### Rules Configuration

|Setting|Type|Default Value|Description|
|-|:-:|:-:|-|
|Enaled|Boolean|true|Enable / disable rule|
|Name|String|Default|Rule name|
|Modifiers|Object|[Rule Modifiers](#rule-modifiers)|Rule modifiers|
|Conditions|Array|[Rule Conditions](#rule-conditions)|Rule conditions|
|Trailing|Object|[Rule trailing](#rule-trailing)|Rule trailing|

###### Rule Modifiers

All modifiers (both signal and trading) are optional and can be omitted.  
Signal modifiers currently only support the *CostMultiplier* (MaxCost multiplier)  
Trading modifiers support any trading setting that begins with Buy, Sell or DCALevels in addition to the following trading-rule specific modifiers:

|Setting|Type|Default Value|Description|
|-|:-:|:-:|-|
|SwapEnabled|Boolean|true|Swap badly performing pairs for better performing ones|
|SwapSignalRules|Array|[ "Swap" ]|Rules used to buy the replacement pair with|
|SwapTimeout|Number|10800|How long to wait before making a swap (in seconds)|

###### Rule Conditions

Rule is considered satisfied when all the conditions are met. When trailing is enabled, its conditions must be met first.  
You can have multiple sets of conditions within a single rule.  
Is is not necessary to specify a Signal if none of the conditions are signal-specific.

|Setting|Type|Signal Specific|Description|
|-|:-:|:-:|-|
|Signal|String|N/A|Signal with which to calculate signal-specific conditions|
|MinGlobalRating|Number|No|Minimal global rating calculated from all individual coins ratings. Is a value between -1 and 1. Value -0.2 is considered a bearish market|
|MaxGlobalRating|Number|No|Maximum global rating calculated from all individual coins ratings. Is a value between -1 and 1. Value +0.2 is considered a bullish market|
|MinRating|Number|Yes|Minimal rating of a coin within the specified signal's period. Expect a value between -1 and 1. A good place to start is 0.3+|
|MaxRating|Number|Yes|Maximal ration of a coin within the specified signal's period. Expect a value between -1 and 1. In a normal market do not expect it to go over 0.6|
|MinRatingChange|Number|Yes|The minimal rate of change of coins rating (percentage). For a reference values let the bot run and then have a look in the dashboard|
|MaxRatingChange|Number|Yes|The maximal rate of change of coins rating (percentage). For a reference values let the bot run and then have a look in the dashboard|
|MinPrice|Number|No|Minimum current price of the coin|
|MaxPrice|Number|No|Maximum current price of the coin|
|MinPriceChange|Number|Yes|The minimal rate of change of price withing specified period frame (percentage)|
|MaxPriceChange|Number|Yes|The maximal rate of change of price withing specified period frame (percentage)|
|MinSpread|Number|No|Minimum difference between current bid and ask price (percentage)|
|MaxSpread|Number|No|Maximum difference between current bid and ask price (percentage)|
|MinVolume|Number|Yes|Minimum coin volume within the specified signal's period. Do not expect 24h volume in this category|
|MaxVolume|Number|Yes|Maximum coin volume within the specified signal's period. Do not expect 24h volume in this category|
|MinVolumeChange|Number|Yes|The minimal rate of change of volume withing specified period frame (percentage)|
|MaxVolumeChange|Number|Yes|The maximal rate of change of volume withing specified period frame (percentage)|
|MinVolatility|Number|Yes|Minimum average volatility of a coin within its own specified timeframe|
|MaxVolatility|Number|Yes|Maximum average volatility of a coin within its own specified timeframe|
|MinAge|Number|No|Minimum trading pair's age (in days, e.g. 1.5 is 36 hours)|
|MaxAge|Number|No|Maximum trading pair's age (in days, e.g. 1.5 is 36 hours)|
|MinLastBuyAge|Number|No|Minimum trading pair's age since last buy (in days, e.g. 1.5 is 36 hours)|
|MaxLastBuyAge|Number|No|Maximum trading pair's age since last buy (in days, e.g. 1.5 is 36 hours)|
|MinMargin|Number|No|Minimum trading pair's margin|
|MaxMargin|Number|No|Maximum trading pair's margin|
|MinMarginChange|Number|No|Minimum trading pair's margin change since last buy|
|MaxMarginChange|Number|No|Maximum trading pair's margin change since last buy|
|MinAmount|Number|No|Minimum trading pair's total purchase amount|
|MaxAmount|Number|No|Maximum trading pair's total purchase amount|
|MinCost|Number|No|Minimum trading pair's total current cost|
|MaxCost|Number|No|Maximum trading pair's total current cost|
|MinDCALevel|Number|No|Minimum trading pair's DCA level|
|MaxDCALevel|Number|No|Maximum trading pair's DCA level|
|Pairs|Array|No|List of pairs to directly apply the rule to|
|SignalRules|Array|No|List of signal rules that were used to buy a pair|

###### Rule Trailing

Trailing is optional and is only supported by the Signal Rules.   
Trailing starts when all the Rule Trailing StartConditions are met.
Trailing ends when all the conditions of the rule are met, at any point between MinDuration and MaxDuration.

|Setting|Type|Default Value|Description|
|-|:-:|:-:|-|
|Enabled|Boolean|true|Enable / disable trailing|
|MinDuration|Number|25|Minimum trail duration (in seconds)|
|MaxDuration|Number|240|Maximum trail duration (in seconds)|
|StartConditions|Array|[Rule Conditions](#rule-conditions)|Begin trailing when all the below conditions are met.|

#### Notification Configuration

Read more about how nofitications work in the [notifications](#notifications) section.

|Setting|Type|Default Value|Description|
|-|:-:|:-:|-|
|Enabled|Boolean|false|Enable / disable notifications|
|TelegramEnabled|Boolean|true|Enabled / disable Telegram notifications|
|TelegramBotToken|String|-|Your Telegram bot's token|
|TelegramChatId|Number|-|Your Telegram chat id|
|TelegramAlertsEnabled|Boolean|true|Enable phone alerts with Telegram messages|

#### Backtesting Configuration

Read more about how backtesting works in the [backtesting](#backtesting) section.

|Setting|Type|Default Value|Description|
|-|:-:|:-:|-|
|Enabled|Boolean|false|Enable / disable backtesting|
|Replay|Boolean|false|Enabled / disable snapshots replay|
|ReplayOutput|Boolean|true|Display replay output in the log|
|ReplaySpeed|Number|500|Replay speed (1 = SnapshotInterval speed)|
|ReplayStartIndex|Number|null|Snapshot index to start replay with|
|ReplayEndIndex|Number|null|Snapshot index to end replay with|
|DeleteLogs|Boolean|false|Delete all existing logs before running backtesting|
|DeleteAccountData|Boolean|false|Delete account data before running backtesting|
|CopyAccountDataPath|String|null|Path to copy existing account data file from for backtesting|
|SnapshotsInterval|Number|1|How often to take snapshots (in seconds)|
|SnapshotsPath|String|data/backtesting|Where to save the new snapshots or to load existing ones when replaying|

#### Other Configuration

###### Exchange

Exchange-specific configuration. Don't change the default values unless you know what you're doing.

###### Logging

Logging configuration. Here you can disable logging completely, change the default log file paths, verbosity and format.

###### Paths

Paths to all the configuration files. Do not change unless you know what you're doing.

###### Caching

Currently not in use.

###### Integration

Currently not in use.

Trading
--------------

There are two ways you can trade with IntelliTrader. Virtual trading is enabled on by default.  
DO NOT switch to live trading until you are fully confident with the bot and are familiar with the way it does trading.  
Conceptually there is no difference between virtual and live trading, so virtual trading is a very good way to learn the ins and outs, experiment, and try out new settings.

#### Virtual Trading

Virtual trading is enabled by default. You don't need to provide your API Key to virtual trade.  
To enable virtual trading, set *VirtualTrading* to true in config\trading.json. 
You might want to change *VirtualAccountInitialBalance* to reflect your starting balance for testing. 

#### Live Trading

To trade on an exchange, first you need to create an encrypted file that will hold your API keys. 
Open Encrypt-Keys.bat and change publickey to your API key and privatekey to your API secret, then run it.  
You should now have the generated keys.bin file in your IntelliTrader directory. This file contains your encrypted API keys and it is only valid for the current user and only on the computer it is created on.  
Important: Make sure to remove your keys from Encrypt-Keys.bat file after generating the keys.bin. Now change *VirtualTrading* to false in config\trading.json. 
Also, to make the profit stats accurate, you need to set *AccountInitialBalance* to your current BTC/ETH balance (depending on the market you use). 
That's it, you are ready for exchange trading!

#### Trailing

When all the buy conditions are true, then the bot is in its final phase where it tries to find the bottom with the help of trailing. The bot is watching the price of a coin closely. The price needs to fall and then rise by at least the percentage specified for trailing in order to make a buy.

Example trailing story for value -1 (percent):

*The coin keeps falling more than 3 percent. It then rises by 0.7 percent. This move is smaller than 1 percent, meaning the bot does nothing and the trailing continues. After another drop the coin jumps 1.5 percent, so the bot will buy because the trailing has exceeded our buy value.*

#### Signals

Signals are used to buy new pairs based on the predefined rules.

Read more about how signal rules work in the [signal rules](#signal-rules) section.

#### Global Rating

An average value of all the ratings combined (for current market & exchange)

#### DCA

Buy additional position at a lower price than the original purchase price. This brings the average price you've paid the pair down.

#### Stop Loss

Sell a pair at a specified negative margin to avoid it dipping even lower.

#### Pair Swapping

Swap badly performing pairs for better performing ones.
You would need at least one signal rule and one trading rule to enable this feature. The trading rule will enable swapping feature for the specific pairs (e.g. when the pair is old, has low margin, low rating, etc.) and the signal rule will determine the conditions for which pairs to buy instead of the swapped ones.

#### Backtesting

Backtesting works by taking snapshots of signals and exchange tickers over a period of time (the longer the period the better) and then replaing them at high speed to the bot. Essentially what this means is that you could snapshot a week's worth of data and then replay it in 10 minutes, saving yourself a lot of time. Snapshots can be reused for as many times as you like and with any settings you wish.

Rules
-------------

#### Signal Rules

Signal Rules are used to buy new pairs based on the specific conditions. This can also optionally have trailing.
If enabled then Signal Trailing in IntelliTrader is the first stage of buying where it will trail a coin based on the settings you specify for the time duration's you specify. If a coin matches the trailing conditions it then checks the conditions of the coin and buys the coin if these are met. 
If trailing isn't enabled it buys based only on the specific conditions set.

Add "Action": "Swap" if you want the signal rule to only be used for pair swapping.

#### Trading Rules

Trading Rules apply to Pairs you already hold. Trailing is not available for trading rules. Conditions for Trading rules work the same way as in signal rules.
Available modifiers for Trading Rules are any trading.json setting that begins with Buy, Sell or DCALevels.
This is also where you would setup your Swap Specific Rules.

Web Interface
-------------

To access the web interface, simply open http://localhost:7000 in your browser (or replace 7000 with the port you have configured).

#### Status Bar

Status bar contains information about your available balance, current global rating, trailing buys/sells/signals and the current status. Hover over the status icon (ON) to see the latest health check information.

#### Dashboard

This is the default screen that IntelliTrader starts at. The table shows all the pairs that you currently hold. The data automatically refreshes every 5 seconds.

Columns can be adjusted. You can reorder them, move them around and even remove the ones that you don't need. There are some options for exporting the data: you can copy or export it to a spreadsheet.

You can click on the table rows to expand them and access additional options. From there you can manually buy, sell or view the current pair's settings. 

The log button will display the last 5 lines from the log file.

|Column|Description|
|-|-|
|More|Displayed on low resolution screens and when clicked shows additional data|
|Pair #1|Pair's name|
|Pair #2|Pair's name, including the current DCA level|
|DCA|Current DCA level|
|Margin|Current profit percentage. If you sold right now, this is what you would make, approximately|
|Target|Target sell point in profit. Also known as the profit margin|
|Rating|Pair's current rating based on the signals source, Green means it is currently higher than the purchase rating|
|Rating Bought|Rating that the pair was purchased at|
|Age|How long a pair has been held for|
|Amount|Total amount of the pair you currently hold|
|Cost|Current value of the pair|
|Cost Bought|Purchase value of the pair|
|Price|Current price of the pair on the exchange|
|Price Bought|Price paid on purchase|
|Spread|Difference between current bid and ask price|
|Signal Rule|Signal rule used to buy the pair|
|Trading Rules|Trading rules currently applied to the pair|
|Order Dates|The dates of the purchase orders|
|Order IDs|The order Ids on the exchange|

Along the bottom of the main area you will find useful information regarding the current pairs, Total Pairs, Average Margin, Average Rating, Average Age, and Total Cost of all pairs together.

#### Market

This is the current market information page. The table shows all the pairs that are currently available on the exchange along with a variety of data. The data automatically refreshes every 5 seconds.

Columns can be adjust. You can reorder them, move them around and even remove the ones that you don't need. There are some options for exporting the data: you can copy or export it to a spreadsheet.

You can click on the table rows to expand them and access additional options. From there you can manually buy, buy default (buys at the max cost) or view the current pair's settings. 

The log button will display the last 5 lines from the log file.

|Column|Description|
|-|-|
|More|Displayed on low resolution screens and when clicked shows additional data|
|Name|Pair's name|
|Rating|Ratings for each signal|
|% Rating Change|Percentage rating change for each signal|
|Price|Current pair's price (same for every signal)|
|% Price Change|Percentage price change for each signal|
|Spread|Difference between current bid and ask price|
|Volume|Volume for each signal|
|% Volume Change|Percentage volume change for each signal|
|Volatility|Volatility for each signal|
|Trading Rules|Currently applied trading rules|
|Signal Rules|Currently applied signal rules|

#### Stats

Here you can see your total overall profit and current account balance.
There is also a breakdown of profits by day, along with other information, like average margin.

You can click on the number of trades to see information about the orders completed on that particular day.

#### Settings

Here you can temporarily enable or disable a small subset of bot options.  

If you would like to make permanent changes to settings, you can use the Advanced panel. Saving the changes there will take immediate and permanent effect. In the advanced editor you can switch between different editing modes (Tree, Code, Form, Text, View).

There are also three options available on that page:
- Restart Services: this will restarts all the bot's services (core, trading, signals, rules, etc.). Useful when something went wrong and you don't have access to your machine / VPS.
- Refresh Account: you could use this option if you have made a manual trade on an exchange and would like to update your account immediately. Hoewever, remember that there is a period account refresh (every 6 minutes by default), so this is rarely necessary. 
- Log Out: log out of IntelliTrader

#### Log

Displays the last 500 lines from the log file.

#### Help

This page

Notifications
-------------

#### Telegram

In the config\notification.json change *Enabled* to true, set *TelegramBotToken* to your bot's token and *TelegramChatId* to your chat id to enable Telegram notifications. Set *TelegramAlertsEnabled* to *false* if you don't want to receive alerts with notifications.  

Talk to @botfather to create a new bot and then talk to @FalconGate_Bot to get your telegram chat id (type /get\_my\_id).

Health Checks
-------------

Health check is a periodic test of all the bot's services to make sure that every single one of them is running smoothly and with no interruptions. Trading gets suspended if at least one of the health checks fails until it passes again. If notifications are enabled, you will get a notification for both occasions.

Troubleshooting
-------------

#### IntelliTrader is not starting or crashing on startup
Please made sure that your system meets all the requirements and that you have all the prerequisites. Also check if there are any errors in the log/*-general.log file.

#### IntelliTrader is not trading or is behaving weirdly in general
Again, check the log file for any errors, it is very likely that there will be a clue there.

License & Disclaimer
-------------

By using, or simply downloading IntelliTrader (the Software), you understand and accept the following:


Licensing

The Software is licensed under the Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International licence. Full terms of the licence can be found by following this link:

https://creativecommons.org/licenses/by-nc-sa/4.0/legalcode

A full copy of the license is also included with the download.


Limitation Of Liability

In no event shall liability be accepted for any indirect, incidental, special, consequential or punitive damages, including, without limitation, loss of profits, funds, data, use, goodwill, or other tangible and intangible losses, resulting from:

	(i) your access to, or use of, or inability to access or use the Software; 
	(ii) any content of any third party used with the Software; 
	(iii) any content obtained from the Software; and 
	(iv) unauthorized access, use or alteration of your transmissions or content, whether based on warranty, contract, tort (including negligence) or any other legal theory, whether or not we have been informed of the possibility of such damage, and even if a remedy set forth herein is found to have failed of its essential purpose.

We do not refund losses.


Disclaimer

Your use of the Software is at your sole risk. The Software is provided on an “AS IS” and “AS AVAILABLE” basis. The Software is provided without warranties of any kind, whether expressed or implied, including, but not limited to, implied warranties of merchantability, fitness for a particular purpose, non-infringement or course of performance.

No warranty of any kind is expressed or implied, that:

	a) the Software will function, be secure or operate on any nominated platform; 
	b) any errors or defects will be corrected; 
	c) the Software is free of viruses or other harmful components; or 
	d) the results of using the Software will meet your requirements.


If you do not agree with any of the above, please do not download or use the Software.