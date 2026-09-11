# Codechecker OAuth developer documentation

  * Important: To maintain consistency between GitHub and other providers, we need to fetch primary email
  from another endpoint because GitHub dosn't provide the primary email in the `user_info`,so
  we make an API request to fetch the primary email of the GitHub and use it instead of the username provided by the `user_info`.

  * Github doesn't support PKCE and If GitHub starts supporting PKCE in the future, the code should automatically
  start using it ,and in that case, this note can be removed.

  * If a new OAuth provider is added, add it to `OAUTH_TEMPLATES`, instead of the `server-config.json`.

  * Important: for different providers there are different requirements for providing refresh token.

    In case of of google you need to specify these 2 attributes `access_type='offline'` and `prompt='consent'` prompts `google` to return `refresh_token`.

    ```
    access_type='offline',
    prompt='consent'
    ```

    This is not required for Github and Microsoft and causes Microsoft to request unnecessary admin priveleges.
    The same effect can be reproduced for Microsoft , by adding `offline_access` in scope.
    Whereas GitHub return refresh token by default.

```.py
  if template == "google/v1":
              url, state = session.create_authorization_url(
                  url=authorization_url,
                  state=stored_state,
                  code_verifier=pkce_verifier,
                  access_type='offline',
                  prompt='consent'
              )
          else:
              url, state = session.create_authorization_url(
                  authorization_url,
                  state=stored_state,
                  code_verifier=pkce_verifier
              )
```