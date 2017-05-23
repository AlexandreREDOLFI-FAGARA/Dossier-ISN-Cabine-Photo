#RUBERTE Arthur
#RANCIER Nicolas
#REDOLFI-FAGARA Alexandre
#PROJET CABINE PHOTO

from picamera import PiCamera #on importe le module picamera
from time import sleep #on importe la librairie time et sleep
import time #on importe le temps
from grovepi import *  #on importe les modules GPIO
import subprocess
import os

stat = os.statvfs('cabine photo.py')
espace_min = 2500000  #taille minimum requise en bytes
espace_restant = stat.f_frsize * stat.f_bfree #calculer espace libre en bytes
led = 4
switch = 0

pinMode(led, "OUTPUT")

camera = PiCamera()  #camera est associé à Picamera

camera.resolution = (2592, 1944)  #change la résolution de la photo
camera.start_preview()  #démarre la boucle principale
while True:
  if espace_restant < espace_min:
    print('plus de place')
    break
  for i in range(11):
    camera.annotate_text = "Souhaitez vous conserver cette photo ?"
    camera.annotate_text_size = 50
    switch = not switch
    digitalWrite(led, switch)
    sleep(1)
    print(10-1)
    if i==10:
      print('Souriez !!')
   camera.capture('/home/pi/Desktop/image0.jpg')
   subprocess.call(["gpicview","/home/pi/Desktop/image0.jpg"])
   camera.stop_preview()  #termine la boucle principale
