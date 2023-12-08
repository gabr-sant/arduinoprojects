#include <Adafruit_GFX.h>
#include <Adafruit_TFTLCD.h>
#include <stdint.h>
#include "TouchScreen.h"
#include <SPI.h>
#include <SD.h>
#include "DFRobotDFPlayerMini.h"

//#define SD_CS 10 // Card select for shield use

#define uni 1
#define dez 0

#define YP A1  // must be an analog pin, use "An" notation!
#define XM A2  // must be an analog pin, use "An" notation!
#define YM 7   // can be a digital pin
#define XP 6   // can be a digital pin

#define TS_LEFT 191
#define TS_RT   909
#define TS_TOP  948
#define TS_BOT  219

#define MINPRESSURE 500
#define MAXPRESSURE 1000

#define TS_MINX 280
#define TS_MINY 730
#define TS_MAXX 1014
#define TS_MAXY 1020

#define LCD_CS A3 // Chip Select goes to Analog 3
#define LCD_CD A2 // Command/Data goes to Analog 2
#define LCD_WR A1 // LCD Write goes to Analog 1
#define LCD_RD A0 // LCD Read goes to Analog 0

#define LCD_RESET A4 // Can alternately just connect to Arduino's reset pin


#if defined __AVR_ATmega2560__
#define SD_SCK 13
#define SD_MISO 12
#define SD_MOSI 11

#endif
#define SD_CS 10     // Set the chip select line to whatever you use (10 doesnt conflict with the library)

#define servoPin 28

#define	BLACK   0x0000
#define	BLUE    0x001F
#define	RED     0xF800
#define	GREEN   0x07E0
#define CYAN    0x07FF
#define MAGENTA 0xF81F
#define YELLOW  0xFFE0
#define WHITE   0xFFFF

Adafruit_TFTLCD tft(LCD_CS, LCD_CD, LCD_WR, LCD_RD, LCD_RESET);
TouchScreen ts = TouchScreen(XP, YP, XM, YM, 300);
DFRobotDFPlayerMini playerMP3;

int X, Y;
byte score = 0;
uint8_t         spi_save;

void saudacao(void);
void jogo(void);
void fase(byte level);
void atualizarScore(void);
void fim(void);
void presente(void);
void counter(void);
void atualizarScore(bool status);
unsigned long selecaoData(void);
byte algarism(byte num, bool tipo);
unsigned long teclado(void);
unsigned long exp(byte base, byte expoente);
void bmpDraw(char *filename, int x, int y);
uint16_t read16(File f);
uint32_t read32(File f);


TSPoint waitTouch() {
  TSPoint p;
  do {
    p = ts.getPoint();
    pinMode(XM, OUTPUT);
    pinMode(YP, OUTPUT);
  } while ((p.z < MINPRESSURE ) || (p.z > MAXPRESSURE));
  
  p.x = map(p.x, TS_LEFT, TS_RT, 0, 240);
  p.y = map(p.y, TS_BOT, TS_TOP, 0, 320);
  
  return p;
}

void setup() {
  Serial.begin(9600);

  tft.begin(0x9341);
  tft.setRotation(2);

  #if defined __AVR_ATmega2560__
  if (!SD.begin(SD_CS, SD_MOSI, SD_MISO, SD_SCK )) {
    tft.println(F("failed!"));
    return;
  }
  #else
  if (!SD.begin(SD_CS)) {
    tft.println(F("failed!"));
    return;
  }
  #endif

  Serial1.begin(9600);

  Serial.println(F("Iniciando DFPlayer ... (Espere 3~5 segundos)"));
  if (!playerMP3.begin(Serial1)) {  // COMUNICAÇÃO REALIZADA VIA SOFTWARE SERIAL
    Serial.println(F("Falha:"));
    Serial.println(F("1.conexões!"));
    Serial.println(F("2.cheque o cartão SD!"));
    while(true){
      delay(0);
    }
  }

  pinMode(servoPin, OUTPUT);

  playerMP3.volume(10);

  saudacao();

  jogo();

  fim();

  delay(12000);

  counter();  

  presente();

}

void loop() {
  // put your main code here, to run repeatedly:

}

