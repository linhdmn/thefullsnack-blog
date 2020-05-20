---
title: Làm game Flappy Bird trên Arduino
published: true
date: 2015-03-04 12:37:50
tags: arduino, game, hacking, hardware
description: Với dân gõ code thì chỉ như thế này là đủ, giống như tôn chỉ của RoR, bạn cứ thoả sức sáng tạo, mấy chuyện khác để Arduino lo...
image:
---
## Giới thiệu một tí

Ngày nay, lĩnh vực điện tử đang dần được nhiều người quan tâm, bản thân mình cũng bắt đầu cố gắng tìm hiểu mảng này từ khá lâu (hơn 10 năm trước) nhưng tiếc là dốt Toán/Lý, cũng không phải dân học Điện tử nên sau gần 10 năm mới có làm được ít thứ để khoe với bạn bè. 

Một trong những vị cứu tinh của dân đam mê điện tử nhưng ngoại đạo như mình đó là Arduino, đây là một board mạch lập trình được (vì tích hợp cả mạch nạp, mạch nguồn, output, input,...), bạn chỉ cần tập trung vào sử dụng các chân input, output của Arduino để "sáng tác", chỉ cần một chút kiến thức điện tử cơ bản để không làm nó chập mạch, cháy nhà. 

Với dân gõ code thì chỉ như thế này là đủ, giống như tôn chỉ của RoR, bạn cứ thoả sức sáng tạo, mấy chuyện khác để Arduino lo.

