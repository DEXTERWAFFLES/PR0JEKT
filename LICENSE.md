    import pygame
    import sys
    import random

    pygame.init()

    WIDTH, HEIGHT = 800, 400
    WIN = pygame.display.set_mode((WIDTH, HEIGHT))
    pygame.display.set_caption("GoBrick")

    WHITE = (255, 255, 255)
    BLACK = (0, 0, 0)
    BLUE = (66, 135, 245)
    FPS = 60
    ground_height = 350

    player_size = 40
    player_x = 50
    player_y = ground_height - player_size
    player_vel_y = 0
    grav = 1
    jump_power = 20
    is_jumping = False

    obstacle_width = 20
    obstacle_height = 40
    obstacle_x = WIDTH
    obstacle_speed = 10

    clock = pygame.time.Clock()
    font = pygame.font.SysFont(None, 48)
    retro_font = pygame.font.SysFont("Courier", 64)
    score = 0
    
    def show_splash_screen():
      WIN.fill(BLACK)
      title_text = retro_font.render("GoBrick", True, BLUE)
      sub_text = font.render("Press SPACE to Start", True, WHITE)
      WIN.blit(title_text, (WIDTH // 2 - title_text.get_width() // 2, HEIGHT // 3))
      WIN.blit(sub_text, (WIDTH // 2 - sub_text.get_width() // 2, HEIGHT // 2))
      pygame.display.update()
      waiting = True
      while waiting:
        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                pygame.quit()
                sys.exit()
            if event.type == pygame.KEYDOWN:
                if event.key == pygame.K_SPACE:
                    waiting = False
    show_splash_screen()
   
    running = True
    while running:
      clock.tick(FPS)
      WIN.fill(WHITE)

    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            pygame.quit()
            sys.exit()
        if event.type == pygame.KEYDOWN and not is_jumping:
            if event.key == pygame.K_SPACE:
                player_vel_y = -jump_power
                is_jumping = True

    player_y += player_vel_y
    player_vel_y += grav

    if player_y >= ground_height - player_size:
        player_y = ground_height - player_size
        is_jumping = False

    obstacle_x -= obstacle_speed
    if obstacle_x < -obstacle_width:
        obstacle_x = WIDTH + random.randint(0, 200)
        score += 1

    player_rect = pygame.Rect(player_x, player_y, player_size, player_size)
    obstacle_rect = pygame.Rect(obstacle_x, ground_height - obstacle_height, obstacle_width, obstacle_height)

    if player_rect.colliderect(obstacle_rect):
        msg = font.render(f"Game Over! Score: {score}", True, BLACK)
        WIN.blit(msg, (WIDTH // 2 - 150, HEIGHT // 2))
        pygame.display.update()
        pygame.time.delay(2000)
        pygame.quit()
        sys.exit()

    pygame.draw.rect(WIN, BLUE, player_rect)
    pygame.draw.rect(WIN, BLACK, obstacle_rect)
    pygame.draw.line(WIN, BLACK, (0, ground_height), (WIDTH, ground_height), 2)
    score_text = font.render(f"Score: {score}", True, BLACK)
    WIN.blit(score_text, (10, 10))
    pygame.display.update()

            pygame.quit()

    
