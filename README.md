# STM32WB55 BLE P2P Server — Temperature & Pressure Display
**This project implements a BLE P2P Server on the STM32WB55 and displays temperature and pressure values on an SSD1306 OLED using a custom monochrome graphics engine.
The codebase is divided into several functional modules described below.**

## Configuration of pins:
<p align="center">
<img width="611" height="642" alt="image" src="https://github.com/user-attachments/assets/38750f67-2b23-4be5-90aa-5b5470def4fd" />
</p>

## Configuration of I2C:
<p align="center">
<img width="501" height="656" alt="image" src="https://github.com/user-attachments/assets/e2455002-1e72-409f-afdf-cfc3d4eaa546" />
</p>

## Configuration of DMA:
<p align="center">
<img width="752" height="965" alt="image" src="https://github.com/user-attachments/assets/b17e5533-6b4a-43b6-95cb-7db42ad48cae" />
<img width="1002" height="231" alt="image" src="https://github.com/user-attachments/assets/ff517300-0fba-4f5b-a468-647de9aaae9e" />
</p>

## Configuration of GPIO:
<p align="center">
<img width="1004" height="226" alt="image" src="https://github.com/user-attachments/assets/5befc244-906f-4b27-9975-ca6bd96182f0" />
</p>

## 📘 Source Code Overview

<p align="center">
<img width="301" height="521" alt="image" src="https://github.com/user-attachments/assets/14047ba0-0abe-4f75-8842-fe0846264eb4" />
</p>

**Example of running:**
<p align="center">
<img width="471" height="502" alt="image" src="https://github.com/user-attachments/assets/da37a0f9-df5b-496f-a0d3-334e54c8aeaa" />
<img width="399" height="419" alt="image" src="https://github.com/user-attachments/assets/cc577764-7091-4b0c-80fd-5be76122bd2e" />
<img width="412" height="442" alt="image" src="https://github.com/user-attachments/assets/47362a41-da77-4b17-89cd-ff9de5bafc59" />
<img width="436" height="455" alt="image" src="https://github.com/user-attachments/assets/fe4bf75b-e002-4a36-816f-3e008aeabe37" />
</p>



## 🖼 1. Font System — font_8x5.h & fonts.h
**font_8x5.h**
This file defines a compact 8×5 bitmap font used for rendering text on the OLED.
The first two bytes specify the font dimensions:
````
8, 5 // height, width
````
Each character is stored as 5 bytes representing vertical pixel columns.

**fonts.h**
This file selects which font is compiled into the project:
````
#define FONT_8x5 1
````
It automatically includes the correct font header.


**font_8x5.h**:
````c
#ifndef FONT_8X5_H_
#define FONT_8X5_H_

// Font definition
const uint8_t font_8x5[] =
{
			8, 5, //height, width
			0x00, 0x00, 0x00, 0x00, 0x00,
			0x00, 0x00, 0x5F, 0x00, 0x00,
			0x00, 0x07, 0x00, 0x07, 0x00,
			0x14, 0x7F, 0x14, 0x7F, 0x14,
			0x24, 0x2A, 0x7F, 0x2A, 0x12,
			0x23, 0x13, 0x08, 0x64, 0x62,
			0x36, 0x49, 0x56, 0x20, 0x50,
			0x00, 0x08, 0x07, 0x03, 0x00,
			0x00, 0x1C, 0x22, 0x41, 0x00,
			0x00, 0x41, 0x22, 0x1C, 0x00,
			0x2A, 0x1C, 0x7F, 0x1C, 0x2A,
			0x08, 0x08, 0x3E, 0x08, 0x08,
			0x00, 0x80, 0x70, 0x30, 0x00,
			0x08, 0x08, 0x08, 0x08, 0x08,
			0x00, 0x00, 0x60, 0x60, 0x00,
			0x20, 0x10, 0x08, 0x04, 0x02,
			0x3E, 0x51, 0x49, 0x45, 0x3E,
			0x00, 0x42, 0x7F, 0x40, 0x00,
			0x72, 0x49, 0x49, 0x49, 0x46,
			0x21, 0x41, 0x49, 0x4D, 0x33,
			0x18, 0x14, 0x12, 0x7F, 0x10,
			0x27, 0x45, 0x45, 0x45, 0x39,
			0x3C, 0x4A, 0x49, 0x49, 0x31,
			0x41, 0x21, 0x11, 0x09, 0x07,
			0x36, 0x49, 0x49, 0x49, 0x36,
			0x46, 0x49, 0x49, 0x29, 0x1E,
			0x00, 0x00, 0x14, 0x00, 0x00,
			0x00, 0x40, 0x34, 0x00, 0x00,
			0x00, 0x08, 0x14, 0x22, 0x41,
			0x14, 0x14, 0x14, 0x14, 0x14,
			0x00, 0x41, 0x22, 0x14, 0x08,
			0x02, 0x01, 0x59, 0x09, 0x06,
			0x3E, 0x41, 0x5D, 0x59, 0x4E,
			0x7C, 0x12, 0x11, 0x12, 0x7C,
			0x7F, 0x49, 0x49, 0x49, 0x36,
			0x3E, 0x41, 0x41, 0x41, 0x22,
			0x7F, 0x41, 0x41, 0x41, 0x3E,
			0x7F, 0x49, 0x49, 0x49, 0x41,
			0x7F, 0x09, 0x09, 0x09, 0x01,
			0x3E, 0x41, 0x41, 0x51, 0x73,
			0x7F, 0x08, 0x08, 0x08, 0x7F,
			0x00, 0x41, 0x7F, 0x41, 0x00,
			0x20, 0x40, 0x41, 0x3F, 0x01,
			0x7F, 0x08, 0x14, 0x22, 0x41,
			0x7F, 0x40, 0x40, 0x40, 0x40,
			0x7F, 0x02, 0x1C, 0x02, 0x7F,
			0x7F, 0x04, 0x08, 0x10, 0x7F,
			0x3E, 0x41, 0x41, 0x41, 0x3E,
			0x7F, 0x09, 0x09, 0x09, 0x06,
			0x3E, 0x41, 0x51, 0x21, 0x5E,
			0x7F, 0x09, 0x19, 0x29, 0x46,
			0x26, 0x49, 0x49, 0x49, 0x32,
			0x03, 0x01, 0x7F, 0x01, 0x03,
			0x3F, 0x40, 0x40, 0x40, 0x3F,
			0x1F, 0x20, 0x40, 0x20, 0x1F,
			0x3F, 0x40, 0x38, 0x40, 0x3F,
			0x63, 0x14, 0x08, 0x14, 0x63,
			0x03, 0x04, 0x78, 0x04, 0x03,
			0x61, 0x59, 0x49, 0x4D, 0x43,
			0x00, 0x7F, 0x41, 0x41, 0x41,
			0x02, 0x04, 0x08, 0x10, 0x20,
			0x00, 0x41, 0x41, 0x41, 0x7F,
			0x04, 0x02, 0x01, 0x02, 0x04,
			0x40, 0x40, 0x40, 0x40, 0x40,
			0x00, 0x03, 0x07, 0x08, 0x00,
			0x20, 0x54, 0x54, 0x78, 0x40,
			0x7F, 0x28, 0x44, 0x44, 0x38,
			0x38, 0x44, 0x44, 0x44, 0x28,
			0x38, 0x44, 0x44, 0x28, 0x7F,
			0x38, 0x54, 0x54, 0x54, 0x18,
			0x00, 0x08, 0x7E, 0x09, 0x02,
			0x18, 0xA4, 0xA4, 0x9C, 0x78,
			0x7F, 0x08, 0x04, 0x04, 0x78,
			0x00, 0x44, 0x7D, 0x40, 0x00,
			0x20, 0x40, 0x40, 0x3D, 0x00,
			0x7F, 0x10, 0x28, 0x44, 0x00,
			0x00, 0x41, 0x7F, 0x40, 0x00,
			0x7C, 0x04, 0x78, 0x04, 0x78,
			0x7C, 0x08, 0x04, 0x04, 0x78,
			0x38, 0x44, 0x44, 0x44, 0x38,
			0xFC, 0x18, 0x24, 0x24, 0x18,
			0x18, 0x24, 0x24, 0x18, 0xFC,
			0x7C, 0x08, 0x04, 0x04, 0x08,
			0x48, 0x54, 0x54, 0x54, 0x24,
			0x04, 0x04, 0x3F, 0x44, 0x24,
			0x3C, 0x40, 0x40, 0x20, 0x7C,
			0x1C, 0x20, 0x40, 0x20, 0x1C,
			0x3C, 0x40, 0x30, 0x40, 0x3C,
			0x44, 0x28, 0x10, 0x28, 0x44,
			0x4C, 0x90, 0x90, 0x90, 0x7C,
			0x44, 0x64, 0x54, 0x4C, 0x44,
			0x00, 0x08, 0x36, 0x41, 0x00,
			0x00, 0x00, 0x77, 0x00, 0x00,
			0x00, 0x41, 0x36, 0x08, 0x00,
			0x02, 0x01, 0x02, 0x04, 0x02,
};

#endif /* FONT_8X5_H_ */

````

**fonts.h**:
````c
#ifndef FONTS_FONTS_H_
#define FONTS_FONTS_H_

//
//	Set fonts you want to use
//
#define FONT_8x5 1

//
//	Automatic includes
//
#if(FONT_8x5 ==1)
#include "font_8x5.h"
#endif

#endif /* FONTS_FONTS_H_ */
````

## 🎨 2. Graphics Engine — GFX_BW.c / GFX_BW.h
**This is a lightweight graphics library for monochrome displays.
It provides:**

* pixel drawing

* lines (Bresenham)

* rectangles (filled & unfilled)

* circles and rounded rectangles

* triangles (filled & unfilled)

* bitmap rendering

* text rendering with scalable font size

**The engine uses SSD1306 as the backend:**
````
#define GFX_DrawPixel(x,y,color) SSD1306_DrawPixel(x,y,color)
````
**Text Rendering**
Characters are drawn column‑by‑column:
````
uint8_t line = font[(chr-0x20) * font[1] + i + 2];
````
Scaling is supported via GFX_SetFontSize().

**Bitmap Rendering**
Bitmaps are drawn using 1‑bit packed data:
````
if(img[j * byteWidth + i/8] & (128 >> (i&7))) GFX_DrawPixel(...)
````
Rotation support exists but is disabled (USING_IMAGE_ROTATE 0).



**GFX_BW.c**:
````c

#include "main.h"

#include "OLED_SSD1306.h"
#include "GFX_BW.h"

#if USING_LINES == 1
#include <stdlib.h> // for abs() function
#endif
#if USING_IMAGE_ROTATE == 1
#include <math.h>
#endif

#define _swap_int(a, b) { int t = a; a = b; b = t; }

#if  USING_STRINGS == 1
const uint8_t* font;
uint8_t size = 1;

void GFX_SetFont(const uint8_t* font_t)
{
	font = font_t;
}

void GFX_SetFontSize(uint8_t size_t)
{
	if(size_t != 0)
		size = size_t;
}

uint8_t GFX_GetFontHeight(void)
{
	return font[0];
}

uint8_t GFX_GetFontWidth(void)
{
	return font[1];
}

uint8_t GFX_GetFontSize(void)
{
	return size;
}

void GFX_DrawChar(int x, int y, char chr, uint8_t color, uint8_t background)
{
	if(chr > 0x7E) return; // chr > '~'

	for(uint8_t i=0; i<font[1]; i++ )
	{
        uint8_t line = (uint8_t)font[(chr-0x20) * font[1] + i + 2];

        for(int8_t j=0; j<font[0]; j++, line >>= 1)
        {
            if(line & 1)
            {
            	if(size == 1)
            		GFX_DrawPixel(x+i, y+j, color);
            	else
            		GFX_DrawFillRectangle(x+i*size, y+j*size, size, size, color);
            }
            else if(background == 0)
            {
            	if(size == 1)
					GFX_DrawPixel(x+i, y+j, background);
				else
					GFX_DrawFillRectangle(x+i*size, y+j*size, size, size, background);
            }
        }
    }
}

void GFX_DrawString(int x, int y, char* str, uint8_t color, uint8_t background)
{
	int x_tmp = x;
	char znak;
	znak = *str;
	while(*str++)
	{
		GFX_DrawChar(x_tmp, y, znak, color, background);
		x_tmp += ((uint8_t)font[1] * size) + 1;
		if(background == 0)
		{
			for(uint8_t i=0; i<(font[0]*size); i++)
			{
				GFX_DrawPixel(x_tmp-1, y+i, PIXEL_BLACK);
			}
		}
		znak = *str;
	}
}
#endif
#if USING_LINES == 1
void GFX_WriteLine(int x_start, int y_start, int x_end, int y_end, uint8_t color)
{
	int16_t steep = abs(y_end - y_start) > abs(x_end - x_start);

	    if (steep) {
	        _swap_int(x_start, y_start);
	        _swap_int(x_end, y_end);
	    }

	    if (x_start > x_end) {
	        _swap_int(x_start, x_end);
	        _swap_int(y_start, y_end);
	    }

	    int16_t dx, dy;
	    dx = x_end - x_start;
	    dy = abs(y_end - y_start);

	    int16_t err = dx / 2;
	    int16_t ystep;

	    if (y_start < y_end) {
	        ystep = 1;
	    } else {
	        ystep = -1;
	    }

	    for (; x_start<=x_end; x_start++) {
	        if (steep) {
	        	GFX_DrawPixel(y_start, x_start, color);
	        } else {
	        	GFX_DrawPixel(x_start, y_start, color);
	        }
	        err -= dy;
	        if (err < 0) {
	            y_start += ystep;
	            err += dx;
	        }
	    }
}

void GFX_DrawFastVLine(int x_start, int y_start, int h, uint8_t color)
{
	GFX_WriteLine(x_start, y_start, x_start, y_start+h-1, color);
}

void GFX_DrawFastHLine(int x_start, int y_start, int w, uint8_t color)
{
	GFX_WriteLine(x_start, y_start, x_start+w-1, y_start, color);
}