Nếu chưa có kiến thức về Arduino, các bạn có thể tham khảo bài viết của bác @viethnguyen (http://kipalog.com/posts/JsG_PhVlQM-anwGna4ZT0g)

Còn bây giờ chúng ta sẽ bắt đầu làm đồ chơi.

## Làm game Flappy Bird 

Đây là video demo cho cái game Flappy Bird chúng ta sẽ làm:

![alt text](https://raw.githubusercontent.com/huytd/thefullsnack.com/master/posts/arduinoflappy.jpg)
**Video**: https://www.youtube.com/watch?v=04cY78Y2d3g

Game này mình sử dụng data hình ảnh của một tutorial tương tự từ http://arduino.vn, tuy nhiên vì code logic của trang này khó hiểu quá nên mình chọn cách tự viết từ đầu. 

### Chuẩn bị

Về phần cứng, chúng ta cần có:
- **1x** Màn hình **Nokia 5110 LCD** (giá tầm 40k/cái)
- **1x** Nút bấm (Push button, loại này mua 5k nguyên 1 bao)
- **7x** Sợi dây Jumper Wire (cái này mua 1 bó về dùng dần)
- **1x** Board Arduino. Ở đây mình dùng **UNO R3**, mấy con khác cũng ko khác gì, khác mỗi cách nối dây :v

Tổng thiệt hại chưa đến 50k, chưa tính con Arduino :D  

### Nối dây mặt trước 

Đây là phần thú vị nhất khi làm việc với Arduino, chắc do mình không phải dân chuyên điện tử nên cái khâu này khá là vất vả, để tránh làm các bạn vất vả theo mình thì mình ghi lại cách nối luôn, đỡ phải mày mò datasheet này nọ. 

**Push Button**: Nối vào `Pin 2` và `GND` của Arduino
**Màn hình**: Nối lần lượt các chân tương ứng từ màn hình vào Arduino
- `CLK` --> `Pin 7`
- `DIN` --> `Pin 6`
- `D/C` --> `Pin 5`
- `CS` --> `Pin 4`

Nguồn điện thì ở đây chúng ta sử dụng nguồn từ máy tính cấp cho Arduino, nếu muốn đem ra ngoài chơi thì bạn có thể mua 1 viên pin `9V` và nối 2 cực của pin vào `Vin` và `GND` của Arduino. 

### Nối code mặt sau

Đầu tiên, chúng ta cần define ra các dữ liệu cần dùng, ở đây là các mảng giá trị chứa hình ảnh (bitmap) cho con chim, và mấy cái ống nước. Ngoài ra mình define thêm 2 biến `ANIM_FRAME` để quy định tốc độ thay đổi hình (để tạo hiệu ứng chuyển động) của con chim và `DELAY_FRAME` để điều chỉnh tốc độ xung nhịp xử lý cho tương ứng với tốc độ của Arduino UNO R3. 

**Sprite.h**
```
#define ANIM_FRAME 50
#define DELAY_FRAME 5
static unsigned char PROGMEM flappybird_frame_1[] = { 0x03, 0xF0, 0x0C, 0x48, 0x10, 0x84, 0x78, 0x8A, 0x84, 0x8A, 0x82, 0x42, 0x82, 0x3E, 0x44, 0x41,0x38, 0xBE, 0x20, 0x41, 0x18, 0x3E, 0x07, 0xC0 };
static unsigned char PROGMEM flappybird_frame_2[] = { 0x03, 0xF0, 0x0C, 0x48, 0x10, 0x84, 0x20, 0x8A, 0x40, 0x8A, 0x40, 0x42, 0x7C, 0x3E, 0x82, 0x41, 0x84, 0xBE, 0x88, 0x41, 0x78, 0x3E, 0x07, 0xC0 };
static unsigned char PROGMEM bar_bottom[] = { 0xFF, 0xFF, 0xFF, 0x42, 0x7E, 0x7E, 0x7E, 0x7E, 0x7E, 0x7E, 0x7E, 0x7E, 0x7E, 0x7E, 0x7E, 0x7E, 0x7E, 0x7E, 0x7E, 0x7E };
static unsigned char PROGMEM bar_top[] = { 0x7E, 0x7E, 0x7E, 0x7E, 0x7E, 0x7E, 0x7E, 0x7E, 0x7E, 0x7E, 0x7E, 0x7E, 0x7E, 0x7E, 0x7E, 0x7E, 0x42, 0xFF, 0xFF, 0xFF };
```

Các mảng dữ liệu `flappybird_frame_*` và `bar_bottom`, `bar_top` là mảng được convert từ các tool bitmap converter (tool này chỉ chạy trên Windows nên mình copy luôn từ http://arduino.vn :v)

Tiếp đến chúng ta sẽ viết 2 class để xử lý 2 thành phần chính trong game, đó là con chim (`Chym.h`) và ống nước (`Bar.h`).

Tương tự như khi lập trình game trên các thiết bị khác, chúng ta cần một vòng lặp chính (`main loop`) để xử lý tuần tự và liên tục các thao tác: `Xử lý game logic` > `Xoá màn hình` > `Vẽ lại mọi thứ lên màn hình` 

Chính vì thế nên trong 2 class `Chym.h` và `Bar.h`, chúng ta sẽ có cùng cấu trúc dạng:

```
class Foo {
    void update(); // Hàm xử lý update logic
    void render(); // Hàm vẽ đối tượng lên màn hình
    // các xử lý khác
}
```

Bây giờ chúng ta sẽ bắt tay vào viết code cho đối tượng con `Chym`

**Chym.h**
```
class Chym {

private:
  int frameCount;
  int x; 
  int y;
  int deltaIde;
  int delayFrame;

  int jumpCount; 
  int maxJumpCount;
  int moveSpeed;

  bool _isDead;
public: 
  void respawn() {
    x = 24; 
    y = 20;
    deltaIde = -1;
    moveSpeed = 1;
    jumpCount = 0;
    _isDead = false;
  }
  
  Chym() {
    frameCount = 0;
    delayFrame = 0;
    maxJumpCount = 20;
    respawn();
  }
  
  void render() {
    if (frameCount < ANIM_FRAME / 2) {
      display.drawBitmap(x, y, flappybird_frame_1, 16, 12, 1);      
    } 
    else {
      display.drawBitmap(x, y, flappybird_frame_2, 16, 12, 1);          
    }
  }

  void update() {
    delayFrame++;
    if (delayFrame == DELAY_FRAME) {
      y += deltaIde * moveSpeed;
      delayFrame = 0;
    }

    if (y > 35) {
      _isDead = true;
    }

    frameCount++;
    if (frameCount >= ANIM_FRAME) frameCount = 0;
  }

  bool isDead() {
    return _isDead;
  }
  
  void die() {
    _isDead = true;
  }

  void cancelJump() {
    jumpCount = 0;
    flyDown();
  }

  void flyUp() {
    if (jumpCount < maxJumpCount) {
      deltaIde = -1; 
      moveSpeed = 3;    
      jumpCount++;
    } 
    else {
      flyDown();
    }
  }

  void flyDown() {
    deltaIde = 1; 
    moveSpeed = 1;
  }
  
  int getX() {
    return x;
  }
  
  int getY() {
    return y;
  }

};
```
Giải thích: 
- Hàm `respawn` có nhiệm vụ reset lại toạ độ, trạng thái của Chim khi bắt đầu game (hoặc sau khi chết)
- Hàm `render` có nhiệm vụ vẽ lần lượt 2 hình ảnh tượng trưng cho 2 trạng thái của Chim khi đang bay theo thời gian
- Hàm `update` có nhiệm vụ điều khiển toạ độ của Chim lên/xuống thông qua biến `deltaIde`, kiểm tra khi nào thì con Chim nó chết,...
- Hàm `isDead` kiểm tra xem Chim nó chết chưa 
- Hàm `die` để báo hiệu con Chim đã chết 
- Hàm `cancelJump` có nhiệm vụ xử lý kết thúc việc nhảy của con Chim, và gọi hàm `flyDown` để kéo con Chim đi xuống lại
- Hàm `flyUp` để nâng toạ độ của Chim lên thông qua biến `deltaIde`, tạo hiệu ứng nhảy 
- Hàm `flyDown` dùng để giảm toạ độ của Chim (giả lập môi trường trọng lực đấy)
- Hàm `getX` và `getY` dùng để lấy toạ độ của Chim.

Tiếp theo là code logic cho mấy cái ống nước (`Bar`) 

**Bar.h**:
```
class Bar {

private:
  int x; 
  int y;
  int delayFrame;
  int moveSpeed;
public:   
  Bar() {
    delayFrame = 0;
    x = 0; y = 24;
    moveSpeed = 2;
  }
  
  void setPos(int sx, int sy) {
    x = sx; y = sy;
  }
  
  void render() {
    display.drawBitmap(x, y - 30, bar_top, 8, 20, 1); 
    display.drawBitmap(x, y + 10, bar_bottom, 8, 20, 1); 
  }

  void update() {
    delayFrame++;
    if (delayFrame == DELAY_FRAME) {
      
      x -= moveSpeed;
      if (x < -10) x = 95;
      
      delayFrame = 0;
    }
  }
  
  int hitTest(int tx, int ty) {
    int hitX = ((tx >= x - 16) && (tx <= x))?1:0;
    int hitY = ((ty <= (y - 10)) || (ty + 12 >= y + 10))?1:0;
    if (hitX != 0) {
      return hitY;
    }
    return 0;
  }

};
```

Giải thích: 
- Hàm `setPos` dùng để thiết lập vị trí của ống nước
- Hàm `render` dùng để vẽ 2 cái ống nước (trên và dưới) ra màn hình theo toạ độ hiện tại của nó
- Hàm `update` làm nhiệm vụ thay đổi toạ độ của ống nước, tạo cảm giác là con chim đang bay tới (nhưng thực ra là cái ống nước bay lui)
- Hàm `hitTest` làm nhiệm vụ kiểm tra xem con chim và cái ống nước có va chạm với nhau không

Phù... chúng ta đã xây dựng xong code xử lý logic cho các đối tượng có trong game. Bây giờ là lúc ghép nối tất cả lại thành một game hoàn chỉnh. 

Ở chương trình chính của Arduino, quăng vào đoạn code sau:

```
#include <SPI.h>
#include <Adafruit_GFX.h>
#include <Adafruit_PCD8544.h>

Adafruit_PCD8544 display = Adafruit_PCD8544(7, 6, 5, 4, 3);

#include "Sprite.h"
#include "Chym.h"
#include "Bar.h"

Chym player;
Bar bar; Bar bar2;
int gameScore = 0;

int KNOCK_PIN = 2;
int LED_PIN = 8;

boolean clicked = false;

void loop(){}

void resetGame() {
  player.respawn();
  bar.setPos(0, 20);
  bar2.setPos(50, 30);
  gameScore = 0;
}

void setup() {
  Serial.begin(9600);

  display.begin();
  display.setContrast(50);
  display.clearDisplay();
  display.display();

  digitalWrite(LED_PIN, HIGH);

  pinMode(KNOCK_PIN, INPUT_PULLUP);
  pinMode(LED_PIN, OUTPUT);
  
  resetGame();
  
  while(1) {
    getInput();
    player.update();
    bar.update(); bar2.update();
    drawLCD();
  }
}

void getInput() {
  int knock = digitalRead(KNOCK_PIN);
  if (knock == 0) { // push down
    clicked = true;
  } 
  else {
    clicked = false;
  }
}

void drawLCD() {
  display.clearDisplay();

  if (!player.isDead()) {
    int ht1 = bar.hitTest(player.getX(), player.getY());
    int ht2 = bar2.hitTest(player.getX(), player.getY());
    int die = ht1 + ht2;
    if (die == 1) {
        // game over
        player.die();
    }

    if (clicked) {
      player.flyUp();
    } 
    else {
      player.cancelJump();
    }
    player.render();    
    
    bar.render(); bar2.render(); 
  } 
  else {
    display.setCursor(0, 0);
    display.setTextSize(2);
    display.println("Game   Over!!!");
    if (clicked) {
      resetGame();
    }
  }

  display.display();
}
```

Giải thích: 

Dòng lệnh đầu tiên này làm nhiệm vụ khởi tạo driver giao tiếp với màn hình LCD mà chúng ta đã nối vào Arduino:
```Adafruit_PCD8544 display = Adafruit_PCD8544(7, 6, 5, 4, 3);```

Từ bây giờ, chúng ta có thể giao tiếp với màn hình thông qua đối tượng `display`.

Hàm `resetGame` dùng để restart lại game sau khi Chim chết.

Hàm `setup` khởi tạo các thông số cần thiết cho màn hình. Trong hàm này có một vòng lặp vô hạn:
```while(1) { ... }```

Đây là vòng lặp chính của game, các lệnh bên trong đó sẽ chạy tuần từ và liên tục. Trong vòng lặp này chúng ta xử lý việc đọc trạng thái nhấn nút của Push Button thông qua hàm `getInput` và update xử lý logic cho `player` (con chim) và 2 cặp ống nước (`bar` và `bar2`), cuối cùng gọi hàm `drawLCD`.

Hàm `drawLCD` này ngoài nhiệm vụ vẽ mọi thứ lên màn hình thì còn xử lý việc kiểm tra xem Chim có đụng vào cặp ống nước nào chưa, nếu đụng thì sẽ gọi lệnh `player.die()` để báo hiệu là Chim chết. 

Hàm này còn kiểm tra biến `clicked` để nhận biết khi nào nút bấm được nhấn, nếu có thì sẽ gọi lệnh `player.flyUp()` để chim bay lên.

Cuối cùng gọi các hàm `render()` để vẽ các đối tượng hiện có ra màn hình.

Nếu chim đã chết thì khối lệnh `else` sẽ được chạy để in ra màn hình dòng chữ "Game Over", đồng thời reset lại game khi người dùng bấm vào Push Button.

### Xong rồi đấy

Bây giờ, mọi thứ đã hoàn thành, bạn có thể nhấn nút **Run** để build và nạp chương trình vào Arduino, sau đó thưởng thức thành quả của mình. 

Các bạn có thể tham khảo mã nguồn của game tại Github: https://github.com/huytd/arduino-flappybird
