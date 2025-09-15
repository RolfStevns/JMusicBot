# JMusicBot

### Status - Original Repo
[![Downloads](https://img.shields.io/github/downloads/jagrosh/MusicBot/total.svg)](https://github.com/jagrosh/MusicBot/releases/latest)
[![Stars](https://img.shields.io/github/stars/jagrosh/MusicBot.svg)](https://github.com/jagrosh/MusicBot/stargazers)
[![Release](https://img.shields.io/github/release/jagrosh/MusicBot.svg)](https://github.com/jagrosh/MusicBot/releases/latest)
[![License](https://img.shields.io/github/license/jagrosh/MusicBot.svg)](https://github.com/jagrosh/MusicBot/blob/master/LICENSE)
[![Discord](https://discordapp.com/api/guilds/147698382092238848/widget.png)](https://discord.gg/0p9LSGoRLu6Pet0k)<br>
[![CircleCI](https://dl.circleci.com/status-badge/img/gh/jagrosh/MusicBot/tree/master.svg?style=svg)](https://dl.circleci.com/status-badge/redirect/gh/jagrosh/MusicBot/tree/master)
[![Build and Test](https://github.com/jagrosh/MusicBot/actions/workflows/build-and-test.yml/badge.svg)](https://github.com/jagrosh/MusicBot/actions/workflows/build-and-test.yml)
[![CodeFactor](https://www.codefactor.io/repository/github/jagrosh/musicbot/badge)](https://www.codefactor.io/repository/github/jagrosh/musicbot)

---

### Notes
> **<p style="color:yellow;background-color:dark-grey">This is just a fork of the original MusicBot from Jagrosh for fixing some errors that has not been addressed yet - for v4.3.0</p>**
> **<p style="color:yellow;background-color:dark-grey">This is just a fork of that for to enable it in docker.</p>**
---
### Patches
#### Updated Lavaplayer to v2.2.4
```
<version>2.2.4</version> >> <version>2.2.1</version>
```
- **Source** : https://github.com/lavalink-devs/lavaplayer/releases
#### Updated Lavalink Youtube Source to v1.13.3
```
<version>1.5.2</version> >> <version>1.13.3</version>
```
- **Source** : https://github.com/lavalink-devs/youtube-source/releases
---
$\longrightarrow$ ***Reason*** : [[SignatureCipherManager Error](https://github.com/jagrosh/MusicBot/issues1694)]

---
#### Changed JDA Utilities repo for Local Compilation from Source
```
<dependency>
    <groupId>com.jagrosh</groupId>
    <artifactId>jda-utilities</artifactId>
    <version>3.0.5</version>
    <type>pom</type>
</dependency>
```
- **To**
```
<dependency>
    <groupId>com.github.Nagami-Yukki</groupId>
    <artifactId>JDA-Utilities</artifactId>
    <version>1.0</version>
</dependency>
```
---
$\longrightarrow$ **Reason** : ***Missing Dependency*** : [[Issue-1686](https://github.com/jagrosh/MusicBot/issues/1686)]

---

### Compiling
- Generally you will need to have maven installed as this is a maven project
- **[ And Java as well of course ]**
---
> <!> **An IDE should be able to automatically do this**<br>
> <!> But if you intend on compiling from CLI, this is what I did
---
- To compile via CLI
```
mvn clean install
```
- This will put the compiled project in the target directory
- Choose the jar file : **`JMusicBot-Snapshot-All.jar`** **[ in the target directory ]**
<br> $\longrightarrow$ Use this to run the bot
---
- More info on Maven Compilation : [[StackOverflow](https://stackoverflow.com/questions/38315279/how-to-compile-maven-project-from-command-line-with-all-dependencies)]
---
### Docker Compose
- Change the ./config/config.CHANGE.txt to config.txt and add token for discord and OwnerId
**docker-compose.yml**
```yaml
services:
  jmusicbot:
    # Option A: pull from GHCR (recommended for users of this repo)
    image: ghcr.io/rolfstevns/jmusicbot:4.3.0.5
    # Option B: build locally from Dockerfile in this repo
    # build: .
    container_name: jmusicbot
    restart: unless-stopped
    environment:
      # JVM & bot flags
      JAVA_OPTS: -Xms256m -Xmx512m
      BOT_OPTS: -Dnogui=true
      # Optional entrypoint behavior (default copies every start)
      # MOVE_ONCE: "true"
      # CONFIG_PATH_SRC: /data/config.txt
      # CONFIG_PATH_DEST: /app/config.txt
      # JAR_PATH: /app/JMusicBot.jar
    volumes:
      # Host ./config -> container /data (source for config.txt)
      - ./config:/data
    # No ports required; the bot connects outbound to Discord
```
## Troubleshooting

### Bot doesn’t connect to Discord
- Check `./config/config.txt` exists and has a valid `token` and `owner`
- Ensure **Discord intents** are enabled in the Developer Portal:
    - **Message Content Intent**
    - **Server Members Intent**
- Regenerate the bot token if unsure and update `config.txt`
- Confirm the container can reach the internet (no outbound firewall blocks)
- Run docker compose with sudo
- Tail logs with:
  ```bash
  docker compose logs -f jmusicbot
  ```