void GFX_DrawLine(int x_start, int y_start, int x_end, int y_end, uint8_t color)
{
	if(x_start == x_end){
	        if(y_start > y_end) _swap_int(y_start, y_end);
	        GFX_DrawFastVLine(x_start, y_start, y_end - y_start + 1, color);
	    } else if(y_start == y_end){
	        if(x_start > x_end) _swap_int(x_start, x_end);
	        GFX_DrawFastHLine(x_start, y_start, x_end - x_start + 1, color);
	    } else {

	    	GFX_WriteLine(x_start, y_start, x_end, y_end, color);
	    }
}
#endif
#if USING_RECTANGLE == 1
void GFX_DrawRectangle(int x, int y, uint16_t w, uint16_t h, uint8_t color)
{

    GFX_DrawFastHLine(x, y, w, color);
    GFX_DrawFastHLine(x, y+h-1, w, color);
    GFX_DrawFastVLine(x, y, h, color);
    GFX_DrawFastVLine(x+w-1, y, h, color);

}
#endif
#if USING_FILL_RECTANGLE == 1
void GFX_DrawFillRectangle(int x, int y, uint16_t w, uint16_t h, uint8_t color)
{
    for (int i=x; i<x+w; i++) {
    	GFX_DrawFastVLine(i, y, h, color);
    }

}
#endif
#if USING_CIRCLE == 1
void GFX_DrawCircle(int x0, int y0, uint16_t r, uint8_t color)
{
    int16_t f = 1 - r;
    int16_t ddF_x = 1;
    int16_t ddF_y = -2 * r;
    int16_t x = 0;
    int16_t y = r;

    GFX_DrawPixel(x0  , y0+r, color);
    GFX_DrawPixel(x0  , y0-r, color);
    GFX_DrawPixel(x0+r, y0  , color);
    GFX_DrawPixel(x0-r, y0  , color);

    while (x<y) {
        if (f >= 0) {
            y--;
            ddF_y += 2;
            f += ddF_y;
        }
        x++;
        ddF_x += 2;
        f += ddF_x;

        GFX_DrawPixel(x0 + x, y0 + y, color);
        GFX_DrawPixel(x0 - x, y0 + y, color);
        GFX_DrawPixel(x0 + x, y0 - y, color);
        GFX_DrawPixel(x0 - x, y0 - y, color);
        GFX_DrawPixel(x0 + y, y0 + x, color);
        GFX_DrawPixel(x0 - y, y0 + x, color);
        GFX_DrawPixel(x0 + y, y0 - x, color);
        GFX_DrawPixel(x0 - y, y0 - x, color);
    }

}
#endif
#ifdef CIRCLE_HELPER
void GFX_DrawCircleHelper( int x0, int y0, uint16_t r, uint8_t cornername, uint8_t color)
{
    int16_t f     = 1 - r;
    int16_t ddF_x = 1;
    int16_t ddF_y = -2 * r;
    int16_t x     = 0;
    int16_t y     = r;

    while (x<y) {
        if (f >= 0) {
            y--;
            ddF_y += 2;
            f     += ddF_y;
        }
        x++;
        ddF_x += 2;
        f     += ddF_x;
        if (cornername & 0x4) {
            GFX_DrawPixel(x0 + x, y0 + y, color);
            GFX_DrawPixel(x0 + y, y0 + x, color);
        }
        if (cornername & 0x2) {
            GFX_DrawPixel(x0 + x, y0 - y, color);
            GFX_DrawPixel(x0 + y, y0 - x, color);
        }
        if (cornername & 0x8) {
            GFX_DrawPixel(x0 - y, y0 + x, color);
            GFX_DrawPixel(x0 - x, y0 + y, color);
        }
        if (cornername & 0x1) {
            GFX_DrawPixel(x0 - y, y0 - x, color);
            GFX_DrawPixel(x0 - x, y0 - y, color);
        }
    }
}
#endif
#ifdef FILL_CIRCLE_HELPER
void GFX_DrawFillCircleHelper(int x0, int y0, uint16_t r, uint8_t cornername, int16_t delta, uint8_t color)
{

    int16_t f     = 1 - r;
    int16_t ddF_x = 1;
    int16_t ddF_y = -2 * r;
    int16_t x     = 0;
    int16_t y     = r;

    while (x<y) {
        if (f >= 0) {
            y--;
            ddF_y += 2;
            f     += ddF_y;
        }
        x++;
        ddF_x += 2;
        f     += ddF_x;

        if (cornername & 0x1) {
            GFX_DrawFastVLine(x0+x, y0-y, 2*y+1+delta, color);
            GFX_DrawFastVLine(x0+y, y0-x, 2*x+1+delta, color);
        }
        if (cornername & 0x2) {
            GFX_DrawFastVLine(x0-x, y0-y, 2*y+1+delta, color);
            GFX_DrawFastVLine(x0-y, y0-x, 2*x+1+delta, color);
        }
    }
}
#endif
#if USING_FILL_CIRCLE == 1
void GFX_DrawFillCircle(int x0, int y0, uint16_t r, uint8_t color)
{

	GFX_DrawFastVLine(x0, y0-r, 2*r+1, color);
    GFX_DrawFillCircleHelper(x0, y0, r, 3, 0, color);
}
#endif
#if USING_ROUND_RECTANGLE == 1
void GFX_DrawRoundRectangle(int x, int y, uint16_t w, uint16_t h, uint16_t r, uint8_t color)
{
	GFX_DrawFastHLine(x+r  , y    , w-2*r, color); // Top
    GFX_DrawFastHLine(x+r  , y+h-1, w-2*r, color); // Bottom
    GFX_DrawFastVLine(x    , y+r  , h-2*r, color); // Left
    GFX_DrawFastVLine(x+w-1, y+r  , h-2*r, color); // Right
    // draw four corners
    GFX_DrawCircleHelper(x+r    , y+r    , r, 1, color);
    GFX_DrawCircleHelper(x+w-r-1, y+r    , r, 2, color);
    GFX_DrawCircleHelper(x+w-r-1, y+h-r-1, r, 4, color);
    GFX_DrawCircleHelper(x+r    , y+h-r-1, r, 8, color);
}
#endif
#if USING_FILL_ROUND_RECTANGLE == 1
void GFX_DrawFillRoundRectangle(int x, int y, uint16_t w, uint16_t h, uint16_t r, uint8_t color)
{
    // smarter version

	GFX_DrawFillRectangle(x+r, y, w-2*r, h, color);

    // draw four corners
	GFX_DrawFillCircleHelper(x+w-r-1, y+r, r, 1, h-2*r-1, color);
	GFX_DrawFillCircleHelper(x+r    , y+r, r, 2, h-2*r-1, color);
}
#endif
#if USING_TRIANGLE == 1
void GFX_DrawTriangle(int x0, int y0, int x1, int y1, int x2, int y2, uint8_t color)
{
	GFX_DrawLine(x0, y0, x1, y1, color);
    GFX_DrawLine(x1, y1, x2, y2, color);
    GFX_DrawLine(x2, y2, x0, y0, color);
}
#endif
#if USING_FILL_TRIANGLE == 1
void GFX_DrawFillTriangle(int x0, int y0, int x1, int y1, int x2, int y2, uint8_t color)
{

    int16_t a, b, y, last;

    // Sort coordinates by Y order (y2 >= y1 >= y0)
    if (y0 > y1) {
    	_swap_int(y0, y1); _swap_int(x0, x1);
    }
    if (y1 > y2) {
    	_swap_int(y2, y1); _swap_int(x2, x1);
    }
    if (y0 > y1) {
    	_swap_int(y0, y1); _swap_int(x0, x1);
    }

    if(y0 == y2) { // Handle awkward all-on-same-line case as its own thing
        a = b = x0;
        if(x1 < a)      a = x1;
        else if(x1 > b) b = x1;
        if(x2 < a)      a = x2;
        else if(x2 > b) b = x2;
        GFX_DrawFastHLine(a, y0, b-a+1, color);
        return;
    }

    int16_t
    dx01 = x1 - x0,
    dy01 = y1 - y0,
    dx02 = x2 - x0,
    dy02 = y2 - y0,
    dx12 = x2 - x1,
    dy12 = y2 - y1;
    int32_t
    sa   = 0,
    sb   = 0;

    // For upper part of triangle, find scanline crossings for segments
    // 0-1 and 0-2.  If y1=y2 (flat-bottomed triangle), the scanline y1
    // is included here (and second loop will be skipped, avoiding a /0
    // error there), otherwise scanline y1 is skipped here and handled
    // in the second loop...which also avoids a /0 error here if y0=y1
    // (flat-topped triangle).
    if(y1 == y2) last = y1;   // Include y1 scanline
    else         last = y1-1; // Skip it

    for(y=y0; y<=last; y++) {
        a   = x0 + sa / dy01;
        b   = x0 + sb / dy02;
        sa += dx01;
        sb += dx02;
        /* longhand:
        a = x0 + (x1 - x0) * (y - y0) / (y1 - y0);
        b = x0 + (x2 - x0) * (y - y0) / (y2 - y0);
        */
        if(a > b) _swap_int(a,b);
        GFX_DrawFastHLine(a, y, b-a+1, color);
    }

    // For lower part of triangle, find scanline crossings for segments
    // 0-2 and 1-2.  This loop is skipped if y1=y2.
    sa = dx12 * (y - y1);
    sb = dx02 * (y - y0);
    for(; y<=y2; y++) {
        a   = x1 + sa / dy12;
        b   = x0 + sb / dy02;
        sa += dx12;
        sb += dx02;
        /* longhand:
        a = x1 + (x2 - x1) * (y - y1) / (y2 - y1);
        b = x0 + (x2 - x0) * (y - y0) / (y2 - y0);
        */
        if(a > b) _swap_int(a,b);
        GFX_DrawFastHLine(a, y, b-a+1, color);
    }
}
#endif
#if USING_IMAGE == 1
#if AVR_USING == 1
void GFX_Image_P(int x, int y, uint8_t *img, uint8_t w, uint8_t h, uint8_t color)
{
	uint8_t i, j, byteWidth = (w+7)/8;

	for(j = 0; j < h; j++)
	{
		for(i = 0; i < w; i++)
		{
			if(pgm_read_byte(img + j *byteWidth + i /8) & (128 >> (i&7)) )
				GFX_DrawPixel(x+i, y+j, color);
		}
	}
}
#endif
#if STM32_USING ==1
void GFX_Image(int x, int y, const uint8_t *img, uint8_t w, uint8_t h, uint8_t color)
{
	uint8_t i, j, byteWidth = (w+7)/8;

	for(j = 0; j < h; j++)
	{
		for(i = 0; i < w; i++)
		{
			if(img[j *byteWidth + i /8] & (128 >> (i&7)) )
				GFX_DrawPixel(x+i, y+j, color);
		}
	}
}
#if USING_IMAGE_ROTATE == 1

const double sinus_LUT[] =
{// 0* to 90* = 91 valuse
		0.0000,
		0.0175,
		0.0349,
		0.0523,
		0.0698,
		0.0872,
		0.1045,
		0.1219,
		0.1392,
		0.1564,
		0.1736,
		0.1908,
		0.2079,
		0.2250,
		0.2419,
		0.2588,
		0.2756,
		0.2924,
		0.3090,
		0.3256,
		0.3420,
		0.3584,
		0.3746,
		0.3907,
		0.4067,
		0.4226,
		0.4384,
		0.4540,
		0.4695,
		0.4848,
		0.5000,
		0.5150,
		0.5299,
		0.5446,
		0.5592,
		0.5736,
		0.5878,
		0.6018,
		0.6157,
		0.6293,
		0.6428,
		0.6561,
		0.6691,
		0.6820,
		0.6947,
		0.7071,
		0.7193,
		0.7314,
		0.7431,
		0.7547,
		0.7660,
		0.7771,
		0.7880,
		0.7986,
		0.8090,
		0.8192,
		0.8290,
		0.8387,
		0.8480,
		0.8572,
		0.8660,
		0.8746,
		0.8829,
		0.8910,
		0.8988,
		0.9063,
		0.9135,
		0.9205,
		0.9272,
		0.9336,
		0.9397,
		0.9455,
		0.9511,
		0.9563,
		0.9613,
		0.9659,
		0.9703,
		0.9744,
		0.9781,
		0.9816,
		0.9848,
		0.9877,
		0.9903,
		0.9925,
		0.9945,
		0.9962,
		0.9976,
		0.9986,
		0.9994,
		0.9998,
		1.0000,
};

double sinus(uint16_t angle)
{
	angle %= 360;
	if((angle >= 0) && (angle < 90)) return sinus_LUT[angle];
	if((angle >= 90) && (angle < 180)) return sinus_LUT[180 - angle];
	if((angle >= 180) && (angle < 270)) return -(sinus_LUT[angle - 180]);
	if((angle >= 270) && (angle < 360)) return -(sinus_LUT[180 - (angle - 180)]);
	return 0; // will be never here
}

void GFX_ImageRotate(int x, int y, const uint8_t *img, uint8_t w, uint8_t h, uint8_t color, uint16_t angle)
{
	angle %= 360;

	uint8_t wHalf = w/2;
	uint8_t hHalf = h/2;

	uint8_t i, j, byteWidth = (w+7)/8;

	double sinma = sinus(angle);
	double cosma = sinus(angle + 90);

	for(j = 0; j < h; j++)
	{
		for(i = 0; i < w; i++)
		{
			if(img[j *byteWidth + i /8] & (128 >> (i&7)) )
			{
				int xt = i - wHalf;
				int yt = j - hHalf;

				int xs = (xt*cosma - yt*sinma) + wHalf;
				int ys = (xt*sinma + yt*cosma) + hHalf;

				GFX_DrawPixel(xs+x, ys+y, color);
			}
		}
	}
}
#endif
#endif
#endif
````
**GFX_BW.h**:
````c


#ifndef GFX_BW_H_
#define GFX_BW_H_

/***************************************************************
 *
 * 		SETTINGS
 *
 * 		Please set what functionality you want to use.
 * 		Some functions need other functionalities. It should works automatically.
 *
 * 		1 - will be compiled
 * 		0 - won't be compiled
 *
 * */
#define AVR_USING 0
#define STM32_USING 1

#define GFX_DrawPixel(x,y,color) SSD1306_DrawPixel(x,y,color)
#define WIDTH SSD1306_LCDWIDTH
#define HEIGHT SSD1306_LCDHEIGHT
#define PIXEL_BLACK	BLACK
#define PIXEL_WHITE	WHITE
#define PIXEL_INVERSE	INVERSE

#define USING_STRINGS 1 // 0 - do not compile, 1 - compile

#define USING_IMAGE 1
#if USING_IMAGE == 1
#define USING_IMAGE_ROTATE 0
#endif

// Trygonometric graphic functions
#define USING_RECTANGLE 1
#define USING_CIRCLE 1
#define USING_FILL_CIRCLE 1
#define USING_ROUND_RECTANGLE 1
#define USING_FILL_ROUND_RECTANGLE 1
#define USING_TRIANGLE 1
#define USING_FILL_TRIANGLE 1
#if ((USING_FILL_ROUND_RECTANGLE == 0) && (USING_STRINGS == 0))
#define USING_FILL_RECTANGLE 0
#endif
#if (USING_RECTANGLE == 0) && (USING_FILL_RECTANGLE == 0) && (USING_FILL_CIRCLE == 0) && (USING_ROUND_RECTANGLE == 0) && (USING_TRIANGLE == 0) && (USING_FILL_TRIANGLE == 0)
#define USING_LINES 0
#endif

/****************************************************************/

#if (USING_FILL_ROUND_RECTANGLE == 1 || USING_STRINGS == 1)
#define USING_FILL_RECTANGLE 1
#endif
#if (USING_RECTANGLE == 1) || (USING_FILL_RECTANGLE == 1) || (USING_FILL_CIRCLE == 1) || (USING_ROUND_RECTANGLE == 1) || (USING_TRIANGLE == 1) || (USING_FILL_TRIANGLE == 1)
#define USING_LINES 1
#endif
#if USING_ROUND_RECTANGLE == 1
#define CIRCLE_HELPER
#endif
#if (USING_FILL_CIRCLE == 1) || (USING_FILL_ROUND_RECTANGLE == 1)
#define FILL_CIRCLE_HELPER
#endif

#if USING_STRINGS == 1
/*
 *
 */
void GFX_SetFont(const uint8_t* font_t);
void GFX_SetFontSize(uint8_t size_t);
uint8_t GFX_GetFontHeight(void);
uint8_t GFX_GetFontWidth(void);
uint8_t  GFX_GetFontSize(void);
void GFX_DrawChar(int x, int y, char chr, uint8_t color, uint8_t background);
void GFX_DrawString(int x, int y, char* str, uint8_t color, uint8_t background);
#endif

#if USING_LINES == 1
void GFX_DrawLine(int x_start, int y_start, int x_end, int y_end, uint8_t color);
#endif

#if USING_RECTANGLE == 1
void GFX_DrawRectangle(int x, int y, uint16_t w, uint16_t h, uint8_t color);
#endif
#if USING_FILL_RECTANGLE ==1
void GFX_DrawFillRectangle(int x, int y, uint16_t w, uint16_t h, uint8_t color);
#endif
#if USING_CIRCLE == 1
void GFX_DrawCircle(int x0, int y0, uint16_t r, uint8_t color);
#endif
#if USING_FILL_CIRCLE == 1
void GFX_DrawFillCircle(int x0, int y0, uint16_t r, uint8_t color);
#endif
#if USING_ROUND_RECTANGLE == 1
void GFX_DrawRoundRectangle(int x, int y, uint16_t w, uint16_t h, uint16_t r, uint8_t color);
#endif
#if USING_FILL_ROUND_RECTANGLE == 1
void GFX_DrawFillRoundRectangle(int x, int y, uint16_t w, uint16_t h, uint16_t r, uint8_t color);
#endif
#if USING_TRIANGLE == 1
void GFX_DrawTriangle(int x0, int y0, int x1, int y1, int x2, int y2, uint8_t color);
#endif
#if USING_FILL_TRIANGLE == 1
void GFX_DrawFillTriangle(int x0, int y0, int x1, int y1, int x2, int y2, uint8_t color);
#endif
#if USING_IMAGE == 1
#if AVR_USING ==1
void GFX_Image_P(int x, int y, uint8_t *img, uint8_t w, uint8_t h, uint8_t color);
#endif
#if STM32_USING ==1
void GFX_Image(int x, int y, const uint8_t *img, uint8_t w, uint8_t h, uint8_t color);
#if USING_IMAGE_ROTATE == 1
void GFX_ImageRotate(int x, int y, const uint8_t *img, uint8_t w, uint8_t h, uint8_t color, uint16_t angle);
#endif
#endif
#endif

#endif /* GFX_BW_H_ */
````
## 📡 4. BLE Application — STM32WB55
**The project uses the STM32WB wireless coprocessor.
Initialization follows ST’s standard BLE template:**

* MX_APPE_Config()

* MX_APPE_Init()

* MX_APPE_Process()

**The BLE stack must be flashed separately as noted in the header comment.**

##🌡 5. Main Application Logic — main.c
The main loop updates the OLED with temperature and pressure values received from BLE.

**Display Initialization**
A splash screen is shown:
````
SSD1306_Bitmap((uint8_t*)picture);
````
Then the font is configured:
````
GFX_SetFont(font_8x5);
GFX_SetFontSize(1);
````
**Periodic Update**
Every frame, the display is cleared and redrawn:

````c
SSD1306_Clear(BLACK);

sprintf(txt_temp, "TEMP: %d C", temp);
GFX_DrawString(10, 10, txt_temp, WHITE, BLACK);

sprintf(txt_press, "PRES: %lu hPa", pressure / 100);
GFX_DrawString(10, 20, txt_press, WHITE, BLACK);

SSD1306_Display();
````
The variables temp and pressure are updated externally (e.g., via BLE notifications).

**DMA Optimization**
The screen is updated only when the I2C DMA channel is idle:
````
if(hi2c1.hdmatx->State == HAL_DMA_STATE_READY)
````
This prevents tearing and ensures smooth refresh.

##🧩 6. Hardware & Peripheral Setup
**The project configures:**

* I2C1 for OLED

* DMA for I2C and UART

* RNG

* RTC

* RF subsystem

* GPIO

Clock configuration uses HSE as SYSCLK:
````
RCC_ClkInitStruct.SYSCLKSource = RCC_SYSCLKSOURCE_HSE;
````


**main.c**:
````c
/* USER CODE BEGIN Header */
/**
  ******************************************************************************
 * @file    main.c
 * @author  MCD Application Team
 * @brief   BLE application with BLE core
 *
  ******************************************************************************
  * @attention
  *
  * Copyright (c) 2019-2021 STMicroelectronics.
  * All rights reserved.
  *
  * This software is licensed under terms that can be found in the LICENSE file
  * in the root directory of this software component.
  * If no LICENSE file comes with this software, it is provided AS-IS.
  *
  ******************************************************************************
  @verbatim
  ==============================================================================
                    ##### IMPORTANT NOTE #####
  ==============================================================================

  This application requests having the stm32wb5x_BLE_Stack_fw.bin binary
  flashed on the Wireless Coprocessor.
  If it is not the case, you need to use STM32CubeProgrammer to load the appropriate
  binary.

  All available binaries are located under following directory:
  /Projects/STM32_Copro_Wireless_Binaries

  Refer to UM2237 to learn how to use/install STM32CubeProgrammer.
  Refer to /Projects/STM32_Copro_Wireless_Binaries/ReleaseNote.html for the
  detailed procedure to change the Wireless Coprocessor binary.

  @endverbatim
  ******************************************************************************
  */
/* USER CODE END Header */
/* Includes ------------------------------------------------------------------*/
#include "main.h"

/* Private includes ----------------------------------------------------------*/
/* USER CODE BEGIN Includes */
#include "stdio.h"
#include "stdlib.h"
#include "string.h"
#include "OLED_SSD1306.h"
#include "GFX_BW.h"
#include "fonts.h"
#include "picture.h"
/* USER CODE END Includes */

/* Private typedef -----------------------------------------------------------*/
/* USER CODE BEGIN PTD */

/* USER CODE END PTD */

/* Private define ------------------------------------------------------------*/
/* USER CODE BEGIN PD */

/* USER CODE END PD */

/* Private macro -------------------------------------------------------------*/
/* USER CODE BEGIN PM */

/* USER CODE END PM */

