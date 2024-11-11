import itertools

def calculate_distance(matrix, path):
    distance = 0
    for i in range(len(path) - 1):
        distance += matrix[path[i]][path[i+1]]
    distance += matrix[path[-1]][path[0]]  # Return to the starting point
    return distance

def tsp_brute_force(matrix):
    cities = range(len(matrix))
    shortest_path = None
    min_distance = float('inf')
    
    for path in itertools.permutations(cities):
        current_distance = calculate_distance(matrix, path)
        if current_distance < min_distance:
            min_distance = current_distance
            shortest_path = path
    
    return shortest_path, min_distance

# Example usage:
distance_matrix = [
    [0, 10, 15, 20],
    [10, 0, 35, 25],
    [15, 35, 0, 30],
    [20, 25, 30, 0]
]

path, distance = tsp_brute_force(distance_matrix)
print(f"Shortest path: {path}")
print(f"Minimum distance: {distance}")