void saudacao() {
  tft.fillScreen(WHITE);

  tft.setCursor(40, 20);
  tft.setTextColor(RED);  tft.setTextSize(3);
  tft.println("Oi, linda");
  tft.setCursor(40, 50);
  tft.println("Tudo bem?");

  tft.setCursor(5, 100);
  tft.setTextColor(BLUE);  tft.setTextSize(2);
  tft.println("Enfim chegamos a um");
  tft.setCursor(5, 120);
  tft.println("ano juntinhos, ne?");

  tft.setCursor(5, 170);
  tft.println("Eu pensei em muitas");
  tft.setCursor(5, 190);
  tft.println("formas de te dar");
  tft.setCursor(5, 210);
  tft.println("algo unico e");
  tft.setCursor(5, 230);
  tft.println("especial nessa data");

  tft.setTextColor(BLACK);  tft.setTextSize(1);
  tft.setCursor(30, 280);
  tft.println("Clica na tela pra continuar...");

  waitTouch();

  playerMP3.loopFolder(1);

  tft.fillScreen(WHITE);

  tft.setCursor(45, 100);
  tft.setTextColor(RED);  tft.setTextSize(2);
  tft.println("Entao decidi:");
  tft.setTextColor(BLUE);
  tft.setCursor(20, 130);
  tft.println("Por que nao fazer");
  tft.setCursor(27, 150);
  tft.println("o que eu sei de");
  tft.setCursor(32, 170);
  tft.println("melhor, para a");
  tft.setTextColor(RED);
  tft.setCursor(38, 190);
  tft.println("melhor pessoa");
  tft.setTextColor(BLUE);
  tft.setCursor(34, 210);
  tft.println("da minha vida?");

  tft.setTextColor(BLACK);  tft.setTextSize(1);
  tft.setCursor(30, 280);
  tft.println("Clica na tela pra continuar...");

  waitTouch();

  tft.fillScreen(WHITE);

  tft.setCursor(12, 90);
  tft.setTextColor(RED);  tft.setTextSize(2);
  tft.println("Entao, o seguinte:");
  tft.setTextColor(BLUE);  tft.setTextSize(2);
  tft.setCursor(12, 130);
  tft.println("Se voce conseguir");
  tft.setCursor(12, 150);
  tft.println("descobrir todos os");
  tft.setCursor(12, 170);
  tft.println("enigmas desse jogo,");
  tft.setCursor(12, 190);
  tft.println("no fim vai ter uma");
  tft.setCursor(12, 210);
  tft.println("surpresinha rsrs");

  tft.setTextColor(BLACK);  tft.setTextSize(1);
  tft.setCursor(30, 280);
  tft.println("Clica na tela pra continuar...");

  waitTouch();

  tft.fillScreen(WHITE);

  tft.setCursor(58, 150);
  tft.setTextColor(RED);  tft.setTextSize(2);
  tft.println("Ta pronta?");

  tft.setTextColor(BLACK);  tft.setTextSize(1);
  tft.setCursor(30, 280);
  tft.println("Clica na tela pra continuar...");

  waitTouch();

}

void jogo() {
  for(byte level=1; level<10; level++) {
    fase(level);

  }

}