/* Private variables ---------------------------------------------------------*/
I2C_HandleTypeDef hi2c1;
DMA_HandleTypeDef hdma_i2c1_tx;

IPCC_HandleTypeDef hipcc;

UART_HandleTypeDef hlpuart1;
UART_HandleTypeDef huart1;
DMA_HandleTypeDef hdma_lpuart1_tx;
DMA_HandleTypeDef hdma_usart1_tx;

RNG_HandleTypeDef hrng;

RTC_HandleTypeDef hrtc;

/* USER CODE BEGIN PV */
volatile uint16_t timer_1s;
extern uint8_t temp;
extern uint32_t pressure;


uint8_t screen = 0;
uint8_t last_state = 0;


/* USER CODE END PV */

/* Private function prototypes -----------------------------------------------*/
void SystemClock_Config(void);
void PeriphCommonClock_Config(void);
static void MX_GPIO_Init(void);
static void MX_DMA_Init(void);
static void MX_RTC_Init(void);
static void MX_IPCC_Init(void);
static void MX_RNG_Init(void);
static void MX_I2C1_Init(void);
static void MX_RF_Init(void);
/* USER CODE BEGIN PFP */

/* USER CODE END PFP */

/* Private user code ---------------------------------------------------------*/
/* USER CODE BEGIN 0 */

/* USER CODE END 0 */

/**
  * @brief  The application entry point.
  * @retval int
  */
int main(void)
{

  /* USER CODE BEGIN 1 */

  /* USER CODE END 1 */

  /* MCU Configuration--------------------------------------------------------*/

  /* Reset of all peripherals, Initializes the Flash interface and the Systick. */
  HAL_Init();
  /* Config code for STM32_WPAN (HSE Tuning must be done before system clock configuration) */
  MX_APPE_Config();

  /* USER CODE BEGIN Init */

  /* USER CODE END Init */

  /* Configure the system clock */
  SystemClock_Config();

  /* Configure the peripherals common clocks */
  PeriphCommonClock_Config();

  /* IPCC initialisation */
  MX_IPCC_Init();

  /* USER CODE BEGIN SysInit */

  /* USER CODE END SysInit */

  /* Initialize all configured peripherals */
  MX_GPIO_Init();
  MX_DMA_Init();
  MX_RTC_Init();
  MX_RNG_Init();
  MX_I2C1_Init();
  MX_RF_Init();
  /* USER CODE BEGIN 2 */
  MX_DMA_Init();
  SSD1306_I2cInit(&hi2c1);





  SSD1306_Bitmap((uint8_t*)picture);
    HAL_Delay(5000);
    GFX_SetFont(font_8x5);
    GFX_SetFontSize(1);

    uint16_t frames = 0, fps = 0;
    uint32_t loops = 0, loops_overal = 0;
    char fps_c[20];


    RTC_TimeTypeDef rtcTime = {0};
    RTC_DateTypeDef rtcDate = {0};

    rtcTime.Hours = 18;
    rtcTime.Minutes = 37;
    rtcTime.Seconds = 0;

    rtcDate.WeekDay = RTC_WEEKDAY_THURSDAY;
    rtcDate.Date = 15;
    rtcDate.Month = 6;
    rtcDate.Year = 26; // 2025


    HAL_RTC_SetTime(&hrtc, &rtcTime, RTC_FORMAT_BIN);
    HAL_RTC_SetDate(&hrtc, &rtcDate, RTC_FORMAT_BIN);




  /* USER CODE END 2 */

  /* Init code for STM32_WPAN */
  MX_APPE_Init();

  /* Infinite loop */
  /* USER CODE BEGIN WHILE */
  while(1)
  	{
	  HAL_RTC_GetTime(&hrtc, &rtcTime, RTC_FORMAT_BIN);
	  HAL_RTC_GetDate(&hrtc, &rtcDate, RTC_FORMAT_BIN);











	  uint8_t pin = HAL_GPIO_ReadPin(next_screen_GPIO_Port, next_screen_Pin);

	  if (pin == GPIO_PIN_SET && last_state == GPIO_PIN_RESET)
	  {
		  screen++;
		  if (screen > 3)
		  {
		      screen = 0;
		  }
	  }

	  last_state = pin;




















	      if(!timer_1s)
	      {
	          timer_1s = 1000;
	          fps = frames;
	          frames = 0;
	          loops_overal = loops;
	          loops = 0;
	      }










	      if(hi2c1.hdmatx->State == HAL_DMA_STATE_READY)
	      {
	          SSD1306_Clear(BLACK);

	          if (screen == 0)
	          {
	              // -------------------------
	              // EKRAN 1 – Twój aktualny
	              // -------------------------
	              GFX_DrawLine(0, 0, 127, 0, WHITE);
	              GFX_DrawLine(0, 63, 127, 63, WHITE);
	              GFX_DrawLine(0, 0, 0, 63, WHITE);
	              GFX_DrawLine(127, 0, 127, 63, WHITE);

	              GFX_DrawString(2, 3, "Gothenburg", WHITE, BLACK);

	              char txt_time[10];
	              sprintf(txt_time, "%02d:%02d", rtcTime.Hours, rtcTime.Minutes);
	              GFX_DrawString(70, 3, txt_time, WHITE, BLACK);

	              GFX_DrawLine(0, 25, 127, 25, WHITE);

	              char txt_date[20];
	              sprintf(txt_date, "%02d.%02d.%04d", rtcDate.Date, rtcDate.Month, 2000 + rtcDate.Year);
	              GFX_DrawString(2, 15, txt_date, WHITE, BLACK);

	              char txt_temp[20];
	              sprintf(txt_temp, "Temperature: %d C", temp);
	              GFX_DrawString(2, 30, txt_temp, WHITE, BLACK);

	              char txt_press[20];
	              sprintf(txt_press, "Pressure: %lu hPa", pressure / 100);
	              GFX_DrawString(2, 45, txt_press, WHITE, BLACK);
	          }

	          else if (screen == 1)
	          {
	              // -------------------------
	              // EKRAN 2 – przykładowy
	              // -------------------------
	              GFX_DrawString(10, 10, "SECOND SCREEN", WHITE, BLACK);
	              GFX_DrawLine(0, 20, 127, 20, WHITE);
	              if(temp>=28)
	              {
	              GFX_DrawString(10, 30, "Actually sunny!", WHITE, BLACK);
	              }
	              else if(temp<28)
	              GFX_DrawString(10, 30, "Actually cloudy!", WHITE, BLACK);
	              GFX_DrawString(10, 45, "Press button to ", WHITE, BLACK);
	              GFX_DrawString(10, 55, " exit second screen", WHITE, BLACK);
	          }
	          else if (screen == 2)
	          {
	              GFX_DrawString(10, 10, "THIRD SCREEN", WHITE, BLACK);
	              GFX_DrawLine(0, 20, 127, 20, WHITE);

	              char buf[30];
	              GFX_DrawString(10, 30, "Uptime of", WHITE, BLACK);
	              sprintf(buf, "measure: %lu s", HAL_GetTick() / 1000);
	              GFX_DrawString(10, 45, buf, WHITE, BLACK);

	              if (pressure > 101000)
	                  GFX_DrawString(10, 55, "High pressure", WHITE, BLACK);
	              else
	                  GFX_DrawString(10, 55, "Normal pressure", WHITE, BLACK);
	          }
	          else if (screen == 3)
	          {
	          	  GFX_DrawString(10, 10, "FOUR SCREEN", WHITE, BLACK);
	          	  GFX_DrawLine(0, 20, 127, 20, WHITE);


	          	}

	          SSD1306_Display();
	      }

  			    HAL_GPIO_WritePin(TEST_GPIO_Port, TEST_Pin, 0);
  			    loops++;











    /* USER CODE END WHILE */
    MX_APPE_Process();

    /* USER CODE BEGIN 3 */
    }
  /* USER CODE END 3 */
}

/**
  * @brief System Clock Configuration
  * @retval None
  */
void SystemClock_Config(void)
{
  RCC_OscInitTypeDef RCC_OscInitStruct = {0};
  RCC_ClkInitTypeDef RCC_ClkInitStruct = {0};

  /** Configure LSE Drive Capability
  */
  HAL_PWR_EnableBkUpAccess();
  __HAL_RCC_LSEDRIVE_CONFIG(RCC_LSEDRIVE_LOW);

  /** Configure the main internal regulator output voltage
  */
  __HAL_PWR_VOLTAGESCALING_CONFIG(PWR_REGULATOR_VOLTAGE_SCALE1);

  /** Initializes the RCC Oscillators according to the specified parameters
  * in the RCC_OscInitTypeDef structure.
  */
  RCC_OscInitStruct.OscillatorType = RCC_OSCILLATORTYPE_HSI48|RCC_OSCILLATORTYPE_HSI
                              |RCC_OSCILLATORTYPE_HSE|RCC_OSCILLATORTYPE_LSE;
  RCC_OscInitStruct.HSEState = RCC_HSE_ON;
  RCC_OscInitStruct.LSEState = RCC_LSE_ON;
  RCC_OscInitStruct.HSIState = RCC_HSI_ON;
  RCC_OscInitStruct.HSI48State = RCC_HSI48_ON;
  RCC_OscInitStruct.HSICalibrationValue = RCC_HSICALIBRATION_DEFAULT;
  RCC_OscInitStruct.PLL.PLLState = RCC_PLL_NONE;
  if (HAL_RCC_OscConfig(&RCC_OscInitStruct) != HAL_OK)
  {
    Error_Handler();
  }

  /** Configure the SYSCLKSource, HCLK, PCLK1 and PCLK2 clocks dividers
  */
  RCC_ClkInitStruct.ClockType = RCC_CLOCKTYPE_HCLK4|RCC_CLOCKTYPE_HCLK2
                              |RCC_CLOCKTYPE_HCLK|RCC_CLOCKTYPE_SYSCLK
                              |RCC_CLOCKTYPE_PCLK1|RCC_CLOCKTYPE_PCLK2;
  RCC_ClkInitStruct.SYSCLKSource = RCC_SYSCLKSOURCE_HSE;
  RCC_ClkInitStruct.AHBCLKDivider = RCC_SYSCLK_DIV1;
  RCC_ClkInitStruct.APB1CLKDivider = RCC_HCLK_DIV1;
  RCC_ClkInitStruct.APB2CLKDivider = RCC_HCLK_DIV1;
  RCC_ClkInitStruct.AHBCLK2Divider = RCC_SYSCLK_DIV1;
  RCC_ClkInitStruct.AHBCLK4Divider = RCC_SYSCLK_DIV1;

  if (HAL_RCC_ClockConfig(&RCC_ClkInitStruct, FLASH_LATENCY_1) != HAL_OK)
  {
    Error_Handler();
  }
}

/**
  * @brief Peripherals Common Clock Configuration
  * @retval None
  */
void PeriphCommonClock_Config(void)
{
  RCC_PeriphCLKInitTypeDef PeriphClkInitStruct = {0};

  /** Initializes the peripherals clock
  */
  PeriphClkInitStruct.PeriphClockSelection = RCC_PERIPHCLK_SMPS|RCC_PERIPHCLK_RFWAKEUP;
  PeriphClkInitStruct.RFWakeUpClockSelection = RCC_RFWKPCLKSOURCE_LSE;
  PeriphClkInitStruct.SmpsClockSelection = RCC_SMPSCLKSOURCE_HSE;
  PeriphClkInitStruct.SmpsDivSelection = RCC_SMPSCLKDIV_RANGE1;

  if (HAL_RCCEx_PeriphCLKConfig(&PeriphClkInitStruct) != HAL_OK)
  {
    Error_Handler();
  }
  /* USER CODE BEGIN Smps */

  /* USER CODE END Smps */
}

/**
  * @brief I2C1 Initialization Function
  * @param None
  * @retval None
  */
static void MX_I2C1_Init(void)
{

  /* USER CODE BEGIN I2C1_Init 0 */

  /* USER CODE END I2C1_Init 0 */

  /* USER CODE BEGIN I2C1_Init 1 */

  /* USER CODE END I2C1_Init 1 */
  hi2c1.Instance = I2C1;
  hi2c1.Init.Timing = 0x00B07CB4;
  hi2c1.Init.OwnAddress1 = 0;
  hi2c1.Init.AddressingMode = I2C_ADDRESSINGMODE_7BIT;
  hi2c1.Init.DualAddressMode = I2C_DUALADDRESS_DISABLE;
  hi2c1.Init.OwnAddress2 = 0;
  hi2c1.Init.OwnAddress2Masks = I2C_OA2_NOMASK;
  hi2c1.Init.GeneralCallMode = I2C_GENERALCALL_DISABLE;
  hi2c1.Init.NoStretchMode = I2C_NOSTRETCH_DISABLE;
  if (HAL_I2C_Init(&hi2c1) != HAL_OK)
  {
    Error_Handler();
  }

  /** Configure Analogue filter
  */
  if (HAL_I2CEx_ConfigAnalogFilter(&hi2c1, I2C_ANALOGFILTER_ENABLE) != HAL_OK)
  {
    Error_Handler();
  }

  /** Configure Digital filter
  */
  if (HAL_I2CEx_ConfigDigitalFilter(&hi2c1, 0) != HAL_OK)
  {
    Error_Handler();
  }
  /* USER CODE BEGIN I2C1_Init 2 */

  /* USER CODE END I2C1_Init 2 */

}

/**
  * @brief IPCC Initialization Function
  * @param None
  * @retval None
  */
static void MX_IPCC_Init(void)
{

  /* USER CODE BEGIN IPCC_Init 0 */

  /* USER CODE END IPCC_Init 0 */

  /* USER CODE BEGIN IPCC_Init 1 */

  /* USER CODE END IPCC_Init 1 */
  hipcc.Instance = IPCC;
  if (HAL_IPCC_Init(&hipcc) != HAL_OK)
  {
    Error_Handler();
  }
  /* USER CODE BEGIN IPCC_Init 2 */

  /* USER CODE END IPCC_Init 2 */

}

/**
  * @brief LPUART1 Initialization Function
  * @param None
  * @retval None
  */
void MX_LPUART1_UART_Init(void)
{

  /* USER CODE BEGIN LPUART1_Init 0 */

  /* USER CODE END LPUART1_Init 0 */

  /* USER CODE BEGIN LPUART1_Init 1 */

  /* USER CODE END LPUART1_Init 1 */
  hlpuart1.Instance = LPUART1;
  hlpuart1.Init.BaudRate = 115200;
  hlpuart1.Init.WordLength = UART_WORDLENGTH_8B;
  hlpuart1.Init.StopBits = UART_STOPBITS_1;
  hlpuart1.Init.Parity = UART_PARITY_NONE;
  hlpuart1.Init.Mode = UART_MODE_TX_RX;
  hlpuart1.Init.HwFlowCtl = UART_HWCONTROL_NONE;
  hlpuart1.Init.OneBitSampling = UART_ONE_BIT_SAMPLE_DISABLE;
  hlpuart1.Init.ClockPrescaler = UART_PRESCALER_DIV1;
  hlpuart1.AdvancedInit.AdvFeatureInit = UART_ADVFEATURE_NO_INIT;
  hlpuart1.FifoMode = UART_FIFOMODE_DISABLE;
  if (HAL_UART_Init(&hlpuart1) != HAL_OK)
  {
    Error_Handler();
  }
  if (HAL_UARTEx_SetTxFifoThreshold(&hlpuart1, UART_TXFIFO_THRESHOLD_1_8) != HAL_OK)
  {
    Error_Handler();
  }
  if (HAL_UARTEx_SetRxFifoThreshold(&hlpuart1, UART_RXFIFO_THRESHOLD_1_8) != HAL_OK)
  {
    Error_Handler();
  }
  if (HAL_UARTEx_DisableFifoMode(&hlpuart1) != HAL_OK)
  {
    Error_Handler();
  }
  /* USER CODE BEGIN LPUART1_Init 2 */

  /* USER CODE END LPUART1_Init 2 */

}

/**
  * @brief USART1 Initialization Function
  * @param None
  * @retval None
  */
void MX_USART1_UART_Init(void)
{

  /* USER CODE BEGIN USART1_Init 0 */

  /* USER CODE END USART1_Init 0 */

  /* USER CODE BEGIN USART1_Init 1 */

  /* USER CODE END USART1_Init 1 */
  huart1.Instance = USART1;
  huart1.Init.BaudRate = 115200;
  huart1.Init.WordLength = UART_WORDLENGTH_8B;
  huart1.Init.StopBits = UART_STOPBITS_1;
  huart1.Init.Parity = UART_PARITY_NONE;
  huart1.Init.Mode = UART_MODE_TX_RX;
  huart1.Init.HwFlowCtl = UART_HWCONTROL_NONE;
  huart1.Init.OverSampling = UART_OVERSAMPLING_8;
  huart1.Init.OneBitSampling = UART_ONE_BIT_SAMPLE_DISABLE;
  huart1.Init.ClockPrescaler = UART_PRESCALER_DIV1;
  huart1.AdvancedInit.AdvFeatureInit = UART_ADVFEATURE_NO_INIT;
  if (HAL_UART_Init(&huart1) != HAL_OK)
  {
    Error_Handler();
  }
  if (HAL_UARTEx_SetTxFifoThreshold(&huart1, UART_TXFIFO_THRESHOLD_1_8) != HAL_OK)
  {
    Error_Handler();
  }
  if (HAL_UARTEx_SetRxFifoThreshold(&huart1, UART_RXFIFO_THRESHOLD_1_8) != HAL_OK)
  {
    Error_Handler();
  }
  if (HAL_UARTEx_DisableFifoMode(&huart1) != HAL_OK)
  {
    Error_Handler();
  }
  /* USER CODE BEGIN USART1_Init 2 */

  /* USER CODE END USART1_Init 2 */

}

/**
  * @brief RF Initialization Function
  * @param None
  * @retval None
  */
static void MX_RF_Init(void)
{

  /* USER CODE BEGIN RF_Init 0 */

  /* USER CODE END RF_Init 0 */

  /* USER CODE BEGIN RF_Init 1 */

  /* USER CODE END RF_Init 1 */
  /* USER CODE BEGIN RF_Init 2 */

  /* USER CODE END RF_Init 2 */

}

