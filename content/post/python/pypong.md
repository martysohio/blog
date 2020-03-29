+++
title = "Py Pong Starter"
date = "2019-10-31T13:50:46+02:00"
tags = ["random","tech","python","coding"]
categories = ["Python"]

+++

I've been messing around with PyGame and its certainly a trip.  Game logic is much easier than the mechanics of drawing something in the proper order and the proper way. It took way longer than I expected to get a basic version of Pong functional. Needs some final tweaks to beat the 80/20 rule and have something that is truly complete. I keep redoing it as I find something else out about PyGame and game logic in general.  My main source of frustration is fixed resolution and dealing with window size.  Its not clear if PyGame supports swapping resolutions etc. Would be nice if there was some bootstrap method to it so I never had to worry about it.  Testing 1080p resolution on a 4K monitor is weird.

Here is the full terrible code:

```
# import the pygame module, so you can use it
import pygame
import math
from pygame.locals import *
import random

#constants
FPS_LIMIT=120
X_RES=1920
Y_RES=1080
clock = pygame.time.Clock()
WHITE=(255,255,255)
BLUE=(0,0,255)
RED=(255,0,0)
SCORE = [0,0]
class Player(pygame.sprite.Sprite):

    def __init__(self,x,y):
       # Call the parent class (Sprite) constructor
       pygame.sprite.Sprite.__init__(self)
       self.x = x
       self.y = y
       self.height = 250
       self.width = 25
       self.speed = .5
       # Create an image of the block, anda color.
       # This could also be an image loadek.
       self.image = pygame.Surface([self.width,self.height])
       self.image.fill(WHITE)

       # Fetch the rectangle object that hons of the image
       # Update the position of this objeche values of rect.x and rect.y
       self.rect = self.image.get_rect()
       self.rect.x = self.x
       self.rect.y = self.y
       self.last_y_pos = 0
       self.y_speed = 0

    def boundary_check(self):      
        if self.rect.y <= Y_RES*.05 and self.speed < 0:
            self.rect.y = Y_RES*.05

        if self.rect.y >= Y_RES*.95 - self.height and self.speed > 0:
            self.rect.y = Y_RES*.95 - self.height

    def move(self,mousey,timedelta):
        self.last_y_pos = self.rect.y
        self.rect.y = mousey
        self.y_speed = abs(self.rect.y - self.last_y_pos)/timedelta

    def update(self):
        self.boundary_check()


class Computer(pygame.sprite.Sprite):

    def __init__(self,x,y):
       # Call the parent class (Sprite) constructor
       pygame.sprite.Sprite.__init__(self)
       self.x = x
       self.y = y
       self.height = 250
       self.width = 25
       self.speed = .5
       # Create an image of the block, anda color.
       # This could also be an image loadek.
       self.image = pygame.Surface([self.width,self.height])
       self.image.fill(WHITE)

       # Fetch the rectangle object that hons of the image
       # Update the position of this objeche values of rect.x and rect.y
       self.rect = self.image.get_rect()
       self.rect.x = self.x
       self.rect.y = self.y
       self.last_y_pos = 0
       self.y_speed = 0

    def boundary_check(self):      
        if self.rect.y <= Y_RES*.05:
            self.rect.y = Y_RES*.05

        if self.rect.y >= Y_RES*.95 - self.height:
            self.rect.y = Y_RES*.95 - self.height

    def update(self,ball,timedelta):
        self.last_y_pos = self.rect.y
        self.y_speed = abs(self.rect.y - self.last_y_pos)/timedelta
        self.rect.y = ball.rect.y + random.randint(-10,10)
        self.boundary_check()



class Ball(pygame.sprite.Sprite):
    def __init__(self,x,y):
        # Call the parent class (Sprite) constructor
        pygame.sprite.Sprite.__init__(self)
        self.x = x
        self.y = y
        self.height = 25
        self.width = 25

        self.speed = {"x" : -1 ,"y" : 0}
        # Create an image of the block, and fill it with a color.
        # This could also be an image loaded from the disk.
        self.image = pygame.Surface([self.width, self.height])
        self.image.fill(WHITE)
        self.rect = self.image.get_rect()
        self.rect.x = self.x
        self.rect.y = self.y


    def boundary_check(self):      
        if self.rect.y <= 0 and self.speed["y"] < 0:
            self.speed["y"] = -self.speed["y"]  

        if self.rect.y >= Y_RES and self.speed["y"] > 0:
            self.speed["y"] = -self.speed["y"]

        if self.rect.x < 26:
            SCORE[1]+=1
            self.rect.x = self.x
            self.rect.y = self.y
        if self.rect.x > X_RES - 26:
            SCORE[0]+=1
            self.rect.x = self.x
            self.rect.y = self.y


    def player_bounce(self,paddle_speed):
        self.speed["x"] = -self.speed["x"]
        self.speed["y"] = paddle_speed


    def update(self,timedelta):

        self.boundary_check()
        self.rect.x+=int(self.speed["x"]*timedelta)
        self.rect.y+=int(self.speed["y"]*timedelta)


def score_handler(score):
    text = font.render(f"{score[0]} : {score[1]}", True, WHITE)
    return text


if __name__=="__main__":
    # call the main function

    pygame.init()
    #freeserif
    font = pygame.font.SysFont("ubuntumono", 72)


    pygame.display.set_caption("PONG")

    screen = pygame.display.set_mode((X_RES,Y_RES), DOUBLEBUF)


    #initial state


    player = Player(10,Y_RES*.05)
    computer = Computer(X_RES - 35,Y_RES*.05)

    ball = Ball(X_RES/2,Y_RES/2)

    paddles = pygame.sprite.Group()
    balls = pygame.sprite.Group()
    paddles.add(player)
    balls.add(ball)
    paddles.add(computer)

    pygame.display.flip()   
    # define a variable to control the main loop
    running = True
    # main loop
    while running:
        timedelta = clock.tick(FPS_LIMIT)

        screen.fill((0,0,0))

        text = score_handler(SCORE)
        """
        keys = pygame.key.get_pressed()
        if keys[pygame.K_s]:
            player.move(0)
        if keys[pygame.K_w]:
            player.move(1)
        else:
        """

        # event handling, gets all event from the event queue
        for event in pygame.event.get():
            # only do something if the event is of type QUIT
            # if event.type == pygame.KEYDOWN:
            #     surf.fill((0,255,0))
            if event.type == pygame.QUIT:
                running = False
            if event.type == MOUSEMOTION:
                mousex, mousey = event.pos
                player.move(mousey,timedelta)



        player.update()
        computer.update(ball,timedelta)
        ball.update(timedelta)
        for paddle in paddles:
            coll = pygame.sprite.spritecollide(paddle,balls,False)
            if coll:
                ball.player_bounce(paddle.y_speed)
        paddles.draw(screen)
        balls.draw(screen)
        screen.blit(text,[X_RES/2 - 72,0])
       # print(screen.get_rect())
        pygame.display.flip()


```
