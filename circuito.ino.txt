#include<LiquidCrystal.h>
#define PIN_SensorDeUmidade A5

int valorLidoUmidade; 
int menuChoice;

String mensagem1 = "";
String mensagem2 = "";

LiquidCrystal lcd(7, 6, 5, 4, 3, 2);

int pushbutton = 13; 
int led = 12; 
bool estadoled = 0;


void setup(){
	pinMode(PIN_SensorDeUmidade, INPUT);
    pinMode(pushbutton, INPUT_PULLUP); 
    pinMode(led, OUTPUT);
  	lcd.begin(16, 2);
	Serial.begin(9600); 
  	delay(500);
}
 
void loop(){ 
  	if (digitalRead(pushbutton) == LOW)
    {
      estadoled = !estadoled; 
      digitalWrite(led, estadoled);
      while (digitalRead(pushbutton) == LOW);
      delay(100);
    }
  	if(estadoled==1){
  	  umidadeDaTerra(); 
      display();
  	}
  	else{
  	  lcd.clear();
    }
}

void umidadeDaTerra(){  
    valorLidoUmidade = constrain(analogRead(PIN_SensorDeUmidade),0,876); 
	valorLidoUmidade = map(valorLidoUmidade,0,876,0,100);
  	mensagem1 ="    Umidade:    ";
  	mensagem2 = valorLidoUmidade;
  	mensagem2 = "  "+mensagem2+"%";
}
  
void display() {
    lcd.setCursor(0,0);          
    lcd.print(mensagem1); 
    lcd.setCursor(2,1);           
    lcd.print(mensagem2);
}
