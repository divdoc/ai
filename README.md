
practical 1
dfs
import collections

def dfs(g, n, seen, d):
    if n not in seen:
        seen.append(n)
        for i in g[n]:
            if seen[-1] == d:
                break
            dfs(g, i, seen, d)
    return seen

graph = {
    'M': ['R', 'Q', 'N'],
    'N': ['M', 'Q', 'O'],
    'O': ['N', 'P'],
    'R': ['M'],
    'Q': ['M', 'N', 'P'],
    'P': ['O', 'Q']
}

print(dfs(graph, 'M', [], 'P'))
----------

# BFS

import collections

# Define visit function first so BFS can call it safely
def visit(n):
    print(n)

# BFS Traversal
def bfs(graph, root):
    seen, queue = set([root]), collections.deque([root])
    while queue:
        vertex = queue.popleft()
        visit(vertex)
        for node in graph[vertex]:
            if node not in seen:
                seen.add(node)
                queue.append(node)

# All Paths Generator
def all_path(st, end, gr):
    todo = [(st, [st])]
    while len(todo):
        node, path = todo.pop(0)
        for next_node in gr[node]:
            if next_node in path:
                continue
            elif next_node == end:
                yield path + [next_node]
            else:
                todo.append((next_node, path + [next_node]))

# BFS Shortest Path
def bfs_shortest_path(graph, source, destination):
    checked = []
    queue = [[source]]
    if source == destination:
        return "source is destination"
    while queue:
        path = queue.pop(0)
        node = path[-1]
        if node not in checked:
            neighbours = graph[node]
            for neighbour in neighbours:
                new_path = list(path)
                new_path.append(neighbour)
                queue.append(new_path)
                if neighbour == destination:
                    return new_path
            checked.append(node)
    return "path does not exist"

# Merged duplicate key 'C' to preserve 'G'
graph = {
    'A': ['B', 'D'],
    'B': ['C', 'F'],
    'C': ['E', 'G', 'H'],
    'E': ['G', 'F'],
    'D': ['F'],
    'H': ['A']
}

print("Graph Traversal:")
bfs(graph, 'A')

print("\nAll paths are:")
print([path for path in all_path('A', 'E', graph)])

print("\nShortest path of Graph is:")
print(bfs_shortest_path(graph, 'A', 'E'))
practical 2
n queen
def print_board(board):
    for row in board:
        print(" ".join(row))


def check_q(board, row, col, n):

    # Check column
    for i in range(row):
        if board[i][col] == "Q":
            return False

    # Check left diagonal
    i = row - 1
    j = col - 1
    while i >= 0 and j >= 0:
        if board[i][j] == "Q":
            return False
        i -= 1
        j -= 1

    # Check right diagonal
    i = row - 1
    j = col + 1
    while i >= 0 and j < n:
        if board[i][j] == "Q":
            return False
        i -= 1
        j += 1

    return True


def solve_queens(board, row, n):
    if row == n:
        print_board(board)
        return True

    for col in range(n):
        if check_q(board, row, col, n):
            board[row][col] = "Q"

            if solve_queens(board, row + 1, n):
                return True

            board[row][col] = "."

    return False


def queens():
    n = int(input("Enter value of N: "))

    board = []

    for i in range(n):
        row = []
        for j in range(n):
            row.append(".")
        board.append(row)

    if not solve_queens(board, 0, n):
        print("No sol found")


queens()
-------------
def tower_of_hanoi(n, source, auxiliary, destination):
    if n == 1:
        print("Move disk 1 from", source, "to", destination)
        return

    tower_of_hanoi(n - 1, source, destination, auxiliary)

    print("Move disk", n, "from", source, "to", destination)

    tower_of_hanoi(n - 1, auxiliary, source, destination)


n = int(input("Enter number of disks: "))

print("Steps to solve Tower of Hanoi:")

tower_of_hanoi(n, 'A', 'B', 'C')
----------
practical 3
Practical 3
Alpha beta climbing
tree = {
    'A' : ['B','C'],
    'B' : ['D','E'],
    'C' :  ['F','G'],
    'D' : [4,3],
    'E' : [6,2],
    'F' :[2,1],
    'G' : [9,5]
    }
