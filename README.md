# Entgelttabellen TVöD und TV-L
Strukturierte Entgelttabellen in CSV-Form für https://www.gew.de/entgelttabellen.

---

## Was macht dieses Repository?

In diesem Repository liegen die Tabellen (als CSV-Dateien), die als Datengrundlage für die Diagramme und Grafiken auf https://www.gew.de/entgelttabellen und den zugehörigen Unterseiten (TV-L, TVöD, Jahressonderzahlung, Pflege, SuE, Praktikant\*innen/Auszubildende) dienen. Die Tabellen hier sind die einzige Quelle: Änderst du eine Tabelle, aktualisieren sich die Grafiken auf der Website automatisch. Du musst nichts weiter tun.

## Wie funktioniert die automatische Aktualisierung?

Der Hintergrund: Datawrapper (der Dienst, der die Grafiken anzeigt) liest die Tabellen nicht direkt von hier, sondern hält eine eigene Kopie vor. Diese Kopie aktualisiert Datawrapper von sich aus nur eine begrenzte Zeit — nach 30 Tagen hört die automatische Aktualisierung ganz auf.

Deshalb gibt es zwei automatische Vorgänge, beide laufen als sogenannte *GitHub Action* (ein automatischer Helfer-Roboter auf GitHub, der für dich im Hintergrund arbeitet — niemand muss etwas von Hand tun):

1. **Sofortige Aktualisierung:** Bei jeder Änderung an einer CSV-Datei hier werden alle Diagramme sofort auf den neuen Stand gebracht.
2. **Wöchentliche Erneuerung:** Jeden Montag werden alle Diagramme zusätzlich neu veröffentlicht. Dadurch läuft die automatische Aktualisierung nie ab.

## Tabelle ändern

So änderst du eine bestehende Tabelle:

1. Öffne die gewünschte CSV-Datei und bearbeite sie (z. B. mit einem Tabellenprogramm).
2. Speichere die Änderung und lade sie hoch — in GitHub heißt das *committen und pushen* (also: Änderung dauerhaft speichern und auf GitHub hochladen).
3. Kurz danach sind die Grafiken auf der Website aktuell. Sonst ist nichts weiter nötig.

## Neuen Chart hinzufügen

Wenn du eine neue Grafik (einen neuen Chart) einbinden willst, geht das so:

1. Lege den Chart in Datawrapper an.
2. Notiere dir die Chart-ID: eine kurze, meist 5-stellige Zeichenkette aus dem Einbett-Code bzw. der Chart-URL, z. B. `sYOIq`.
3. Öffne die Datei `datawrapper-zuordnung.csv` und füge eine neue Zeile an. Das Format ist `Dateiname.csv; ID` (Semikolon als Trennzeichen):

   ```
   CSV; ID
   jahressonderzahlung-tv-l.csv; sYOIq
   jahressonderzahlung-tvoed-bund.csv; leWiT
   jahressonderzahlung-tvoed-vka.csv; mmryD
   ...
   ```

4. Committe und pushe die Änderung (siehe oben).
5. Starte danach einmal manuell im Actions-Tab (siehe "Fehlerbehebung"). Wichtig: Eine reine Änderung an der Zuordnungsdatei löst die automatische Aktualisierung **nicht** aus — der manuelle Start ist daher nötig.

## Fehlerbehebung

- **Wo sehe ich, ob alles geklappt hat?** Im Actions-Tab deines GitHub-Repositorys. Ein grüner Haken bedeutet: alles gut. Ein rotes X bedeutet: etwas ist schiefgelaufen. Im Log siehst du, welche Chart-ID betroffen ist, mit einer Meldung wie `HTTP 404`.
- **Was bedeutet `HTTP 404`?** Die Chart-ID ist falsch geschrieben oder der Chart gehört zu einem anderen Datawrapper-Konto.
- **Notfallmaßnahme:** Öffne die Chart-URL einmal direkt im Browser. Das erzwingt eine sofortige Aktualisierung der Grafik.

## Einmalige Einrichtung (nur für Tech-Admins)

<details>
<summary>Technische Einrichtung anzeigen</summary>

1. Erstelle auf [app.datawrapper.de/account/api-tokens](https://app.datawrapper.de/account/api-tokens) einen API-Token (Zugriffsschlüssel) mit den Scopes `chart:read`, `chart:write`, `theme:read` und `visualization:read`.
2. Hinterlege den Token als Repository-Secret (geheime Variable) mit dem Namen `DATAWRAPPER_API_TOKEN` unter **Settings → Secrets and variables → Actions**.
3. Technischer Hintergrund: Der Workflow ruft die Datawrapper-API auf (`POST /v3/charts/{ID}/data/refresh` und `POST /v3/charts/{ID}/publish`), um die Charts zu aktualisieren und neu zu veröffentlichen.
4. Die Workflow-Datei liegt unter `.github/workflows/datawrapper-refresh.yml`.

</details>