###写真撮影
import time
import picamera
import cv2 as cv

fn = 'my_pic.jpg'

with picamera.Picamera() as camera:
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
from PIL Image
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