void fase(byte level) {
  if(level == 1) {
    tft.fillScreen(BLACK);

    tft.setCursor(18, 50);
    tft.setTextColor(WHITE);  tft.setTextSize(2);
    tft.println("Vou comecar leve:");
    tft.setCursor(35, 70);
    tft.println("Quando a gente");
    tft.setCursor(10, 90);
    tft.println("comecou a se falar?");
    tft.setCursor(10, 110);

    if(selecaoData() == 61122)
      atualizarScore(true);
    else
      atualizarScore(false);

  }
  else if(level == 2) {
    tft.fillScreen(BLACK);

    tft.setTextColor(WHITE);  tft.setTextSize(2);
    tft.setCursor(55, 40);
    tft.println("Ainda leve:");
    tft.setCursor(18, 60);
    tft.println("Quando a gente se");
    tft.setCursor(18, 80);
    tft.println("viu pela primeira");
    tft.setCursor(18, 100);
    tft.println("vez?");

    if(selecaoData() == 271122)
      atualizarScore(true);
    else
      atualizarScore(false);

  }

  else if(level == 3) {
    tft.fillScreen(WHITE);

    tft.setCursor(55, 120);
    tft.setTextColor(RED);  tft.setTextSize(2);
    tft.println("Vou comecar");
    tft.setCursor(30, 140);
    tft.println("a dificultar...");
    tft.setCursor(60, 180);
    tft.println("Preparada?");

    tft.setTextColor(BLACK);  tft.setTextSize(1);
    tft.setCursor(30, 280);
    tft.println("Clica na tela pra continuar...");

    waitTouch();

    tft.fillScreen(BLACK);

    tft.setCursor(30, 50);
    tft.setTextColor(WHITE);  tft.setTextSize(2);
    tft.println("Quando eu disse");
    tft.setCursor(48, 70);
    tft.println("que te amava");
    tft.setCursor(14, 90);
    tft.println("pela primeira vez?");

    if(selecaoData() == 161222)
      atualizarScore(true);
    else
      atualizarScore(false);

  }

  else if(level == 4) {
    tft.fillScreen(WHITE);

    tft.setCursor(38, 120);
    tft.setTextColor(RED);  tft.setTextSize(2);
    tft.println("Agora voce vai");
    tft.setCursor(25, 140);
    tft.println("ter que analisar");
    tft.setCursor(25, 160);
    tft.println("umas fotinhas rs");

    tft.setTextColor(BLACK);  tft.setTextSize(1);
    tft.setCursor(30, 280);
    tft.println("Clica na tela pra continuar...");

    waitTouch();

    bmpDraw("foto1.bmp", 0, 0);
    counter();

    tft.fillScreen(BLACK);

    tft.setCursor(5, 50);
    tft.setTextColor(WHITE);  tft.setTextSize(2);
    tft.println("Onde a gente estava");
    tft.setCursor(32, 70);
    tft.println("na ultima foto?");

    tft.drawRect(19, 109, 202, 42, WHITE);
    tft.drawRect(19, 159, 202, 42, WHITE);
    tft.drawRect(19, 209, 202, 42, WHITE);
    tft.drawRect(19, 259, 202, 42, WHITE);
    tft.fillRect(20, 110, 200, 40, CYAN);
    tft.fillRect(20, 160, 200, 40, CYAN);
    tft.fillRect(20, 210, 200, 40, CYAN);
    tft.fillRect(20, 260, 200, 40, CYAN);

    tft.setTextColor(BLACK);
    tft.setCursor(50, 114);
    tft.println("Padaria,");
    tft.setCursor(50, 132);
    tft.println("pos-missa");

    tft.setCursor(50, 164);
    tft.println("Yara, num");
    tft.setCursor(50, 182);
    tft.println("dia qualquer");

    tft.setCursor(50, 214);
    tft.println("Yara, nosso");
    tft.setCursor(50, 232);
    tft.println("primeiro cafe");

    tft.setCursor(50, 264);
    tft.println("Lanchonete");
    tft.setCursor(50, 282);
    tft.println("qualquer");

    //8, 88 / 220, 56 correto
    //8, 198 / 220, 109 errado
    //8, 140 / 220, 105 errado
    //8, 33 / 220, 0 errado
    bool acerto = false,
          respondido = false;
    while(respondido == false) {
      TSPoint p = waitTouch();
      if((p.x >= 8 && p.x <= 220)) {
        if(p.y <= 88 && p.y >= 56) {
          acerto = true;
          respondido = true;
        }
        else if((p.y <= 198 && p.y >= 109) || (p.y <= 140 && p.y >= 105) || (p.y <= 33 && p.y >= 0)) {
          acerto = false;
          respondido = true;
        }

      }

    }
    if(acerto)
      atualizarScore(true);
    else
      atualizarScore(false);

  }

  else if(level == 5) {
    bmpDraw("foto2.bmp", 0, 0);
    counter();

    tft.fillScreen(BLACK);

    tft.setTextColor(WHITE);  tft.setTextSize(2);
    tft.setCursor(12, 20);
    tft.println("Nao vale pesquisar:");
    tft.setCursor(12, 40);
    tft.println("Qual o nome do");
    tft.setCursor(12, 60);
    tft.println("bar que a gente");
    tft.setCursor(12, 80);
    tft.println("estava nessa foto?");

    tft.drawRect(19, 109, 202, 42, WHITE);
    tft.drawRect(19, 159, 202, 42, WHITE);
    tft.drawRect(19, 209, 202, 42, WHITE);
    tft.drawRect(19, 259, 202, 42, WHITE);
    tft.fillRect(20, 110, 200, 40, CYAN);
    tft.fillRect(20, 160, 200, 40, CYAN);
    tft.fillRect(20, 210, 200, 40, CYAN);
    tft.fillRect(20, 260, 200, 40, CYAN);

    tft.setTextColor(BLACK);
    tft.setCursor(50, 114);
    tft.println("Bar");
    tft.setCursor(50, 132);
    tft.println("Pimenta");

    tft.setCursor(50, 164);
    tft.println("Bar do");
    tft.setCursor(50, 182);
    tft.println("Joao");

    tft.setCursor(50, 214);
    tft.println("Bar do");
    tft.setCursor(50, 232);
    tft.println("Jao");

    tft.setCursor(50, 264);
    tft.println("Comida di");
    tft.setCursor(50, 282);
    tft.println("Buteco");

    //8, 88 / 220, 56 correto
    //8, 198 / 220, 109 errado
    //8, 140 / 220, 105 errado
    //8, 33 / 220, 0 errado
    bool acerto = false,
          respondido = false;
    while(respondido == false) {
      TSPoint p = waitTouch();
      if((p.x >= 8 && p.x <= 220)) {
        if(p.y <= 88 && p.y >= 56) {
          acerto = true;
          respondido = true;
        }
        else if((p.y <= 198 && p.y >= 109) || (p.y <= 140 && p.y >= 105) || (p.y <= 33 && p.y >= 0)) {
          acerto = false;
          respondido = true;
        }

      }

    }
    if(acerto)
      atualizarScore(true);
    else
      atualizarScore(false);

  }

  else if(level == 6) {
    bmpDraw("foto3.bmp", 0, 0);
    counter();

    tft.fillScreen(BLACK);

    tft.setTextColor(WHITE);  tft.setTextSize(2);
    tft.setCursor(12, 70);
    tft.println("Pode pesquisar:");
    tft.setCursor(12, 90);
    tft.println("Qual o cep do");
    tft.setCursor(12, 110);
    tft.println("parque que a gente");
    tft.setCursor(12, 130);
    tft.println("estava nessa foto?");

    tft.setTextColor(WHITE);  tft.setTextSize(1);
    tft.setCursor(30, 280);
    tft.println("Clica na tela pra digitar...");

    waitTouch();

    if(teclado() == 5317020)
      atualizarScore(true);
    else
      atualizarScore(false);

  }

  else if(level == 7) {
    bmpDraw("foto4.bmp", 0, 0);
    counter();

    tft.fillScreen(BLACK);

    tft.setTextColor(WHITE);  tft.setTextSize(2);
    tft.setCursor(12, 30);
    tft.println("Quero ver agora:");
    tft.setCursor(12, 50);
    tft.println("Qual a data em que");
    tft.setCursor(12, 70);
    tft.println("voce e o Miguel pe-");
    tft.setCursor(12, 90);
    tft.println("garam essa pelucia?");

    if(selecaoData() == 311222)
      atualizarScore(true);
    else
      atualizarScore(false);

  }

  else if(level == 8) {
    bmpDraw("foto5.bmp", 0, 0);
    counter();

    tft.fillScreen(BLACK);

    tft.setTextColor(WHITE);  tft.setTextSize(2);
    tft.setCursor(3, 50);
    tft.println("Aqui eu disse que");
    tft.setCursor(3, 70);
    tft.println("te amava pela pri-");
    tft.setCursor(3, 90);
    tft.println("meira vez... Qual o");
    tft.setCursor(3, 110);
    tft.println("numero do estabele-");
    tft.setCursor(3, 130);
    tft.println("cimento?");

    tft.setTextColor(WHITE);  tft.setTextSize(1);
    tft.setCursor(30, 280);
    tft.println("Clica na tela pra digitar...");

    waitTouch();

    if(teclado() == 54700000)
      atualizarScore(true);
    else
      atualizarScore(false);

  }

}

