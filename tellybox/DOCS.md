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

Restart the add-on after changing options. The time zone follows Home Assistant's.

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
