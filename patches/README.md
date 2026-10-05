# Patches

Applied on top of the selected source by the **Build Paperclip image** workflow (when *patches* is enabled), in filename order.

| Patch | Why | Drop when |
| --- | --- | --- |
| `slack-mcp-user-oauth.patch` | The Slack "Use this connection as an agent tool" connector uses Slack's bot OAuth endpoints, but `mcp.slack.com` requires the user-token flow, so Slack answers "Invalid permissions requested". Switches to `oauth/v2_user/authorize` + `oauth.v2.user.access`. Upstream: paperclipai/paperclip#13935, PRs #13954 / #14037. | The workflow reports the patch as already applied, or it stops applying. |

Slack app setup this patch expects: add the scopes under **User Token Scopes**, add Paperclip's `/api/tools/oauth/callback` URL as a Redirect URL, and turn on **Model Context Protocol** under Features → Agents & AI Apps.
