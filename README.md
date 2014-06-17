Esquivaobjetos
==============
/*
Coche esquiva-objetos
 */

#include <Servo.h> 
const int echoPin=7;
const int trigPin=8;
float distance=0;
Servo myservo; //crea un "objeto servo" para controlar un servo
int valorCentro=0;
int valorDerecha=0;
int valorIzquierda=0;
int IN1=2; //motor derecho
int IN2=3; //motor derecho
int IN3=4; //motor izquierdo
int IN4=5; //motor izquierdo

void setup() 
{ 
  myservo.attach(11); // Servo conectado al pin  9
  Serial.begin(9600); // inicializa el puerto serial
  pinMode(trigPin, OUTPUT); // trig salida
  pinMode(echoPin, INPUT);  // echo entrada
  myservo.write(90); //servo mirando adelante
} 



int medir(){
  digitalWrite(trigPin, LOW); // al principio esta "callado"
  delayMicroseconds(5); // espera 5 microsegundos
  digitalWrite(trigPin, HIGH); // emite un sonido durate 10 microsegundos
  delayMicroseconds(10); // espera 10 microsegundos
  digitalWrite(trigPin, LOW); //se apaga
  //lo que tarda en llegar el sonido al sensor lo multiplica por 0,0175 que es la distancia que hay hasta llegar al coche.
  distance = pulseIn(echoPin, HIGH); 
  distance = distance*0.0175;// se multiplica por 0,0175 porque el sonido viaja a una velocidad de 340m/s (a una temperatura de 20ºC)

  // Serial.println(distance);
  delay(500); //espera un segundo
  return distance;

}



void loop(){

  valorCentro=medir(); // mide al frente y saca la distancia
  if (valorCentro>30 || valorCentro==0){
    Serial.println("adelante");
    adelante(); //andar hacia delante
  }
  else{
    parar();
    mirar(); //gira el sensor para ver que que objetos estan mas lejos

      if (valorIzquierda>=valorDerecha){
        izquierda(); //gira a la izquierda
        Serial.println("izquierda");
        delay (550); //espera
        parar();
      }
      else{
        derecha(); //gira a la derecha
        Serial.println("derecha");
        delay (550); //espera
        parar();
      }
}
}


void adelante(){
  digitalWrite(IN1, HIGH);
  digitalWrite(IN2, LOW);
  digitalWrite(IN3, HIGH);
  digitalWrite(IN4, LOW);
}

void atras(){
  digitalWrite(IN1, LOW);
  digitalWrite(IN2, HIGH);
  digitalWrite(IN3, LOW);
  digitalWrite(IN4, HIGH);
}

void parar(){
  digitalWrite(IN1, LOW);
  digitalWrite(IN2, LOW);
  digitalWrite(IN3, LOW);
  digitalWrite(IN4, LOW);
}

void derecha(){
  digitalWrite(IN1, HIGH);
  digitalWrite(IN2, LOW);
  digitalWrite(IN3, HIGH);
  digitalWrite(IN4, LOW);  
}

void izquierda(){
  digitalWrite(IN1, HIGH);
  digitalWrite(IN2, LOW);
  digitalWrite(IN3, LOW);
  digitalWrite(IN4, HIGH);  
}

void mirar (){
  myservo.write(90); //servo mirando hacia delante
  delay(500);
  valorCentro=medir(); //mide al frente y saca la distancia
  Serial.print ("valor centro=");
  Serial.println(valorCentro);
  myservo.write(20); //servo mirando a la derecha
  delay (500);
  valorDerecha=medir(); //mide a la derecha y saca la distancia
  Serial.print("valor derecha=");
  Serial.println(valorDerecha);
  myservo.write(160); //servo mirando a la izquierda
  delay(800);
  valorIzquierda=medir(); //mide a la izquierda y saca la distancia
  Serial.print("valor izquierda=");
  Serial.println(valorIzquierda);
  myservo.write(90); //servo mirando hacia delante
}