void fim() {
  tft.fillScreen(WHITE);

  tft.setCursor(22, 90);  tft.setTextSize(3);
  tft.setTextColor(RED);
  tft.println("FIM DO JOGO");

  tft.setCursor(20, 150);
  tft.setTextColor(BLUE);
  tft.println("Score Final:");

  tft.fillCircle(120, 228, 30, BLUE);

  tft.setCursor(110, 215);
  tft.setTextSize(4);
  tft.setTextColor(WHITE);
  tft.println(score);

  delay(3000);

  tft.fillScreen(WHITE);

  tft.setTextSize(3);
  tft.setCursor(30, 30);
  tft.setTextColor(BLUE);
  tft.println("O que voce");
  tft.setCursor(60, 60);
  tft.println("ganhou:");

  tft.setTextSize(2);

  if(score >= 6) {
    tft.setCursor(53, 100);
    tft.setTextColor(RED);
    tft.println("Direito a 3");
    tft.setCursor(51, 120);
    tft.println("dos premios:");

  }
  else if(score >= 3 && score < 6) {
    tft.setCursor(53, 100);
    tft.setTextColor(RED);
    tft.println("Direito a 2");
    tft.setCursor(51, 120);
    tft.println("dos premios:");

  }
  else if(score < 3) {
    tft.setCursor(53, 100);
    tft.setTextColor(RED);
    tft.println("Direito a 1");
    tft.setCursor(51, 120);
    tft.println("dos premios:");

  }

  tft.fillRect(10, 145, 220, 165, GREEN);

  tft.setTextSize(1);

  tft.setTextColor(BLACK);
  tft.setCursor(20, 155);
  tft.println(". 1 NOITE NUM MOTEL A SUA ESCOLHA");

  tft.setCursor(20, 215);
  tft.println(". 1 JANTAR NUM RESTAURANTE A SUA");
  tft.setCursor(44, 225);
  tft.println("ESCOLHA");

  tft.setCursor(20, 275);
  tft.println(". 1 BRUNCH NUM LUGAR A SUA ESCOLHA");

}