/**
  * @brief RNG Initialization Function
  * @param None
  * @retval None
  */
static void MX_RNG_Init(void)
{

  /* USER CODE BEGIN RNG_Init 0 */

  /* USER CODE END RNG_Init 0 */

  /* USER CODE BEGIN RNG_Init 1 */

  /* USER CODE END RNG_Init 1 */
  hrng.Instance = RNG;
  hrng.Init.ClockErrorDetection = RNG_CED_ENABLE;
  if (HAL_RNG_Init(&hrng) != HAL_OK)
  {
    Error_Handler();
  }
  /* USER CODE BEGIN RNG_Init 2 */

  /* USER CODE END RNG_Init 2 */

}

/**
  * @brief RTC Initialization Function
  * @param None
  * @retval None
  */
static void MX_RTC_Init(void)
{

  /* USER CODE BEGIN RTC_Init 0 */

  /* USER CODE END RTC_Init 0 */

  /* USER CODE BEGIN RTC_Init 1 */

  /* USER CODE END RTC_Init 1 */

  /** Initialize RTC Only
  */
  hrtc.Instance = RTC;
  hrtc.Init.HourFormat = RTC_HOURFORMAT_24;
  hrtc.Init.AsynchPrediv = CFG_RTC_ASYNCH_PRESCALER;
  hrtc.Init.SynchPrediv = CFG_RTC_SYNCH_PRESCALER;
  hrtc.Init.OutPut = RTC_OUTPUT_DISABLE;
  hrtc.Init.OutPutPolarity = RTC_OUTPUT_POLARITY_HIGH;
  hrtc.Init.OutPutType = RTC_OUTPUT_TYPE_OPENDRAIN;
  hrtc.Init.OutPutRemap = RTC_OUTPUT_REMAP_NONE;
  if (HAL_RTC_Init(&hrtc) != HAL_OK)
  {
    Error_Handler();
  }

  /** Enable the WakeUp
  */
  if (HAL_RTCEx_SetWakeUpTimer_IT(&hrtc, 0, RTC_WAKEUPCLOCK_RTCCLK_DIV16) != HAL_OK)
  {
    Error_Handler();
  }
  /* USER CODE BEGIN RTC_Init 2 */

  /* USER CODE END RTC_Init 2 */

}

/**
  * Enable DMA controller clock
  */
static void MX_DMA_Init(void)
{

  /* DMA controller clock enable */
  __HAL_RCC_DMAMUX1_CLK_ENABLE();
  __HAL_RCC_DMA1_CLK_ENABLE();
  __HAL_RCC_DMA2_CLK_ENABLE();

  /* DMA interrupt init */
  /* DMA1_Channel1_IRQn interrupt configuration */
  HAL_NVIC_SetPriority(DMA1_Channel1_IRQn, 0, 0);
  HAL_NVIC_EnableIRQ(DMA1_Channel1_IRQn);
  /* DMA1_Channel4_IRQn interrupt configuration */
  HAL_NVIC_SetPriority(DMA1_Channel4_IRQn, 15, 0);
  HAL_NVIC_EnableIRQ(DMA1_Channel4_IRQn);
  /* DMA2_Channel4_IRQn interrupt configuration */
  HAL_NVIC_SetPriority(DMA2_Channel4_IRQn, 15, 0);
  HAL_NVIC_EnableIRQ(DMA2_Channel4_IRQn);

}

/**
  * @brief GPIO Initialization Function
  * @param None
  * @retval None
  */
static void MX_GPIO_Init(void)
{
  GPIO_InitTypeDef GPIO_InitStruct = {0};
  /* USER CODE BEGIN MX_GPIO_Init_1 */
  /* USER CODE END MX_GPIO_Init_1 */

  /* GPIO Ports Clock Enable */
  __HAL_RCC_GPIOC_CLK_ENABLE();
  __HAL_RCC_GPIOB_CLK_ENABLE();
  __HAL_RCC_GPIOA_CLK_ENABLE();

  /*Configure GPIO pin Output Level */
  HAL_GPIO_WritePin(TEST_GPIO_Port, TEST_Pin, GPIO_PIN_RESET);

  /*Configure GPIO pin : TEST_Pin */
  GPIO_InitStruct.Pin = TEST_Pin;
  GPIO_InitStruct.Mode = GPIO_MODE_OUTPUT_PP;
  GPIO_InitStruct.Pull = GPIO_NOPULL;
  GPIO_InitStruct.Speed = GPIO_SPEED_FREQ_LOW;
  HAL_GPIO_Init(TEST_GPIO_Port, &GPIO_InitStruct);

  /*Configure GPIO pin : next_screen_Pin */
  GPIO_InitStruct.Pin = next_screen_Pin;
  GPIO_InitStruct.Mode = GPIO_MODE_INPUT;
  GPIO_InitStruct.Pull = GPIO_NOPULL;
  HAL_GPIO_Init(next_screen_GPIO_Port, &GPIO_InitStruct);

  /* USER CODE BEGIN MX_GPIO_Init_2 */
  /* USER CODE END MX_GPIO_Init_2 */
}

/* USER CODE BEGIN 4 */

/* USER CODE END 4 */

/**
  * @brief  This function is executed in case of error occurrence.
  * @retval None
  */
void Error_Handler(void)
{
  /* USER CODE BEGIN Error_Handler_Debug */
  /* User can add his own implementation to report the HAL error return state */

  /* USER CODE END Error_Handler_Debug */
}
#ifdef USE_FULL_ASSERT
/**
  * @brief  Reports the name of the source file and the source line number
  *         where the assert_param error has occurred.
  * @param  file: pointer to the source file name
  * @param  line: assert_param error line source number
  * @retval None
  */
void assert_failed(uint8_t *file, uint32_t line)
{
  /* USER CODE BEGIN 6 */
  /* User can add his own implementation to report the file name and line number,
     ex: printf("Wrong parameters value: file %s on line %d\r\n", file, line) */
  /* USER CODE END 6 */
}
#endif /* USE_FULL_ASSERT */

````
## 🖥 3. OLED Driver — OLED_SSD1306
**The graphics engine relies on the SSD1306 driver for:**

* I2C communication

* screen buffer management

* DMA‑based transfers

* display refresh (SSD1306_Display())

**The display is initialized in main.c:**
````
SSD1306_I2cInit(&hi2c1);
````

**OLED_SSD1306.c**:
````c
/* USER CODE BEGIN Header */
/**
  ******************************************************************************
 * @file    main.c
 * @author  MCD Application Team
 * @brief   BLE application with BLE core
 *
  ******************************************************************************
  * @attention
  *
  * Copyright (c) 2019-2021 STMicroelectronics.
  * All rights reserved.
  *
  * This software is licensed under terms that can be found in the LICENSE file
  * in the root directory of this software component.
  * If no LICENSE file comes with this software, it is provided AS-IS.
  *
  ******************************************************************************
  @verbatim
  ==============================================================================
                    ##### IMPORTANT NOTE #####
  ==============================================================================

  This application requests having the stm32wb5x_BLE_Stack_fw.bin binary
  flashed on the Wireless Coprocessor.
  If it is not the case, you need to use STM32CubeProgrammer to load the appropriate
  binary.

  All available binaries are located under following directory:
  /Projects/STM32_Copro_Wireless_Binaries

  Refer to UM2237 to learn how to use/install STM32CubeProgrammer.
  Refer to /Projects/STM32_Copro_Wireless_Binaries/ReleaseNote.html for the
  detailed procedure to change the Wireless Coprocessor binary.

  @endverbatim
  ******************************************************************************
  */
/* USER CODE END Header */
/* Includes ------------------------------------------------------------------*/
#include "main.h"

/* Private includes ----------------------------------------------------------*/
/* USER CODE BEGIN Includes */
#include "stdio.h"
#include "stdlib.h"
#include "string.h"
#include "OLED_SSD1306.h"
#include "GFX_BW.h"
#include "fonts.h"
#include "picture.h"
/* USER CODE END Includes */

/* Private typedef -----------------------------------------------------------*/
/* USER CODE BEGIN PTD */

/* USER CODE END PTD */

/* Private define ------------------------------------------------------------*/
/* USER CODE BEGIN PD */

/* USER CODE END PD */

/* Private macro -------------------------------------------------------------*/
/* USER CODE BEGIN PM */

/* USER CODE END PM */

/* Private variables ---------------------------------------------------------*/
I2C_HandleTypeDef hi2c1;
DMA_HandleTypeDef hdma_i2c1_tx;

IPCC_HandleTypeDef hipcc;

UART_HandleTypeDef hlpuart1;
UART_HandleTypeDef huart1;
DMA_HandleTypeDef hdma_lpuart1_tx;
DMA_HandleTypeDef hdma_usart1_tx;

RNG_HandleTypeDef hrng;

RTC_HandleTypeDef hrtc;

/* USER CODE BEGIN PV */
volatile uint16_t timer_1s;
extern uint8_t temp;
extern uint32_t pressure;
/* USER CODE END PV */

/* Private function prototypes -----------------------------------------------*/
void SystemClock_Config(void);
void PeriphCommonClock_Config(void);
static void MX_GPIO_Init(void);
static void MX_DMA_Init(void);
static void MX_RTC_Init(void);
static void MX_IPCC_Init(void);
static void MX_RNG_Init(void);
static void MX_I2C1_Init(void);
static void MX_RF_Init(void);
/* USER CODE BEGIN PFP */

/* USER CODE END PFP */

/* Private user code ---------------------------------------------------------*/
/* USER CODE BEGIN 0 */

/* USER CODE END 0 */

/**
  * @brief  The application entry point.
  * @retval int
  */
int main(void)
{

  /* USER CODE BEGIN 1 */

  /* USER CODE END 1 */

  /* MCU Configuration--------------------------------------------------------*/

  /* Reset of all peripherals, Initializes the Flash interface and the Systick. */
  HAL_Init();
  /* Config code for STM32_WPAN (HSE Tuning must be done before system clock configuration) */
  MX_APPE_Config();

  /* USER CODE BEGIN Init */

  /* USER CODE END Init */

  /* Configure the system clock */
  SystemClock_Config();

  /* Configure the peripherals common clocks */
  PeriphCommonClock_Config();

  /* IPCC initialisation */
  MX_IPCC_Init();

  /* USER CODE BEGIN SysInit */

  /* USER CODE END SysInit */

  /* Initialize all configured peripherals */
  MX_GPIO_Init();
  MX_DMA_Init();
  MX_RTC_Init();
  MX_RNG_Init();
  MX_I2C1_Init();
  MX_RF_Init();
  /* USER CODE BEGIN 2 */
  MX_DMA_Init();
  SSD1306_I2cInit(&hi2c1);





  SSD1306_Bitmap((uint8_t*)picture);
    HAL_Delay(5000);
    GFX_SetFont(font_8x5);
    GFX_SetFontSize(1);

    uint16_t frames = 0, fps = 0;
    uint32_t loops = 0, loops_overal = 0;
    char fps_c[20];





  /* USER CODE END 2 */

  /* Init code for STM32_WPAN */
  MX_APPE_Init();

  /* Infinite loop */
  /* USER CODE BEGIN WHILE */
	while(1)
	{
		 if(!timer_1s)
			  	{
			  		  timer_1s = 1000;
			  		  fps = frames;
			  		  frames = 0;
			  		  loops_overal = loops;
			  		  loops = 0;
			  	}

			  if(hi2c1.hdmatx->State == HAL_DMA_STATE_READY)
			  	{
				  SSD1306_Clear(BLACK);

				      // TEMPERATURA
				      char txt_temp[20];
				      sprintf(txt_temp, "TEMP: %d C", temp);
				      GFX_DrawString(10,10, txt_temp, WHITE, BLACK);

				      // CISNIENIE
				      char txt_press[20];
				      sprintf(txt_press, "PRES: %lu hPa", pressure / 100);
				      GFX_DrawString(10,20, txt_press, WHITE, BLACK);


				      SSD1306_Display();   // aktualizacja ekranu

				      loops++;
			  	}
			    HAL_GPIO_WritePin(TEST_GPIO_Port, TEST_Pin, 0);
			    loops++;











    /* USER CODE END WHILE */
    MX_APPE_Process();

    /* USER CODE BEGIN 3 */
  }
  /* USER CODE END 3 */
}

/**
  * @brief System Clock Configuration
  * @retval None
  */
void SystemClock_Config(void)
{
  RCC_OscInitTypeDef RCC_OscInitStruct = {0};
  RCC_ClkInitTypeDef RCC_ClkInitStruct = {0};

  /** Configure LSE Drive Capability
  */
  HAL_PWR_EnableBkUpAccess();
  __HAL_RCC_LSEDRIVE_CONFIG(RCC_LSEDRIVE_LOW);

  /** Configure the main internal regulator output voltage
  */
  __HAL_PWR_VOLTAGESCALING_CONFIG(PWR_REGULATOR_VOLTAGE_SCALE1);

  /** Initializes the RCC Oscillators according to the specified parameters
  * in the RCC_OscInitTypeDef structure.
  */
  RCC_OscInitStruct.OscillatorType = RCC_OSCILLATORTYPE_HSI48|RCC_OSCILLATORTYPE_HSI
                              |RCC_OSCILLATORTYPE_HSE|RCC_OSCILLATORTYPE_LSE;
  RCC_OscInitStruct.HSEState = RCC_HSE_ON;
  RCC_OscInitStruct.LSEState = RCC_LSE_ON;
  RCC_OscInitStruct.HSIState = RCC_HSI_ON;
  RCC_OscInitStruct.HSI48State = RCC_HSI48_ON;
  RCC_OscInitStruct.HSICalibrationValue = RCC_HSICALIBRATION_DEFAULT;
  RCC_OscInitStruct.PLL.PLLState = RCC_PLL_NONE;
  if (HAL_RCC_OscConfig(&RCC_OscInitStruct) != HAL_OK)
  {
    Error_Handler();
  }

  /** Configure the SYSCLKSource, HCLK, PCLK1 and PCLK2 clocks dividers
  */
  RCC_ClkInitStruct.ClockType = RCC_CLOCKTYPE_HCLK4|RCC_CLOCKTYPE_HCLK2
                              |RCC_CLOCKTYPE_HCLK|RCC_CLOCKTYPE_SYSCLK
                              |RCC_CLOCKTYPE_PCLK1|RCC_CLOCKTYPE_PCLK2;
  RCC_ClkInitStruct.SYSCLKSource = RCC_SYSCLKSOURCE_HSE;
  RCC_ClkInitStruct.AHBCLKDivider = RCC_SYSCLK_DIV1;
  RCC_ClkInitStruct.APB1CLKDivider = RCC_HCLK_DIV1;
  RCC_ClkInitStruct.APB2CLKDivider = RCC_HCLK_DIV1;
  RCC_ClkInitStruct.AHBCLK2Divider = RCC_SYSCLK_DIV1;
  RCC_ClkInitStruct.AHBCLK4Divider = RCC_SYSCLK_DIV1;

  if (HAL_RCC_ClockConfig(&RCC_ClkInitStruct, FLASH_LATENCY_1) != HAL_OK)
  {
    Error_Handler();
  }
}

/**
  * @brief Peripherals Common Clock Configuration
  * @retval None
  */
void PeriphCommonClock_Config(void)
{
  RCC_PeriphCLKInitTypeDef PeriphClkInitStruct = {0};

  /** Initializes the peripherals clock
  */
  PeriphClkInitStruct.PeriphClockSelection = RCC_PERIPHCLK_SMPS|RCC_PERIPHCLK_RFWAKEUP;
  PeriphClkInitStruct.RFWakeUpClockSelection = RCC_RFWKPCLKSOURCE_LSE;
  PeriphClkInitStruct.SmpsClockSelection = RCC_SMPSCLKSOURCE_HSE;
  PeriphClkInitStruct.SmpsDivSelection = RCC_SMPSCLKDIV_RANGE1;

  if (HAL_RCCEx_PeriphCLKConfig(&PeriphClkInitStruct) != HAL_OK)
  {
    Error_Handler();
  }
  /* USER CODE BEGIN Smps */

  /* USER CODE END Smps */
}

/**
  * @brief I2C1 Initialization Function
  * @param None
  * @retval None
  */
static void MX_I2C1_Init(void)
{

  /* USER CODE BEGIN I2C1_Init 0 */

  /* USER CODE END I2C1_Init 0 */

  /* USER CODE BEGIN I2C1_Init 1 */

  /* USER CODE END I2C1_Init 1 */
  hi2c1.Instance = I2C1;
  hi2c1.Init.Timing = 0x00B07CB4;
  hi2c1.Init.OwnAddress1 = 0;
  hi2c1.Init.AddressingMode = I2C_ADDRESSINGMODE_7BIT;
  hi2c1.Init.DualAddressMode = I2C_DUALADDRESS_DISABLE;
  hi2c1.Init.OwnAddress2 = 0;
  hi2c1.Init.OwnAddress2Masks = I2C_OA2_NOMASK;
  hi2c1.Init.GeneralCallMode = I2C_GENERALCALL_DISABLE;
  hi2c1.Init.NoStretchMode = I2C_NOSTRETCH_DISABLE;
  if (HAL_I2C_Init(&hi2c1) != HAL_OK)
  {
    Error_Handler();
  }

  /** Configure Analogue filter
  */
  if (HAL_I2CEx_ConfigAnalogFilter(&hi2c1, I2C_ANALOGFILTER_ENABLE) != HAL_OK)
  {
    Error_Handler();
  }

  /** Configure Digital filter
  */
  if (HAL_I2CEx_ConfigDigitalFilter(&hi2c1, 0) != HAL_OK)
  {
    Error_Handler();
  }
  /* USER CODE BEGIN I2C1_Init 2 */

  /* USER CODE END I2C1_Init 2 */

}

/**
  * @brief IPCC Initialization Function
  * @param None
  * @retval None
  */
static void MX_IPCC_Init(void)
{

  /* USER CODE BEGIN IPCC_Init 0 */

  /* USER CODE END IPCC_Init 0 */

  /* USER CODE BEGIN IPCC_Init 1 */

  /* USER CODE END IPCC_Init 1 */
  hipcc.Instance = IPCC;
  if (HAL_IPCC_Init(&hipcc) != HAL_OK)
  {
    Error_Handler();
  }
  /* USER CODE BEGIN IPCC_Init 2 */

  /* USER CODE END IPCC_Init 2 */

}

