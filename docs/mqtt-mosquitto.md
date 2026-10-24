# MQTT — Mosquitto broker

Oficiální addon **Mosquitto broker** jako lokální MQTT broker v Home Assistant OS. Anonymní přístup **nepoužívejte**.

## 1. Instalace addonu

1. V HA: **Settings → Add-ons → Add-on Store**.
2. Najděte **Mosquitto broker**.
3. **Install** → po dokončení **Start**.
4. Zapněte volitelně *Watchdog* a *Start on boot*.
5. V logu addonu ověřte, že broker běží bez chyb.

Hostname addonu v HA síti je typicky:

```text
core-mosquitto
```

Port (výchozí): `1883` (MQTT), `8883` (MQTT over TLS — pokud konfigurujete).

## 2. MQTT uživatel (bez anonymous)

1. V konfiguraci addonu Mosquitto nastavte, že **anonymous login je vypnutý**.
2. Vytvořte uživatele pro MQTT (v UI addonu / HA uživatelé dle aktuální dokumentace addonu), např.:

| Pole | Placeholder |
|------|-------------|
| Username | `YOUR_MQTT_USERNAME` |
| Password | `YOUR_MQTT_PASSWORD` |

3. Uložte a **restartujte** addon Mosquitto.
4. Heslo a jméno **neukládejte do gitu** — jen do správce hesel / lokálních poznámek mimo repo.

## 3. Integrace MQTT v Home Assistant

1. **Settings → Devices & services → Add integration → MQTT**.
2. Napojení na broker:

| Pole | Hodnota |
|------|---------|
| Broker | `core-mosquitto` |
| Port | `1883` |
| Username | `YOUR_MQTT_USERNAME` |
| Password | `YOUR_MQTT_PASSWORD` |

3. Dokončete průvodce. Integrace by měla být ve stavu *Connected*.

Pokud HA nabízí automatickou detekci Mosquitto addonu, potvrďte ji a doplňte stejné přihlašovací údaje.

## 4. Test publish / subscribe

### Developer Tools (v HA)

1. **Developer tools → MQTT**.
2. **Listen to a topic:** zadejte např. `home/test` → Start listening.
3. **Publish a packet:** topic `home/test`, payload `hello` → Publish.
4. V části Listen by se měla objevit zpráva `hello`.

### Externí klient (volitelně)

Z PC ve stejné LAN (MQTT Explorer nebo `mosquitto_pub` / `mosquitto_sub`):

```bash
# Subscribe
mosquitto_sub -h RESERVED_IP -p 1883 -u YOUR_MQTT_USERNAME -P YOUR_MQTT_PASSWORD -t 'home/test'

# Publish (jiný terminál)
mosquitto_pub -h RESERVED_IP -p 1883 -u YOUR_MQTT_USERNAME -P YOUR_MQTT_PASSWORD -t 'home/test' -m 'hello'
```

Placeholdery: `RESERVED_IP`, `YOUR_MQTT_USERNAME`, `YOUR_MQTT_PASSWORD`.

Úspěch = zpráva dorazí subscriberovi. Tím je lokální MQTT ověřené.

## Troubleshooting

| Problém | Co zkontrolovat |
|---------|-----------------|
| Integrace se nepřipojí | Addon běží? Správný host `core-mosquitto`? Restart addonu i HA |
| Auth failed | Username/password, anonymous vypnuté, restart Mosquitto po změně |
| Externí klient nefunguje | Firewall / síť; používejte IP NUC; port 1883 je otevřený v addonu |

## Další kroky

- [Tuya Local](tuya-local.md)
- [Checklist](checklist.md)
