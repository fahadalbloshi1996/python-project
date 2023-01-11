# python-project.
#i want to share with you a project in python.
#its about how to do a game called [rock paper scissors] using classes.

from random import randint
"""This program plays a game of Rock, Paper, Scissors between two Players,
and reports both Player's scores each round."""

moves = ["rock", "paper", "scissors"]


class Player:
    """The Player class is the parent class for all the Players in this game.
    Simple Player class which always returns "rock"
    whenever move function is called"""

    def move(self):
        return "rock"

    def learn(self, my_move, their_move):
        pass


class RandomPlayer(Player):
    """Random Player class which return a random move from the list "moves"
    whenever move function is called"""

    def move(self):
        return moves[randint(0, 2)]


class HumanPlayer(Player):
    """Human Player class which asks user for an input
    and then returns that move"""

    def move(self):
        self.current_move = input("Enter your move: ")
        while self.current_move not in moves:
            print("Enter valid move! i.e 'rock', 'paper', 'scissors'")
            self.current_move = input("Enter your move: ")
        return self.current_move


class ReflectPlayer(Player):
    """Relfect Player class which initiates with a random move.
    It learns opponent's move and when next move function is called
    it returns the last move that opponent used"""

    def __init__(self):
        self.current_move = moves[randint(0, 2)]

    def move(self):
        return self.current_move

    def learn(self, my_move, their_move):
        self.current_move = their_move


class CounterPlayer(Player):
    """Counter Player class which initiates with a random move.
    When opponent chooses some move it learns it and next time
    uses it's counter when move function is called."""

    def __init__(self):
        self.current_move = moves[randint(0, 2)]

    def move(self):
        return self.current_move

    def learn(self, my_move, their_move):
        if their_move == "rock":
            self.current_move = "paper"
        elif their_move == "paper":
            self.current_move = "scissors"
        elif their_move == "scissors":
            self.current_move = "rock"


class CyclePlayer(Player):
    """Cycle Player class initiates with "rock" and
    cycles through the moves. It learns what it's move is
    and next time move function is called it returns next move"""

    def __init__(self):
        self.current_move = "rock"

    def move(self):
        return self.current_move

    def learn(self, my_move, their_move):
        if my_move == "rock":
            self.current_move = "paper"
        elif my_move == "paper":
            self.current_move = "scissors"
        elif my_move == "scissors":
            self.current_move = "rock"


def beats(one, two):
    """This functions checks which player has won the round.
    If both moves are same then it returns None"""
    if one == two:
        return None
    return ((one == "rock" and two == "scissors") or
            (one == "scissors" and two == "paper") or
            (one == "paper" and two == "rock"))


def winner(player_score1, player_score2, game):
    """This function checks which player has more score.
    The player with more score is considered winner.
    If score is same then the match is considered a draw"""
    if player_score1 > player_score2:
        print(f"Player 1 has won the {game}!")
    elif player_score1 < player_score2:
        print(f"Player 2 has won the {game}!")
    else:
        print(f"The {game} has ended in draw!")


def score_board(score1, score2):
    print(f"\nPlayer 1 Score: {score1}   Player 2 Score: {score2}")


class Game:
    """Game Class which initializes two players
    and their score board. It has two functions to play game.
    "play_game" allows the game to run one time.
    While "play_games" method asks user
    how many times you want to play."""

    def __init__(self, p1, p2):
        self.p1 = p1
        self.p2 = p2
        self.score1 = 0
        self.score2 = 0

    def play_round(self):
        move1 = self.p1.move()
        move2 = self.p2.move()
        print(f"Player 1: {move1}   Player 2: {move2}")
        # If player 1 beats player 2 increase score for player 1
        # and vice versa
        if beats(move1, move2):
            self.score1 += 1
        elif beats(move2, move1):
            self.score2 += 1
        score_board(self.score1, self.score2)
        winner(self.score1, self.score2, "round")
        self.p1.learn(move1, move2)
        self.p2.learn(move2, move1)

    def play_game(self):
        print("-----------------------------")
        print("Game Start!")
        print("Round 1:")
        self.play_round()
        score_board(self.score1, self.score2)
        winner(self.score1, self.score2, "game")
        print("Game Over!")
        print("-----------------------------\n\n")

    def play_games(self):
        # Continues to ask the number of rounds user wants to play
        # until user gives a correct integer
        while True:
            try:
                rounds = int(input("How many rounds do you want to play? "))
                break
            except ValueError:
                print("Enter proper integer")

        print("-----------------------------")
        print("Game Start!")
        for round in range(rounds):
            print(f"\nRound {round + 1}:")
            self.play_round()
        score_board(self.score1, self.score2)
        winner(self.score1, self.score2, "game")
        print("Game Over, Thank you for play with us!")
        print("-----------------------------\n\n")


if __name__ == "__main__":
    # you can change the play strategies from game = Game(here choose your strategie(), RandomPlayer())
    game = Game(HumanPlayer(), RandomPlayer())
    game.play_games()
