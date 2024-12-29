Here is the text with all Chinese parts rewritten in English:

---

# CHAT2API

🤖 A simple ChatGPT TO API proxy

🌟 Use GPT-3.5 for free and unlimited without any account

💥 Supports AccessToken usage with account, supports `O1-Preview/mini`, `GPT-4`, `GPT-4o/mini`, `GPTs`

🔍 The reply format is fully consistent with the real API, compatible with almost all clients

👮 Includes a user management backend [Chat-Share](https://github.com/h88782481/Chat-Share). Before using, make sure the environment variables (ENABLE_GATEWAY set to True, AUTO_SEED set to False) are configured.

## Community Group

[https://t.me/chat2api](https://t.me/chat2api)

Please read the repository documentation, especially the FAQ section, before asking questions.

When asking questions, please provide:

1. Screenshot of the startup logs (with sensitive information masked, including environment variables and version numbers)
2. Log information of the error (with sensitive information masked)
3. The status code and response body returned by the API

## Sponsors

Thanks to Capsolver for sponsoring this project. You can use [Capsolver](https://www.capsolver.com/zh?utm_source=github&utm_medium=repo&utm_campaign=scraping&utm_term=chat2api) to solve any CAPTCHA challenges on the market.

[![Capsolver](docs/capsolver.png)](https://www.capsolver.com/zh?utm_source=github&utm_medium=repo&utm_campaign=scraping&utm_term=chat2api)

## Features

### Latest Version is stored in `version.txt`

### Reverse API Features
> - [x] Stream and non-streaming support
> - [x] No-login GPT-3.5 chat
> - [x] GPT-3.5 model chat (If no `gpt-4` in the model name, the default model is `gpt-3.5`, i.e., `text-davinci-002-render-sha`)
> - [x] GPT-4 series models chat (For models containing `gpt-4`, `gpt-4o`, `gpt-4o-mini`, `gpt-4-mobile`, use the corresponding model with AccessToken)
> - [x] O1 series models chat (For models containing `o1-preview`, `o1-mini`, use the corresponding model with AccessToken)
> - [x] GPT-4 model drawing, coding, and internet access
> - [x] Supports GPTs (For models like `gpt-4-gizmo-g-*`)
> - [x] Supports Team Plus account (requires passing the `ChatGPT-Account-ID`)
> - [x] Upload images and files (formats according to the corresponding API, supports URL and base64)
> - [x] Can be used as a gateway, supports multi-machine distributed deployment
> - [x] Multi-account polling, supports `AccessToken` and `RefreshToken`
> - [x] Retry failed requests and automatically poll the next token
> - [x] Token management, supports upload and clearing of tokens
> - [x] Scheduled use of `RefreshToken` to refresh `AccessToken` / Every startup will refresh all tokens once non-forced, and force refresh every 4 days at 3 AM
> - [x] Supports file download, requires enabling history
> - [x] Supports `O1-Preview/mini` model inference process output

### Official Mirror Features
> - [x] Supports official native mirror
> - [x] Random account selection from the backend
> - [x] Input `RefreshToken` or `AccessToken` for login
> - [x] Supports O1-Preview/mini, GPT-4, GPT-4o/mini
> - [x] Sensitive information and certain settings interfaces are disabled
> - [x] /login login page, automatically redirects after logging out
> - [x] /?token=xxx directly login with `RefreshToken` or `AccessToken` or `SeedToken` (random seed)

> TODO
> - [ ] Mirror support for `GPTs`
> - [ ] None, feel free to raise an issue

## Reverse API

Fully `OpenAI` formatted API, supports passing `AccessToken` or `RefreshToken`, can use GPT-4, GPT-4o, GPTs, O1-Preview, O1-Mini:

```bash
curl --location 'http://127.0.0.1:5005/v1/chat/completions' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Token}}' \
--data '{
     "model": "gpt-3.5-turbo",
     "messages": [{"role": "user", "content": "Say this is a test!"}],
     "stream": true
   }'
```

Pass your account's `AccessToken` or `RefreshToken` as `{{ Token }}`. You can also set the environment variable `Authorization` to randomly select a backend account.

If you have a team account, you can pass `ChatGPT-Account-ID` to use the Team workspace:

- Method 1: Pass `ChatGPT-Account-ID` value in `headers`
- Method 2: Pass `Authorization: Bearer <AccessToken or RefreshToken>,<ChatGPT-Account-ID>`

If the `AUTHORIZATION` environment variable is set, you can pass its value as `{{ Token }}` for multi-token polling.

> - `AccessToken` can be obtained after logging into ChatGPT and opening [https://chatgpt.com/api/auth/session](https://chatgpt.com/api/auth/session).
> - `RefreshToken` method is not provided here.

## Token Management

1. Set the environment variable `AUTHORIZATION` as your authorization code, then run the program.
2. Visit `/tokens` or `/{api_prefix}/tokens` to view the available tokens, upload new ones, or clear them.
3. Pass the `AUTHORIZATION` code as the `APIKEY` to use polling tokens for conversation.

![tokens.png](docs/tokens.png)

## Official Native Mirror

1. Set the environment variable `ENABLE_GATEWAY` to `true` and run the program. After enabling, others can access your gateway through the domain.
2. Upload `RefreshToken` or `AccessToken` in the Token Management page.
3. Visit `/login` to access the login page.
4. Enter the official mirror page to use.

![login.png](docs/login.png)

![chatgpt.png](docs/chatgpt.png)

## Environment Variables

Each environment variable has a default value. If you're unsure about the meaning of a variable, do not set it, and avoid leaving values empty. Strings do not require quotation marks.

| Category | Variable Name        | Example Value                                           | Default Value       | Description                                                  |
|----------|----------------------|---------------------------------------------------------|---------------------|--------------------------------------------------------------|
| Security | API_PREFIX           | `your_prefix`                                           | `None`              | API prefix password. If set, requests must include this prefix in `/your_prefix/v1/chat/completions` |
|          | AUTHORIZATION        | `your_first_authorization`,<br/>`your_second_authorization` | `[]`                | The authorization code you set for token polling, separated by commas. |
|          | AUTH_KEY             | `your_auth_key`                                         | `None`              | Set if your private gateway requires an `auth_key` header for requests. |
| Request   | CHATGPT_BASE_URL     | `https://chatgpt.com`                                   | `https://chatgpt.com` | ChatGPT gateway URL, changing this will affect the requested website. |
|          | PROXY_URL            | `http://ip:port`,<br/>`http://username:password@ip:port`  | `[]`                | Global proxy URL, enabled when 403 occurs, multiple proxies separated by commas. |
|          | EXPORT_PROXY_URL     | `http://ip:port` or<br/>`http://username:password@ip:port` | `None`              | Used to prevent leakage of source IP during image and file requests. |
| Feature   | HISTORY_DISABLED     | `true`                                                  | `true`              | Whether to disable chat history and return `conversation_id`. |
|          | POW_DIFFICULTY       | `00003a`                                                | `00003a`            | Proof-of-work difficulty level, do not set if unsure. |
|          | RETRY_TIMES          | `3`                                                     | `3`                 | Number of retry attempts if an error occurs. |
|          | CONVERSATION_ONLY    | `false`                                                 | `false`             | Whether to directly use the conversation interface, only enabled if your gateway supports automatic POW resolution. |
|          | ENABLE_LIMIT         | `true`                                                  | `true`              | When enabled, the program will avoid breaking official request limits to prevent account bans. |
|          | UPLOAD_BY_URL        | `false`                                                 | `false`             | When enabled, automatically uploads content from URL+text input, multiple URLs separated by spaces. |
|          | SCHEDULED_REFRESH    | `false`                                                 | `false`             | Whether to schedule `AccessToken` refresh, forcing a refresh every 4 days at 3 AM. |
|          | RANDOM_TOKEN         | `true`                                                  | `true`              | Whether to randomly select a backend token for polling or use sequential polling. |
| Gateway   | ENABLE_GATEWAY       | `false`                                                 | `false`             | Whether to enable gateway mode, which allows using

 `/v1/chat/completions` directly. |
|          | ALLOW_RESET          | `true`                                                  | `true`              | Whether to allow resetting token values when API key fails. |

## Running the Program

1. Clone the repository
2. Install dependencies:
    ```bash
    pip install -r requirements.txt
    ```
3. Start the program:
    ```bash
    python chat2api.py
    ```

## Docker

1. Clone the repository
2. Build the Docker image:
    ```bash
    docker build -t chat2api .
    ```
3. Run the Docker container:
    ```bash
    docker run -d -p 5005:5005 chat2api
    ```

## Feedback and Contributions

Feel free to open an issue or create a pull request. Please read the guidelines for contributing.

---

Let me know if you need more modifications!
