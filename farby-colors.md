# 🎨 Farby a zvýrazňovanie textu v termináli Windows a Linux

Praktická príručka na nastavenie farieb v **Microsoft Windows CMD, Windows Terminal, PowerShell, Ubuntu Linux a Kali Linux**. Naučíme sa meniť farbu textu a pozadia, používať farebné schémy, zvýrazňovať syntax príkazov, nastavovať prompt a farebne odlišovať informačné, varovné a chybové výstupy.

Dokument je pripravený na precvičovanie príkazov v termináli a na praktické demonštrácie vo videu. Príklady označené ako `cmd`, `powershell`, `bash` alebo `zsh` spúšťame **v zodpovedajúcom prostredí**.

> **Dôležité:** Terminálová aplikácia (napríklad Windows Terminal, GNOME Terminal alebo Xfce Terminal), shell (CMD, PowerShell, Bash, Zsh) a samotný vypisovaný program sú tri rozdielne vrstvy. Každá môže ovplyvňovať farby iným spôsobom.

## 📑 Obsah

1. [Ako fungujú farby v termináli](#ako-funguju-farby)
2. [Microsoft Windows CMD – príkaz color](#windows-cmd)
3. [Farebná tabuľka CMD – 16 farieb](#cmd-farebna-tabulka)
4. [Windows Terminal – farebné schémy, kurzor a výber](#windows-terminal)
5. [PowerShell – farebný výstup Write-Host](#powershell-write-host)
6. [PowerShell – zvýrazňovanie syntaxe PSReadLine](#powershell-psreadline)
7. [PowerShell 7 – štýly a RGB pomocou PSStyle](#powershell-psstyle)
8. [Linux – ANSI escape sekvencie](#linux-ansi)
9. [Linux – bold, underline, reverse, 256 farieb a RGB](#linux-styly)
10. [Linux – tput a terminfo](#linux-tput)
11. [Ubuntu a Kali – grafické nastavenia terminálu](#linux-graficke)
12. [Ubuntu Bash – farebný prompt](#bash-prompt)
13. [Kali Zsh – farebný prompt](#zsh-prompt)
14. [Farebné zvýrazňovanie ls, grep a logov](#zvyraznovanie)
15. [Praktické cvičenia](#cvicenia)
16. [Cheat Sheet](#cheat-sheet)
17. [Časté chyby a ich riešenie](#caste-chyby)
18. [Bezpečnostné poznámky](#bezpecnost)
19. [Užitočné odkazy a zdroje](#zdroje)

<a id="ako-funguju-farby"></a>
## 🧠 Ako fungujú farby v termináli

Farby pomáhajú rozlišovať **príkazy, argumenty, cesty, súbory, varovania, chyby a úspešné operácie**. V prvom rade musíme vedieť, ktorá časť prostredia konkrétnu farbu nastavuje.

| Vrstva | Čo nastavuje | Typické nástroje |
|---|---|---|
| Terminálová aplikácia | pozadie, predvolený text, paletu, kurzor, označený text | Windows Terminal, GNOME Terminal, Xfce Terminal |
| Shell / editor príkazového riadku | prompt, farbu zadávaných príkazov, parametrov a reťazcov | PowerShell + PSReadLine, Bash, Zsh |
| Spustený príkaz / skript | farebné hlásenia a zvýraznenie vlastného výstupu | `color`, `Write-Host`, `printf`, `grep --color`, `ls --color` |

**Základné pojmy:**

| Pojem | Vysvetlenie |
|---|---|
| Foreground | farba textu (popredie) |
| Background | farba pozadia |
| Cursor | kurzor označujúci pozíciu, kde píšeme |
| Selection | vybratý / označený text, jeho podklad a kontrast |
| Prompt | výzva na zadávanie príkazov, napríklad `C:\>` alebo `user@host:~$` |
| ANSI escape | riadiaca sekvencia na nastavenie farby alebo textového efektu |
| SGR | Select Graphic Rendition – ANSI parametre, napríklad `31` (červená) alebo `1` (tučné) |
| HEX | hexadecimálny zápis farby, napríklad `#FF0000` |
| RGB | červená, zelená a modrá zložka farby |
| Color scheme | súprava predvolených farieb a farebnej palety |

**Pozor:** Zmena farieb okna nemení automaticky farby syntaxe pri písaní príkazov. Napríklad **PSReadLine** sa nastavuje nezávisle od farebnej schémy **Windows Terminalu**.

<a id="windows-cmd"></a>
## 🪟 Microsoft Windows CMD – príkaz color

Príkaz `color` je vstavaný príkaz **Windows Command Prompt (cmd.exe)**. Mení farbu popredia a pozadia aktuálnej konzolovej relácie. Nepredstavuje hackerský nástroj a nijako nemení oprávnenia systému.

**Základný tvar:**

```cmd
color [BG][FG]
```

| Časť | Význam |
|---|---|
| `color` | spustí vstavaný príkaz CMD |
| `BG` | prvá hexadecimálna číslica – farba pozadia (background) |
| `FG` | druhá hexadecimálna číslica – farba textu (foreground) |
| `color A` | zvolí svetlozelený text a predvolené pozadie relácie |
| `color 0A` | čierne pozadie a svetlozelený text |
| `color` | bez argumentu obnoví predvolené farby konzoly |
| `color /?` | vypíše nápovedu príkazu |

> Pri zápise **jednej** číslice `color A` sa obnovuje **predvolená farba pozadia**, ktorá nemusí byť čierna. Ak chceme výslovne čierne pozadie, zadáme dve číslice, napríklad `color 0A`.

**1. Zobrazíme nápovedu:**

```cmd
color /?
```

**2. Zmeníme text na svetlozelený:**

```cmd
color a
```

**3. Nastavíme čierne pozadie a svetlozelený text – efekt Matrix:**

```cmd
color 0a
```

**4. Kombinujeme modré pozadie a jasnobiely text:**

```cmd
color 1f
```

**5. Kombinujeme červené pozadie a svetložltý text:**

```cmd
color 4e
```

**6. Obnovíme predvolené farby:**

```cmd
color
```

**7. Explicitne nastavíme klasický čierny podklad a sivobiely text:**

```cmd
color 07
```

**Vysvetlenie:** Príkaz `color` mení atribúty konzoly, zatiaľ čo nasledujúce príkazy, napríklad `dir`, už vypisujú text s aktuálne nastavenými farbami. Výsledné odtiene sú závislé od farebnej palety hostiteľskej terminálovej aplikácie.

Praktická ukážka:

```cmd
color 0a
dir
color
```

Často šírený príklad `dir /s` vypisuje rekurzívne všetky súbory a priečinky pod aktuálnou cestou; pri veľkej štruktúre vytvára veľmi dlhý výstup. Na školenie je vhodnejší jednoduchý `dir`.

<a id="cmd-farebna-tabulka"></a>
## 🎨 Farebná tabuľka CMD – 16 farieb

CMD používa 16 farebných indexov zapísaných číslicami `0–9` a písmenami `A–F`. Písmená môžeme zapisovať malými aj veľkými znakmi.

| Kód | Farba | Kód | Farba |
|---|---|---|---|
| `0` | čierna | `8` | sivá |
| `1` | tmavomodrá | `9` | svetlomodrá |
| `2` | tmavozelená | `A` | svetlozelená |
| `3` | tmavoazúrová | `B` | svetloazúrová |
| `4` | tmavočervená | `C` | svetločervená |
| `5` | tmavofialová | `D` | svetlofialová |
| `6` | tmavožltá / hnedá | `E` | svetložltá |
| `7` | svetlosivá / biela | `F` | jasnobiela |

**Obľúbené kombinácie:**

| Príkaz | Pozadie | Text | Použitie |
|---|---|---|---|
| `color 0a` | čierne | svetlozelený | systémové demo, Matrix |
| `color 0b` | čierne | svetloazúrový | diagnostika |
| `color 0e` | čierne | svetložltý | upozornenia |
| `color 0f` | čierne | jasnobiely | vysoký kontrast |
| `color 1f` | modré | jasnobiely | prezentácia / výučba |
| `color 4f` | červené | jasnobiely | vizuálne upozornenie |
| `color 70` | svetlosivé | čierny | svetlý režim |
| `color 07` | čierne | svetlosivý | klasický vzhľad |

**Chybný príklad:**

```cmd
color 00
```

Ak sú obidva farebné indexy rovnaké, CMD zmenu odmietne a nastaví chybový `ERRORLEVEL`. Nemá zmysel cielene nastaviť rovnakú farbu textu a pozadia.

**Grafické nastavenie klasického okna CMD:** Otvoríme okno `cmd.exe`, klikneme pravým tlačidlom na titulok okna a vyberieme `Properties / Vlastnosti` alebo `Defaults / Predvolené nastavenia`. Dostupné karty a spôsob ukladania závisia od toho, či CMD prevádzkujeme v klasickom Console Host alebo vo Windows Terminali.

<a id="windows-terminal"></a>
## 🖥️ Windows Terminal – farebné schémy, kurzor a výber

**Windows Terminal** je aplikácia na zobrazovanie terminálových relácií. V samostatných kartách môže hostiť CMD, PowerShell aj distribúcie Linuxu cez WSL. Farby možno nastaviť osobitne pre každý profil.

**Postup cez grafické rozhranie:**

1. Otvoríme **Windows Terminal**.
2. Klikneme na šípku `▼` pri kartách a zvolíme **Settings / Nastavenia**.
3. Vyberieme konkrétny profil (napríklad **Command Prompt**, **PowerShell** alebo **Ubuntu**).
4. V časti **Appearance / Vzhľad** nastavíme **Color scheme / Farebnú schému**.
5. Zvolíme aj farbu kurzora, vzhľad písma, podklad a ďalšie dostupné vizuálne nastavenia.
6. Uložíme zmeny a porovnáme vzhľad jednotlivých kariet.

Dostupné schémy zahŕňajú napríklad **Campbell**, **One Half Dark**, **One Half Light**, **Tango Dark** a **Tango Light**. Zoznam sa môže líšiť podľa verzie a vlastných schém.

### 1. Nastavenia profilu v settings.json

Vo Windows Terminal otvoríme **Settings → Open JSON file**. Nižšie je **výrez obsahu jedného profilu**; nejde o kompletnú náhradu celého súboru `settings.json`.

```json
{
  "name": "Command Prompt",
  "commandline": "cmd.exe",
  "colorScheme": "One Half Dark",
  "foreground": "#E5E7EB",
  "background": "#111827",
  "cursorColor": "#FACC15",
  "selectionBackground": "#334155",
  "tabColor": "#2563EB"
}
```

Pri úprave existujúceho profilu zachováme jeho pôvodné identifikačné položky, napríklad `guid`. Vyššie uvedený objekt je vzor na vysvetlenie volieb, nie návod na prepísanie existujúceho profilu.

| Vlastnosť | Význam |
|---|---|
| `name` | názov profilu |
| `commandline` | spúšťaný shell alebo program |
| `colorScheme` | názov farebnej schémy |
| `foreground` | základná farba textu |
| `background` | farba pozadia |
| `cursorColor` | farba kurzora |
| `selectionBackground` | farba pozadia označeného textu |
| `tabColor` | farba karty terminálu |

### 2. Nastavenie pre všetky profily

Ak chceme používať rovnakú farebnú schému vo viacerých profiloch, môžeme do **existujúceho** objektu `profiles.defaults` pridať napríklad:

```json
"defaults": {
  "colorScheme": "One Half Dark",
  "cursorColor": "#FACC15",
  "selectionBackground": "#334155"
}
```

**Neodstraňujeme obsah `profiles.list`.** Úprava `settings.json` musí zachovať platnú štruktúru existujúceho súboru. Individuálne nastavenia profilu môžu prepísať predvolené nastavenia.

**Rozdiel:** `foreground` určuje základnú farbu textu, ale nemusí prepísať farby, ktoré samotný program zámerne nastaví cez ANSI sekvencie.

<a id="powershell-write-host"></a>
## 🔵 PowerShell – farebný výstup pomocou Write-Host

V PowerShelli nepoužívame vstavaný CMD príkaz `color` ako univerzálny spôsob formátovania. Na jednotlivé farebné hlásenia je vhodný cmdlet `Write-Host`.

**Základná syntax:**

```powershell
Write-Host "Text" -ForegroundColor Green -BackgroundColor Black
```

| Parameter | Význam |
|---|---|
| `Write-Host` | vypíše správu v terminálovom hostiteľovi |
| `-ForegroundColor` | nastaví farbu písma |
| `-BackgroundColor` | nastaví farbu pozadia písma |
| `-NoNewline` | po výpise neprechádza automaticky na nový riadok |
| `-Separator` | určí oddeľovač medzi vypisovanými objektmi |

**1. Farebné informácie a stavové správy:**

```powershell
Write-Host "INFO: Služba bola spustená" -ForegroundColor Cyan
Write-Host "OK: Operácia sa dokončila" -ForegroundColor Green
Write-Host "WARNING: Blíži sa limit disku" -ForegroundColor Yellow
Write-Host "ERROR: Pripojenie zlyhalo" -ForegroundColor White -BackgroundColor DarkRed
```

**2. Formátovanie bez nového riadku:**

```powershell
Write-Host "Stav servera: " -NoNewline
Write-Host "ONLINE" -ForegroundColor Green
```

**3. Zobrazenie farebných kombinácií:**

```powershell
[Enum]::GetNames([ConsoleColor]) | ForEach-Object {
    Write-Host $_ -ForegroundColor $_
}
```

Príkaz zobrazí dostupné hodnoty enumerácie `ConsoleColor` v ich vlastnej farbe. Čitateľnosť niektorých kombinácií závisí od pozadia.

**Poznámka k automatizácii:** `Write-Host` je určený najmä pre správy zobrazované používateľovi. Ak chceme ďalej spracúvať dáta v PowerShell pipeline, uprednostníme objektový výstup, napríklad `Write-Output`.

<a id="powershell-psreadline"></a>
## ⌨️ PowerShell – zvýrazňovanie syntaxe pomocou PSReadLine

**PSReadLine** zabezpečuje interaktívne editovanie príkazového riadku, históriu príkazov a farebné zvýrazňovanie syntaxe. Farby `Command`, `Parameter` alebo `String` teda **nie sú farbami celej aplikácie Windows Terminal**.

**1. Skontrolujeme aktuálne nastavenia:**

```powershell
Get-PSReadLineOption
```

**2. Zmeníme farby jednotlivých prvkov syntaxe:**

```powershell
Set-PSReadLineOption -Colors @{
    Command   = 'Cyan'
    Parameter = 'Yellow'
    String    = 'Green'
    Error     = 'Red'
    Variable  = 'Magenta'
    Number    = 'White'
    Keyword   = 'Blue'
}
```

**3. Vyskúšame interaktívne písanie nasledujúceho príkazu:**

```powershell
Get-Process -Name "explorer" | Select-Object Name, Id
```

Počas písania sledujeme, ako PSReadLine zobrazuje farby názvu príkazu, parametra, reťazca a ostatných tokenov.

| Kľúč `-Colors` | Čo zvýrazňuje |
|---|---|
| `Command` | názvy príkazov a cmdletov |
| `Parameter` | názvy parametrov, napríklad `-Name` |
| `String` | textové reťazce |
| `Variable` | premenné, napríklad `$name` |
| `Number` | číselné literály |
| `Keyword` | kľúčové slová jazyka |
| `Error` | syntakticky problémové časti vstupu |
| `Comment` | komentáre |
| `Operator` | operátory |
| `Selection` | výber pri editovaní vstupu, nie všeobecne myšou označený text v termináli |
| `Emphasis` | zvýraznenie v rámci interaktívnych funkcií PSReadLine |

**4. Trvalé nastavenie:** Ak chceme farby načítať pri každom spustení PowerShellu, vložíme `Set-PSReadLineOption ...` do svojho PowerShell profilu. Profil otvoríme takto:

```powershell
$PROFILE
```

```powershell
if (!(Test-Path -LiteralPath $PROFILE)) {
    New-Item -ItemType File -Path $PROFILE -Force | Out-Null
}
notepad $PROFILE
```

Najprv si vytvoríme zálohu existujúceho profilu. Nastavenia z `Set-PSReadLineOption` bez zápisu do profilu platia pre aktuálnu reláciu. Ich zrušenie je jednoduché otvorením novej relácie, pokiaľ zmenu nemáme uloženú v profile.

<a id="powershell-psstyle"></a>
## 🌈 PowerShell 7 – štýly a RGB pomocou PSStyle

Od **PowerShellu 7.2** máme k dispozícii premennú `$PSStyle`, ktorá uľahčuje používanie ANSI formátovania. **Windows PowerShell 5.1** túto premennú neposkytuje.

Overíme verziu:

```powershell
$PSVersionTable.PSVersion
```

**1. Tučný zelený text:**

```powershell
"$($PSStyle.Bold)$($PSStyle.Foreground.Green)USPECH$($PSStyle.Reset)"
```

**2. Červené upozornenie s obrátenými farbami:**

```powershell
"$($PSStyle.Reverse)$($PSStyle.Foreground.Red)UPOZORNENIE$($PSStyle.Reset)"
```

**3. Vlastná RGB farba:**

```powershell
"$($PSStyle.Foreground.FromRgb(255,165,0))ORANZOVY TEXT$($PSStyle.Reset)"
```

| Prvok | Význam |
|---|---|
| `$PSStyle.Foreground.Green` | zelená farba textu |
| `$PSStyle.Foreground.FromRgb(255,165,0)` | vlastná RGB farba |
| `$PSStyle.Bold` | tučné / intenzívne zvýraznenie |
| `$PSStyle.Underline` | podčiarknutie |
| `$PSStyle.Reverse` | zámena farieb popredia a pozadia |
| `$PSStyle.Reset` | odstránenie aktívnych textových efektov |

**Pozor:** Nie každý terminál alebo font zobrazuje všetky efekty rovnako. Pri presmerovaní výstupu do súboru tiež zohľadníme správanie ANSI formátovania a nastavenie `$PSStyle.OutputRendering`.

<a id="linux-ansi"></a>
## 🐧 Linux – ANSI escape sekvencie

V Ubuntu a Kali Linuxe môžeme farebný výstup vytvoriť pomocou ANSI riadiacich sekvencií, napríklad cez shellový príkaz `printf`.

**Základný tvar:**

```bash
printf '\033[31mCERVENY TEXT\033[0m\n'
```

| Prvok | Význam |
|---|---|
| `printf` | vytvára formátovaný výstup |
| `\033` | znak ESC (escape), oktálovo 033 |
| `[31m` | nastaví červené popredie cez SGR |
| `\033[0m` | resetuje aktívne textové štýly |
| `\n` | ukončí výpis novým riadkom |

**1. Výpis základných farieb:**

```bash
printf '\033[30mCierny text\033[0m\n'
printf '\033[31mCerveny text\033[0m\n'
printf '\033[32mZeleny text\033[0m\n'
printf '\033[33mZlty text\033[0m\n'
printf '\033[34mModry text\033[0m\n'
printf '\033[35mFialovy text\033[0m\n'
printf '\033[36mAzurovy text\033[0m\n'
printf '\033[37mBiely text\033[0m\n'
```

**2. Farba textu a pozadia súčasne:**

```bash
printf '\033[37;41mBiely text na cervenom pozadi\033[0m\n'
```

**3. Zobrazenie všetkých štandardných farebných indexov 30–37:**

```bash
for kod in 30 31 32 33 34 35 36 37; do
    printf '\033[%smKod %s - vzorka textu\033[0m\n' "$kod" "$kod"
done
```

**Prečo používame `printf` a nie vždy `echo -e`:** Správanie `echo` sa medzi shellmi líši; `printf` je predvídateľnejší pre prácu s riadiacimi sekvenciami.

<a id="linux-styly"></a>
## ✨ Linux – bold, underline, reverse, 256 farieb a RGB

### 1. Textové efekty

```bash
printf '\033[1mTucny text\033[0m\n'
printf '\033[4mPodciarknuty text\033[0m\n'
printf '\033[7mObratene farby\033[0m\n'
printf '\033[1;32mTucny zeleny text\033[0m\n'
```

| SGR kód | Význam |
|---|---|
| `0` | reset všetkých aktuálnych efektov |
| `1` | bold / zvýšená intenzita |
| `2` | znížená intenzita |
| `3` | kurzíva, ak ich terminál podporuje |
| `4` | podčiarknutie |
| `7` | obrátenie farieb popredia a pozadia |
| `9` | prečiarknutie, ak ho terminál podporuje |
| `22` | vypnutie zvýšenej a zníženej intenzity |
| `24` | vypnutie podčiarknutia |
| `27` | vypnutie obrátenia farieb |
| `30–37` | základných 8 farieb textu |
| `40–47` | základných 8 farieb pozadia |
| `90–97` | jasné farby textu |
| `100–107` | jasné farby pozadia |

### 2. Paleta 256 farieb

```bash
printf '\033[38;5;208mOranzovy text - index 208\033[0m\n'
printf '\033[48;5;21mPozadie - index 21\033[0m\n'
```

**Vysvetlenie:** `38;5;N` nastavuje popredie a `48;5;N` pozadie, kde `N` je index palety od 0 do 255.

### 3. RGB / True Color

```bash
printf '\033[38;2;255;165;0mOranzovy text RGB\033[0m\n'
printf '\033[48;2;30;64;175mModre pozadie RGB\033[0m\n'
```

**Vysvetlenie:** `38;2;R;G;B` nastavuje RGB popredie a `48;2;R;G;B` RGB pozadie. V tomto príklade používame červenú, zelenú a modrú zložku v rozsahu 0–255.

### 4. Reset aktuálneho formátovania

```bash
printf '\033[0m\n'
```

Reset SGR obnoví predvolené textové efekty, nemusí však resetovať všetky funkcie terminálovej aplikácie. Pri vážnejšie narušenom termináli možno použiť `reset`, ktorý vykonáva širšiu reinicializáciu podľa prostredia.

<a id="linux-tput"></a>
## 🧰 Linux – tput a terminfo

Príkaz `tput` pracuje s databázou **terminfo**, v ktorej sú opísané schopnosti daného terminálu. Je alternatívou k ručnému zapisovaniu ANSI sekvencií.

**1. Zistíme typ terminálu:**

```bash
printf 'TERM=%s\n' "$TERM"
```

**2. Zobrazíme zelený text:**

```bash
tput setaf 2
printf 'Zeleny text\n'
tput sgr0
```

**3. Tučný a podčiarknutý text:**

```bash
tput bold
printf 'Tucny text\n'
tput sgr0

tput smul
printf 'Podciarknuty text\n'
tput rmul
```

| Príkaz | Význam |
|---|---|
| `tput setaf 2` | farba textu s indexom 2, typicky zelená |
| `tput setab 4` | farba pozadia s indexom 4, typicky modrá |
| `tput bold` | zvýraznenie textu |
| `tput smul` | zapne podčiarknutie |
| `tput rmul` | vypne podčiarknutie |
| `tput sgr0` | resetuje textové atribúty |
| `tput colors` | zobrazí počet farieb deklarovaný v terminfo |

Pri nesprávnej hodnote `TERM` alebo chýbajúcom zázname terminfo nemusí `tput` fungovať podľa očakávania.

<a id="linux-graficke"></a>
## 🖱️ Ubuntu a Kali – grafické nastavenia terminálu

### Ubuntu – GNOME Terminal

Ak máme nainštalovaný **GNOME Terminal**, postupujeme spravidla cez **Preferences / Predvoľby → vybraný profil → Colors / Farby**. Nastavíme predvolenú farbu textu, pozadia, paletu alebo použitie farieb systémovej témy.

Nie každá verzia Ubuntu používa rovnakú terminálovú aplikáciu. V prostredí **GNOME Console**, **Ptyxis** alebo inom emulátore môže mať nastavenie odlišné umiestnenie alebo rozsah funkcií.

### Kali Linux – Xfce Terminal

Pri Kali s prostredím Xfce otvoríme **Xfce Terminal → Edit → Preferences → Colors**. Podľa verzie nastavíme napríklad:

- farbu popredia a pozadia,
- farebnú paletu,
- zvýraznený / bold text,
- farbu kurzora a výberu textu,
- použitie systémovej témy.

**Podstatný rozdiel:** Grafické nastavenie farby textu nemení syntaktické zvýrazňovanie Bash/Zsh príkazov. Na to potrebujeme konfiguráciu shellu alebo doplnok na zvýrazňovanie syntaxe.

<a id="bash-prompt"></a>
## 🟢 Ubuntu Bash – farebný prompt cez PS1

**Prompt** je text, ktorý Bash zobrazí pred miestom na zadanie príkazu. Premenná `PS1` definuje jeho základnú podobu.

**1. Zistíme aktuálny shell:**

```bash
ps -p $$ -o comm=
```

**2. Zobrazíme aktuálnu definíciu promptu:**

```bash
printf '%s\n' "$PS1"
```

**3. Uložíme aktuálny prompt a dočasne nastavíme farby:**

```bash
old_ps1=$PS1
PS1='\[\e[1;32m\]\u@\h\[\e[0m\]:\[\e[1;34m\]\w\[\e[0m\]\$ '
```

**4. Obnovíme pôvodný prompt:**

```bash
PS1=$old_ps1
```

| Sekvencia | Význam |
|---|---|
| `PS1` | premenná so základným promptom Bash |
| `\u` | používateľské meno |
| `\h` | krátky názov počítača |
| `\w` | aktuálny adresár |
| `\$` | `$` pre bežného používateľa, `#` pri efektívnom UID 0 |
| `\e[1;32m` | tučný zelený text |
| `\e[1;34m` | tučný modrý text |
| `\e[0m` | reset atribútov |
| `\[` a `\]` | ohraničenie netlačiteľnej sekvencie, aby sa prompt správne zalamoval |

**Trvalé nastavenie:** Ak používame interaktívny Bash, konfiguráciu môžeme doplniť do `~/.bashrc`. Pred zmenou si vytvoríme zálohu:

```bash
cp ~/.bashrc ~/.bashrc.bak
nano ~/.bashrc
```

Novú hodnotu `PS1` vložíme do vhodnej časti konfigurácie tak, aby ju neskoršie pravidlá v súbore neprepísali. Návrat je možný obnovou zálohy alebo odstránením pridanej úpravy.

<a id="zsh-prompt"></a>
## 🐉 Kali Linux Zsh – farebný prompt

Kali môže mať predvolený interaktívny shell **Zsh**, no v konkrétnej inštalácii to vždy overíme. Syntax Zsh promptu nie je totožná so syntaxou Bash `PS1`.

**1. Overíme shell:**

```bash
ps -p $$ -o comm=
```

**2. V interaktívnom Zsh uložíme aktuálny prompt a zmeníme jeho farby:**

```zsh
old_prompt=$PROMPT
PROMPT='%F{green}%n@%m%f:%F{blue}%~%f %# '
```

**3. Obnovíme pôvodný prompt:**

```zsh
PROMPT=$old_prompt
```

| Prvok Zsh | Význam |
|---|---|
| `PROMPT` | text hlavného promptu |
| `%F{green}` | začne zelený text |
| `%F{blue}` | začne modrý text |
| `%f` | vráti predvolenú farbu popredia |
| `%n` | používateľské meno |
| `%m` | skrátený hostname |
| `%~` | aktuálna cesta, s vlnovkou pri domovskom adresári |
| `%#` | znak `%` alebo `#` podľa oprávnení |

Trvalé nastavenia používateľského Zsh sa spravidla ukladajú do `~/.zshrc`. Kali môže mať vlastný viacriadkový prompt alebo ďalší framework; **pred úpravou si zálohujeme existujúcu konfiguráciu**:

```zsh
cp ~/.zshrc ~/.zshrc.bak
nano ~/.zshrc
```

**Zvýrazňovanie príkazov pri písaní:** V Zsh môže túto úlohu riešiť doplnok `zsh-syntax-highlighting` alebo iná konfigurácia. Samotná farebná schéma terminálu takýto doplnok nenahrádza.

<a id="zvyraznovanie"></a>
## 🔍 Farebné zvýrazňovanie výstupov ls, grep a logov

Farebné zvýrazňovanie nie je obmedzené na vzhľad terminálu. Niektoré nástroje si generujú ANSI farby priamo pri vypisovaní výsledkov.

**1. Vytvoríme jednoduchý testovací log:**

```bash
printf 'INFO Server bezi\nERROR Problem\nWARNING Disk\nERROR Timeout\n' > demo.log
```

**2. Zvýrazníme nájdené chyby:**

```bash
grep --color=auto 'ERROR' demo.log
```

**3. Pridáme čísla riadkov:**

```bash
grep -n --color=auto 'ERROR' demo.log
```

**4. Upravíme farbu zhody pomocou GREP_COLORS:**

```bash
export GREP_COLORS='ms=01;33:fn=35:ln=32'
grep -n --color=auto 'ERROR' demo.log
```

Tu `ms=01;33` nastavuje tučnú žltú pre nájdený text, `fn=35` fialovú pre názvy súborov a `ln=32` zelenú pre čísla riadkov.

**5. Farebne odlíšime súbory a priečinky:**

```bash
ls --color=auto
ls -l --color=auto
```

**6. Skontrolujeme nastavenie farieb pre ls:**

```bash
printf '%s\n' "$LS_COLORS"
dircolors --print-database
```

`LS_COLORS` určuje farby rôznych kategórií súborov (napríklad adresáre, symbolické odkazy alebo spustiteľné súbory). `dircolors` pripravuje konfiguráciu tejto premennej.

| Prvok | Význam |
|---|---|
| `--color=auto` | farby používame spravidla iba pri výstupe do terminálu |
| `--color=always` | farebné sekvencie vynútime aj pri presmerovaní |
| `--color=never` | farby vypneme |
| `grep -n` | vypíše čísla riadkov s nálezmi |
| `GREP_COLORS` | konfiguruje zvýrazňovanie zhôd a ďalších prvkov grep |
| `LS_COLORS` | určuje pravidlá farbenia typov súborov pre GNU ls |
| `dircolors` | generuje shellovú konfiguráciu palety pre GNU ls |

**Pozor pri pipeline:** Pri `--color=always` môžu ANSI escape sekvencie zostať aj v exportovaných dátach. Pri spracovaní textu ďalšími nástrojmi používame `auto` alebo `never`, ak farby nepotrebujeme.

<a id="cvicenia"></a>
## 🧪 Praktické cvičenia

Nasledujúce cvičenia realizujeme v zodpovedajúcom prostredí. Pri prvom prechode používame dočasné nastavenia a nemeníme konfiguračné súbory profilov.

**1. Windows CMD – svetlozelený text na čiernom pozadí**

```cmd
color 0a
echo Ahoj z Windows CMD
dir
color
```

Čo to robí: zmeníme farbu textu, zobrazíme informáciu a obsah aktuálneho priečinka, potom obnovíme predvolené farby.

**2. Windows CMD – porovnanie farebných kombinácií**

```cmd
color 1f
echo Modre pozadie a jasnobiely text
color 4e
echo Cervene pozadie a svetlozlty text
color
```

Čo to robí: porovnáme dvojice hexadecimálnych kódov a vrátime pôvodné nastavenie.

**3. Windows Terminal – samostatná schéma pre CMD**

Otvoriť `Settings → Profiles → Command Prompt → Appearance`, nastaviť schému **One Half Dark**, zmeniť kurzor a označenie textu. Následne otvoriť PowerShell a porovnať, či má iný profil inú schému.

Čo to robí: naučíme sa rozdiel medzi nastavením jednej karty a predvoleným nastavením celej aplikácie.

**4. PowerShell – stavové výpisy**

```powershell
Write-Host "INFO" -ForegroundColor Cyan
Write-Host "OK" -ForegroundColor Green
Write-Host "WARNING" -ForegroundColor Yellow
Write-Host "ERROR" -ForegroundColor White -BackgroundColor Red
```

Čo to robí: vytvoríme štyri farebne rozlíšené stavové správy.

**5. PowerShell – zvýrazňovanie príkazov**

```powershell
Set-PSReadLineOption -Colors @{
    Command   = 'Cyan'
    Parameter = 'Yellow'
    String    = 'Green'
}
```

Čo to robí: pri písaní príkazu skontrolujeme odlišné farby názvu cmdletu, parametra a textového argumentu.

**6. PowerShell 7 – RGB**

```powershell
"$($PSStyle.Foreground.FromRgb(255,165,0))Oranzovy text$($PSStyle.Reset)"
```

Čo to robí: nastavíme vlastnú RGB farbu textu a resetujeme štýl.

**7. Ubuntu / Kali – tri základné ANSI farby**

```bash
printf '\033[31mERROR\033[0m\n'
printf '\033[33mWARNING\033[0m\n'
printf '\033[32mOK\033[0m\n'
```

Čo to robí: pri každom výpise použijeme vlastnú farbu a reset. Farby sa neprenášajú na ďalšie príkazy.

**8. Ubuntu / Kali – tučný, podčiarknutý a obrátený text**

```bash
printf '\033[1mBOLD\033[0m\n'
printf '\033[4mUNDERLINE\033[0m\n'
printf '\033[7mREVERSE\033[0m\n'
```

Čo to robí: porovnáme tri textové atribúty, ktoré podporuje množstvo terminálov.

**9. Ubuntu / Kali – test práce s tput**

```bash
tput colors
tput setaf 2
printf 'ZELENY TEXT\n'
tput sgr0
```

Čo to robí: zistíme deklarovanú veľkosť palety terminálu a nastavíme zelené popredie pomocou terminfo.

**10. Ubuntu / Kali – farebné zvýraznenie chýb v logu**

```bash
printf 'INFO Start\nERROR Timeout\nWARNING CPU\nERROR Disk\n' > demo.log
grep -n --color=auto 'ERROR' demo.log
```

Čo to robí: zviditeľníme iba vyhľadané chyby a ich čísla riadkov.

<a id="cheat-sheet"></a>
## 🧾 Cheat Sheet – rýchly prehľad príkazov

### 🪟 Windows CMD – farby a pomoc

| Príkaz | Význam |
|---|---|
| `color /?` | nápoveda |
| `color a` | svetlozelený text, predvolené pozadie |
| `color 0a` | čierne pozadie, svetlozelený text |
| `color 0f` | čierne pozadie, jasnobiely text |
| `color 1f` | modré pozadie, jasnobiely text |
| `color 4e` | červené pozadie, svetložltý text |
| `color 07` | klasický čierny podklad a svetlosivý text |
| `color` | obnovenie predvolených farieb |

### 🔵 Windows PowerShell – farby a zvýraznenie

| Príkaz / voľba | Význam |
|---|---|
| `Write-Host "OK" -ForegroundColor Green` | zelené stavové hlásenie |
| `-BackgroundColor Red` | nastaví červené pozadie výpisu |
| `Get-PSReadLineOption` | zobrazí nastavenia editora riadku |
| `Set-PSReadLineOption -Colors @{...}` | zmení farby interaktívnej syntaxe |
| `$PSVersionTable.PSVersion` | zistí verziu PowerShellu |
| `$PSStyle.Bold` | zapne tučné formátovanie (PowerShell 7.2+) |
| `$PSStyle.Foreground.Green` | zelené popredie (PowerShell 7.2+) |
| `$PSStyle.Reset` | reset ANSI štýlov (PowerShell 7.2+) |

### 🐧 Linux – ANSI farby

| Kód | Význam |
|---|---|
| `\033[0m` | reset atribútov |
| `\033[1m` | tučný text |
| `\033[4m` | podčiarknutie |
| `\033[7m` | obrátené farby |
| `\033[31m` | červený text |
| `\033[32m` | zelený text |
| `\033[33m` | žltý text |
| `\033[34m` | modrý text |
| `\033[37;41m` | biely text na červenom pozadí |
| `\033[38;5;208m` | 256-farebná paleta, index 208 |
| `\033[38;2;255;165;0m` | oranžové RGB popredie |

### 🧰 Linux – terminál, shell a zvýrazňovanie

| Príkaz / položka | Význam |
|---|---|
| `tput setaf 2` | zelená farba textu |
| `tput sgr0` | reset atribútov |
| `tput colors` | počet farieb podľa terminfo |
| `ls --color=auto` | farebný výpis súborov |
| `grep -n --color=auto ERROR demo.log` | nájdený text so zvýraznením a číslami riadkov |
| `GREP_COLORS` | konfigurácia farieb grep |
| `LS_COLORS` | konfigurácia farieb GNU ls |
| `PS1` | hlavný Bash prompt |
| `PROMPT` | hlavný Zsh prompt |
| `~/.bashrc` | typická používateľská konfigurácia interaktívneho Bash |
| `~/.zshrc` | typická používateľská konfigurácia interaktívneho Zsh |

<a id="caste-chyby"></a>
## 🛠️ Časté chyby a ich riešenie

| Problém | Príčina | Riešenie |
|---|---|---|
| `color` nefunguje v PowerShelli | nejde o vstavaný PowerShell cmdlet na farby | použiť `Write-Host` alebo spustiť `cmd.exe` |
| `color 00` nefunguje | rovnaká farba popredia a pozadia | zvoliť rozdielne indexy, napríklad `0A` |
| `color a` nemá čierne pozadie | jeden index používa predvolené pozadie | použiť výslovne `color 0a` |
| Zmenil sa vzhľad okna, nie syntax príkazov | iná vrstva farebného nastavenia | v PowerShelli nastaviť `PSReadLine` |
| `$PSStyle` neexistuje | starší PowerShell (napr. 5.1) | použiť `Write-Host` alebo aktualizovať na podporovanú verziu PowerShellu |
| V Bash sú pri úprave promptu chyby zalamovania | netlačiteľné ANSI sekvencie nie sú ohraničené | použiť `\[...]` v definícii `PS1` |
| Zsh nereaguje na Bash sekvencie promptu | Zsh používa iný formát promptu | používať `%F{...}`, `%f`, `%n` atď. |
| Vo výpise vidíme `[31m` alebo iné kódy | terminál alebo prostredie neinterpretuje ANSI | overiť terminál a jeho podporu ANSI |
| Farby sa prenášajú do ďalších riadkov | chýba reset | na koniec pridať `\033[0m` alebo `tput sgr0` |
| `grep` vo výstupnom súbore obsahuje ANSI kódy | vynútené `--color=always` | pre export použiť `--color=never` |
| Svetlý text na svetlom pozadí je nečitateľný | nevhodný kontrast | zmeniť foreground/background alebo farebnú schému |
| Zmena po novom spustení zmizne | išlo len o nastavenie aktuálnej relácie | uložiť správnu konfiguráciu profilu alebo shellu |

<a id="bezpecnost"></a>
## 🔒 Bezpečnostné poznámky

- **Farby nie sú oprávnenia ani bezpečnostný stav.** Zelený text neznamená, že proces alebo súbor je bezpečný; iba vizuálne označuje výstup.
- **Najprv použijeme dočasné nastavenia.** Pred úpravou `settings.json`, `~/.bashrc`, `~/.zshrc` alebo PowerShell profilu vytvoríme zálohu.
- **Nezamieňame zobrazenie s dátami.** ANSI sekvencie môžu narušiť spracovanie logov, kopírovanie textu alebo textové exporty.
- **Nespúšťame nedôveryhodné skripty** iba preto, aby sme dosiahli vizuálny efekt terminálu. Príkaz `color a` nemá žiadne hackerské schopnosti.
- **Pri produkčných serveroch nemeníme konfiguráciu shellu bez testu.** Chybný prompt alebo konfigurácia môže zhoršiť interaktívnu správu.
- **Pri prihlasovaní cez SSH** zohľadňujeme rozdiel medzi lokálnou terminálovou aplikáciou a vzdialeným shellom.
- **Uprednostňujeme prístupnosť.** Dostatočný kontrast, odlíšenie aj iným spôsobom než farbou a neprehnané používanie efektov pomáhajú pri čítaní logov.
- **Rovnako farebné indexy nemusia vyzerať rovnako** vo Windows Terminali, Xfce Terminali, GNOME Terminali alebo inom emulátore, pretože majú rôzne palety a schémy.
- **Na zistenie shellu používame vhodnú diagnostiku.** `ps -p $$ -o comm=` ukazuje názov procesu aktuálneho shellu; premenná `$SHELL` môže označovať predvolený prihlasovací shell, nie nevyhnutne práve bežiaci.

<a id="zdroje"></a>
## 📚 Užitočné odkazy a zdroje

**Microsoft Windows a PowerShell**

1. [Microsoft Learn – Windows CMD color](https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/color)
2. [Microsoft Learn – Windows Terminal color schemes](https://learn.microsoft.com/en-us/windows/terminal/customize-settings/color-schemes)
3. [Microsoft Learn – Windows Terminal profile appearance](https://learn.microsoft.com/en-us/windows/terminal/customize-settings/profile-appearance)
4. [Microsoft Learn – Write-Host](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.utility/write-host)
5. [Microsoft Learn – Set-PSReadLineOption](https://learn.microsoft.com/en-us/powershell/module/psreadline/set-psreadlineoption)
6. [Microsoft Learn – about ANSI Terminals a PSStyle](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_ansi_terminals)

**GNU/Linux, Bash, grep a coreutils**

7. [GNU Bash Manual – Controlling the Prompt](https://www.gnu.org/software/bash/manual/html_node/Controlling-the-Prompt.html)
8. [GNU grep Manual – General Output Control](https://www.gnu.org/software/grep/manual/html_node/General-Output-Control.html)
9. [GNU Coreutils Manual – dircolors](https://www.gnu.org/software/coreutils/manual/html_node/dircolors-invocation.html)
10. [Xfce Terminal – dokumentácia](https://docs.xfce.org/apps/xfce4-terminal/start)
11. [Zsh Documentation – Prompt Expansion](https://zsh.sourceforge.io/Doc/Release/Prompt-Expansion.html)

**Súvisiace vzdelávanie a administrácia**

- [VITA Academy – Online kurz Administrátor a Správca IT](https://www.vita.sk/online-kurz-administrator-a-spravca-it/)
- [VITA Academy – Online kurz Linux Administrátor](https://www.vita.sk/online-kurz-linux-administrator-linux-admin-i-zaciatocnik/)
- [VITA Academy – Online kurz PowerShell](https://www.vita.sk/online-kurz-powershell-i-zaciatocnik/)
- [admin-lab – hlavná dokumentácia](README.md)

**Cieľ cvičení:** Rozlišovať nastavenia terminálovej aplikácie, shellu a formátovania výstupov; vedieť použiť základné farby, zvýrazňovanie syntaxe a bezpečné obnovenie pôvodného vzhľadu.