def minimax_alpha_beta(node, depth,alpha, beta,max_player):
    if depth == 0:
        if node in tree:
            return tree[node][0] if max_player else tree[node][0]
        else:
            return node

    if max_player:
        value = float('-inf')
        for child in tree[node]:
            value = max(value, minimax_alpha_beta(child, depth - 1, alpha, beta,False))
            alpha = max(alpha, value)
            if beta<=alpha :
                print(f"Pruning branch at node {node}")
                break
        return value
    else:
        value = float('inf')
        for child in tree[node]:
            value =  min(value, minimax_alpha_beta(child, depth - 1,alpha,beta,True))
            beta = min(beta, value)
            if beta<=alpha:
                print(f"Pruning branch at node {node}")
                break
        return value

best_score = minimax_alpha_beta('A',3,float('-inf'),float('inf'),True)
print(f"The best score is: {best_score}")
                        
--------------------
Hill climbing
import random
distance = [
    [0,2,9,10],
    [2,0,6,4],
    [9,6,0,3],
    [10,4,3,0]
    ]
def get_cost(tour):
    cost = 0
    for i in range(len(tour)):
        cost +=distance[tour[i-1]][tour[i]]
        print("Cost of tour",cost)
    return cost
def get_neighbor(tour):
    a,b = random.sample(range(len(tour)),2)
    tour[a], tour[b] = tour[b], tour[a]
    return tour

def hill_climb():
    current = [0,1,2,3]
    random.shuffle(current)
    current_cost = get_cost(current)
    print("Starting tour:", current, "Cost:", current_cost)

    for i in range(10):
        neighbor = current[:]
        print("Current neighbor:", neighbor)
        neighbor = get_neighbor(neighbor)
        print("Connected neighbor", neighbor)
        neighbor_cost = get_cost(neighbor)

        if neighbor_cost < current_cost:
            current = neighbor
            print("Current neighbor", current)
            current_cost = neighbor_cost
            print("Current Cost", current_cost)
            print("Better tour found:", current, "Cost:", current_cost)
    return current, current_cost
best_tour, best_cost = hill_climb()
print("\nBest tour:", best_tour)
print("\nBest Cost:", best_cost)
--------
practical 4
greedy bfs
def print_board(board):
    for row in board:
        print(" ".join(row))


def check_q(board, row, col, n):

    # Check column
    for i in range(row):
        if board[i][col] == "Q":
            return False

    # Check left diagonal
    i = row - 1
    j = col - 1
    while i >= 0 and j >= 0:
        if board[i][j] == "Q":
            return False
        i -= 1
        j -= 1

    # Check right diagonal
    i = row - 1
    j = col + 1
    while i >= 0 and j < n:
        if board[i][j] == "Q":
            return False
        i -= 1
        j += 1

    return True


def solve_queens(board, row, n):
    if row == n:
        print_board(board)
        return True

    for col in range(n):
        if check_q(board, row, col, n):
            board[row][col] = "Q"

            if solve_queens(board, row + 1, n):
                return True

            board[row][col] = "."

    return False


def queens():
    n = int(input("Enter value of N: "))

    board = []

    for i in range(n):
        row = []
        for j in range(n):
            row.append(".")
        board.append(row)

    if not solve_queens(board, 0, n):
        print("No sol found")


queens()
------
a*
Practical 4
A* algortithm
# Graph structure with adjacency costs and heuristic h(n) values
# Format: "node": ({neighbor: edge_cost, ...}, heuristic_value)
graph = {
    "a": ({"b": 1, "d": 2, "e": 3}, 4),
    "b": ({"c": 2, "d": 3}, 3),
    "c": ({"f": 2}, 2),
    "d": ({"f": 2}, 3),
    "e": ({"d": 3, "f": 4}, 2),
    "f": ({}, 0)
}

