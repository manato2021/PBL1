/home/pi/work/camera_smile/
├app.py・・・main処理 
├camera.py・・・笑顔判定処理 
└talk.py・・・スピーカーから音声出力
#coding:utf-8
import camera
import talk

SMILE_JUDGE = 1


if __name__ == '__main__':
    camera = camera.Camera()
    smile_count = 0
    while True:
        if camera.detect_smile():
            print('smile detect')
            smile_count += 1
        else:
            smile_count = 0
        if camera.check_stop():
            print('camera stop')
            camera.stop_camera()
            break
        if smile_count >= SMILE_JUDGE:
            smile_count = 0
           talk.talk(f'
           import RPi.GPIO as GPIO
           import time
           pin = 21
           GPIO.setmode(GPIO.BCM)
           GPIO.setup(pin,GPIO.OUT,initial=GPIO.LOW)
           p = GPIO.PWM(pin,1)
           p.start(50)
           p.ChangeFrequency(220)
           time.sleep(3)
           p.stop()
           GPIO.cleanup()
           ')






###写真撮影
import time
import picamera
import cv2 as cv

fn = 'my_pic.jpg'

with picamera.PiCamera() as camera:
camera.resolution = (480,360)
#準備
camera.start_preview()
#少しの間待機
time.sleep(2)
#写真を指定した名前で保存
camera.capture(fn)

##yoloでの画像認識どう使う？

############

#文字の取り込み
from PIL import Image
import sys

import pyocr
import pyocr.builders

tools = pyocr.get_available_tools()
if len(tools) == 0:
    print("No OCR tool found")
    sys.exit(1)
tool = tools[0]

line_andword_boxes = tool.image_to_string(
    Image.open(fn),
    lang="jpn",
    builder=pyocr.builders.TextBuilder()
)

print(line_andword_boxes)

####画像に含まれる文字にpeopeleがあればスピーカーを鳴らす



人が通ったら音が鳴るらしいから画像は無理説
import time
import RPi.GPIO as GPIO
import pygame.mixer

PLAY_TIME = 19
SLEEPTIME = 1
SENSOR_PIN = 18

GPIO.setmode(GPIO.BCM)
GPIO.setup(SENSOR_PIN, GPIO.IN)

pygame.mixer.init()
pygame.mixer.music.load("Bell.mp3")
pygame.mixer.music.play(-1)

try:
    while True:
        print (GPIO.input(SENSOR_PIN))
        if(GPIO.input(SENSOR_PIN) == GPIO.HIGH):
            pygame.mixer.music.unpause()
            time.sleep(PLAY_TIME)
        if(GPIO.input(SENSOR_PIN) == GPIO.LOW):
            pygame.mixer.music.pause()
        time.sleep(SLEEPTIME)

except KeyboardInterrupt:
    pygame.mixer.music.stop()
    pygame.mixer.quit()
    pygame.quit()
    GPIO.cleanup()




音が鳴るコードどの音
　
import time
import numpy as np            # install : conda install numpy
from scipy.io import wavfile  # install : conda install scipy
from pygame import mixer      # pip install pygame
# パラメータ
FREQ = 261.626          # 生成するサイン波の周波数（note#60 C4 ド）
SAMPLE_RATE = 44100     # サンプリングレート
# 16bitのwavファイルを作成
wavfile.write("do.wav", SAMPLE_RATE,
              (np.sin(np.arange(SAMPLE_RATE) * FREQ * np.pi * 2 / SAMPLE_RATE) * 32767.0).astype(np.int16))
# wavファイルをロードして再生
mixer.init()  # mixerを初期化
mixer.music.load("do.wav")  # wavをロード
mixer.music.play(1)  # wavを1回再生
# 1秒（音がおわるまで）待つ
time.sleep(1)
