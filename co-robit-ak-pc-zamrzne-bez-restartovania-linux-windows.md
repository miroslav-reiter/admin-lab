# Čo robiť, ak PC zamrzne bez reštartovania v Linuxe a Microsoft Windows

Tento praktický materiál ukazuje, ako postupovať pri zamrznutí aplikácie, grafického prostredia, služby, používateľskej relácie alebo celého systému bez okamžitého reštartovania počítača alebo servera. Materiál prepája diagnostiku a správu procesov z piatich online kurzov VITA Academy a je určený na praktické precvičovanie v laboratórnom prostredí.

Príklady sú vhodné najmä pre moderné distribúcie Linuxu používajúce `systemd`, Microsoft Windows 11, Microsoft Windows Server 2022/2025 a PowerShell 7.x alebo Windows PowerShell 5.1. Niektoré príkazy vyžadujú oprávnenia správcu alebo používateľa `root`.

## Zdrojové online kurzy VITA Academy

| Kurz | Hlavné súvisiace témy |
| --- | --- |
| [Online kurz Administrátor a Správca IT](https://www.vita.sk/online-kurz-administrator-a-spravca-it/) | správa procesov, diagnostika, Linux, WSL, PowerShell, CMD, Sysinternals, monitoring služieb |
| [Online kurz Microsoft Windows III. Pokročilý](https://www.vita.sk/online-kurz-microsoft-windows-iii-pokrocily/) | služby, ovládače, optimalizácia systému, príkazový riadok, pokročilé systémové nástroje |
| [Online kurz Microsoft Windows Server I. Začiatočník](https://www.vita.sk/online-kurz-microsoft-windows-server-i-zaciatocnik/) | procesy, služby, terminál, shell, monitoring, výkon, RDP relácie |
| [Online kurz Linux Administrátor I. Začiatočník](https://www.vita.sk/online-kurz-linux-administrator-linux-admin-i-zaciatocnik/) | klávesové skratky, procesy, `top`, `htop`, `lsof`, terminál, systémová administrácia |
| [Online kurz PowerShell I. Začiatočník](https://www.vita.sk/online-kurz-powershell-i-zaciatocnik/) | shell, procesy, služby, príkazy, rúry, skripty, vzdialená správa |

<a id="obsah"></a>
## Obsah

1. [Online kurz Administrátor a Správca IT](#kurz-administrator-spravca-it)
   1. [Najskôr určíme, čo skutočne zamrzlo](#admin-diagnosticky-model)
   2. [Odporúčané poradie zásahov](#admin-poradie-zasahov)
   3. [Praktický scenár diagnostiky](#admin-prakticky-scenar)
2. [Online kurz Microsoft Windows III. Pokročilý](#kurz-windows-iii)
   1. [Čo robiť, ak PC s Microsoft Windows zamrzne bez reštartovania](#windows-zamrznutie)
   2. [Zamrzla iba jedna aplikácia](#windows-aplikacia)
   3. [Zamrzol Windows Explorer](#windows-explorer)
   4. [Zamrzol alebo sčernel obraz](#windows-grafika)
   5. [Problém spôsobuje služba Windows](#windows-sluzba)
   6. [Diagnostika po obnovení systému](#windows-diagnostika)
   7. [Praktické cvičenie Windows](#windows-cvicenie)
3. [Online kurz Microsoft Windows Server I. Začiatočník](#kurz-windows-server)
   1. [Zamrzol proces na serveri](#server-proces)
   2. [Zamrzla služba Windows Server](#server-sluzba)
   3. [Zamrzla RDP relácia](#server-rdp)
   4. [Server môžeme spravovať vzdialene](#server-vzdialena-sprava)
   5. [Praktické cvičenie Windows Server](#server-cvicenie)
4. [Online kurz Linux Administrátor I. Začiatočník](#kurz-linux-admin)
   1. [Čo robiť, ak PC s Linuxom zamrzne bez reštartovania](#linux-zamrznutie)
   2. [Zamrzla iba jedna aplikácia alebo proces](#linux-proces)
   3. [Zamrzlo grafické prostredie](#linux-gui)
   4. [Systém má problém s RAM](#linux-ram)
   5. [Kontrola systémových logov](#linux-logy)
   6. [Praktické cvičenie Linux](#linux-cvicenie)
5. [Online kurz PowerShell I. Začiatočník](#kurz-powershell)
   1. [Vyhľadanie problémových procesov](#powershell-procesy)
   2. [Ukončenie problémového procesu](#powershell-ukoncenie)
   3. [Kontrola a reštart služby](#powershell-sluzby)
   4. [Analýza udalostí Windows](#powershell-udalosti)
   5. [Vzdialená diagnostika servera](#powershell-vzdialena-sprava)
   6. [Praktické cvičenie PowerShell](#powershell-cvicenie)
6. [Rýchla rozhodovacia tabuľka](#rozhodovacia-tabulka)
7. [Praktické laboratórne úlohy](#prakticke-ulohy)
8. [Najčastejšie chyby](#najcastejsie-chyby)
9. [Bezpečnostné zásady](#bezpecnostne-zasady)
10. [Oficiálna dokumentácia a zdroje](#zdroje)

<a id="kurz-administrator-spravca-it"></a>
## 1. Online kurz Administrátor a Správca IT

Táto téma nadväzuje najmä na správu procesov, monitorovanie IT služieb, diagnostiku operačných systémov, Linux terminál, PowerShell, CMD a nástroje Sysinternals. Základným princípom administrátora je nereštartovať celý systém, ak dokážeme izolovať problém na konkrétny proces, službu, ovládač alebo používateľskú reláciu.

<a id="admin-diagnosticky-model"></a>
### 1.1 Najskôr určíme, čo skutočne zamrzlo

| Stav | Typický príznak | Prvý vhodný zásah |
| --- | --- | --- |
| Zamrzla aplikácia | jedna aplikácia nereaguje, systém funguje | ukončíme iba daný proces |
| Zamrzol shell alebo GUI | nefunguje panel, ponuka Štart alebo pracovná plocha | reštartujeme grafický shell |
| Problém s grafikou | čierna obrazovka alebo zamrznutý obraz | resetujeme grafický ovládač |
| Zamrzla služba | konkrétna serverová alebo systémová funkcia nefunguje | skontrolujeme a reštartujeme službu |
| Zamrzla používateľská relácia | systém funguje, ale konkrétny používateľ nie | ukončíme alebo odhlásime reláciu |
| Nedostatok RAM | systém výrazne spomalí, disk intenzívne pracuje, procesy nereagujú | nájdeme proces s vysokou spotrebou pamäte |
| Zamrzol celý operačný systém | nefunguje GUI, terminál ani vzdialená správa | až potom zvažujeme riadený reštart |

<a id="admin-poradie-zasahov"></a>
### 1.2 Odporúčané poradie zásahov

1. Overíme, či reaguje klávesnica a myš
2. Skúsime otvoriť Správcu úloh vo Windows alebo terminál v Linuxe
3. Zistíme, ktorý proces spotrebúva CPU alebo RAM
4. Pokúsime sa problémový proces ukončiť korektne
5. Až keď korektné ukončenie nefunguje, použijeme vynútené ukončenie
6. Ak ide o službu, reštartujeme iba konkrétnu službu
7. Ak ide o používateľskú reláciu, ukončíme iba danú reláciu
8. Ak nefunguje lokálne GUI, skúsime textovú konzolu alebo vzdialenú správu
9. Skontrolujeme systémové logy a udalosti
10. Reštart celého systému použijeme až vtedy, keď menší zásah problém nevyrieši

<a id="admin-prakticky-scenar"></a>
### 1.3 Praktický scenár diagnostiky

Predstavme si pracovnú stanicu, ktorá sa používateľovi javí ako zamrznutá. Kurzor myši sa pohybuje, ale aplikácia nereaguje.

1. Vo Windows otvoríme Správcu úloh pomocou `Ctrl + Shift + Esc`
2. V Linuxe otvoríme terminál alebo virtuálnu konzolu
3. Skontrolujeme CPU a RAM problémového procesu
4. Overíme PID procesu
5. Ukončíme iba konkrétny proces
6. Overíme, či systém opäť reaguje
7. Následne skontrolujeme logy a hľadáme príčinu problému

V administrácii platí princíp najmenšieho potrebného zásahu. Ak zamrzne jedna aplikácia, nereštartujeme celý počítač. Ak zamrzne jedna služba, nereštartujeme celý server.

[Späť na obsah](#obsah)

<a id="kurz-windows-iii"></a>
## 2. Online kurz Microsoft Windows III. Pokročilý

Táto časť nadväzuje na prácu so službami, ovládačmi, optimalizáciou systému, príkazovým riadkom a pokročilými systémovými nástrojmi Windows.

<a id="windows-zamrznutie"></a>
### 2.1 Čo robiť, ak PC s Microsoft Windows zamrzne bez reštartovania

Najskôr rozlišujeme, či zamrzla iba aplikácia, Windows Explorer, grafický ovládač, služba alebo používateľská relácia. Tvrdé vypnutie tlačidlom napájania používame až ako poslednú možnosť.

<a id="windows-aplikacia"></a>
### 2.2 Zamrzla iba jedna aplikácia

Správcu úloh otvoríme priamo:

```text
Ctrl + Shift + Esc
```

Ak táto skratka nefunguje, môžeme skúsiť bezpečnostnú obrazovku:

```text
Ctrl + Alt + Delete
```

V CMD zobrazíme procesy:

```cmd
tasklist
```

Zobrazíme procesy so stavom `NOT RESPONDING`:

```cmd
tasklist /FI "STATUS eq NOT RESPONDING"
```

Konkrétny proces ukončíme podľa názvu:

```cmd
taskkill /IM notepad.exe
```

Ak proces nereaguje na bežné ukončenie, môžeme použiť vynútené ukončenie:

```cmd
taskkill /F /IM notepad.exe
```

Proces môžeme ukončiť aj podľa PID:

```cmd
taskkill /PID 1234
```

Pri použití `/F` môže aplikácia skončiť bez uloženia rozpracovaných dát.

<a id="windows-explorer"></a>
### 2.3 Zamrzol Windows Explorer

Ak funguje systém, ale nereaguje pracovná plocha, panel úloh alebo ponuka Štart, problém môže byť v procese `explorer.exe`.

Najbezpečnejší postup je otvoriť Správcu úloh, nájsť **Windows Explorer** a použiť **Restart**.

Rovnaký zásah môžeme vykonať cez CMD:

```cmd
taskkill /F /IM explorer.exe
start explorer.exe
```

Tým nereštartujeme celý počítač. Reštartujeme iba používateľský shell Windows.

<a id="windows-grafika"></a>
### 2.4 Zamrzol alebo sčernel obraz

Microsoft odporúča pri problémoch s grafickým ovládačom skúsiť túto klávesovú skratku:

```text
Windows + Ctrl + Shift + B
```

Windows sa pokúsi obnoviť grafický ovládač. Obrazovka môže krátko bliknúť a môže zaznieť zvukový signál.

Ak vidíme iba čiernu obrazovku s kurzorom, môžeme následne skúsiť:

```text
Ctrl + Shift + Esc
```

Potom v Správcovi úloh reštartujeme **Windows Explorer**.

<a id="windows-sluzba"></a>
### 2.5 Problém spôsobuje služba Windows

Zoznam služieb môžeme otvoriť pomocou:

```text
services.msc
```

Stav služieb môžeme kontrolovať aj v CMD:

```cmd
sc query
```

Konkrétnu službu najskôr identifikujeme a až potom ju reštartujeme. Na produkčnom počítači alebo serveri nereštartujeme náhodné služby, pretože môžu mať závislosti.

<a id="windows-diagnostika"></a>
### 2.6 Diagnostika po obnovení systému

Po obnovení funkčnosti systému je vhodné zistiť príčinu zamrznutia.

1. Otvoríme Monitor prostriedkov pomocou `resmon`
2. Otvoríme Monitor výkonu pomocou `perfmon`
3. Otvoríme Zobrazovač udalostí pomocou `eventvwr.msc`
4. Skontrolujeme udalosti v protokoloch `System` a `Application`
5. Skontrolujeme procesy s vysokým využitím CPU, RAM alebo disku
6. Overíme ovládače zariadení, ak problém súvisel s obrazom alebo hardvérom

<a id="windows-cvicenie"></a>
### 2.7 Praktické cvičenie Windows

Na bezpečné precvičenie použijeme Poznámkový blok.

1. Spustíme Poznámkový blok

```cmd
start notepad
```

2. Vyhľadáme jeho proces

```cmd
tasklist /FI "IMAGENAME eq notepad.exe"
```

3. Ukončíme proces podľa názvu

```cmd
taskkill /IM notepad.exe
```

4. Znovu spustíme Poznámkový blok

```cmd
start notepad
```

5. Zistíme PID procesu a ukončíme konkrétnu inštanciu

```cmd
tasklist /FI "IMAGENAME eq notepad.exe"
taskkill /PID 1234
```

PID `1234` nahradíme skutočným PID z výstupu príkazu `tasklist`.

[Späť na obsah](#obsah)

<a id="kurz-windows-server"></a>
## 3. Online kurz Microsoft Windows Server I. Začiatočník

Pri serveri je ešte dôležitejšie nereštartovať celý operačný systém, ak problém spôsobuje iba jeden proces, jedna služba alebo jedna RDP relácia. Reštart servera môže prerušiť prácu viacerých používateľov a dostupnosť poskytovaných služieb.

<a id="server-proces"></a>
### 3.1 Zamrzol proces na serveri

Procesy zobrazíme:

```cmd
tasklist
```

Podrobnejší výpis bežiacich procesov:

```cmd
tasklist /V /FI "STATUS eq running"
```

Konkrétny proces ukončíme podľa PID:

```cmd
taskkill /PID 1234
```

Ak proces nereaguje a vieme, že jeho ukončenie je bezpečné:

```cmd
taskkill /F /PID 1234
```

Najskôr vždy overíme názov procesu, PID a jeho význam. Neukončujeme kritické systémové procesy iba preto, že spotrebúvajú veľa CPU alebo RAM.

<a id="server-sluzba"></a>
### 3.2 Zamrzla služba Windows Server

Najskôr zistíme stav služby v PowerShelli:

```powershell
Get-Service -Name Spooler
```

Pred reálnym zásahom môžeme použiť `-WhatIf`:

```powershell
Restart-Service -Name Spooler -WhatIf
```

V testovacom prostredí môžeme službu reštartovať:

```powershell
Restart-Service -Name Spooler
```

`Restart-Service` odošle službe požiadavku na zastavenie a následné spustenie. Pri kritických službách musíme najskôr vyhodnotiť závislosti a dopad na používateľov.

<a id="server-rdp"></a>
### 3.3 Zamrzla RDP relácia

Zobrazíme používateľské relácie:

```cmd
query session
```

Prípadne zobrazíme používateľov:

```cmd
query user
```

Pred odhlásením používateľa ho môžeme upozorniť:

```cmd
msg 4 "Administratorsky zasah: relacia bude odhlasena"
```

Potom môžeme ukončiť konkrétnu reláciu podľa jej ID:

```cmd
logoff 4
```

ID `4` je iba príklad. Vždy použijeme ID získané z `query session` alebo `query user`.

Odhlásenie ukončí procesy daného používateľa a môže spôsobiť stratu neuložených dát. Microsoft preto odporúča používateľa pred zásahom upozorniť.

<a id="server-vzdialena-sprava"></a>
### 3.4 Server môžeme spravovať vzdialene

Ak lokálne GUI alebo konkrétna RDP relácia nereaguje, ale operačný systém a sieť stále fungujú, môžeme skúsiť vzdialenú správu.

Z iného Windows počítača môžeme zobraziť relácie na serveri:

```cmd
query session /server:SERVER01
```

Procesy na vzdialenom serveri môžeme pri správnej konfigurácii a oprávneniach zobraziť:

```cmd
tasklist /S SERVER01
```

V PowerShell Remotingu môžeme spustiť diagnostický príkaz:

```powershell
Invoke-Command -ComputerName SERVER01 -ScriptBlock {
    Get-Process | Sort-Object CPU -Descending | Select-Object -First 10
}
```

Vzdialená správa vyžaduje správne nakonfigurované oprávnenia, sieťovú komunikáciu a PowerShell Remoting alebo príslušné služby Windows.

<a id="server-cvicenie"></a>
### 3.5 Praktické cvičenie Windows Server

Cvičenie vykonávame na testovacom serveri.

1. Zobrazíme desať procesov s najvyšším CPU

```powershell
Get-Process | Sort-Object CPU -Descending | Select-Object -First 10
```

2. Zobrazíme stav služby Print Spooler

```powershell
Get-Service -Name Spooler
```

3. Overíme plánovaný reštart služby bez vykonania zmeny

```powershell
Restart-Service -Name Spooler -WhatIf
```

4. Zobrazíme RDP relácie

```cmd
query session
```

5. Zobrazíme prihlásených používateľov

```cmd
query user
```

6. Vyhodnotíme, či problém patrí procesu, službe alebo používateľskej relácii

[Späť na obsah](#obsah)

<a id="kurz-linux-admin"></a>
## 4. Online kurz Linux Administrátor I. Začiatočník

Táto časť nadväzuje na správu a monitorovanie procesov, klávesové skratky, terminál a nástroje `top`, `htop` a `lsof`.

<a id="linux-zamrznutie"></a>
### 4.1 Čo robiť, ak PC s Linuxom zamrzne bez reštartovania

V Linuxe často dokážeme obnoviť funkčnosť bez reštartu celého systému. Najskôr zisťujeme, či zamrzla iba aplikácia, grafické prostredie alebo systém trpí nedostatkom CPU alebo RAM.

<a id="linux-proces"></a>
### 4.2 Zamrzla iba jedna aplikácia alebo proces

Základný prehľad procesov získame:

```bash
ps aux
```

Interaktívne môžeme sledovať procesy pomocou:

```bash
top
```

Ak máme nainštalovaný `htop`:

```bash
htop
```

Procesy s najvyšším využitím CPU:

```bash
ps -eo pid,comm,%cpu,%mem --sort=-%cpu | head
```

Procesy s najvyšším využitím RAM:

```bash
ps -eo pid,comm,%mem,%cpu --sort=-%mem | head
```

PID konkrétneho procesu môžeme získať pomocou:

```bash
pgrep firefox
```

Proces najskôr ukončíme korektne signálom `SIGTERM`:

```bash
kill 1234
```

Proces môžeme ukončiť aj podľa názvu:

```bash
pkill firefox
```

Až keď proces nereaguje na korektné ukončenie, môžeme použiť `SIGKILL`:

```bash
kill -9 1234
```

`SIGKILL` nedáva procesu možnosť vykonať vlastné upratanie, uloženie dát alebo korektné ukončenie. Preto ho nepoužívame ako prvú voľbu.

<a id="linux-gui"></a>
### 4.3 Zamrzlo grafické prostredie

Ak grafické prostredie nereaguje, ale jadro Linuxu a klávesnica stále fungujú, môžeme sa skúsiť prepnúť do textovej virtuálnej konzoly.

Najčastejšie použijeme:

```text
Ctrl + Alt + F3
```

V závislosti od distribúcie a konfigurácie môžu fungovať aj iné virtuálne konzoly, napríklad `Ctrl + Alt + F2` až `Ctrl + Alt + F6`.

Po prihlásení skontrolujeme procesy:

```bash
top
```

Skontrolujeme zlyhané služby:

```bash
systemctl --failed
```

Ak problém spôsobuje iba správca grafického prihlásenia, na systémoch so `systemd` môžeme zvážiť reštart display managera:

```bash
sudo systemctl restart display-manager
```

Na Ubuntu s GNOME sa môžeme stretnúť aj s:

```bash
sudo systemctl restart gdm3
```

Reštart display managera zvyčajne ukončí aktuálnu grafickú reláciu. Používateľ preto môže stratiť neuloženú prácu.

<a id="linux-ram"></a>
### 4.4 Systém má problém s RAM

Najskôr skontrolujeme dostupnú pamäť:

```bash
free -h
```

Krátkodobo môžeme sledovať stav systému:

```bash
vmstat 1 5
```

Ak systém nereaguje kvôli extrémnemu nedostatku pamäte, Linux Kernel poskytuje mechanizmus Magic SysRq. Najskôr overíme jeho nastavenie:

```bash
cat /proc/sys/kernel/sysrq
```

Ak je príslušná funkcia povolená a jadro stále reaguje, kombinácia:

```text
Alt + SysRq + F
```

vyvolá OOM Killer, ktorý sa pokúsi ukončiť proces spotrebúvajúci veľké množstvo pamäte. Kláves `SysRq` býva často na klávese `Print Screen`.

Tento zásah používame až v situácii, keď bežná správa procesov nie je dostupná. OOM Killer môže ukončiť proces a spôsobiť stratu jeho neuložených dát.

<a id="linux-logy"></a>
### 4.5 Kontrola systémových logov

Po obnovení systému skontrolujeme chyby aktuálneho bootu:

```bash
journalctl -p err -b
```

Posledné udalosti jadra môžeme zobraziť:

```bash
sudo dmesg -T | tail -n 50
```

Ak podozrievame konkrétnu službu:

```bash
systemctl status nazov-sluzby
```

Príklad pre SSH:

```bash
systemctl status ssh
```

Pri distribúciách sa názvy služieb môžu líšiť. Pred reštartovaním služby vždy najskôr zistíme jej presný názov a význam.

<a id="linux-cvicenie"></a>
### 4.6 Praktické cvičenie Linux

Na bezpečné precvičenie si vytvoríme proces `sleep`, ktorý nebude vykonávať žiadnu kritickú činnosť.

1. Spustíme proces na 600 sekúnd na pozadí

```bash
sleep 600 &
```

2. Nájdeme jeho PID

```bash
pgrep sleep
```

3. Zobrazíme proces

```bash
ps -fp $(pgrep sleep)
```

4. Ukončíme ho korektne

```bash
kill $(pgrep sleep)
```

5. Overíme, že proces už neexistuje

```bash
pgrep sleep
```

6. Znovu spustíme testovací proces

```bash
sleep 600 &
```

7. Ukončíme ho podľa názvu

```bash
pkill sleep
```

8. Skontrolujeme CPU a RAM systému

```bash
top
```

[Späť na obsah](#obsah)

<a id="kurz-powershell"></a>
## 5. Online kurz PowerShell I. Začiatočník

PowerShell je vhodný na diagnostiku procesov, služieb, udalostí a vzdialených počítačov. Pri riešení zamrznutia nám umožňuje rovnaký postup opakovať konzistentne a neskôr ho automatizovať skriptom.

<a id="powershell-procesy"></a>
### 5.1 Vyhľadanie problémových procesov

Zobrazíme všetky procesy:

```powershell
Get-Process
```

Zobrazíme desať procesov s najvyššou hodnotou CPU:

```powershell
Get-Process |
    Sort-Object CPU -Descending |
    Select-Object -First 10
```

Zobrazíme desať procesov s najväčšou pracovnou sadou v RAM:

```powershell
Get-Process |
    Sort-Object WorkingSet64 -Descending |
    Select-Object -First 10 Name, Id, CPU, WorkingSet64
```

Vyhľadáme konkrétny proces:

```powershell
Get-Process -Name notepad
```

<a id="powershell-ukoncenie"></a>
### 5.2 Ukončenie problémového procesu

Proces môžeme ukončiť podľa názvu:

```powershell
Stop-Process -Name notepad
```

Bezpečnejšie je pri výučbe vyžiadať potvrdenie:

```powershell
Stop-Process -Name notepad -Confirm
```

Konkrétnu inštanciu procesu ukončíme podľa PID:

```powershell
Stop-Process -Id 1234 -Confirm
```

`Stop-Process` pracuje s procesmi na lokálnom počítači. Na procesy iných používateľov môžeme potrebovať PowerShell spustený s oprávneniami správcu.

<a id="powershell-sluzby"></a>
### 5.3 Kontrola a reštart služby

Zobrazíme služby:

```powershell
Get-Service
```

Zobrazíme konkrétnu službu:

```powershell
Get-Service -Name Spooler
```

Najskôr si môžeme simulovať zásah:

```powershell
Restart-Service -Name Spooler -WhatIf
```

V testovacom prostredí vykonáme reštart:

```powershell
Restart-Service -Name Spooler
```

Na kritických serveroch najskôr preveríme závislosti služby a dopad jej reštartu.

<a id="powershell-udalosti"></a>
### 5.4 Analýza udalostí Windows

Posledných 30 udalostí zo systémového logu:

```powershell
Get-WinEvent -LogName System -MaxEvents 30
```

Posledných 30 udalostí aplikačného logu:

```powershell
Get-WinEvent -LogName Application -MaxEvents 30
```

Zobrazíme systémové chyby z posledných 24 hodín:

```powershell
$start = (Get-Date).AddHours(-24)

Get-WinEvent -FilterHashtable @{
    LogName   = 'System'
    Level     = 2
    StartTime = $start
}
```

Takto môžeme po zamrznutí overiť, či sa v rovnakom čase objavili chyby ovládača, služby alebo systému.

<a id="powershell-vzdialena-sprava"></a>
### 5.5 Vzdialená diagnostika servera

Ak je PowerShell Remoting správne nakonfigurovaný, môžeme diagnostikovať server bez prihlasovania do jeho grafického rozhrania.

Zobrazíme najnáročnejšie procesy na serveri:

```powershell
Invoke-Command -ComputerName SERVER01 -ScriptBlock {
    Get-Process |
        Sort-Object CPU -Descending |
        Select-Object -First 10
}
```

Zobrazíme stav služby:

```powershell
Invoke-Command -ComputerName SERVER01 -ScriptBlock {
    Get-Service -Name Spooler
}
```

Pred reštartom služby použijeme simuláciu:

```powershell
Invoke-Command -ComputerName SERVER01 -ScriptBlock {
    Restart-Service -Name Spooler -WhatIf
}
```

<a id="powershell-cvicenie"></a>
### 5.6 Praktické cvičenie PowerShell

1. Spustíme Poznámkový blok

```powershell
Start-Process notepad
```

2. Nájdeme proces

```powershell
Get-Process -Name notepad
```

3. Zobrazíme jeho PID, CPU a RAM

```powershell
Get-Process -Name notepad |
    Select-Object Name, Id, CPU, WorkingSet64
```

4. Ukončíme ho s potvrdením

```powershell
Stop-Process -Name notepad -Confirm
```

5. Overíme, že proces už nebeží

```powershell
Get-Process -Name notepad -ErrorAction SilentlyContinue
```

6. Skontrolujeme posledné udalosti Windows

```powershell
Get-WinEvent -LogName System -MaxEvents 10
```

7. Zobrazíme desať procesov s najvyššou spotrebou RAM

```powershell
Get-Process |
    Sort-Object WorkingSet64 -Descending |
    Select-Object -First 10 Name, Id, WorkingSet64
```

[Späť na obsah](#obsah)

<a id="rozhodovacia-tabulka"></a>
## 6. Rýchla rozhodovacia tabuľka

| Problém | Windows | Windows Server | Linux | PowerShell |
| --- | --- | --- | --- | --- |
| Zamrzla aplikácia | Task Manager, `taskkill` | `tasklist`, `taskkill` | `top`, `htop`, `kill`, `pkill` | `Get-Process`, `Stop-Process` |
| Zamrzol shell alebo GUI | reštart Windows Explorer | podľa typu inštalácie a relácie | virtuálna konzola, display manager | `Stop-Process` a `Start-Process` podľa potreby |
| Čierna obrazovka | `Windows + Ctrl + Shift + B` | primárne vzdialená diagnostika | virtuálna konzola a kontrola display managera | vzdialený príkaz pri dostupnom Remotingu |
| Zamrzla služba | `services.msc`, `sc query` | `Get-Service`, `Restart-Service` | `systemctl status`, `systemctl restart` | `Get-Service`, `Restart-Service` |
| Zamrzla používateľská relácia | odhlásenie používateľa | `query session`, `logoff` | ukončenie používateľskej relácie podľa prostredia | vzdialená diagnostika |
| Vysoké CPU | Task Manager, `resmon` | Task Manager, PowerShell | `top`, `htop`, `ps` | `Get-Process` a `Sort-Object CPU` |
| Vysoká RAM | Task Manager, `resmon` | Task Manager, PowerShell | `free`, `top`, `ps` | `Get-Process` a `Sort-Object WorkingSet64` |
| Potrebujeme logy | Event Viewer | Event Viewer | `journalctl`, `dmesg` | `Get-WinEvent` |
| Lokálne GUI nefunguje | Task Manager alebo bezpečnostná obrazovka | RDP alebo vzdialená správa | TTY konzola | PowerShell Remoting |

[Späť na obsah](#obsah)

<a id="prakticke-ulohy"></a>
## 7. Praktické laboratórne úlohy

### 7.1 Úloha 1: Identifikácia zamrznutého procesu vo Windows

1. Spustíme testovaciu aplikáciu `notepad.exe`
2. Zistíme jej PID pomocou `tasklist`
3. Zobrazíme iba procesy s názvom `notepad.exe`
4. Ukončíme konkrétny PID pomocou `taskkill`
5. Overíme, že proces už nie je v zozname

### 7.2 Úloha 2: Identifikácia procesu v Linuxe

1. Spustíme `sleep 600 &`
2. Nájdeme PID pomocou `pgrep sleep`
3. Skontrolujeme proces pomocou `ps`
4. Ukončíme ho pomocou `kill`
5. Overíme výsledok pomocou `pgrep`

### 7.3 Úloha 3: Porovnanie SIGTERM a SIGKILL v Linuxe

1. Spustíme testovací proces `sleep 600 &`
2. Ukončíme ho príkazom `kill PID`
3. Vysvetlíme, že predvolený signál je `SIGTERM`
4. Spustíme nový testovací proces
5. Ukončíme ho pomocou `kill -9 PID`
6. Vysvetlíme, prečo `SIGKILL` používame až ako poslednú možnosť

### 7.4 Úloha 4: Diagnostika CPU a RAM v PowerShelli

1. Zobrazíme všetky procesy pomocou `Get-Process`
2. Zoradíme procesy podľa CPU
3. Zoradíme procesy podľa `WorkingSet64`
4. Vyberieme desať najnáročnejších procesov
5. Porovnáme výsledky so Správcom úloh

### 7.5 Úloha 5: Služba Windows Server

1. Zobrazíme stav služby pomocou `Get-Service`
2. Použijeme `Restart-Service -WhatIf`
3. Vyhodnotíme, aký dopad by reštart služby mohol mať
4. Reštart vykonáme iba v laboratórnom prostredí
5. Overíme nový stav služby

### 7.6 Úloha 6: RDP relácie Windows Server

1. Zobrazíme relácie pomocou `query session`
2. Zobrazíme používateľov pomocou `query user`
3. Identifikujeme testovaciu reláciu podľa ID
4. Upozorníme používateľa pomocou `msg`
5. Ukončíme iba testovaciu reláciu pomocou `logoff ID`
6. Overíme, že ostatné relácie zostali aktívne

### 7.7 Úloha 7: Diagnostika zamrznutého grafického prostredia Linuxu

1. Prepíname sa do TTY pomocou `Ctrl + Alt + F3`
2. Prihlásime sa do textovej konzoly
3. Spustíme `top`
4. Skontrolujeme `systemctl --failed`
5. Skontrolujeme chyby pomocou `journalctl -p err -b`
6. Určíme, či je problém v procese, službe alebo grafickom prostredí

### 7.8 Úloha 8: Diagnostika Windows po zamrznutí

1. Otvoríme `eventvwr.msc`
2. Skontrolujeme log `System`
3. Skontrolujeme log `Application`
4. Otvoríme `resmon`
5. Skontrolujeme využitie CPU, RAM a disku
6. Porovnáme čas chyby s časom zamrznutia systému

### 7.9 Úloha 9: PowerShell a systémové udalosti

1. Zobrazíme posledných 30 udalostí zo systémového logu
2. Vyfiltrujeme chyby za posledných 24 hodín
3. Skontrolujeme čas vzniku udalostí
4. Identifikujeme poskytovateľa udalosti
5. Porovnáme udalosť s procesom alebo službou, ktorá mala problém

### 7.10 Úloha 10: Rozhodovanie administrátora

1. Dostaneme scenár s nereagujúcim programom
2. Rozhodneme, či stačí ukončiť proces
3. Dostaneme scenár s nefunkčnou službou
4. Rozhodneme, či stačí reštart služby
5. Dostaneme scenár so zamrznutou RDP reláciou
6. Rozhodneme, či stačí ukončiť iba reláciu
7. Dostaneme scenár s čiernou obrazovkou Windows
8. Rozhodneme, či má zmysel reset grafického ovládača
9. Dostaneme scenár s Linuxom bez funkčného GUI
10. Rozhodneme, či môžeme použiť TTY a diagnostikovať systém bez reštartu

[Späť na obsah](#obsah)

<a id="najcastejsie-chyby"></a>
## 8. Najčastejšie chyby

1. Reštartujeme celý počítač pri zamrznutí jednej aplikácie
2. Použijeme `taskkill /F` skôr, než vyskúšame korektné ukončenie aplikácie
3. V Linuxe použijeme `kill -9` ako prvý spôsob ukončenia procesu
4. Ukončíme proces iba podľa vysokej spotreby CPU bez overenia jeho významu
5. Reštartujeme kritickú službu bez kontroly závislostí
6. Odhlásime používateľa z Windows Servera bez upozornenia a bez kontroly neuložených dát
7. Považujeme zamrznuté grafické prostredie za zamrznutie celého operačného systému
8. Pri čiernej obrazovke Windows okamžite vypneme PC namiesto resetu grafického ovládača
9. Po obnovení systému neskontrolujeme logy a nezistíme skutočnú príčinu problému
10. V produkčnom prostredí testujeme príkazy, ktoré sme si predtým neoverili v laboratóriu

[Späť na obsah](#obsah)

<a id="bezpecnostne-zasady"></a>
## 9. Bezpečnostné zásady

1. Pred ukončením procesu zistíme jeho názov, PID, vlastníka a účel
2. Systémové procesy neukončujeme bez znalosti ich funkcie
3. Vynútené ukončenie používame až po zlyhaní štandardného postupu
4. Pri Linuxe uprednostníme `SIGTERM` pred `SIGKILL`
5. Pri PowerShelli využijeme `-Confirm` alebo `-WhatIf`, keď je to možné
6. Pred reštartovaním služby skontrolujeme jej závislosti a dopad
7. Pred odhlásením RDP používateľa ho upozorníme
8. Na produkčných serveroch preferujeme vzdialenú diagnostiku pred tvrdým reštartom
9. Po incidente skontrolujeme systémové logy a zaznamenáme príčinu
10. Tvrdé vypnutie napájania používame iba ako poslednú možnosť, keď nefunguje lokálna ani vzdialená správa

[Späť na obsah](#obsah)

<a id="zdroje"></a>
## 10. Oficiálna dokumentácia a zdroje

1. [VITA Academy – Online kurz Administrátor a Správca IT](https://www.vita.sk/online-kurz-administrator-a-spravca-it/)
2. [VITA Academy – Online kurz Microsoft Windows III. Pokročilý](https://www.vita.sk/online-kurz-microsoft-windows-iii-pokrocily/)
3. [VITA Academy – Online kurz Microsoft Windows Server I. Začiatočník](https://www.vita.sk/online-kurz-microsoft-windows-server-i-zaciatocnik/)
4. [VITA Academy – Online kurz Linux Administrátor I. Začiatočník](https://www.vita.sk/online-kurz-linux-administrator-linux-admin-i-zaciatocnik/)
5. [VITA Academy – Online kurz PowerShell I. Začiatočník](https://www.vita.sk/online-kurz-powershell-i-zaciatocnik/)
6. [Microsoft Learn – tasklist](https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/tasklist)
7. [Microsoft Learn – taskkill](https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/taskkill)
8. [Microsoft Learn – query session](https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/query-session)
9. [Microsoft Learn – query user](https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/query-user)
10. [Microsoft Learn – logoff](https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/logoff)
11. [Microsoft Learn – Get-Process](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.management/get-process?view=powershell-7.5)
12. [Microsoft Learn – Stop-Process](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.management/stop-process?view=powershell-7.5)
13. [Microsoft Learn – Restart-Service](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.management/restart-service)
14. [Microsoft Learn – Get-WinEvent](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.diagnostics/get-winevent?view=powershell-7.5)
15. [Microsoft Support – Troubleshooting blank screens in Windows](https://support.microsoft.com/en-us/windows/hardware/display-graphics/troubleshooting-blank-screens-in-windows)
16. [Linux Kernel Documentation – Magic SysRq](https://docs.kernel.org/admin-guide/sysrq.html)

[Späť na obsah](#obsah)
