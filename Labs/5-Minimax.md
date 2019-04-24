The Subtraction Game
====================

Beginning with a pile of 21 stones, players alternate removing
stones until none are left. On her turn, a player may take 1, 2,
or 3 stones. The player who takes the last stone is the winner.

A version of this game was played on an episode of Survivor,
where they called it Thai 21.


Steps:

0. Play a few games with your partner.

1. Finish implementing the minimax algorithm using the code below as a starting point. You’ll need to fill in the
   stopping condition, the loop for the max player, and all
   the code for the min player.

2. Play a few rounds against the computer with a low depth
   limit, like 2. Can you win?
   
3. Change the game so that you get to move second. Can you win now?

3. Now increase the depth limit to 5 and then to 10. How does
   the computer’s play change?

4. What’s the winning strategy for this subtraction game?

5. Now increase the depth limit to 20. What happens?

## Tips

Start by looking at the main routine. You can choose whether the human player goes first or second (this turns out to be significant).

The `get_move` method prompts the human player to select 1, 2 or 3 stones. The computer player calls `minimax` to determine its move.

`minimax` takes three inputs:

- the number of stones
- the search depth
- `is_max_player`, a boolean that track whether the current player seeks to maximize or minimize the score

It returns two values:

- the best outcome that could be obtained basic on rational play from the given starting point and depth
- the top-level move that led to that best score

To fill in the minimax code, you need to think about two things:

- What is the ending condition? If a player calls `minimax` and there are no stones, that means the *previous player* must have taken the last stone: by the rules of the game that's a loss for the *previous player*.

- How to implement the recursive call to `minimax?`

## Code

```
# The Subtraction Game
# DSM, 2017

# Beginning with a pile of 21 stones, players alternate removing stones until
# none are left. On her turn, a player may take 1, 2, or 3 stones. The player
# who takes the last stone is the loser.

# Change this line to control who goes first
human_moves_first = True

def minimax(stones, depth, is_max_player):
    
    """Execute the minimax algorithm by exploring the subtree, identifying the
       move at each level that yields the best outcome for its player
       
       Returns: 
           the best score that the player can obtain in this subtree
           the move yielding that best score
    """
    
    # If there are no stones left, this player has won
    # If the max player has won, return 1 (best outcome for max)
    # If the min player has won, return -1 (best outcome for min)
    if stones <= 0:
        # Fill in with two cases for max and min players
        
    # If the depth limit has been reached, return 0
    if depth == 0:
        return 0, None
        
    if is_max_player:
        best_value = -10
        best_remove = None  # Best number of stones to take
        
        for remove in [1, 2, 3]:
        
            # if stones - remove < 0, continue
        
            # Fill in recursive call to minimax
            
            # Test if value returned by minimax is better than current best
                  
    if not is_max_player:
        
            # Fill in details of minimax algorithm for min player
        
        
    return best_value, best_remove
    

def get_move(stones):
    looping = True;
    
    while looping:
        looping = False
        
        print ('Remove 1, 2, or 3 stones: '),
        remove = int(input())
        
        if remove < 1 or remove > 3:
            print ('You can only take 1, 2, or 3.')
            looping = True
        
    return remove
    
    
def play():
    
    stones = 21
    
    playing = True
    turn = 0
    
    if human_moves_first:
        turn_index = 0
    else:
        turn_index = 1
    
    while playing:
        
        print ('')
        print (stones)
        
        # Human move
        if turn % 2 == turn_index:
            remove = get_move(stones)
            stones -= remove
        
        # Computer move
        else:
            value, remove = minimax(stones, 2, True)
            print ("I'll take " + str(remove))
            stones -= remove
            
        # The player who took the last stone is the loser
        if stones <= 0:
            if turn % 2 == turn_index:
                print ('\nWeep, soft human.')
            else:
                print ('\nMy...failure...DOES NOT COMPUTE.')

            playing = False    
            
        turn += 1

    
if __name__ == '__main__':
    play()
```
