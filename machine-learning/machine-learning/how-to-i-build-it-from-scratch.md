# How to I build it from scratch ?

###

### KNN Algorithm:

```python
def eucledian_distance(p, q):
    distance = 0.0
    for i in range(len(p)):
        distance += (p[i] - q[i]) ** 2
    return np.sqrt(distance)

def get_neighbours(train, test_row, num_neighbours):
    distance = []
    for train_row in train:
        dist = eucledian_distance(test_row, train_row[:-1])  # Exclude label
        distance.append((train_row, dist))
    distance.sort(key=lambda x: x[1])
    neighbours = [distance[i][0] for i in range(num_neighbours)]
    return neighbours

def prediction_for_classification(train, test_row, num_neighbours):
    neighbours = get_neighbours(train, test_row, num_neighbours)
    output_values = [row[-1] for row in neighbours]
    pred = max(set(output_values), key=output_values.count)
    return pred
    
dataset = np.array([
    [1.2, 3.0, 0],
    [2.1, 2.9, 0],
    [0.9, 2.5, 0],
    [2.5, 3.2, 0],
    [1.0, 1.8, 0],
    [1.6, 2.2, 0],
    
    [6.5, 5.0, 1],
    [7.8, 4.9, 1],
    [6.9, 6.2, 1],
    [7.2, 5.8, 1],
    [8.1, 6.3, 1],
    [6.4, 5.5, 1]
])

test_point = [5.5, 5.0]
k = 3

prediction = prediction_for_classification(dataset, test_point, k)

```

<figure><img src="../../.gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>

***

