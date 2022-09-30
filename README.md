# Chessboard_puzzle
# Putting knight to a random place on a chessboard then trying to build a path that knight can finish the board with touching a square only once
import random
import numpy as np
import pandas as pd

def jump_knight():
    row = 8
    column = 8
    table = [[0 for i in range(row)] for j in range(column)] #  -----------------> creating an 8x8 table with zeros

    a = random.randint(0,7) #  --------------------------------------------------> getting random positions for knight to start
    b = random.randint(0,7) #  --------------------------------------------------> getting random positions for knight to start
    knight_place = [a, b] #  ----------------------------------------------------> random positions of knight
    index_row = range(1,9) #  ---------------------------------------------------> rows of the chessboard
    index_column = ["a", "b", "c", "d", "e", "f", "g", "h"] #  ------------------> columns of the chessboard
    notasyon = [[0 for i in range(8)] for j in range(8)] #  ---------------------> notation table for moves

    for i in index_row:
        for j in index_row:
            notasyon[i-1][j-1] = index_column[j-1] + str(index_row[i-1])
    move_queue = []
    for i in range(1,65): #  ----------------------------------------------------> enumerating the moves
        move_queue.append(str(i) + ".")
    moves = [] #  ---------------------------------------------------------------> list to get moves that knight made
    move_indexes = [] #  --------------------------------------------------------> indexes of the moves that knight made
    count = 0 #  ----------------------------------------------------------------> counting the reversion of moves
    
    def create_empty_table(): #  ------------------------------------------------> resets the table
        global table
        row = 8
        column = 8
        table = [[0 for i in range(row)] for j in range(column)]
        
    def table_control(): #  -----------------------------------------------------> works untill the table is complete with ones
        t_f = []
        for i in range(8):
            for j in range(8):
                t_f.append(table[i][j])
        if 0 in t_f:
            return True
        else:
            return False
    while table_control() == True: #  -------------------------------------------> works as long as table has zeros
        table[a][b] = 1
        list_moves = [[a+2,b-1],[a+2,b+1],[a-2,b+1],[a-2,b-1],[a+1,b-2],[a+1,b+2],[a-1,b+2],[a-1,b-2]] # possible moves that knight can make
        possible_moves = []
        
        for i in list_moves: #  -------------------------------------------------> listing the possible moves that the knight can jump
            if 0<=i[0]<=7 and 0<=i[1]<=7 and table[i[0]][i[1]]==0:
                possible_moves.append(i)
        if len(possible_moves) > 1: #  ------------------------------------------> when there are more than 1 possible move
            index_move = random.randint(0, len(possible_moves)-1)
            x,y = possible_moves[index_move] #  ---------------------------------> index of the possible moves
            
        elif table_control() == True and len(possible_moves) == 0: #  -----------> when out of move
            print(len(moves))
            if table[7][7]==1 and table[0][7]==1 and table[7][0]==1 and table[0][0]==1 and count<=1: # revert the moves
                m, n = move_indexes[-1]
                if count == 0:
                    move_amount = len(moves)
                table[m][n] = 0
                moves.remove(moves[-1])
                move_indexes.remove(move_indexes[-1])
                x = m
                y = n
                count += 1
           
        else: #  -----------------------------------------------------------------> when only 1 possible move
            index_move = 0
            x,y = possible_moves[index_move]
            
        if table[x][y] == 0 and count<=1:
            table[x][y] = 1
            index = [x, y]
            a = x
            b = y
            move_indexes.append(index)
            moves.append(notasyon[a][b])
            if count 
        elif count>100: #  -------------------------------------------------------> if reversion of the move make the knight get stuck to the same squares
            for i in reversed(range(2, len(moves))): #  --------------------------> revert the moves
                m, n = move_indexes[i]
                table[m][n] = 0
                moves.remove(moves[i])
                move_indexes.remove(move_indexes[i])
                x = m
                y = n
            count = 0
            table[x][y] = 0
        elif table_control() == False and len(moves)==64:
            print("Success")
            print(table)
            for i in range(64): #  -----------------------------------------------> fills the notation table with the moves that been made
                move_queue[i] = str(move_queue[i]) + "Knight " + str(moves[i])
            print(move_queue)
            break
