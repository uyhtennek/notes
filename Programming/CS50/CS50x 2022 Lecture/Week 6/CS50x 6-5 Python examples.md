There are a bunch of fun Python library or online sources to use
here are just a few
___

# `pyttsx3`
* stands for *Python text to speech*
* it can make the computer say some speech

```python
import pyttsx3

engine = pyttsx3.init()
name = input("What's your name? ")
engine.say(f"hello, {name}")
engine.runAndWait()
```
___

# face detection

To detect faces in an image, [source](https://github.com/ageitgey/face_recognition/blob/master/examples/find_faces_in_picture.py)
```python
from PIL import Image
import face_recognition

# Load the jpg file into a numpy array
image = face_recognition.load_image_file("office.jpg")

# Find all the faces in the image using the default HOG-based model.
# This method is fairly accurate, but not as accurate as the CNN model and not GPU accelerated.
# See also: find_faces_in_picture_cnn.py
face_locations = face_recognition.face_locations(image)
print("I found {} face(s) in this photograph.".format(len(face_locations)))

for face_location in face_locations:

    # Print the location of each face in this image
    top, right, bottom, left = face_location

    # You can access the actual face itself like this:
    # This just draw a box
    face_image = image[top:bottom, left:right]
    pil_image = Image.fromarray(face_image)
    pil_image.show()
```

also we can do recognize a specific face in an image, [source](https://github.com/ageitgey/face_recognition/blob/master/examples/identify_and_draw_boxes_on_faces.py)
___

# speech to text

we can have some code to turn speech to text
and some code to analyse the text, to make the computer able to response in a more interesting way

but we'll skip the code -.-
___

# QR code
it just generate a QR code from a youtube video link

```python
import os
import qrcode

img = qrcode.amke("https://youtu.be/xvFZjo5PgG0")

img.save("qr.png", "PNG")

os.system("open qr.png")
```
___
