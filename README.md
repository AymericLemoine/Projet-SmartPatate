
#define SET(x,y) (x |=(1<<y))				
#define CLR(x,y) (x &= (~(1<<y)))       	
#define CHK(x,y) (x & (1<<y))           		
#define TOG(x,y) (x^=(1<<y))            		



#define N 160  

float results[N];           
float freq[N];           
int sizeOfArray = N;

void setup()
{
  
  
  TCCR1A=0b10000010;        
  TCCR1B=0b00011001;        
  ICR1=110;
  OCR1A=55;

  pinMode(9,OUTPUT);        
  pinMode(8,OUTPUT);        
  pinMode(6,OUTPUT);

  Serial.begin(115200);

  for(int i=0;i<N;i++)
  results[i]=0;        
}

void loop()
{
  unsigned int d;

  int counter = 0;
  int frec = 0;
  for(unsigned int d=0;d<N;d++)
  
  {
  
    int v=analogRead(0);   
    CLR(TCCR1B,0);         
    TCNT1=0;               
    ICR1=d;                
    OCR1A=d/2;             
    SET(TCCR1B,0);          

    results[d]=results[d]*0.5+(float)(v)*0.5; 
    freq[d] = d;
    
  }

  TOG(PORTB,0);            
  frec = analogRead(A0);
  
  Serial.println(frec);
    
    if ( frec >=100 &&  frec<=125) //rien
    {
      Serial.print("rien \n");
      digitalWrite(6,LOW);
    }
      else if( frec >=130 && frec<= 159)  //1 doigt
    {
     Serial.print("1 doigt \n");
      digitalWrite(6,HIGH);
      delay (1000);
      digitalWrite(6,LOW);
      delay (500);
      digitalWrite(6,HIGH);
      }
     else if ( frec >=160 && frec<=174) //2 doigts
    {
      Serial.print("2 doigts \n");
      digitalWrite(9,HIGH);
      delay (2000);
    }
     else if( frec  >=175)  //main
    {
      Serial.print("main \n");
      digitalWrite(6, HIGH);
      delay(500);
      digitalWrite(6,LOW);
      delay(500);
      digitalWrite(6,HIGH);
      delay(500);
      digitalWrite(6,LOW);
      digitalWrite(6,HIGH);
    }
    
    
// Serial.println(analogRead(A0));
  
}
   
