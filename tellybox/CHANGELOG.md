# Changelog

## Unreleased

- Sign in to the admin with an OpenID Connect provider (Pocket ID, Authentik, Keycloak, Authelia, ...): new options `oidc_issuer`, `oidc_client_id`, `oidc_client_secret`, `oidc_redirect_uri` and `oidc_admin_group`. Needs the Tellybox release that adds OIDC sign-in (AD-6). The password stays as the fallback.

## 0.1.1

- Fix install error saying `media_base_url` is required: the option is optional, so it no longer has an empty default.

## 0.1.0

- First release.
