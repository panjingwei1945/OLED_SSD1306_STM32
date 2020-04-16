# OLED_SSD1306_STM32
SSD1306 library for STM32 HAL

* 下载SSD1306控制库，并复制到工程

	* https://github.com/panjingwei1945/OLED_SSD1306_STM32
	* OLED_SSD1306_STM32\ssd1306.c --> Src
	* OLED_SSD1306_STM32\ssd1306.c --> Src
	* OLED_SSD1306_STM32\ssd1306.h --> inc
	* OLED_SSD1306_STM32\oledfont.c --> Src
	* OLED_SSD1306_STM32\oledfont.c --> inc

* 修改ssd1306.h 中的端口操作宏，对于这个开发板定义如下：
```c
//-----------------OLED端口定义----------------
#define OLED_CS_Clr()  HAL_GPIO_WritePin(OLED_CS_GPIO_Port,OLED_CS_Pin,GPIO_PIN_RESET)

#define OLED_CS_Set()  HAL_GPIO_WritePin(OLED_CS_GPIO_Port,OLED_CS_Pin,GPIO_PIN_SET)

#define OLED_RST_Clr() HAL_GPIO_WritePin(OLED_RES_GPIO_Port,OLED_RES_Pin,GPIO_PIN_RESET) //RES
#define OLED_RST_Set() HAL_GPIO_WritePin(OLED_RES_GPIO_Port,OLED_RES_Pin,GPIO_PIN_SET)

#define OLED_DC_Clr() HAL_GPIO_WritePin(OLED_DC_GPIO_Port,OLED_DC_Pin,GPIO_PIN_RESET)

#define OLED_DC_Set() HAL_GPIO_WritePin(OLED_DC_GPIO_Port,OLED_DC_Pin,GPIO_PIN_SET)

#define SPI_TRANSMIT(d) HAL_SPI_Transmit(&hspi4, &d, 1, HAL_MAX_DELAY);
```
* main.c中加入头函数

```c
/* USER CODE BEGIN Includes */
#include "ssd1306.h"
/* USER CODE END Includes */
```

* main函数中初始化OLED
```c
/* USER CODE BEGIN 2 */  
OLED_Init();  
OLED_Clear();  
/* USER CODE END 2 */
```

* 测试代码
```c
  OLED_ShowString(0, 0, "hello world!", 8);  OLED_ShowString(0, 1, "Big string",16);  OLED_ShowNum(1, 3, 123456, 8, 8);注意，x坐标是像素值。而y坐标是列值，一列有8个像素。size参数有两个分别是16代表大字符，占8*16像素。8代表小字符，占6*8像素。
```

* 显示中文。
  * 使用PCtoLCD2002生成字模。选项见下。
  * 比如可以生成如下的字模数组，放到oledfont.c文件中。

```c
const unsigned char testChText[][32] = {    
  {0x40,0x40,0x42,0xCC,0x00,0x90,0x90,0x90,0x90,0x90,0xFF,0x10,0x11,0x16,0x10,0x00},    
  {0x00,0x00,0x00,0x3F,0x10,0x28,0x60,0x3F,0x10,0x10,0x01,0x0E,0x30,0x40,0xF0,0x00},/*"试",0*/    
  {0x40,0x40,0x42,0xCC,0x00,0x90,0x90,0x90,0x90,0x90,0xFF,0x10,0x11,0x16,0x10,0x00},    
  {0x00,0x00,0x00,0x3F,0x10,0x28,0x60,0x3F,0x10,0x10,0x01,0x0E,0x30,0x40,0xF0,0x00},/*"试",1*/
  {0x20,0x22,0x2A,0x2A,0xAA,0x6A,0x3A,0x2E,0x29,0x29,0x29,0x29,0x29,0x20,0x20,0x00},    
  {0x08,0x04,0x02,0x01,0xFF,0x55,0x55,0x55,0x55,0x55,0x55,0xFF,0x00,0x00,0x00,0x00},/*"看",2*/    
  {0x00,0x10,0x88,0xC4,0x33,0x00,0x40,0x42,0x42,0x42,0xC2,0x42,0x42,0x42,0x40,0x00},    
  {0x02,0x01,0x00,0xFF,0x00,0x00,0x00,0x00,0x40,0x80,0x7F,0x00,0x00,0x00,0x00,0x00},/*"行",3*/    
  {0x00,0x02,0x02,0x02,0x02,0x82,0x42,0xF2,0x0E,0x42,0x82,0x02,0x02,0x02,0x00,0x00},    
  {0x10,0x08,0x04,0x02,0x01,0x00,0x00,0xFF,0x00,0x00,0x00,0x01,0x02,0x0C,0x00,0x00},/*"不",4*/    
  {0x00,0x10,0x88,0xC4,0x33,0x00,0x40,0x42,0x42,0x42,0xC2,0x42,0x42,0x42,0x40,0x00},    
  {0x02,0x01,0x00,0xFF,0x00,0x00,0x00,0x00,0x40,0x80,0x7F,0x00,0x00,0x00,0x00,0x00},/*"行",5*/};
```

* 显示汉字代码


```c
  for(int i = 0; i<6; i++)
  {
    OLED_ShowCHinese(6 + i*16, 6, i, testChText);
  }
```

> 注意汉字占16*16像素。所以x需要以16为间隔。
