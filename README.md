# manasquan-fixer

ESP32 AI agent with persistent memory and offline rule engine. chat via Telegram, serial console, or NATS to configure behavior; persistent JSON state survives reboot; local rules execute without cloud roundtrips.

## architecture

- **LLM client** — Google Gemini or any OpenAI-compatible API (configurable at runtime)
- **history & memory** — LittleFS JSON state, survives power cycle, stores last N turns for context
- **offline rules** — declarative rule engine for deterministic branching without LLM
- **device registry** — pluggable GPIO/sensor/actuator abstraction
- **interfaces** — Telegram bot, serial console (115200 baud), NATS subscriber/publisher
- **watchdog & heartbeat** — FreeRTOS task WDT, optional LED status indicator

## quick start

1. configure Wi-Fi SSID/password and LLM API credentials via web portal (captive or direct HTTP)
2. connect serial monitor (115200), type a message
3. agent sends to LLM with system prompt + history, echoes response
4. history persists; LED heartbeat confirms operation

chat commands (examples):
- "turn on the lights" → evaluates rules, calls device methods
- "set timezone to UTC" → persists to config.json
- "what's the temperature?" → queries device registry
- "reset memory" → clears history but keeps device state

## configuration

defaults in `src/main.cpp`:
- model: `google/gemini-2.5-flash`
- Wi-Fi: empty (user configures via portal)
- API base URL: configurable per provider
- NATS: optional, disabled by default
- Telegram: optional, disabled by default
- timezone: defaults UTC, user-configurable

web portal listens on 192.168.4.1 if Wi-Fi fails to connect (captive).

## dependencies

- ESP-IDF 5.x or 6.x
- LittleFS for NVS
- FreeRTOS task watchdog
- optional: NATS ESP32 library, Telegram API

## interfaces

see `/docs` for detailed configuration, device registry, rule syntax, and NATS/Telegram setup.
