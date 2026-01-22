# pbsms - Pushbullet SMS Viewer

> [!NOTE]
> If you find this tool useful, please **star this repository**! It helps others in need find this solution.

A CLI tool to view SMS messages from multiple Pushbullet accounts in your terminal.

## Why use this?

### The Problem
For expats living outside their home country, receiving SMS (Often required for OTPs on banking, government services, etc.) on a home country SIM card while in an international roaming area can be an unjustified expense. Maintaining active roaming plans just to receive a few text messages is costly.

### The Solution
`pbsms` solves this by giving you real-time access to your SMS remotely using **Pushbullet**.

While official Pushbullet clients exist, they can introduce friction when managing multiple accounts or handling End-to-End (E2E) encryption across devices. This app leverages the existing Pushbullet infrastructure to deliver your SMS messages to your terminal in real-time, handling decryption seamlessly.

### Is this for everyone?
**No.** If you have a single device and simply want to manually read a text message now and then, the official Pushbullet app or browser extension is likely enough for you.

**This tool is for you if:**
-   You need a **middleware** to build automated workflows.
-   You want to programmatically **fetch OTPs** to trigger async operations.
-   You want to integrate SMS data with **LLMs** (Large Language Models) for analysis.
-   You are building automated systems like **algorithmic trading bots** that need to react to SMS alerts or confirmations.

### Why Terminal / CLI?
You might wonder why this isn't a browser-based web application with a pretty UI. The choice is intentional:

1.  **Data Sovereignty & Security**: A web-based application would typically require cloud services to host the frontend and potentially proxy data. This would require **me** (the developer) to handle your sensitive SMS data, introducing significant data governance and security risks.
2.  **Privacy**: By keeping this a strictly local CLI application, your data never touches my servers. It goes directly from Pushbullet API -> Your Terminal.
3.  **Governance**: I want to strictly avoid being a custodian of user data.

**Want a UI?**
If a User Interface is important to you, I invite you to **contribute financially** to the project! We can work together to devise a solution that maintains these privacy standards (e.g., a local-only electron app or PWA).

## Subscription & Pricing
Developing, maintaining, and distributing this tool requires significant effort and infrastructure.
- A **nominal subscription fee** will be introduced after the initial trial period.
- **Contributors** to the project will receive **priority** for feature requests and support.

## Roadmap
- **Password Managers**: Currently supports Bitwarden. Support for additional password managers will be added based on user requests.

## License & Access Request

Due to the powerful nature of this tool and the potential for misuse, access is **restricted** and requires a valid license key.

### How to get a License Key
1.  **Fill out the Request Form**: Submit your details via [this Google Form](#). You must provide accurate and verifiable information.
2.  **Validation**: Your request will be manually validated.
3.  **License Key**: Upon approval, you will receive a unique license key to activate the software.

### Compliance & Data Policy
> [!WARNING]
> By using this software, you agree to the following terms regarding identity and usage monitoring.

To ensure this tool is not used for malicious purposes (e.g., intercepting SMS for fraud):
- **Identity Logging**: Your Pushbullet ID and Device ID will be recorded and stored securely.
- **Authority reporting**: This data is maintained to assist **local authorities** and **Pushbullet** in the event of any reported misuse or illegal activity involving this software.

## Detailed Setup Guide

Before installing the CLI, you need to set up the supporting services.

### 1. Pushbullet Setup
Pushbullet is the service that mirrors your SMS from your Android phone to the cloud.

1.  **Create an Account**: Go to [pushbullet.com](https://www.pushbullet.com/) and sign up (Google or Facebook login).
2.  **Install Android App**: Download the Pushbullet app from the Google Play Store on the phone that has the SIM card you want to monitor.
3.  **Enable SMS Sync**:
    -   Open the Android app and sign in with the *same account*.
    -   Go to the "SMS" tab/settings.
    -   Enable **SMS Synchronization**. You will need to grant the app permissions to "Send and view SMS messages".
4.  **Get Access Token**:
    -   Go back to [pushbullet.com](https://www.pushbullet.com/) on your computer.
    -   Go to **Settings** > **Account**.
    -   Scroll down to **Access Tokens** and click **Create Access Token**.
    -   **Save this token** securely. You will need it to configure `pbsms`.
5.  **Enable End-to-End Encryption (REQUIRED)**:
    > [!IMPORTANT]
    > **Encryption is MANDATORY**. This app will NOT function if encryption is disabled.
    -   In the mobile app: Settings > End-to-End Encryption. Set a master password.
    -   In the web settings: Enable encryption and enter the *same* password.

### 2. Bitwarden Setup (REQUIRED)
You **must** use Bitwarden to store your Pushbullet encryption password. This is a non-negotiable security requirement.

1.  **Install Bitwarden CLI**:
    -   MacOS: `brew install bitwarden-cli`
    -   npm: `npm install -g @bitwarden/cli`
    -   Windows/Linux: See [official docs](https://bitwarden.com/help/cli/).
2.  **Get API Key (for CLI login)**:
    -   Log in to the [Bitwarden Web Vault](https://vault.bitwarden.com/).
    -   Go to **Settings** > **Security** > **Keys**.
    -   Click **View API Key**. You will see `client_id` and `client_secret`.
3.  **Login via CLI**:
    ```bash
    bw login
    # It will ask for your client_id and client_secret
    ```
4.  **Store your Pushbullet Password**:
    -   Create a new **Login** item in Bitwarden (e.g., named "Pushbullet Encryption").
    -   **Critical**: Put your Pushbullet Encryption Password (from step 1.5) in the **Password** field of this item.
    -   The CLI will search for this item by name to retrieve the password securely.

## Installation

Install the tool globally using npm:

```bash
npm install -g pbsms
```

## Usage

Once installed, you can use the `pbsms` command.

### 1. Add an Account
Link your Pushbullet account and map the encryption password from Bitwarden.

```bash
pbsms add [name] [pushbullet_token] [bitwarden_item_name]
```

- `name`: A nickname for this phone (e.g., "work-sim").
- `pushbullet_token`: The token you created in step 1.4.
- `bitwarden_item_name`: The name of the Bitwarden Login item containing your password.

Example:
```bash
pbsms add work-sim o.AbCdEfGhI... "Pushbullet Encryption"
```

### 2. List Accounts
See all configured accounts.

```bash
pbsms list
```

### 3. Watch for SMS
Start watching for new messages in real-time.

```bash
pbsms watch
```

## Privacy & Security
- **Encryption**: Supports Pushbullet's End-to-End encryption.
- **Local Storage**: Your tokens are stored locally in `~/.pushbullet-sms.json`.
- **Closed Source**: The code is minified for distribution, the source is closed due to security reasons.
- **No Logging**: The code does not log any sensitive information (beyond the registration data mentioned above).
- **No Tracking**: The code does not track usage behavior.
- **Secure Encryption Key**: The code uses a secure encryption key in Bitwarden to decrypt your Pushbullet messages.