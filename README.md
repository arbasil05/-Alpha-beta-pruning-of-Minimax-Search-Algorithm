# ExpNo 7: Implement Alpha-beta pruning of Minimax Search Algorithm for a Simple TIC-TAC-TOE game

### Name: Abdur Rahman Basil A H
### Register Number/Staff Id: 212223040002

## Aim:
Implement Alpha-beta pruning of Minimax Search Algorithm for a Simple TIC-TAC-TOE game.

## Goals of Alpha-Beta Pruning in Minimax Search Algorithm
- Improve the decision-making efficiency of the computer player by reducing the number of evaluated nodes in the game tree.
- Tic-Tac-Toe game implementation incorporating the Alpha-Beta pruning and the Minimax algorithm with Python Code.

## Implementation
The project involves developing a Tic-Tac-Toe game that uses Alpha-Beta pruning with the Minimax algorithm. Using this algorithm, the computer player evaluates possible moves and selects the optimal action.

### The Minimax Algorithm
- Recursively evaluates all possible moves and outcomes, creating a game tree.

### Alpha-Beta Pruning
- Alpha-Beta (𝛼−𝛽) algorithm is an optimized version of Minimax. It stops evaluating a move when it’s confirmed to be worse than a previously examined move. This reduces the number of branches to explore while maintaining the same result, improving efficiency.

---
```python

import time

class Game:
    def __init__(self):
        self.initialize_game()

    def initialize_game(self):
        self.current_state = [['.','.','.'],
                              ['.','.','.'],
                              ['.','.','.']]

        # Player X always plays first
        self.player_turn = 'X'

    def draw_board(self):
        for i in range(0, 3):
            for j in range(0, 3):
                print('{}|'.format(self.current_state[i][j]), end=" ")
            print()
        print()
    def is_valid(self, px, py):
        if px < 0 or px > 2 or py < 0 or py > 2:
            return False
        elif self.current_state[px][py] != '.':
            return False
        else:
            return True
    def is_end(self):
    # Vertical win
        for i in range(0, 3):
            if (self.current_state[0][i] != '.' and
                self.current_state[0][i] == self.current_state[1][i] and
                self.current_state[1][i] == self.current_state[2][i]):
                return self.current_state[0][i]

        # Horizontal win
        for i in range(0, 3):
            if (self.current_state[i] == ['X', 'X', 'X']):
                return 'X'
            elif (self.current_state[i] == ['O', 'O', 'O']):
                return 'O'

    # Main diagonal win
        if (self.current_state[0][0] != '.' and
            self.current_state[0][0] == self.current_state[1][1] and
            self.current_state[0][0] == self.current_state[2][2]):
            return self.current_state[0][0]

    # Second diagonal win
        if (self.current_state[0][2] != '.' and
            self.current_state[0][2] == self.current_state[1][1] and
            self.current_state[0][2] == self.current_state[2][0]):
            return self.current_state[0][2]

    # Is the whole board full?
       "The code scans the entire board, row by row and column by column.
          If it finds at least one empty cell (.), it concludes that the game hasn't finished, and so it returns None, allowing the game to proceed."

    # It's a tie!
        return '.'
    def max_alpha_beta(self, alpha, beta):
        maxv = -2
        px = None
        py = None

        result = self.is_end()

        if result == 'X':
            return (-1, 0, 0)
        elif result == 'O':
            return (1, 0, 0)
        elif result == '.':
            return (0, 0, 0)

        for i in range(0, 3):
            for j in range(0, 3):
                if self.current_state[i][j] == '.':
                    self.current_state[i][j] = 'O'
                    (m, min_i, in_j) = self.min_alpha_beta(alpha, beta)
                    if m > maxv:
                        maxv = m
                        px = i
                        py = j
                    self.current_state[i][j] = '.'

                    # Next two ifs in Max and Min are the only difference between regular algorithm and minimax
                    if maxv >= beta:
                        return (maxv, px, py)

                    if maxv > alpha:
                        alpha = maxv

        return (maxv, px, py)

    def min_alpha_beta(self, alpha, beta):

        minv = 2

        qx = None
        qy = None

        result = self.is_end()

        if result == 'X':
            return (-1, 0, 0)
        elif result == 'O':
            return (1, 0, 0)
        elif result == '.':
            return (0, 0, 0)

        for i in range(0, 3):
            for j in range(0, 3):
                if self.current_state[i][j] == '.':
                    self.current_state[i][j] = 'X'
                    (m, max_i, max_j) = self.max_alpha_beta(alpha, beta)
                    if m < minv:
                        minv = m
                        qx = i
                        qy = j
                    self.current_state[i][j] = '.'

                    if minv <= alpha:
                        return (minv, qx, qy)

                    if minv < beta:
                        beta = minv

        return (minv, qx, qy)
    def play_alpha_beta(self):
        while True:
            self.draw_board()
            self.result = self.is_end()

            if self.result != None:
                if self.result == 'X':
                    print('The winner is X!')
                elif self.result == 'O':
                    print('The winner is O!')
                elif self.result == '.':
                    print("It's a tie!")


                self.initialize_game()
                return

            if self.player_turn == 'X':

                while True:
                    start = time.time()
                    (m, qx, qy) = self.min_alpha_beta(-2, 2)
                    end = time.time()
                    print('Evaluation time: {}s'.format(round(end - start, 7)))
                    print('Recommended move: X = {}, Y = {}'.format(qx, qy))

                    px = int(input('Insert the X coordinate: '))
                    py = int(input('Insert the Y coordinate: '))

                    qx = px
                    qy = py

                    if self.is_valid(px, py):
                        self.current_state[px][py] = 'X'
                        self.player_turn = 'O'
                        break
                    else:
                        print('The move is not valid! Try again.')

            else:
                (m, px, py) = self.max_alpha_beta(-2, 2)
                self.current_state[px][py] = 'O'
                self.player_turn = 'X'



def main():
    g = Game()
    g.play_alpha_beta()

if __name__ == "__main__":
    main()
```

## Sample Input and Output:

![Sample 1](https://github.com/natsaravanan/19AI405FUNDAMENTALSOFARTIFICIALINTELLIGENCE/assets/87870499/8d5e329a-9aff-41a6-bcf0-46efa10e1b92)
![Sample 2](https://github.com/natsaravanan/19AI405FUNDAMENTALSOFARTIFICIALINTELLIGENCE/assets/87870499/438b242d-54ba-443e-b040-a936e6ae3b55)
![Sample 3](https://github.com/natsaravanan/19AI405FUNDAMENTALSOFARTIFICIALINTELLIGENCE/assets/87870499/99a33390-fa11-4ade-a19f-e93bcd7aaec9)
![Sample 4](https://github.com/natsaravanan/19AI405FUNDAMENTALSOFARTIFICIALINTELLIGENCE/assets/87870499/440797bd-53cb-49c1-b18d-89776864c3e7)
![Sample 5](https://github.com/natsaravanan/19AI405FUNDAMENTALSOFARTIFICIALINTELLIGENCE/assets/87870499/81575a16-26b2-46f1-a8ac-27c9ed0a0fe5)

## Output

![Screenshot 2024-09-26 092019](https://github.com/user-attachments/assets/57a87712-581d-47c2-bec6-fbdc96427dbc)
<hr>

## RESULT:
We have successfully implemented Alpha-beta pruning of Minimax Search Algorithm for a Simple TIC-TAC-TOE game
