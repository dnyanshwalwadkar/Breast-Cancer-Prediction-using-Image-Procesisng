import cv2
import numpy as np
import csv

# read and scale down image
# wget https://bigsnarf.files.wordpress.com/2017/05/hammer.png #black and white
# wget https://i1.wp.com/images.hgmsites.net/hug/2011-volvo-s60_100323431_h.jpg
#img=cv2.imread(cv2.GaussianBlur('C:\\Users\\Dnyanesh\\PycharmProjects\\BreastCancer\\input\\30.jpg',3,3))
img = cv2.pyrDown(cv2.imread('C:\\Users\\Dnyanesh\\PycharmProjects\\BreastCancer\\input\\11.jpg', cv2.COLOR_BGR2GRAY))

# threshold image
ret, threshed_img = cv2.threshold(cv2.cvtColor(img, cv2.COLOR_BGR2GRAY),
                205, 255, cv2.THRESH_BINARY)
# find contours and get the external one

contours, hier = cv2.findContours(threshed_img, cv2.RETR_TREE, cv2.CHAIN_APPROX_NONE)
print(type(contours))
contours1=[]
#--------------------------------
length= len(contours)
#print("--------------------------")
#print(contours)
i=0
area=[]
paremeter=[]
for c in contours:
    x, y, w, h = cv2.boundingRect(c)
    area.append(w*h)
    #paremeter.append(2*w+2*h)

print(area)
max_area = max(area)
#max_par=max(paremeter)
print("Max area----------------------------------")
print(max_area)
print("thresh area--------------------------------------")
thresh_area=max_area*0.6

print(thresh_area)
#print("paremeter")
#print()



for c in contours:

    cnt=contours[i]
    x, y, w, h = cv2.boundingRect(c)
    if (w*h)>thresh_area:
        contours1.append(contours[i])
        paremeter.append(2*w+2*h)
    i = i + 1
print("Paremeter ---------------------------------------")
#print(contours1)
print(paremeter)
#cnt = contours[310]
#M = cv2.moments(cnt)
#print(cv2.contourArea(cnt))

#print(M)

#print(len(contours))

#image, contours, hier = cv2.findContours(threshed_img, cv2.RETR_TREE,
#                cv2.CHAIN_APPROX_SIMPLE)

# with each contour, draw boundingRect in green
# a minAreaRect in red and
# a minEnclosingCircle in blue
print("Areas of selected contour")
area1=[]
for c in contours1:
    # get the bounding rect
    x, y, w, h = cv2.boundingRect(c)
    print(w*h)
    area1.append(w*h)
    i=i+1
    #print("----area---")
    #print(i)
    # draw a green rectangle to visualize the bounding rect
    cv2.rectangle(img, (x, y), (x+w, y+h), (0, 255, 0), 2)

    # get the min area rect
    rect = cv2.minAreaRect(c)
    box = cv2.boxPoints(rect)
    # convert all coordinates floating point values to int
    box = np.int0(box)
    # draw a red 'nghien' rectangle
    cv2.drawContours(img, [box], 0, (0, 0, 255))

    # finally, get the min enclosing circle
    (x, y), radius = cv2.minEnclosingCircle(c)
    # convert all values to int
    center = (int(x), int(y))
    radius = int(radius)
    #cnt = contours
#
    #print(cv2.contourArea(cnt))
    # and draw the circle in blue
    #img = cv2.circle(img, center, radius, (255, 0, 0), 2)

#print(len(contours1))
cv2.drawContours(img, contours1, -1, (255, 255, 0), 1)

cv2.imshow("contours", img)

#cv2.imshow("contours", img)
t=len(paremeter)
data=[]
for i in range(t):
    temp=[]
    temp.append(paremeter[i])
    temp.append(area1[i])
    data.append(temp)
#data=[paremeter,area1]
i=0
# for p,a in paremeter,area:
#     i=i+1
#     data.append(i,p,a)

print("=-------data----------=")
print(data)
with open('C:\\Users\\Dnyanesh\\PycharmProjects\\BreastCancer\\office0.csv', 'w', newline='') as file:
    writer = csv.writer(file)
    writer.writerows(data)



while True:
    key = cv2.waitKey(1)
    if key == 27: #ESC key to break
        break


cv2.destroyAllWindows()