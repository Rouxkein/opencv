import cv2
import numpy as np

drawing = False # true if mouse is pressed
mode = True # if True, draw rectangle. Press 'm' to toggle to curve
img = np.zeros((512,512,3), np.uint8)
ix,iy=-9,-9
cv2.namedWindow('image')
def nothing(x):
    pass
cv2.createTrackbar('G','image',0,255,nothing)
cv2.createTrackbar('B','image',0,255,nothing)
cv2.createTrackbar('R','image',0,255,nothing)
cv2.createTrackbar('size','image',-1,8,nothing)
# mouse callback function

def draw_circle(event,x,y,flags,param):
    global ix,iy,drawing,mode,r,g,b

    if event == cv2.EVENT_LBUTTONDOWN:
        drawing = True
        ix,iy = x,y

    elif event == cv2.EVENT_MOUSEMOVE:
        if drawing == True:
            if mode == True:
                cv2.rectangle(img,(ix,iy),(x,y),(r,g,b),-1)
            else:
                cv2.circle(img,(x,y),s,(r,g,b),-1)

    elif event == cv2.EVENT_LBUTTONUP:
        drawing = False
        if mode == True:
            cv2.rectangle(img,(ix,iy),(x,y),(r,g,b),-1)
        else:
            cv2.circle(img,(x,y),s,(r,g,b),-1)

cv2.setMouseCallback('image',draw_circle)

while(1):
    cv2.imshow('image',img)
    k = cv2.waitKey(1) & 0xFF
    if k == ord('m'):
        mode = not mode
    elif k == 27:
        break
    r = cv2.getTrackbarPos('R','image')
    g = cv2.getTrackbarPos('G','image')
    b = cv2.getTrackbarPos('B','image')
    s = cv2.getTrackbarPos('size','image')
cv2.destroyAllWindows()

