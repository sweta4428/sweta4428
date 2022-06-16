- 👋 Hi, I’m @sweta4428
- 👀 I’m interested in ...
- 🌱 I’m currently learning ...
- 💞️ I’m looking to collaborate on ...
- 📫 How to reach me ...

<!---
sweta4428/sweta4428 is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->
print('before import')
from random import randint
import pygame


def display_score():
    current_time = int(pygame.time.get_ticks()/1000) - start_time
    score_surface = text_font.render(f'Time : {current_time}',False,(64,64,64))
    score_rect = score_surface.get_rect(center =(400,50))
    sc.blit(score_surface,score_rect)
    return current_time


pygame.init()
#sc = pygame.display.set_mode((800,400))
sc= pygame.display.set_mode((100,100))
pygame.display.set_caption('Runner')
clock = pygame.time.Clock()
text_font= pygame.font.Font(None,50)


def obatacle_movement(obstacle_list):
    if obstacle_rect_list:
        for obstacle_rect in obstacle_list:
            obstacle_rect.x -= 4 
            if obstacle_rect.bottom ==285:  
                sc.blit(bomb_surface,obstacle_rect)
            else:
                 sc.blit(itachi_surface,obstacle_rect)
            obstacle_list= [obstacle for obstacle in obstacle_list if obstacle.x> -100]
        return obstacle_list
    else :
        return[]

player_gravity =0
game_active =True
start_time =0
score=0

sky_surface = pygame.image.load('sky.jpg').convert()
ground_surface =pygame.image.load('ground.png').convert()

# text_surface =text_font.render('My Game',False,'Black')
# bomb_surface =pygame.image.load()

#obstacle
bomb_surface =pygame.image.load('bomb.png').convert_alpha()
itachi_surface =pygame.image.load('itachi.png').convert_alpha()
# bomb_rect = bomb_surface.get_rect(bottomright=(600,285))

obstacle_rect_list =[]

#player
player_surface =pygame.image.load('char.png').convert_alpha()
player_rect =player_surface.get_rect(midbottom=(80,285))
player_stand =pygame.image.load('stand.png').convert_alpha()
player_stand =pygame.transform.rotozoom(player_stand,0,2)
player_stand_rect = player_stand.get_rect(center =(400,200))

#msg
game_name = text_font.render('Pixel Runner',False,'black')
game_name_rect = game_name.get_rect(center = (400,80))

game_msg =text_font.render('Press Spacebar to run',False,'black')
game_msg_rect =game_msg.get_rect(center=(400,320))

score_msg =text_font.render(f'Your Score :{score}',False,'black')
score_msg_rect = score_msg.get_rect(center=(400,320))

#Timer
obstacle_timer =pygame.USEREVENT +1
pygame.time.set_timer(obstacle_timer,1500)

print('outside while')

while True:
    print('inside while')
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            pygame.quit()
            exit()
        if game_active:
            if event.type == pygame.MOUSEBUTTONDOWN:
                if player_rect.collidepoint(event.pos) and player_rect.bottom >=280:
                    player_gravity = -22  

            if event.type == pygame.KEYDOWN:
                if event.key == pygame.K_SPACE and player_rect.bottom >=280:
                    player_gravity = -22
        else:
            if event.type == pygame.KEYDOWN and event.key == pygame.K_SPACE: 
                game_active =True
                # bomb_rect.left= 800
                start_time=int(pygame.time.get_ticks()/1000)

        if event.type == obstacle_timer and game_active:
            if randint(0,2):
                obstacle_rect_list.append(bomb_surface.get_rect(bottomright = (randint(900,1110),285)))
            else:
                obstacle_rect_list.append(itachi_surface.get_rect(bottomright = (randint(900,1110),200))) 


    if game_active:
        print('inside if')
        sc.blit(sky_surface,(0,0))
        sc.blit(ground_surface,(0,280))
        # sc.blit(text_surface,(310,50))
        score = display_score()
        #bomb
        # bomb_rect.x -=3
        # if bomb_rect.right<=0 : bomb_rect.left= 800
        # sc.blit(bomb_surface,bomb_rect)
        # player
        player_gravity += 1
        player_rect.y += player_gravity
        if player_rect.bottom >=280 : player_rect.bottom =280
        sc.blit(player_surface,player_rect)

        #obstacle movement
        obstacle_rect_list=obatacle_movement(obstacle_rect_list)
