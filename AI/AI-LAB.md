---
dg-publish: true
---
## AI LAB

### 1. Write a Python Primer Program for Lists and Tuples.
#### List
```python
my_list = [1, 2, 3, 4, 5]

print("First Element:", my_list[0]) # Accessing elements
print("Last Element:", my_list[-1])

print("Sliced List:", my_list[2:4]) # Slicing

my_list[0] = 10 # Modifying elements
my_list.append(6) # Appending elements

my_list.remove(3) # removing elements

print("Length of List:", len(my_list)) # len

my_list.insert(0, 0) # Appending elements

my_list.extend([8, 9]) # Extend

del my_list[6] # delete

my_list.sort() # sorting

my_list.reverse() # reverse

print('Length of my_list:', len(my_list))
print('Sum of elements in my_list:', sum(my_list))
print('Minimum element in my_list:', min(my_list))
print('Maximum element in my_list:', max(my_list))
print('Is 9 in my_list?', 9 in my_list)
```
>[!TIP] Do
>make sure to print the list after performing any operation
#### Tuples
>[!danger] Note
>there is no attribute to add or remove an element in tuples as they are Not Mutable
```py
print('\nTuple Demonstration:')
my_tuple = (1, 2, 3, 4, 5)

my_tuple += (6, 7) # Concatenation

# Accessing elements in reverse order
print('Last element of tuple (accessed in reverse order):', my_tuple[-1])

# Length of tuple
print(f'Length of Tuple: {len(my_tuple)}')

# Additional Tuple Demonstrations
print('\nAdditional Tuple Demonstrations:')

# Creating and printing a tuple
tup = (1, 4, 7, 8)

# Reassigning tuple elements and printing
tup = ("Quadeer", 4, 5, 6)

# Concatenating and printing a new tuple
tup_new = tup + ("ash", 17)

# Accessing elements by index
print('First element of tuple tup:', tup[0])
print('Second element of tuple tup:', tup[1])

# Nested tuples and accessing elements
x = (1, ("Quadeer", "ash"))
print('Accessing nested tuple element:', x[1][1])
print('Accessing element in tuple:', x[0])

# Nested tuples and accessing elements (more complex)
x = (1, ("Quadeer", "ash"), ("python", "master"))
print('Accessing nested tuple element:', x[2][1])
print('Accessing nested tuple element:', x[2][0])
```
### 2. Write a program to implement a family tree using Prolog.
```prolog
% Visual representation of the family tree
%                  pam   tom
%                  |     | 
%                ---   -----
%               |         |  |
%             bob      liz  ann
%               |         |
%          ------       ---
%         |      |     |   |
%        pat    peter jim  | 
%          |              |
%          jim           jim

```

