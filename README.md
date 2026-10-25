# INUC home server

Domácí server na **Intel NUC** s **Home Assistant OS** pro lokální ovládání zařízení Tuya / Smart Life a lokální MQTT broker.

## Cíl

- Nahradit Windows na NUC systémem Home Assistant OS (`generic-x86-64`)
- Lokální MQTT přes oficiální addon **Mosquitto broker**
- Lokální řízení Tuya / Smart Life přes **Tuya Local** (HACS) — bez runtime závislosti na cloudu

Smart Life aplikace zůstává jen pro **párování** nových zařízení do Wi‑Fi; běžné ovládání jde přes Home Assistant.

## Dokumentace

| Dokument | Obsah |
|----------|--------|
| [docs/nuc-install.md](docs/nuc-install.md) | Záloha Windows, Ubuntu Live USB, zápis HAOS na interní disk, první boot, DHCP rezervace |
| [docs/mqtt-mosquitto.md](docs/mqtt-mosquitto.md) | Mosquitto addon, MQTT uživatel, integrace, publish/subscribe test |
| [docs/tuya-local.md](docs/tuya-local.md) | HACS, Tuya Local, local key, offline WAN test |
| [docs/checklist.md](docs/checklist.md) | Checklist „server je hotový“ |

## Quick start

1. Zálohujte Windows a nastavte BIOS (**UEFI on**, **Secure Boot off**) — viz [instalace NUC](docs/nuc-install.md).
2. Z Ubuntu Live USB zapište HAOS `generic-x86-64` na **interní disk** NUC (HAOS nemá instalátor); po bootu otevřete `http://homeassistant.local:8123`.
3. V routeru nastavte DHCP rezervaci pro NUC.
4. Nainstalujte Mosquitto a ověřte MQTT — viz [MQTT](docs/mqtt-mosquitto.md).
5. Nainstalujte HACS + Tuya Local a přidejte zařízení — viz [Tuya Local](docs/tuya-local.md).
6. Projďěte [checklist](docs/checklist.md).

## Rozhodnutí

- **Instalace:** HAOS přímo na NUC (Windows se smaže)
- **Tuya:** Tuya Local přes HACS
- **MQTT:** oficiální Mosquitto addon

## Bezpečnost

Do gitu **nepatří** hesla, local keys, device ID ani Access ID/Secret. V dokumentaci používejte jen placeholdery (`YOUR_MQTT_PASSWORD`, `YOUR_LOCAL_KEY`, …).

## Mimo tento milník

- Vzdálený přístup (Tailscale / Nabu Casa)
- Migrace historie z jiného HA
- Kompletní katalog všech Tuya zařízení
