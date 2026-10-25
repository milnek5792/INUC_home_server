# Instalace Home Assistant OS na Intel NUC

Oficiální postup: [Generic x86-64](https://www.home-assistant.io/installation/generic-x86-64)

**Důležité:** Home Assistant OS (**HAOS**) **nemá** klasický instalátor ani průvodce jako Windows. Image se **přímo zapíše** na disk. Doporučená metoda = **Ubuntu Live na USB** → z Ubuntu zapíšete HAOS na **interní disk** NUC (Metoda 1).

Etcher/Rufus nejdřív slouží k vytvoření **Ubuntu** USB — ne k „instalaci HAOS průvodcem“.

Instalace **přepíše** interní disk. Windows a všechna data na disku NUC budou smazána.

---

## Co budete potřebovat

| Věc | Poznámka |
|-----|----------|
| Intel NUC s Windows | Disk se **kompletně smaže** |
| Druhý počítač (Windows/Mac/Linux) | Pro stažení a zápis USB |
| USB flash **min. 8 GB** | Obsah se přepíše |
| Ethernet kabel | NUC při prvním startu potřebuje internet |
| Monitor + klávesnice na NUC | Aspoň pro BIOS a Ubuntu |

---

## Metoda 1 (doporučená) — Ubuntu Live → zápis na interní disk

Toto je oficiálně doporučený postup pro zápis HAOS na **interní disk** NUC.

### Fáze A — Záloha (povinné)

1. Zapněte NUC s Windows.
2. Zkopírujte všechna data, která chcete zachovat (Dokumenty, fotky, hesla z prohlížeče…).
3. Zapište si:
   - heslo k Wi‑Fi (pro Tuya zařízení později)
   - jméno sítě
4. **Windows po instalaci HAOS už nespustíte** (disk bude přepsán).

Až máte zálohu hotovou, pokračujte.

### Fáze B — Stáhnout nástroje na druhém PC

Pracujete na **druhém počítači**, ne na NUC.

#### B1. Stáhnout Rufus (Windows) — doporučeno pro Ubuntu USB

1. Otevřete: https://rufus.ie/
2. Stáhněte **Rufus** (portable nebo installer — jedno).
3. Spusťte Rufus (může chtít oprávnění správce).

*(Místo Rufusu můžete použít [balenaEtcher](https://etcher.balena.io/) — oboje funguje.)*

#### B2. Stáhnout Ubuntu Desktop ISO

1. Otevřete: https://ubuntu.com/download/desktop
2. Stáhněte aktuální **Ubuntu Desktop** (`.iso`, cca 5–6 GB).
3. Počkejte na dokončení stahování (Soubory / Downloads).

#### B3. (Zatím nestahujte HAOS image)

Image HAOS stáhnete **až v Ubuntu na NUC** (máte tam internet a rovnou Disks). Alternativně můžete stáhnout i teď a zkopírovat na USB — ale jednodušší je stáhnout v Ubuntu.

### Fáze C — Vytvořit bootovací Ubuntu USB

1. Vložte USB flash do **druhého PC**.
2. **Uložte si data z USB** — flash přepíše celý USB disk.
3. Spusťte **Rufus**.
4. Nastavení (typicky):
   - **Device / Zařízení:** vyberte vaše USB (zkontrolujte velikost, ať to není jiný disk!)
   - **Boot selection:** klikněte **SELECT** → vyberte stažený `ubuntu-….iso`
   - **Partition scheme:** **GPT**
   - **Target system:** **UEFI (non CSM)**
5. Klikněte **START**.
6. Pokud se Rufus zeptá na režim zápisu ISO: zvolte doporučený (**ISO mode** / Write in ISO Image mode).
7. Potvrďte varování, že data na USB budou smazána.
8. Počkejte, až Rufus doběhne (zelený status READY / Hotovo).
9. Bezpečně USB odpojte.

USB je hotové, když Rufus hlásí úspěch.

### Fáze D — BIOS na NUC

1. Do NUC zapojte: monitor (HDMI), klávesnici, **ethernet** (do routeru), napájení.
2. Zapněte NUC.
3. Hned po zapnutí **opakovaně mačkejte F2** (na některých NUC `Del` / `F10`) — vstoupíte do BIOS.
4. Najděte a nastavte:
   - **Boot mode: UEFI** (ne Legacy / CSM)
   - **Secure Boot: Disabled** (vypnuto) — povinné pro HAOS
5. Uložte a ukončete (**Save & Exit**, často F10 → Yes).
6. NUC se restartuje.

### Fáze E — Boot Ubuntu Live z USB

1. Vložte Ubuntu USB do NUC.
2. Zapněte NUC.
3. Pokud nenabootuje z USB:
   - při startu mačkejte **F10** (boot menu; někdy F12)
   - vyberte USB položku s **UEFI** v názvu
4. Na obrazovce Ubuntu:
   - vyberte **Try Ubuntu** / **Vyzkoušet Ubuntu**
   - **NE** „Install Ubuntu“ — Ubuntu neinstalujeme, jen spouštíme z USB
5. Počkejte, až se načte plocha Ubuntu.
6. Ověřte internet: otevřete Firefox, zkuste libovolnou stránku.
   - Ethernet by měl fungovat sám.
   - Wi‑Fi: klikněte na ikonu sítě vpravo nahoře a připojte se.

### Fáze F — Stáhnout HAOS image v Ubuntu

1. V Firefoxu otevřete aktuální image:

   ```text
   https://github.com/home-assistant/operating-system/releases/download/18.3/haos_generic-x86-64-18.3.img.xz
   ```

2. Soubor se stáhne do **Downloads** (Stažené).
3. Počkejte na dokončení (cca 500+ MB).
4. **Rozbalte** `.xz`:
   - otevřete složku Downloads
   - pravý klik na `haos_generic-x86-64-18.3.img.xz` → **Extract Here** / Extrahovat sem  
   - nebo: dvojklik → Extrahovat
5. Výsledek: soubor `haos_generic-x86-64-18.3.img` (větší, bez `.xz`).

### Fáze G — Zapsat HAOS na interní disk NUC

**Toto smaže Windows.**

1. Vlevo dole klikněte **Show Applications** (mřížka / tečky).
2. Napište `Disks` a otevřete aplikaci **Disks** (Disky).
3. V **levém sloupci** vyberte **interní disk** NUC:
   - typicky SSD / NVMe, větší než USB
   - **ne** USB flash s Ubuntu
4. Nahoře klikněte na **tři tečky** (menu) → **Restore Disk Image…**
5. Vyberte rozbalený soubor `haos_generic-x86-64-18.3.img`.
6. Spusťte obnovu (**Start Restoring…** → potvrďte **Restore**).
7. Pokud chyba „target is busy“ / Swap:
   - ujistěte se, že běžíte **Try Ubuntu** z USB, ne z disku
   - v Disks vyberte partition **Swap** → Stop (odpojit) → zkuste znovu
8. Počkejte na dokončení zápisu (progress v Disks).

### Fáze H — První start Home Assistant

1. V Ubuntu: **Shut Down** (vypnout).
2. Až Ubuntu řekne, že můžete vyjmout USB — **vyjměte USB**.
3. Zapněte NUC **bez** USB (ethernet stále zapojený).
4. Po 1–2 minutách by na monitoru měl být banner Home Assistant (console).
5. Na druhém PC / telefonu ve stejné Wi‑Fi/LAN otevřete prohlížeč:

   ```text
   http://homeassistant.local:8123
   ```

6. Pokud nefunguje:
   - v routeru najděte nové zařízení / IP NUC
   - otevřete `http://IP_ADRESA:8123`
7. První start stahuje komponenty — může trvat **5–20 minut**. Buďte trpěliví.
8. Dokončete onboarding (účet admin, lokalita, síť).

**Nikdy necommitujte** heslo administrátora do gitu. V dokumentaci používejte jen placeholder `YOUR_HA_ADMIN_PASSWORD`.

---

## Metoda 2 (alternativa) — Etcher zapíše HAOS přímo na boot médium

Použijte jen když **nechcete** přepisovat interní disk a HAOS má běžet z **USB / vyměnitelného disku**, nebo když Metoda 1 není dostupná.

1. Na druhém PC stáhněte image:

   ```text
   https://github.com/home-assistant/operating-system/releases/download/18.3/haos_generic-x86-64-18.3.img.xz
   ```

2. Nainstalujte [balenaEtcher](https://etcher.balena.io/).
3. V Etcheru:
   - vyberte stažený `.img.xz` (Etcher umí `.xz` bez ručního rozbalení)
   - vyberte **USB / vyměnitelný disk** (obsah se smaže)
   - **Flash** — počkejte na dokončení
4. V BIOS NUC: **UEFI on**, **Secure Boot off**.
5. Bootujte NUC z tohoto média (F10 / F12 boot menu → UEFI USB).
6. Otevřete `http://homeassistant.local:8123`.

**Poznámka:** Tato metoda **nezapisuje** HAOS na interní SSD NUC. Pro trvalou instalaci místo Windows použijte **Metodu 1**.

---

## Když NUC nenajde bootovatelný disk

V BIOS přidejte EFI boot entry na:

```text
\EFI\BOOT\bootx64.efi
```

Nebo z Ubuntu Live (UEFI boot):

```bash
sudo efibootmgr --create --disk /dev/nvme0n1 --part 1 --label "HAOS" \
  --loader '\EFI\BOOT\bootx64.efi'
```

(Disk může být i `/dev/sda` — ověřte v Disks / `lsblk`.)

---

## DHCP rezervace

Aby měla NUC stabilní adresa:

1. V administraci routeru najděte klienta Home Assistant / NUC (MAC adresa).
2. Vytvořte **DHCP rezervaci** (statická IP v rozsahu LAN, mimo dynamický pool pokud to router vyžaduje).
3. Restartujte NUC nebo obnovte DHCP lease.
4. Ověřte přístup z PC i telefonu ve stejné síti na `http://homeassistant.local:8123` nebo `http://RESERVED_IP:8123`.

Placeholder: `RESERVED_IP` (např. `192.168.1.50`).

---

## Checklist instalace

- [ ] Záloha Windows hotová
- [ ] Rufus/Etcher stažený
- [ ] Ubuntu ISO stažené
- [ ] Ubuntu USB vytvořené
- [ ] BIOS: UEFI + Secure Boot off
- [ ] Try Ubuntu běží na NUC
- [ ] HAOS image stažený a rozbalený
- [ ] Restore Disk Image na interní disk hotový
- [ ] USB vyjmuté, HA běží na `:8123`
- [ ] DHCP rezervace nastavená

---

## Volitelně později

Mimo první milník: vzdálený přístup (Tailscale, Nabu Casa). Nejdřív dokončete MQTT a Tuya Local v LAN.

## Další kroky

- [MQTT — Mosquitto](mqtt-mosquitto.md)
- [Tuya Local](tuya-local.md)
- [Checklist](checklist.md)

## Odkazy

- Návod HA: https://www.home-assistant.io/installation/generic-x86-64
- Ubuntu Desktop: https://ubuntu.com/download/desktop
- Rufus: https://rufus.ie/
- balenaEtcher: https://etcher.balena.io/
- HAOS image 18.3: https://github.com/home-assistant/operating-system/releases/download/18.3/haos_generic-x86-64-18.3.img.xz