```prolog
% Facts
female(pam).
female(liz).
female(pat).
female(ann).
male(jim).
male(bob).
male(tom).
male(peter).
parent(pam, bob).
parent(tom, bob).
parent(tom, liz).
parent(bob, ann).
parent(bob, pat).
parent(pat, jim).
parent(bob, peter).
parent(peter, jim).

% Rules
mother(X, Y):- parent(X, Y), female(X).
father(X, Y):- parent(X, Y), male(X).
haschild(X):- parent(X, _).
sister(X, Y):- parent(Z, X), parent(Z, Y), female(X), X \== Y.
brother(X, Y):- parent(Z, X), parent(Z, Y), male(X), X \== Y.

```
### 3. Write a Python Primer Program for Dictionaries and Sets.
#### Dictionaries
```py
print('\nDictionaries Demonstration:')
my_dict = {'a': 1, 'b': 2, 'c': 3}
print(f'demonstration of dictionary: {my_dict}')

my_dict['d'] = 4
print('dictionary after adding an element: ', my_dict)

del my_dict['c']
print('dictionary after deleting an element: ', my_dict)

print('length of dictionary: ', len(my_dict))

print(f'displaying Keys of dictionary: {my_dict.keys()}')
print(f'displaying Values of dictionary: {my_dict.values()}')

```
#### sets
```py
# Set Demonstration
print('\nSet Demonstration:')

# Creating a set
my_set = {1, 2, 3, 4, 5}
print(f'Demonstration of set: {my_set}')

# Adding an element to the set
my_set.add(6)
print('Set after adding an element:', my_set)

# Removing an element from the set
my_set.remove(3)
print('Set after removing an element:', my_set)

# Checking if an element is present in the set
print(f'Check if element 6 is present in the set or not: {6 in my_set}')

# Length of the set
print(f'Length of the set: {len(my_set)}')

# Intersection of sets
s1 = {'India', 'America', 'Mecca'}
s2 = {'USA', 'UK', 'India'}
s3 = s1 & s2  # or s3 = s1.intersection(s2)
print('Intersection of sets s1 and s2:', s3)

```
### 4. Program to implement informed Search technique for A* Algorithm.
```py
def aStarAlgo(start_node, stop_node):
    open_set = {start_node}
    closed_set = set()
    g = {}  # store distance from starting node
    parents = {}
    g[start_node] = 0
    parents[start_node] = start_node

    while len(open_set) > 0:
        n = None
        for v in open_set:
            if n is None or g[v] + heuristic(v) < g[n] + heuristic(n):
                n = v

        if n == stop_node or Graph_nodes[n] is None:
            pass
        else:
            for (m, weight) in get_neighbors(n):
                # nodes 'm' not in first and last set are added to first
                # n is set its parent
                if m not in open_set and m not in closed_set:
                    open_set.add(m)
                    parents[m] = n
                    g[m] = g[n] + weight
                # from start through n node
                else:
                    if g[m] > g[n] + weight:
                        # update g(m)
                        g[m] = g[n] + weight
                        # change parent of m to n
                        parents[m] = n
                        # if m in closed set,remove and add to open
                        if m in closed_set:
                            closed_set.remove(m)
                            open_set.add(m)

        if n is None:
            print('Path does not exist!')
            return None

        # if the current node is the stop_node
        # then we begin reconstruction the path from it to the start_node
        if n == stop_node:
            path = []
            while parents[n] != n:
                path.append(n)
                n = parents[n]
            path.append(start_node)
            path.reverse()
            print('Path found: {}'.format(path))
            return path

        # remove n from the open_list, and add it to closed_list
        # because all of his neighbors were inspected
        open_set.remove(n)
        closed_set.add(n)
    print('Path does not exist!')
    return None

# define function to return neighbor and its dist from the passed node
def get_neighbors(v):
    if v in Graph_nodes:
        return Graph_nodes[v]
    else:
        return None

# for simplicity we'll consider heuristic distances given
# and this function returns heuristic distance for all nodes
def heuristic(n):
    H_dist = {
        'A': 11,
        'B': 6,
        'C': 5,
        'D': 7,
        'E': 3,
        'F': 6,
        'G': 5,
        'H': 3,
        'I': 1,
        'J': 0
    }
    return H_dist[n]

# Describe your graph here
Graph_nodes = {
    'A': [('B', 6), ('F', 3)],
    'B': [('A', 6), ('C', 3), ('D', 2)],
    'C': [('B', 3), ('D', 1), ('E', 5)],
    'D': [('B', 2), ('C', 1), ('E', 8)],
    'E': [('C', 5), ('D', 8), ('I', 5), ('J', 5)],
    'F': [('A', 3), ('G', 1), ('H', 7)],
    'G': [('F', 1), ('I', 3)],
    'H': [('F', 7), ('I', 2)],
    'I': [('E', 5), ('G', 3), ('H', 2), ('J', 3)],
}

# Call the function to find the path
aStarAlgo('A', 'J')
```

### 5.Program to implement Uninformed Search technique for BFS technique.
```py
graph = {
    '5': ['3', '7'],
    '3': ['2', '4'],
    '7': ['8'],
    '2': [],
    '4': ['8'],
    '8': []
}

def bfs(graph, initial):
    visited = []
    queue = [initial]

    while queue:
        node = queue.pop(0)
        if node not in visited:
            visited.append(node)
            neighbours = graph[node]
            for k in neighbours:
                queue.append(k)
    return visited

print(bfs(graph, '5'))

```
### 6. Write Simple facts for the statement and query it using Prolog.

**Knowledge base:**
```prolog
father_of(joe, paul).
father_of(joe, mary).
mother_of(jane, paul).
mother_of(jane, mary).
male(paul).
male(joe).
female(mary).
female(jane).
```

