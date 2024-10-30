## Projeto-Iot-Ariane-Bruna-Khaue

# OTA
 
__include <ESP8266WiFi.h>__  
__include <ArduinoOTA.h>__  
* Bibliotecas utilizadas para conectar o ESP8266 ao Wi-Fi e para atualizar o OTA.

__const char* ssid = "Bruna";  
const char* password = "senha";__  
* O usuário e senha do Wi-Fi;

__//IPAddress local_IP(192, 168, 1, 184);   
//IPAddress gateway(192, 168, 1, 1);  
//IPAddress subnet(255, 255, 255, 0);__  
* IP fixo, gateway e máscara de sub-rede para uma configuração de IP.

## Configuração do dispositivo
__void setup() {__  
  
  __Serial.begin(9600);  
  WiFi.mode(WIFI_STA);  
  WiFi.begin(ssid, password);__  

* Neste trecho é feito a inicialização da comunicação serial, a configuração do ESP8266 para o modo de estação e conecta o ESP8266 à rede Wi-Fi informada anteriormente
  
  __while (WiFi.waitForConnectResult() != WL_CONNECTED) {  
    Serial.println("Conexão falhou, tentando novamente...");  
    WiFi.begin(ssid, password);  
    delay(5000);   
  }__  
* É feito um loop enquanto a conexão não é estabelecida, além disto, existe uma mensagem caso a conexão não funcione. 
* Também existe a tentativa de se reconectar onde ele vai esperar o delay de 5000 para tentar novamente.

## Configuração para atualização do OTA
  __ArduinoOTA.onStart([]() {  
    Serial.println("Iniciando atualização OTA...");  
  });  
  ArduinoOTA.onEnd([]() {  
    Serial.println("\nAtualização concluída.");  
  });  
  ArduinoOTA.onProgress([](unsigned int progress, unsigned int total) {
    Serial.printf("Progresso: %u%%\r", (progress / (total / 100)));  
  });  
  ArduinoOTA.onError([](ota_error_t error) {  
    Serial.printf("Erro[%u]: ", error);  
    if (error == OTA_AUTH_ERROR) Serial.println("Falha na autenticação");  
    else if (error == OTA_BEGIN_ERROR) Serial.println("Falha no início");  
    else if (error == OTA_CONNECT_ERROR) Serial.println("Falha na conexão");  
    else if (error == OTA_RECEIVE_ERROR) Serial.println("Falha no recebimento");  
    else if (error == OTA_END_ERROR) Serial.println("Falha no finalização");  
  });__  

  * A primeira função é chamada quando a atualização do OTA inicia, a segunda é chamada quando a atualização finaliza.

  * A terceira função é chamada quando está sendo feita a atualização do OTA, onde ele vai exibir o progresso e a porcentagem da atualização.

  * A quarta função é chamada quando ocorre algum erro durante a atualização do OTA, onde elas terão as seguintes atribuições:
    - Exibir erro de autenticação;
    - Exibir erro quando iniciado a atualização;
    - Exibir erro de conexão;
    - Exibir erro ao receber informações;
    - Exibir erro ao finalizar a atualização.

## Inicializar o serviço OTA
__ArduinoOTA.begin();  
  Serial.println("Pronto para atualizações OTA.");  
  Serial.print("Endereço IP: ");  
  Serial.println(WiFi.localIP());__  

* Serve para inicializar o serviço do OTA e apresentar o endereço IP do dispositivo após conexão com o Wi-Fi.

## Inicializar o servidor Telnet
__server.begin();
  server.setNoDelay(true);
  Serial.println("Servidor Telnet iniciado...");
}__

* Serve para configurar o servidor Telnet para envio de dados.

## Deixar o serviço OTA ativo
__void loop() {  
  ArduinoOTA.handle();  
}__  

* Mantém o serviço do OTA ativo.

---
# Telnet 
↓ Incluindo bibliotecas para funcionalidades WiFi e atualização OTA  
  **include <ESP8266WiFi.h>**  
  **include <ArduinoOTA.h>**  

↓ Configuração da rede WiFi  
  **const char* ssid = "XuXa";**
             
