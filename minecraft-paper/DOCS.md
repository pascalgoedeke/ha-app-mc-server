# Home Assistant Add-on: Minecraft Paper Server

A stable and production-ready Minecraft Paper Server optimized for Home Assistant. All your plugins, worlds, and properties are properly persisted to the `/data` directory.

## Configuration Defaults

- **accept_eula**: (Required) Must be changed to `true` to agree to the Minecraft EULA.
- **server_version**: The Minecraft version to run (e.g., `1.20.4`, `1.21.1`) or `latest`.
- **auto_update**: If `true`, the add-on checks and installs the newest Paper build for the selected version on startup.
- **memory_min** / **memory_max**: RAM allocation limits for your host device (e.g., `2G`, `4G`).
- **motd**: The MOTD displayed in the multiplayer server list.
- **online_mode**: If `true` (recommended), only authenticated Mojang/Microsoft accounts can join.
- **enforce_whitelist**: If `true`, only players on the whitelist can join.
- **max_players**: Maximum number of concurrent players.
- **expose_rcon**: If `false` (default), RCON is bound to `127.0.0.1` and not reachable externally.
- **rcon_password**: The RCON password used for graceful shutdown and optional remote administration.

## Security & Production Notes

- The Java server runs as a non-root user by default.
- Keep `online_mode: true` for public servers.
- If `rcon_password` is left at the default `__AUTO_GENERATE__`, the add-on generates a random internal password on startup.
- Use a long, unique `rcon_password` (at least 12 chars) whenever you need external RCON access.
- If `expose_rcon` is enabled without an explicit strong password, startup is blocked.
- External RCON is disabled by default. To expose it, set `expose_rcon: true` and map port `25575`.

## Home Assistant Integration

This add-on configures RCON for internal lifecycle management (save-all + graceful stop). You can additionally expose it for Home Assistant automations if required.

### 1. The Native Minecraft Server Integration

1. In Home Assistant, go to **Settings > Devices & Services > Add Integration**.
2. Search for **Minecraft Server**.
3. Set the **Host** to your Home Assistant IP or `localhost`.
4. The integration will automatically track player counts, online status, and network latency!

### 2. Controlling the Server via Automations (Native RCON)

You can send chat messages, change the weather, or execute system console commands directly from your Automations via Home Assistant's built-in RCON integration.

First, set up the YAML integration in your base `configuration.yaml` in HA:

```yaml
rcon:
  - host: localhost
    port: 25575
    password: "your-unique-rcon-password"
```

Restart Home Assistant. You can now call a service from any Automation in your house!

```yaml
alias: Minecraft - Announce Arrival
trigger:
  - platform: state
    entity_id: person.pascal
    to: "home"
action:
  - service: rcon.send
    data:
      command: "say Pascal has arrived home! Welcome him warmly!"
```

## Data Persistence & Backup

> **Upgrade note:** This add-on uses the `homeassistant_config` mapping (instead of the deprecated `config` mapping) to stay compatible with current Home Assistant add-on requirements.

All worlds, plugins, and properties are safely isolated inside `/data`. This makes it fully compatible with Home Assistant's built-in full and partial backup systems.
