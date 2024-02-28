# Xanela-coche-1
Codigo para funcionamento xanela coche
/************************************************************************+************************************************************************
*  Imos imitar o funcionamento simplificado dunha xanela dun coche.
*  Cando prememos unha vez, o pulsador activa un motor que move o vidro cara arriba; ao premer unha segunda vez, 
*  o pulsador activa outro motor que move o vidro cara abaixo. Tanto as entradas como as saídas son dixitais, 
*  polo que imos activar os motores mediante dous relés. Cada un dos motores para ao estar aceso 7 segundos. 
*  Durante ese tempo, o pulsador pode en calquera momento parar o motor activo e activar o que está parado 
*  (volver atrás, sen pasar por paro). É dicir, hai que asegurar que o pulsador é reactivo ante calquer accionamento do usuario.
*  Imos facer a montaxe dos dous relés e o pulsador, asegurando que non haxa posibilidade de queimar a placa Arduíno mediante unha sobrecarga .
*  En particular teremos de protexer o pulsador, de maneira que ante un erro de programación, non se chegue a pór en curto o pin de entrada. 
*  O As resistencias de protección deben ter valores comerciais e soportar a potencia empregada.
*  pin debe estar protexido para funcionar a máximo 20 mA e dar un ‘1’ lóxico ao chegarlle mínimo 3.5 V. 
* 
*  Autor: Javier Figueiro Resúa
*  Data: Febreiro 2024
*
*************************************************************************************************************************************************/
 

//Definir pins de entrada e saída
const int pinEntrada = 13;
const int pinSalida1 = 8;
const int pinSalida2 = 7;

//Variabel para almacenar o estado da entrada
int estadoEntradaAnterior = LOW;
//Variabel para controlar que a saída está activa
int salidaActiva = 0;
//Variabel para controlar o tempo das saídas activas
int contadortiempo = 7000;
void setup(){
  //Iniciar os pins
  pinMode(pinEntrada, INPUT);
  pinMode(pinSalida1, OUTPUT);
  pinMode(pinSalida2, OUTPUT);
}

void loop() {
  //Ler o estado actual da entrada
  int estadoEntrada = digitalRead(pinEntrada);
  //Contar o tempo para desactivar as salidas
  if (contadortiempo>=7000){
    digitalWrite(pinSalida2, LOW);
    digitalWrite(pinSalida1, LOW);
  }
  else contadortiempo=contadortiempo+10;
  
  //Si a entrada cambiou de baixo a alto
  if (estadoEntrada == HIGH && estadoEntradaAnterior == LOW) {
    //Cambiar estado saídas según estado saída activa
    if (salidaActiva == 0) {
      digitalWrite(pinSalida1, HIGH);
      digitalWrite(pinSalida2, LOW);
      salidaActiva = 1;
    } else {
      digitalWrite(pinSalida1, LOW);
      digitalWrite(pinSalida2, HIGH);
      salidaActiva = 0;
    }
    contadortiempo = 0;
  }
  delay (10);
  //actualizamos estado anterior da entrada
  estadoEntradaAnterior = estadoEntrada;
}
  
