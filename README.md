import math
import random

def print_board(board):
    for row in board:
        print(" | ".join(row))
        print("-" * 9)

def is_moves_left(board):
    for row in board:
        if " " in row:
            return True
    return False

def evaluate(board):
    for row in board:
        if row.count("X") == 3:
            return 10
        if row.count("O") == 3:
            return -10
    
    for col in range(3):
        if board[0][col] == board[1][col] == board[2][col] and board[0][col] != " ":
            return 10 if board[0][col] == "X" else -10
    
    if board[0][0] == board[1][1] == board[2][2] and board[0][0] != " ":
        return 10 if board[0][0] == "X" else -10
    
    if board[0][2] == board[1][1] == board[2][0] and board[0][2] != " ":
        return 10 if board[0][2] == "X" else -10
    
    return 0

def minimax(board, depth, is_max):
    score = evaluate(board)
    if score == 10 or score == -10:
        return score
    if not is_moves_left(board):
        return 0
    
    if is_max:
        best = -math.inf
        for i in range(3):
            for j in range(3):
                if board[i][j] == " ":
                    board[i][j] = "X"
                    best = max(best, minimax(board, depth + 1, False))
                    board[i][j] = " "
        return best
    else:
        best = math.inf
        for i in range(3):
            for j in range(3):
                if board[i][j] == " ":
                    board[i][j] = "O"
                    best = min(best, minimax(board, depth + 1, True))
                    board[i][j] = " "
        return best

def find_best_move(board):
    best_val = -math.inf
    best_move = (-1, -1)
    
    for i in range(3):
        for j in range(3):
            if board[i][j] == " ":
                board[i][j] = "X"
                move_val = minimax(board, 0, False)
                board[i][j] = " "
                if move_val > best_val:
                    best_val = move_val
                    best_move = (i, j)
    
    return best_move

def play_tic_tac_toe():
    board = [[" " for _ in range(3)] for _ in range(3)]
    player = input("Choose your symbol (X/O): ").upper()
    ai = "O" if player == "X" else "X"
    
    for _ in range(9):
        print_board(board)
        if (player == "X" and _ % 2 == 0) or (player == "O" and _ % 2 == 1):
            row, col = map(int, input("Enter row and column (0-2): ").split())
            if board[row][col] == " ":
                board[row][col] = player
            else:
                print("Invalid move, try again!")
                continue
        else:
            print("AI is making a move...")
            row, col = find_best_move(board)
            board[row][col] = ai
        
        score = evaluate(board)
        if score == 10:
            print_board(board)
            print("AI wins!")
            return
        elif score == -10:
            print_board(board)
            print("You win!")
            return
    
    print_board(board)
    print("It's a draw!")

if __name__ == "__main__":
    play_tic_tac_toe()
