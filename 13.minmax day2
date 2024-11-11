class GameTree:
    def __init__(self, value=None, children=None):
        self.value = value
        self.children = children if children is not None else []

def minimax(node, depth, maximizing_player):
    if depth == 0 or not node.children:
        return node.value

    if maximizing_player:
        max_eval = float('-inf')
        for child in node.children:
            eval = minimax(child, depth - 1, False)
            max_eval = max(max_eval, eval)
        return max_eval
    else:
        min_eval = float('inf')
        for child in node.children:
            eval = minimax(child, depth - 1, True)
            min_eval = min(min_eval, eval)
        return min_eval


tree = GameTree(0, [
    GameTree(3, [GameTree(1), GameTree(2)]),
    GameTree(5, [GameTree(4)]),
    GameTree(6)
])

result = minimax(tree, 3, True)
print(f"The optimal value is: {result}")