void presente() {
  tft.fillScreen(WHITE);

  tft.setTextSize(3);
  tft.setTextColor(BLUE);
  tft.setCursor(10, 30);
  tft.println("Enfim, linda");

  tft.setTextColor(RED);
  tft.setCursor(30, 70);
  tft.println("Agora que");
  tft.setCursor(5, 100);
  tft.println("voce terminou");
  tft.setCursor(30, 130);
  tft.println("o jogo, eu");
  tft.setCursor(40, 160);
  tft.println("queria te");
  tft.setCursor(40, 190);
  tft.println("falar que");
  tft.setCursor(50, 220);
  tft.println("tem sido");
  tft.setCursor(40, 250);
  tft.println("um prazer");
  tft.setCursor(40, 280);
  tft.println("dividir a");

  delay(8000);
  tft.fillScreen(WHITE);

  tft.setTextSize(3);
  tft.setTextColor(RED);

  tft.setCursor(5, 20);
  tft.println("vida com voce");
  tft.setCursor(5, 50);
  tft.println("Eu nunca pen-");
  tft.setCursor(5, 80);
  tft.println("sei que fosse");
  tft.setCursor(20, 110);
  tft.println("amar alguem");
  tft.setCursor(25, 140);
  tft.println("como eu te");
  tft.setCursor(10, 170);
  tft.println("amo, mesmo q");
  tft.setCursor(5, 200);
  tft.println("nem sempre eu");
  tft.setCursor(10, 230);
  tft.println("te demonstre");
  tft.setCursor(10, 260);
  tft.println("da forma que");
  tft.setCursor(20, 290);
  tft.println("voce merece");

  delay(8000);
  tft.fillScreen(WHITE);

  tft.setCursor(5, 20);
  tft.println("De toda forma");
  tft.setCursor(5, 50);
  tft.println("espero que vc");
  tft.setCursor(10, 80);
  tft.println("saiba que eu");
  tft.setCursor(20, 110);
  tft.println("quero ficar");
  tft.setCursor(15, 140);
  tft.println("velhinho ao");
  tft.setCursor(15, 170);
  tft.println("seu lado, e");
  tft.setCursor(5, 200);
  tft.println("que nos ainda");
  tft.setCursor(20, 230);
  tft.println("vamos viver");
  tft.setCursor(20, 260);
  tft.println("muita coisa");
  tft.setCursor(30, 290);
  tft.println("juntinhos.");

  delay(8000);
  tft.fillScreen(WHITE);

  tft.setCursor(25, 20);
  tft.println("EU TE AMO!");
  tft.setCursor(25, 50);
  tft.println("E TO AQUI");
  tft.setCursor(25, 80);
  tft.println("PRO QUE DER");
  tft.setCursor(25, 110);
  tft.println("E VIER...");

  tft.fillTriangle(120, 300, 28, 230, 212, 230, RED);
  tft.fillCircle(68, 200, 50, WHITE);
  tft.fillCircle(172, 200, 50, WHITE);
  tft.fillCircle(75, 210, 45, RED);
  tft.fillCircle(165, 210, 45, RED);
  tft.fillRect(100, 200, 40, 50, RED);

  delay(1000);

  tft.setTextSize(2);
  tft.setTextColor(WHITE);
  tft.setCursor(50, 215);
  tft.println("Clica aqui..");

  waitTouch();

  tft.fillScreen(WHITE);

  tft.setTextSize(3);
  tft.setTextColor(RED);

  tft.setCursor(5, 50);
  tft.println("SEU PRESENTE:");

  tft.setTextColor(BLUE);

  tft.setCursor(20, 100);
  tft.println("VALE TATTOO");
  tft.setCursor(15, 130);
  tft.println("ATE R$500,00");

  tft.setTextColor(BLACK);

  tft.setCursor(60, 200);
  tft.println("FIM...");

}

void counter() {
  for(int z=3; z>0; z--) {
    tft.setCursor(110, 270);
    tft.fillCircle(120, 285, 30, BLUE);
    tft.drawCircle(120, 285, 30, WHITE);
    tft.setTextColor(WHITE); tft.setTextSize(4);
    tft.print(z);
    delay(1000);

  }

}

void atualizarScore(bool status) {
  if(status == true) {
    score++;
    tft.fillCircle(14, 307, 11, GREEN);
    tft.fillCircle(124, 307, 11, GREEN);
    tft.fillRect(14, 296, 110, 23, GREEN);
    tft.setTextSize(2);
    tft.setTextColor(BLACK);
    tft.setCursor(16, 300);
    tft.print("ACERTOU");


  }
  else {
    tft.fillCircle(14, 307, 11, RED);
    tft.fillCircle(124, 307, 11, RED);
    tft.fillRect(14, 296, 110, 23, RED);
    tft.setTextSize(2);
    tft.setTextColor(WHITE);
    tft.setCursor(23, 300);
    tft.print("ERROU");

  }

  tft.fillCircle(113, 307, 11, BLUE);
  tft.fillCircle(223, 307, 11, BLUE);
  tft.fillRect(113, 296, 110, 23, BLUE);
  tft.setTextSize(2);
  tft.setTextColor(YELLOW);
  tft.setCursor(122, 300);
  tft.print("Score: ");
  tft.println(String(score));

  delay(2000);

}

