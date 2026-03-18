# Home Assistant Add-on: Minecraft Paper Server

A stable and production-ready Minecraft Paper Server optimized for Home Assistant. All your plugins, worlds, and properties are properly persisted to the `/data` directory.

## Configuration Defaults

- **accept_eula**: (Required) Must be changed to `true` to agree to the Minecraft EULA.
- **server_version**: The version of Minecraft you want (e.g., `1.20.4`, `1.21.1` or automatically resolved if `latest`).
- **memory_min** / **memory_max**: RAM allocation limits for your host device (e.g., `2G`, `4G`).
- **motd**: The MOTD displayed in the multiplayer server list.
- **rcon_password**: The remote console password required for HA interactions and graceful shutdowns.

## Home Assistant Integration

Because this add-on automatically configures RCON inherently to save the worlds, you can connect Home Assistant directly to your Minecraft Server using the official core HA integrations.

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
    password: "ChangeMePlease"
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
All worlds, plugins, and properties are safely isolated inside `/data`. This makes it fully compatible with Home Assistant's built-in full and partial backup systems.
