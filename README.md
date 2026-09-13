# 🧪 Online Kurzy Administrátor, Windows, Windows Server, Linux a PowerShell (admin-lab)

Praktický repozitár na precvičovanie **administrácie IT systémov, Microsoft Windows, Microsoft Windows Server, Linuxu a PowerShellu**. Obsah je zameraný na príkazy, diagnostiku, správu procesov, služieb, sietí, portov, databáz, automatizáciu a riešenie reálnych administrátorských úloh.

Repozitár nadväzuje na rôzne online kurzov VITA Academy:

1. [Online kurz Administrátor a Správca IT](https://www.vita.sk/online-kurz-administrator-a-spravca-it/)
2. [Online kurz Microsoft Windows](https://www.vita.sk/online-kurz-microsoft-windows-iii-pokrocily/)
3. [Online kurz Microsoft Windows Server](https://www.vita.sk/online-kurz-microsoft-windows-server-i-zaciatocnik/)
4. [Online kurz Linux Administrátor (Linux Admin)](https://www.vita.sk/online-kurz-linux-administrator-linux-admin-i-zaciatocnik/)
5. [Online kurz PowerShell](https://www.vita.sk/online-kurz-powershell-i-zaciatocnik/)


## 📑 Obsah

1. [Administrátor a Správca IT](#administrator-a-spravca-it)
   1. [Porty a sieťová komunikácia](#admin-01)
   2. [Linux terminál, shell a WSL](#admin-02)
   3. [IP adresy, DHCP a DNS](#admin-03)
   4. [CMD, Systeminfo, Netstat, Netsh a Sysinternals](#admin-04)
   5. [Databázy, MySQL a DBeaver](#admin-05)
   6. [Zálohovanie databáz a ODBC](#admin-06)
   7. [ITIL, monitoring a dostupnosť služieb](#admin-07)
   8. [Procesy, súbory, cURL a jq](#admin-08)
   9. [Prevádzková bezpečnosť a riziká](#admin-09)
   10. [Docker a Jenkins](#admin-10)
2. [Microsoft Windows III. Pokročilý](#windows-iii-pokrocily)
   1. [Príkazový riadok, zariadenia a Firewall](#windows-01)
   2. [Služby, Autoruns a ovládače](#windows-02)
   3. [Indexovanie a Windows Search](#windows-03)
   4. [Diskový priestor a Robocopy](#windows-04)
   5. [God Mode a systémové nástroje](#windows-05)
   6. [Hosts File a sieťové porty](#windows-06)
3. [Microsoft Windows Server I. Začiatočník](#windows-server-i-zaciatocnik)
   1. [Serverové roly, DNS a DHCP](#server-01)
   2. [Hosts File](#server-02)
   3. [Služby, Remote Access a terminál](#server-03)
   4. [Procesy a porty](#server-04)
   5. [Monitoring a Wireshark](#server-05)
4. [Linux Administrátor I. Začiatočník](#linux-administrator-i-zaciatocnik)
   1. [Aplikácie a softvérové balíky](#linux-01)
   2. [Klávesové skratky a Midnight Commander](#linux-02)
   3. [Procesy a systémové prostriedky](#linux-03)
   4. [Porty, lsof, ss a netstat](#linux-04)
   5. [AWK, MAWK a GAWK](#linux-05)
   6. [SED](#linux-06)
   7. [PostgreSQL a DBeaver](#linux-07)
   8. [Docker v Linuxe](#linux-08)
5. [PowerShell I. Začiatočník](#powershell-i-zaciatocnik)
   1. [PowerShell, Terminal a Shell](#powershell-01)
   2. [Súbory, priečinky a kopírovanie](#powershell-02)
   3. [Aplikácie, adaptéry a ovládače](#powershell-03)
   4. [Get príkazy, dátum, čas a rúry](#powershell-04)
   5. [Skripty, cykly a dátové typy](#powershell-05)
   6. [MAC adresa a IP adresa](#powershell-06)
6. [Praktické dokumenty repozitára](#prakticke-dokumenty)

<a id="prakticke-dokumenty"></a>
## 🗂️ Praktické dokumenty repozitára

```text
admin-lab/
├── README.md
└── zazmrnutie-os-pc-server.md
```

1. [`zazmrnutie-os-pc-server.md`](zazmrnutie-os-pc-server.md) – čo robiť, ak Microsoft Windows, Windows Server alebo Linux zamrzne bez okamžitého reštartovania

<a id="administrator-a-spravca-it"></a>
# 🔵 Administrátor a Správca IT

Kurz prepája správu operačných systémov, počítačových sietí, databáz, IT služieb a DevOps nástrojov. Praktické príklady nižšie sú určené na bezpečné laboratórne precvičovanie.

<a id="admin-01"></a>
## 1. Porty a sieťová komunikácia

Zobrazíme aktívne TCP spojenia a PID procesov vo Windows:

```cmd
netstat -ano
```

Zobrazíme iba počúvajúce porty:

```cmd
netstat -ano | findstr LISTENING
```

Otestujeme dostupnosť HTTPS portu:

```powershell
Test-NetConnection www.vita.sk -Port 443
```

V Linuxe zobrazíme počúvajúce TCP a UDP porty:

```bash
ss -tulpn
```

<a id="admin-02"></a>
## 2. Linux terminál, shell a WSL

Zobrazíme dostupné WSL distribúcie:

```powershell
wsl --list --verbose
```

Spustíme predvolenú Linux distribúciu:

```powershell
wsl
```

V Linuxe zobrazíme jadro a architektúru systému:

```bash
uname -a
```

Zistíme aktuálny shell:

```bash
echo $SHELL
```

Zobrazíme aktuálny pracovný adresár:

```bash
pwd
```

<a id="admin-03"></a>
## 3. IP adresy, DHCP a DNS

Vo Windows zobrazíme kompletnú IP konfiguráciu vrátane DHCP a DNS:

```cmd
ipconfig /all
```

Zistíme DNS záznam domény:

```cmd
nslookup vita.sk
```

V PowerShelli zobrazíme IP konfiguráciu:

```powershell
Get-NetIPConfiguration
```

Zobrazíme nastavené DNS servery:

```powershell
Get-DnsClientServerAddress
```

V Linuxe zobrazíme IP adresy:

```bash
ip addr
```

Zobrazíme routovaciu tabuľku:

```bash
ip route
```

<a id="admin-04"></a>
## 4. CMD, Systeminfo, Netstat, Netsh a Sysinternals

Získame základné informácie o systéme:

```cmd
systeminfo
```

Zobrazíme sieťové adaptéry cez `netsh`:

```cmd
netsh interface show interface
```

Zobrazíme profily Windows Firewall:

```cmd
netsh advfirewall show allprofiles
```

Nainštalujeme Microsoft Sysinternals pomocou WinGet:

```powershell
winget install Microsoft.Sysinternals
```

Po inštalácii môžeme na analýzu procesov použiť napríklad Process Explorer a na analýzu sieťových spojení TCPView.

<a id="admin-05"></a>
## 5. Databázy, MySQL a DBeaver

Pripojíme sa k MySQL serveru:

```bash
mysql -u root -p
```

Vytvoríme laboratórnu databázu:

```sql
CREATE DATABASE admin_lab
CHARACTER SET utf8mb4
COLLATE utf8mb4_unicode_ci;
```

Prepneme sa do databázy:

```sql
USE admin_lab;
```

Vytvoríme jednoduchú tabuľku serverov:

```sql
CREATE TABLE servery (
    id INT PRIMARY KEY AUTO_INCREMENT,
    hostname VARCHAR(100) NOT NULL,
    ip_adresa VARCHAR(45) NOT NULL,
    stav VARCHAR(20) NOT NULL
);
```

Pridáme testovací záznam:

```sql
INSERT INTO servery (hostname, ip_adresa, stav)
VALUES ('SERVER01', '192.168.10.10', 'online');
```

Zobrazíme údaje:

```sql
SELECT * FROM servery;
```

V DBeaveri vytvoríme nové databázové pripojenie a rovnaký SQL skript spustíme v SQL Editore.

<a id="admin-06"></a>
## 6. Zálohovanie databáz a ODBC

Vytvoríme SQL zálohu MySQL databázy:

```bash
mysqldump -u root -p admin_lab > admin_lab_backup.sql
```

Obnovíme databázu zo zálohy:

```bash
mysql -u root -p admin_lab < admin_lab_backup.sql
```

Vo Windows otvoríme správcu ODBC zdrojov:

```text
odbcad32.exe
```

Pri ODBC vždy kontrolujeme, či používame správnu 32-bitovú alebo 64-bitovú verziu ovládača.

<a id="admin-07"></a>
## 7. ITIL, monitoring a dostupnosť služieb

Overíme DNS a TCP dostupnosť webovej služby:

```powershell
Test-NetConnection www.vita.sk -Port 443
```

Získame HTTP hlavičky služby:

```bash
curl -I https://www.vita.sk/
```

Jednoduchý PowerShell test dostupnosti viacerých serverov:

```powershell
$servers = 'SERVER01', 'SERVER02', 'SERVER03'

foreach ($server in $servers) {
    Test-Connection -ComputerName $server -Count 1 -Quiet
}
```

Pri monitoringu sledujeme minimálne dostupnosť, odozvu, chybovosť a kapacitné limity služby.

<a id="admin-08"></a>
## 8. Procesy, súbory, cURL a jq

Vo Windows zobrazíme procesy:

```cmd
tasklist
```

V PowerShelli zobrazíme procesy s najvyšším CPU:

```powershell
Get-Process |
    Sort-Object CPU -Descending |
    Select-Object -First 10 Name, Id, CPU
```

V Linuxe použijeme:

```bash
top
```

Vyhľadáme súbor podľa názvu:

```bash
find /var/log -type f -name "*.log" 2>/dev/null
```

Načítame JSON API pomocou cURL:

```bash
curl https://api.github.com/repos/miroslav-reiter/admin-lab
```

Ak máme nainštalovaný `jq`, vyberieme iba názov repozitára:

```bash
curl -s https://api.github.com/repos/miroslav-reiter/admin-lab | jq '.full_name'
```

<a id="admin-09"></a>
## 9. Prevádzková bezpečnosť a riziká

V PowerShelli vypočítame SHA-256 hash súboru:

```powershell
Get-FileHash .\subor.zip -Algorithm SHA256
```

V Linuxe použijeme:

```bash
sha256sum subor.zip
```

Skontrolujeme aktuálne prihlásených používateľov v Linuxe:

```bash
who
```

Vo Windows zobrazíme lokálnych používateľov:

```powershell
Get-LocalUser
```

Pri hodnotení prevádzkového rizika dokumentujeme aktívum, hrozbu, zraniteľnosť, pravdepodobnosť, dopad a navrhované opatrenie.

<a id="admin-10"></a>
## 10. Docker a Jenkins

Overíme Docker:

```bash
docker version
```

Spustíme testovací kontajner:

```bash
docker run --rm hello-world
```

Zobrazíme bežiace kontajnery:

```bash
docker ps
```

Zobrazíme lokálne images:

```bash
docker images
```

Ak máme Jenkins spustený lokálne na predvolenom porte, overíme jeho HTTP odpoveď:

```bash
curl -I http://localhost:8080
```

<a id="windows-iii-pokrocily"></a>
# 🪟 Microsoft Windows III. Pokročilý

<a id="windows-01"></a>
## 1. Príkazový riadok, zariadenia a Firewall

Zobrazíme zariadenia vo Windows:

```powershell
Get-PnpDevice |
    Select-Object Status, Class, FriendlyName
```

Zobrazíme stav profilov Windows Firewall:

```powershell
Get-NetFirewallProfile
```

Zobrazíme iba povolené pravidlá Firewallu:

```powershell
Get-NetFirewallRule -Enabled True |
    Select-Object DisplayName, Direction, Action
```

CMD alternatíva:

```cmd
netsh advfirewall show allprofiles
```

<a id="windows-02"></a>
## 2. Služby, Autoruns a ovládače

Zobrazíme služby:

```powershell
Get-Service
```

Zobrazíme iba zastavené služby:

```powershell
Get-Service |
    Where-Object Status -eq 'Stopped'
```

Zobrazíme ovládače cez CMD:

```cmd
driverquery
```

Podrobnejší PowerShell výpis podpísaných ovládačov:

```powershell
Get-CimInstance Win32_PnPSignedDriver |
    Select-Object DeviceName, DriverVersion, Manufacturer
```

Zobrazíme vybrané položky spúšťané pri štarte:

```powershell
Get-CimInstance Win32_StartupCommand |
    Select-Object Name, Command, Location
```

Na detailnú analýzu automatického spúšťania môžeme použiť Microsoft Sysinternals Autoruns.

<a id="windows-03"></a>
## 3. Indexovanie a Windows Search

Skontrolujeme službu Windows Search:

```powershell
Get-Service -Name WSearch
```

Zobrazíme jej detailný stav:

```powershell
Get-CimInstance Win32_Service -Filter "Name='WSearch'" |
    Select-Object Name, State, StartMode, PathName
```

Pred prípadným reštartom služby použijeme simuláciu:

```powershell
Restart-Service -Name WSearch -WhatIf
```

<a id="windows-04"></a>
## 4. Diskový priestor a Robocopy

Zobrazíme disky a voľné miesto:

```powershell
Get-Volume |
    Select-Object DriveLetter, FileSystemLabel, Size, SizeRemaining
```

Zobrazíme PowerShell disky:

```powershell
Get-PSDrive -PSProvider FileSystem
```

Skopírujeme adresár pomocou Robocopy:

```cmd
robocopy C:\Data D:\Backup\Data /E /COPY:DAT /R:2 /W:2
```

Najskôr môžeme použiť režim iba na zobrazenie plánovaných operácií:

```cmd
robocopy C:\Data D:\Backup\Data /E /L
```

Parameter `/L` nevykoná kopírovanie a iba zobrazí plánované zmeny.

<a id="windows-05"></a>
## 5. God Mode a systémové nástroje

Vytvoríme priečinok God Mode na pracovnej ploche:

```cmd
mkdir "%USERPROFILE%\Desktop\GodMode.{ED7BA470-8E54-465E-825C-99712043E01C}"
```

Otvoríme Monitor výkonu:

```text
perfmon
```

Otvoríme Monitor prostriedkov:

```text
resmon
```

Otvoríme Zobrazovač udalostí:

```text
eventvwr.msc
```

<a id="windows-06"></a>
## 6. Hosts File a sieťové porty

Otvoríme Hosts File ako administrátor:

```cmd
notepad C:\Windows\System32\drivers\etc\hosts
```

Príklad lokálneho záznamu:

```text
127.0.0.1 test.local
```

Vyprázdnime DNS cache:

```cmd
ipconfig /flushdns
```

Zobrazíme počúvajúce TCP porty:

```powershell
Get-NetTCPConnection -State Listen |
    Sort-Object LocalPort |
    Select-Object LocalAddress, LocalPort, OwningProcess
```

Zistíme proces podľa PID:

```powershell
Get-Process -Id 1234
```

<a id="windows-server-i-zaciatocnik"></a>
# 🖥️ Microsoft Windows Server I. Začiatočník

<a id="server-01"></a>
## 1. Serverové roly, DNS a DHCP

Zobrazíme nainštalované a dostupné roly:

```powershell
Get-WindowsFeature
```

Zobrazíme iba nainštalované roly a funkcie:

```powershell
Get-WindowsFeature |
    Where-Object Installed
```

Na DNS serveri zobrazíme zóny:

```powershell
Get-DnsServerZone
```

Na DHCP serveri zobrazíme IPv4 scopes:

```powershell
Get-DhcpServerv4Scope
```

Zobrazíme IP konfiguráciu servera:

```powershell
Get-NetIPConfiguration
```

<a id="server-02"></a>
## 2. Hosts File

Zobrazíme obsah Hosts File:

```powershell
Get-Content C:\Windows\System32\drivers\etc\hosts
```

Otestujeme preklad mena:

```powershell
Resolve-DnsName SERVER01
```

Hosts File používame na lokálne statické mapovanie názvu na IP adresu. Nenahrádza plnohodnotnú správu DNS v doménovom alebo produkčnom prostredí.

<a id="server-03"></a>
## 3. Služby, Remote Access a terminál

Zobrazíme služby:

```powershell
Get-Service
```

Vyhľadáme služby podľa názvu:

```powershell
Get-Service -Name '*Remote*'
```

Zobrazíme stav roly Remote Access:

```powershell
Get-WindowsFeature RemoteAccess
```

Zobrazíme prihlásené relácie:

```cmd
query session
```

Zobrazíme prihlásených používateľov:

```cmd
query user
```

<a id="server-04"></a>
## 4. Procesy a porty

Zobrazíme desať procesov s najvyšším CPU:

```powershell
Get-Process |
    Sort-Object CPU -Descending |
    Select-Object -First 10 Name, Id, CPU
```

Zobrazíme desať procesov s najvyššou pracovnou sadou RAM:

```powershell
Get-Process |
    Sort-Object WorkingSet64 -Descending |
    Select-Object -First 10 Name, Id, WorkingSet64
```

Zobrazíme počúvajúce porty:

```powershell
Get-NetTCPConnection -State Listen |
    Sort-Object LocalPort
```

Zistíme proces vlastniaci konkrétny port:

```powershell
Get-NetTCPConnection -LocalPort 443 |
    Select-Object LocalAddress, LocalPort, State, OwningProcess
```

Podrobný návod pre zamrznuté procesy, služby a RDP relácie je v [`zazmrnutie-os-pc-server.md`](zazmrnutie-os-pc-server.md).

<a id="server-05"></a>
## 5. Monitoring a Wireshark

Ak je dostupný TShark, zobrazíme sieťové rozhrania:

```cmd
tshark -D
```

Krátky desaťsekundový záznam na rozhraní číslo 1:

```cmd
tshark -i 1 -a duration:10
```

Na Linux serveri môžeme výkon sledovať pomocou:

```bash
top
```

alebo:

```bash
htop
```

Pri Wiresharku a TSharku zachytávame iba komunikáciu, na ktorú máme oprávnenie.

<a id="linux-administrator-i-zaciatocnik"></a>
# 🐧 Linux Administrátor I. Začiatočník

<a id="linux-01"></a>
## 1. Aplikácie a softvérové balíky

Zobrazíme nainštalované balíky v Debian alebo Ubuntu systéme:

```bash
dpkg -l
```

Vyhľadáme konkrétny balík:

```bash
dpkg -l | grep openssh
```

Aktualizujeme zoznam dostupných balíkov:

```bash
sudo apt update
```

Zobrazíme informácie o balíku:

```bash
apt show openssh-server
```

<a id="linux-02"></a>
## 2. Klávesové skratky a Midnight Commander

V termináli používame napríklad:

```text
Ctrl + C       ukončenie aktuálneho príkazu
Ctrl + L       vyčistenie obrazovky
Ctrl + A       presun na začiatok riadka
Ctrl + E       presun na koniec riadka
Ctrl + R       vyhľadávanie v histórii príkazov
Tab            automatické dopĺňanie
```

Spustíme Midnight Commander:

```bash
mc
```

Ak nie je nainštalovaný:

```bash
sudo apt install mc
```

<a id="linux-03"></a>
## 3. Procesy a systémové prostriedky

Zobrazíme procesy:

```bash
ps aux
```

Interaktívny monitoring:

```bash
top
```

Ak máme `htop`:

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

Bezpečný test ukončenia procesu:

```bash
sleep 600 &
pgrep sleep
kill $(pgrep sleep)
```

Podrobnejší postup pri zamrznutí systému je v [`zazmrnutie-os-pc-server.md`](zazmrnutie-os-pc-server.md).

<a id="linux-04"></a>
## 4. Porty, lsof, ss a netstat

Zobrazíme počúvajúce porty:

```bash
sudo ss -tulpn
```

Zobrazíme sieťové spojenia pomocou `lsof`:

```bash
sudo lsof -i
```

Zistíme proces používajúci port 22:

```bash
sudo lsof -i :22
```

Ak máme nainštalované `net-tools`, môžeme použiť:

```bash
sudo netstat -tulpn
```

Na moderných Linux distribúciách preferujeme `ss` pred starším `netstat`.

<a id="linux-05"></a>
## 5. AWK, MAWK a GAWK

Vytvoríme testovací súbor:

```bash
printf "server01 online 45\nserver02 offline 0\nserver03 online 82\n" > servery.txt
```

Zobrazíme prvý stĺpec:

```bash
awk '{print $1}' servery.txt
```

Vyfiltrujeme iba online servery:

```bash
awk '$2 == "online" {print $1, $3}' servery.txt
```

Spočítame servery:

```bash
awk 'END {print NR}' servery.txt
```

Zobrazíme číslo riadka pomocou zabudovanej premennej `NR`:

```bash
awk '{print NR, $0}' servery.txt
```

Zistíme dostupnú implementáciu AWK:

```bash
awk --version
```

Pri niektorých systémoch môžeme samostatne používať `mawk` alebo `gawk`.

<a id="linux-06"></a>
## 6. SED

Vytvoríme testovací konfiguračný súbor:

```bash
printf "server=dev\nport=8080\nmode=test\n" > app.conf
```

Nahradíme text iba vo výstupe:

```bash
sed 's/mode=test/mode=production/' app.conf
```

Vytvoríme zálohu a upravíme súbor priamo:

```bash
sed -i.bak 's/mode=test/mode=production/' app.conf
```

Zobrazíme iba riadok obsahujúci port:

```bash
sed -n '/port/p' app.conf
```

<a id="linux-07"></a>
## 7. PostgreSQL a DBeaver

Nainštalujeme PostgreSQL na Ubuntu:

```bash
sudo apt update
sudo apt install postgresql
```

Skontrolujeme službu:

```bash
systemctl status postgresql
```

Otvoríme PostgreSQL konzolu:

```bash
sudo -u postgres psql
```

Vytvoríme databázu:

```sql
CREATE DATABASE admin_lab;
```

Zobrazíme databázy v `psql`:

```text
\l
```

V DBeaveri vytvoríme PostgreSQL pripojenie na správny hostname, port, databázu a používateľa.

<a id="linux-08"></a>
## 8. Docker v Linuxe

Overíme Docker:

```bash
docker version
```

Spustíme testovací kontajner:

```bash
docker run --rm hello-world
```

Zobrazíme kontajnery:

```bash
docker ps -a
```

Zobrazíme images:

```bash
docker images
```

Spustíme jednoduchý Nginx kontajner na porte 8080:

```bash
docker run --rm -d --name web-test -p 8080:80 nginx
```

Otestujeme službu:

```bash
curl http://localhost:8080
```

Zastavíme kontajner:

```bash
docker stop web-test
```

<a id="powershell-i-zaciatocnik"></a>
# 🔷 PowerShell I. Začiatočník

<a id="powershell-01"></a>
## 1. PowerShell, Terminal a Shell

Zobrazíme verziu PowerShellu:

```powershell
$PSVersionTable
```

Vyhľadáme dostupné príkazy:

```powershell
Get-Command
```

Vyhľadáme príkazy pre procesy:

```powershell
Get-Command -Noun Process
```

Otvoríme pomoc k cmdletu:

```powershell
Get-Help Get-Process
```

Zobrazíme príklady použitia:

```powershell
Get-Help Get-Process -Examples
```

<a id="powershell-02"></a>
## 2. Súbory, priečinky a kopírovanie

Zobrazíme aktuálny adresár:

```powershell
Get-Location
```

Zobrazíme obsah:

```powershell
Get-ChildItem
```

Vytvoríme laboratórny priečinok:

```powershell
New-Item -ItemType Directory -Path .\admin-lab-data
```

Vytvoríme textový súbor:

```powershell
Set-Content -Path .\admin-lab-data\server.txt -Value 'SERVER01'
```

Skopírujeme súbor:

```powershell
Copy-Item .\admin-lab-data\server.txt .\admin-lab-data\server-backup.txt
```

<a id="powershell-03"></a>
## 3. Aplikácie, adaptéry a ovládače

Spustíme aplikáciu:

```powershell
Start-Process notepad
```

Zobrazíme sieťové adaptéry:

```powershell
Get-NetAdapter
```

Zobrazíme iba aktívne adaptéry:

```powershell
Get-NetAdapter |
    Where-Object Status -eq 'Up'
```

Zobrazíme podpísané ovládače:

```powershell
Get-CimInstance Win32_PnPSignedDriver |
    Select-Object DeviceName, DriverVersion, Manufacturer
```

<a id="powershell-04"></a>
## 4. Get príkazy, dátum, čas a rúry

Zobrazíme dátum a čas:

```powershell
Get-Date
```

Zobrazíme iba aktuálny rok:

```powershell
(Get-Date).Year
```

Použijeme rúru na výber služieb:

```powershell
Get-Service |
    Where-Object Status -eq 'Running' |
    Select-Object -First 10 Name, Status
```

Zoradíme procesy podľa CPU:

```powershell
Get-Process |
    Sort-Object CPU -Descending |
    Select-Object -First 10 Name, Id, CPU
```

<a id="powershell-05"></a>
## 5. Skripty, cykly a dátové typy

Premenná typu reťazec:

```powershell
$server = 'SERVER01'
$server.GetType()
```

Pole serverov:

```powershell
$servers = 'SERVER01', 'SERVER02', 'SERVER03'
```

Cyklus `foreach`:

```powershell
foreach ($server in $servers) {
    Write-Host "Kontrolujem $server"
}
```

Cyklus `for`:

```powershell
for ($i = 1; $i -le 5; $i++) {
    Write-Host "Pokus $i"
}
```

Cyklus `while`:

```powershell
$i = 1

while ($i -le 3) {
    Write-Host "Kontrola $i"
    $i++
}
```

Export objektov do CSV:

```powershell
Get-Process |
    Select-Object Name, Id, CPU |
    Export-Csv .\procesy.csv -NoTypeInformation -Encoding UTF8
```

<a id="powershell-06"></a>
## 6. MAC adresa a IP adresa

Zobrazíme MAC adresy:

```powershell
Get-NetAdapter |
    Select-Object Name, Status, MacAddress
```

Zobrazíme IP adresy:

```powershell
Get-NetIPAddress |
    Select-Object InterfaceAlias, AddressFamily, IPAddress
```

Zobrazíme kompletnú IP konfiguráciu:

```powershell
Get-NetIPConfiguration
```

Otestujeme sieťové spojenie:

```powershell
Test-NetConnection www.vita.sk -Port 443
```

## 📚 Zdroje kurzov

1. [VITA Academy – Administrátor a Správca IT](https://www.vita.sk/online-kurz-administrator-a-spravca-it/)
2. [VITA Academy – Microsoft Windows III. Pokročilý](https://www.vita.sk/online-kurz-microsoft-windows-iii-pokrocily/)
3. [VITA Academy – Microsoft Windows Server I. Začiatočník](https://www.vita.sk/online-kurz-microsoft-windows-server-i-zaciatocnik/)
4. [VITA Academy – Linux Administrátor I. Začiatočník](https://www.vita.sk/online-kurz-linux-administrator-linux-admin-i-zaciatocnik/)
5. [VITA Academy – PowerShell I. Začiatočník](https://www.vita.sk/online-kurz-powershell-i-zaciatocnik/)
