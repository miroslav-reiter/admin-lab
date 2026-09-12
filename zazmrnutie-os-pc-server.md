# 🧊 Čo robiť, ak PC alebo server zamrzne bez reštartovania

Praktický návod pre **Microsoft Windows**, **Microsoft Windows Server** a **Linux**. Cieľom je najskôr obnoviť konkrétnu aplikáciu, proces, službu, grafické prostredie alebo používateľskú reláciu a až potom uvažovať o reštarte celého operačného systému.

> **Zásada administrátora:** používame najmenší zásah, ktorý dokáže problém vyriešiť. Ak zamrzne jedna aplikácia, nereštartujeme celý počítač. Ak zamrzne jedna služba alebo RDP relácia, nereštartujeme celý server.

## 📑 Obsah

1. [Rýchla diagnostika](#rychla-diagnostika)
2. [Microsoft Windows](#microsoft-windows)
   1. [Zamrzla jedna aplikácia](#windows-aplikacia)
   2. [Zamrzol Windows Explorer](#windows-explorer)
   3. [Zamrzol alebo sčernel obraz](#windows-grafika)
   4. [Problém spôsobuje služba](#windows-sluzba)
   5. [Diagnostika po obnovení systému](#windows-diagnostika)
   6. [Praktické cvičenie Windows](#windows-cvicenie)
3. [Microsoft Windows Server](#microsoft-windows-server)
   1. [Zamrzol proces](#server-proces)
   2. [Zamrzla služba](#server-sluzba)
   3. [Zamrzla RDP relácia](#server-rdp)
   4. [Lokálne GUI nereaguje](#server-vzdialene)
   5. [Praktické cvičenie Windows Server](#server-cvicenie)
4. [Linux](#linux)
   1. [Zamrzla jedna aplikácia alebo proces](#linux-proces)
   2. [Zamrzlo grafické prostredie](#linux-gui)
   3. [Nedostatok RAM](#linux-ram)
   4. [Kontrola služieb a logov](#linux-logy)
   5. [Praktické cvičenie Linux](#linux-cvicenie)
5. [Rozhodovacia tabuľka](#rozhodovacia-tabulka)
6. [Najčastejšie chyby](#najcastejsie-chyby)
7. [Oficiálna dokumentácia](#oficialna-dokumentacia)

<a id="rychla-diagnostika"></a>
## 1. Rýchla diagnostika

Najskôr potrebujeme určiť, **čo presne zamrzlo**.

| Stav | Typický príznak | Prvý zásah |
| --- | --- | --- |
| Aplikácia | jedna aplikácia nereaguje, systém funguje | ukončíme iba proces aplikácie |
| Windows Explorer | nefunguje pracovná plocha, panel úloh alebo ponuka Štart | reštartujeme `explorer.exe` |
| Grafický ovládač | obraz je čierny alebo zamrznutý | resetujeme grafický ovládač |
| Služba | konkrétna funkcia systému alebo servera nefunguje | skontrolujeme a reštartujeme iba službu |
| RDP relácia | server funguje, ale konkrétna relácia nereaguje | ukončíme iba danú reláciu |
| Linux GUI | grafické prostredie nereaguje, jadro systému funguje | prepneme sa do TTY |
| Nedostatok RAM | systém extrémne spomalí a procesy nereagujú | identifikujeme proces s vysokou spotrebou RAM |
| Celý systém | nereaguje GUI, terminál ani vzdialená správa | až potom použijeme riadený reštart |

Odporúčané poradie zásahov:

1. Overíme, či reaguje klávesnica a myš
2. Skúsime otvoriť Správcu úloh, terminál alebo textovú konzolu
3. Zistíme názov procesu a jeho PID
4. Skontrolujeme využitie CPU a RAM
5. Pokúsime sa proces ukončiť korektne
6. Až potom použijeme vynútené ukončenie
7. Pri službe reštartujeme iba konkrétnu službu
8. Pri RDP probléme ukončíme iba konkrétnu reláciu
9. Ak lokálne rozhranie nefunguje, skúsime vzdialenú správu
10. Reštart celého systému použijeme až ako ďalší krok
11. Tvrdé vypnutie napájania používame iba ako poslednú možnosť

<a id="microsoft-windows"></a>
# 🪟 2. Microsoft Windows

<a id="windows-aplikacia"></a>
## 2.1 Zamrzla jedna aplikácia

Najskôr otvoríme Správcu úloh:

```text
Ctrl + Shift + Esc
```

Ak skratka nereaguje, môžeme skúsiť:

```text
Ctrl + Alt + Delete
```

V CMD zobrazíme procesy:

```cmd
tasklist
```

Vyfiltrujeme procesy so stavom `NOT RESPONDING`:

```cmd
tasklist /FI "STATUS eq NOT RESPONDING"
```

Vyhľadáme konkrétnu aplikáciu:

```cmd
tasklist /FI "IMAGENAME eq notepad.exe"
```

Najskôr skúsime ukončenie bez `/F`:

```cmd
taskkill /IM notepad.exe
```

Konkrétnu inštanciu ukončíme podľa PID:

```cmd
taskkill /PID 1234
```

Až keď proces nereaguje, použijeme vynútené ukončenie:

```cmd
taskkill /F /PID 1234
```

Parameter `/F` môže spôsobiť stratu neuložených dát aplikácie.

<a id="windows-explorer"></a>
## 2.2 Zamrzol Windows Explorer

Ak nereaguje pracovná plocha, panel úloh alebo ponuka Štart, ale samotný Windows funguje, môžeme reštartovať používateľský shell.

Cez Správcu úloh nájdeme **Windows Explorer** a použijeme **Restart**.

Cez CMD môžeme použiť:

```cmd
taskkill /F /IM explorer.exe
start explorer.exe
```

Tým nereštartujeme celý operačný systém.

<a id="windows-grafika"></a>
## 2.3 Zamrzol alebo sčernel obraz

Pri probléme s grafickým ovládačom môžeme skúsiť:

```text
Windows + Ctrl + Shift + B
```

Windows sa pokúsi resetovať grafický ovládač. Obrazovka môže krátko bliknúť a môže zaznieť zvukový signál.

Ak sa následne zobrazí čierna obrazovka s kurzorom, môžeme skúsiť:

```text
Ctrl + Shift + Esc
```

Potom v Správcovi úloh reštartujeme **Windows Explorer**.

<a id="windows-sluzba"></a>
## 2.4 Problém spôsobuje služba

Grafickú konzolu služieb otvoríme:

```text
services.msc
```

Stav služieb môžeme kontrolovať aj cez CMD:

```cmd
sc query
```

V PowerShelli zobrazíme služby:

```powershell
Get-Service
```

Konkrétnu službu najskôr skontrolujeme:

```powershell
Get-Service -Name Spooler
```

Pred zásahom môžeme použiť simuláciu:

```powershell
Restart-Service -Name Spooler -WhatIf
```

Na testovacom počítači môžeme službu reštartovať:

```powershell
Restart-Service -Name Spooler
```

Službu nereštartujeme iba podľa názvu bez znalosti jej účelu a závislostí.

<a id="windows-diagnostika"></a>
## 2.5 Diagnostika po obnovení systému

Po obnovení systému skontrolujeme príčinu problému:

1. Otvoríme Monitor prostriedkov pomocou `resmon`
2. Otvoríme Monitor výkonu pomocou `perfmon`
3. Otvoríme Zobrazovač udalostí pomocou `eventvwr.msc`
4. Skontrolujeme log `System`
5. Skontrolujeme log `Application`
6. Porovnáme čas chyby s časom zamrznutia
7. Skontrolujeme procesy s vysokým CPU
8. Skontrolujeme procesy s vysokou spotrebou RAM
9. Pri probléme s obrazom skontrolujeme grafický ovládač

<a id="windows-cvicenie"></a>
## 2.6 Praktické cvičenie Windows

### Cvičenie A: Bezpečné ukončenie testovacej aplikácie

1. Spustíme Poznámkový blok

```cmd
start notepad
```

2. Zistíme jeho PID

```cmd
tasklist /FI "IMAGENAME eq notepad.exe"
```

3. Ukončíme konkrétnu inštanciu

```cmd
taskkill /PID 1234
```

4. PID `1234` nahradíme skutočným PID z výstupu

### Cvičenie B: Reštart Windows Explorer

1. Otvoríme Správcu úloh
2. Nájdeme proces **Windows Explorer**
3. Použijeme **Restart**
4. Overíme, že panel úloh a pracovná plocha znovu fungujú

<a id="microsoft-windows-server"></a>
# 🖥️ 3. Microsoft Windows Server

Pri serveri je dôležité odlíšiť problém jedného procesu, služby alebo používateľskej relácie od problému celého servera.

<a id="server-proces"></a>
## 3.1 Zamrzol proces

Zobrazíme procesy:

```cmd
tasklist
```

Podrobnejší výpis:

```cmd
tasklist /V
```

Proces ukončíme podľa PID:

```cmd
taskkill /PID 1234
```

Až pri nereagujúcom procese použijeme:

```cmd
taskkill /F /PID 1234
```

V PowerShelli zobrazíme procesy s najvyšším CPU:

```powershell
Get-Process |
    Sort-Object CPU -Descending |
    Select-Object -First 10 Name, Id, CPU
```

Procesy s najvyššou pracovnou sadou RAM:

```powershell
Get-Process |
    Sort-Object WorkingSet64 -Descending |
    Select-Object -First 10 Name, Id, WorkingSet64
```

<a id="server-sluzba"></a>
## 3.2 Zamrzla služba

Najskôr zistíme stav služby:

```powershell
Get-Service -Name Spooler
```

Overíme plánovaný zásah:

```powershell
Restart-Service -Name Spooler -WhatIf
```

V laboratórnom prostredí môžeme službu reštartovať:

```powershell
Restart-Service -Name Spooler
```

Na produkčnom serveri najskôr preveríme závislosti a dopad na používateľov.

<a id="server-rdp"></a>
## 3.3 Zamrzla RDP relácia

Zobrazíme relácie:

```cmd
query session
```

Zobrazíme prihlásených používateľov:

```cmd
query user
```

Používateľa môžeme pred zásahom upozorniť:

```cmd
msg 4 "Administratorsky zasah: relacia bude odhlasena"
```

Konkrétnu reláciu ukončíme podľa jej ID:

```cmd
logoff 4
```

ID `4` je iba príklad. Použijeme skutočné ID z výstupu `query session` alebo `query user`.

<a id="server-vzdialene"></a>
## 3.4 Lokálne GUI nereaguje

Ak nefunguje lokálne GUI alebo jedna RDP relácia, server ešte nemusí byť zamrznutý.

Z iného počítača môžeme skúsiť zobraziť relácie:

```cmd
query session /server:SERVER01
```

Zobrazíme procesy vzdialeného servera:

```cmd
tasklist /S SERVER01
```

Pri nakonfigurovanom PowerShell Remotingu môžeme použiť:

```powershell
Invoke-Command -ComputerName SERVER01 -ScriptBlock {
    Get-Process |
        Sort-Object CPU -Descending |
        Select-Object -First 10 Name, Id, CPU
}
```

Skontrolujeme službu na vzdialenom serveri:

```powershell
Invoke-Command -ComputerName SERVER01 -ScriptBlock {
    Get-Service -Name Spooler
}
```

<a id="server-cvicenie"></a>
## 3.5 Praktické cvičenie Windows Server

1. Zobrazíme desať procesov s najvyšším CPU

```powershell
Get-Process |
    Sort-Object CPU -Descending |
    Select-Object -First 10 Name, Id, CPU
```

2. Skontrolujeme stav služby

```powershell
Get-Service -Name Spooler
```

3. Simulujeme jej reštart

```powershell
Restart-Service -Name Spooler -WhatIf
```

4. Zobrazíme používateľské relácie

```cmd
query session
```

5. Rozhodneme, či problém patrí procesu, službe alebo konkrétnej relácii

<a id="linux"></a>
# 🐧 4. Linux

<a id="linux-proces"></a>
## 4.1 Zamrzla jedna aplikácia alebo proces

Zobrazíme procesy:

```bash
ps aux
```

Interaktívny monitoring:

```bash
top
```

Ak máme nainštalovaný `htop`:

```bash
htop
```

Procesy s najvyšším CPU:

```bash
ps -eo pid,comm,%cpu,%mem --sort=-%cpu | head
```

Procesy s najvyššou RAM:

```bash
ps -eo pid,comm,%mem,%cpu --sort=-%mem | head
```

PID procesu vyhľadáme:

```bash
pgrep firefox
```

Najskôr použijeme korektné ukončenie signálom `SIGTERM`:

```bash
kill 1234
```

Podľa názvu môžeme použiť:

```bash
pkill firefox
```

Až keď proces nereaguje na `SIGTERM`, použijeme `SIGKILL`:

```bash
kill -9 1234
```

`SIGKILL` nedáva procesu možnosť korektne uložiť stav alebo vykonať ukončovacie operácie.

<a id="linux-gui"></a>
## 4.2 Zamrzlo grafické prostredie

Ak GUI nereaguje, ale jadro systému stále funguje, skúsime textovú virtuálnu konzolu:

```text
Ctrl + Alt + F3
```

Podľa distribúcie môžu fungovať aj ďalšie TTY konzoly napríklad `F2` až `F6`.

Po prihlásení skontrolujeme systém:

```bash
top
```

Skontrolujeme zlyhané služby:

```bash
systemctl --failed
```

Stav display managera:

```bash
systemctl status display-manager
```

Ak vieme, že problém spôsobuje iba grafická relácia, môžeme zvážiť:

```bash
sudo systemctl restart display-manager
```

Reštart display managera môže ukončiť aktuálnu grafickú reláciu a spôsobiť stratu neuložených dát.

<a id="linux-ram"></a>
## 4.3 Nedostatok RAM

Stav pamäte:

```bash
free -h
```

Krátkodobé sledovanie CPU, pamäte a swapu:

```bash
vmstat 1 5
```

Skontrolujeme nastavenie Magic SysRq:

```bash
cat /proc/sys/kernel/sysrq
```

Ak je príslušná funkcia povolená a jadro stále reaguje, môžeme pri kritickom nedostatku pamäte použiť:

```text
Alt + SysRq + F
```

Príkaz `f` vyvolá OOM Killer. Ten sa pokúsi uvoľniť pamäť ukončením procesu. Môže preto dôjsť k strate neuložených dát daného procesu.

<a id="linux-logy"></a>
## 4.4 Kontrola služieb a logov

Zlyhané služby:

```bash
systemctl --failed
```

Stav konkrétnej služby:

```bash
systemctl status ssh
```

Chyby aktuálneho bootu:

```bash
journalctl -p err -b
```

Posledné správy jadra:

```bash
sudo dmesg -T | tail -n 50
```

Aktuálne otvorené súbory a sieťové spojenia procesu:

```bash
sudo lsof -p 1234
```

<a id="linux-cvicenie"></a>
## 4.5 Praktické cvičenie Linux

Na bezpečné precvičenie použijeme proces `sleep`.

1. Spustíme testovací proces na pozadí

```bash
sleep 600 &
```

2. Nájdeme jeho PID

```bash
pgrep sleep
```

3. Zobrazíme detaily procesu

```bash
ps -fp $(pgrep sleep)
```

4. Ukončíme ho korektne

```bash
kill $(pgrep sleep)
```

5. Overíme, že proces už nebeží

```bash
pgrep sleep
```

6. Skontrolujeme stav pamäte

```bash
free -h
```

7. Skontrolujeme zlyhané služby

```bash
systemctl --failed
```

<a id="rozhodovacia-tabulka"></a>
## 5. Rozhodovacia tabuľka

| Problém | Microsoft Windows | Microsoft Windows Server | Linux |
| --- | --- | --- | --- |
| Jedna aplikácia | Task Manager, `taskkill` | `tasklist`, `taskkill` | `ps`, `top`, `htop`, `kill` |
| Shell alebo GUI | reštart `explorer.exe` | podľa typu relácie a inštalácie | TTY a display manager |
| Čierna obrazovka | `Win + Ctrl + Shift + B` | vzdialená diagnostika | TTY a kontrola grafickej služby |
| Služba | `services.msc`, `Get-Service` | `Get-Service`, `Restart-Service` | `systemctl` |
| Používateľská relácia | odhlásenie používateľa | `query session`, `logoff` | podľa použitého desktopového prostredia |
| Vysoké CPU | Task Manager, `resmon` | PowerShell | `top`, `htop`, `ps` |
| Vysoká RAM | Task Manager, `resmon` | PowerShell | `free`, `top`, `ps` |
| Logy | Event Viewer | Event Viewer, PowerShell | `journalctl`, `dmesg` |
| Lokálne GUI nefunguje | Task Manager alebo bezpečnostná obrazovka | vzdialená správa | TTY alebo SSH |

<a id="najcastejsie-chyby"></a>
## 6. Najčastejšie chyby

1. Reštartujeme celý počítač pri zamrznutí jednej aplikácie
2. Použijeme `taskkill /F` ako prvý zásah
3. Použijeme `kill -9` ako prvý zásah v Linuxe
4. Ukončíme proces iba preto, že má vysoké CPU
5. Reštartujeme službu bez kontroly jej účelu a závislostí
6. Odhlásime používateľa zo servera bez upozornenia
7. Považujeme zamrznuté GUI za zamrznutie celého operačného systému
8. Pri čiernej obrazovke okamžite vypneme Windows namiesto resetu grafického ovládača
9. Po incidente neskontrolujeme systémové logy
10. Tvrdým vypnutím napájania riskujeme poškodenie práve zapisovaných dát

<a id="oficialna-dokumentacia"></a>
## 7. Oficiálna dokumentácia

1. [Microsoft Learn – tasklist](https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/tasklist)
2. [Microsoft Learn – taskkill](https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/taskkill)
3. [Microsoft Learn – query session](https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/query-session)
4. [Microsoft Learn – query user](https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/query-user)
5. [Microsoft Learn – logoff](https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/logoff)
6. [Microsoft Learn – Get-Process](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.management/get-process)
7. [Microsoft Learn – Stop-Process](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.management/stop-process)
8. [Microsoft Learn – Restart-Service](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.management/restart-service)
9. [Microsoft Support – Troubleshooting blank screens in Windows](https://support.microsoft.com/en-us/windows/hardware/display-graphics/troubleshooting-blank-screens-in-windows)
10. [Linux Kernel Documentation – Magic SysRq](https://docs.kernel.org/admin-guide/sysrq.html)