↓ Nome da rede WiFi  
  **const char* password = "soparabaixinhosrsrs";** *Senha da rede WiFi*  

↓ Definição do pino onde o LED está conectado  
  **const int LED_PIN = 5; // D1**  

↓ Inicializando servidor Telnet na porta 23  
  **WiFiServer server(23);**  
  **WiFiClient client;**  
  **void setup() {**  

↓ Configurando o pino do LED como saída  
  **pinMode(LED_PIN, OUTPUT);**  
 
↓ Inicializando comunicação serial  
  **Serial.begin(9600);**  
 
↓ Conectando à rede WiFi no modo estação (STA)
  **WiFi.mode(WIFI_STA);**  
  **WiFi.begin(ssid, password);**  
 
↓ Tentativa de conexão com WiFi  
  **while (WiFi.waitForConnectResult() != WL_CONNECTED) {**  
    **Serial.println("Conexão falhou, tentando novamente...");**  
    **WiFi.begin(ssid, password);**  
    **delay(5000);** ↓ Aguarda 5 segundos antes de tentar novamente  
  }  

↓ Configuração da atualização OTA  
  **ArduinoOTA.onStart([]() {**  
    **Serial.println("Iniciando atualização OTA...");**  
  });  
  **ArduinoOTA.onEnd([]() {**  
    **Serial.println("\nAtualização concluída.");**  
  });  
  **ArduinoOTA.onProgress([](unsigned int progress, unsigned int total) {**  
    **Serial.printf("Progresso: %u%%\r", (progress / (total / 100)));**  
  });  
  **ArduinoOTA.onError([](ota_error_t error) {**  
    **Serial.printf("Erro[%u]: ", error);**
    **if (error == OTA_AUTH_ERROR) Serial.println("Falha na autenticação");**  
    **else if (error == OTA_BEGIN_ERROR) Serial.println("Falha no início");**  
    **else if (error == OTA_CONNECT_ERROR) Serial.println("Falha na conexão");**  
    **else if (error == OTA_RECEIVE_ERROR) Serial.println("Falha no recebimento");**
    **else if (error == OTA_END_ERROR) Serial.println("Falha na finalização");**  
  });  

↓ Inicia o serviço OTA  
  **ArduinoOTA.begin();**  
  **Serial.println("Pronto para atualizações OTA.");**  
  **Serial.print("Endereço IP: ");**  
  **Serial.println(WiFi.localIP());**

↓ Inicia o servidor Telnet e define sem atraso de resposta  
  **server.begin();**  
  **server.setNoDelay(true);**  
  **Serial.println("Servidor Telnet iniciado...");**  
}  

**void loop() {**  
↓ Verifica e executa o processo de atualização OTA  
  **ArduinoOTA.handle();**  
   
↓ Aceita conexão de clientes Telnet  
  **if (server.hasClient()) {**  
    **if (!client || !client.connected()) {**  
      **if (client) client.stop(); ↓ Encerra conexão com cliente anterior, se existir**
      **client = server.available(); ↓ Aceita novo cliente**  
      **Serial.println("Cliente conectado via Telnet.");**  
      **client.println("Digite '1' para ligar o LED e '0' para desligar o LED.");**  
    **} else {**  
      **server.available().stop(); ↓ Encerra conexão extra, mantendo um único cliente**  
    }  
  }  
 
↓ Comandos do cliente para controle do LED  
  **if (client && client.connected() && client.available()) {**  
    **char command = client.read();**  
    **if (command == '1') {**  
      **digitalWrite(LED_PIN, LOW);** ↓ Liga o LED (LOW pois o LED é invertido)  
      **client.println("LED ligado.");**  
      **Serial.println("LED ligado.");**  
    **} else if (command == '0') {**  
      **digitalWrite(LED_PIN, HIGH); ↓ Desliga o LED**  
      **client.println("LED desligado.");**  
      **Serial.println("LED desligado.");**  
    **} else {**  
      **client.println("Comando inválido. Digite '1' para ligar o LED e '0' para desligar o LED.**");  
    }  
  }  
}  

  