# World Record Exoskeleton
// This is a senior design project still in progress. This repo is where the control system will live.

// ![img](https://github.com/ARLab-VT/world-record-exoskeleton/blob/main/CAD.jpg)

#include <ODriveUART.h>
#include <SoftwareSerial.h>

// setting up Odrive for UART
odrv0.config.uart_a_baudrate = 19200 # 19200 if use Arduino UNO, 115200 if use a hardware serial port
odrv0.config.enable_uart_a = True
odrv0.config.gpio12_mode = GpioMode.UART_A
odrv0.config.gpio13_mode = GpioMode.UART_A
odrv0.save_configuration()


// pin 8: RX - connect to ODrive TX
// pin 9: TX - connect to ODrive RX
SoftwareSerial odrive_serial(8, 9);
int baudrate = 19200; // Must match what you configure on the ODrive (see docs for details)

ODriveUART odrive(odrive_serial);

void setup() {
  odrive_serial.begin(baudrate);

  Serial.begin(115200); // Serial to PC
  
  delay(10);

  Serial.println("Waiting for ODrive...");
  while (odrive.getState() == AXIS_STATE_UNDEFINED) {
    delay(100);
  }

  Serial.println("found ODrive");
  
  Serial.print("DC voltage: ");
  Serial.println(odrive.getParameterAsFloat("vbus_voltage"));
  
  Serial.println("Enabling closed loop control...");
  while (odrive.getState() != AXIS_STATE_CLOSED_LOOP_CONTROL) {
    odrive.clearErrors();
    odrive.setState(AXIS_STATE_CLOSED_LOOP_CONTROL);
    delay(10);
  }
  
  Serial.println("ODrive running!");
}

void loop() {
  odrive.setPosition(
  
  );

  ODriveFeedback feedback = odrive.getFeedback();
  Serial.print("pos:");
  Serial.print(feedback.pos);
  Serial.print(", ");
  Serial.print("vel:");
  Serial.print(feedback.vel);
  Serial.println();
}



