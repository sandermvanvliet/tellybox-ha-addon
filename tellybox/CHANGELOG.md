# Changelog

## 0.4.0

- Based on Tellybox 0.4.0: the admin state now includes each profile's picture, whether it may watch in the app and its kid app style, which the Home Assistant integration (ha-tellybox 0.4.0) uses for the new Picture entity and per-kid sensors.

## 0.3.0

- Based on Tellybox 0.3.0: watching in the app instead of casting (a TV/device toggle and in-app player per profile), channel subscriptions with an approval inbox, and per-profile control over which shows can be seen.

## 0.2.0

- Sign in to the admin with an OpenID Connect provider (Pocket ID, Authentik, Keycloak, Authelia, ...): new options `oidc_issuer`, `oidc_client_id`, `oidc_client_secret`, `oidc_redirect_uri` and `oidc_admin_group`. Needs Tellybox 0.2.0, which adds OIDC sign-in (AD-6). The password stays as the fallback.
- Based on Tellybox 0.2.0: reader UI and per-profile TV, and per-profile limits (inherit, custom or unlimited).

## 0.1.1

- Fix install error saying `media_base_url` is required: the option is optional, so it no longer has an empty default.

## 0.1.0

- First release.
