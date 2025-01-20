# othello2024
import random

BLACK = 1
WHITE = 2

# 初期ボードの設定
def initialize_board():
    board = [
        [0, 0, 0, 0, 0, 0],
        [0, 0, 0, 0, 0, 0],
        [0, 0, 1, 2, 0, 0],
        [0, 0, 2, 1, 0, 0],
        [0, 0, 0, 0, 0, 0],
        [0, 0, 0, 0, 0, 0],
    ]
    return board

# ボードの表示
def print_board(board):
    for row in board:
        print(" ".join(str(cell) if cell != 0 else "." for cell in row))
    print()

# 石を置けるか判定する
def can_place_x_y(board, stone, x, y):
    if board[y][x] != 0:
        return False

    opponent = 3 - stone
    directions = [(-1, -1), (-1, 0), (-1, 1), (0, -1), (0, 1), (1, -1), (1, 0), (1, 1)]

    for dx, dy in directions:
        nx, ny = x + dx, y + dy
        found_opponent = False

        while 0 <= nx < len(board[0]) and 0 <= ny < len(board) and board[ny][nx] == opponent:
            found_opponent = True
            nx += dx
            ny += dy

        if found_opponent and 0 <= nx < len(board[0]) and 0 <= ny < len(board) and board[ny][nx] == stone:
            return True

    return False

# ボード上で置けるか判定
def can_place(board, stone):
    for y in range(len(board)):
        for x in range(len(board[0])):
            if can_place_x_y(board, stone, x, y):
                return True
    return False

# 石を置く
def place_stone(board, stone, x, y):
    if not can_place_x_y(board, stone, x, y):
        return False

    board[y][x] = stone
    opponent = 3 - stone
    directions = [(-1, -1), (-1, 0), (-1, 1), (0, -1), (0, 1), (1, -1), (1, 0), (1, 1)]

    for dx, dy in directions:
        nx, ny = x + dx, y + dy
        stones_to_flip = []

        while 0 <= nx < len(board[0]) and 0 <= ny < len(board) and board[ny][nx] == opponent:
            stones_to_flip.append((nx, ny))
            nx += dx
            ny += dy

        if stones_to_flip and 0 <= nx < len(board[0]) and 0 <= ny < len(board) and board[ny][nx] == stone:
            for flip_x, flip_y in stones_to_flip:
                board[flip_y][flip_x] = stone

    return True

# ランダムに石を置くAI
def random_ai(board, stone):
    possible_moves = [(x, y) for y in range(len(board)) for x in range(len(board[0])) if can_place_x_y(board, stone, x, y)]
    if possible_moves:
        return random.choice(possible_moves)
    return None

# ゲームの実行
def play_othello():
    board = initialize_board()
    current_player = BLACK

    print("ゲーム開始！")
    print_board(board)

    while can_place(board, BLACK) or can_place(board, WHITE):
        if can_place(board, current_player):
            if current_player == BLACK:
                print("プレイヤー(黒)の番です。")
                x, y = random_ai(board, BLACK)
            else:
                print("AI(白)の番です。")
                x, y = random_ai(board, WHITE)

            place_stone(board, current_player, x, y)
            print(f"{'黒' if current_player == BLACK else '白'}が({x}, {y})に置きました。")
            print_board(board)
        else:
            print(f"{'黒' if current_player == BLACK else '白'}は置ける場所がありません。スキップします。")

        current_player = 3 - current_player

    # ゲーム終了、結果表示
    black_count = sum(row.count(BLACK) for row in board)
    white_count = sum(row.count(WHITE) for row in board)
    print(f"最終結果: 黒: {black_count}, 白: {white_count}")
    if black_count > white_count:
        print("黒の勝ち！")
    elif black_count < white_count:
        print("白の勝ち！")
    else:
        print("引き分け！")

# 実行
play_othello()
AI オセロを作ろう！！ 
