# Node-MCU
/* Comment this out to disable prints and save space */
#define BLYNK_PRINT Serial

/* Fill-in your Template ID (only if using Blynk.Cloud) */
//#define BLYNK_TEMPLATE_ID   "YourTemplateID"


#include <ESP8266WiFi.h>
#include <BlynkSimpleStream.h>

// Your WiFi credentials.
// Set password to "" for open networks.
const char* ssid = "No.1";
const char* pass = "what`syourname";

// You should get Auth Token in the Blynk App.
// Go to the Project Settings (nut icon).
char auth[] = "9qnt9lH46hkBjuhoZGFyWxGQX9m2cCU_";

WiFiClient wifiClient;

// This function tries to connect to the cloud using TCP
bool connectBlynk()
{
  wifiClient.stop();
  return wifiClient.connect(BLYNK_DEFAULT_DOMAIN, BLYNK_DEFAULT_PORT);
}

// This function tries to connect to your WiFi network
void connectWiFi()
{
  Serial.print("Connecting to ");
  Serial.println(ssid);

  if (pass && strlen(pass)) {
    WiFi.begin((char*)ssid, (char*)pass);
  } else {
    WiFi.begin((char*)ssid);
  }

  while (WiFi.status() != WL_CONNECTED) {
    delay(500);
    Serial.print(".");
  }
}


void setup()
{
  // Debug console
  Serial.begin(9600);

  connectWiFi();

  connectBlynk();

  Blynk.begin(wifiClient, auth);
}

void loop()
{
  // Reconnect WiFi
  if (WiFi.status() != WL_CONNECTED) {
    connectWiFi();
    return;
  }

  // Reconnect to Blynk Cloud
  if (!wifiClient.connected()) {
    connectBlynk();
    return;
  }

  Blynk.run();
}
