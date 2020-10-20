#  From Scratch: The Game of Life

[
  ](https://towardsdatascience.com/@moukarzeljoseph?source=post_page-----161430453ee3--------------------------------)

Hello everyone and welcome to the second article in the “From Scratch” series. (Previous one:[ From Scratch: Bayesian Inference, Markov Chain Monte Carlo and Metropolis Hastings, in python](https://towardsdatascience.com/from-scratch-bayesian-inference-markov-chain-monte-carlo-and-metropolis-hastings-in-python-ef21a29e25a))

In this article we explain and provide an implementation for “The Game of Life”. I say ‘we’ because this time I am joined by my friend and colleague [Michel Haber](https://medium.com/u/15ee6fa2e7df?source=post_page-----161430453ee3--------------------------------). The code is provided on both of our GitHub profiles: [Joseph94m](https://github.com/Joseph94m/The-game-of-life), [Michel-Haber](https://github.com/Michel-Haber/GoL-hs).

The first section will be focused on giving an explanation of the rules of the game as well as examples of how it is played/defined. The second section will provide the implementation details in Python and Haskell for the game of life. In the third section, we compare the performance and elegance of our implementations. We chose these two languages because they represent two of the most used programming paradigms, imperative (Python) and functional (Haskell), and because of the relative ease they provide in writing expressive and understandable code.

# 1-Introduction to the Game of Life

The game of life is a cellular automaton imagined by John H. Conway in the 1970s and is probably, the best known of all cellular automata. Despite very simple rules, the game of life is [Turing-complete](https://en.wikipedia.org/wiki/Turing_completeness) and deterministic.

The game of life is a game in the mathematical sense rather than a playable game. It is “zero player game”.

The game takes place on a two-dimensional finite or infinite grid whose cells can take two distinct states: “alive” or “dead”.

At each stage, the evolution of a cell is entirely determined by its current state and the state of its eight neighbours as follows:

> 1) A dead cell with exactly three living neighbours becomes alive.
> 2) A living cell with two or three living neighbours remains alive.
> 3) In all other cases, the cell becomes (or remains) dead.

Let’s assume a simple initial state where only 3 cells are alive: *left cell, middle cell, and right cell*.



![Image for post](https://miro.medium.com/max/645/1*YTPfWUa_7MDVwJOwK3gVhA.png)

Starting from the simple configuration in Figure 1, and letting the simulation run for one iteration results in Figure 2.

So how does this happen?



![Image for post](https://miro.medium.com/max/636/1*t-eEotQXTiFY1zYN9Gu99Q.png)

Figure 2

The cells on the left and on the right have only one neighbour, the middle cell, so they die. The middle cell has two neighbours, left and right, so it stays alive. The top and bottom cells have 3 neighbours (middle, right and left) so they become alive. It is really important to note that a cell does not die or live until the end of the iteration. I.e. the algorithm decides which cells are going to die or come to life and then gives them the news all together. This ensures that the order in which cells are evaluated does not matter.

Running one more iteration gives us the state represented in Figure 1(initial state).

As such, starting with this configuration, the game enters the infinite loop represented in Figure 3. This configuration is called a *blinker.*



![Image for post](https://miro.medium.com/max/625/1*YPE9QNuMItF9CUsu4Ogs_A.gif)

Figure 3: Blinker



![Image for post](https://miro.medium.com/max/555/1*SPREh_6fWZGCqJtNAAMpIw.png)

Figure 4: An early state from figure 5 that shows some interesting structures.

Now for a more interesting setup to show what impressed Conway the most. Starting from the simple configuration in the left image of Figure 5 and letting the code run, the board evolves into something completely unexpected. We can observe stable structures: *blocks* and *beehives.* We can also observe looping structures such as the *blinkers* from Figure 3 and the larger blinkers that are made up of 3 or 4 smaller ones. Another interesting structure observed in this simulation is the glider. A [*glider*](https://upload.wikimedia.org/wikipedia/commons/f/f2/Game_of_life_animated_glider.gif) looks like an old video game spaceship. It is unbounded by space, and keeps on going — *Ad vitam æternam*.



![Image for post](https://miro.medium.com/max/600/1*h3kQedlclM8SIvFB-WkoNg.gif)

![Image for post](https://miro.medium.com/freeze/max/60/1*5sx95AWKhkX8tMjpkUNtFA.gif?q=20)



Figure 5. Left: Initial state. Right: Evolution

# 2-Coding the Game of Life

We implement the game in two languages, Python and Haskell. Our main purpose for implementing the game in these two languages, is to compare their performances in terms of speed, as well as the codes’ elegance.

## 2.1-In Python

In Figure 6, we define the **Game** class. It is made up from a set of *rules*, an *initial state* and a *maximum size* (width and height). The game is started by calling the run_game function and specifying the maximum number of iterations.

<iframe src="https://towardsdatascience.com/media/6da244458f663a8099a6b267ed813cf6" allowfullscreen="" frameborder="0" height="406" width="680" title="Class of the game of life" class="t u v hb aj" scrolling="auto" style="box-sizing: inherit; position: absolute; top: 0px; left: 0px; width: 680px; height: 406px;"></iframe>

```python
class Game:
    def __init__(self, initial_state, rules,max_size):
        self.initial_state = initial_state
        self.rules = rules
        self.max_size = max_size
    def run_game(self, it):
        state = self.initial_state
        previous_state = None
        progression = []
        i = 0
        while (not state.equals(previous_state) and i < it):
            i += 1
            previous_state = state.copy()
            progression.append(previous_state.grid)
            state = state.apply_rules(self.rules,self.max_size)
        progression.append(state.grid)
        return progression
```

Figure 6: Game Class

There are two possible ways to represent a state:

> A dense representation in which all the cells are encoded in a width*height matrix. The dead cells are represented by a zero (or false) and the alive cells are represented by a 1 (or true). The position of a cell on the grid is defined by its position in the matrix.
>
> A sparse representation in which only living cells are encoded. Each living cell is represented by its position (int, int) on the grid. All these living cells are aggregated in a dictionary structure.

In our case, a sparse representation has the advantage, seeing as it requires less memory (since most cells will be dead) and allows us to iterate through the set of living cells only (since dead cells do not have direct effect on their neighbours).

In any case, we implemented both representations so as to provide a performance baseline. To avoid redundancy, however, we only show the implementation for the sparse representation in this article.

Figure 7 shows the implementation of the sparse representation. It is made up of a single attribute, the *grid*, which records the positions of living cells in a set.

<iframe src="https://towardsdatascience.com/media/1ca9848f613db3030e663310bf3ea8d6" allowfullscreen="" frameborder="0" height="855" width="680" title="Sparse representation of the game of life" class="t u v hb aj" scrolling="auto" style="box-sizing: inherit; position: absolute; top: 0px; left: 0px; width: 680px; height: 855px;"></iframe>

```python
class SparseSetState(State):
    def __init__(self, grid):
        self.grid = grid

    def copy(self):
        return SparseSetState(copy(self.grid))
    
    def get_neighbours(self, elem, max_size):
        #Returns the neighbours of a live cell if they lie within the bounds of the grid specified by max_size
        l = []
        if elem[0]-1 >= 0:
            l.append((elem[0]-1, elem[1]))
        if elem[0]-1 >= 0 and elem[1]-1 >= 0:
            l.append((elem[0]-1, elem[1]-1))
        if elem[0]-1 >= 0 and elem[1]+1 < max_size:
            l.append((elem[0]-1, elem[1]+1))
        if elem[1]-1 >= 0:
            l.append((elem[0], elem[1]-1))
        if elem[1]-1 >= 0 and elem[0]+1 < max_size:
            l.append((elem[0]+1, elem[1]-1))
        if elem[1]+1 < max_size:
            l.append((elem[0], elem[1]+1))
        if elem[0]+1 < max_size:
            l.append((elem[0]+1, elem[1]))
        if elem[1]+1 < max_size and elem[0]+1 < max_size:
            l.append((elem[0]+1, elem[1]+1))
        return l

    def equals(self, other):
        if other is None:
            return False
        return self.grid == other.grid

    def apply_rules(self, rules, max_size):
        #Calls the actual rules and provides them with the grid and the neighbour function
        self.grid = rules.apply_rules(self.grid, max_size,self.get_neighbours)
        return self
```

Figure 7: State Class

Figure 8 shows the rules of the game. Applying the rules on the grid of a state returns the grid with the updated dead & alive cells.

<iframe src="https://towardsdatascience.com/media/c0fb5e5ad9acd44acbbb19c456f54408" allowfullscreen="" frameborder="0" height="450" width="680" title="Sparse game of life rules" class="t u v hb aj" scrolling="auto" style="box-sizing: inherit; position: absolute; top: 0px; left: 0px; width: 680px; height: 449.987px;"></iframe>

```python
class SparseSetRules(Rule):
    def apply_rules(self, grid, max_size,get_neighbours):
        #grid = state.grid
        counter = {}
        for elem in grid:
            if elem not in counter:
                counter[elem]=0
            nb = get_neighbours(elem, max_size)
            for n in nb:
                if n not in counter:
                    counter[n] = 1
                else:
                    counter[n] += 1
        for c in counter:
            if (counter[c] < 2 or  counter[c] > 3) :
                grid.discard(c)
            if counter[c] == 3:
                grid.add(c)
        return grid
```

Figure 8: Sparse Rule class

```python
MAX_ITER = 1500
MAX_SIZE = 80
board = {(39, 40),(39, 41),(40, 39),(40, 40),(41, 40)}
rules = SparseSetRules()
game = Game(SparseSetState(board), rules,MAX_SIZE)
t = time.time()
rw = game.run_game(MAX_ITER)
```

Figure 9 shows an example on how to run the game with the configurations of Figure 5.

<iframe src="https://towardsdatascience.com/media/b64906b939e0c18fa8e9f311739a270f" allowfullscreen="" frameborder="0" height="212" width="680" title="Running sparse game of life example" class="t u v hb aj" scrolling="auto" style="box-sizing: inherit; position: absolute; top: 0px; left: 0px; width: 680px; height: 212px;"></iframe>

Figure 9: Example how to run the game

![Image for post](https://miro.medium.com/freeze/max/60/1*oEJIT5fcIb95WNH_ViJTxA.gif?q=20)

## 3.2-Programming elegance

Evaluating elegance of an implementation is a rather subjective task. We propose instead to show a few metrics for both implementations: effective lines of code, number of functions, and number of explicit conditional statements.

Code elegance, as such, relies heavily on personal favourites, we therefore leave the decision in the apt hands of the reader ;)

**Effective Lines of Code:**

> Dense Python representation: ~75
>
> Sparse Python representation: ~70
>
> Sparse Haskell representation: ~40

**Number of functions:**

> Dense Python representation: 6
>
> Sparse Python representation: 6
>
> Sparse Haskell representation: 7

**Number of explicit conditional statements:**

> Dense Python representation: 13
>
> Sparse Python representation: 11
>
> Sparse Haskell representation: 0

# 4-Conclusion

One Final simulation that we obviously did not do :). A Game of Life that simulates a Game of life.

It is interesting to see that such complex systems can emerge from such elementary rules and configurations — It kinds of reminds us of the real world, where elementary blocks congregate to generate much more complex structures such as *life*.

Concerning our two average(ish) implementations, the Haskell one outperforms the Python one in speed (and elegance?). That said, one can imagine some other Python and Haskell implementations that would result in a better performance and code elegance.

Finally, thanks for reading and don’t forget to try the code yourselves!

```python
class Game:
    def __init__(self, initial_state, rules,max_size):
        self.initial_state = initial_state
        self.rules = rules
        self.max_size = max_size
   
	def run_game(self, it):
        state = self.initial_state
        previous_state = None
        progression = []
        i = 0
        while (not state.equals(previous_state) and i < it):
            i += 1
            previous_state = state.copy()
            progression.append(previous_state.grid)
            state = state.apply_rules(self.rules,self.max_size)
        progression.append(state.grid)
        return progression

    
class SparseSetState(State):
    def __init__(self, grid):
        self.grid = grid

    def copy(self):
        return SparseSetState(copy(self.grid))
    
    def get_neighbours(self, elem, max_size):
        #Returns the neighbours of a live cell if they lie within the bounds of the grid specified by max_size
        l = []
        if elem[0]-1 >= 0:
            l.append((elem[0]-1, elem[1]))
        if elem[0]-1 >= 0 and elem[1]-1 >= 0:
            l.append((elem[0]-1, elem[1]-1))
        if elem[0]-1 >= 0 and elem[1]+1 < max_size:
            l.append((elem[0]-1, elem[1]+1))
        if elem[1]-1 >= 0:
            l.append((elem[0], elem[1]-1))
        if elem[1]-1 >= 0 and elem[0]+1 < max_size:
            l.append((elem[0]+1, elem[1]-1))
        if elem[1]+1 < max_size:
            l.append((elem[0], elem[1]+1))
        if elem[0]+1 < max_size:
            l.append((elem[0]+1, elem[1]))
        if elem[1]+1 < max_size and elem[0]+1 < max_size:
            l.append((elem[0]+1, elem[1]+1))
        return l

    def equals(self, other):
        if other is None:
            return False
        return self.grid == other.grid

    def apply_rules(self, rules, max_size):
        #Calls the actual rules and provides them with the grid and the neighbour function
        self.grid = rules.apply_rules(self.grid, max_size,self.get_neighbours)
        return self

    
class SparseSetRules(Rule):
    def apply_rules(self, grid, max_size,get_neighbours):
        #grid = state.grid
        counter = {}
        for elem in grid:
            if elem not in counter:
                counter[elem]=0
            nb = get_neighbours(elem, max_size)
            for n in nb:
                if n not in counter:
                    counter[n] = 1
                else:
                    counter[n] += 1
        for c in counter:
            if (counter[c] < 2 or  counter[c] > 3) :
                grid.discard(c)
            if counter[c] == 3:
                grid.add(c)
        return grid
    
    


def main():
    MAX_ITER = 1500
    MAX_SIZE = 80
    board = {(39, 40),(39, 41),(40, 39),(40, 40),(41, 40)}
    rules = SparseSetRules()
    game = Game(SparseSetState(board), rules, MAX_SIZE)
    t = time.time()
    rw = game.run_game(MAX_ITER)
    print(time.time()-t)


if __name__ = "__main__":
    main()
```

-----------------------------------------

another not using sparse matrix

# How to Write Conway’s “Game of Life” in Python

# A Brief Summary of the Game

You have a grid with cells. They can either spawn dead or alive.

The status of the neighbouring cells determines whether the cell dies, reproduces, or stays alive for the next generation.



![Image for post](https://miro.medium.com/max/343/1*qFxV4ZE9i_VxVbmv95ViqQ.png)

checking cell “E”. It has five dead neighbours (A,B,C,H,I) and three living ones (D,F,G)

If a cell is **alive** it will:

- Die if there are less than two living neighbours.
- Continue living if there are exactly two or three living neighbours.
- Die if there are more than three living neighbours.

If a cell is **dead** it will:

- Resurrect (become alive) if there are exactly three living neighbours.

In the example above, E would resurrect if it were dead — because it has three living neighbours — and stay alive if it were alive — because it has exactly two or three living neighbours

# Breaking Down the Python Part

The two biggest parts of this game are the *cells* and the *board*. We will make two classes for these: `cell.py` and `board.py` .

## cell.py

Let’s start with the cell. All cells will be dead initially. We will randomly generate the dead or alive status for the first generation. For the next generation, the rules above apply.

We also need the ability to check whether the cell is alive and we will create a method function to tell the board what to print, depending on the cell status. Here are the methods:

```python
#set status to dead
def set_dead(self):#set status to alive
def set_alive(self):#is the cell alive?
def is_alive(self):#what the board should print.
def get_print_character(self):
```

The cell is probably the easiest thing to write. Here’s the code:

```python
class Cell:
    def __init__(self):
        '''
        Class holding init status of cell (dead).
        Ability to set- and fetch new statuses with functions
        '''
        self._status = 'Dead'

    def set_dead(self):
        '''
        method sets the cell status to DEAD
        '''
        self._status = 'Dead'

    def set_alive(self):
        '''
        method sets the cell status to ALIVE
        '''
        self._status = 'Alive'

    def is_alive(self):
        '''
        method checks if the cell is ALIVE
        returns True if it is alive, False if not.
        '''
        if self._status == 'Alive':
            return True
        return False

    def get_print_character(self):
        '''
        method returning a status character of our choice to print on the board
        '''
        if self.is_alive():
            return 'O'
        return '*'
```

## board.py

This class is much harder to write and has several intricate solutions.

For the board we need:

- To define the rows and columns to set the size (constructor will handle this)
- A method to draw the board
- A method to check all the neighbours of all the cells
- A method which updates the board based on the neighbour statuses

```python
#initial generation based on randomness.
def _generate_board(self):#draw the board in the terminal
def draw_board(self):#update for the next generation of cells
def update_board(self):#find all the neighbours of a cell
def find_neighbour(self,row,column):
```

# Writing the Grid

The constructor takes care of creating all the variables we need. It also creates the grid list based on how many rows the user wants. Notice that the constructor also calls the generate method to generate the board right away.

```python
from cell import Cell
from random import randint

class Board:
    def __init__(self , rows , columns):
        self._rows = rows
        self._columns = columns   
        self._grid = [[Cell() for column_cells in range(self._columns)] for row_cells in range(self._rows)]

        self._generate_board()

    def draw_board(self):
        print('\n'*10)
        print('printing board')
        for row in self._grid:
            for column in row:
                print (column.get_print_character(),end='')
            print () # to create a new line pr. row.

    def _generate_board(self):
        for row in self._grid:
            for column in row:
                #there is a 33% chance the cells spawn alive.
                chance_number = randint(0,2)
                if chance_number == 1:
                    column.set_alive()
```

## generate_board(self)

This method gives each cell a 33% chance to spawn as a living cell.
The lucky cells will get a living status by using the cell method `.set_alive()`.

## draw_board(self)

First, it prints a number of rows to make sure we print a clean terminal. The number will vary depending on your setup.

Then we use a nested loop to check what kind of character we have to print for that square. We do that by using the cell method `.get_print_character()`.

## check_neighbour(self,check_row,check_column)

Before we actually solve this one, we need to take a look at how this is built and how we can check the neighbours. These are vital for the game and we ***must\*** nail them!

I’m going to write a test class called `Tablecheck` in `table_check.py` and a `main.py` to test the code with. This is so we can look at the theory together. The actual grid is made with list comprehension because it can be any size the user wants.



![Image for post](https://miro.medium.com/max/343/1*qFxV4ZE9i_VxVbmv95ViqQ.png)

building this grid in python

If we break down the grid, we can see that there are rows and columns. If we convert this to Python you can place lists within lists to mimic this behaviour. A,B,C is the first list (self._grid[0]), D,E,F is the second list (self._grid[1]) and G,H,I is the third list (self._grid[2]).

To access the letters inside these lists, we need to get into them. The syntax would be:

```python
self._grid[X][Y]
```

```
A lives in self._grid[0][0]
E lives in self._grid[1][1]
G lives in self._grid[2][0]
```

We can write the grid layout like this in Python:

```python
class Tablecheck:
    def __init__(self):
        self._grid = [['A','B','C'] ,
                      ['D','E','F'] ,
                      ['G','H','I'] ,
                      ['J','K','L']]
                      
    
    def print_grid(self):
        for row in self._grid:
            print (row)

```

`self._grid` is a list with three lists in it, creating a grid-like pattern. To draw a board like this I add this to my `main.py`:

```python
'''
Game of Life
Martin A. Aaberge
'''

from table_check import Tablecheck

table1 = Tablecheck()
table1.print_grid()
```

This prints the following:



![Image for post](https://miro.medium.com/max/991/1*47axXnglMF0Y1yr90jMKxA.png)



## check_neighbours(self,check_row,check_column)

OK, now on to what I believe is the trickiest part about the whole code: checking all the neighbours.

With `check_neighbours()` we have to pass in the coordinates we want to check and then we have to travel within our lists to check all the surrounding neighbours.

This means we have to jump one row back (-1), check our own row (0) and the next row (1). In Python terms that means going through a range from -1 to 2 , range(-1,2). We have to do the same with the columns.

If I want to stay true to my piece, [Document Your Python Code Without Writing Documentation](https://medium.com/better-programming/document-your-python-code-without-writing-documentation-2d8bf3a42e1a), I have to set start and end range as variables and make it clear how they can be affected.

```python
# how deep I will search around the current square.
search_min = -1
search_max = 2
```

Cycling through a range is easy, but you’re probably already wondering how we deal with going outside the board.


![Image for post](https://miro.medium.com/max/508/1*6J1lbH7jn4HmIW-VYi67aw.png)



*When we check “A”, we don’t want to waste time checking what is outside of the board. The same goes for the other corner elements.*



What we have to do here is to check if we’re working in any of the corners or any of the edges.

For the check itself, we need to cycle through the lists based on what the program puts into `check_row` and `check_column` (the parameters for `check_neighbour`).

We will loop through the range we have set, `search_min — search_max`. Since the range starts at -1 normally we add 1, so we check the correct rows. We want to jump 1 unit back, but we also want to work on the correct index. Lists in Python start at 0

The conditions in the loops are to ignore squares outside of the board and itself.

```python
def check_neighbour(self, check_row , check_column):
    #how deep the search is:
    search_min = -1
    search_max = 2

    #empty list to append neighbours into.
    neighbour_list = []
    for row in range(search_min,search_max):
        for column in range(search_min,search_max):
            neighbour_row = check_row + row
            neighbour_column = check_column + column 

            valid_neighbour = True

            if (neighbour_row) == check_row and (neighbour_column) == check_column:
                valid_neighbour = False

            if (neighbour_row) < 0 or (neighbour_row) >= self._rows:
                valid_neighbour = False

            if (neighbour_column) < 0 or (neighbour_column) >= self._columns:
                valid_neighbour = False

            if valid_neighbour:
                neighbour_list.append(self._grid[neighbour_row][neighbour_column])
    return neighbour_list  
```

Here’s what is going on in this function:

- `search depth`: I want to hop one row back, check my own row and hop one forward- as we’ve already discussed.
- I’ll create an empty list to place all the neighbours in. It’s important that we update all cells at once, so we can’t change the status on any cell before we’ve checked everything. This will be a temporary container of cells.
- We enter the row loop and loop through the columns in the row.
- `neighbour_row` and `neighbour_column` are variables that make sure we’re checking the correct rows and columns in the conditions.
- `valid_neighbour` is true by default. If a neighbour is inside the board and not itself, it is a valid neighbour. That is what all the if statements are for.
- When we are done checking, we return the list of valid neighbours.

## update_board(self)

Another hard function is the update board function. This cycles through all the cells, check the neighbours and, based on how many alive cells it finds, it stores the cell it’s currently checking in either a death list or a life list.

Then, after all cells are checked, they change status.

```python
def update_board(self):
    print ('updating board')
    #cells list for living cells to kill and cells to resurrect or keep alive
    goes_alive = []
    gets_killed = []

    for row in range(len(self._grid)):
        for column in range(len(self._grid[row])):
            
            #check neighbour pr. square:
            check_neighbour = self.check_neighbour(row , column)

            living_neighbours_count = []

            for neighbour_cell in check_neighbour:
                #check live status for neighbour_cell:
                if neighbour_cell.is_alive():
                    living_neighbours_count.append(neighbour_cell)

            cell_object = self._grid[row][column]
            status_main_cell = cell_object.is_alive()

            #If the cell is alive, check the neighbour status.
            if status_main_cell == True:
                if len(living_neighbours_count) < 2 or len(living_neighbours_count) > 3:
                    gets_killed.append(cell_object)

                if len(living_neighbours_count) == 3 or len(living_neighbours_count) == 2:
                    goes_alive.append(cell_object)

            else:
                if len(living_neighbours_count) == 3:
                    goes_alive.append(cell_object)

    #set cell statuses
    for cell_items in goes_alive:
        cell_items.set_alive()

    for cell_items in gets_killed:
        cell_items.set_dead()
```

- First, we create two empty lists to hold cells that are to be killed and cells that will survive or become alive.
- We go through the grid with a nested loop
- We use the `check_neighbour` function on the cell we’re checking out
- We create a list holding living cells from the check.
- We loop through the returned list from the function and see how many of the neighbours are alive
- Based on the rules of the game, the current cell is placed in a list to kill or a list to keep alive/become alive.

**Phew!** The hardest part is over — it’s time to sew it all together.



## main.py

```python
'''
Game of Life
Martin A. Aaberge
'''

from board import Board

def main():
    #assume the user types in a number
    user_rows = int(input('how many rows? '))
    user_columns = int(input('how many columns? '))

    # create a board:
    game_of_life_board = Board(user_rows,user_columns)

    #run the first iteration of the board:
    game_of_life_board.draw_board()
    
    user_action = ''
    while user_action != 'q':
        user_action = input('Press enter to add generation or q to quit:')

        if user_action == '':
            game_of_life_board.update_board()
            game_of_life_board.draw_board()


main()
```

We want the user to decide how many rows and columns we’re working with.

- import board
- ask for rows and columns
- create a board object with the user row and columns as arguments.
- `.draw_board()` to draw it in the terminal
- create a menu where the user can hit enter to create another generation or “q” to quit.

