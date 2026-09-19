# Workshop-Skript: „Lizenz zum Deployen“

**Change IT GmbH: CloudStack per MCP mit Claude Desktop inventarisieren und VMs deployen**

Cisco Akademientage 2026 · Dienstag, 17.11.2026 · Dauer: 60 Minuten · Zielgruppe: Lehrkräfte

Referent: Dr. Jörg Tuttas, Multi-Media Berufsbildende Schulen Hannover

## Zeitübersicht

| Zeit | Phase | Sozialform | Ergebnis |
|---|---|---|---|
| 00–04 | Einstieg: Auftrag bei der Change IT GmbH | Plenum | Problem ist klar |
| 04–08 | Der klassische Weg | Demo | Aufwand wird sichtbar |
| 08–20 | Live-Demo: Inventar, Excel, Dashboard | Demo | Inventarbericht, .xlsx, Dashboard |
| 20–30 | Einrichtung: Keys erzeugen, MCPB installieren | Einzel/Tandem | Verbindungstest erfolgreich |
| 30–48 | Arbeitsphase: Linux-VM mit Public IP und SSH | Einzel/Tandem | SSH-Login auf eigener VM |
| 48–55 | Ergebnissicherung & Reflexion | Plenum | Chancen/Risiken benannt |
| 55–60 | Transfer, Aufräumen, Abschluss | Plenum | Materialien, Ressourcen freigegeben |

## Phase 1 – Einstieg: Der Auftrag (00:00–00:04)

**Folie „Montag, 8:15 Uhr bei der Change IT GmbH“:** Ein Fachinformatiker Systemintegration im 2. Ausbildungsjahr erhält eine Mail der Teamleitung:

> „Für das Controlling brauche ich bis Mittag eine Excel-Liste aller virtuellen Maschinen in unserer CloudStack-Umgebung mit Status, Ressourcen und IP-Adressen. Für die Geschäftsführung außerdem ein übersichtliches Dashboard zur Auslastung. Und: Das Entwicklerteam braucht heute noch eine Test-VM, die per SSH über eine öffentliche IP erreichbar ist.“

Impulsfrage ans Plenum: *„Wie würden Ihre Azubis das angehen, und wie lange bräuchten sie?“*

## Phase 2 – Der klassische Weg (00:04–00:08)

- Im CloudStack-Web-UI zeigen: VM-Liste, Details einer VM, Netzwerke, öffentliche IPs.
- Das Abtippen in eine Excel-Tabelle bewusst mühsam andeuten.
- Alternative kurz nennen: API-Skripting mit CloudMonkey oder Python, was Programmierkenntnisse erfordert.

Überleitung: *„Was, wenn der Azubi die Umgebung einfach fragen könnte, mit genau den Rechten, die er ohnehin hat?“*

## Phase 3 – Live-Demo: Inventar, Excel, Dashboard (00:08–00:20)

**Architekturfolie (2 Min.):** Claude Desktop ↔ Desktop Extension (.mcpb, lokaler MCP-Server) ↔ CloudStack-API (`https://cloudstack.mm-bbs.de/client/api`, API-Key/Secret). Kernaussagen:

- Claude ruft nur definierte Tools auf.
- Die Rechte bestimmt der CloudStack-Account, nicht die KI.
- Jede Tool-Nutzung ist im Freigabedialog sichtbar.

**Demo-Prompts (in dieser Reihenfolge):**

1. **Überblick** (2 Min.): *„Welche Zonen, Service Offerings und Templates gibt es in der CloudStack-Umgebung der Change IT GmbH?“*
2. **Inventar** (2 Min.): *„Liste alle virtuellen Maschinen mit Name, Status, CPU, RAM, Template, Netzwerk und IP-Adresse als Tabelle.“*
3. **Excel** (3 Min.): *„Erstelle aus diesen Daten eine Excel-Datei ‚VM-Inventar_ChangeIT.xlsx‘ mit einem Tabellenblatt ‚VMs‘ als formatierte Tabelle mit Filter und einem Blatt ‚Zusammenfassung‘ mit Summen für CPU und RAM pro Status.“*
4. **Dashboard** (3 Min.): *„Erstelle ein Dashboard für die Geschäftsführung der Change IT GmbH: Kennzahlen (Anzahl VMs, laufend/gestoppt, CPU und RAM gesamt), ein Diagramm zur Ressourcenverteilung und eine Tabelle der VMs.“*

Die Excel-Datei kurz öffnen und die Werte stichprobenartig mit dem Web-UI vergleichen. Dabei lässt sich der Punkt „KI-Ergebnisse verifizieren“ direkt vorleben. Das Deployment der Test-VM **nicht** vorführen, denn das ist der Auftrag der Teilnehmenden.

**Absicherung:** Excel-Datei und Dashboard vorab einmal erzeugen und bereithalten, falls die Generierung live zu lange dauert.

## Phase 4 – Einrichtung (00:20–00:30)

