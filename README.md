# snake-game-by-py
import pygame
import sys
import random
pygame.init()

screen_width=600
screen_height=400

white=(211,211,211)
black=(0,0,0)
red=(255,0,0)
darkBlue=(25,25,112)
blue=(50,153,213)
yellow=(255,255,0)

dis=pygame.display.set_mode((screen_width,screen_height))
pygame.display.set_caption('Snake Game')
clock=pygame.time.Clock()
snake_block=10
snake_speed=15
font_style=pygame.font.SysFont("bahnschrift",25)

def score(score):
    value=font_style.render("Your Score: "+str(score),True,black)
    dis.blit(value,[0,0])

def our_snake(snake_block,snake_List):
    for x in snake_List:
        pygame.draw.rect(dis,darkBlue,[x[0],x[1],snake_block,snake_block])

def message(msj,color):
    mesg=font_style.render(msj,True,color)
    dis.blit(mesg,[screen_width/6,screen_height/3])

def gameLoop():
    game_over=False
    game_close=False

    x1=screen_width/2
    y1=screen_height/2

    x1_change=0
    y1_change=0

    snake_List=[]
    Length_of_snake=1
    foodx=round(random.randrange(0,screen_width-snake_block)/10.0)*10.0
    foody=round(random.randrange(0,screen_height-snake_block)/10.0)*10.0

    while not game_over:

        while game_close==True:
            dis.fill(yellow)
            message("Game Over!Press Q-Quit or C-Play Again",black)
            pygame.display.update()

            for event in pygame.event.get():
                if event.type==pygame.KEYDOWN:
                    if event.key==pygame.K_q:
                        game_over=True
                        game_close= False
                    if event.key==pygame.K_c:
                        gameLoop()

        for event in pygame.event.get():
            if event.type==pygame.QUIT:
                game_over=True
            elif event.type==pygame.KEYDOWN:
                if event.key==pygame.K_LEFT:
                    x1_change=-snake_block
                    y1_change=0
                elif event.key==pygame.K_RIGHT:
                    x1_change=snake_block
                    y1_change=0
                elif event.key==pygame.K_UP:
                    x1_change=0
                    y1_change=-snake_block
                elif event.key==pygame.K_DOWN:
                    x1_change=0
                    y1_change=snake_block
        if x1 >= screen_width or x1 < 0 or y1>= screen_height or y1 < 0:
            game_close=True
        x1+=x1_change
        y1+=y1_change
        dis.fill(white)

        pygame.draw.rect(dis,red,[foodx,foody,snake_block,snake_block])
        snake_Head=[]
        snake_Head.append(x1)
        snake_Head.append(y1)
        snake_List.append(snake_Head)
        if len(snake_List)>Length_of_snake:
            del snake_List[0]

        for x in snake_Head:
            if x==snake_Head:
                game_close=True

        our_snake(snake_block,snake_List)
        score(Length_of_snake-1)

        pygame.display.update()

        if x1==foodx and y1==foody:
            foodx=round(random.randrange(0,screen_width-snake_block)/10.0)*10.0
            foody=round(random.randrange(0,screen_height-snake_block)/10.0)*10.0
            Length_of_snake+=1

        clock.tick(snake_speed)

    pygame.quit()
    quit()
gameLoop()