**Output:**
```prolog
(10 ms) yes
| ?- father_of(joe,paul).
true ?
yes
| ?- father_of(paul,mary).
no
| ?- father_of(X,mary).
X = joe
yes
| ?-
```
### 7. Program to implement Uninformed Search technique for DFS technique.
```py
graph = {
    '5': ['3', '7'],
    '3': ['2', '4'],
    '7': ['8'],
    '2': [],
    '4': ['8'],
    '8': []
}

def dfs(graph, initial):
    visited = []
    stack = [initial]
    
    while stack:
        node = stack.pop()
        if node not in visited:
            visited.append(node)
            neighbours = graph[node]

            for k in neighbours:
                stack.append(k)
                
    return visited

print(dfs(graph, '5'))
```

### 8. Write a program for Implementing Stemming and Lemmatization Using NLTK.
#### Stemming
```py
# Install nltk library
%pip install nltk

# Import nltk library
import nltk
# Download all nltk resources
nltk.download('all')

# Import PorterStemmer from nltk.stem
from nltk.stem import PorterStemmer

# Create a PorterStemmer object
porter = PorterStemmer()

# Perform stemming on various words
print(porter.stem("play"))
print(porter.stem("plays"))
print(porter.stem("playing"))
print(porter.stem("played"))
```
>[!TIP]- explanation
>Certainly! Let's go through the code step by step:
>1. `pip install nltk`: This line installs the Natural Language Toolkit (NLTK) library using the pip package manager. NLTK is a popular library in Python for natural language processing tasks.
2. `import nltk`: This line imports the nltk library, allowing us to use its functionalities in our code.
3. `nltk.download('all')`: This line downloads all available resources from NLTK. These resources include datasets, corpora, trained models, and other necessary files for various natural language processing tasks. Note that downloading all resources may take some time and disk space.
4. `from nltk.stem import PorterStemmer`: This line imports the `PorterStemmer` class from the `nltk.stem` module. The `PorterStemmer` class is used for stemming words based on the Porter stemming algorithm, which is a widely used stemming algorithm in NLP.
5. `porter = PorterStemmer()`: This line creates an instance of the `PorterStemmer` class and assigns it to the variable `porter`. This instance will be used to perform stemming operations on words.
6. `print(porter.stem("play"))`: This line calls the `stem()` method of the `PorterStemmer` object `porter` and passes the word "play" as an argument. The `stem()` method applies the stemming algorithm to the word and returns its stem. In this case, the stem of "play" is "play" itself, as "play" is already in its base form.
7. Similarly, the next three `print()` statements perform stemming on the words "plays", "playing", and "played" using the `PorterStemmer` object and print their stems.
8. The output of the code is:   ```
   play
   play
   play
   play   ```
   As you can see, all forms of the word ("plays", "playing", "played") are stemmed to the same base form "play" using the Porter stemming algorithm. This is because stemming reduces words to their root or base form, ignoring suffixes and prefixes.

#### Lemmatization
```py
from nltk.stem import WordNetLemmatizer

# Create a WordNetLemmatizer object
lemmatizer = WordNetLemmatizer()

# Perform lemmatization on various words
print(lemmatizer.lemmatize("plays", 'v'))
print(lemmatizer.lemmatize("played", 'v'))
print(lemmatizer.lemmatize("play", 'v'))
print(lemmatizer.lemmatize("playing", 'v'))
```

>[!tip] explanation- Lemmatization is the process of reducing words to their base or dictionary form (called lemma). Unlike stemming, which simply removes suffixes or prefixes from words, lemmatization considers the context and morphological analysis to produce the root form.

 ### 9. Program to implement informed Search technique for Greedy Best First Search.
 ```py
from queue import PriorityQueue

v = 14
graph = [[] for i in range(v)]

def best_first_search(actual_src, target, n):
    visited = [False] * n
    pq = PriorityQueue()
    pq.put((0, actual_src))
    visited[actual_src] = True
    
    while not pq.empty():
        u = pq.get()[1]
        print(u, end=" ")
        
        if u == target:
            break
        
        for v, c in graph[u]:
            if not visited[v]:
                visited[v] = True
                pq.put((c, v))
        
        print()

def addedge(x, y, cost):
    graph[x].append((y, cost))
    graph[y].append((x, cost))

addedge(0, 1, 3)
addedge(0, 2, 6)
addedge(0, 3, 5)
addedge(1, 4, 9)
addedge(1, 5, 8)
addedge(2, 6, 12)
addedge(2, 7, 14)
addedge(3, 8, 7)
addedge(8, 9, 5)
addedge(8, 10, 6)
addedge(9, 11, 1)
addedge(9, 12, 10)
addedge(9, 13, 2)

source = 0
target = 9
best_first_search(source, target, v)

```