/**
  * @brief LPUART1 Initialization Function
  * @param None
  * @retval None
  */
void MX_LPUART1_UART_Init(void)
{

  /* USER CODE BEGIN LPUART1_Init 0 */

  /* USER CODE END LPUART1_Init 0 */

  /* USER CODE BEGIN LPUART1_Init 1 */

  /* USER CODE END LPUART1_Init 1 */
  hlpuart1.Instance = LPUART1;
  hlpuart1.Init.BaudRate = 115200;
  hlpuart1.Init.WordLength = UART_WORDLENGTH_8B;
  hlpuart1.Init.StopBits = UART_STOPBITS_1;
  hlpuart1.Init.Parity = UART_PARITY_NONE;
  hlpuart1.Init.Mode = UART_MODE_TX_RX;
  hlpuart1.Init.HwFlowCtl = UART_HWCONTROL_NONE;
  hlpuart1.Init.OneBitSampling = UART_ONE_BIT_SAMPLE_DISABLE;
  hlpuart1.Init.ClockPrescaler = UART_PRESCALER_DIV1;
  hlpuart1.AdvancedInit.AdvFeatureInit = UART_ADVFEATURE_NO_INIT;
  hlpuart1.FifoMode = UART_FIFOMODE_DISABLE;
  if (HAL_UART_Init(&hlpuart1) != HAL_OK)
  {
    Error_Handler();
  }
  if (HAL_UARTEx_SetTxFifoThreshold(&hlpuart1, UART_TXFIFO_THRESHOLD_1_8) != HAL_OK)
  {
    Error_Handler();
  }
  if (HAL_UARTEx_SetRxFifoThreshold(&hlpuart1, UART_RXFIFO_THRESHOLD_1_8) != HAL_OK)
  {
    Error_Handler();
  }
  if (HAL_UARTEx_DisableFifoMode(&hlpuart1) != HAL_OK)
  {
    Error_Handler();
  }
  /* USER CODE BEGIN LPUART1_Init 2 */

  /* USER CODE END LPUART1_Init 2 */

}

/**
  * @brief USART1 Initialization Function
  * @param None
  * @retval None
  */
void MX_USART1_UART_Init(void)
{

  /* USER CODE BEGIN USART1_Init 0 */

  /* USER CODE END USART1_Init 0 */

  /* USER CODE BEGIN USART1_Init 1 */

  /* USER CODE END USART1_Init 1 */
  huart1.Instance = USART1;
  huart1.Init.BaudRate = 115200;
  huart1.Init.WordLength = UART_WORDLENGTH_8B;
  huart1.Init.StopBits = UART_STOPBITS_1;
  huart1.Init.Parity = UART_PARITY_NONE;
  huart1.Init.Mode = UART_MODE_TX_RX;
  huart1.Init.HwFlowCtl = UART_HWCONTROL_NONE;
  huart1.Init.OverSampling = UART_OVERSAMPLING_8;
  huart1.Init.OneBitSampling = UART_ONE_BIT_SAMPLE_DISABLE;
  huart1.Init.ClockPrescaler = UART_PRESCALER_DIV1;
  huart1.AdvancedInit.AdvFeatureInit = UART_ADVFEATURE_NO_INIT;
  if (HAL_UART_Init(&huart1) != HAL_OK)
  {
    Error_Handler();
  }
  if (HAL_UARTEx_SetTxFifoThreshold(&huart1, UART_TXFIFO_THRESHOLD_1_8) != HAL_OK)
  {
    Error_Handler();
  }
  if (HAL_UARTEx_SetRxFifoThreshold(&huart1, UART_RXFIFO_THRESHOLD_1_8) != HAL_OK)
  {
    Error_Handler();
  }
  if (HAL_UARTEx_DisableFifoMode(&huart1) != HAL_OK)
  {
    Error_Handler();
  }
  /* USER CODE BEGIN USART1_Init 2 */

  /* USER CODE END USART1_Init 2 */

}

/**
  * @brief RF Initialization Function
  * @param None
  * @retval None
  */
static void MX_RF_Init(void)
{

  /* USER CODE BEGIN RF_Init 0 */

  /* USER CODE END RF_Init 0 */

  /* USER CODE BEGIN RF_Init 1 */

  /* USER CODE END RF_Init 1 */
  /* USER CODE BEGIN RF_Init 2 */

  /* USER CODE END RF_Init 2 */

}

/**
  * @brief RNG Initialization Function
  * @param None
  * @retval None
  */
static void MX_RNG_Init(void)
{

  /* USER CODE BEGIN RNG_Init 0 */

  /* USER CODE END RNG_Init 0 */

  /* USER CODE BEGIN RNG_Init 1 */

  /* USER CODE END RNG_Init 1 */
  hrng.Instance = RNG;
  hrng.Init.ClockErrorDetection = RNG_CED_ENABLE;
  if (HAL_RNG_Init(&hrng) != HAL_OK)
  {
    Error_Handler();
  }
  /* USER CODE BEGIN RNG_Init 2 */

  /* USER CODE END RNG_Init 2 */

}

/**
  * @brief RTC Initialization Function
  * @param None
  * @retval None
  */
static void MX_RTC_Init(void)
{

  /* USER CODE BEGIN RTC_Init 0 */

  /* USER CODE END RTC_Init 0 */

  /* USER CODE BEGIN RTC_Init 1 */

  /* USER CODE END RTC_Init 1 */

  /** Initialize RTC Only
  */
  hrtc.Instance = RTC;
  hrtc.Init.HourFormat = RTC_HOURFORMAT_24;
  hrtc.Init.AsynchPrediv = CFG_RTC_ASYNCH_PRESCALER;
  hrtc.Init.SynchPrediv = CFG_RTC_SYNCH_PRESCALER;
  hrtc.Init.OutPut = RTC_OUTPUT_DISABLE;
  hrtc.Init.OutPutPolarity = RTC_OUTPUT_POLARITY_HIGH;
  hrtc.Init.OutPutType = RTC_OUTPUT_TYPE_OPENDRAIN;
  hrtc.Init.OutPutRemap = RTC_OUTPUT_REMAP_NONE;
  if (HAL_RTC_Init(&hrtc) != HAL_OK)
  {
    Error_Handler();
  }

  /** Enable the WakeUp
  */
  if (HAL_RTCEx_SetWakeUpTimer_IT(&hrtc, 0, RTC_WAKEUPCLOCK_RTCCLK_DIV16) != HAL_OK)
  {
    Error_Handler();
  }
  /* USER CODE BEGIN RTC_Init 2 */

  /* USER CODE END RTC_Init 2 */

}

/**
  * Enable DMA controller clock
  */
static void MX_DMA_Init(void)
{

  /* DMA controller clock enable */
  __HAL_RCC_DMAMUX1_CLK_ENABLE();
  __HAL_RCC_DMA1_CLK_ENABLE();
  __HAL_RCC_DMA2_CLK_ENABLE();

  /* DMA interrupt init */
  /* DMA1_Channel1_IRQn interrupt configuration */
  HAL_NVIC_SetPriority(DMA1_Channel1_IRQn, 0, 0);
  HAL_NVIC_EnableIRQ(DMA1_Channel1_IRQn);
  /* DMA1_Channel4_IRQn interrupt configuration */
  HAL_NVIC_SetPriority(DMA1_Channel4_IRQn, 15, 0);
  HAL_NVIC_EnableIRQ(DMA1_Channel4_IRQn);
  /* DMA2_Channel4_IRQn interrupt configuration */
  HAL_NVIC_SetPriority(DMA2_Channel4_IRQn, 15, 0);
  HAL_NVIC_EnableIRQ(DMA2_Channel4_IRQn);

}

/**
  * @brief GPIO Initialization Function
  * @param None
  * @retval None
  */
static void MX_GPIO_Init(void)
{
  GPIO_InitTypeDef GPIO_InitStruct = {0};
  /* USER CODE BEGIN MX_GPIO_Init_1 */
  /* USER CODE END MX_GPIO_Init_1 */

  /* GPIO Ports Clock Enable */
  __HAL_RCC_GPIOC_CLK_ENABLE();
  __HAL_RCC_GPIOB_CLK_ENABLE();
  __HAL_RCC_GPIOA_CLK_ENABLE();

  /*Configure GPIO pin Output Level */
  HAL_GPIO_WritePin(TEST_GPIO_Port, TEST_Pin, GPIO_PIN_RESET);

  /*Configure GPIO pin : TEST_Pin */
  GPIO_InitStruct.Pin = TEST_Pin;
  GPIO_InitStruct.Mode = GPIO_MODE_OUTPUT_PP;
  GPIO_InitStruct.Pull = GPIO_NOPULL;
  GPIO_InitStruct.Speed = GPIO_SPEED_FREQ_LOW;
  HAL_GPIO_Init(TEST_GPIO_Port, &GPIO_InitStruct);

  /* USER CODE BEGIN MX_GPIO_Init_2 */
  /* USER CODE END MX_GPIO_Init_2 */
}

/* USER CODE BEGIN 4 */

/* USER CODE END 4 */

/**
  * @brief  This function is executed in case of error occurrence.
  * @retval None
  */
void Error_Handler(void)
{
  /* USER CODE BEGIN Error_Handler_Debug */
  /* User can add his own implementation to report the HAL error return state */

  /* USER CODE END Error_Handler_Debug */
}
#ifdef USE_FULL_ASSERT
/**
  * @brief  Reports the name of the source file and the source line number
  *         where the assert_param error has occurred.
  * @param  file: pointer to the source file name
  * @param  line: assert_param error line source number
  * @retval None
  */
void assert_failed(uint8_t *file, uint32_t line)
{
  /* USER CODE BEGIN 6 */
  /* User can add his own implementation to report the file name and line number,
     ex: printf("Wrong parameters value: file %s on line %d\r\n", file, line) */
  /* USER CODE END 6 */
}
#endif /* USE_FULL_ASSERT */
````

**OLED_SSD1306.h**:
````c

#ifndef OLED_SSD1306_H_
#define OLED_SSD1306_H_

/*
 *
 *    SETTINGS
 *
 *    Please set only one interface. It won't work with both one time.
 *
 */
//#define SSD1306_SPI_CONTROL
#define SSD1306_I2C_CONTROL

#ifdef SSD1306_I2C_CONTROL
//#define SSD1306_I2C_DMA_ENABLE
#define SSD1306_I2C_ADDRESS (0x3C << 1)
#endif
#ifdef SSD1306_SPI_CONTROL
#define SSD1306_RESET_USE
#define SSD1306_SPI_DMA_ENABLE
#define SPI_CS_HARDWARE_CONTROL
#endif

//
// Resolution
//
#define SSD1306_LCDWIDTH                  128
#define SSD1306_LCDHEIGHT                 64

/*
 * 		Please set what functionality you want to use.
 * 		Some functions need other functionalities. It should works automatically.
 *
 * 		1 - will be compiled
 * 		0 - won't be compiled
 */
#define GRAPHIC_ACCELERATION_COMMANDS 1
#define ADVANCED_GRAPHIC_COMMANDS 1

/****************************************************************/

//
// Commands
//
#define SSD1306_SETCONTRAST 0x81
#define SSD1306_DISPLAYALLON_RESUME 0xA4
#define SSD1306_DISPLAYALLON 0xA5
#define SSD1306_NORMALDISPLAY 0xA6
#define SSD1306_INVERTDISPLAY 0xA7
#define SSD1306_DISPLAYOFF 0xAE
#define SSD1306_DISPLAYON 0xAF
#define SSD1306_SETDISPLAYOFFSET 0xD3
#define SSD1306_SETCOMPINS 0xDA
#define SSD1306_SETVCOMDETECT 0xDB
#define SSD1306_SETDISPLAYCLOCKDIV 0xD5
#define SSD1306_SETPRECHARGE 0xD9
#define SSD1306_SETMULTIPLEX 0xA8
#define SSD1306_SETLOWCOLUMN 0x00
#define SSD1306_SETHIGHCOLUMN 0x10
#define SSD1306_SETSTARTLINE 0x40
#define SSD1306_MEMORYMODE 0x20
#define SSD1306_COLUMNADDR 0x21
#define SSD1306_PAGEADDR   0x22
#define SSD1306_COMSCANINC 0xC0
#define SSD1306_COMSCANDEC 0xC8
#define SSD1306_SEGREMAP 0xA0
#define SSD1306_CHARGEPUMP 0x8D
#define SSD1306_EXTERNALVCC 0x1
#define SSD1306_SWITCHCAPVCC 0x2

//
// Scrolling #defines
//
#define SSD1306_ACTIVATE_SCROLL 0x2F
#define SSD1306_DEACTIVATE_SCROLL 0x2E
#define SSD1306_SET_VERTICAL_SCROLL_AREA 0xA3
#define SSD1306_RIGHT_HORIZONTAL_SCROLL 0x26
#define SSD1306_LEFT_HORIZONTAL_SCROLL 0x27
#define SSD1306_VERTICAL_AND_RIGHT_HORIZONTAL_SCROLL 0x29
#define SSD1306_VERTICAL_AND_LEFT_HORIZONTAL_SCROLL 0x2A

//
// Advanced Graphic defines
//
#define SSD1306_FADE_OUT 0x23
#define SSD1306_ZOOM_IN 0xD6

//
// Colors
//
#define BLACK 0
#define WHITE 1
#define INVERSE 2

//
// Scrolling enums
//
typedef enum
{
	SCROLL_EVERY_5_FRAMES,
	SCROLL_EVERY_64_FRAMES,
	SCROLL_EVERY_128_FRAMES,
	SCROLL_EVERY_256_FRAMES,
	SCROLL_EVERY_3_FRAMES,
	SCROLL_EVERY_4_FRAMES,
	SCROLL_EVERY_25_FRAMES,
	SCROLL_EVERY_2_FRAMES,
} scroll_horizontal_speed;
//
// Functions
//
#ifdef SSD1306_I2C_CONTROL
void SSD1306_I2cInit(I2C_HandleTypeDef *i2c);
#endif
#ifdef SSD1306_SPI_CONTROL
void SSD1306_SpiInit(SPI_HandleTypeDef *spi);
#endif
#if defined(SSD1306_SPI_CONTROL) && !defined(SSD1306_SPI_DMA_ENABLE)
void SSD1306_DmaEndCallback(SPI_HandleTypeDef *hspi);
#endif

//
// Configuration
//
void SSD1306_DisplayON(uint8_t On);
void SSD1306_InvertColors(uint8_t Invert);
void SSD1306_RotateDisplay(uint8_t Rotate);
void SSD1306_SetContrast(uint8_t Contrast);

//
// Drawing
//
void SSD1306_DrawPixel(int16_t x, int16_t y, uint8_t Color);
void SSD1306_Clear(uint8_t Color);
void SSD1306_Display(void);
void SSD1306_Bitmap(uint8_t *bitmap);

#if GRAPHIC_ACCELERATION_COMMANDS == 1
//
// Graphic Acceleration Commands
//
void SSD1306_StartScrollRight(uint8_t StartPage, uint8_t EndPage, scroll_horizontal_speed Speed);
void SSD1306_StartScrollLeft(uint8_t StartPage, uint8_t EndPage, scroll_horizontal_speed Speed);
void SSD1306_StartScrollRightUp(uint8_t StartPage, uint8_t EndPage, scroll_horizontal_speed HorizontalSpeed, uint8_t VerticalOffset);
void SSD1306_StartScrollLeftUp(uint8_t StartPage, uint8_t EndPage, scroll_horizontal_speed HorizontalSpeed, uint8_t VerticalOffset);
void SSD1306_StopScroll(void);
#endif

#if ADVANCED_GRAPHIC_COMMANDS == 1
//
// Advanced Graphic Commands
//
void SSD1306_StartFadeOut(uint8_t Interval);
void SSD1306_StartBlinking(uint8_t Interval);
void SSD1306_StopFadeOutOrBlinking(void);
void SSD1306_ZoomIn(uint8_t Zoom);
#endif

#endif /* OLED_SSD1306_H_ */
````


**picture.h**:
````c