unsigned long selecaoData() {
  long dd = 0, mm = 0, aa = 0;
  bool ok = false;

  tft.fillRect(tft.width()/2 - 98, tft.height()/2 + 77, 196, 56, WHITE);
  tft.fillRect(tft.width()/2 - 95, tft.height()/2 + 80, 190, 50, GREEN);
  tft.setTextColor(BLACK);  tft.setTextSize(3);
  tft.setCursor(tft.width()/2 - 80, tft.height()/2 + 95);
  tft.print("CONFIRMAR");

  tft.fillRect(tft.width()/2 - 95, tft.height()/2 - 15, 60, 50, WHITE);
  tft.fillRect(tft.width()/2 - 30, tft.height()/2 - 15, 60, 50, WHITE);
  tft.fillRect(tft.width()/2 + 35, tft.height()/2 - 15, 60, 50, WHITE);

  tft.fillRect(tft.width()/2 - 95, tft.height()/2 - 30, 60, 15, GREEN);
  tft.fillRect(tft.width()/2 - 30, tft.height()/2 - 30, 60, 15, GREEN);
  tft.fillRect(tft.width()/2 + 35, tft.height()/2 - 30, 60, 15, GREEN);

  tft.fillRect(tft.width()/2 - 95, tft.height()/2 + 35, 60, 15, RED);
  tft.fillRect(tft.width()/2 - 30, tft.height()/2 + 35, 60, 15, RED);
  tft.fillRect(tft.width()/2 + 35, tft.height()/2 + 35, 60, 15, RED);

  tft.setTextColor(BLACK);  tft.setTextSize(2);

  tft.setCursor(tft.width()/2 - 70, tft.height()/2 - 30);
  tft.print("+");
  tft.setCursor(tft.width()/2 - 5, tft.height()/2 - 30);
  tft.print("+");
  tft.setCursor(tft.width()/2 + 60, tft.height()/2 - 30);
  tft.print("+");

  tft.setTextColor(WHITE);  tft.setTextSize(2);

  tft.setCursor(tft.width()/2 - 70, tft.height()/2 + 35);
  tft.print("-");
  tft.setCursor(tft.width()/2 - 5, tft.height()/2 + 35);
  tft.print("-");
  tft.setCursor(tft.width()/2 + 60, tft.height()/2 + 35);
  tft.print("-");

  tft.setTextColor(BLACK);  tft.setTextSize(3);

  tft.setCursor(tft.width()/2 - 83, tft.height()/2);
  tft.println("00");
  tft.setCursor(tft.width()/2 - 18, tft.height()/2);
  tft.println("00");
  tft.setCursor(tft.width()/2 + 47, tft.height()/2);
  tft.println("00");

  /*
  x1min = 10
  x1max = 65
  x2min = 80
  x2max = 145
  x3min = 160
  x3max = 215

  ygmin = 185
  ygmax = 160
  yrmin = 115
  yrmax = 90

  xokmin = 8
  xokmax = 215
  yokmin = 8
  yokmax = 60

  */

  while(ok == false) {
    TSPoint p = waitTouch();

    if(p.y >= 8 && p.y <= 60 && p.x >= 8 && p.x <= 215) {
      ok = true;
    }
    else if(p.y >= 90 && p.y <= 115) {
      if(p.x >= 10 && p.x <= 65)
        dd--;
      else if(p.x >= 80 && p.x <= 145)
        mm--;
      else if(p.x >= 160 && p.x <= 215)
        aa--;

    }
    else if(p.y >= 160 && p.y <= 185) {
      if(p.x >= 10 && p.x <= 65)
        dd++;
      else if(p.x >= 80 && p.x <= 145)
        mm++;
      else if(p.x >= 160 && p.x <= 215)
        aa++;

    }

    dd = (dd < 0) ? 31 : dd;
    mm = (mm < 0) ? 12 : mm;
    aa = (aa < 0) ? 23 : aa;

    dd = (dd > 31) ? 0 : dd;
    mm = (mm > 12) ? 0 : mm;
    aa = (aa > 23) ? 0 : aa;

    tft.fillRect(tft.width()/2 - 95, tft.height()/2 - 15, 60, 50, WHITE);
    tft.fillRect(tft.width()/2 - 30, tft.height()/2 - 15, 60, 50, WHITE);
    tft.fillRect(tft.width()/2 + 35, tft.height()/2 - 15, 60, 50, WHITE);

    tft.setTextColor(BLACK);  tft.setTextSize(3);

    tft.setCursor(tft.width()/2 - 83, tft.height()/2);
    tft.print(algarism(dd, dez));
    tft.print(algarism(dd, uni));
    tft.setCursor(tft.width()/2 - 18, tft.height()/2);
    tft.print(algarism(mm, dez));
    tft.print(algarism(mm, uni));
    tft.setCursor(tft.width()/2 + 47, tft.height()/2);
    tft.print(algarism(aa, dez));
    tft.print(algarism(aa, uni));
    delay(200);

  }

  return (dd*10000)+(mm*100)+aa;

}

byte algarism(byte num, bool tipo) {
  byte numUni = num%10,
  numDez = num/10;

  if(tipo == uni) return numUni;
  else if(tipo == dez) return numDez;

}

void servo() {
  for(int x=1000; x<2000; x++) {
    digitalWrite(servoPin, HIGH);
    delayMicroseconds(x);
    digitalWrite(servoPin, LOW);
    delayMicroseconds(20000-x);

  }

}

