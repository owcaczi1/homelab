# 🧪 Homelab

![Status](https://img.shields.io/badge/Status-Under_Construction-orange)
![Philosophy](https://img.shields.io/badge/Philosophy-Break_to_Learn-red)
![Goal](https://img.shields.io/badge/Goal-CCNA_%2F_LPIC-blue)

<br>

> **"Im więcej się psuje, tym lepiej - bo więcej się uczę."**
>
> **Filozofia:** "Break it to fix it". Ten projekt to celowy chaos architektoniczny. Mieszam vendorów i wdrażam rozwiązania Enterprise "na twardo", żeby wymusić błędy i nauczyć się prawdziwego troubleshootingu. Zero gotowców, czysta praktyka pod CCNA i LPIC.

---

<br>

## 🏗️ Aktualny Sprzęt (Hardware Inventory)


| Rola | Sprzęt | Uwagi |
| :--- | :--- | :--- |
| **WAN** | Orange FTTH 8/1 Gbps + LEOX ONT | XGS-PON |
| **Firewall** | Ubiquiti UCG Fiber | IDS/IPS, NGFW-Like|
| **Switching** | Ubiquiti USW-Pro-HD-24 | L3 Switch |
| **Lab Network** | Mikrotik RB5009, Cisco 1921 x2, Cisco 3560 x2 | Środowisko testowe |
| **Compute** | Lenovo Tiny M720q, Raspberry Pi 4B | Proxmox, Debian |


 


<img src="Images\spe.png" alt="Topologia Sieci HomeLab" width="50%">
<img src="Images\rak.jpg" alt="Topologia Sieci HomeLab" width="50%">
<br>


## 🗺️ Topologia sieci

<details>
<summary><b>📷 Zobacz schemat graficzny </b></summary>
<br>
<img src="Images\topology.png" alt="Topologia Sieci HomeLab" width="50%">
<br><br>
</details>

---
### 🏗️ Architektura logiczna

Infrastruktura została podzielona na dwa odseparowane logicznie środowiska (Environments), aby zapewnić stabilność usług domowych przy jednoczesnym zachowaniu swobody testów inżynierskich.

<details><summary><b>Logiczny podział sieci</b></summary>
<br>


| Środowisko | Komponent fizyczny | Szczegóły |
| :--- | :--- | :--- |
| **Produkcyjne (Ubiquiti UniFi)** | Edge | Orange ONT ➔ **UCG Fiber** Gateway. |
| **Produkcyjne (Ubiquiti UniFi)** | Core Switching | **USW Pro 24 HD** L3 Switching. |
| **Produkcyjne (Ubiquiti UniFi)** | Access | **U7 Pro XGS** – łączność dla urządzeń bezprzewodowych. |
| **Produkcyjne (Ubiquiti UniFi)** | IoT | Izolowana strefa dla IoT (fizyczna separacja via porty/switch). |
| **Laboratoryjne (Mikrotik, Cisco)** | Symulowany ISP | **MikroTik RB5009**. Pełni rolę dostawcy WAN dla labu. |
| **Laboratoryjne (Mikrotik, Cisco)** | Lab Edge | 2x **Cisco 1941**. Działają w HSRP. |
| **Laboratoryjne (Mikrotik, Cisco)** | Lab Core | 3x **Cisco 3560 Catalyst**. Topologia Spine-Leaf. |

</details>
<br>

---

### 🛡️ Plan adresacji (logiczny podział VLAN-ów)

<details><summary><b>Zastosowano standard **RFC1918** z podziałem na VLAN-y funkcjonalne.</b></summary>
<br>

To jest czysto logiczne: separacja na poziomie warstw 2/3 bez zmiany fizycznego okablowania. VLAN-y pozwalają na izolację ruchu bez dodatkowych switchy.

| VLAN ID | Nazwa sieci | Podsieć | Opis / Rola |
| :---: | :--- | :--- | :--- |
| **10** | `MGMT_INFRA` | `10.10.0.0/24` | Zarządzanie przełącznikami i AP (Sieć Natywna). |
| **20** | `HOME_LAN` | `10.20.0.0/24` | Urządzenia końcowe. (Trusted). |
| **30** | `IOT_ISOLATED` | `10.30.0.0/24` | Urządzenia IoT. **Pełna izolacja od LAN.** |
| **99** | `LAB_WAN_UPLINK` | `172.16.99.0/30` | Link P2P: USW Pro ↔ RB5009 (Interconnect). |
| **100** | `CISCO_LAB_INSIDE`| `192.168.100.0/24` | Wewnętrzna sieć za routerami Cisco 1941. |
| **666** | `GUEST` | `192.168.254.0/24` | Sieć dla gości|
</details>
<br>

---
<br>

## 🎯 Cele i Certyfikacja
<br>
<details>
<summary><b>⏳ Short-term Goals: Cisco CCNA</b></summary>
<br>

| Kurs / Egzamin | Status | Deadline | Badge |
| :--- | :---: | :---: | :---: |
| **1. Introduction to Networks** | ✅ **DONE** | - | <img src="Images\badge.png" height="50"> |
| **2. Switching, Routing, & Wireless** | 🔄 **In Progress** | **14.01** | 🔒 |
| **3. Enterprise, Security, & Automation** | ⏳ **Planned** | **31.01** | 🔒 |
| **4. Egzamin CCNA 200-301** | 🎯 **Cel** | **15.02** | 🏆 |

</details>

<details>
<summary><b>🚀 Long-term Goals: Linux Professional Institute (LPIC)</b></summary>
<br>

**Ścieżka administracji systemami Linux (LPI):**

-  **1. LPIC 1-101** - *Fundamenty systemu Linux + sieć i storage (baza pod HA).*
-  **2. LPIC-1 102** - *Usługi, bezpieczeństwo i automatyzacja podstawowa.*
-  **3. LPIC-2** - *Administracja zaawansowana + zarządzanie środowiskami produkcyjnymi.*
-  **4. LPIC 3-305/306** - *High Availability (HA), klastry i wirtualizacja.*
-  **5. LPIC 3-303** - *Bezpieczeństwo infrastruktury i usług krytycznych.*

</details>


<br>

## 📅 Roadmapa implementacji technologii w homelabie

> **Cel:** Komplikować życie, mieszać vendorów, unikać gotowców, budować od zera.

---

<details>
<summary><b>🏆 Level 1: Networking </b></summary>
<br>

*Celem jest zrozumienie, jak naprawdę działa sieć, wychodząc poza prosty router od dostawcy.*

- ✅ **Analiza potrzeb i rozplanowanie homelaba**
  - ✅ Wybór hardware
  - ✅ Podłączenie i konfiguracja sprzętu sieciowego
  - ✅ Wdrożenie **IDS/IPS**.
  - ✅ Konfiguracja RoS na mieszanym sprzęcie: Ubiquiti + MikroTik.
  - ✅ Celowe wymuszanie routingu między urządzeniami różnych producentów.

- ⚠️ **Segmentacja sieci (VLANs & Security Zones)**
  - ✅ Utworzenie minimum 5 VLAN-ów:
    - `GUEST` (izolowany całkowicie)
    - `IoT` (izolacja "niebezpiecznych" urządzeń)
    - `HOME INFRA` (zaufane urządzenia)
    - `CAM` (CCTV - odcięcie od Internetu)
    - `DMZ` (dla usług wystawionych na świat, np. Nextcloud)
  - ✅ **Polityki Firewall:** Blokada ruchu między VLAN-ami 
  - ❌ Konfiguracja "Zone-Based Firewall".
  - ✅ Ograniczanie przepustowości (QoS/Limiters) między VLAN-ami.

</details>

<details>
<summary><b>🏗️ Level 2: Core Infrastructure Services (Self-Hosted)</b></summary>
<br>

*Przestajemy polegać na routerze w kwestii usług. Wszystko hostujemy sami na serwerach.*

- ✅ **DHCP Server**
  - ❌ Wyniesienie DHCP z routera na dedykowany serwer (Linux/Windows Server).

- ✅ **DNS & AdBlocking**
  - ✅ **Pihole + Unbound:** Instalacja Recursive DNS Server oraz AdBlockera
  - ❌ **AdGuard Home:** Instalacja dwóch instancji (Primary/Secondary) dla High Availability.
  - ❌ **AdGuard Home Sync:** Konfiguracja synchronizacji między instancjami.
  - ❌ **DNS Rewrite:** Lokalne domeny (np. `serwer.lan`) bez wychodzenia do publicznego DNS.

- ❌ **Zarządzanie hasłami**
  - ❌ **Vaultwarden (Bitwarden):** Wdrożenie wersji Self-hosted.

- ❌ **Reverse Proxy**
  - ❌ Nauka narzędzi: **Nginx Proxy Manager**, **Traefik** lub **Caddy**.
  - ❌ Cel: Wystawienie usług pod własną domeną (np. `bitwarden.mojadomena.pl`).

- ❌ **Certyfikaty SSL (PKI)**
  - ❌ Let's Encrypt (automatyzacja).
  - ❌ **Hard Mode (LPIC-303):** Własne CA (Certificate Authority), generowanie kluczy, instalacja Root CA na urządzeniach końcowych.

</details>

<details>
<summary><b>☁️ Level 3: Virtualization & Storage (Home Data Center)</b></summary>
<br>

*Budowa wydajnego klastra obliczeniowego i walka z wydajnością I/O.*

- ⚠️ **Hypervisory**
  - ✅ **Proxmox VE:** Podstawa
  - ❌ **XCP-ng + Xen Orchestra:** Alternatywa Open Source.
  - ✅ **VMware ESXi:** (Opcjonalnie, dla znajomości standardu legacy).

- ❌ **High Availability (HA) Cluster**
  - ❌ Minimum 2-3 węzły (PC/SFF, Intel/AMD).
  - ❌ Symulacja awarii: Fizyczne odłączenie węzła ("pull the plug") i test migracji maszyn.

- ❌ **Storage & NAS**
  - ❌ Systemy: **TrueNAS Scale** lub **OpenMediaVault**.
  - ❌ **ZFS:** Zrozumienie pooli, datasetów, snapshotów, ZIL/SLOG.
  - ❌ Protokóły: iSCSI vs NFS dla wirtualizacji.
  - ❌ **Stress Test:** Symulacja pracy 100 użytkowników (generowanie obciążenia I/O).

- ❌ **Networking w wirtualizacji**
  - ❌ **Agregacja łączy:** LACP (L2) vs SMB Multichannel (L7).
  - ❌ Rozwiązywanie problemów z wąskim gardłem sieciowym dla maszyn wirtualnych.

- [ ] **Konteneryzacja**
  - ✅ **LXC:** Lekkie kontenery systemowe (Proxmox).
  - ❌ **Docker & Portainer:** Zarządzanie mikroserwisami.

</details>

<details>
<summary><b>🔐 Level 4: Secure Remote Access & VPN</b></summary>
<br>

*Dostęp do domu z każdego miejsca na ziemi, ale bezpiecznie.*

- ✅ **VPN Tradycyjny**
  - ✅ OpenVPN
  - ✅ WireGuard

- ✅ **Mesh VPN (SD-WAN)**
  - ✅ **Tailscale / Netbird:** Omijanie braku publicznego IP (CGNAT).

- ⚠️ **Tunele**
  - ✅ **Cloudflare Tunnel:** Bez otwierania portów na routerze.
  - ❌ **Pangolin:** Alternatywa Self-hosted dla Cloudflare.

</details>

<details>
<summary><b>🌍 Level 5: VPS & "Exit to Cloud"</b></summary>
<br>

*Wychodzimy z Home Labu na serwery publiczne. Nauka prawdziwego świata.*

- ❌ **Infrastruktura na VPS**
  - ❌ Wynajem VPS (OVH, Hetzner, Oracle).
  - ❌ **Netbird (Self-hosted):** Własny kontroler sieci Mesh na VPS.
  - ❌ **Nextcloud na VPS:** Odciążenie łącza domowego.
  - ❌ **Mail Server (Hard Mode):** Postawienie poczty od zera (Postfix, Dovecot, SPF, DKIM, DMARC)

- ❌ **Hardening VPS (Security)**
  - ❌ SSH: Zmiana portów, klucze RSA/Ed25519, brak haseł.
  - ❌ **CrowdSec:** Nowoczesny IPS/IDS (analiza behawioralna).
  - ❌ **Wazuh:** SIEM - zbieranie i analiza logów bezpieczeństwa.

</details>

<details>
<summary><b>🆔 Level 6: Identity Management (SSO) & Enterprise</b></summary>
<br>

*Jeden login by wszystkimi rządzić.*

- ❌ **Identity Provider (IdP)**
  - ❌ **Authentik** lub **Keycloak**.
  - ❌ Integracja usług (Proxmox, Portainer, Wiki) przez **OAuth2 / OIDC**.

- ❌ **Active Directory**
  - ❌ Postawienie Windows Server DC.
  - ❌ Integracja usług Linuxowych z AD (LDAP/Kerberos).

- ✅ **MFA / 2FA**
  - ✅ Wymuszenie 2FA wszędzie.
  - ✅ Implementacja kluczy sprzętowych (YubiKey) lub Passkeys.

</details>

<details>
<summary><b>🤖 Level 7: DevOps, Automation & IaC (The Endgame)</b></summary>
<br>

*Koniec z "klikaniem". Wszystko jako kod.*

- ❌ **Ansible (Configuration Management)**
  - ❌ Automatyzacja konfiguracji serwerów (aktualizacje, pakiety).
  - ❌ Tworzenie Playbooków zastępujących ręczną konfigurację.

- ❌ **Terraform (Provisioning)**
  - ❌ Powoływanie maszyn na Proxmoxie/VPS kodem.

- ⚠️ **Git & CI/CD**
  - ❌ **Gitea:** Własne repozytorium kodu.
  - ✅ **Jenkins / GitHub Actions:** Potoki wdrażania (Pipeline).
  - ❌ Scenariusz: *Zmiana w kodzie -> Terraform stawia VM -> Ansible konfiguruje -> Testy.*

- ❌ **Low-Code Automation**
  - ❌ **n8n:** Automatyzacja powiadomień i przepływów pracy.

</details>

<br>

---
*Dokumentacja aktualizowana na bieżąco w miarę postępów w nauce.*
