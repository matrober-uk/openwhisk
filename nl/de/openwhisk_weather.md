---

copyright:
  years: 2016, 2017
lastupdated: "2017-06-01"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Weather-Paket verwenden
{: #openwhisk_catalog_weather}

Das Paket `/whisk.system/weather` bietet eine komfortable Methode zum Aufrufen der Weather Company Data for IBM Bluemix-API.

Das Paket enthält die folgende Aktion.

| Entität | Typ | Parameter | Beschreibung |
| --- | --- | --- | --- |
| `/whisk.system/weather` | Paket | username, password | Services der Weather Company Data for IBM Bluemix-API  |
| `/whisk.system/weather/forecast` | Aktion | latitude, longitude, timePeriod | Vorhersage für angegebenen Zeitraum|

Es wird empfohlen, eine Paketbindung mit den Werten `username` und `password` zu erstellen. Auf diese Weise brauchen Sie die Berechtigungsnachweise nicht jedes Mal anzugeben, wenn Sie die Aktionen im Paket aufrufen.

## Weather-Paket in Bluemix einrichten

Wenn Sie OpenWhisk von Bluemix verwenden, erstellt OpenWhisk automatisch Paketbindungen für Ihre Bluemix Weather-Serviceinstanzen.

1. Erstellen Sie eine Serviceinstanz von Weather Company Data in Ihrem Bluemix-[Dashboard](http://console.ng.Bluemix.net).
  
  Stellen Sie sicher, dass Sie sich den Namen der Serviceinstanz sowie der Bluemix-Organisation und den Bereich merken, in dem Sie sich befinden.
  
2. Aktualisieren Sie die Pakete in Ihrem Namensbereich. Die Aktualisierung erstellt automatisch eine Paketbindung für die Serviceinstanz von Weather Companty Data, die Sie erstellt haben.
  
  ```
  wsk package refresh
  ```
  {: pre}
  
  
  ```
  created bindings:
  Bluemix_Weather_Company_Data_Credentials-1
  ```
  ```
  wsk package list
  ```
  {: pre}
  ```
  packages
  /myBluemixOrg_myBluemixSpace/Weather Bluemix_Weather_Company_Data_Credentials-1 private
  ```
  
 
## Weather-Paket außerhalb von Bluemix einrichten

Wenn Sie OpenWhisk nicht in Bluemix verwenden oder wenn Sie den Weather Company Data-Service außerhalb von Bluemix einrichten möchten, müssen Sie manuell eine Paketbindung für Ihren Weather Company Data-Service erstellen. Sie benötigen hierzu den Benutzernamen und das Kennwort für den Weather Company Data-Service.

- Erstellen Sie eine Paketbindung, die für Ihren Watson Translator-Service konfiguriert ist.

  ```
  wsk package bind /whisk.system/weather myWeather -p username MYUSERNAME -p password MYPASSWORD
  ```
  {: pre}


## Wettervorhersage für einen Standort abrufen
{: #openwhisk_catalog_weather_forecast}

Die Aktion `/whisk.system/weather/forecast` gibt eine Wettervorhersage für einen Standort durch einen Aufruf der API für The Weather Company zurück. Die folgenden Parameter sind verfügbar:

- `username`: Der Benutzername für Weather Company Data for IBM Bluemix, der berechtigt ist, die API für die Vorhersage aufzurufen.
- `password`: Das Kennwort für Weather Company Data for IBM Bluemix, das berechtigt ist, die API für die Vorhersage aufzurufen.
- `latitude`: Die Breitengradkoordinate des Standorts.
- `longitude`: Die Längengradkoordinate des Standorts.
- `timePeriod`: Der Zeitraum für die Vorhersage. Gültige Optionen sind:
  - `10day` (Standardwert) Gibt eine tägliche 10-Tage-Vorhersage zurück
  - `48hour` - Gibt eine stündliche 2-Tages-Vorhersage zurück
  - `current` - Gibt die aktuellen Wetterbedingungen zurück
  - `timeseries` - Gibt sowohl die aktuellen Beobachtungen als auch vergangene Beobachtungen rückwirkend bis zu 24 Stunden ab dem aktuellen Zeitpunkt (Datum und Uhrzeit) zurück.


- Das folgende Beispiel zeigt die Erstellung einer Paketbindung und den anschließenden Abruf einer 10-Tage-Vorhersage. 1. Erstellen Sie eine Paketbindung mit Ihrem API-Schlüssel.
  
  ```
  wsk package bind /whisk.system/weather myWeather --param username MY_USERNAME --param password MY_PASSWORD
  ```
  {: pre}

- Rufen Sie die Aktion `forecast` in Ihrer Paketbindung auf, um die Wettervorhersage abzurufen.
  
  ```
  wsk action invoke myWeather/forecast --result \
  --param latitude 43.7 \
  --param longitude -79.4
  ```
  {: pre}
  ```json
  {
      "forecasts": [
          {
              "dow": "Wednesday",
              "max_temp": -1,
              "min_temp": -16,
              "narrative": "Chance of a few snow showers. Highs -2 to 0C and lows -17 to -15C.",
              ...
          },
          {
              "class": "fod_long_range_daily",
              "dow": "Thursday",
              "max_temp": -4,
              "min_temp": -8,
              "narrative": "Mostly sunny. Highs -5 to -3C and lows -9 to -7C.",
              ...
          },
          ...
      ],
  }
  ```
  
