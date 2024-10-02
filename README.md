# sfi_unidad3

# Actividad 1
## ¿Cómo se ve un protocolo binario?
Un protocolo binario se representa como una secuencia de bits (0s y 1s) que siguen un formato estructurado. Esta secuencia se utiliza para enviar datos de manera eficiente entre dos sistemas. En lugar de usar texto legible por humanos, los mensajes binarios son compactos y requieren menos espacio y tiempo para ser transmitidos.
## ¿Puedes describir las partes de un mensaje?
### Cabecera (Header):
Contiene metadatos sobre el mensaje (como el tipo de mensaje, versión del protocolo, tamaño del mensaje, etc.).
### Cuerpo (Payload):
Contiene los datos principales que se están transmitiendo, como información útil o comandos.
### Checksum o CRC:
Un valor que verifica la integridad del mensaje para detectar errores durante la transmisión.
### Final de Mensaje (Footer):
Un indicador que marca el final del mensaje, si es necesario.
## ¿Para qué sirve cada parte del mensaje?
### Cabecera: 
Proporciona información esencial para que el receptor entienda cómo interpretar el mensaje y su estructura.
### Cuerpo:
Contiene los datos reales que deben procesarse o usarse.
### Checksum/CRC:
Asegura que los datos no han sido corrompidos durante la transmisión, permitiendo al receptor verificar la integridad.
### Final del mensaje:
Indica cuándo termina el mensaje, ayudando al receptor a saber dónde detener la lectura, si no se especifica el tamaño previamente.


# Actividad 4
## ¿En qué endian estamos transmitiendo el número?
Por defecto, los procesadores de las placas Arduino basadas en arquitectura AVR utilizan little-endian.

## Y si queremos transmitir en el endian contrario, ¿Cómo se modifica el código?
Para transmitir el número en formato big-endian, se necesita invertir el orden de los bytes antes de enviarlo. Esto se puede hacer intercambiando las posiciones de los bytes manualmente en un buffer antes de enviarlos.

#  Actividad 5
```
void setup() {
    Serial.begin(115200);
}

void loop() {
    // Definir dos números flotantes
    static float num1 = 3589.3645;
    static float num2 = 123.456;
    static uint8_t arr1[4] = {0};
    static uint8_t arr2[4] = {0};

    if (Serial.available()) {
        if (Serial.read() == 's') {
            // Copiar los bytes de ambos números en los arreglos
            memcpy(arr1, (uint8_t *)&num1, 4);
            memcpy(arr2, (uint8_t *)&num2, 4);

            // Transmitir en formato little-endian
            Serial.println("Transmitting in Little-endian:");
            for (int8_t i = 0; i < 4; i++) {
                Serial.write(arr1[i]);
            }
            for (int8_t i = 0; i < 4; i++) {
                Serial.write(arr2[i]);
            }

            // Transmitir en formato big-endian
            Serial.println("Transmitting in Big-endian:");
            for (int8_t i = 3; i >= 0; i--) {
                Serial.write(arr1[i]);
            }
            for (int8_t i = 3; i >= 0; i--) {
                Serial.write(arr2[i]);
            }
        }
    }
}

```