def a_star(graph, prev, dst, path, q):
    print("Connected nodes of current node", prev, "with h(n) values:")
    
    # Explore neighbor nodes
    for n in graph[prev][0]:
        if n not in path:
            # Store heuristic value and edge cost
            q[n] = (graph[n][1], graph[prev][0][n])
            print(n, "->", q[n])
            
            # f(n) calculation printed for each branch
            add = sum(q[n])
            print("A* value for", n, "is:", add)

    while q:
        # Get node with the minimum heuristic/cost value
        mn = min(q, key=q.get)
        print("Taking minimum vertex:", mn)
        
        # Remove selected node from queue
        del q[mn]
        
        # Target reached
        if dst == mn:
            return path + [dst]
        
        # Recursive exploration
        new_path = a_star(graph, mn, dst, path + [mn], q)
        if new_path:
            return new_path

    return None

# --- Main Program Execution ---
source = input("Enter source vertex: ")
dest = input("Enter destination vertex: ")

# Execute search
path = a_star(graph, source, dest, [source], {})

if path:
    print("\nPath found:", path)
else:
    print("\nPath not found")
-----------------------------------
output
Enter source vertex: a
Enter destination vertex: f
Connected nodes of current node a with h(n) values:
b -> (3, 1)
A* value for b is: 4
d -> (3, 2)
A* value for d is: 5
e -> (2, 3)
A* value for e is: 5
Taking minimum vertex: e
Connected nodes of current node e with h(n) values:
d -> (3, 3)
A* value for d is: 6
f -> (0, 4)
A* value for f is: 4
Taking minimum vertex: f

Path found: ['a', 'e', 'f']

practical 5
practical 5
water jug problem
from collections import deque

def is_visited(state, visited):
    return state in visited

def water_jug_bfs():
    max_a, max_b = 5, 4
    visited = set()
    queue = deque()
    queue.append((0, 0))
    
    while queue:
        a, b = queue.popleft()
        if is_visited((a, b), visited):
            continue
        visited.add((a, b))
        print(f"Jug A: {a}L, Jug B: {b}L")
        
        if a == 2 or b == 2:
            print("Found a solution!")
            return
            
        possible_states = [
            (max_a, b),
            (a, max_b),
            (0, b),
            (a, 0),
            (a - min(a, max_b - b), b + min(a, max_b - b)),
            (a + min(b, max_a - a), b - min(b, max_a - a))
        ]
        
        for state in possible_states:
            if not is_visited(state, visited):
                queue.append(state)

water_jug_bfs()
-------------
travelling salesperson
from itertools import permutations

dist = [
    [0, 10, 15, 20],
    [10, 0, 35, 25],
    [15, 35, 0, 30],
    [20, 25, 30, 0]
]

n = len(dist)
cities = range(1, n)
min_distance = float('inf')
best_path = None

for perm in permutations(cities):
    current_path = (0,) + perm + (0,)
    distance = 0
    for i in range(len(current_path) - 1):
        distance += dist[current_path[i]][current_path[i + 1]]
    
    if distance < min_distance:
        min_distance = distance
        best_path = current_path

print("Shortest distance : ", min_distance)
print("Best path: ", best_path)
-----
practical 6
[07/10, 00:09] Never give up: practical 6
missionaries and cnnonal
from collections import deque

moves = [(2, 0), (0, 2), (1, 1), (1, 0), (0, 1)]

def is_valid(m_left, c_left, m_right, c_right):
    if m_left < 0 or c_left < 0 or m_right < 0 or c_right < 0:
        return False
    if (m_left > 0 and m_left < c_left) or (m_right > 0 and m_right < c_right):
        return False
    return True