unsigned long teclado() {
  byte ctrl  = 7;
  unsigned long numero = 0;

  tft.fillScreen(BLACK);

  tft.fillRect(tft.width()/2 - 100, tft.height()/2 - 130, 200, 60, WHITE);

  tft.fillRect(tft.width()/2 - 95, tft.height()/2 - 60, 60, 50, WHITE);
  tft.fillRect(tft.width()/2 - 30, tft.height()/2 - 60, 60, 50, WHITE);
  tft.fillRect(tft.width()/2 + 35, tft.height()/2 - 60, 60, 50, WHITE);

  tft.fillRect(tft.width()/2 - 95, tft.height()/2 -  5, 60, 50, WHITE);
  tft.fillRect(tft.width()/2 - 30, tft.height()/2 -  5, 60, 50, WHITE);
  tft.fillRect(tft.width()/2 + 35, tft.height()/2 -  5, 60, 50, WHITE);

  tft.fillRect(tft.width()/2 - 95, tft.height()/2 + 50, 60, 50, WHITE);
  tft.fillRect(tft.width()/2 - 30, tft.height()/2 + 50, 60, 50, WHITE);
  tft.fillRect(tft.width()/2 + 35, tft.height()/2 + 50, 60, 50, WHITE);

  tft.fillRect(tft.width()/2 - 95, tft.height()/2 + 105, 60, 50, WHITE);
  tft.fillRect(tft.width()/2 - 30, tft.height()/2 + 105, 60, 50, WHITE);
  tft.fillRect(tft.width()/2 + 35, tft.height()/2 + 105, 60, 50, WHITE);

  tft.setTextColor(BLACK);  tft.setTextSize(3);

  tft.setCursor(tft.width()/2 - 72, tft.height()/2 - 45);
  tft.println("1");
  tft.setCursor(tft.width()/2 - 7,  tft.height()/2 - 45);
  tft.println("2");
  tft.setCursor(tft.width()/2 + 56, tft.height()/2 - 45);
  tft.println("3");

  tft.setCursor(tft.width()/2 - 72, tft.height()/2 + 10);
  tft.println("4");
  tft.setCursor(tft.width()/2 - 7,  tft.height()/2 + 10);
  tft.println("5");
  tft.setCursor(tft.width()/2 + 56, tft.height()/2 + 10);
  tft.println("6");

  tft.setCursor(tft.width()/2 - 72, tft.height()/2 + 65);
  tft.println("7");
  tft.setCursor(tft.width()/2 - 7,  tft.height()/2 + 65);
  tft.println("8");
  tft.setCursor(tft.width()/2 + 56, tft.height()/2 + 65);
  tft.println("9");

  tft.setTextColor(RED);
  tft.setCursor(tft.width()/2 - 72, tft.height()/2 + 120);
  tft.println("X");
  tft.setTextColor(BLACK);
  tft.setCursor(tft.width()/2 - 7,  tft.height()/2 + 120);
  tft.println("0");
  tft.setTextColor(GREEN);
  tft.setCursor(tft.width()/2 + 50,  tft.height()/2 + 120);
  tft.println("OK");
  tft.setTextColor(BLACK);

  tft.setCursor(tft.width()/2 - 70, tft.height()/2 - 108);
  bool ok = false;
  byte key = 0,
       keysPressed = 0;
  while(ok == false) {
    TSPoint p = waitTouch();
    if(keysPressed <= 8) {
      if(p.x >= 8 && p.x <= 65) {
        keysPressed++;
        if(p.y >= -10 && p.y <= 30)
          key = 10; //10 = X
        else if(p.y >= 40 && p.y <= 90)
          key = 7;
        else if(p.y >= 100 && p.y <= 150)
          key = 4;
        else if(p.y >= 170 && p.y <= 210)
          key = 1;

      }
      else if(p.x >= 80 && p.x <= 140) {
        keysPressed++;
        if(p.y >= -10 && p.y <= 30)
          key = 0;
        else if(p.y >= 40 && p.y <= 90)
          key = 8;
        else if(p.y >= 100 && p.y <= 150)
          key = 5;
        else if(p.y >= 170 && p.y <= 210)
          key = 2;

      }
      else if(p.x >= 150 && p.x <= 210) {
        keysPressed++;
        if(p.y >= -10 && p.y <= 30)
          key = 11; //11 = OK
        else if(p.y >= 40 && p.y <= 90)
          key = 9;
        else if(p.y >= 100 && p.y <= 150)
          key = 6;
        else if(p.y >= 170 && p.y <= 210)
          key = 3;

      }

      if(key != 10 && key != 11) {
        tft.print(key);
        numero += key*(exp(10, ctrl));
        ctrl--;

      }
      else if(key == 10) {
        numero = 0;
        ctrl = 7;
        keysPressed = 0;
        key = 0;
        tft.fillRect(tft.width()/2 - 100, tft.height()/2 - 130, 200, 60, WHITE);
        tft.setCursor(tft.width()/2 - 80, tft.height()/2 - 115);

      }
      ok = (key == 11) ? true : false;
      Serial.print(key); Serial.print("\t"); Serial.print(exp(10, ctrl)); Serial.print("\t"); Serial.println(numero);
      delay(200);

    }

  }

  Serial.println(numero);
  return numero;

}

unsigned long exp(byte base, byte expoente) {
  unsigned long result = base;

  for(int z=1; z<expoente; z++)
    result = result*base;

  result = (expoente == 0) ? 1 : result;
  return result;

}


#define BUFFPIXEL 2

