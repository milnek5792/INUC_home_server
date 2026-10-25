# Checklist — server je hotový

Označte položky po ověření. Žádná položka nevyžaduje commit secretů do gitu.

## Home Assistant OS na NUC

- [ ] Windows data zálohována (disk NUC byl přepsán)
- [ ] BIOS: UEFI on, Secure Boot off, boot z interního disku
- [ ] HAOS `generic-x86-64` zapsán na interní disk (Ubuntu Live → Restore Disk Image)
- [ ] HAOS běží místo Windows
- [ ] UI dostupné na `http://homeassistant.local:8123` (nebo rezervované IP)
- [ ] Onboarding dokončen (admin účet, lokalita)
- [ ] DHCP rezervace pro NUC v routeru

## MQTT (Mosquitto)

- [ ] Addon **Mosquitto broker** nainstalován a běží
- [ ] MQTT uživatel vytvořen, **anonymous vypnuté**
- [ ] Integrace MQTT napojená na `core-mosquitto`
- [ ] Publish/subscribe test prošel (např. topic `home/test`)

## Tuya Local

- [ ] HACS nainstalován
- [ ] Tuya Local nainstalován přes HACS
- [ ] Local key získán jednorázově (Tuya IoT / tinytuya) — uložen mimo git
- [ ] Alespoň jedno Smart Life zařízení přidané v Tuya Local
- [ ] On/off z HA funguje
- [ ] Offline WAN test: řízení funguje **bez internetu**
- [ ] Smart Life app používána jen pro párování

## Dokumentace a provoz

- [ ] Postup odpovídá docs v tomto repu
- [ ] V gitu nejsou hesla, local keys ani device ID

## Kritéria milníku (souhrn)

| Kritérium | Hotovo |
|-----------|--------|
| HAOS běží na NUC místo Windows | [ ] |
| Mosquitto přijímá lokální MQTT zprávy | [ ] |
| Alespoň jedno Smart Life zařízení se ovládá z HA bez cloudu | [ ] |
| Dokumentace pokrývá instalaci a provoz | [ ] |

## Související docs

- [Instalace NUC](nuc-install.md)
- [MQTT Mosquitto](mqtt-mosquitto.md)
- [Tuya Local](tuya-local.md)