const uint8_t picture[]  = {
		// 'experience', 128x64px
		0xff, 0xff, 0xff, 0xff, 0xff, 0xff, 0xff, 0xff, 0xff, 0xff, 0xff, 0xff, 0xff, 0xff, 0xff, 0xff,
		0xff, 0xff, 0xff, 0x7f, 0xbf, 0xbf, 0x5f, 0x5f, 0x2f, 0x2f, 0x2f, 0x2f, 0x17, 0x17, 0x0b, 0x0b,
		0x0b, 0x0b, 0x0b, 0x0b, 0x0b, 0x17, 0x17, 0x27, 0x2f, 0x2f, 0x4f, 0x5f, 0x5f, 0x9f, 0x3f, 0x7f,
		0x7f, 0xff, 0xff, 0xff, 0xff, 0xff, 0xff, 0xff, 0xff, 0xff, 0xff, 0xff, 0xff, 0xff, 0xff, 0xff,
		0xff, 0xff, 0xff, 0xff, 0xff, 0xff, 0xff, 0xbf, 0x7f, 0xbf, 0xff, 0xff, 0xff, 0xff, 0xff, 0xff,
		0xff, 0xff, 0xff, 0xff, 0xff, 0xff, 0xff, 0xff, 0xff, 0xbf, 0x7f, 0x9f, 0x5f, 0xaf, 0x0f, 0x87,
		0x47, 0x87, 0x47, 0x8f, 0x5f, 0x9f, 0xdf, 0xdf, 0xff, 0xff, 0xff, 0xff, 0xff, 0xff, 0xff, 0xff,
		0xff, 0xff, 0x7f, 0x7f, 0x7f, 0xff, 0xff, 0xff, 0x7f, 0x3f, 0x1f, 0x0f, 0x0f, 0x0f, 0x0f, 0x1f,
		0xff, 0xff, 0xff, 0xff, 0xff, 0xff, 0xff, 0xff, 0xff, 0xff, 0x7f, 0xbf, 0x5f, 0x2f, 0x17, 0x0b,
		0x05, 0x05, 0x02, 0x49, 0x24, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00,
		0x00, 0x80, 0xc0, 0xc0, 0x80, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x01, 0x01,
		0x02, 0x04, 0x09, 0x13, 0x67, 0x8f, 0x3f, 0x7f, 0xff, 0xff, 0xff, 0xff, 0xff, 0xff, 0xff, 0xff,
		0xff, 0xff, 0xff, 0xfe, 0xfd, 0xfe, 0xf7, 0xfb, 0xf7, 0xff, 0xff, 0xff, 0xff, 0xff, 0xff, 0xff,
		0xff, 0xff, 0xff, 0xff, 0x7f, 0x7f, 0x3f, 0x3f, 0x3f, 0x3f, 0x3f, 0x3e, 0x3f, 0x3e, 0x3f, 0x3e,
		0x3f, 0x7f, 0x7f, 0x7f, 0xff, 0xff, 0xfd, 0xfb, 0xf5, 0xfb, 0xf5, 0xeb, 0xf7, 0xeb, 0xf7, 0xeb,
		0xf5, 0xea, 0xf5, 0xea, 0xf4, 0xe8, 0xd4, 0xe8, 0xd0, 0xe8, 0xc0, 0xc0, 0xe0, 0xd0, 0xe8, 0xd0,
		0xdf, 0xaf, 0x57, 0x57, 0x4b, 0x4b, 0x45, 0x45, 0x42, 0xe2, 0x61, 0x30, 0x10, 0x08, 0x00, 0x00,
		0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0xa0, 0x30, 0x30, 0x38, 0x78, 0x7c, 0xfc, 0xfe, 0xfe,
		0xff, 0xff, 0xff, 0x7f, 0x3f, 0xbf, 0xbf, 0x3e, 0x7c, 0x78, 0x70, 0xe0, 0xc0, 0x00, 0x02, 0x08,
		0x22, 0x08, 0x22, 0x88, 0x20, 0x80, 0x21, 0x86, 0x18, 0x62, 0x8f, 0x3b, 0xff, 0xaa, 0xdf, 0xeb,
		0x7f, 0xaa, 0xdf, 0xab, 0x5f, 0xaa, 0xdf, 0xab, 0x7f, 0xaa, 0xdf, 0x2b, 0x1f, 0x0a, 0xc7, 0xe3,
		0xf1, 0xf8, 0xfc, 0xfc, 0xfe, 0xfe, 0xfe, 0xff, 0xff, 0xff, 0xff, 0xff, 0xff, 0xff, 0xff, 0xff,
		0xfe, 0xfe, 0xfe, 0xfc, 0xfc, 0xf8, 0xf1, 0xe3, 0xc7, 0x0b, 0x1f, 0x2b, 0x7f, 0xab, 0xdf, 0xab,
		0x7f, 0xab, 0xdf, 0xab, 0x5f, 0xab, 0xdf, 0xab, 0x7f, 0xab, 0xdf, 0xab, 0x7f, 0xab, 0xdf, 0xab,
		0x95, 0xa3, 0x15, 0x0b, 0xc5, 0x23, 0x19, 0x04, 0x02, 0x01, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00,
		0x80, 0x00, 0x00, 0xfa, 0xf3, 0xc3, 0xb3, 0xfb, 0xc3, 0x83, 0x03, 0x83, 0xf6, 0xef, 0xff, 0xff,
		0xff, 0xff, 0xff, 0xde, 0xef, 0xe7, 0xf7, 0x07, 0x07, 0x07, 0x06, 0xf6, 0x25, 0x87, 0xc0, 0x00,
		0x00, 0x00, 0xc0, 0xe0, 0x61, 0xc0, 0x01, 0x02, 0x05, 0x02, 0x01, 0x06, 0x08, 0x71, 0x85, 0x3a,
		0xd5, 0xa2, 0x95, 0xaa, 0x95, 0xa2, 0x95, 0xaa, 0x95, 0x82, 0xc1, 0xe0, 0xfc, 0xff, 0xff, 0xff,
		0xff, 0xff, 0xff, 0xff, 0xff, 0xff, 0xff, 0xff, 0xff, 0xff, 0xff, 0xff, 0xff, 0xff, 0xff, 0xff,
		0xff, 0xff, 0xff, 0xff, 0xff, 0xff, 0xff, 0xff, 0xff, 0xff, 0xfc, 0xe0, 0xc1, 0x82, 0x95, 0xaa,
		0x95, 0xa2, 0x95, 0xaa, 0x95, 0xa2, 0x95, 0xaa, 0x95, 0xa2, 0x95, 0xaa, 0x95, 0xa2, 0x95, 0xaa,
		0x00, 0x00, 0x3c, 0x13, 0x08, 0x04, 0xf2, 0x0e, 0x01, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x03,
		0x0f, 0x10, 0x00, 0x07, 0x3f, 0xff, 0xfb, 0xfd, 0xff, 0xfd, 0xff, 0xff, 0xff, 0xe7, 0x8b, 0xf5,
		0xfb, 0xff, 0xff, 0xff, 0xff, 0xff, 0xef, 0xf7, 0xfe, 0xee, 0xf7, 0xff, 0xff, 0xff, 0x1f, 0x00,
		0x80, 0x08, 0x0d, 0x06, 0x03, 0x01, 0x00, 0x00, 0x00, 0x00, 0x00, 0xc0, 0x38, 0x40, 0x81, 0x0e,
		0x30, 0xc1, 0x01, 0x01, 0x11, 0x45, 0x51, 0x15, 0x41, 0x55, 0x15, 0x41, 0x15, 0x41, 0x55, 0x17,
		0x5f, 0x5f, 0x57, 0xd7, 0xd7, 0x57, 0x47, 0x5f, 0x5f, 0x77, 0x77, 0x57, 0x57, 0x5f, 0x5f, 0xc7,
		0xdf, 0x17, 0xf7, 0x77, 0x57, 0x7f, 0x5f, 0x57, 0x15, 0x51, 0x55, 0x05, 0x55, 0x55, 0x51, 0x05,
		0x51, 0x15, 0x45, 0x55, 0x01, 0x01, 0x01, 0x21, 0x01, 0x01, 0x01, 0x01, 0x21, 0x01, 0x21, 0x01,
		0x00, 0x00, 0x02, 0x00, 0x00, 0x00, 0x01, 0x1e, 0xe0, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00,
		0x80, 0x00, 0x00, 0x00, 0x00, 0x00, 0x01, 0x07, 0x0f, 0x9f, 0x3f, 0x7f, 0xfb, 0xff, 0xf7, 0xf7,
		0xff, 0xff, 0xf7, 0xf7, 0xfb, 0xff, 0x7f, 0x3f, 0x9f, 0x0f, 0x07, 0x03, 0x01, 0x00, 0x00, 0x02,
		0x00, 0x00, 0x80, 0x40, 0x00, 0x00, 0x80, 0x40, 0x20, 0x10, 0x8e, 0x03, 0x80, 0x00, 0x01, 0x02,
		0x3c, 0x03, 0xc0, 0x00, 0x00, 0x40, 0x42, 0x00, 0x80, 0x45, 0x41, 0x84, 0x11, 0x10, 0x84, 0x95,
		0x10, 0x05, 0x15, 0x05, 0x07, 0x55, 0x45, 0x1e, 0x1f, 0x96, 0x97, 0x15, 0x05, 0x17, 0x1f, 0x1d,
		0x15, 0x55, 0x87, 0x17, 0x01, 0x95, 0x90, 0x85, 0x11, 0x14, 0x15, 0x01, 0x15, 0x05, 0x00, 0x09,
		0x00, 0x04, 0x00, 0x04, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x08, 0x00,
		0x00, 0x01, 0x00, 0x02, 0x00, 0x00, 0x80, 0x40, 0x00, 0x41, 0x02, 0x04, 0x08, 0x08, 0x10, 0x10,
		0x20, 0x22, 0x2c, 0x10, 0x20, 0xc0, 0x80, 0x40, 0x30, 0x1f, 0x80, 0x54, 0xaa, 0x54, 0xa9, 0x55,
		0x29, 0x55, 0x29, 0x15, 0x08, 0x04, 0x0a, 0x00, 0x3f, 0x40, 0xa0, 0xa0, 0x10, 0x08, 0x08, 0x04,
		0x06, 0x05, 0x04, 0x02, 0x02, 0x01, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00,
		0x04, 0x07, 0x04, 0x80, 0xc6, 0xa1, 0x60, 0x46, 0x01, 0xa1, 0x40, 0xc0, 0x22, 0x87, 0xe6, 0xa5,
		0xa1, 0x40, 0x80, 0xc0, 0xa6, 0x61, 0x40, 0x04, 0x85, 0x62, 0x20, 0x40, 0x00, 0x80, 0x52, 0x15,
		0x85, 0xc7, 0xa1, 0x60, 0x46, 0x01, 0x80, 0x66, 0x21, 0xa0, 0x40, 0x00, 0x80, 0x40, 0x20, 0x20,
		0x00, 0x80, 0xc0, 0xa0, 0x60, 0x40, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00,
		0x08, 0x00, 0x00, 0xc0, 0xf0, 0xf8, 0xfc, 0xfc, 0xfe, 0xfe, 0xff, 0xff, 0xff, 0xff, 0xff, 0x3f,
		0x0f, 0x03, 0x01, 0x01, 0x11, 0x10, 0x14, 0x04, 0x04, 0x01, 0x00, 0x01, 0x00, 0x00, 0x00, 0x00,
		0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x01, 0x01, 0x01, 0x1e,
		0x7e, 0xfe, 0xfe, 0xfe, 0xfe, 0xfe, 0xfc, 0xfc, 0xf8, 0xf8, 0xf0, 0xe0, 0x80, 0x00, 0x00, 0x00,
		0x00, 0x00, 0x00, 0x00, 0x01, 0x01, 0x01, 0x68, 0x19, 0x0c, 0x08, 0x61, 0x5a, 0x49, 0x38, 0x00,
		0x00, 0x00, 0x60, 0x59, 0x55, 0x31, 0x00, 0x20, 0x71, 0x68, 0x58, 0x10, 0x00, 0x01, 0x00, 0x60,
		0x18, 0x55, 0x31, 0x01, 0x20, 0x50, 0x51, 0x74, 0x18, 0x01, 0x20, 0x50, 0x50, 0x71, 0x19, 0x05,
		0x40, 0x18, 0x0d, 0x01, 0x01, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00
};
````

**stm32wbxx_it.c**:
````c
/* USER CODE BEGIN Header */
/**
  ******************************************************************************
  * @file    stm32wbxx_it.c
  * @author  MCD Application Team
  * @brief   Main Interrupt Service Routines.
  *          This file provides template for all exceptions handler and
  *          peripherals interrupt service routine.
  ******************************************************************************
  * @attention
  *
  * Copyright (c) 2019-2021 STMicroelectronics.
  * All rights reserved.
  *
  * This software is licensed under terms that can be found in the LICENSE file
  * in the root directory of this software component.
  * If no LICENSE file comes with this software, it is provided AS-IS.
  *
  ******************************************************************************
  */
/* USER CODE END Header */

/* Includes ------------------------------------------------------------------*/
#include "main.h"
#include "stm32wbxx_it.h"
/* Private includes ----------------------------------------------------------*/
/* USER CODE BEGIN Includes */

/* USER CODE END Includes */

/* Private typedef -----------------------------------------------------------*/
/* USER CODE BEGIN TD */

/* USER CODE END TD */

/* Private define ------------------------------------------------------------*/
/* USER CODE BEGIN PD */
 
/* USER CODE END PD */

/* Private macro -------------------------------------------------------------*/
/* USER CODE BEGIN PM */

/* USER CODE END PM */

/* Private variables ---------------------------------------------------------*/
/* USER CODE BEGIN PV */

/* USER CODE END PV */

/* Private function prototypes -----------------------------------------------*/
/* USER CODE BEGIN PFP */

/* USER CODE END PFP */

/* Private user code ---------------------------------------------------------*/
/* USER CODE BEGIN 0 */

/* USER CODE END 0 */

/* External variables --------------------------------------------------------*/
extern DMA_HandleTypeDef hdma_i2c1_tx;
extern IPCC_HandleTypeDef hipcc;
extern DMA_HandleTypeDef hdma_lpuart1_tx;
extern DMA_HandleTypeDef hdma_usart1_tx;
extern UART_HandleTypeDef hlpuart1;
extern UART_HandleTypeDef huart1;
extern RTC_HandleTypeDef hrtc;
/* USER CODE BEGIN EV */

/* USER CODE END EV */

/******************************************************************************/
/*           Cortex Processor Interruption and Exception Handlers          */
/******************************************************************************/
/**
  * @brief This function handles Non maskable interrupt.
  */
void NMI_Handler(void)
{
  /* USER CODE BEGIN NonMaskableInt_IRQn 0 */

  /* USER CODE END NonMaskableInt_IRQn 0 */
  /* USER CODE BEGIN NonMaskableInt_IRQn 1 */

  /* USER CODE END NonMaskableInt_IRQn 1 */
}

/**
  * @brief This function handles Hard fault interrupt.
  */
void HardFault_Handler(void)
{
  /* USER CODE BEGIN HardFault_IRQn 0 */

  /* USER CODE END HardFault_IRQn 0 */
  while (1)
  {
    /* USER CODE BEGIN W1_HardFault_IRQn 0 */
    /* USER CODE END W1_HardFault_IRQn 0 */
  }
}

/**
  * @brief This function handles Memory management fault.
  */
void MemManage_Handler(void)
{
  /* USER CODE BEGIN MemoryManagement_IRQn 0 */

  /* USER CODE END MemoryManagement_IRQn 0 */
  while (1)
  {
    /* USER CODE BEGIN W1_MemoryManagement_IRQn 0 */
    /* USER CODE END W1_MemoryManagement_IRQn 0 */
  }
}

/**
  * @brief This function handles Prefetch fault, memory access fault.
  */
void BusFault_Handler(void)
{
  /* USER CODE BEGIN BusFault_IRQn 0 */

  /* USER CODE END BusFault_IRQn 0 */
  while (1)
  {
    /* USER CODE BEGIN W1_BusFault_IRQn 0 */
    /* USER CODE END W1_BusFault_IRQn 0 */
  }
}

/**
  * @brief This function handles Undefined instruction or illegal state.
  */
void UsageFault_Handler(void)
{
  /* USER CODE BEGIN UsageFault_IRQn 0 */

  /* USER CODE END UsageFault_IRQn 0 */
  while (1)
  {
    /* USER CODE BEGIN W1_UsageFault_IRQn 0 */
    /* USER CODE END W1_UsageFault_IRQn 0 */
  }
}

/**
  * @brief This function handles System service call via SWI instruction.
  */
void SVC_Handler(void)
{
  /* USER CODE BEGIN SVCall_IRQn 0 */

  /* USER CODE END SVCall_IRQn 0 */
  /* USER CODE BEGIN SVCall_IRQn 1 */

  /* USER CODE END SVCall_IRQn 1 */
}

/**
  * @brief This function handles Debug monitor.
  */
void DebugMon_Handler(void)
{
  /* USER CODE BEGIN DebugMonitor_IRQn 0 */

  /* USER CODE END DebugMonitor_IRQn 0 */
  /* USER CODE BEGIN DebugMonitor_IRQn 1 */

  /* USER CODE END DebugMonitor_IRQn 1 */
}

/**
  * @brief This function handles Pendable request for system service.
  */
void PendSV_Handler(void)
{
  /* USER CODE BEGIN PendSV_IRQn 0 */

  /* USER CODE END PendSV_IRQn 0 */
  /* USER CODE BEGIN PendSV_IRQn 1 */

  /* USER CODE END PendSV_IRQn 1 */
}

/**
  * @brief This function handles System tick timer.
  */
void SysTick_Handler(void)
{
  /* USER CODE BEGIN SysTick_IRQn 0 */

  /* USER CODE END SysTick_IRQn 0 */
  HAL_IncTick();
  /* USER CODE BEGIN SysTick_IRQn 1 */

  /* USER CODE END SysTick_IRQn 1 */
}

/******************************************************************************/
/* STM32WBxx Peripheral Interrupt Handlers                                    */
/* Add here the Interrupt Handlers for the used peripherals.                  */
/* For the available peripheral interrupt handler names,                      */
/* please refer to the startup file (startup_stm32wbxx.s).                    */
/******************************************************************************/

/**
  * @brief This function handles RTC wake-up interrupt through EXTI line 19.
  */
void RTC_WKUP_IRQHandler(void)
{
  /* USER CODE BEGIN RTC_WKUP_IRQn 0 */

  /* USER CODE END RTC_WKUP_IRQn 0 */
  HAL_RTCEx_WakeUpTimerIRQHandler(&hrtc);
  /* USER CODE BEGIN RTC_WKUP_IRQn 1 */

  /* USER CODE END RTC_WKUP_IRQn 1 */
}

/**
  * @brief This function handles DMA1 channel1 global interrupt.
  */
void DMA1_Channel1_IRQHandler(void)
{
  /* USER CODE BEGIN DMA1_Channel1_IRQn 0 */

  /* USER CODE END DMA1_Channel1_IRQn 0 */
  HAL_DMA_IRQHandler(&hdma_i2c1_tx);
  /* USER CODE BEGIN DMA1_Channel1_IRQn 1 */

  /* USER CODE END DMA1_Channel1_IRQn 1 */
}

/**
  * @brief This function handles DMA1 channel4 global interrupt.
  */
void DMA1_Channel4_IRQHandler(void)
{
  /* USER CODE BEGIN DMA1_Channel4_IRQn 0 */

  /* USER CODE END DMA1_Channel4_IRQn 0 */
  HAL_DMA_IRQHandler(&hdma_lpuart1_tx);
  /* USER CODE BEGIN DMA1_Channel4_IRQn 1 */

  /* USER CODE END DMA1_Channel4_IRQn 1 */
}

/**
  * @brief This function handles USART1 global interrupt.
  */
void USART1_IRQHandler(void)
{
  /* USER CODE BEGIN USART1_IRQn 0 */

  /* USER CODE END USART1_IRQn 0 */
  HAL_UART_IRQHandler(&huart1);
  /* USER CODE BEGIN USART1_IRQn 1 */

  /* USER CODE END USART1_IRQn 1 */
}

/**
  * @brief This function handles LPUART1 global interrupt.
  */
