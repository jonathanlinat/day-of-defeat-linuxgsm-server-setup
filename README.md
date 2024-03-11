# Day of Defeat: Set up a server with LinuxGSM

| 💬 Join an Active Community |
| --------------------------- |
| [![](https://dcbadge.vercel.app/api/server/dodcommunity?style=plastic)](https://discord.gg/dodcommunity) |

This guide will walk you through setting up a Day of Defeat server using Linux Game Server Managers (LinuxGSM), providing detailed instructions to ensure a smooth setup process.

## Features

- **Day of Defeat**: [Game Store Page](https://store.steampowered.com/app/30/Day_of_Defeat/)
- **LinuxGSM**: [Official Website](https://linuxgsm.com/)
- **ReHLDS**: [GitHub Repository](https://github.com/dreamstalker/rehlds)
- **Metamod-r**: [Releases Page](https://github.com/theAsmodai/metamod-r/releases)

## Pre-requisites

Ensure you have access to a Virtual Private Server (VPS) or a dedicated home server setup. A VPS is essentially a remote computer that can host your game server.

Learn more about VPS: [Google Cloud's Explanation](https://cloud.google.com/learn/what-is-a-virtual-private-server). For VPS options, consider providers like [Namecheap](https://www.namecheap.com/hosting/vps/).

## Step-by-Step Instructions

### 1. Connect to Your VPS

Establish a secure connection to your VPS using SSH. Replace `<ip-address>` with your server's IP address:

```bash
ssh root@<ip-address> -p 22
```

### 2. Update Operating System Dependencies

Ensure all system packages are up-to-date with:

```bash
sudo apt update && sudo apt dist-upgrade --auto-remove --purge -y
```

### 3. Install LinuxGSM Dependencies

Install necessary dependencies for LinuxGSM:

```bash
sudo dpkg --add-architecture i386 && sudo apt update && sudo apt install -y curl wget file tar bzip2 gzip unzip bsdmainutils python3 util-linux ca-certificates binutils bc jq tmux netcat lib32gcc-s1 lib32stdc++6 libsdl2-2.0-0:i386 steamcmd
```

### 4. Create a New Unix User

Create a non-root user to enhance security:

```bash
adduser dodserver && su - dodserver
```

### 5. Download LinuxGSM

Download and prepare the LinuxGSM script:

```bash
curl -Lo linuxgsm.sh https://linuxgsm.sh && chmod +x linuxgsm.sh && bash linuxgsm.sh dodserver
```

### 6. Install Server Files

Initiate the server installation:

```bash
./dodserver install
```

## Optional Steps for Legacy Version Setup

If you want to use `ReHLDS` and `Metamod-r`, you must download the legacy version of Day of Defeat (before the 25th update).

### Reset the Server Files

Clear existing server files:

```bash
rm -rf serverfiles/
```

### Download the Legacy Version with SteamCMD

1. **Start SteamCMD:**

    ```bash
    steamcmd
    ```

2. **Set Installation Directory:**

    ```bash
    force_install_dir /home/dodserver/serverfiles/
    ```

3. **Login Anonymously:**

    ```bash
    login anonymous
    ```

4. **Specify Mod and Download:**

    ```bash
    app_set_config 90 mod dod
    app_update 90 -beta steam_legacy validate
    ```

5. **Exit SteamCMD:**

    ```bash
    quit
    ```

## Optional Enhancements

### Installing ReHLDS

Enhance security and performance with ReHLDS:

1. **Download and Unzip:**

    ```bash
    mkdir -p tmp && wget -O tmp/rehlds-bin-3.13.0.788.zip https://github.com/dreamstalker/rehlds/releases/download/3.13.0.788/rehlds-bin-3.13.0.788.zip && unzip tmp/rehlds-bin-3.13.0.788.zip -d tmp/rehlds/
    ```

2. **Install:**

    ```bash
    cp -rf tmp/rehlds/bin/linux32/* serverfiles/ && rm -rf tmp/
    ```

### Installing Metamod-r

Add functionality with Metamod-r:

1. **Download and Install:**

    ```bash
    mkdir -p tmp && wget -O tmp/metamod-bin-1.3.0.138.zip https://github.com/theAsmodai/metamod-r/releases/download/1.3.0.138/metamod-bin-1.3.0.138.zip && unzip tmp/metamod-bin-1.3.0.138.zip -d tmp/metamod/ && mkdir -p serverfiles/dod/addons/ && cp -rf tmp/metamod/addons/* serverfiles/dod/addons/metamod/ && rm -rf tmp/
    ```

2. **Configure `liblist.gam`:**



    ```bash
    nano serverfiles/dod/liblist.gam
    ```

    Update `gamedll_linux`:

    ```
    gamedll_linux "addons/metamod/metamod_i386.so"
    ```

## Configuring LinuxGSM

### Adjust LinuxGSM Settings

Optimize your server settings:

1. **Copy Configuration Template:**

    ```bash
    cp -rf lgsm/config-lgsm/dodserver/_default.cfg lgsm/config-lgsm/dodserver/dodserver.cfg
    ```

2. **Edit Configuration:**

    ```bash
    nano lgsm/config-lgsm/dodserver/dodserver.cfg
    ```

    Include `-addons` in `startparameters` for custom content and set `maxplayers` to `32`.

### Pre-launch Check

Ensure configurations are effective:

```bash
./dodserver start && ./dodserver stop
```

### Final Server Setup

#### Customize Server Configurations

Edit your Day of Defeat server settings for a personalized setup:

```bash
nano serverfiles/dod/dodserver.cfg
```

For example, assign a custom `hostname`.

#### Launch the Server

Start your server:

```bash
./dodserver start
```

Check the operational status of your server:

```bash
./dodserver monitor
```

Congratulations on successfully setting up your Day of Defeat server. it is now ready to welcome players.
