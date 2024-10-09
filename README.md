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
# Actividad 6
El primer fragmento configura y abre el puerto serial.
###
El segundo fragmento envía un byte a través del puerto serial. Envía datos al microcontrolador.
###
El tercer fragmento lee 4 bytes y los imprime en formato hexadecimal. Recibe datos del microcontrolador.

# Actividad 7
Vas a enviar 2 números en punto flotante desde un microcontrolador a una aplicación en Unity usando comunicaciones binarias. Además, inventa una aplicación en Unity que modifique dos dimensiones de un game object usando los valores recibidos.

En Arduino
```
float number1 = 1.23;
float number2 = 4.56;

void setup() {
  Serial.begin(115200);
}

void loop() {
  // Empaquetar los números flotantes en binario
  byte* data1 = (byte*)&number1; // Convierte el float a un array de bytes
  byte* data2 = (byte*)&number2;

  // Enviar 4 bytes correspondientes a cada número
  for (int i = 0; i < 4; i++) {
    Serial.write(data1[i]);
  }

  for (int i = 0; i < 4; i++) {
    Serial.write(data2[i]);
  }

  delay(1000);  // Envía los datos una vez por segundo
}
```

En Unity
```
using UnityEngine;
using System.IO.Ports;
using System.Threading.Tasks;

public class Mannager : MonoBehaviour
{

    private SerialPort _serialPort;

    private byte[] buffer;
    public GameObject targetObject;  // El objeto cuyo tamaño vamos a modificar

    void Start()
    {
        _serialPort = new SerialPort();
        _serialPort.PortName = "COM5";
        _serialPort.BaudRate = 115200;
        _serialPort.DtrEnable = true;
        _serialPort.NewLine = "\n";
        _serialPort.Open();
        Debug.Log("Open Serial Port");
        buffer = new byte[128];
        _serialPort.ReadTimeout = 1000;  // Tiempo de espera para la lectura
    }

    void Update()
    {
        if (_serialPort.BytesToRead >= 8)  // Verifica si hay al menos 8 bytes disponibles
        {
            // Buffer para almacenar los dos números flotantes
            byte[] buffer = new byte[8];
            _serialPort.Read(buffer, 0, 8);  // Lee 8 bytes del puerto serial

            // Convertir los primeros 4 bytes a un número en punto flotante
            float width = System.BitConverter.ToSingle(buffer, 0);

            // Convertir los siguientes 4 bytes a un número en punto flotante
            float height = System.BitConverter.ToSingle(buffer, 4);

            // Modificar el tamaño del GameObject
            targetObject.transform.localScale = new Vector3(width, height, targetObject.transform.localScale.z);

            Debug.Log("Width: " + width + ", Height: " + height);
        }
    }

    void OnApplicationQuit()
    {
        _serialPort.Close();  // Cierra el puerto serial al salir de la aplicación
    }
}

```

![image](https://github.com/user-attachments/assets/62648f45-6e48-4228-9105-04e74492513e)
![image](https://github.com/user-attachments/assets/e2086c75-011f-49cf-abfa-77e5ee34e3f1)
![image](https://github.com/user-attachments/assets/a45d3f9b-13c6-4fa4-97bb-f9120a2fc8f8)



