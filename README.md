# 📺✨ ESP32 Ambilight Sync met WLED-strip
Deze Arduino-code maakt het mogelijk om een WLED-strip te synchroniseren met de kleuren van een Philips TV met Ambilight-ondersteuning via het netwerk. Het project draait op een ESP32- (of ESP8266-)board en leest de Ambilight-kleurwaarden van de TV uit via de interne JSON API. Vervolgens stuurt het deze kleuren realtime door naar een WLED-controller.

## 🚀 Features
- Realtime synchronisatie van Ambilight naar WLED
- Ondersteuning voor `ESP32` en `ESP8266`
- Maakt gebruik van de JSON API van Philips TV (`http://<TV_IP>:1925/6/ambilight/processed`)
- Eenvoudig aanpasbaar voor meerdere zones en LEDs

## 📦 Bestandsoverzicht
| Bestand             | Omschrijving |
|---------------------|--------------|
| `hue.ico` | firmware voor je ESP32-board |

## 📦 Benodigdheden
- `ESP32` of `ESP8266` microcontroller
- WLED-compatibele LED-strip met netwerktoegang
- Philips TV met Ambilight en ingeschakelde JSON API (`http://<TV_IP>:1925/6/ambilight/processed`)

## ⚙️ Installatie
Verander de gegevens in `hue.ico` naar eigen inzicht.
```C++
const char* ssid = "JouwWiFiNaam";
const char* password = "JouwWiFiWachtwoord";
const char* AmbilightSource = "http://<TV_IP>:1925/6/ambilight/processed";
const char* WLEDSource = "http://<WLED_IP>/json";
```    
### 🧠 Hoe het werkt
1. Ophalen Ambilight-data:
   - Elke ~400ms wordt een HTTP GET-verzoek gestuurd naar de Philips TV (`http://<TV_IP>:1925/6/ambilight/processed`).<br/> De response bevat RGB-waarden van LED-segmenten aan de:
     - linkerzijde
     - bovenzijde
     - rechterzijde van de TV
    
2. JSON Parsing:
   - De JSON-respons wordt geparsed met ArduinoJson en de RGB-waarden worden eruit gehaald.
3. WLED aansturen:
   - Een HTTP POST-verzoek wordt gestuurd naar de WLED-controller. Deze bevat een JSON-payload die de LED-strip opdraagt om te veranderen naar de opgehaalde Ambilight-kleuren.<br/>De LED-strip is in 3 segmenten verdeeld:
     - Segment 0: Links (LEDs 0–10)
     - Segment 1: Boven (LEDs 10–26)
     - Segment 2: Rechts (LEDs 26–35)

### 🧩 Voorbeeld JSON WLED Payload
```json
{
  "on": true,
  "bri": 65,
  "transition": 2,
  "mainseg": 0,
  "seg": [
    { "id": 0, "start": 0, "stop": 10, "col": [[R, G, B], [0, 0, 0], [0, 0, 0]] },
    { "id": 1, "start": 10, "stop": 26, "col": [[R, G, B], [0, 0, 0], [0, 0, 0]] },
    { "id": 2, "start": 26, "stop": 35, "col": [[R, G, B], [0, 0, 0], [0, 0, 0]] }
  ]
}
``` 
📸 Demo
- [x] fimpje gemaakt
- [ ] film omzetten naar gif bestand :tada:
