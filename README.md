# AntiAbuse
## About
AntiAbuse is going to detect when a user of the given groupId is abusing and will instantly demote that user to the demotedRank.

# Information
## Installation
Simply open your terminal on your code editor and run `npm install antiabuse`.

## Explanation
AntiAbuse scans your group every second and looks for abuse through the roleset feature looking at the audit log.
When the client detects that a user used the roleset features more times than the specified on the maxActions setting, the client will instantly demote that user to the demotedRank setting. 

## Current Features
As of right now, AntiAbuse detects roleset abuse and instantly demote the user to the demotedRank rank and send a warning to Discord.
More features are going to come soon, do not fret, this is just V1!

## Utilization Guide
An AntiAbuse client is required for the group you want to defend.
If you would like to defend more than 1 group, you have to make more AntiAbuse clients.
Example:
```js
const AntiAbuse = require("antiabuse")
const antiAbuseClient = new AntiAbuse({
  robloxCookie: 'Your cookie', 
  discordWebhook: 'Discord Webhook URL',
  groupId: '123456',
  delay: 60,
  demotedRank: 1,
  maxActions: 10,
  duration: 10
})
antiAbuseClient.monit()
```
You can make use of the methods manually, or use the `monit` method to automatically check the group.

All time values should be supplied in seconds.

## Client Creation
The module exports the AntiAbuse class, which is your primary class. One of these is required for each group, but you can have many clients.
Multi-group clients are not currently supported. You are recommended to simply use the `monit` method to watch your group.
```JS
const AntiAbuse = require("antiabuse")
const antiAbuseClient = new AntiAbuse(Your_Configuration)
```

### Configuration Options
- `robloxCookie`: **Required**: Your full Roblox cookie (`.ROBLOSECURITY`). It is recommended that you use a bot account.
- `discordWebhook`: **Required**: Your Discord Webhook, which will executed when an attack is detected.
- `groupId`: **Required**: The ID of the group you want to protect.
- `delay`: The delay for the monit function. It will scan your group every `delay` seconds. Default: 90 seconds.
- `demotedRank`: The rank which users will be demoted to if they exceed the threshold. Default: 1
- `maxActions`: The number of actions which when exceeded, will cause the user to be demoted. Default: 10.
- `duration`: The time which that the maximum actions can be reached within. Default: 10 minutes, so users can perform 10 actions in 10 minutes.

## Client Methods
Once you have a client, you can use its methods which are documented below:
### Monit
`AntiAbuse.monit(delay?)`

Runs a "scan" of the group and checks that no abuse has taken place. This will be run every `delay` seconds. A delay can be passed, or the value given to the config will be used.

This is the recommended way to make use of this package.


### getRecent
`AntiAbuse.getRecent()`

Returns any logs that have taken place since the last time `getRecent` was run. This will return an empty array the first time it is used. 

### updateActions
`AntiAbuse.updateActions(Logs)` (Internal)

This takes the logs array outputted by getRecent and updates the action totals. It is not expected to be used outside of the class itself, as it's very niche.

### checkActions
`AntiAbuse.checkActions()`

Iterates the actions map and checks if any users are above the threshold value. If they are, it runs the `takeAction` method which demotes them.


### takeAction
`AntiAbuse.takeAction(Info)` (Internal)

Takes the information from checkActions(The user's ActionInfo) and deranks them, then sends a webhook.

# Support
DM me on Discord: `bxnny#8147`.

Alternatively, create an issue on this repo.