def solve():
    start = (3, 3, 1)
    goal = (0, 0, 0)
    queue = deque()
    queue.append((start, [start]))
    visited = set()

    while queue:
        (m_left, c_left, boat), path = queue.popleft()

        if (m_left, c_left, boat) in visited:
            continue
        visited.add((m_left, c_left, boat))

        if (m_left, c_left, boat) == goal:
            return path

        for m, c in moves:
            if boat == 1:
                new_m_left = m_left - m
                new_c_left = c_left - c
                new_boat = 0
            else:
                new_m_left = m_left + m
                new_c_left = c_left + c
                new_boat = 1

            new_m_right = 3 - new_m_left
            new_c_right = 3 - new_c_left

            if is_valid(new_m_left, new_c_left, new_m_right, new_c_right):
                new_state = (new_m_left, new_c_left, new_boat)
                if new_state not in visited:
                    queue.append((new_state, path + [new_state]))

    return None

steps = solve()

if steps:
    # Print steps up to step 7 (inclusive)
    for i in range(min(8, len(steps))):
        m, c, b = steps[i]
        side = "left" if b == 1 else "right"
        print(f"step {i} :- missioner left : {m}, cannibal left : {c}, boat on {side}")
else:
    print("no solution found")
Practical 6
Number puzzle
from collections import deque
goal = '123456780'
moves ={
    0 :[1,3],
    1:[0,2,4],
    2:[1,5],
    3:[0,4,6],
    4:[1,3,5,7],
    5:[2,4,8],
    6:[3,7],
    7:[4,6,8],
    8:[5,7]
}
def bfs(start):
    visited = set()
    queue = deque([(start,[])])

    while queue:
        state, path = queue.popleft()

        if state == goal :
            return path + [state]

        if state in visited:
            continue
        visited.add(state)
        zero = state.index('0')
        for move in moves[zero]:
            new_state = list(state)
            new_state[zero],new_state[move] = new_state[move],new_state[zero]
            queue.append((''.join(new_state), path + [state]))

    return None

start = '123405678'
solution = bfs(start)
if solution:
    print("Steps to solve: ")
    for s in solution:
        print(s[0:3])
        print(s[3:6])
        print(s[6:9])
        print("-----")
else:
    print("No solution found")

[07/10, 00:10] Divyansha yc yc: Practical 7
Tictactoe
board = ['   ' for _ in range(9)]

player = 'X'

def show_board():
    print(f"|{board[0]}|{board[1]}|{board[2]}|")
    print("-------------")
    print(f"|{board[3]}|{board[4]}|{board[5]}|")
    print("-------------")
    print(f"|{board[6]}|{board[7]}|{board[8]}|")


def is_winner(p):
    return (
        (board[0] == p and board[1] == p and board[2] == p) or
        (board[3] == p and board[4] == p and board[5] == p) or
        (board[6] == p and board[7] == p and board[8] == p) or
        (board[0] == p and board[3] == p and board[6] == p) or
        (board[1] == p and board[4] == p and board[7] == p) or
        (board[2] == p and board[5] == p and board[8] == p) or
        (board[0] == p and board[4] == p and board[8] == p) or
        (board[2] == p and board[4] == p and board[6] == p)
    )


def is_tie():
    return '   ' not in board


def game():
    global player

    while True:
        show_board()

        try:
            move = int(input(f"Player {player}, enter a position (0-8): "))

            if 0 <= move <= 8 and board[move] == '   ':
                board[move] = player

                if is_winner(player):
                    show_board()
                    print(f"Player {player} wins!")
                    break

                if is_tie():
                    show_board()
                    print("It's a tie!")
                    break

                # Switch player
                if player == 'X':
                    player = 'O'
                else:
                    player = 'X'

            else:
                print("Invalid move. Try again.")

        except ValueError:
            print("Please enter a number between 0 and 8.")


# Start the game
game()
-----------
Shuffle cards
import random

suits = ['Hearts', 'Diamonds', 'Clubs', 'Spades']
ranks = ['A', '2', '3', '4', '5', '6', '7', '8', '9', '10', 'J', 'Q', 'K']

deck = [rank + " of  " + suit for suit in suits for rank in ranks]


random.shuffle(deck)

print("\nShuffled deck of Cards")
for card in deck:
        print(card)
[07/10, 00:11] Divyansha yc yc: Practical 8
Constraint satisfaction problem 
import itertools

variables = ["A", "B", "C"]
colors = ["Red", "Blue", "yellow"]


