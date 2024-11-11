import heapq

class PuzzleNode:
    def __init__(self, puzzle_state, parent=None, move=None):
        self.puzzle_state = puzzle_state
        self.parent = parent
        self.move = move
        self.depth = 0
        if parent:
            self.depth = parent.depth + 1

    def __lt__(self, other):
        return (self.depth + self.heuristic()) < (other.depth + other.heuristic())

    def __eq__(self, other):
        return self.puzzle_state == other.puzzle_state

    def get_blank_position(self):
        for i in range(3):
            for j in range(3):
                if self.puzzle_state[i][j] == 0:
                    return i, j

    def get_possible_moves(self):
        moves = []
        row, col = self.get_blank_position()

        if row > 0:
            moves.append((-1, 0))
        if row < 2:
            moves.append((1, 0))
        if col > 0:
            moves.append((0, -1))
        if col < 2:
            moves.append((0, 1))

        return moves

    def generate_child(self, move):
        row, col = self.get_blank_position()
        new_row = row + move[0]
        new_col = col + move[1]

        new_state = [list(row) for row in self.puzzle_state]
        new_state[row][col], new_state[new_row][new_col] = new_state[new_row][new_col], new_state[row][col]

        return PuzzleNode(new_state, self, move)

    def heuristic(self):
        distance = 0
        for i in range(3):
            for j in range(3):
                value = self.puzzle_state[i][j]
                if value != 0:
                    target_row = (value - 1) // 3
                    target_col = (value - 1) % 3
                    distance += abs(i - target_row) + abs(j - target_col)
        return distance

def solve_puzzle(initial_state):
    initial_node = PuzzleNode(initial_state)
    visited = set()
    frontier = []
    heapq.heappush(frontier, initial_node)

    while frontier:
        current_node = heapq.heappop(frontier)
        visited.add(tuple(map(tuple, current_node.puzzle_state)))

        if current_node.heuristic() == 0:
            path = []
            while current_node:
                path.append(current_node.puzzle_state)
                current_node = current_node.parent
            return path[::-1]

        possible_moves = current_node.get_possible_moves()
        for move in possible_moves:
            child = current_node.generate_child(move)
            if tuple(map(tuple, child.puzzle_state)) not in visited:
                heapq.heappush(frontier, child)

    return None

def print_solution(path):
    for i, state in enumerate(path):
        print(f"Move {i}:")
        for row in state:
            print(row)
        print()

if __name__ == "__main__":
    initial_state = [
        [1, 2, 3],
        [4, 0, 6],
        [7, 5, 8]
    ]
    solution = solve_puzzle(initial_state)
    if solution:
        print("Solution found!")
        print_solution(solution)
    else:
        print("No solution exists.")
