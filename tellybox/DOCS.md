# Tellybox

Tellybox lets young kids pick parent-approved videos on any device and plays them on the TV through a Chromecast, within a daily time allowance. You approve everything; nothing reaches the kids' app without you.

## Installation

1. In Home Assistant, open **Settings > Add-ons > Add-on store**, then the menu (three dots) > **Repositories**, and add `https://github.com/sandermvanvliet/tellybox-ha-addon`.
2. Install **Tellybox** and click **Start**.
3. Open the **Log** tab and look for the line `admin setup code: XXXX-XXXX`.
4. Click **Open Web UI**. It goes to `/admin`; open `/admin/setup`, enter the setup code and choose an admin password. (Or set the `admin_password` option before the first start.)
5. In the admin pages, pick your Chromecast, add videos and approve them.
6. Give the kids `http://<home assistant address>:8080` on their tablet or phone.

## Options

| Option | Default | Meaning |
| --- | --- | --- |
| `web_port` | `8080` | Port for the kid app, admin pages and media files. Change it if another add-on already uses 8080. |
| `cast_api_port` | `8081` | Internal port of the cast service (listens on localhost only). Change it on a clash. |
| `media_base_url` | empty | Optional. The URL the Chromecast uses to fetch videos is detected automatically. Set it (plain `http://` and an IP address, for example `http://192.168.1.10:8080`) if detection picks the wrong network interface. |
| `admin_password` | empty | Optional. If set, it overrides a password chosen in the browser. If empty, you choose one at `/admin/setup`. |
| `oidc_issuer` | empty | Optional. Turns on [sign-in with OIDC](#sign-in-with-oidc): your provider's issuer URL, for example `https://id.example.org`. Then `oidc_client_id`, `oidc_redirect_uri` and `oidc_admin_group` are required too, or the add-on doesn't start. |
| `oidc_client_id` | empty | The client id from the provider. |
| `oidc_client_secret` | empty | The client secret. Leave empty for a public client; PKCE is always used. |
| `oidc_redirect_uri` | empty | Your Tellybox address followed by `/admin/oidc/callback`, exactly as registered at the provider, for example `https://tellybox.example.org/admin/oidc/callback`. |
| `oidc_admin_group` | empty | Only members of this group may use the admin pages with OIDC. |

Restart the add-on after changing options. The time zone follows Home Assistant's.

## Sign in with OIDC

If you sign in to other apps with an identity provider, the admin pages can use it too. The sign-in page then shows **Sign in with OIDC** above the password form. The password stays as the fallback, and only members of `oidc_admin_group` get in.

1. Register Tellybox at the provider as an OpenID Connect client, with `<your Tellybox address>/admin/oidc/callback` as the redirect URL. The address must be one your browser can reach, usually an HTTPS name on your reverse proxy.
2. Make a group for the parents, and make the provider send the user's groups in a `groups` claim.
3. Fill in the five `oidc_*` options and restart the add-on. Choose a password first (see Installation): OIDC sign-in works once a password exists.

If something fails, the Log tab says why. Providers that name the groups claim differently or need another scope are not supported by the add-on options yet. See the Tellybox documentation: https://github.com/sandermvanvliet/tellybox

## Where your files live

- **Videos and images:** `/media/tellybox`, which shows up as `tellybox` in Home Assistant's **Media** browser. Home Assistant backups do not include the media folder unless you choose it.
- **Database, settings and signing key:** the add-on's own data folder (`/data/tellybox`). It is included in Home Assistant backups of this add-on.

## Tellybox receiver

Tellybox can show its own screens on the TV (sky, loading, goodnight) through its own Chromecast receiver. This is optional; without it Tellybox falls back to the default receiver. See the Tellybox documentation: https://github.com/sandermvanvliet/tellybox

## Home Assistant integration

The [Tellybox integration](https://github.com/sandermvanvliet/ha-tellybox) lets Home Assistant see and control Tellybox (allowance left, block, extra time) through the admin API. Create an API token in the Tellybox admin pages and point the integration at `http://<home assistant address>:8080`.

## Troubleshooting

- **No Chromecast found:** the Chromecast and Home Assistant must be on the same network segment (no VLAN or guest-network isolation). The add-on uses host networking for discovery.
- **The TV does not start playing:** the Chromecast must reach the media URL. Check `media_base_url`.
- **Port already in use:** change `web_port` (or `cast_api_port`), restart, and use the new port on the kids' devices. The Web UI button may keep pointing at 8080 until the add-on is reloaded.
- **Lost the setup code:** it is logged on every start until a password is set. Restart the add-on and read the Log tab.