void bmpDraw(char *filename, int x, int y) {

  File     bmpFile;
  int      bmpWidth, bmpHeight;   // W+H in pixels
  uint8_t  bmpDepth;              // Bit depth (currently must be 24)
  uint32_t bmpImageoffset;        // Start of image data in file
  uint32_t rowSize;               // Not always = bmpWidth; may have padding
  uint8_t  sdbuffer[3*BUFFPIXEL]; // pixel in buffer (R+G+B per pixel)
  uint16_t lcdbuffer[BUFFPIXEL];  // pixel out buffer (16-bit per pixel)
  uint8_t  buffidx = sizeof(sdbuffer); // Current position in sdbuffer
  boolean  goodBmp = false;       // Set to true on valid header parse
  boolean  flip    = true;        // BMP is stored bottom-to-top
  int      w, h, row, col;
  uint8_t  r, g, b;
  uint32_t pos = 0, startTime = millis();
  uint8_t  lcdidx = 0;
  boolean  first = true;

  if((x >= tft.width()) || (y >= tft.height())) return;

#ifdef DEBUG
  tft.fillScreen(0);
  tft.setCursor(0,0);
  tft.print(F("Loading image '"));
  tft.print(filename);
  tft.println('\'');
#endif
  // Open requested file on SD card
  if ((bmpFile = SD.open(filename)) == NULL) {
    #ifdef DEBUG
    tft.println(F("File not found"));
    #endif
    return;
  }

  // Parse BMP header
  if(read16(bmpFile) == 0x4D42) {
      // BMP signature
    #ifdef DEBUG
    tft.println(F("File size: ")); tft.println(read32(bmpFile));
    #else
        read32(bmpFile);
    #endif
    (void)read32(bmpFile); // Read & ignore creator bytes
    bmpImageoffset = read32(bmpFile); // Start of image data
    #ifdef DEBUG
    tft.print(F("Image Offset: ")); tft.println(bmpImageoffset, DEC);
    // Read DIB header
    tft.print(F("Header size: ")); tft.println(read32(bmpFile));
    #else
    read32(bmpFile);
    #endif

    bmpWidth  = read32(bmpFile);
    bmpHeight = read32(bmpFile);
    if(read16(bmpFile) == 1) { // # planes -- must be '1'
      bmpDepth = read16(bmpFile); // bits per pixel
      #ifdef DEBUG
      Serial.print(F("Bit Depth: ")); Serial.println(bmpDepth);
      #endif
      if((bmpDepth == 24) && (read32(bmpFile) == 0)) { // 0 = uncompressed

        goodBmp = true; // Supported BMP format -- proceed!
        #ifdef DEBUG
        tft.print(F("Image size: "));
        tft.print(bmpWidth);
        tft.print('x');
        tft.println(bmpHeight);
        delay(3000);
        #endif
        // BMP rows are padded (if needed) to 4-byte boundary
        rowSize = (bmpWidth * 3 + 3) & ~3;

        // If bmpHeight is negative, image is in top-down order.
        // This is not canon but has been observed in the wild.
        if(bmpHeight < 0) {
          bmpHeight = -bmpHeight;
          flip      = false;
        }

        // Crop area to be loaded
        w = bmpWidth;
        h = bmpHeight;
        if((x+w-1) >= tft.width())  w = tft.width()  - x;
        if((y+h-1) >= tft.height()) h = tft.height() - y;

        // Set TFT address window to clipped image bounds
        tft.setAddrWindow(x, y, x+w-1, y+h-1);

        for (row=0; row<h; row++) { // For each scanline...
          // Seek to start of scan line.  It might seem labor-
          // intensive to be doing this on every line, but this
          // method covers a lot of gritty details like cropping
          // and scanline padding.  Also, the seek only takes
          // place if the file position actually needs to change
          // (avoids a lot of cluster math in SD library).
          if(flip) // Bitmap is stored bottom-to-top order (normal BMP)
            pos = bmpImageoffset + (bmpHeight - 1 - row) * rowSize;
          else     // Bitmap is stored top-to-bottom
            pos = bmpImageoffset + row * rowSize;
          if(bmpFile.position() != pos) { // Need seek?
            bmpFile.seek(pos);
            buffidx = sizeof(sdbuffer); // Force buffer reload
          }

          for (col=0; col<w; col++) { // For each column...
            // Time to read more pixel data?
            if (buffidx >= sizeof(sdbuffer)) { // Indeed
              // Push LCD buffer to the display first
              if(lcdidx > 0) {
                tft.pushColors(lcdbuffer, lcdidx, first);
                lcdidx = 0;
                first  = false;
              }
              bmpFile.read(sdbuffer, sizeof(sdbuffer));
              buffidx = 0; // Set index to beginning
            }

            // Convert pixel from BMP to TFT format
            b = sdbuffer[buffidx++];
            g = sdbuffer[buffidx++];
            r = sdbuffer[buffidx++];
            lcdbuffer[lcdidx++] = tft.color565(r,g,b);
          } // end pixel
        } // end scanline
        // Write any remaining data to LCD
        if(lcdidx > 0) {
          tft.pushColors(lcdbuffer, lcdidx, first);
        }

      } // end goodBmp
    }
  }

  bmpFile.close();
  if(!goodBmp) tft.println(F("BMP format not recognized."));
}

// These read 16- and 32-bit types from the SD card file.
// BMP data is stored little-endian, Arduino is little-endian too.
// May need to reverse subscript order if porting elsewhere.

uint16_t read16(File f) {
  uint16_t result;
  ((uint8_t *)&result)[0] = f.read(); // LSB
  ((uint8_t *)&result)[1] = f.read(); // MSB
  return result;
}

uint32_t read32(File f) {
  uint32_t result;
  ((uint8_t *)&result)[0] = f.read(); // LSB
  ((uint8_t *)&result)[1] = f.read();
  ((uint8_t *)&result)[2] = f.read();
  ((uint8_t *)&result)[3] = f.read(); // MSB
  return result;
}
