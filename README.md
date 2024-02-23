import pygame

pygame.init()
screen = pygame.display.set_mode((800,800))

pygame.display.set_caption('sidescrolling game')
clock=pygame.time.Clock()

img = pygame.image.load("forestt.png").convert() #loads image, goes above globals
running = True
player = [350,650,0,0]
isOnGround = False
offset = 0
platforms = [(400, 600), (600, 500), (800, 400), (1000, 500), (1200, 600)]


 
def move_player():
    global isOnGround #modify global variable from in function
    global offset
    isOnGround = True
    #platform collision
    for i in range(len(platforms)):
        if player[0]+50>platforms[i][0]+offset and player[0]<platforms[i][0]+100+offset and player[1]+50>platforms[i][1] and player[1]+50< platforms[i][1]+50:
            isOnGround = False
            player[1] = platforms[i][1]-50
            player[3] = 0
            print("on platform")
    
    #left
    if keys[pygame.K_LEFT]:
        if offset > 260 and player[0]>=0:
            print("flag1")
            player[2] =- 5
        elif player[0]>400:
            print("flag2")
            player[2] = -5
        elif player [0]>0:
            print("flag3")
            offset += 5
            player[2] = 0
        else:
            print("flag4")
            player[2] = 0
            
    elif keys[pygame.K_RIGHT]: # right key (calling the key that causes you to go right.)
        if offset < -1500 and player[0]<750:
            player[2] = 5
        elif offset>260 and player[0] < 400: #offset + Player
            player[2] = 5
        elif player [0]<750:
            offset -= 5
            player[2] = 0
        else:
            player[2] = 0
    else:
        player[2]=0
        
    if isOnGround == True:
        player[3] += 1 #gravity
        
    if isOnGround == True and keys[pygame.K_UP]:
        player[3] = -15
        isOnGround = False
        
    player[0]+=player[2]
    player[1]+=player[3]
    
    if player[1] > 650:
        player[1] = 650
        isOnGround = False
        
def draw_platforms():
    for i in range(len(platforms)):
        pygame.draw.rect(screen, (150, 10, 10), (platforms[i][0] + offset, platforms[i][1], 100, 30))
def draw_clouds():
    #draw cloud(s)
    for x in range(100,3000,300):
        for i in range(3):
            pygame.draw.circle(screen, (255,255,255), (x + offset / 2, 100), 40)
            pygame.draw.circle(screen, (255,255,255), (x-50 + offset / 2, 125), 40)
            pygame.draw.circle(screen, (255,255,255), (x+50 + offset / 2, 125), 40)
        pygame.draw.rect(screen, (255,255,255), (x-50 + offset / 2, 100, 100, 65))
        
def draw_bushandstick():
    for x in range(100,2000,300):
        pygame.draw.circle(screen, (16, 92, 18), (x-43 + offset, 664), 30)
        pygame.draw.circle(screen, (16, 92, 18), (x-10 + offset, 700), 40)
        pygame.draw.circle(screen, (16, 92, 18), (x-50 + offset, 700), 40)
        pygame.draw.rect(screen, (128, 100, 55), (x-10 + offset, 700, 50, 75))
        
while running: # Game loop 1
    clock.tick(60)
    #input
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            running = False
            
    keys = pygame.key.get_pressed()
    
    #physics
    
    move_player()
    # render
    
    screen.fill((135, 206, 235))
    screen.blit(img, (-900 + offset / 4, 0)) #draws to screen, goes in render section
    draw_platforms()
    draw_clouds()
    draw_bushandstick()
    print(offset)
    pygame.draw.rect(screen, (62,97,45), (0, 700, 1000, 200))
    pygame.draw.rect(screen, (0,0,0), (player[0], player[1], 50,50))
    pygame.display.flip()
    
pygame.quit()
