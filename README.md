# SymconHP

SymconHP ist eine Erweiterung für die Heimautomatisierung IP Symcon. Diese Erweiterung stellt eine Anbindung an den Homepiloten 1 oder 2 dar. Dabei ist zu beachtet, dass dieses Modul einige Konfiguration vornimmt, welche sich nicht ändern bzw. überschreiben lassen. Damit möchte ich sicher stellen, dass die Aktoren und Sensoren direkt Out-Off-The-Box funktionieren ohne weitere Einrichtungen vornehmen zu müssen.

## Einrichtung

Die Einrichtung erfolgt über die Modulverwaltung von Symcon. Nach der Installation des Moduls sollte der Dienst neugestartet werden. Jetzt kann eine neue Instanz vom Typ "_Homepilot Bridge_" angelegt und konfiguriert werden. Nach dem Starten des "Gräte abgleichs", wird für jeden Aktor (Knoten) oder Sensor eine Instanz in der angegebenen Kategorie bzw. jede Gruppe in der angegebenen Gruppenkategorie angelegt. Ihr müsst somit **nicht** selber für jeden Knoten eine Instanz anlegen.

## Einstellungen

* **Host**  _Der Hostname bzw. die IP-Adresse des Homepiloten_
* **Interval**  _In welchem Abstand soll der Status abgeglichen werden_
* **Homepilot-Knoten**  _In der ausgewählten Kategorie werden die Knoteninstanzen bereit gestellt_
* **Homepilot-Sensoren**  _In der ausgewählten Kategorie werden die Sensorinstanzen bereit gestellt_

**Schalter**

* **Geräte abgleichen** _Es werden für jeden, im Hompiloten angemeldeten Knoten (Aktor) oder Sensor, eine Instanz in der jeweiligen Kategorie angelegt._
* **Status abgleichen** _Manueller Abgleich des Status aller Knoten/Sensoren_

**Linearisierung der Positionen**

Da die prozentualen Positionen der RolloTron Gurtwickler in der Regel nicht mit den realen Positionen der Rollade übereinstimmen gibt es die Möglichkeit die Position durch 3 Stützpunkte zu liniarisieren. Hierzu fährt man die realen Rolladenpositionen 25%, 50% und 75% an und notiert sich die angezeigten Prozentwerte. Dannach werden die notierten Werte in die Konfiguration der einzelnen Knoten eingetragen. Nach der Übernahme der Konfiguration werden die Werte bei der Anzeige und Ansteuerung entsprechend liniarisiert. D.h. möchte man die Rollade zu 50% schließen muss man den Wert nun auch 50% vorgeben.

## Voraussetzung

* Rademacher Homepilot 1 oder 2.
* ab Symcon Version 4

## Funktionen

	// Abgleich aller Knoten
	HP_SyncDevices($bridgeId);

	// Abgleich des Status aller Knoten
	HP_SyncStates($bridgeId);

	// Liefert zu einer UniqueID die passende Lampeninstanz
	HP_GetDeviceByUniqueId($bridgeId, $uniqueId);

	// Abgleich des Status eines Knoten (HP_SyncStates sollte bevorzugewerden,
	// da direkt alle Lampen abgeglichen werden mit nur 1 Request zur Homepiloten)
	HP_RequestData($lightId);

	// Anpassung eines Lampenparameter (siehe SetValues)
	HP_SetValue($lightId, $key, $value);

	// Anpassung eines Parameter
	//
	// Mögliche Keys (ja nach Typ unterschiedlich):
	// "Schaltaktor"
	// SWITCH       -> true oder false für an/aus
	// "RolloTron", 
	// SHUTTER      -> true oder false für geschlossen/offens
	// SHUTTERPOS   -> Werte für eine Rolladenposition zwichen 0 und 100%
	// AUTOMATIC    -> true oder false für Automatik an/aus
	// "Dimmer"
	// DIMMERSTATE  -> true oder false für an/aus
	// DIMMERPOS    -> Werte für eine Helligkeit zwichen 0 und 100%
	// Für Sensoren:
	// SUN          -> Sonner erkannt / nicht erkannt
	// RAIN         -> Regen erkannt / nicht erkannt
	// LUX          -> Lichtwert in Lux
	// WIND         -> Windgweschwindigkeit in m/s
	// TEMPERATURE  -> Temperatur in Grad C
	// SUNHEIGHT    -> Sonnehöhe in Grad
	// SUNDIRECTION -> Sonnenrichtung in Grad
	// ACTTIME      -> Aktualisierungszeit als string

	// Liefert einen Lampenparameter (siehe HP_SetValue)
	HP_GetValue($lightId, $key);
	
	// Weitere Helpergunktionen für Direktverknüpfungen
	HP_SetState($lightId, $value)
	HP_GetState($lightId)
	HP_SetPosition($lightId, $value)
	HP_GetPosition($lightId)
	HP_SetAutomatic($lightId, $value)
	HP_GetAutomatic($lightId)