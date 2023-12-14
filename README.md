import pygame
import sys

# 초기화
pygame.init()

# 화면 크기 설정
screen_width = 800
screen_height = 560
screen = pygame.display.set_mode((screen_width, screen_height))

# 색상 정의
WHITE = (255, 255, 255)

# 이미지 로드
background_image = pygame.image.load("ground.png")
start_image = pygame.image.load("start.png")  # 시작 화면 이미지
end_image = pygame.image.load("end.png")  # 게임 종료 이미지
green_image = pygame.image.load("green.png")
black_image = pygame.image.load("black.png")
tom_image = pygame.image.load("tom.png")
rcheese_image = pygame.image.load("rcheese.png")
bcheese_image = pygame.image.load("bcheese.png")
bhouse_image = pygame.image.load("bhouse.png")
rhouse_image = pygame.image.load("rhouse.png")
yhouse_image = pygame.image.load("yhouse.png") 
ycheese_image = pygame.image.load("ycheese.png") 
# 게임 변수 초기화 함수
def initialize_game():
    global mx, my, cheese_positions, game_over
    mx, my = 2, 4
    cheese_positions = [(3, 3, 'y'), (3, 5, 'r'), (6, 3, 'r'), 
                    (3, 4, 'b'), (5, 4, 'b'), (6, 5, 'y')]
    game_over = False
# 게임 초기화
initialize_game()

# 게임 변수 초기화
start_screen = True
game_over = False

# 배경 음악 로드
pygame.mixer.music.load("bgm.mp3")

