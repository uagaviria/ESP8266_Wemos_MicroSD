# Wemos y ESP8266 con MicroSD

## Cambio en la libreria SPI.cpp:
Debemos hacer el cambio en la libreria de los pines para que pueda funcionar el modulo y reconozca la micro SD.
Pongo una imagen de como debe quedar el cambio.

![SPI](https://raw.githubusercontent.com/uagaviria/ESP8266_Wemos_MicroSD/master/imagen/spi.png)

## Mas informacion en: http://www.nextionec.com/descargas.html

Quiero compartir un ejemplo cuyos botones en pantalla envian un dato variable. cuenta con dos paginas (page 0 y page 1). Anexo el ejemplo de arduino y el HMI de nextion para que lo abran y lo analicen. la pantalla que estoy usando es de 2.4 pulgadas.
los botones puedes poner las imagenes que quieras, y las variables deben ser globales en el vscope, si estas trabajando la pantalla con mas de una pagina.

![Pantalla nextion](https://github.com/uagaviria/Pantallas-Nextion/blob/master/Imagenes/inicio.png) 
![Pagina 0](https://github.com/uagaviria/Pantallas-Nextion/blob/master/Imagenes/prog1.png) 
![Pagina 0](https://github.com/uagaviria/Pantallas-Nextion/blob/master/Imagenes/prog2.png)
![Pagina 0](https://github.com/uagaviria/Pantallas-Nextion/blob/master/Imagenes/prog3.png)

Presionando n0 pasamos a la pagina 1. (page 1)

![Pagina 1](https://github.com/uagaviria/Pantallas-Nextion/blob/master/Imagenes/prog4.png)

Presionando b1 pasamos a la pagina 0. (page 0)



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