all_assignments = itertools.product(colors, repeat=len(variables))


def valid(i):
    A, B, C = i
    return (A != B) and (B != C) and (A != C)

solutions = []

for i in all_assignments:
    if valid(i):
        solutions.append(dict(zip(variables, i)))

print("Valid colorings of the map:")
for sol in solutions:
    print(sol)
-----------------------------------
output
Valid colorings of the map:
{'A': 'Red', 'B': 'Blue', 'C': 'yellow'}
{'A': 'Red', 'B': 'yellow', 'C': 'Blue'}
{'A': 'Blue', 'B': 'Red', 'C': 'yellow'}
{'A': 'Blue', 'B': 'yellow', 'C': 'Red'}
{'A': 'yellow', 'B': 'Red', 'C': 'Blue'}
{'A': 'yellow', 'B': 'Blue', 'C': 'Red'}
-----
practical 9
[07/10, 00:10] Divyansha yc yc: Practical 9 
Associative law
a = int(input("Enter a: "))
b = int(input("Enter b: "))
c = int(input("Enter c: "))

lhs = (a+b)+c
rhs = a+(b+c)

if lhs == rhs:
    print("It is Associative law")
else:
    print("It is not")
Distributive law
a = int(input("enter a: "))
b = int(input("enter b: "))
c = int(input("enter c: "))

lhs = a*(b+c)
rhs = (a*b)+(a*c)

if lhs == rhs:
    print("it is distributive law")
else:
    print("it is not")
[07/10, 00:11] Divyansha yc yc: AI PRACTICAL  10 B WE HAVE TO PERFORM THIS IN SWI-PROLOG WHICH IS IN ITDSCA32 machine VM
1)---------GF
male(john).
male(mike).
male(david).
female(lisa).
female(susan).
female(anna).
parent(john,mike).
parent(john,lisa).
parent(susan,mike).
parent(susan,lisa).
parent(mike,david).
parent(anna,david).

father(F,C):- male(F),parent(F,C).
mother(M,C):- female(M),parent(M,C).

grandmother(GM,C):- female(GM), parent(GM,P),parent(P,C).
grandfather(GF,C):- male(GF), parent(GF,P),parent(P,C).

siblings(X,Y):- parent(P,X), parent(P,Y),X\=Y.
2)---------------ANCESTOR
male(john).
male(mike).
male(david).
female(lisa).
female(susan).
female(anna).
parent(john,mike).
parent(john,lisa).
parent(susan,mike).
parent(susan,lisa).
parent(mike,david).
parent(anna,david).

father(F,C):- male(F),parent(F,C).
mother(M,C):- female(M),parent(M,C).

grandmother(GM,C):- female(GM), parent(GM,P),parent(P,C).
grandfather(GF,C):- male(GF), parent(GF,P),parent(P,C).

siblings(X,Y):- parent(P,X), parent(P,Y),X\=Y.

ancestor(A,C) :- parent(A,C).
ancestor(A,C) :- parent(A,P), ancestor(P,C).
-----------------
10a
1)-------
batsman(sachin).
batsman(rohit).
batsman(dhoni).
batsman(virat).
cricketer(X):- batsman(X).
sportsman(X):-cricketer(X).
famous(X):-sportsman(X).
2)----------
teacher(anita).
teacher(raj).
teacher(meera).
teacher(rahul).
employee(X):- teacher(X).
human(X) :- employee(X).
being(X):- human(X).
3)---------
student(riya).
student(amit).
student(sam).
student(neha).
learner(X):- student(X).
knowledge_seeker(X):- learner(X).
future_professional(X):- knowledge_seeker(X).
4)-------
dog(champ).
dog(daisy).
dog(pluto).
dog(rocky).
animal(X) :- dog(X).
pet(X):- animal(X).
living_being(X) :- pet(X).
5)-----------
book(physics).
book(math).
book(history).
book(computers).
knowledge_source(X):- book(X).
educational_material(X):- knowledge_source(X).
valuable_resource(X) :- educational_material(X).