**Anleitung (Folie + README im GitHub-Repository https://github.com/jtuttas/cloudstack-mcp-workshop, Link als QR-Code):**

1. **Anmelden** im CloudStack-Web-UI: Benutzername `teilnehmera`, `teilnehmerb`, `teilnehmerc` … (klein, ohne Leerzeichen), Kennwort `geheim`, **Domain `/mmbbs/FIAE`**. Die Domain muss im Anmeldeformular ausgewählt bzw. eingetragen werden, sonst schlägt die Anmeldung fehl.
2. **API-Keys erzeugen:** Im Benutzerprofil „Generate Keys“ bzw. „Schlüssel generieren“ ausführen, dann API-Key und Secret kopieren
3. **MCP-Server herunterladen:** Die `.mcpb`-Datei aus dem GitHub-Repository (Ordner `05_MCP-Server/`) laden
4. **Installieren:** Die `.mcpb`-Datei per Doppelklick öffnen oder in Claude Desktop unter *Einstellungen → Erweiterungen* einfügen
5. **Konfigurieren:** API-Key und API-Secret in die Felder der Erweiterung eintragen und die Erweiterung aktivieren
6. **Verbindungstest:** *„Welche Zonen stehen mir zur Verfügung, und welche VMs habe ich bereits?“*

**Meilenstein:** Wer die Zonenliste sieht, zeigt die grüne Karte. Wer nach 8 Minuten hängt, bildet ein Tandem.

## Phase 5 – Arbeitsphase: Test-VM deployen (00:30–00:48)

**Arbeitsauftrag (Handout/Folie):**

> Das Entwicklerteam der Change IT GmbH benötigt eine Test-VM. Beauftragen Sie Claude mit einem einzigen Prompt, zum Beispiel:
>
> *„Richte mir auf Basis des Templates Ubuntu22.04_cloudinit eine VM mit dem Namen test-a ein, die über eine öffentliche IP-Adresse per SSH erreichbar ist.“*
>
> Verbinden Sie sich anschließend mit der VM: `ssh mmbbs@<public-ip>` (Kennwort: `geheim`)

**Beobachtungsauftrag:** Notieren Sie, welche Einzelschritte Claude selbstständig geplant und ausgeführt hat und wo Claude nachgefragt hat.

Erwartbar sind etwa diese Schritte:

- Offering und Template auswählen
- Netzwerk auswählen
- VM deployen
- Öffentliche IP beziehen
- Static NAT einrichten
- Firewall-Regel für Port 22 anlegen

**Differenzierung:**

- **Basis:** VM per Prompt einrichten und per SSH anmelden
- **Erweitert:** Die eigene VM in die Excel-Inventarliste ergänzen lassen
- **Profi:** Claude die Sicherheit der Konfiguration bewerten und härten lassen, z. B. SSH nur aus dem Tagungsnetz zulassen

**Stolpersteine für die Betreuung:**

- **Template:** Claude sucht das Template über `list_templates`. Falls es nicht gefunden wird, liegt das meist am Template-Filter: Das Template muss für die Teilnehmer-Accounts sichtbar sein (öffentlich bzw. „featured“ oder für die Domain freigegeben).
- **Boot-Zeit:** SSH antwortet erst 1–2 Minuten nach dem Deployment; cloud-init-Images brauchen beim ersten Start etwas länger.
- **Host-Key-Warnung:** Beim ersten Login mit `yes` bestätigen. Werden IPs im Workshop mehrfach vergeben, kann eine „REMOTE HOST IDENTIFICATION HAS CHANGED“-Warnung auftreten; Lösung: `ssh-keygen -R <public-ip>`.
- **Unvollständige Umsetzung:** Vergisst Claude einen Schritt, z. B. die Firewall-Regel, diagnostizieren die Teilnehmenden den Fehler gemeinsam mit Claude. Das ist ein guter Reflexionsanlass.
- **Zeitansage** bei 00:45.

## Phase 6 – Ergebnissicherung & Reflexion (00:48–00:55)

- Im Web-UI am Beamer alle Teilnehmer-VMs und öffentlichen IPs zeigen, oder live: *„Liste alle VMs, die mit test- beginnen.“*
- 2–3 Teilnehmende berichten von ihrer Beobachtung.

**Diskussionsimpulse:**

- Ein Prompt hat mehrere API-Aktionen ausgelöst. Wer trägt die Verantwortung für die Konfiguration, und wie viel Fachwissen braucht man, um das Ergebnis prüfen zu können?
- Was muss ein FISI noch selbst verstehen? (NAT, Firewall, SSH, Offerings)
- Offener SSH-Zugang mit Standardkennwort: Hat Claude gewarnt? *„Claude hat genau das umgesetzt, was wir wollten. Aber war das eine gute Idee?“*
- Sicherheit allgemein: Least-Privilege-Rechte, Tool-Freigaben, destruktive Tools wie `destroy_virtual_machine`
- Unterrichtseinsatz: Das Change-IT-Szenario als Lernsituation, z. B. im Cloud- oder Netzwerk-Lernfeld; Excel/Dashboard als Bezug zur Datenauswertung

## Phase 7 – Transfer & Abschluss (00:55–01:00)

- **Aufräumen:** Entweder lassen die Teilnehmenden ihre VM und die öffentliche IP von Claude freigeben, was die Freigabe destruktiver Aktionen bewusst erlebbar macht, oder der Referent räumt zentral auf. In jedem Fall werden die VMs direkt am Workshop-Ende gelöscht.
- Hinweis: Accounts und API-Keys werden nach dem Workshop deaktiviert.
- Materialien per QR-Code (`06_Bilder/QR-Code_Repository.png`): GitHub-Repository https://github.com/jtuttas/cloudstack-mcp-workshop mit `.mcpb`, README, Arbeitsauftrag, Szenario-Vorlage
- Kurzes Feedback

## Vorbereitung (nicht Teil der 60 Minuten)

### CloudStack

- Accounts `teilnehmera`, `teilnehmerb`, `teilnehmerc` … in der Domain `/mmbbs/FIAE` mit Kennwort `geheim` und eigener Rolle ohne Admin-Rechte anlegen; Resource Limits pro Account setzen (z. B. 1 VM, 1 öffentliche IP, kleines Offering)
- **Public-IP-Pool prüfen:** Es wird mindestens eine IP pro Teilnehmer plus Reserve benötigt. Das ist der wahrscheinlichste Engpass.
- Pro Account ein isoliertes Netzwerk vorab anlegen, oder Claude das Netzwerk im Rahmen des Auftrags anlegen lassen und das vorab testen
- Demo-Umgebung mit einigen „Change IT“-VMs (z. B. `web-01`, `db-01`, `mail-01`) befüllen, damit Excel und Dashboard aussagekräftig werden

### Template `Ubuntu22.04_cloudinit`

- **Sichtbarkeit prüfen:** Mit einem Teilnehmer-Account im Web-UI kontrollieren, ob das Template angezeigt wird, und im Claude-Test `list_templates` aufrufen lassen.
- **Passwort-Login prüfen:** Sicherstellen, dass cloud-init den Passwort-Login für `mmbbs` nicht deaktiviert. In vielen Ubuntu-Cloud-Images ist standardmäßig `ssh_pwauth: false` gesetzt, sodass der SSH-Login per Kennwort trotz korrektem Benutzer scheitert.
- **End-to-End-Test:** VM mit einem Teilnehmer-Account per Claude deployen, öffentliche IP zuweisen lassen und per `ssh mmbbs@<public-ip>` anmelden. Die Zeit vom Prompt bis zum erfolgreichen Login messen und in die Zeitplanung von Phase 5 einbeziehen.

### Sicherheit

Die Kombination aus öffentlicher IP, offenem Port 22, bekanntem Benutzernamen und dem Kennwort `geheim` ist kritisch. SSH-Ports im Internet werden innerhalb von Minuten automatisiert per Brute Force angegriffen, und `geheim` steht in gängigen Passwortlisten. Eine kompromittierte VM läge in der IP-Range der Schule.

Empfohlene Maßnahmen (mindestens eine davon):

1. **Quell-IP einschränken (am wirksamsten):** Firewall-Regel nur für die öffentliche IP des Tagungsnetzes zulassen. Das lässt sich im Prompt vorgeben (*„… per SSH erreichbar, aber nur von der IP-Adresse X“*). Vorab die öffentliche IP des Tagungsorts ermitteln.
2. **Kurze Lebensdauer:** VMs direkt am Workshop-Ende löschen und zusätzlich zentral nach dem Workshop prüfen.
3. **Image härten:** `fail2ban` im Template vorinstallieren und das Kennwort nach dem ersten Login ändern lassen (`passwd`).

Zusätzlich: Teilnehmer-Accounts erst kurz vor dem Termin aktivieren und direkt danach deaktivieren; API-Keys anschließend neu generieren.

### Teilnehmende (Einladung)

- Claude Desktop vorab installieren und anmelden. Node.js muss nicht installiert werden, da die Desktop Extension ohne separate Laufzeitumgebung auskommt.
- Vorher klären, ob Desktop Extensions und die Erstellung von Dateien (Excel) bzw. Artefakten (Dashboard) mit dem Kontotyp der Teilnehmenden funktionieren (kostenloser Plan vs. Pro), und das in der Einladung angeben.
- SSH-Client: Unter Windows 10/11 ist OpenSSH in der Regel vorinstalliert, trotzdem in der Einladung erwähnen.

### Plan B

- Tagungs-WLAN blockiert die API oder ausgehendes SSH (Port 22): vorab testen, mobilen Hotspot bereithalten
- Installation scheitert: Tandemarbeit
- Live-Demo scheitert: Vorab einen Screencast der Demo aufzeichnen

### Zeitkritische Stellen

Phase 4 (Einrichtung) und die Boot-Zeit der VMs in Phase 5. Da die Arbeitsphase nur einen Prompt erfordert, entsteht Puffer: Bei Verzug in Phase 4 kann Phase 5 auf 12 Minuten gekürzt werden, ohne die Basisaufgabe zu gefährden. Phase 6 bleibt in jedem Fall erhalten.