# 각 레벨의 초기 상태를 저장하는 리스트 추가
initial_states = [None, None, None]
# 레벨 초기 설정 함수
def initialize_level(level):
    global mx, my, cheese_positions, house_positions, maze

    if level == 1:
        mx = 2
        my = 4
        cheese_positions = [(3, 3, 'y'), (3, 5, 'r'), (6, 3, 'r'),
                            (3, 4, 'b'), (5, 4, 'b'), (6, 5, 'y')]
        house_positions = [(4, 3, 'y'), (4, 4, 'b'), (4, 5, 'y'),
                           (5, 3, 'r'), (5, 4, 'r'), (5, 5, 'b')]
        maze = [
            [0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
            [0, 1, 1, 1, 1, 1, 1, 1, 1, 0, 0, 0, 0],
            [0, 1, 0, 0, 1, 0, 0, 0, 1, 0, 0, 0, 0],
            [0, 1, 0, 0, 0, 0, 0, 0, 1, 0, 0, 0, 0],
            [0, 1, 0, 0, 0, 0, 0, 1, 1, 0, 0, 0, 0],
            [0, 1, 0, 0, 0, 0, 0, 0, 1, 0, 0, 0, 0],
            [0, 1, 0, 0, 1, 0, 0, 0, 1, 0, 0, 0, 0],
            [0, 1, 1, 1, 1, 1, 1, 1, 1, 0, 0, 0, 0],
            [0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0]
        ]
        # 레벨 1의 초기 상태 저장
        initial_states[level - 1] = (mx, my, cheese_positions.copy(), house_positions.copy(), maze.copy())

    elif level == 2:
        mx = 1
        my = 5
        cheese_positions =  [(2, 3, 'y'), (2, 4, 'b'), (2, 5, 'r'), (3, 4, 'y'), (7, 4, 'b'), (7, 5, 'r')]

        house_positions = [(5, 3, 'y'), (4, 4, 'b'), (4, 5, 'r'), (4, 3, 'y'), (5, 4, 'b'), (5, 5, 'r')]

        maze = [
             [0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
            [0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
            [0, 1, 1, 1, 1, 1, 1, 1, 0, 0],
            [1, 0, 0, 0, 0, 0, 0, 0, 1, 0],
            [1, 0, 0, 0, 0, 0, 0, 0, 1, 0],
            [1, 0, 0, 0, 0, 0, 0, 0, 0, 1],
            [0, 1, 1, 1, 1, 0, 0, 0, 0, 1],
            [0, 0, 0, 0, 0, 1, 1, 1, 1, 0],
            [0, 0, 0, 0, 0, 0, 0, 0, 0, 0]
        ]
        # 레벨 2의 초기 상태 저장
        initial_states[level - 2] = (mx, my, cheese_positions.copy(), house_positions.copy(), maze.copy())
    
    elif level == 3:
        mx = 4
        my = 5
        cheese_positions =  [(2, 2, 'b'), (4, 2, 'r'), (5, 2, 'r'), (2, 4, 'b'), (3, 4, 'b'), (5, 4, 'r')]

        house_positions = [(1, 3, 'b'), (2, 3, 'b'), (3, 3, 'r'), (4, 3, 'b'), (5, 3, 'r'), (6, 3, 'r')]

        maze = [
             [0, 1, 1, 1, 1, 1, 1, 0],
            [1, 1, 0, 0, 0, 0, 1, 1],
            [1, 0, 0, 0, 0, 0, 0, 1],
            [1, 0, 0, 0, 0, 0, 0, 1],
            [1, 0, 0, 0, 0, 0, 0, 1],
            [1, 0, 0, 0, 0, 0, 0, 1],
            [1, 1, 1, 0, 0, 1, 1, 1],
            [0, 0, 1, 1, 1, 1, 0, 0],
        ]
        # 레벨 3의 초기 상태 저장
        initial_states[level - 3] = (mx, my, cheese_positions.copy(), house_positions.copy(), maze.copy())

    elif level == 4:
        mx = 7
        my = 7
        cheese_positions = [(3, 2, 'r'), (4, 2, 'b'), (5, 4, 'r'),
                            (6, 5, 'b'), (2, 6, 'b'), (5, 6, 'r')]
        house_positions = [(2, 3, 'b'), (3, 3, 'r'), (4, 3, 'b'),
                           (2, 4, 'b'), (3, 4, 'r'), (4, 4, 'r')]
        maze = [
            [1, 1, 1, 1, 1, 1, 1, 1, 0],
            [1, 0, 0, 0, 0, 0, 0, 1, 0],
            [1, 0, 1, 0, 0, 0, 0, 1, 0],
            [1, 0, 0, 0, 0, 1, 0, 1, 0],
            [1, 1, 0, 0, 0, 0, 0, 1, 1],
            [0, 1, 0, 1, 1, 0, 0, 0, 1],
            [0, 1, 0, 0, 0, 0, 0, 0, 1],
            [0, 1, 0, 0, 1, 0, 0, 0, 1],
            [0, 1, 1, 1, 1, 1, 1, 1, 1]
        ]
        # 레벨 4의 초기 상태 저장
        initial_states[level - 4] = (mx, my, cheese_positions.copy(), house_positions.copy(), maze.copy())

    elif level == 5:
        mx = 6
        my = 7
        cheese_positions =  [(4, 2, 'y'), (4, 3, 'y'), (4, 5, 'y'),
                              (5, 4, 'y'), (6, 5, 'y'), (7, 6, 'y')]

        house_positions =  [(1, 4, 'y'), (2, 4, 'y'), (3, 4, 'y'),
                             (1, 5, 'y'), (2, 5, 'y'), (3, 5, 'y')]

        maze = [
             [0, 0, 1, 1, 1, 1, 1, 1, 0, 0],
            [0, 0, 1, 0, 0, 0, 0, 1, 1, 1],
            [0, 0, 1, 0, 0, 0, 0, 0, 0, 1],
            [1, 1, 1, 0, 0, 0, 1, 1, 0, 1],
            [1, 0, 0, 0, 0, 0, 0, 0, 0, 1],
            [1, 0, 0, 0, 0, 1, 0, 0, 1, 1],
            [1, 1, 1, 1, 0, 1, 0, 0, 0, 1],
            [0, 0, 0, 1, 0, 0, 0, 0, 0, 1],
            [0, 0, 0, 1, 1, 1, 1, 1, 1, 1]
    ]
        # 레벨 5의 초기 상태 저장
        initial_states[level - 5] = (mx, my, cheese_positions.copy(), house_positions.copy(), maze.copy())
    

    elif level == 6:
        mx = 8
        my = 3
        cheese_positions =  [(4, 2, 'y'), (6, 2, 'y'), (2, 3, 'y'), 
                             (6, 3, 'y'), (3, 4, 'y'), (4, 4, 'y')]

        house_positions =  [(3, 6, 'y'), (4, 6, 'y'), (5, 6, 'y'), 
                            (6, 6, 'y'), (5, 5, 'y'), (6, 5, 'y')]

        maze = [
            [0, 0, 1, 1, 1, 1, 1, 0, 0, 0],
            [1, 1, 1, 0, 0, 0, 1, 1, 1, 1],
            [1, 0, 0, 0, 0, 0, 0, 0, 0, 1],
            [1, 0, 0, 0, 0, 0, 0, 0, 0, 1],
            [1, 0, 0, 0, 0, 0, 0, 1, 1, 1],
            [1, 0, 0, 0, 0, 0, 0, 1, 0, 0],
            [0, 1, 1, 0, 0, 0, 0, 1, 0, 0],
            [0, 0, 1, 0, 0, 0, 0, 1, 0, 0],
            [0, 0, 1, 1, 1, 1, 1, 1, 0, 0]
            
        ]
        # 레벨 6의 초기 상태 저장
        initial_states[level - 6] = (mx, my, cheese_positions.copy(), house_positions.copy(), maze.copy())

    elif level == 7:
        mx = 5
        my = 6
        cheese_positions =  [(4, 3, 'y'), (5, 3, 'y'),
                              (6, 3, 'y'), (8, 5, 'y'),]

        house_positions =  [(6, 4, 'y'), (7, 4, 'y'),
                             (6, 5, 'y'), (7, 5, 'y'),]

        maze = [
            [0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
            [0, 0, 1, 1, 1, 1, 1, 1, 0, 0, 0, 0, 0],
            [0, 0, 1, 0, 0, 0, 0, 1, 0, 0, 0, 0, 0],
            [0, 0, 1, 0, 0, 0, 0, 1, 1, 0, 0, 0, 0],
            [0, 0, 1, 0, 0, 1, 0, 0, 1, 1, 1, 0, 0],
            [0, 0, 1, 1, 0, 0, 0, 0, 0, 0, 1, 0, 0],
            [0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 1, 0, 0],
            [0, 0, 0, 1, 1, 1, 1, 1, 1, 1, 1, 0, 0],
            [0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
            [0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0]
            
        ]
        # 레벨 7의 초기 상태 저장
        initial_states[level - 7] = (mx, my, cheese_positions.copy(), house_positions.copy(), maze.copy())

    elif level == 8:
        mx = 2
        my = 4
        cheese_positions = [(9, 4, 'y'), (6, 5, 'y'), (3, 5, 'y'),
                            (3, 4, 'y'), (6, 3, 'y')]
        house_positions = [(4, 4, 'y'), (6, 4, 'y'), (6, 2, 'y'),
                           (6, 6, 'y'), (8, 4, 'y')]
        maze = [
            [0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
            [0, 0, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 0],
            [1, 1, 1, 0, 0, 0, 0, 0, 0, 0, 0, 1, 0],
            [1, 0, 0, 0, 1, 1, 0, 1, 1, 0, 0, 1, 0],
            [1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 1, 0],
            [1, 1, 0, 0, 1, 1, 0, 1, 1, 0, 1, 0, 0],
            [0, 1, 0, 0, 0, 0, 0, 0, 0, 0, 1, 0, 0],
            [0, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 0, 0],
            [0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
            [0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0]
            
        ]
        # 레벨 8의 초기 상태 저장
        initial_states[level - 8] = (mx, my, cheese_positions.copy(), house_positions.copy(), maze.copy())

# 초기 레벨 설정
current_level = 1
initialize_level(current_level)

# 미로 시작 위치 계산
maze_start_x = (screen_width - 500) // 2
maze_start_y = (screen_height - 450) // 2

# 이동 가능성 확인 함수
def can_move_cheese(cx, cy, dx, dy, cheese_positions):
    new_x = cx + dx
    new_y = cy + dy
    if 0 <= new_x < 13 and 0 <= new_y < 9 and maze[new_y][new_x] == 0:
        existing_positions = [(x, y) for x, y, _ in cheese_positions]
        if (new_x, new_y) not in existing_positions:
            return True
    return False

# 배경 음악 재생
pygame.mixer.music.play(-1)  # -1을 전달하여 반복 재생

# 레벨 초기 설정 함수 추가
def reset_level():
    global current_level, game_over
    current_level = 1
    initialize_level(current_level)
    game_over = False



# 게임 루프
reset_game = False  # 초기화 플래그 추가
while True:
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            pygame.quit()
            sys.exit()
        elif start_screen:
            if event.type == pygame.MOUSEBUTTONDOWN:
                start_screen = False
            screen.blit(start_image, (0, 0))
        else:
            # 키보드 입력 감지
            if event.type == pygame.KEYDOWN:
                if event.key == pygame.K_q:
                    # Q 키가 눌리면 이전 상태로 되돌리기
                    if previous_state:
                        mx, my = previous_state["mx"], previous_state["my"]
                        cheese_positions = previous_state["cheese_positions"].copy()
                elif event.key == pygame.K_z:
                    # 'z' 키를 누르면 게임 초기화
                    initialize_game()
                    reset_game = True  # 초기화 플래그 설정
                else:
                    dx, dy = 0, 0
                    if event.key == pygame.K_UP and maze[my - 1][mx] == 0:
                        dy = -1
                    elif event.key == pygame.K_DOWN and maze[my + 1][mx] == 0:
                        dy = 1
                    elif event.key == pygame.K_LEFT and maze[my][mx - 1] == 0:
                        dx = -1
                    elif event.key == pygame.K_RIGHT and maze[my][mx + 1] == 0:
                        dx = 1
                
                    # 현재 상태 저장 (Q 키와 'z' 키가 아닌 다른 키가 눌렸을 때)
                    previous_state = {
                        "mx": mx,
                        "my": my,
                        "cheese_positions": cheese_positions.copy()
                    }

                    # 톰 이동 처리
                    dx, dy = 0, 0
                    if event.key == pygame.K_UP and maze[my - 1][mx] == 0:
                        dy = -1
                    elif event.key == pygame.K_DOWN and maze[my + 1][mx] == 0:
                        dy = 1
                    elif event.key == pygame.K_LEFT and maze[my][mx - 1] == 0:
                        dx = -1
                    elif event.key == pygame.K_RIGHT and maze[my][mx + 1] == 0:
                        dx = 1
                
                    # 톰이 치즈와 충돌하는지 확인
                    cheese_collision = False
                    for i, (cx, cy, cheese_type) in enumerate(cheese_positions):
                        if mx + dx == cx and my + dy == cy:
                            cheese_collision = True
                            if not can_move_cheese(cx, cy, dx, dy, cheese_positions):
                                dx, dy = 0, 0
                            else:
                                new_cx, new_cy = cx + dx, cy + dy
                                cheese_positions[i] = (new_cx, new_cy, cheese_type)
                            break

                    # 톰 이동
                    if not cheese_collision or (dx != 0 or dy != 0):
                        mx += dx
                        my += dy
    if start_screen:
        screen.blit(start_image, (0, 0))
    elif game_over:
        screen.blit(end_image, (0, 0))
    else:
        # 모든 집이 black.png로 바뀌었는지 확인
        all_black = all((x, y) in [(cx, cy) for cx, cy, _ in cheese_positions] for x, y, _ in house_positions)

        if all_black:
            # 현재 레벨 종료, 다음 레벨로 이동
            current_level += 1
            if current_level <= 8:
                initialize_level(current_level)
            else:
                # 모든 레벨 클리어 시 게임 종료
                game_over = True

        screen.blit(background_image, (0, 0))

    if start_screen:
        screen.blit(start_image, (0, 0))
    elif game_over:
        screen.blit(end_image, (0, 0))
    else:
        # 모든 집이 black.png로 바뀌었는지 확인
        all_black = all((x, y) in [(cx, cy) for cx, cy, _ in cheese_positions] for x, y, _ in house_positions)

        if all_black:
            game_over = True
            continue

        screen.blit(background_image, (0, 0))
       # 미로 그리기
        for y in range(9):
            for x in range(13):
                if 0 <= y < len(maze) and 0 <= x < len(maze[y]) and maze[y][x] == 1:
                    screen.blit(green_image, (maze_start_x + x * 50, maze_start_y + y * 50))

        # 치즈 그리기
        for x, y, cheese_type in cheese_positions:
            if maze[y][x] == 0:
                cheese_image = ycheese_image if cheese_type == 'y' else bcheese_image if cheese_type == 'b' else rcheese_image
                screen.blit(cheese_image, (maze_start_x + x * 50, maze_start_y + y * 50))

        # 집 그리기
        for x, y, house_type in house_positions:
            if (x, y) in [(cx, cy) for cx, cy, _ in cheese_positions]:
                screen.blit(black_image, (maze_start_x + x * 50, maze_start_y + y * 50))
            else:
                if house_type == 'b':
                    house_image = bhouse_image
                elif house_type == 'r':
                    house_image = rhouse_image
                elif house_type == 'y':
                    house_image = yhouse_image
                screen.blit(house_image, (maze_start_x + x * 50, maze_start_y + y * 50))

        # 톰 그리기
        screen.blit(tom_image, (maze_start_x + mx * 50, maze_start_y + my * 50))

    pygame.display.flip()
