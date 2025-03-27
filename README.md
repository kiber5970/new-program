import pygame
import random

pygame.init()
width, height = 600, 400
win = pygame.display.set_mode((width, height))
clock = pygame.time.Clock()

snake_block = 20
snake_speed = 10
font = pygame.font.SysFont(None, 35)

def draw_snake(snake_list):
    for x in snake_list:
        pygame.draw.rect(win, (0, 255, 0), [x[0], x[1], snake_block, snake_block])

def main():
    game_over = False
    x, y = width//2, height//2
    dx, dy = 0, 0
    snake = []
    length = 1

    food = [random.randrange(0, width - snake_block, snake_block),
            random.randrange(0, height - snake_block, snake_block)]

    while not game_over:
        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                game_over = True
            if event.type == pygame.KEYDOWN:
                if event.key == pygame.K_LEFT: dx, dy = -snake_block, 0
                elif event.key == pygame.K_RIGHT: dx, dy = snake_block, 0
                elif event.key == pygame.K_UP: dx, dy = 0, -snake_block
                elif event.key == pygame.K_DOWN: dx, dy = 0, snake_block

        x += dx
        y += dy

        if x < 0 or x >= width or y < 0 or y >= height or [x, y] in snake:
            game_over = True

        snake.append([x, y])
        if len(snake) > length:
            del snake[0]

        if x == food[0] and y == food[1]:
            food = [random.randrange(0, width - snake_block, snake_block),
                    random.randrange(0, height - snake_block, snake_block)]
            length += 1

        win.fill((0, 0, 0))
        draw_snake(snake)
        pygame.draw.rect(win, (255, 0, 0), [food[0], food[1], snake_block, snake_block])
        pygame.display.update()
        clock.tick(snake_speed)

    pygame.quit()

main()
