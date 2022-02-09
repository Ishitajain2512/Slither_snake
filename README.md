# Slither_snake
This is a snake game developed using python. It is the edited version of the previous code I uploaded in the repository :- "Slither_Snake_game"

import pygame
import random

pygame.init()

bg = (255, 255, 255)
h = (255, 150, 20)
outer = (205, 91, 69)
fo = (0, 200, 255)
sc = (0, 0, 0)
red = (213, 50, 80)
body = (135, 206, 250)

dis_width = 600
dis_hwidth = 300
dis_height = 400
dis_hheight = 200
dis = pygame.display.set_mode((dis_width, dis_height))
pygame.display.set_caption('Slither Snake by Ishita')
clock = pygame.time.Clock()
snake_block = 10
snake_speed = 7
font_style = pygame.font.SysFont("bahnschrift", 25)
score_font = pygame.font.SysFont("freesans", 20)


def thescore(score):
    value = score_font.render("Score: " + str(score), True, sc)
    dis.blit(value, [0, 0])


def our_snake(snake_block, snake_list):
    pygame.draw.rect(dis, outer, [int(snake_list[-1][0]), int(snake_list[-1][1]), snake_block, snake_block])
    pygame.draw.rect(dis, h, [int(snake_list[-1][0]), int(snake_list[-1][1]), snake_block - 2, snake_block - 2])
    for x in snake_list[0:-1]:
        pygame.draw.rect(dis, outer, [int(x[0]), int(x[1]), snake_block, snake_block])
        pygame.draw.rect(dis, body, [int(x[0]), int(x[1]), snake_block - 2, snake_block - 2])


def message(msg, color):
    mesg = font_style.render(msg, True, color)
    dis.blit(mesg, [int(dis_width / 4), int(dis_height / 2.5)])


def gameLoop():
    game_over = False
    game_close = False
    x1 = dis_width / 2
    y1 = dis_height / 2
    x1_change = 0
    y1_change = 0
    snake_List = []
    Length_of_snake = 1
    foodx = snake_block * random.randint(0, (dis_width / snake_block) - 1)
    foody = snake_block * random.randint(0, (dis_height / snake_block) - 1)
    while (game_over == False):
        while game_close == True:
            dis.fill(bg)
            message("Press C to continue, Q to quit", red)
            thescore(Length_of_snake - 1)
            pygame.display.update()
            for event in pygame.event.get():
                if event.type == pygame.QUIT:
                    game_over = True
                    game_close = False
                if event.type == pygame.KEYDOWN:
                    if event.key == pygame.K_q:
                        game_over = True
                        game_close = False
                    if event.key == pygame.K_c:
                        gameLoop()
        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                game_over = True
            if event.type == pygame.KEYDOWN:
                if event.key == pygame.K_LEFT:
                    x1_change = -snake_block
                    y1_change = 0
                elif event.key == pygame.K_RIGHT:
                    x1_change = snake_block
                    y1_change = 0
                elif event.key == pygame.K_UP:
                    y1_change = -snake_block
                    x1_change = 0
                elif event.key == pygame.K_DOWN:
                    y1_change = snake_block
                    x1_change = 0
        if x1 >= dis_width or x1 < 0 or y1 >= dis_height or y1 < 0:
            game_close = True
        x1 += x1_change
        y1 += y1_change
        dis.fill(bg)
        pygame.draw.rect(dis, red, [foodx, foody, snake_block, snake_block])
        snake_Head = []
        snake_Head.append(x1)
        snake_Head.append(y1)
        snake_List.append(snake_Head)
        if len(snake_List) > Length_of_snake:
            del snake_List[0]
        for x in snake_List[:-1]:
            if x == snake_Head:
                game_close = True
        our_snake(snake_block, snake_List)
        thescore(Length_of_snake - 1)
        pygame.display.update()
        if x1 == foodx and y1 == foody:
            foodx = snake_block * random.randint(0, (dis_width / snake_block) - 1)
            foody = snake_block * random.randint(0, (dis_height / snake_block) - 1)
            Length_of_snake += 1
        clock.tick(snake_speed)
    pygame.quit()


gameLoop()
