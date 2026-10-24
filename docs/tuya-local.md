# Tuya Local — lokální ovládání Smart Life

Integrace **Tuya Local** (přes **HACS**) řídí zařízení Tuya / Smart Life po LAN. Cloud je potřeba jen **jednorázově** pro získání local key; za běhu má řízení fungovat **bez cloudu**.

Aplikace **Smart Life** slouží jen k **párování** zařízení do 2.4 GHz Wi‑Fi.

## Předpoklady

- HAOS běží a jste admin
- Zařízení a NUC jsou ve **stejné LAN** (typicky 2.4 GHz Wi‑Fi pro IoT)
- Máte účet Smart Life / Tuya a zařízení už spárovaná v aplikaci

## 1. Instalace HACS

1. Nainstalujte **HACS** podle oficiálního návodu pro Home Assistant OS (download + restart HA).
2. Dokončete onboarding HACS (GitHub účet dle aktuálního postupu HACS).
3. Ověřte, že HACS je v postranním menu.

## 2. Instalace Tuya Local

1. V HACS → **Integrations** vyhledejte **Tuya Local** (maintainer / repo dle HACS záznamu).
2. **Download** → restart Home Assistant.
3. **Settings → Devices & services → Add integration → Tuya Local**.

## 3. Local key (jednorázově)

Pro každé zařízení potřebujete typicky:

| Údaj | Placeholder |
|------|-------------|
| Device ID | `YOUR_DEVICE_ID` |
| Local key | `YOUR_LOCAL_KEY` |
| IP adresa v LAN | `YOUR_DEVICE_IP` |
| Protocol / DPS | dle průvodce Tuya Local |

### Varianta A — Tuya IoT Platform

1. Vytvořte projekt na [Tuya IoT Platform](https://iot.tuya.com/) (cloud project linked to Smart Life / Tuya app účet).
2. Získejte **Access ID** / **Access Secret** — placeholdery: `YOUR_TUYA_ACCESS_ID`, `YOUR_TUYA_ACCESS_SECRET`.
3. Propojte app účet s projektem (QR / linking dle aktuální dokumentace Tuya).
4. V seznamu zařízení zkopírujte Device ID a local key.

### Varianta B — tinytuya

1. Na PC v LAN nainstalujte `tinytuya` (Python).
2. Spusťte wizard / scan dle dokumentace tinytuya a exportujte devices soubor.
3. Z výsledku vezměte `id`, `key`, `ip` — **necommitujte** je do gitu.

Příklad struktury (pouze ilustrace s placeholdery):

```json
{
  "devices": [
    {
      "name": "zasuvka-obyvak",
      "id": "YOUR_DEVICE_ID",
      "key": "YOUR_LOCAL_KEY",
      "ip": "YOUR_DEVICE_IP"
    }
  ]
}
```

Po získání klíčů cloud **není** potřeba pro běžné on/off řízení přes Tuya Local.

## 4. Přidání zařízení v Tuya Local

1. Spusťte průvodce **Tuya Local**.
2. Zadejte IP, Device ID, local key (nebo použijte autodiscovery, pokud nabízí a klíče doplníte).
3. Vyberte správný produkt / DPS mapování (zásuvka, světlo, spínač).
4. Ověřte entitu v HA: přepněte on/off z UI.

Začněte **kritickými** zařízeními (zásuvky, světla, spínače), ne celým katalogem.

## 5. Role Smart Life aplikace

- **Ano:** párování nového zařízení do Wi‑Fi, reset do pairing módu
- **Ne (cíl):** běžné denní ovládání — to má dělat Home Assistant

Po párování zařízení nechte na stejné Wi‑Fi jako NUC. Neměňte síť bez aktualizace IP / rezervace v routeru.

## 6. Offline WAN test (bez cloudu)

Ověření, že řízení nepotřebuje internet:

1. Zapněte / vypněte zařízení z HA — funguje.
2. Na routeru **dočasně odpojte WAN** (nebo zakažte internet pro NUC + IoT zařízení, LAN nechte).
3. Znovu přepněte zařízení z HA.
4. Úspěch = zařízení reaguje i bez WAN.
5. WAN zase zapněte.

Pokud bez WAN nefunguje:

- Zařízení stále komunikuje přes cloudovou integraci místo Tuya Local — zkontrolujte, že ovládáte entity z **Tuya Local**.
- Špatný local key / IP / protocol version — znovu načtěte klíč a IP.
- Zařízení na jiné VLAN / AP izolace — vypněte client isolation na Wi‑Fi.

## Troubleshooting

| Problém | Co zkontrolovat |
|---------|-----------------|
| Timeout / unavailable | Stejná LAN? Správná IP? DHCP rezervace pro zařízení? |
| Auth / decrypt error | Neplatný local key — znovu export z Tuya IoT / tinytuya |
| Funguje jen s internetem | Používáte cloudovou Tuya integraci — přepněte na Tuya Local entity |
| Zařízení nevidí HA | 2.4 GHz, AP isolation off, firewall mezi IoT a NUC |

## Bezpečnost

Do repozitáře **nikdy** nedávejte:

- `YOUR_LOCAL_KEY`, `YOUR_DEVICE_ID`
- `YOUR_TUYA_ACCESS_ID`, `YOUR_TUYA_ACCESS_SECRET`
- hesla Smart Life / Tuya

## Další kroky

- [Checklist](checklist.md)
