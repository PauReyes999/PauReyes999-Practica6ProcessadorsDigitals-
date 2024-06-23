# Práctica 6: Comunicación SPI con ESP32

## Objetivo

El objetivo de esta práctica es comprender el funcionamiento del bus SPI y su implementación en el microcontrolador ESP32.

## Introducción al SPI

El bus de Interfaz Periférica Serial (SPI) es una especificación de interfaz de comunicación serial síncrona utilizada para comunicaciones de corta distancia, principalmente en sistemas embebidos. Utiliza una arquitectura maestro-esclavo con un mínimo de tres líneas para la comunicación:

- **MOSI (Master Out Slave In)**: Línea de datos del maestro al esclavo.
- **MISO (Master In Slave Out)**: Línea de datos del esclavo al maestro.
- **SCK (Serial Clock)**: Señal de reloj generada por el maestro.
- **SS (Slave Select)**: Línea de selección del esclavo.

## SPI en ESP32

El microcontrolador ESP32 proporciona múltiples interfaces SPI:

- **VSPI**: Pines por defecto son GPIO 23 (MOSI), GPIO 19 (MISO), GPIO 18 (SCK), GPIO 5 (SS).
- **HSPI**: Pines por defecto son GPIO 13 (MOSI), GPIO 12 (MISO), GPIO 14 (SCK), GPIO 15 (SS).

Para ESP32-S3, existen buses adicionales con sus respectivas asignaciones de pines por defecto:

- **SPI2**: GPIO 35 (MOSI), GPIO 37 (MISO), GPIO 36 (SCK), GPIO 39 (SS).
- **SPI3**: GPIO 11 (MOSI), GPIO 13 (MISO), GPIO 12 (SCK), GPIO 10 (SS).

## Ejercicios Prácticos

### Ejercicio 1: Lectura/Escritura en Tarjeta SD

**Hardware Requerido**: Módulo de tarjeta SD.

**Conexiones**:
- Módulo SD `CS` a GPIO 5
- Módulo SD `MOSI` a GPIO 23
- Módulo SD `MISO` a GPIO 19
- Módulo SD `SCK` a GPIO 18
- Conexiones de alimentación (VCC y GND)

**Código**:
```cpp
#include <SPI.h>
#include <SD.h>

File myFile;

void setup() {
  Serial.begin(115200);
  if (!SD.begin(5)) {
    Serial.println("Initialization failed!");
    return;
  }
  Serial.println("Initialization done.");

  myFile = SD.open("test.txt", FILE_WRITE);
  if (myFile) {
    Serial.println("Writing to test.txt...");
    myFile.println("Hello, ESP32!");
    myFile.close();
    Serial.println("Done.");
  } else {
    Serial.println("Error opening test.txt");
  }

  myFile = SD.open("test.txt");
  if (myFile) {
    Serial.println("test.txt:");
    while (myFile.available()) {
      Serial.write(myFile.read());
    }
    myFile.close();
  } else {
    Serial.println("Error opening test.txt");
  }
}

void loop() {
  // Nothing to do here.
}
