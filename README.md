# Wemos y ESP8266 con MicroSD

## Cambio en la libreria SPI.cpp:
Debemos hacer el cambio en la libreria de los pines para que pueda funcionar el modulo y reconozca la micro SD.
Pongo una imagen de como debe quedar el cambio.

![SPI](https://github.com/uagaviria/ESP8266_Wemos_MicroSD/blob/master/imagen/spi.png)

## imagen de conexiones: 

![SPI](https://github.com/uagaviria/ESP8266_Wemos_MicroSD/blob/master/imagen/WEMOSD1.png)




## El programa de arduino:

```cpp
#include "Nextion.h" /*Libreria para Pantalla Nexion*/

// pagina 0
NexButton b0 = NexButton(0,4,"b0");
NexButton b1 = NexButton(0,6,"b1");

//pagina 1
NexButton b0p1 = NexButton(1,1,"b0");

NexTouch *nex_listen_list[] =  // si no adiciona el objeto a esta lista no recibe datos el arduino.
{

    &b0p1,
    &b0,
    &b1,
    NULL
};

uint32_t numeros;
uint32_t valor;
uint32_t valorp1;

void b0PopCallback(void *ptr)
{ 
    Serial.println();
    recvRetNumber(&numeros);
    Serial.print ("Variable numeros = ");
    Serial.print(numeros);
}
   
void b1PopCallback(void *ptr)
{ 
    Serial.println();
    recvRetNumber(&valor);
    Serial.print ("Variable valor = ");
    Serial.print(valor);

}

void b0p1PopCallback(void *ptr)
{ 
    Serial.println();
    recvRetNumber(&valorp1);
    Serial.print ("Variable valor0 pagina 1 = ");
    Serial.print(valorp1);
}

void setup()
{
    nexInit();
    b0.attachPop(b0PopCallback);
    b1.attachPop(b1PopCallback);
    b0p1.attachPop(b0p1PopCallback);
}
void loop()
{
   nexLoop(nex_listen_list);
}       

```


