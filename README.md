# 123
1. Download
[Pykinect2](https://github.com/Kinect/PyKinect2)

2. Download
[Pykinect2SandMount](https://github.com/twwspes/pykinect2SandMount)

3. After installations, connect your computer with KinectV2

#Important codes are posted below

```Python
#From Python
import pygame
from pykinect2 import PyKinectV2
from pykinect2.PyKinectV2 import *
from pykinect2 import PyKinectRuntime
import time

import ctypes
import _ctypes
import pygame
import sys

import random
import math
import datetime

#Button class
class Button:
    def __init__(self, x, y, sx, sy, bborder,bcolour, fbcolour, font, fontsize, fcolour, text):
        self.x = x
        self.y = y
        self.sx = sx
        self.sy = sy
        self.bborder = bborder
        self.bcolour = bcolour
        self.fbcolour = fbcolour
        self.fcolour = fcolour
        self.fontsize = fontsize
        self.text = text
        self.current = False
        self.buttonf = pygame.font.SysFont(font, fontsize)
            
#a Method that recognizes the user's mouse click 
    def focusCheck(self, mousepos, mouseclick):
        if(mousepos[0] >= self.x and mousepos[0] <= self.x + self.sx and mousepos[1] >= self.y and mousepos[1] <= self.y + self.sy):
            self.current = True
            return mouseclick[0]
        else:
            self.current = False
            return False
 
 #an Indispensable class to operate this game
 class PyKinectCollect(object):
     def __init__(self, title, width = 1400, height=800, fill=YELLOW):
         self._clock = pygame.time.Clock()

         #Set the screen size; width, height
         self._infoObject = pygame.display.Info()
         self._screen = pygame.display.set_mode((self._infoObject.current_w >> 1, self._infoObject.current_h >> 1), 
                                               pygame.HWSURFACE|pygame.DOUBLEBUF|pygame.RESIZABLE)                                          
         #Unless the user clicks the close button
         self._done = False

         #Kinect runtime object 
         self._kinect = PyKinectRuntime.PyKinectRuntime(PyKinectV2.FrameSourceTypes_Color | PyKinectV2.FrameSourceTypes_Body)

         #a Space(surface) for getting Kinect color frames, 32bit color, width and height 
         self._frame_surface = pygame.Surface((self._kinect.color_frame_desc.Width, self._kinect.color_frame_desc.Height), 0, 32)

         #a Space for skeleton data 
         self._bodies = None

         self.current = False
         self.title = title
         self.width = width
         self.height = height
         self.fill = fill        

#a Method that realizes the user's body on the screen and contains the principle to operate 
     def draw_body_bone(self, joints, jointPoints, color, joint0, joint1, boardN):
         joint0State = joints[joint0].TrackingState;
         joint1State = joints[joint1].TrackingState;

         #If the user's joints are not tracked perfectly
         if (joint0State == PyKinectV2.TrackingState_NotTracked) or (joint1State == PyKinectV2.TrackingState_NotTracked):
             return

         #If the user's joints are not tracked at all
         if (joint0State == PyKinectV2.TrackingState_Inferred) and (joint1State == PyKinectV2.TrackingState_Inferred):
             return

         start = (0, 0)
         end = (0, 0)
         global starttime
         global flag
         
         #The case for recognizing the user's head
         if (boardN == 1):
            
            #Define coordinates x, y for the user's head recognition
            JointX = jointPoints[PyKinectV2.JointType_Head].x
            JointY = jointPoints[PyKinectV2.JointType_Head].y
            
            #If the user's head is within this scope
            if (1400 <= JointX) and (1000 >= JointY):
                
                #First time
                if (starttime == 0):
                    timestamp = datetime.datetime.now().timestamp()
                    fts = timestamp
                    starttime = fts
                    print('time start!!!!!!!!!!!!!!!!!!!!!!!!!')
                    pygame.display.update()
                    print (starttime)
                else:
                    timestamp = datetime.datetime.now().timestamp()
                    fts2 = timestamp
                    print('right', fts2)
                    if (starttime+2 < fts2):
                        print('touch1')
                        flag = 0
                        
            elif (700 >= JointX) and (1000 >= JointY):
                if (starttime == 0):
                    timestamp = datetime.datetime.now().timestamp()
                    fts = timestamp
                    starttime = fts
                    print('time start!!!!!!!!!!!!!!!!!!!!!!!!!')
                    pygame.display.update()
                    print (starttime)
                else:
                    timestamp = datetime.datetime.now().timestamp()
                    fts2 = timestamp
                    print('left', fts2)
                    if (starttime+2 < fts2):
                        print('touch1')
                        flag = 2
            else:
                starttime = 0

        elif (boardN == 2):
            JointX = jointPoints[PyKinectV2.JointType_Head].x
            JointY = jointPoints[PyKinectV2.JointType_Head].y
         
            if (700 >= JointX) and (50 <= JointY) and (250 >= JointY):            
                print('touch2 left')
                flag = 2

            elif (1400 <= JointX) and (50 <= JointY) and (250 >= JointY):
                print('touch2 right')
                flag = 0

        #The case for recognizing the user's hands
        elif (boardN == 3):
            #Define coordinates x, y for the user's hands recognition
            JointXR = jointPoints[PyKinectV2.JointType_HandRight].x
            JointYR = jointPoints[PyKinectV2.JointType_HandRight].y
            JointXL = jointPoints[PyKinectV2.JointType_HandLeft].x
            JointYL = jointPoints[PyKinectV2.JointType_HandLeft].y
            
            if (1400 <= JointXR) and (1000 >= JointYR):
                if (starttime == 0):
                    timestamp = datetime.datetime.now().timestamp()
                    fts = timestamp
                    starttime = fts
                    print('time start!!!!!!!!!!!!!!!!!!!!!!!!!')
                    pygame.display.update()
                    print (starttime)
                else:
                    timestamp = datetime.datetime.now().timestamp()
                    fts2 = timestamp
                    print('HandRight', fts2)
                    if (starttime+2 < fts2):
                        print('touch3')
                        flag = 0
                             
            elif(700 >= JointXL) and (1000 >= JointYL):
                if (starttime == 0):
                    timestamp = datetime.datetime.now().timestamp()
                    fts = timestamp
                    starttime = fts
                    print('time start!!!!!!!!!!!!!!!!!!!!!!!!!')
                    pygame.display.update()
                    print (starttime)
                else:
                    timestamp = datetime.datetime.now().timestamp()
                    fts2 = timestamp
                    print('HandLeft', fts2)
                    if (starttime+2 < fts2):
                        print('touch3')
                        flag = 2
            else:
                starttime = 0

        #The case for recognizing the user's knees
        elif (boardN == 5):
            #Define coordinates x, y for the user's knees recognition
            JointXR = jointPoints[PyKinectV2.JointType_KneeRight].x
            JointYR = jointPoints[PyKinectV2.JointType_KneeRight].y
            JointXL = jointPoints[PyKinectV2.JointType_KneeLeft].x
            JointYL = jointPoints[PyKinectV2.JointType_KneeLeft].y
            
            if (1200 <= JointXR) and (1000 <= JointYR):    
                if (starttime == 0):
                    timestamp = datetime.datetime.now().timestamp()
                    fts = timestamp
                    starttime = fts
                    print('time start!!!!!!!!!!!!!!!!!!!!!!!!!')
                    pygame.display.update()
                    print (starttime)
                else:
                    timestamp = datetime.datetime.now().timestamp()
                    fts2 = timestamp
                    print('KneeRight', fts2)
                    if (starttime+1 < fts2):
                        print('touch3')
                        flag = 0
                                  
            elif(850 >= JointXL) and (1000 <= JointYL) and (1200 >= JointYL):
                if (starttime == 0):
                    timestamp = datetime.datetime.now().timestamp()
                    fts = timestamp
                    starttime = fts
                    print('time start!!!!!!!!!!!!!!!!!!!!!!!!!')
                    pygame.display.update()
                    print (starttime)
                else:
                    timestamp = datetime.datetime.now().timestamp()
                    fts2 = timestamp
                    print('KneeLeft', fts2)
                    if (starttime+1 < fts2):
                        print('touch3')
                        flag = 2
            else:
                starttime = 0

        #The case for recognizing the user's elbows
        elif (boardN == 9):
            #Define coordinates x, y for the user's elbows recognition
            JointXR = jointPoints[PyKinectV2.JointType_ElbowRight].x
            JointYR = jointPoints[PyKinectV2.JointType_ElbowRight].y
            JointXL = jointPoints[PyKinectV2.JointType_ElbowLeft].x
            JointYL = jointPoints[PyKinectV2.JointType_ElbowLeft].y
            
            if (1500 <= JointXR):
                if (starttime == 0):
                    timestamp = datetime.datetime.now().timestamp()
                    fts = timestamp
                    starttime = fts
                    print('time start!!!!!!!!!!!!!!!!!!!!!!!!!')
                    pygame.display.update()
                    print (starttime)      
                else:
                    timestamp = datetime.datetime.now().timestamp()
                    fts2 = timestamp
                    print('ElbowRight', fts2)
                    if (starttime+1 < fts2):
                        print('touch3')
                        flag = 0
   
            elif(600 >= JointXL):
                if (starttime == 0):
                    timestamp = datetime.datetime.now().timestamp()
                    fts = timestamp
                    starttime = fts
                    print('time start!!!!!!!!!!!!!!!!!!!!!!!!!')
                    pygame.display.update()
                    print (starttime)
                else:
                    timestamp = datetime.datetime.now().timestamp()
                    fts2 = timestamp
                    print('ElbowLeft', fts2)
                    if (starttime+1 < fts2):
                        print('touch3')
                        flag = 2
            else:
                starttime = 0

#a Method that enables the user to play this game
     def playGame(self,boardN):
        global flag
        flag = 1
        
        #Main loop
        while not self._done:
            
            #If the user did something
            for event in pygame.event.get():
            
                #If the user clicks close
                if event.type == pygame.QUIT:
                
                    #a Flag that enables the user to get out of this loop
                    self._done = True # Flag that we are done so we exit this loo
                
                #The size of window changes
                elif event.type == pygame.VIDEORESIZE: 
                    self._screen = pygame.display.set_mode(event.dict['size'], 
                                               pygame.HWSURFACE|pygame.DOUBLEBUF|pygame.RESIZABLE)
                    
            #To get frames and do drawing  
            #It fills back buffer surface with frame data 
            if self._kinect.has_new_color_frame():
                frame = self._kinect.get_last_color_frame()
                self.draw_color_frame(frame, self._frame_surface)
                frame = None

            #To get the user's skeletons
            if self._kinect.has_new_body_frame(): 
                self._bodies = self._kinect.get_last_body_frame()

            #It draws the user's skeletons to _frame_surface
            if self._bodies is not None: 
                for i in range(0, self._kinect.max_body_count):
                    body = self._bodies.bodies[i]
                    if body.is_tracked and flag==1:
                        joints = body.joints
                        
                        #Convert the user's joint coordinates to color space 
                        joint_points = self._kinect.body_joints_to_color_space(joints)
                        self.draw_body(joints, joint_points, SKELETON_COLORS[i],boardN)
                    elif (flag == 0) or (flag ==2):
                        break

            if (flag == 0) or (flag == 2):
                break

            #It copies back buffer surface pixels to the screen and resizes it 
            #Screen size depends on the Kinect's color frame size
            h_to_w = float(self._frame_surface.get_height()) / self._frame_surface.get_width()
            target_height = int(h_to_w * self._screen.get_width())
            surface_to_draw = pygame.transform.scale(self._frame_surface, (self._screen.get_width(), target_height));
            self._screen.blit(surface_to_draw, (0,0))
            surface_to_draw = None
            
            #Problem images
            titleImg = str(boardN) +"-3.png"
            
            #One of two option(선택 문항) images
            OptionImg1 = str(boardN)+ "-1.png"
            
            #One of two option(선택 문항) images
            OptionImg2 = str(boardN )+"-2.png"
            
            #It loads image and adjusts it as the set scale
            img = pygame.image.load(OptionImg1)
            img = pygame.transform.scale(img, (230, 180))
            
            img2 = pygame.image.load(OptionImg2)
            img2 = pygame.transform.scale(img2, (230, 180))
           
            img3 = pygame.image.load(titleImg)
            img3 = pygame.transform.scale(img3, (1000, 140))
            
            #It shows images on the screen
            screen.blit(img, (250, 170))
            screen.blit(img2, (1050, 170))
            screen.blit(img3, (270, 10))

            pygame.display.update()
            
            #The screen changes
            pygame.display.flip()

            #Limitation of 60 frames per second
            self._clock.tick(60)

        #It notifies the user know he/she is correct or wrong
        if flag == 0:
            global buttonlist
            buttonlist[boardN-1]=1         
            show_correct_img()
            print("0")           
        elif flag == 2:
            show_incorrect_img()
            print('2')
            
        pygame.time.delay(3000)

#The function that enables this game to work
def w_game():
    pygame.font.init()

    game = PyKinectCollect("Bingo Game for Etiquette")
    b_screen = Screen("Bingo_Screen")
    win = b_screen.makeCurrent()
    done = False

    #It draws buttons
    returnButton = Button(1000, 650, 300, 100,3, colours["Black"], colours["Cyan"], "arial", 20, colours["Black"], "RETURN")
    bingo_1 = Button(800,200,150,150,3, colours["White"], colours["Cyan"],"arial", 20, colours["Black"], "1")
    bingo_2 = Button(950,200,150,150,3, colours["White"], colours["Cyan"],"arial", 20, colours["Black"], "2")
    bingo_3 = Button(1100,200,150,150,3, colours["White"], colours["Cyan"],"arial", 20, colours["Black"], "3")
    bingo_4 = Button(800,350,150,150,3, colours["White"], colours["Cyan"],"arial", 20, colours["Black"], "4")
    bingo_5 = Button(950,350,150,150,3, colours["White"], colours["Cyan"],"arial", 20, colours["Black"], "5")
    bingo_6 = Button(1100,350,150,150,3, colours["White"], colours["Cyan"],"arial", 20, colours["Black"], "6")
    bingo_7 = Button(800,500,150,150,3, colours["White"], colours["Cyan"],"arial", 20, colours["Black"], "7")
    bingo_8 = Button(950,500,150,150,3, colours["White"], colours["Cyan"],"arial", 20, colours["Black"], "8")
    bingo_9 = Button(1100,500,150,150,3, colours["White"], colours["Cyan"],"arial", 20, colours["Black"], "9")

    toggle = False
    while not done:
        b_screen.screenUpdate()
        b_screen.show_middle_img()
        b_screen.show_left_img()
        game.screenUpdate()
        mouse_pos = pygame.mouse.get_pos()
        mouse_click = pygame.mouse.get_pressed()
        keys = pygame.key.get_pressed()
        
    #We gave the page number according to its coordinates based on its row and column's number of a 3 by 3 bingo table
    #b_1(row: 1, column: 1)
        if b_screen.checkUpdate():
            screen2button = bingo_1.focusCheck(mouse_pos, mouse_click)
            bingo_1.showButton(b_screen.returnTitle(),buttonlist[0])
            if screen2button:
                win = game.makeCurrent()
                g_screen=game.playGame(1)
                b_screen.endCurrent()

        elif game.checkUpdate():
            b_screen.show_return_img()
            returnm = returnButton.focusCheck(mouse_pos, mouse_click)
            returnButton.showButton(game.returnTitle(),0)
            
            if returnm:
                win = b_screen.makeCurrent()
                game.endCurrent()
                
    #b_2(row: 1, column: 2)  
        if b_screen.checkUpdate():
            screen2button = bingo_2.focusCheck(mouse_pos, mouse_click)
            bingo_2.showButton(b_screen.returnTitle(),buttonlist[1])
            if screen2button:
                win = game.makeCurrent()
                g_screen=game.playGame(2)
                b_screen.endCurrent()

    #b_3(row: 1, column: 3)  
        if b_screen.checkUpdate():
            screen2button = bingo_3.focusCheck(mouse_pos, mouse_click)
            bingo_3.showButton(b_screen.returnTitle(),buttonlist[2])
            if screen2button:
                win = game.makeCurrent()
                g_screen=game.playGame(3)
                b_screen.endCurrent()
                
    #b_4(row: 2, column: 1)   
        if b_screen.checkUpdate():
            screen2button = bingo_4.focusCheck(mouse_pos, mouse_click)
            bingo_4.showButton(b_screen.returnTitle(),buttonlist[3])
            if screen2button:
                win = game.makeCurrent()
                g_screen=game.playGame(4)
                b_screen.endCurrent()
                
    #b_5(row: 2, column: 2)   
        if b_screen.checkUpdate():
            screen2button = bingo_5.focusCheck(mouse_pos, mouse_click)
            bingo_5.showButton(b_screen.returnTitle(),buttonlist[4])
            if screen2button:
                win = game.makeCurrent()
                g_screen=game.playGame(5)
                b_screen.endCurrent()

    #b_6(row: 2, column: 3)  
        if b_screen.checkUpdate():
            screen2button = bingo_6.focusCheck(mouse_pos, mouse_click)
            bingo_6.showButton(b_screen.returnTitle(),buttonlist[5])
            if screen2button:
                win = game.makeCurrent()
                g_screen=game.playGame(6)
                b_screen.endCurrent()

    #b_7(row: 3, column: 1)   
        if b_screen.checkUpdate():
            screen2button = bingo_7.focusCheck(mouse_pos, mouse_click)
            bingo_7.showButton(b_screen.returnTitle(),buttonlist[6])
            if screen2button:
                win = game.makeCurrent()
                g_screen=game.playGame(7)
                b_screen.endCurrent()

    #b_8(row: 3, column: 2)   
        if b_screen.checkUpdate():
            screen2button = bingo_8.focusCheck(mouse_pos, mouse_click)
            bingo_8.showButton(b_screen.returnTitle(),buttonlist[7])
            if screen2button:
                win = game.makeCurrent()
                g_screen=game.playGame(8)
                b_screen.endCurrent()

    #b_9(row: 3, column: 3)   
        if b_screen.checkUpdate():
            screen2button = bingo_9.focusCheck(mouse_pos, mouse_click)
            bingo_9.showButton(b_screen.returnTitle(),buttonlist[8])
            if screen2button:
                win = game.makeCurrent()
                g_screen=game.playGame(9)
                b_screen.endCurrent()

        for event in pygame.event.get():
            if(event.type == pygame.QUIT):
                done = True
        for event in pygame.event.get():
            if(event.type == pygame.QUIT):
                done = True
                
        pygame.display.update()
        
    pygame.quit()
```
