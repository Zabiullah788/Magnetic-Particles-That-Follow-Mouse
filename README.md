# Magnetic-Particles-That-Follow-Mouse

python
import pygame
import sys
import random
import math

pygame.init()
screen = pygame.display.set_mode((800, 600))
pygame.display.set_caption("Magnetic Particles")
clock = pygame.time.Clock()

particles = [{"pos": [random.randint(0, 800), random.randint(0, 600)],
              "vel": [0, 0]} for _ in range(100)]

while True:
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            pygame.quit()
            sys.exit()

    mx, my = pygame.mouse.get_pos()
    screen.fill((0, 0, 0))

    for p in particles:
        dx, dy = mx - p["pos"][0], my - p["pos"][1]
        dist = math.hypot(dx, dy)
        angle = math.atan2(dy, dx)
        if dist > 1:
            p["vel"][0] += math.cos(angle) * 0.5
            p["vel"][1] += math.sin(angle) * 0.5

        # Friction and update
        p["vel"][0] *= 0.9
        p["vel"][1] *= 0.9
        p["pos"][0] += p["vel"][0]
        p["pos"][1] += p["vel"][1]
        pygame.draw.circle(screen, (0, 255, 255), (int(p["pos"][0]), int(p["pos"][1])), 3)

    pygame.display.flip()
    clock.tick(60)
