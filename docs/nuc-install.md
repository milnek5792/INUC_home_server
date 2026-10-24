# Instalace Home Assistant OS na Intel NUC

Instalace HAOS **přepíše** interní disk. Windows a všechna data na disku NUC budou smazána.

## 1. Záloha Windows

Před instalací:

1. Zkopírujte důležitá data z NUC na externí disk / NAS / cloud.
2. Poznamenejte si Wi‑Fi heslo, IP rozsah routeru a případné licence softwaru (pokud je budete potřebovat jinde).
3. Ověřte, že máte druhý počítač pro zápis USB image.

## 2. BIOS (UEFI / Secure Boot)

1. Restartujte NUC a vstupte do BIOS/UEFI (typicky `F2` nebo `Del` při startu — dle modelu).
2. Nastavte boot mód na **UEFI** (ne Legacy/CSM).
3. Pořadí bootu: nejdřív USB, pak interní disk (pro instalaci).
4. **Secure Boot:**
   - Pokud USB s HAOS nenabootuje, Secure Boot **vypněte**.
   - Po úspěšné instalaci můžete Secure Boot nechat vypnutý, pokud HAOS image není podepsaný pro váš firmware.
5. Uložte změny a restartujte.

## 3. USB flash s HAOS generic-x86-64

### Požadavky

- USB flash min. **8 GB** (obsah bude přepsán)
- Druhý počítač s balenaEtcher, Rufus, nebo `dd`

### Stážení image

1. Otevřete oficiální stránku Home Assistant OS pro **Generic x86-64**.
2. Stáhněte image `haos_generic-x86-64-….img.xz` (nebo aktuální ekvivalent).
3. Ověřte checksum, pokud je na stránce uveden.

### Zápis na USB

**balenaEtcher (doporučeno):**

1. Vyberte stažený image (Etcher umí `.xz`).
2. Vyberte USB disk.
3. Flash — počkejte na dokončení.

**Linux (`dd`) — příklad:**

```bash
# Zjistěte zařízení (např. /dev/sdX) — NIKDY nezapisujte na systémový disk
lsblk

# Rozbalení a zápis (nahraďte /dev/sdX)
xzcat haos_generic-x86-64-VERSION.img.xz | sudo dd of=/dev/sdX bs=4M status=progress conv=fsync
```

Placeholdery: `VERSION`, `/dev/sdX`.

## 4. Instalace na interní disk NUC

1. Vložte USB do NUC a bootujte z USB.
2. Postupujte podle průvodce instalací HAOS na **interní disk** (ne na USB).
3. Po zápisu image vyjměte USB a restartujte NUC.
4. NUC by měl nabootovat HAOS z interního disku (LED / síťová aktivita).

Pokud se NUC pořád vrací na Windows nebo nejde bootovat:

- Zkontrolujte boot order v BIOS (interní disk první, bez USB).
- Ověřte UEFI mód a Secure Boot.
- Znovu zapište image na USB a opakujte instalaci.

## 5. První boot a onboarding

1. Počkejte několik minut na první start (inicializace může trvat déle).
2. V prohlížeči na stejném LAN otevřete:

   ```text
   http://homeassistant.local:8123
   ```

3. Pokud mDNS nefunguje, najděte IP NUC v routeru (DHCP klienty) a otevřete `http://IP_ADDRESS:8123`.
4. Dokončete onboarding:
   - jméno administrátora a heslo (`YOUR_HA_ADMIN_PASSWORD`)
   - lokalita / časová zóna
   - síť dle potřeby

**Nikdy necommitujte** heslo administrátora do gitu.

## 6. DHCP rezervace

Aby měla NUC stabilní adresa:

1. V administraci routeru najděte klienta Home Assistant / NUC (MAC adresa).
2. Vytvořte **DHCP rezervaci** (statická IP v rozsahu LAN, mimo dynamický pool pokud to router vyžaduje).
3. Restartujte NUC nebo obnovte DHCP lease.
4. Ověřte přístup z PC i telefonu ve stejné síti na `http://homeassistant.local:8123` nebo `http://RESERVED_IP:8123`.

Placeholder: `RESERVED_IP` (např. `192.168.1.50`).

## 7. Volitelně později

Mimo první milník: vzdálený přístup (Tailscale, Nabu Casa). Nejdřív dokončete MQTT a Tuya Local v LAN.

## Další kroky

- [MQTT — Mosquitto](mqtt-mosquitto.md)
- [Tuya Local](tuya-local.md)
- [Checklist](checklist.md)
