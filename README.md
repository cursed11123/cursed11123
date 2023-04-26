import sys

import pygame

# ініціалізація Pygame
pygame.init()

# кольори
WHITE = (255, 255, 255)
BLACK = (0, 0, 0)

# розміри екрану
SCREEN_WIDTH = 800
SCREEN_HEIGHT = 600

# створення екрану
screen = pygame.display.set_mode((SCREEN_WIDTH, SCREEN_HEIGHT))

# створення гравців
player1 = pygame.Rect(50, SCREEN_HEIGHT/2 - 50, 20, 100)
player2 = pygame.Rect(SCREEN_WIDTH-70, SCREEN_HEIGHT/2 - 50, 20, 100)

# створення м'яча
ball = pygame.Rect(SCREEN_WIDTH/2 - 10, SCREEN_HEIGHT/2 - 10, 20, 20)

# початкова швидкість м'яча
ball_speed_x = 0.5
ball_speed_y = 0.5

# головний цикл гри
while True:
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            pygame.quit()
            sys.exit()

    # рух гравців
    keys = pygame.key.get_pressed()
    if keys[pygame.K_w]:
        player1.y -= 1
    if keys[pygame.K_s]:
        player1.y += 1
    if keys[pygame.K_UP]:
        player2.y -= 1
    if keys[pygame.K_DOWN]:
        player2.y += 1

    # рух м'яча
    ball.x += ball_speed_x
    ball.y += ball_speed_y

    # зіткнення м'яча з гравцем 1
    if ball.colliderect(player1):
        ball_speed_x *= -1

    # зіткнення м'яча з гравцем 2
    if ball.colliderect(player2):
        ball_speed_x *= -1

    # зіткнення м'яча з верхньою та нижньою стінками
    if ball.top <= 0 or ball.bottom >= SCREEN_HEIGHT:
        ball_speed_y *= -1

    # якщо м'яч покидає екран зправа або зліва, гра закінчується
    if ball.left <= 0 or ball.right >= SCREEN_WIDTH:
        pygame.quit()
        sys.exit()

    # очистка екрану
    screen.fill(BLACK)

    # відображення гравців та м'яча на екрані
    pygame.draw.rect(screen, WHITE, player1)
    pygame.draw.rect(screen, WHITE, player2)
    pygame.draw.ellipse(screen, WHITE, ball)

    # оновлення екрану
    pygame.display.flip()
