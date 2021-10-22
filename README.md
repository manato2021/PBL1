/home/pi/work/camera_smile/
├app.py・・・main処理 
├camera.py・・・笑顔判定処理 
└talk.py・・・文字列を音声に変換し、スピーカーから音声出力
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
