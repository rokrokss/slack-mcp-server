### 1. Authentication Setup

Open up your Slack in your browser and login.

> **Note**: You need one of the following: an `xoxp-*` User OAuth token, an `xoxb-*` Bot token, or both `xoxc-*` and `xoxd-*` session tokens. For full functionality including search, you can combine `xoxb` (bot) + `xoxp` (user) tokens. User/Bot tokens are more secure and do not require a browser session. If multiple are provided, priority is `xoxb` > `xoxp` > `xoxc/xoxd`.

#### Option 1: Using `SLACK_MCP_XOXC_TOKEN`/`SLACK_MCP_XOXD_TOKEN` (Browser session)

##### Lookup `SLACK_MCP_XOXC_TOKEN`

- Open your browser's Developer Console.
- In Firefox, under `Tools -> Browser Tools -> Web Developer tools` in the menu bar
- In Chrome, click the "three dots" button to the right of the URL Bar, then select
  `More Tools -> Developer Tools`
- Switch to the console tab.
- Type "allow pasting" and press ENTER.
- Paste the following snippet and press ENTER to execute:
  `JSON.parse(localStorage.localConfig_v2).teams[document.location.pathname.match(/^\/client\/([A-Z0-9]+)/)[1]].token`

Token value is printed right after the executed command (it starts with
`xoxc-`), save it somewhere for now.

##### Lookup `SLACK_MCP_XOXD_TOKEN`

- Switch to "Application" tab and select "Cookies" in the left navigation pane.
- Find the cookie with the name `d`.  That's right, just the letter `d`.
- Double-click the Value of this cookie.
- Press Ctrl+C or Cmd+C to copy it's value to clipboard.
- Save it for later.

#### Option 2: Using `SLACK_MCP_XOXP_TOKEN` (User OAuth)

Instead of using browser-based tokens (`xoxc`/`xoxd`), you can use a User OAuth token:

1. Go to [api.slack.com/apps](https://api.slack.com/apps) and create a new app
2. Under "OAuth & Permissions", add the following scopes:
    - `channels:history` - View messages in public channels
    - `channels:read` - View basic information about public channels
    - `groups:history` - View messages in private channels
    - `groups:read` - View basic information about private channels
    - `im:history` - View messages in direct messages.
    - `im:read` - View basic information about direct messages
    - `im:write` - Start direct messages with people on a user’s behalf (new since `v1.1.18`)
    - `mpim:history` - View messages in group direct messages
    - `mpim:read` - View basic information about group direct messages
    - `mpim:write` - Start group direct messages with people on a user’s behalf (new since `v1.1.18`)
    - `users:read` - View people in a workspace.
    - `chat:write` - Send messages on a user’s behalf. (new since `v1.1.18`)
    - `search:read` - Search a workspace’s content. (new since `v1.1.18`)

3. Install the app to your workspace
4. Copy the "User OAuth Token" (starts with `xoxp-`)

##### App manifest (preconfigured scopes)
To create the app from a manifest with permissions preconfigured, use the following code snippet:

```json
{
    "display_information": {
        "name": "Slack MCP"
    },
    "oauth_config": {
        "scopes": {
            "user": [
                "channels:history",
                "channels:read",
                "groups:history",
                "groups:read",
                "im:history",
                "im:read",
                "im:write",
                "mpim:history",
                "mpim:read",
                "mpim:write",
                "users:read",
                "chat:write",
                "search:read"
            ]
        }
    },
    "settings": {
        "org_deploy_enabled": false,
        "socket_mode_enabled": false,
        "token_rotation_enabled": false
    }
}
```

#### Option 3: Using `SLACK_MCP_XOXB_TOKEN` (Bot Token)

You can use a Bot token, which is the recommended approach for general API calls:

1. Go to [api.slack.com/apps](https://api.slack.com/apps) and create a new app
2. Under "OAuth & Permissions", add Bot Token Scopes:
    - `channels:history` - View messages in public channels
    - `channels:read` - View basic information about public channels
    - `groups:history` - View messages in private channels
    - `groups:read` - View basic information about private channels
    - `im:history` - View messages in direct messages
    - `im:read` - View basic information about direct messages
    - `im:write` - Start direct messages with people
    - `mpim:history` - View messages in group direct messages
    - `mpim:read` - View basic information about group direct messages
    - `mpim:write` - Start group direct messages with people
    - `users:read` - View people in a workspace
    - `chat:write` - Send messages as the bot
3. Install the app to your workspace
4. Copy the "Bot User OAuth Token" (starts with `xoxb-`)
5. **Important**: Bot must be invited to channels for access

> **Note**: Bot tokens cannot use the `search.messages` API. To enable search functionality, see Option 4 below.

#### Option 4: Using Both `SLACK_MCP_XOXB_TOKEN` and `SLACK_MCP_XOXP_TOKEN` (Recommended)

For full functionality including search, use both Bot and User OAuth tokens together:

1. Create an app following steps from Option 3 to get your Bot token (`xoxb-`)
2. In the same app, under "OAuth & Permissions", add User Token Scopes:
    - `search:read` - Search a workspace's content (required for search functionality)
    - Optionally add other user scopes if you want user-level permissions
3. Install/reinstall the app to your workspace
4. Copy both tokens:
    - "Bot User OAuth Token" for `SLACK_MCP_XOXB_TOKEN`
    - "User OAuth Token" for `SLACK_MCP_XOXP_TOKEN`

**How it works:**
- General API calls (messages, channels, users) use the Bot token
- Search API calls automatically use the User OAuth token
- You get the security of bot-scoped permissions for most operations while enabling search

See next: [Installation](02-installation.md)
