# __Programm zur Ansteuerung des Fischer FMP100 in einer Baan-Session (Infor ERP)__


__Idee__

Das Programm fmp100.exe verbindet sich mit dem über USB verbunden Schichtdickenmessgerät Fischer FMP100 und wandelt dessen Daten in eine XML-Datei um, die von einer aufgerufenen Baan-Session eingelesen und verarbeitet wird.
## Start des






## Kommandozeilenargumente
	
* [--help, --h] Listet alle zulässigen Kommandozeilenargumente und ihre Funktionsbeschreibung ein
* [--start_time, --s] Angabe der Startzeit des Programms als Timestamp. Dieser wird von der Baan-Session erzeugt und an das aufrufende Programm übergeben.
* [--console, --c] Startet des Programm im interaktiven Konsolenmodus





## Interaktiver Konsolenmodus

Durch den Start des Programms im interaktiven Konsolenmodus kann über spezifische COM-Port-Befehle eine direkte Kommunikation mit dem Fischer FMP100 hergestellt werden.
Die Rückantwort des Gerätes wird direkt in der Konsole ausgegeben.

Momentan stehen folgende Befehle zur Verfügung:

* [VV] Gibt den Namen des Gerätes und die verwendete Firmwareversion aus.
* [NAMHEX]
* [PE] Angabe des für die Messapplikation konfigurierten Gruppenseparators. Folgende Gruppenseparatoren können über die COM-Port-Einstellungen des Gerätes eingestellt werden: GS (Hex code 0x1d), "*", ";", "#", ":" und ","
* [SAM] Gibt alle Daten der aktuellen Messapplikation entsprechend der COM-Port-Einstellungen und der Blockergebnisvorlage aus
* [DAT0-DATxxx] Gibt Datum und Uhrzeit der Erstellung eines Messblocks xxx aus. Der erste Block beginnt entsprechend mit DAT0
* unbekannte bzw. falsche Steuerbefehle liefern als Antwort ein Fragezeichen zurück [?]
* Die Bedeutung fOlgende Steuerbefehle ist noch nicht bekannt: SL, LSL, USL


Der interaktive Konsolenmodus kann durch die Eingabe des Steuerbefehls [exit] beendet werden.


## XML-Struktur

Das Programm fmp100 erzeugt aus der aktuell gewählten Messapplikation eine XML-Datei mit den Elementen "application", "block" und "value".

1. Das Element "application" ist das Wurzelelement der XML-Datei und beinhaltet alle relevanten Daten zur Messapplikation (Name der Messanwendung, obere und untere Toleranzgrenze sowie die verwendete Messeinheit)
2. Das Element "block" beinhaltet alle Daten für die Beschreibung eines Messblockes (u.a. Auftragsnummer, Blockkommentar, Anzahl der Messwerte und Zeitpunkt der Erstellung zerlegt in Tag, Monat, Jahr, Stunden und Minuten)
3. Das Element "value" beinhaltet den Zahlenwert des Messwertes


Anmerkung: Auftragsnummer und Kommentar können jeweils leer sein.


Die folgende Datei zeigt eine Beispielanwendung  "Farbschichtmessung" mit 3 Messblöcken:

	<?xml version="1.0" encoding="utf-8" standalone="yes" ?>
	<application name="Farbschichtmessung" max="2000" min="455" unit="um">
	<block ordernumber="121987654" day="16" month="2" year="2012" hour="16" minute="0" comment="Kommentar 1" amount="16">
	<value>3.59143</value>
        <value>0.978486</value>
        <value>2.68681</value>
        <value>0.0290042</value>
        <value>3.08025</value>
        <value>1.30557</value>
        <value>2.41982</value>
        <value>1.13978</value>
        <value>0.873436</value>
        <value>1.43767</value>
        <value>1.45676</value>
        <value>1.47592</value>
        <value>0.856119</value>
        <value>3.24752</value>
        <value>2.70945</value>
        <value>2.77777</value>
    </block>
    <block ordernumber="" day="17" month="2" year="2012" hour="8" minute="46" comment="Kommentar 2" amount="14">
        <value>2.3154</value>
        <value>2.31013</value>
        <value>1.13798</value>
        <value>1.49795</value>
        <value>1.38427</value>
        <value>1.83822</value>
        <value>0.685929</value>
        <value>0.908234</value>
        <value>0.804493</value>
        <value>0.925715</value>
        <value>1.61164</value>
        <value>2.07399</value>
        <value>2.2901</value>
        <value>2.34477</value>
    </block>
    <block ordernumber="" day="20" month="2" year="2012" hour="7" minute="5" comment="" amount="4">
        <value>3.4179</value>
        <value>1.67061</value>
        <value>1.32427</value>
        <value>3.20199</value>
    </block>
</application>
