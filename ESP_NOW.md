# Configuration:

  ### Include Libraries:
  ```c
  #include <esp_now.h>
  #include <WiFi.h>
  ```
  ### Message Structure:
  
  ```c
  typedef struct struct_message {
    char header[50];
    char content[200];
  } struct_message;
  ```
  
  ### Receiver Mac Address:
  
  ```cc
  uint8_t receiverMacAddress[] = {0x00, 0x00, 0x00, 0x00, 0x00, 0x00};
  ```
  
  ### Setup:
  
  ```c
  
  //Set WiFi to STA (Station) mode
  WiFi.mode(WIFI_STA);
  
  // Initialize ESP-NOW
  if (esp_now_init() != ESP_OK) {
    Serial.println("Error initializing ESP-NOW");
    return;
  }
  ```

---

# Simple Communication:


  ### Add the receiver as a peer:
  ```c
  esp_now_peer_info_t peerInfo;
  
    //encrypt the message
    memcpy(peerInfo.peer_addr, receiverMacAddress, sizeof(receiverMacAddress));
  
    peerInfo.channel = 0;  // Use the current channel
    peerInfo.encrypt = false;  // No encryption
  
    if (esp_now_add_peer(&peerInfo) != ESP_OK) {
      Serial.println("Failed to add peer");
      return;
    }
  ```
  - Put it in Setup() function


  ### Send Message:
  ```c
  struct_message xmsg = {"Info", "Hello from ESP32!"};
  
  esp_err_t result = esp_now_send(receiverAddress, (uint8_t *)&xmsg, sizeof(xmsg));

  //result
  if (result == ESP_OK) {
    Serial.println("Message sent successfully");
  }
  else {
    Serial.println("Error sending message");
  }


  ```


  ### Callback Function Register:
  ```c
  esp_now_register_recv_cb(OnDataReceived);
  ```
  - Put it in Setup() function
  
  
  ### (Callback) Receiver Function:
  ```cc
  void OnDataReceived(const uint8_t *mac, const uint8_t *data, int len) {

    // Copy received data into the msg structure
    memcpy(&msg, data, sizeof(msg));
  
    //msg.header, msg.content
  }
  ```
  - OnDataReceived(mac, data, len)
