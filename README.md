# Metal-slug
import pygame
import sys

# Inicializar o Pygame
pygame.init()

# Configurações da janela
WIDTH, HEIGHT = 800, 400
WIN = pygame.display.set_mode((WIDTH, HEIGHT))
pygame.display.set_caption("Mini Metal Slug Style Game")

# Cores
WHITE = (255, 255, 255)
BLACK = (0, 0, 0)

# FPS
FPS = 60
clock = pygame.time.Clock()

# Jogador
player_width, player_height = 40, 60
player_x, player_y = 50, HEIGHT - player_height - 20
player_vel = 5
is_jumping = False
jump_count = 10

# Projéteis
bullets = []
bullet_speed = 10

def draw_window():
    WIN.fill(WHITE)
    pygame.draw.rect(WIN, BLACK, (player_x, player_y, player_width, player_height))
    for bullet in bullets:
        pygame.draw.rect(WIN, (255, 0, 0), bullet)
    pygame.display.update()

# Loop principal
run = True
while run:
    clock.tick(FPS)

    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            run = False

    # Movimentação do jogador
    keys = pygame.key.get_pressed()
    if keys[pygame.K_LEFT] and player_x - player_vel > 0:
        player_x -= player_vel
    if keys[pygame.K_RIGHT] and player_x + player_width + player_vel < WIDTH:
        player_x += player_vel
    if not is_jumping:
        if keys[pygame.K_SPACE]:
            is_jumping = True
    else:
        if jump_count >= -10:
            neg = 1 if jump_count > 0 else -1
            player_y -= (jump_count ** 2) * 0.3 * neg
            jump_count -= 1
        else:
            is_jumping = False
            jump_count = 10

    # Atirar
    if keys[pygame.K_z]:
        if len(bullets) < 5:
            bullet = pygame.Rect(player_x + player_width, player_y + player_height // 2, 10, 5)
            bullets.append(bullet)

    # Atualizar projéteis
    for bullet in bullets[:]:
        bullet.x += bullet_speed
        if bullet.x > WIDTH:
            bullets.remove(bullet)

    draw_window()

pygame.quit()
sys.exit()
