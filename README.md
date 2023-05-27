# Ping-Pong
Yes the real ping pong
import pygame
import random

# Initialize Pygame
pygame.init()

# Set up the game window
width, height = 800, 400
screen = pygame.display.set_mode((width, height))
pygame.display.set_caption("Pong")

# Define colors
BLACK = (0, 0, 0)
WHITE = (255, 255, 255)

# Set up game variables
ball_pos = [width // 2, height // 2]
ball_radius = 10
ball_velocity = [random.choice([-2, 2]), random.choice([-2, 2])]
paddle_width, paddle_height = 10, 60
paddle_velocity = 5
player1_pos = [0, height // 2 - paddle_height // 2]
player2_pos = [width - paddle_width, height // 2 - paddle_height // 2]
score1 = 0
score2 = 0
font = pygame.font.Font(None, 36)

# Game loop
running = True
while running:
    # Handle events
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            running = False

    # Move paddles
    keys = pygame.key.get_pressed()
    if keys[pygame.K_w] and player1_pos[1] > 0:
        player1_pos[1] -= paddle_velocity
    if keys[pygame.K_s] and player1_pos[1] < height - paddle_height:
        player1_pos[1] += paddle_velocity
    if keys[pygame.K_UP] and player2_pos[1] > 0:
        player2_pos[1] -= paddle_velocity
    if keys[pygame.K_DOWN] and player2_pos[1] < height - paddle_height:
        player2_pos[1] += paddle_velocity

    # Update ball position
    ball_pos[0] += ball_velocity[0]
    ball_pos[1] += ball_velocity[1]

    # Check ball collision with walls
    if ball_pos[1] >= height - ball_radius or ball_pos[1] <= ball_radius:
        ball_velocity[1] *= -1
    if ball_pos[0] >= width - ball_radius:
        score1 += 1
        ball_pos = [width // 2, height // 2]
        ball_velocity = [random.choice([-2, 2]), random.choice([-2, 2])]
    if ball_pos[0] <= ball_radius:
        score2 += 1
        ball_pos = [width // 2, height // 2]
        ball_velocity = [random.choice([-2, 2]), random.choice([-2, 2])]

    # Check ball collision with paddles
    if ball_pos[0] <= player1_pos[0] + paddle_width and player1_pos[1] - ball_radius <= ball_pos[1] <= player1_pos[1] + paddle_height + ball_radius:
        ball_velocity[0] *= -1
    if ball_pos[0] >= player2_pos[0] - ball_radius and player2_pos[1] - ball_radius <= ball_pos[1] <= player2_pos[1] + paddle_height + ball_radius:
        ball_velocity[0] *= -1

    # Clear the screen
    screen.fill(BLACK)

    # Draw paddles and ball
    pygame.draw.rect(screen, WHITE, (player1
