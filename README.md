# domino-game
This is the domino game for internal programming contest :) with Vietnamese rule. The competitor should write algorithm to with games.


Domino Contest with Computer Algorithm:

Donino Rule:
- 28 domino tiles.
- Each player have 7 random tiles.
- First game with highest domino (6:6).
- Turn in counter clock direction.
- Place domino tiles in sequence.
- If player doesn't have suitable tile in their turn, the turn is passed to next player. 
- The player with a suitable tile might decide to give their turn to next player but if the rest player can not play or don't want to play, the player should place their tile.
- If a player place all of their tile, they will win. The score of player is decide by win order.
- First player have 3 point, second is 2 and third is 1. 
- If all playing players are unable to play next tile. Then the order is decide by total count point of titles. Who has the least point will win.
- If players have the same point. The order is decide by who is next in last turn.
- If the player hold just one double blank. It will be count as 13 point. Otherwise, count as zero.

Computer Game rule:
- Player clients will use Rest API with HTTP Long Polling to interact with the board server.
- Server shall randomly distribute tiles to players when the join the game.
- Server shall decide the turn and keep the turn accordingly time of player joined. 
- Server shall make sure players obey rule 
- Server shall decide who should play next and force if one player should place the tile (case that they can't pass).
- Server shall decide the winner and order in case of players can't move.
- Server shall keep the leaderboard for multiple games.
- Server shall receive the tile from player.
- Server shall return the board for players after each turn. 
- Server shall notify the player in their turn.
- Server shall check the connection of client and pause game in case of 1 client lost connection.

Json format:

Tile: [6,6], [6,5] ... [1,0], [0,0] (bigger value first)

Player tiles: [[6,6], [6,4], [5,3], [4,2], [4,1], [1,1], [1,0]]






Board: 
{
    "head": 6,
    "tail": 3,
    "sequence_tile_list": [
        [6, 0],
        [0, 5],
        [5, 6],
        [6, 6],
        [6, 3]
    ],
   "ordered_tile_list": [
        [6, 0],
        [5, 0],
        [6, 5],
        [6, 6],
        [6, 3]
    ]
}

Leaderboard:

{ 
  "current": [ {"player3":6}, {"player2":4}, {"player1": 2}, {"player4" :0}],
  "history": [
   [ {"player3":3}, {"player2":2}, {"player1": 1}, {"player4" :0}],
   [ {"player3":3}, {"player2":2}, {"player1": 1}, {"player4" :0}],
}


# Luan: Leaderboard should use simple data type, as follow:
players = [’player1’, ’player2’, ’player3’, ’player4’]

score_board = [  # playing history
    [3, 2, 1, 0], # player1
    [1, 2, 3, 3], # player2
    [1, 3, 3, 1], # player3
    [3, 1, 2, 2], # player4
]

# Then getting current score as below
# In this case, we need the order, so a list is a good choice

def get_current_scores():
    ’’’
    Return a list of scores with the highest on top
    Ie.. [{’player1’: 6}, {’player2’: 3},... ]
    ’’’
    scores_dict = {} # a dict of {3: ’player2’, 6: ’player1’,... }
    for i in range(len(players)):
        scores_dict[sum(score_board[i])] = players[i]

    return dict(
        [(scores_dict[i], i) for i in reversed(sorted(scores_dict.keys()))]
    )




API:

get - /board/join 
Desc: Join the game 
Parameters: name.
Return: of player tiles

get - /board/leaderboard
Desc: Get current leaderboard
Return: Current score of players and history of game.
