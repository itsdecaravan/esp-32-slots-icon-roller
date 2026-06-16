
from machine import Pin, I2C
from time import sleep_ms
import random
import ssd1306

i2c = I2C(0, scl=Pin(22), sda=Pin(21))
oled = ssd1306.SSD1306_I2C(128, 64, i2c)

symbols = ["cherry", "star", "diamond", "heart", "lemon", "seven"]
reels = [0, 1, 2]

def block(x, y, w=2, h=2):
    oled.fill_rect(x, y, w, h, 1)

def draw_cherry(x, y):
    block(x + 1, y + 8, 5, 5)
    block(x + 9, y + 8, 5, 5)
    oled.line(x + 4, y + 7, x + 7, y + 2, 1)
    oled.line(x + 11, y + 7, x + 7, y + 2, 1)
    oled.pixel(x + 7, y + 1, 1)

def draw_star(x, y):
    oled.line(x + 7, y, x + 7, y + 14, 1)
    oled.line(x, y + 7, x + 14, y + 7, 1)
    oled.line(x + 2, y + 2, x + 12, y + 12, 1)
    oled.line(x + 12, y + 2, x + 2, y + 12, 1)

def draw_diamond(x, y):
    oled.line(x + 7, y, x + 14, y + 7, 1)
    oled.line(x + 14, y + 7, x + 7, y + 14, 1)
    oled.line(x + 7, y + 14, x, y + 7, 1)
    oled.line(x, y + 7, x + 7, y, 1)

def draw_heart(x, y):
    block(x + 1, y + 2, 4, 4)
    block(x + 9, y + 2, 4, 4)
    oled.line(x + 1, y + 5, x + 7, y + 13, 1)
    oled.line(x + 13, y + 5, x + 7, y + 13, 1)

def draw_lemon(x, y):
    oled.fill_rect(x + 2, y + 4, 10, 7, 1)

def draw_seven(x, y):
    oled.line(x + 1, y + 1, x + 13, y + 1, 1)
    oled.line(x + 13, y + 1, x + 5, y + 13, 1)

def draw_symbol(name, x, y):
    if name == "cherry":
        draw_cherry(x, y)
    elif name == "star":
        draw_star(x, y)
    elif name == "diamond":
        draw_diamond(x, y)
    elif name == "heart":
        draw_heart(x, y)
    elif name == "lemon":
        draw_lemon(x, y)
    elif name == "seven":
        draw_seven(x, y)

def draw_screen(msg):
    oled.fill(0)
    oled.text("ICON REELS", 24, 0)

    for x in (4, 44, 84):
        oled.rect(x, 18, 28, 24, 1)

    draw_symbol(symbols[reels[0]], 10, 23)
    draw_symbol(symbols[reels[1]], 50, 23)
    draw_symbol(symbols[reels[2]], 90, 23)

    oled.text(msg, 0, 54)
    oled.show()

while True:
    for _ in range(20):
        reels[0] = (reels[0] + 1) % len(symbols)
        reels[1] = (reels[1] + 2) % len(symbols)
        reels[2] = (reels[2] + 3) % len(symbols)
        draw_screen("Rolling...")
        sleep_ms(120)

    draw_screen("Result")
    sleep_ms(1500)
