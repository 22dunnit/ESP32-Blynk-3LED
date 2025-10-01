# 🔌 Controllo di 3 LED con ESP32 tramite Blynk IoT

## 📍 Descrizione del Progetto

Questo progetto dimostra come controllare **tre LED** (rosso, giallo e verde) utilizzando una **scheda ESP32** connessa tramite **Wi-Fi** e gestita da remoto tramite l’app **Blynk IoT**. È un esempio pratico di applicazione **IoT** (Internet of Things) per imparare a interfacciare hardware e software attraverso un’app mobile.

---

## 🧰 Materiali Utilizzati

| Quantità | Componente         | Descrizione                              |
|----------|--------------------|------------------------------------------|
| 1        | ESP32              | Microcontrollore con Wi-Fi integrato     |
| 3        | LED (Rosso, Giallo, Verde) | Indicatori luminosi                  |
| 3        | Resistenze da 220Ω | Per protezione corrente LED              |
| 1        | Breadboard         | Per i collegamenti temporanei            |
| 6+       | Cavi jumper        | Collegamenti elettrici                   |
| 1        | Smartphone          | Con app Blynk installata                 |
| 1        | Accesso Wi-Fi      | Rete Wi-Fi per la ESP32                  |

---

## 🖼️ Schema Elettrico

### 📌 Collegamenti LED ↔️ ESP32

| LED    | Colore  | Pin ESP32 (GPIO) | Collegamento  |
|--------|---------|------------------|----------------|
| LED1   | Rosso   | GPIO 13          | In serie con resistenza da 220Ω verso GND |
| LED2   | Giallo  | GPIO 12          | In serie con resistenza da 220Ω verso GND |
| LED3   | Verde   | GPIO 14          | In serie con resistenza da 220Ω verso GND |

> 🔧 Ogni LED è connesso in modalità **attiva alta**: il pin GPIO fornisce 3.3V quando è HIGH, accendendo il LED.

### 📷 Immagine del Circuito

![Schema del circuito](https://imgur.com/a/TYNPWQx)

---

## 📱 Configurazione dell’App Blynk

1. **Crea un nuovo progetto** su [Blynk IoT](https://blynk.io/)
2. Seleziona **ESP32 Dev Board** come dispositivo
3. Inserisci il **nome del progetto**: `3LED CONTROL`
4. Riceverai via email un **Auth Token** da usare nel codice
5. Nell’interfaccia dell’app, aggiungi **3 pulsanti (switch)**:
   - Pulsante 1 → **V0** (LED rosso)
   - Pulsante 2 → **V1** (LED giallo)
   - Pulsante 3 → **V2** (LED verde)
6. Imposta ogni pulsante in **modalità switch (ON/OFF)**

---

## 💻 Codice Arduino (ESP32)

```cpp
#define BLYNK_TEMPLATE_ID "TMPLPmOtjTT5"
#define BLYNK_TEMPLATE_NAME "3LED CONTROL"
#define BLYNK_AUTH_TOKEN "1syCx1QQtA5TcxaehbY77Nsn_gzgP5Cl"

#define BLYNK_PRINT Serial

#include <WiFi.h>
#include <WiFiClient.h>
#include <BlynkSimpleEsp32.h>

// Credenziali di rete
char auth[] = BLYNK_AUTH_TOKEN;
char ssid[] = "Wokwi-GUEST";  // Nome rete Wi-Fi
char pass[] = "";             // Nessuna password (rete aperta)

BlynkTimer timer;

// LED GPIO Pin
#define LED1 13  // Rosso
#define LED2 12  // Giallo
#define LED3 14  // Verde

// Callback per Blynk Virtual Pins
BLYNK_WRITE(V0) {
  int value = param.asInt();
  digitalWrite(LED1, value);
  Serial.println(value ? "RED ON" : "RED OFF");
}

BLYNK_WRITE(V1) {
  int value = param.asInt();
  digitalWrite(LED2, value);
  Serial.println(value ? "YELLOW ON" : "YELLOW OFF");
}

BLYNK_WRITE(V2) {
  int value = param.asInt();
  digitalWrite(LED3, value);
  Serial.println(value ? "GREEN ON" : "GREEN OFF");
}

void setup() {
  pinMode(LED1, OUTPUT);
  pinMode(LED2, OUTPUT);
  pinMode(LED3, OUTPUT);

  Serial.begin(9600);
  Blynk.begin(auth, ssid, pass);  // Connessione a Wi-Fi e Blynk
}

void loop() {
  Blynk.run();   // Mantiene la connessione attiva
  timer.run();   // Gestisce eventuali timer futuri
}
