# DeepDoku

## What is this?
This project was made to see to what extent DeepLearning was able to learn the rules of sudoku puzzles and if it could predict the solution of a sudoku based on the initial puzzle. The machine learning library [TensorFlow](https://github.com/tensorflow/tensorflow) was utilized for this project and to improve the readability I used [Jupyter Notebook](https://github.com/jupyter/notebook). Thus, all the code is written in [Python](https://www.python.org/).

## 📊 Dataset
The dataset used for training and testing the model is available on [Kaggle](https://www.kaggle.com/datasets/bryanpark/sudoku) and contains 1 million valid sudokus puzzles and their corresponding solutions.

## ⚙️ Model and training
The model is composed of 3 convolution layers, 2 batch normalization layers and a Dense layer. The ReLU function is activated at each convolution layer since Sudokus are non-linear puzzles. We also activate to SoftMax function at the end to get a probability for each number in each box and we'll simply take the one that has the highest probability. After some hyperparameter optimization, the loss came down to 0.358. 


## 💡First approach
My first approach to this problem was simply to feed the dataset of sudoku to the model and see how well it would manage to predict the full solutions based on the full sudokus puzzles. However, the accuracy measured with this approach was terrible, the model wasn't able to solve a single sudoku of the test cases. But measuring the full sudoku may not be totally representative so I measured the accuracy for each initially empty box:
```
Total puzzle accuracy: 0.00%
Empty box accuracy: 69.19%
Global box accuracy: 82.12%
```
We can see that in 69.19% of the cases the model guessed the final number correctly for the empty boxes which result in a "Global box accuracy" of 82.12% (since all boxes aren't empty initially).

## 💡Second approach
Seeing these results I opted for **"a more human approach"**, when we solve sudokus we don't solve the entire grid at once, we solve it **box by box** and this is exactly what I experimented with the model. Since we output the probability for each number in each box, I took the number having highest probability among all empty boxes, updated the sudoku and fed it back to the model until all the boxes were filled. This approach led to far greater results with **100% accuracy** on the test batch. This is explained by the fact that the model gets more and more information and only selects the number that he's the most confident about.

Since we have a probability for each selected digit, I calculated the model's confidence by multiplying the probabilities of each selected digit. On the test batch the average confidence was **97.80%** which is really high. But what about some more complex sudokus, some **world class sudokus**?

## 🏆2017 World Sudoku Grand Prix final
For this example I selected a sudoku that was recommended by [Richard Stolk](https://logic-masters.de/Raetselportal/Benutzer/allgemein.php?name=Richard&chlang=en), a **world classed sudoku solver**. This sudoku was the 4th Sudoku of the [2017 World sudoku Grand Prix final](https://gp.worldpuzzle.org/content/final-results-5) and here's what our model predicts:

Sudoku puzzle:

<img src="https://raw.githubusercontent.com/Luzivog/DeepDoku/main/assets/sudoku_grand_prix.png" width="200" height="200" />

Model Output:

```
[[1. 5. 9. 2. 7. 6. 3. 4. 8.]
 [8. 3. 4. 9. 5. 1. 6. 7. 2.]
 [6. 7. 2. 4. 3. 8. 9. 5. 1.]
 [7. 2. 6. 8. 4. 3. 5. 1. 9.]
 [5. 9. 1. 6. 2. 7. 4. 8. 3.]
 [3. 4. 8. 1. 9. 5. 7. 2. 6.]
 [4. 8. 3. 5. 1. 9. 2. 6. 7.]
 [2. 6. 7. 3. 8. 4. 1. 9. 5.]
 [9. 1. 5. 7. 6. 2. 8. 3. 4.]] 

Model confidence: 14.88%
Valid: True
```

Eventhough the confidence of the model is low (14.88%) the **solution is correct**! Here the confidence indicates well the world class difficulty of this puzzle.

## Why not use a "classic" sudoku solver?
The point of DeepLearning is that the program doesn't have any information on the **rules or patterns** of the Sudoku game. It **learns** on it's own by **observing the data** and by trying to figure out some patterns between the input and the expected output. It is, in my opinion, a lot more interesting to make a computer learn something than to give him the answer right away. We saw here that the computer was able to learn the rules of sudoku puzzles in **less than an hour** (may fluctuate depending on your machine) and in a **few seconds** he solved a world class sudoku puzzle.