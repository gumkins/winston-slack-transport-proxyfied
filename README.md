# winston-slack-transport-proxyfied

A Slack transport for Winston 3+ that logs to a channel via webhooks.

[![NPM](https://nodei.co/npm/winston-slack-transport-proxyfied.png?downloads=true)](https://nodei.co/npm/winston-slack-transport-proxyfied/)

## Installation

```
npm install winston winston-slack-transport-proxyfied
```

## Usage

### Set up with transports

```javascript
const winston = require("winston");
const SlackHook = require("winston-slack-transport-proxyfied");

const logger = winston.createLogger({
	level: "info",
	transports: [
		new SlackHook({
			webhookUrl: "https://hooks.slack.com/services/xxx/xxx/xxx"
		})
	]
});

logger.info("This should now appear on Slack");
```

### Set up by adding

```javascript
const winston = require("winston");
const SlackHook = require("winston-slack-transport-proxyfied");

const logger = winston.createLogger({});

logger.add(SlackHook, {webhookUrl: "https://hooks.slack.com/services/xxx/xxx/xxx"});
```

### Options

* `webhookUrl` - Slack incoming webhook URL. This can be from a basic integration or a bot. **REQUIRED**
* `channel` - Slack channel to post message to.
* `username` - Username to post message with.
* `iconEmoji` - Status icon to post message with. (interchangeable with `iconUrl`)
* `iconUrl` - Status icon to post message with. (interchangeable with `iconEmoji`)
* `formatter` - Custom function to format messages with. This function accepts the `info` object ([see Winston documentation](https://github.com/winstonjs/winston/blob/master/README.md#streams-objectmode-and-info-objects)) and must return an object with at least one of the following three keys: `text` (string), `attachments` (array of [attachment objects](https://api.slack.com/docs/message-attachments)), `blocks` (array of [layout block objects](https://api.slack.com/messaging/composing/layouts)). These will be used to structure the format of the logged Slack message. By default, messages will use the format of `[level]: [message]` with no attachments or layout blocks.
* `level` - Level to log. Global settings will apply if this is blank.
* `unfurlLinks` - Enables or disables [link unfurling.](https://api.slack.com/docs/message-attachments#unfurling) (Default: false)
* `unfurlMedia` - Enables or disables [media unfurling.](https://api.slack.com/docs/message-link-unfurling) (Default: false)
* `mrkdwn` - Enables or disables [`mrkdwn` formatting](https://api.slack.com/messaging/composing/formatting#basics) within attachments or layout blocks (Default: false)
* `proxy` - proxy server URL

### Message formatting

`winston-slack-webhook-transport` supports the ability to format messages using Slack's message layout features. To do this, supply a custom formatter that supplies the [requisite object structure](https://api.slack.com/messaging/composing/layouts) to create the desired layout.

```javascript
const winston = require("winston");
const SlackHook = require("winston-slack-transport-proxyfied");
 
const logger = winston.createLogger({
    level: "info",
    transports: [
        new SlackHook({
			webhookUrl: "https://hooks.slack.com/services/xxx/xxx/xxx",
			formatter: info => {
				return {
					text: `${info.level}: ${info.message}`,
					attachments: [
						{
							text: "Or don't pass anything. That's fine too"
						}
					],
					blocks: [
						{
							type: "section",
							text: {
								type: "plain_text",
								text: "You can pass more info to the formatter by supplying additional parameters in the logger call"
							}
						}
					]
				}
			}
        })
    ]
});

logger.info("Definitely try playing around with this.")
```

### Setup proxy server

```javascript
const winston = require("winston");
const SlackHook = require("winston-slack-transport-proxyfied");

const logger = winston.createLogger({
        level: "info",
        transports: [
                new SlackHook({
                        webhookUrl: "https://hooks.slack.com/services/xxx/xxx/xxx",
                        proxy: "http://192.168.1.5" // TODO: replace with your proxy server URL
                })
        ]
});
```