void LPUART1_IRQHandler(void)
{
  /* USER CODE BEGIN LPUART1_IRQn 0 */

  /* USER CODE END LPUART1_IRQn 0 */
  HAL_UART_IRQHandler(&hlpuart1);
  /* USER CODE BEGIN LPUART1_IRQn 1 */

  /* USER CODE END LPUART1_IRQn 1 */
}

/**
  * @brief This function handles IPCC RX occupied interrupt.
  */
void IPCC_C1_RX_IRQHandler(void)
{
  /* USER CODE BEGIN IPCC_C1_RX_IRQn 0 */

  /* USER CODE END IPCC_C1_RX_IRQn 0 */
  HAL_IPCC_RX_IRQHandler(&hipcc);
  /* USER CODE BEGIN IPCC_C1_RX_IRQn 1 */

  /* USER CODE END IPCC_C1_RX_IRQn 1 */
}

/**
  * @brief This function handles IPCC TX free interrupt.
  */
void IPCC_C1_TX_IRQHandler(void)
{
  /* USER CODE BEGIN IPCC_C1_TX_IRQn 0 */

  /* USER CODE END IPCC_C1_TX_IRQn 0 */
  HAL_IPCC_TX_IRQHandler(&hipcc);
  /* USER CODE BEGIN IPCC_C1_TX_IRQn 1 */

  /* USER CODE END IPCC_C1_TX_IRQn 1 */
}

/**
  * @brief This function handles HSEM global interrupt.
  */
void HSEM_IRQHandler(void)
{
  /* USER CODE BEGIN HSEM_IRQn 0 */

  /* USER CODE END HSEM_IRQn 0 */
  HAL_HSEM_IRQHandler();
  /* USER CODE BEGIN HSEM_IRQn 1 */

  /* USER CODE END HSEM_IRQn 1 */
}

/**
  * @brief This function handles DMA2 channel4 global interrupt.
  */
void DMA2_Channel4_IRQHandler(void)
{
  /* USER CODE BEGIN DMA2_Channel4_IRQn 0 */

  /* USER CODE END DMA2_Channel4_IRQn 0 */
  HAL_DMA_IRQHandler(&hdma_usart1_tx);
  /* USER CODE BEGIN DMA2_Channel4_IRQn 1 */

  /* USER CODE END DMA2_Channel4_IRQn 1 */
}

/* USER CODE BEGIN 1 */
/**
 * @brief  This function handles External line
 *         interrupt request.
 * @param  None
 * @retval None
 */
void PUSH_BUTTON_SW1_EXTI_IRQHandler(void)
{
  HAL_GPIO_EXTI_IRQHandler(BUTTON_SW1_PIN);
}

/**
 * @brief  This function handles External line
 *         interrupt request.
 * @param  None
 * @retval None
 */
void PUSH_BUTTON_SW2_EXTI_IRQHandler(void)
{
  HAL_GPIO_EXTI_IRQHandler(BUTTON_SW2_PIN);
}

/**
 * @brief  This function handles External line
 *         interrupt request.
 * @param  None
 * @retval None
 */
void PUSH_BUTTON_SW3_EXTI_IRQHandler(void)
{
  HAL_GPIO_EXTI_IRQHandler(BUTTON_SW3_PIN);
}

/* USER CODE END 1 */
````

**p2p_server_app.c**:
````c
/* USER CODE BEGIN Header */
/**
  ******************************************************************************
  * @file    p2p_server_app.c
  * @author  MCD Application Team
  * @brief   Peer to peer Server Application
  ******************************************************************************
  * @attention
  *
  * Copyright (c) 2019-2021 STMicroelectronics.
  * All rights reserved.
  *
  * This software is licensed under terms that can be found in the LICENSE file
  * in the root directory of this software component.
  * If no LICENSE file comes with this software, it is provided AS-IS.
  *
  ******************************************************************************
  */
/* USER CODE END Header */

/* Includes ------------------------------------------------------------------*/
#include "main.h"
#include "app_common.h"
#include "dbg_trace.h"
#include "ble.h"
#include "p2p_server_app.h"
#include "stm32_seq.h"

/* Private includes ----------------------------------------------------------*/
/* USER CODE BEGIN Includes */

/* USER CODE END Includes */

/* Private typedef -----------------------------------------------------------*/
/* USER CODE BEGIN PTD */
 typedef struct{
    uint8_t             Device_Led_Selection;
    uint8_t             Led1;
 }P2P_LedCharValue_t;

 typedef struct{
    uint8_t             Device_Button_Selection;
    uint8_t             ButtonStatus;
 }P2P_ButtonCharValue_t;

typedef struct
{
  uint8_t               Notification_Status; /* used to check if P2P Server is enabled to Notify */
  P2P_LedCharValue_t    LedControl;
  P2P_ButtonCharValue_t ButtonControl;
  uint16_t              ConnectionHandle;
} P2P_Server_App_Context_t;
/* USER CODE END PTD */

/* Private defines ------------------------------------------------------------*/
/* USER CODE BEGIN PD */

/* USER CODE END PD */

/* Private macros -------------------------------------------------------------*/
/* USER CODE BEGIN PM */

/* USER CODE END PM */

/* Private variables ---------------------------------------------------------*/
/* USER CODE BEGIN PV */
uint8_t temp=0;
uint32_t pressure=0;
/**
 * START of Section BLE_APP_CONTEXT
 */

static P2P_Server_App_Context_t P2P_Server_App_Context;

/**
 * END of Section BLE_APP_CONTEXT
 */
/* USER CODE END PV */

/* Private function prototypes -----------------------------------------------*/
/* USER CODE BEGIN PFP */
static void P2PS_Send_Notification(void);
static void P2PS_APP_LED_BUTTON_context_Init(void);
/* USER CODE END PFP */

/* Functions Definition ------------------------------------------------------*/
void P2PS_STM_App_Notification(P2PS_STM_App_Notification_evt_t *pNotification)
{
/* USER CODE BEGIN P2PS_STM_App_Notification_1 */

/* USER CODE END P2PS_STM_App_Notification_1 */
  switch(pNotification->P2P_Evt_Opcode)
  {
/* USER CODE BEGIN P2PS_STM_App_Notification_P2P_Evt_Opcode */
#if(BLE_CFG_OTA_REBOOT_CHAR != 0)
    case P2PS_STM_BOOT_REQUEST_EVT:
      APP_DBG_MSG("-- P2P APPLICATION SERVER : BOOT REQUESTED\n");
      APP_DBG_MSG(" \n\r");

      *(uint32_t*)SRAM1_BASE = *(uint32_t*)pNotification->DataTransfered.pPayload;
      NVIC_SystemReset();
      break;
#endif
/* USER CODE END P2PS_STM_App_Notification_P2P_Evt_Opcode */

    case P2PS_STM__NOTIFY_ENABLED_EVT:
/* USER CODE BEGIN P2PS_STM__NOTIFY_ENABLED_EVT */
      P2P_Server_App_Context.Notification_Status = 1;
      APP_DBG_MSG("-- P2P APPLICATION SERVER : NOTIFICATION ENABLED\n"); 
      APP_DBG_MSG(" \n\r");
/* USER CODE END P2PS_STM__NOTIFY_ENABLED_EVT */
      break;

    case P2PS_STM_NOTIFY_DISABLED_EVT:
/* USER CODE BEGIN P2PS_STM_NOTIFY_DISABLED_EVT */
      P2P_Server_App_Context.Notification_Status = 0;
      APP_DBG_MSG("-- P2P APPLICATION SERVER : NOTIFICATION DISABLED\n");
      APP_DBG_MSG(" \n\r");
/* USER CODE END P2PS_STM_NOTIFY_DISABLED_EVT */
      break;

    case P2PS_STM_WRITE_EVT:
/* USER CODE BEGIN P2PS_STM_WRITE_EVT */


































    	uint8_t *raw = pNotification->DataTransfered.pPayload;
    	uint8_t len  = pNotification->DataTransfered.Length;

    	if(raw[0] == 0xB0)   // ramka temperatury + ciśnienia
    	{
    	    temp = raw[1];

    	    pressure  = raw[2];
    	    pressure |= raw[3] << 8;
    	    pressure |= raw[4] << 16;
    	    pressure |= raw[5] << 24;

    	    APP_DBG_MSG("TEMP=%d, PRESS=%lu\n", temp, pressure);
    	    return;
    	}


//    	APP_DBG_MSG("RAW (%d bytes): ", len);
//    	for(uint8_t i = 0; i < len; i++)
//    	{
//    	    APP_DBG_MSG("0x%02X ", raw[i]);
//    	}
//    	APP_DBG_MSG("\n");
//
//    	if(len < 2)
//    	{
//    	    APP_DBG_MSG("FRAME TOO SHORT\n");
//    	    return;
//    	}
//
//    	// -------------------------
//    	// ODBIÓR TEMPERATURY
//    	// -------------------------
//    	temp = raw[1];
//
//    	// -------------------------
//    	// ODBIÓR CIŚNIENIA (4 bajty)
//    	// -------------------------
//    	uint32_t press = 0;
//
//    	if(len >= 6)
//    	{
//    	    press  = ((uint32_t)raw[2] << 0);
//    	    press |= ((uint32_t)raw[3] << 8);
//    	    press |= ((uint32_t)raw[4] << 16);
//    	    press |= ((uint32_t)raw[5] << 24);
//    	}
//    	else
//    	{
//    	    APP_DBG_MSG("NO PRESSURE DATA\n");
//    	}
//
//    	pressure = press;
//
//    	APP_DBG_MSG(">> RX | TEMP=%d C, PRESS=%lu Pa\r\n", temp, pressure);

























//
//
//
//
//
//
//      if(pNotification->DataTransfered.pPayload[0] == 0x00){ /* ALL Deviceselected - may be necessary as LB Routeur informs all connection */
//        if(pNotification->DataTransfered.pPayload[1] == 0x01)
//        {
//          BSP_LED_On(LED_BLUE);
//          APP_DBG_MSG("-- P2P APPLICATION SERVER  : LED1 ON\n");
//          APP_DBG_MSG(" \n\r");
//          P2P_Server_App_Context.LedControl.Led1=0x01; /* LED1 ON */
//        }
//        if(pNotification->DataTransfered.pPayload[1] == 0x00)
//        {
//          BSP_LED_Off(LED_BLUE);
//          APP_DBG_MSG("-- P2P APPLICATION SERVER  : LED1 OFF\n");
//          APP_DBG_MSG(" \n\r");
//          P2P_Server_App_Context.LedControl.Led1=0x00; /* LED1 OFF */
//        }
//      }
//#if(P2P_SERVER1 != 0)
//      if(pNotification->DataTransfered.pPayload[0] == 0x01){ /* end device 1 selected - may be necessary as LB Routeur informs all connection */
//        if(pNotification->DataTransfered.pPayload[1] == 0x01)
//        {
//          BSP_LED_On(LED_BLUE);
//          APP_DBG_MSG("-- P2P APPLICATION SERVER 1 : LED1 ON\n");
//          APP_DBG_MSG(" \n\r");
//          P2P_Server_App_Context.LedControl.Led1=0x01; /* LED1 ON */
//        }
//        if(pNotification->DataTransfered.pPayload[1] == 0x00)
//        {
//          BSP_LED_Off(LED_BLUE);
//          APP_DBG_MSG("-- P2P APPLICATION SERVER 1 : LED1 OFF\n");
//          APP_DBG_MSG(" \n\r");
//          P2P_Server_App_Context.LedControl.Led1=0x00; /* LED1 OFF */
//        }
//      }
//#endif
//#if(P2P_SERVER2 != 0)
//      if(pNotification->DataTransfered.pPayload[0] == 0x02){ /* end device 2 selected */
//        if(pNotification->DataTransfered.pPayload[1] == 0x01)
//        {
//          BSP_LED_On(LED_BLUE);
//           APP_DBG_MSG("-- P2P APPLICATION SERVER 2 : LED1 ON\n");
//          APP_DBG_MSG(" \n\r");
//          P2P_Server_App_Context.LedControl.Led1=0x01; /* LED1 ON */
//        }
//        if(pNotification->DataTransfered.pPayload[1] == 0x00)
//        {
//          BSP_LED_Off(LED_BLUE);
//          APP_DBG_MSG("-- P2P APPLICATION SERVER 2 : LED1 OFF\n");
//          APP_DBG_MSG(" \n\r");
//          P2P_Server_App_Context.LedControl.Led1=0x00; /* LED1 OFF */
//        }
//      }
//#endif
//#if(P2P_SERVER3 != 0)
//      if(pNotification->DataTransfered.pPayload[0] == 0x03){ /* end device 3 selected - may be necessary as LB Routeur informs all connection */
//        if(pNotification->DataTransfered.pPayload[1] == 0x01)
//        {
//          BSP_LED_On(LED_BLUE);
//          APP_DBG_MSG("-- P2P APPLICATION SERVER 3 : LED1 ON\n");
//          APP_DBG_MSG(" \n\r");
//          P2P_Server_App_Context.LedControl.Led1=0x01; /* LED1 ON */
//        }
//        if(pNotification->DataTransfered.pPayload[1] == 0x00)
//        {
//          BSP_LED_Off(LED_BLUE);
//          APP_DBG_MSG("-- P2P APPLICATION SERVER 3 : LED1 OFF\n");
//          APP_DBG_MSG(" \n\r");
//          P2P_Server_App_Context.LedControl.Led1=0x00; /* LED1 OFF */
//        }
//      }
//#endif
//#if(P2P_SERVER4 != 0)
//      if(pNotification->DataTransfered.pPayload[0] == 0x04){ /* end device 4 selected */
//        if(pNotification->DataTransfered.pPayload[1] == 0x01)
//        {
//          BSP_LED_On(LED_BLUE);
//           APP_DBG_MSG("-- P2P APPLICATION SERVER 2 : LED1 ON\n");
//          APP_DBG_MSG(" \n\r");
//          P2P_Server_App_Context.LedControl.Led1=0x01; /* LED1 ON */
//        }
//        if(pNotification->DataTransfered.pPayload[1] == 0x00)
//        {
//          BSP_LED_Off(LED_BLUE);
//          APP_DBG_MSG("-- P2P APPLICATION SERVER 2 : LED1 OFF\n");
//          APP_DBG_MSG(" \n\r");
//          P2P_Server_App_Context.LedControl.Led1=0x00; /* LED1 OFF */
//        }
//      }
//#endif
//#if(P2P_SERVER5 != 0)
//      if(pNotification->DataTransfered.pPayload[0] == 0x05){ /* end device 5 selected - may be necessary as LB Routeur informs all connection */
//        if(pNotification->DataTransfered.pPayload[1] == 0x01)
//        {
//          BSP_LED_On(LED_BLUE);
//          APP_DBG_MSG("-- P2P APPLICATION SERVER 5 : LED1 ON\n");
//          APP_DBG_MSG(" \n\r");
//          P2P_Server_App_Context.LedControl.Led1=0x01; /* LED1 ON */
//        }
//        if(pNotification->DataTransfered.pPayload[1] == 0x00)
//        {
//          BSP_LED_Off(LED_BLUE);
//          APP_DBG_MSG("-- P2P APPLICATION SERVER 5 : LED1 OFF\n");
//          APP_DBG_MSG(" \n\r");
//          P2P_Server_App_Context.LedControl.Led1=0x00; /* LED1 OFF */
//        }
//      }
//#endif
//#if(P2P_SERVER6 != 0)
//      if(pNotification->DataTransfered.pPayload[0] == 0x06){ /* end device 6 selected */
//        if(pNotification->DataTransfered.pPayload[1] == 0x01)
//        {
//          BSP_LED_On(LED_BLUE);
//           APP_DBG_MSG("-- P2P APPLICATION SERVER 6 : LED1 ON\n");
//          APP_DBG_MSG(" \n\r");
//          P2P_Server_App_Context.LedControl.Led1=0x01; /* LED1 ON */
//        }
//        if(pNotification->DataTransfered.pPayload[1] == 0x00)
//        {
//          BSP_LED_Off(LED_BLUE);
//          APP_DBG_MSG("-- P2P APPLICATION SERVER 6 : LED1 OFF\n");
//          APP_DBG_MSG(" \n\r");
//          P2P_Server_App_Context.LedControl.Led1=0x00; /* LED1 OFF */
//        }
//      }
//#endif
/* USER CODE END P2PS_STM_WRITE_EVT */
      break;

    default:
/* USER CODE BEGIN P2PS_STM_App_Notification_default */
      
/* USER CODE END P2PS_STM_App_Notification_default */
      break;
  }
/* USER CODE BEGIN P2PS_STM_App_Notification_2 */

/* USER CODE END P2PS_STM_App_Notification_2 */
  return;
}

void P2PS_APP_Notification(P2PS_APP_ConnHandle_Not_evt_t *pNotification)
{
/* USER CODE BEGIN P2PS_APP_Notification_1 */

/* USER CODE END P2PS_APP_Notification_1 */
  switch(pNotification->P2P_Evt_Opcode)
  {
/* USER CODE BEGIN P2PS_APP_Notification_P2P_Evt_Opcode */

/* USER CODE END P2PS_APP_Notification_P2P_Evt_Opcode */
  case PEER_CONN_HANDLE_EVT :
/* USER CODE BEGIN PEER_CONN_HANDLE_EVT */
          
/* USER CODE END PEER_CONN_HANDLE_EVT */
    break;

    case PEER_DISCON_HANDLE_EVT :
/* USER CODE BEGIN PEER_DISCON_HANDLE_EVT */
       P2PS_APP_LED_BUTTON_context_Init();       
/* USER CODE END PEER_DISCON_HANDLE_EVT */
    break;

    default:
/* USER CODE BEGIN P2PS_APP_Notification_default */

/* USER CODE END P2PS_APP_Notification_default */
      break;
  }
/* USER CODE BEGIN P2PS_APP_Notification_2 */

/* USER CODE END P2PS_APP_Notification_2 */
  return;
}

void P2PS_APP_Init(void)
{
/* USER CODE BEGIN P2PS_APP_Init */
  UTIL_SEQ_RegTask( 1<< CFG_TASK_SW1_BUTTON_PUSHED_ID, UTIL_SEQ_RFU, P2PS_Send_Notification );

  /**
   * Initialize LedButton Service
   */
  P2P_Server_App_Context.Notification_Status=0; 
  P2PS_APP_LED_BUTTON_context_Init();
/* USER CODE END P2PS_APP_Init */
  return;
}

