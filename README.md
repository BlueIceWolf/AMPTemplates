# GitHub Actions Runner für AMP

Ich nutze dieses Repository, um einen eigenen GitHub Actions Runner direkt über **CubeCoders AMP** zu verwalten. Der Runner übernimmt Builds und andere GitHub-Actions-Aufgaben auf meinem eigenen Server.

Die Vorlage ist bewusst schlank: eine Anwendung, keine unnötigen Ports und keine Zugangsdaten im Repository.

## Was funktioniert – und was noch nicht?

AMP kann mit der Vorlage den offiziellen Linux-x64-Runner herunterladen und den Runner-Prozess starten bzw. stoppen. Die GitHub-Registrierung muss ich **einmalig per SSH** durchführen. Eine automatische Registrierung über die AMP-Oberfläche ist noch nicht umgesetzt.

**Stand:** erste, noch nicht auf AMP 2.8.0.4 getestete Version. Bitte nicht als fertig getestete Installation verstehen. Der Download ist zunächst auf die geprüfte Release-Dateibezeichnung **v2.338.2** festgelegt; ein späteres Update erfordert eine Anpassung der Vorlage. Der direkte Prozessstart verwendet bewusst die ausführbare Datei `bin/Runner.Listener` statt eines Shellskripts; das automatische Neustartverhalten des offiziellen `run.sh` wird dabei nicht vollständig nachgebildet.

## In AMP hinzufügen

1. **Configuration → Instance Deployment → Configuration Repositories** öffnen.
2. Als Repository **`BlueIceWolf/AMPTemplates:main`** eintragen.
3. **Fetch Latest** ausführen.
4. Unter **Create New Instance** nach **GitHub Actions Runner** suchen.
5. Eine Instanz erstellen und zunächst **nicht starten**.
6. In dieser Instanz über AMP einmal **Update** ausführen. Dadurch wird das Runner-Paket heruntergeladen.

## Einmalig mit GitHub verbinden

Im Repository auf GitHub **Settings → Actions → Runners → New self-hosted runner** öffnen. Dort bekomme ich die richtige GitHub-URL und einen **kurzlebigen Registrierungstoken**.

Die Registrierungsbefehle führe ich als Benutzer aus, dem die AMP-Instanzdateien gehören – **nicht als root** – und zwar direkt im Verzeichnis mit `config.sh` und `bin/`. Beispiel:

```bash
./config.sh --url https://github.com/MEIN-BENUTZER/MEIN-REPO --token EINMAL-TOKEN --unattended --name amp-runner-01 --labels amp,debian12
```

Das Beispiel ist **kein vollständiger Befehl zum Kopieren**: URL, Token, Runner-Name und das richtige Arbeitsverzeichnis müssen angepasst werden. Eine Repository-Registrierung gilt nur für das gewählte Repository. Für die Nutzung in mehreren Repositories kann stattdessen ein Runner auf Organisationsebene eingerichtet werden.

**Wichtig:** Ein Runner verarbeitet normalerweise einen Job gleichzeitig. Mehrere parallele Jobs brauchen mehrere Runner-Instanzen.

## Sicherheit

Ich betreibe GitHub-Runner möglichst **isoliert** und gebe ihnen keinen direkten Zugriff auf AMP, Gameserver, Docker-Socket oder produktive Dienste. Besonders bei öffentlichen Repositories können Workflows fremden Code ausführen. Ich speichere weder Tokens noch Passwörter in dieser Vorlage. Für Docker-Builds ist eine separat abgesicherte Build-Umgebung notwendig.

## Technische Grundlage

- CubeCoders AMP, Generic Module, mindestens 2.8.0.4
- Linux x86-64, ausgelegt für Debian 12
- Offizieller [GitHub Actions Runner](https://github.com/actions/runner)
- [Vorlagenaufbau von CubeCoders](https://github.com/CubeCoders/AMPTemplates)

Die Vorlage nutzt keine eingehenden Netzwerkports. Der Runner verbindet sich selbst ausgehend mit GitHub über HTTPS.
