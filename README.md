# 🎄 Python Christmas Tree

Анимация новогодней ёлки на Python с цветными звёздочками и текстом песни.  
Каждая звёздочка меняет цвет, а справа появляется текст по буквам, создавая эффект "печатающегося текста".

## 🔹 Возможности
- Цветная анимация ёлки в консоли  
- Печатающийся текст справа от ёлки  
- Настраиваемые задержки анимации  
- Работает на Windows, Linux и MacOS  

## 🔹 Как использовать
1. Скопируйте `tree.py` на свой компьютер  
2. Запустите через Python 3:
```bash
python tree.py
import os
import time
import random
import sys

if os.name == 'nt':
    os.system('')

# ===== Настройки =====
TREE_RAW = [
    "      * ",
    "     *** ",
    "    ***** ",
    "   ******* ",
    "  ********* ",
    " *********** ",
    "     |||     ",
    "             "  
]

COLORS = ["\033[91m", "\033[93m", "\033[94m", "\033[95m", "\033[96m"]
GREEN_TEXT = "\033[92m"
RESET = "\033[0m"
CLEAR = "\033[H"

H_GAP = 5          
FRAME_DELAY = 0.08 
MAX_TREE_WIDTH = max(len(line) for line in TREE_RAW)

LYRICS = [
    "A face on a lover",
    "With a fire in his heart",
    "A man undercover,",
    "But you tore me apart",
    "Oh oh",  
    "oh oh",  
    "Now I've found a real love,",
    "You'll never fool me again" # Теперь эта строка появится!
]

def run_animation():
    text_index = 0
    char_index = 0
    
    os.system('cls' if os.name == 'nt' else 'clear')
    sys.stdout.write("\033[?25l")

    try:
        while True:
            output = [CLEAR] 
            # Теперь цикл идет по самому длинному списку
            max_lines = max(len(TREE_RAW), len(LYRICS))
            
            for i in range(max_lines):
                # Рисуем елку (если строки елки кончились, рисуем пустоту)
                if i < len(TREE_RAW):
                    tree_line = "".join(random.choice(COLORS) + "*" + RESET if c == "*" else c for c in TREE_RAW[i])
                    current_tree_width = len(TREE_RAW[i])
                else:
                    tree_line = " " * MAX_TREE_WIDTH
                    current_tree_width = MAX_TREE_WIDTH
                
                # Выравнивание
                padding = " " * (MAX_TREE_WIDTH - current_tree_width + H_GAP)
                
                # Текст
                lyric_part = ""
                if i < len(LYRICS):
                    if i < text_index:
                        lyric_part = GREEN_TEXT + LYRICS[i] + RESET
                    elif i == text_index:
                        lyric_part = GREEN_TEXT + LYRICS[i][:char_index] + RESET
                
                output.append(tree_line + padding + lyric_part)

            sys.stdout.write("\n".join(output) + "\n")
            sys.stdout.flush()

            # Логика пауз
            if char_index == 0:
                if LYRICS[text_index].lower() == "oh oh":
                    time.sleep(1.0) # Задержка для "oh oh"
                else:
                    time.sleep(0.4)

            char_index += 1
            
            if char_index > len(LYRICS[text_index]):
                time.sleep(0.2)
                char_index = 0
                text_index += 1
                
                if text_index >= len(LYRICS):
                    time.sleep(2.0)
                    text_index = 0
                    sys.stdout.write("\033[J") 
            
            time.sleep(FRAME_DELAY)

    except KeyboardInterrupt:
        sys.stdout.write("\033[?25h")
        print("\nСчастливого Нового Года!")

if __name__ == "__main__":
    run_animation()