/* USER CODE BEGIN FD */
void P2PS_APP_LED_BUTTON_context_Init(void){
  
  BSP_LED_Off(LED_BLUE);
  APP_DBG_MSG("LED BLUE OFF\n");
  
  #if(P2P_SERVER1 != 0)
  P2P_Server_App_Context.LedControl.Device_Led_Selection=0x01; /* Device1 */
  P2P_Server_App_Context.LedControl.Led1=0x00; /* led OFF */
  P2P_Server_App_Context.ButtonControl.Device_Button_Selection=0x01;/* Device1 */
  P2P_Server_App_Context.ButtonControl.ButtonStatus=0x00;
#endif
#if(P2P_SERVER2 != 0)
  P2P_Server_App_Context.LedControl.Device_Led_Selection=0x02; /* Device2 */
  P2P_Server_App_Context.LedControl.Led1=0x00; /* led OFF */
  P2P_Server_App_Context.ButtonControl.Device_Button_Selection=0x02;/* Device2 */
  P2P_Server_App_Context.ButtonControl.ButtonStatus=0x00;
#endif  
#if(P2P_SERVER3 != 0)
  P2P_Server_App_Context.LedControl.Device_Led_Selection=0x03; /* Device3 */
  P2P_Server_App_Context.LedControl.Led1=0x00; /* led OFF */
  P2P_Server_App_Context.ButtonControl.Device_Button_Selection=0x03; /* Device3 */
  P2P_Server_App_Context.ButtonControl.ButtonStatus=0x00;
#endif
#if(P2P_SERVER4 != 0)
  P2P_Server_App_Context.LedControl.Device_Led_Selection=0x04; /* Device4 */
  P2P_Server_App_Context.LedControl.Led1=0x00; /* led OFF */
  P2P_Server_App_Context.ButtonControl.Device_Button_Selection=0x04; /* Device4 */
  P2P_Server_App_Context.ButtonControl.ButtonStatus=0x00;
#endif  
 #if(P2P_SERVER5 != 0)
  P2P_Server_App_Context.LedControl.Device_Led_Selection=0x05; /* Device5 */
  P2P_Server_App_Context.LedControl.Led1=0x00; /* led OFF */
  P2P_Server_App_Context.ButtonControl.Device_Button_Selection=0x05; /* Device5 */
  P2P_Server_App_Context.ButtonControl.ButtonStatus=0x00;
#endif
#if(P2P_SERVER6 != 0)
  P2P_Server_App_Context.LedControl.Device_Led_Selection=0x06; /* device6 */
  P2P_Server_App_Context.LedControl.Led1=0x00; /* led OFF */
  P2P_Server_App_Context.ButtonControl.Device_Button_Selection=0x06; /* Device6 */
  P2P_Server_App_Context.ButtonControl.ButtonStatus=0x00;
#endif  
}

void P2PS_APP_SW1_Button_Action(void)
{
  UTIL_SEQ_SetTask( 1<<CFG_TASK_SW1_BUTTON_PUSHED_ID, CFG_SCH_PRIO_0);

  return;
}
/* USER CODE END FD */

/*************************************************************
 *
 * LOCAL FUNCTIONS
 *
 *************************************************************/
/* USER CODE BEGIN FD_LOCAL_FUNCTIONS*/
void P2PS_Send_Notification(void)
{
 
  if(P2P_Server_App_Context.ButtonControl.ButtonStatus == 0x00){
    P2P_Server_App_Context.ButtonControl.ButtonStatus=0x01;
  } else {
    P2P_Server_App_Context.ButtonControl.ButtonStatus=0x00;
  }
  
   if(P2P_Server_App_Context.Notification_Status){ 
    APP_DBG_MSG("-- P2P APPLICATION SERVER  : INFORM CLIENT BUTTON 1 PUSHED \n ");
    APP_DBG_MSG(" \n\r");
    P2PS_STM_App_Update_Char(P2P_NOTIFY_CHAR_UUID, (uint8_t *)&P2P_Server_App_Context.ButtonControl);
   } else {
    APP_DBG_MSG("-- P2P APPLICATION SERVER : CAN'T INFORM CLIENT -  NOTIFICATION DISABLED\n "); 
   }

  return;
}

/* USER CODE END FD_LOCAL_FUNCTIONS*/
````
# The server should know how many bytes it will receive - we change the characteristics in p2p_stm.c file:
**It is neccessary to change this every generation code in cubeMx!**
````c
/**
     *  Add LED Characteristic
     */
    COPY_P2P_WRITE_CHAR_UUID(uuid16.Char_UUID_128);
    aci_gatt_add_char(aPeerToPeerContext.PeerToPeerSvcHdle,
                      UUID_TYPE_128, &uuid16,
                      6,	//<-------the change from default 2 to 6
                      CHAR_PROP_WRITE_WITHOUT_RESP|CHAR_PROP_READ,
                      ATTR_PERMISSION_NONE,
                      GATT_NOTIFY_ATTRIBUTE_WRITE, /* gattEvtMask */
                      10, /* encryKeySize */
                      1, /* isVariable */
                      &(aPeerToPeerContext.P2PWriteClientToServerCharHdle));

    /**
     *   Add Button Characteristic
     */
    COPY_P2P_NOTIFY_UUID(uuid16.Char_UUID_128);
    aci_gatt_add_char(aPeerToPeerContext.PeerToPeerSvcHdle,
                      UUID_TYPE_128, &uuid16,
                      6,	//<-------the change from default 2 to 6
                      CHAR_PROP_NOTIFY,
                      ATTR_PERMISSION_NONE,
                      GATT_NOTIFY_ATTRIBUTE_WRITE, /* gattEvtMask */
                      10, /* encryKeySize */
                      1, /* isVariable: 1 */
                      &(aPeerToPeerContext.P2PNotifyServerToClientCharHdle));
 ````




**p2p_stm.c**:
````c
/**
  ******************************************************************************
  * @file    p2p_stm.c
  * @author  MCD Application Team
  * @brief   Peer to Peer Service (Custom STM)
  ******************************************************************************
  * @attention
  *
  * Copyright (c) 2018-2021 STMicroelectronics.
  * All rights reserved.
  *
  * This software is licensed under terms that can be found in the LICENSE file
  * in the root directory of this software component.
  * If no LICENSE file comes with this software, it is provided AS-IS.
  *
  ******************************************************************************
  */


/* Includes ------------------------------------------------------------------*/
#include "common_blesvc.h"

/* Private typedef -----------------------------------------------------------*/
typedef struct{
  uint16_t	PeerToPeerSvcHdle;				        /**< Service handle */
  uint16_t	P2PWriteClientToServerCharHdle;	  /**< Characteristic handle */
  uint16_t	P2PNotifyServerToClientCharHdle;	/**< Characteristic handle */
#if(BLE_CFG_OTA_REBOOT_CHAR != 0)
  uint16_t  RebootReqCharHdle;                /**< Characteristic handle */
#endif
}PeerToPeerContext_t;

/* Private defines -----------------------------------------------------------*/
#define UUID_128_SUPPORTED  1

#if (UUID_128_SUPPORTED == 1)
#define BM_UUID_LENGTH  UUID_TYPE_128
#else
#define BM_UUID_LENGTH  UUID_TYPE_16
#endif

#define BM_REQ_CHAR_SIZE    (3)


/* Private macros ------------------------------------------------------------*/

/* Private variables ---------------------------------------------------------*/
/**
 * Reboot Characteristic UUID
 * 0000fe11-8e22-4541-9d4c-21edae82ed19
 */
#if(BLE_CFG_OTA_REBOOT_CHAR != 0)
#if (UUID_128_SUPPORTED == 1)
static const uint8_t BM_REQ_CHAR_UUID[16] = {0x19, 0xed, 0x82, 0xae,
                                       0xed, 0x21, 0x4c, 0x9d,
                                       0x41, 0x45, 0x22, 0x8e,
                                       0x11, 0xFE, 0x00, 0x00};
#else
static const uint8_t BM_REQ_CHAR_UUID[2] = {0x11, 0xFE};
#endif
#endif

/**
 * START of Section BLE_DRIVER_CONTEXT
 */
PLACE_IN_SECTION("BLE_DRIVER_CONTEXT") static PeerToPeerContext_t aPeerToPeerContext;

/**
 * END of Section BLE_DRIVER_CONTEXT
 */
/* Private function prototypes -----------------------------------------------*/
static SVCCTL_EvtAckStatus_t PeerToPeer_Event_Handler(void *Event);


/* Functions Definition ------------------------------------------------------*/
/* Private functions ----------------------------------------------------------*/

#define COPY_UUID_128(uuid_struct, uuid_15, uuid_14, uuid_13, uuid_12, uuid_11, uuid_10, uuid_9, uuid_8, uuid_7, uuid_6, uuid_5, uuid_4, uuid_3, uuid_2, uuid_1, uuid_0) \
do {\
    uuid_struct[0] = uuid_0; uuid_struct[1] = uuid_1; uuid_struct[2] = uuid_2; uuid_struct[3] = uuid_3; \
        uuid_struct[4] = uuid_4; uuid_struct[5] = uuid_5; uuid_struct[6] = uuid_6; uuid_struct[7] = uuid_7; \
            uuid_struct[8] = uuid_8; uuid_struct[9] = uuid_9; uuid_struct[10] = uuid_10; uuid_struct[11] = uuid_11; \
                uuid_struct[12] = uuid_12; uuid_struct[13] = uuid_13; uuid_struct[14] = uuid_14; uuid_struct[15] = uuid_15; \
}while(0)

/* Hardware Characteristics Service */
/*
 The following 128bits UUIDs have been generated from the random UUID
 generator:
 D973F2E0-B19E-11E2-9E96-0800200C9A66: Service 128bits UUID
 D973F2E1-B19E-11E2-9E96-0800200C9A66: Characteristic_1 128bits UUID
 D973F2E2-B19E-11E2-9E96-0800200C9A66: Characteristic_2 128bits UUID
 */
#define COPY_P2P_SERVICE_UUID(uuid_struct)       COPY_UUID_128(uuid_struct,0x00,0x00,0xfe,0x40,0xcc,0x7a,0x48,0x2a,0x98,0x4a,0x7f,0x2e,0xd5,0xb3,0xe5,0x8f)
#define COPY_P2P_WRITE_CHAR_UUID(uuid_struct)    COPY_UUID_128(uuid_struct,0x00,0x00,0xfe,0x41,0x8e,0x22,0x45,0x41,0x9d,0x4c,0x21,0xed,0xae,0x82,0xed,0x19)
#define COPY_P2P_NOTIFY_UUID(uuid_struct)        COPY_UUID_128(uuid_struct,0x00,0x00,0xfe,0x42,0x8e,0x22,0x45,0x41,0x9d,0x4c,0x21,0xed,0xae,0x82,0xed,0x19)



/**
 * @brief  Event handler
 * @param  Event: Address of the buffer holding the Event
 * @retval Ack: Return whether the Event has been managed or not
 */
static SVCCTL_EvtAckStatus_t PeerToPeer_Event_Handler(void *Event)
{
  SVCCTL_EvtAckStatus_t return_value;
  hci_event_pckt *event_pckt;
  evt_blecore_aci *blecore_evt;
  aci_gatt_attribute_modified_event_rp0    * attribute_modified;
  P2PS_STM_App_Notification_evt_t Notification;

  return_value = SVCCTL_EvtNotAck;
  event_pckt = (hci_event_pckt *)(((hci_uart_pckt*)Event)->data);

  switch(event_pckt->evt)
  {
    case HCI_VENDOR_SPECIFIC_DEBUG_EVT_CODE:
    {
      blecore_evt = (evt_blecore_aci*)event_pckt->data;
      switch(blecore_evt->ecode)
      {
        case ACI_GATT_ATTRIBUTE_MODIFIED_VSEVT_CODE:
       {
          attribute_modified = (aci_gatt_attribute_modified_event_rp0*)blecore_evt->data;
            if(attribute_modified->Attr_Handle == (aPeerToPeerContext.P2PNotifyServerToClientCharHdle + 2))
            {
              /**
               * Descriptor handle
               */
              return_value = SVCCTL_EvtAckFlowEnable;
              /**
               * Notify to application
               */
              if(attribute_modified->Attr_Data[0] & COMSVC_Notification)
              {
                Notification.P2P_Evt_Opcode = P2PS_STM__NOTIFY_ENABLED_EVT;
                P2PS_STM_App_Notification(&Notification);
              }
              else
              {
                Notification.P2P_Evt_Opcode = P2PS_STM_NOTIFY_DISABLED_EVT;
                P2PS_STM_App_Notification(&Notification);
              }
            }
            
            else if(attribute_modified->Attr_Handle == (aPeerToPeerContext.P2PWriteClientToServerCharHdle + 1))
            {
              BLE_DBG_P2P_STM_MSG("-- GATT : LED CONFIGURATION RECEIVED\n");
              Notification.P2P_Evt_Opcode = P2PS_STM_WRITE_EVT;
              Notification.DataTransfered.Length=attribute_modified->Attr_Data_Length;
              Notification.DataTransfered.pPayload=attribute_modified->Attr_Data;
              P2PS_STM_App_Notification(&Notification);  
            }
#if(BLE_CFG_OTA_REBOOT_CHAR != 0)
            else if(attribute_modified->Attr_Handle == (aPeerToPeerContext.RebootReqCharHdle + 1))
            {
              BLE_DBG_P2P_STM_MSG("-- GATT : REBOOT REQUEST RECEIVED\n");
              Notification.P2P_Evt_Opcode = P2PS_STM_BOOT_REQUEST_EVT;
              Notification.DataTransfered.Length=attribute_modified->Attr_Data_Length;
              Notification.DataTransfered.pPayload=attribute_modified->Attr_Data;
              P2PS_STM_App_Notification(&Notification);
            }
#endif
        }
        break;

        default:
          break;
      }
    }
    break; /* HCI_HCI_VENDOR_SPECIFIC_DEBUG_EVT_CODE_SPECIFIC */

    default:
      break;
  }

  return(return_value);
}/* end SVCCTL_EvtAckStatus_t */


/* Public functions ----------------------------------------------------------*/

/**
 * @brief  Service initialization
 * @param  None
 * @retval None
 */
void P2PS_STM_Init(void)
{
 
  Char_UUID_t  uuid16;

  /**
   *	Register the event handler to the BLE controller
   */
  SVCCTL_RegisterSvcHandler(PeerToPeer_Event_Handler);
  
    /**
     *  Peer To Peer Service
     *
     * Max_Attribute_Records = 2*no_of_char + 1
     * service_max_attribute_record = 1 for Peer To Peer service +
     *                                2 for P2P Write characteristic +
     *                                2 for P2P Notify characteristic +
     *                                1 for client char configuration descriptor +
     *                                
     */
    COPY_P2P_SERVICE_UUID(uuid16.Char_UUID_128);
    aci_gatt_add_service(UUID_TYPE_128,
                      (Service_UUID_t *) &uuid16,
                      PRIMARY_SERVICE,
#if (BLE_CFG_OTA_REBOOT_CHAR != 0)
                      2+
#endif                      
                      6,
                      &(aPeerToPeerContext.PeerToPeerSvcHdle));

    /**
     *  Add LED Characteristic
     */
    COPY_P2P_WRITE_CHAR_UUID(uuid16.Char_UUID_128);
    aci_gatt_add_char(aPeerToPeerContext.PeerToPeerSvcHdle,
                      UUID_TYPE_128, &uuid16,
                      6,
                      CHAR_PROP_WRITE_WITHOUT_RESP|CHAR_PROP_READ,
                      ATTR_PERMISSION_NONE,
                      GATT_NOTIFY_ATTRIBUTE_WRITE, /* gattEvtMask */
                      10, /* encryKeySize */
                      1, /* isVariable */
                      &(aPeerToPeerContext.P2PWriteClientToServerCharHdle));

    /**
     *   Add Button Characteristic
     */
    COPY_P2P_NOTIFY_UUID(uuid16.Char_UUID_128);
    aci_gatt_add_char(aPeerToPeerContext.PeerToPeerSvcHdle,
                      UUID_TYPE_128, &uuid16,
                      6,
                      CHAR_PROP_NOTIFY,
                      ATTR_PERMISSION_NONE,
                      GATT_NOTIFY_ATTRIBUTE_WRITE, /* gattEvtMask */
                      10, /* encryKeySize */
                      1, /* isVariable: 1 */
                      &(aPeerToPeerContext.P2PNotifyServerToClientCharHdle));
 
#if(BLE_CFG_OTA_REBOOT_CHAR != 0)      
    /**
     *  Add Boot Request Characteristic
     */
    aci_gatt_add_char(aPeerToPeerContext.PeerToPeerSvcHdle,
                      BM_UUID_LENGTH,
                      (Char_UUID_t *)BM_REQ_CHAR_UUID,
                      BM_REQ_CHAR_SIZE,
                      CHAR_PROP_WRITE_WITHOUT_RESP,
                      ATTR_PERMISSION_NONE,
                      GATT_NOTIFY_ATTRIBUTE_WRITE,
                      10,
                      0,
                      &(aPeerToPeerContext.RebootReqCharHdle));
#endif    

    
  return;
}

/**
 * @brief  Characteristic update
 * @param  UUID: UUID of the characteristic
 * @param  Service_Instance: Instance of the service to which the characteristic belongs
 * 
 */
tBleStatus P2PS_STM_App_Update_Char(uint16_t UUID, uint8_t *pPayload) 
{
  tBleStatus result = BLE_STATUS_INVALID_PARAMS;
  switch(UUID)
  {
    case P2P_NOTIFY_CHAR_UUID:
      
     result = aci_gatt_update_char_value(aPeerToPeerContext.PeerToPeerSvcHdle,
                             aPeerToPeerContext.P2PNotifyServerToClientCharHdle,
                              0, /* charValOffset */
                             2, /* charValueLen */
                             (uint8_t *)  pPayload);
    
      break;

    default:
      break;
  }

  return result;
}/* end P2PS_STM_Init() */


````