### 10. Write a program for text processing using NLTK for Removing Stop Words.
```py
# Install nltk library
%pip install nltk

# Import nltk library
import nltk

# Download all nltk resources
nltk.download('all')

# Import necessary modules
from nltk.corpus import stopwords
from nltk.tokenize import word_tokenize

# Example sentence
example_sent = "This is a sample sentence showing off the stop words filtration."

# Get the set of English stopwords
stop_words = set(stopwords.words('english'))

# Tokenize the example sentence
word_tokens = word_tokenize(example_sent)

# Filter out stopwords using list comprehension with lowercase conversion
filtered_sentence = [w for w in word_tokens if not w.lower() in stop_words]

# Filter out stopwords without lowercase conversion
filtered_sentence = []
for w in word_tokens:
    if w not in stop_words:
        filtered_sentence.append(w)

# Print the original tokens and filtered sentence
print("Original Tokens:", word_tokens)
print("Filtered Sentence:", filtered_sentence)
```
### 11. Write a program to train and validate Decision Tree classifier using Scikit-learn.
```py
import numpy as np
import matplotlib.pyplot as plt
import pandas as pd
from sklearn.tree import DecisionTreeRegressor

# Define the dataset
dataset = np.array([
    ['famous engg works', 11155, 155000],
    ['asset flip', 11134, 33100],
    ['bmw', 555, 10340],
    ['mjcet', 1332, 10400],
    ['tax', 1141, 12000],
    ['jeee', 1411, 10040],
    ['yoyo', 111, 10200],
    [' flip', 1141, 1000],
    ['nessain', 1161, 10400],
    ['asset', 1611, 10040]
])

# Print the dataset
print(dataset)

# Extract features (x) and target (y) variables
x = dataset[:, 1:2].astype(int)
y = dataset[:, 2].astype(int)

# Create a DecisionTreeRegressor model
regressor = DecisionTreeRegressor(random_state=0)

# Fit the model to the data
regressor.fit(x, y)

# Predict the profit for a given cost
y_pred = regressor.predict([[111]])
print(y_pred)

# Generate a range of values for plotting
x_grid = np.arange(min(x), max(x), 0.01)
x_grid = x_grid.reshape((len(x_grid), 1))

# Plot the data and the regression line
plt.scatter(x, y, color='red')
plt.plot(x_grid, regressor.predict(x_grid), color='blue')
plt.title('Business Analytics')
plt.xlabel('Cost')
plt.ylabel('Profit')
plt.show()

```
### 12. Write a program for Identifying POS (Parts of Speech) Tagging Using NLTK.
```py
# Install nltk library 
%pip install nltk

# Import nltk library
import nltk

# Download all nltk resources
nltk.download('all')

# Import necessary modules
from nltk import word_tokenize, pos_tag

# Define the text
text = 'geeksforgeeks is a great learning platform'

# Tokenize the text
tokenized_text = word_tokenize(text)

# Perform Part-of-Speech (POS) tagging
tags = pos_tag(tokenized_text)

# Print the tags
print(tags)
```

### 13. Write a program to train and validate Multi-Layer Feed Forward Neural Network using Scikit-learn.
```py
import numpy as np
from sklearn.datasets import load_digits
from sklearn.model_selection import train_test_split
from sklearn.neural_network import MLPClassifier
from sklearn.metrics import accuracy_score, confusion_matrix

# Load the digits dataset
dataset = load_digits()

# Split the dataset into training and testing sets
x_train, x_test, y_train, y_test = train_test_split(dataset.data, dataset.target, test_size=0.2)

# Create and train the MLPClassifier
NN = MLPClassifier(hidden_layer_sizes=(10), max_iter=1000)
NN.fit(x_train, y_train)

# Make predictions on the test set
y_pred = NN.predict(x_test)

# Calculate the accuracy of the model
score = accuracy_score(y_test, y_pred) * 100
print('Accuracy for neural network is:', score)

# Visualize one of the digits
import matplotlib.pyplot as plt
plt.gray()
plt.matshow(dataset.images[1730])
plt.show()

# Print the corresponding data for the digit
print(dataset.data[1730])

```
### 14. Write Simple facts for the statement and query it using Prolog.
>[!Danger] Note
>refer Q.6