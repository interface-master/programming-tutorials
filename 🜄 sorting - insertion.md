---
permalink: sorting/insertion/
---

## Insertion Sort

|                       |           |
| --------------------- | :-------: |
| Type:                 | Selection |
| Best Running Time:    |     n     |
| Average Running Time: |     n²    |
| Worst Running Time:   |     n²    |
| Memory consumption:   |     1     |

We will now look at the Insertion Sort algorith, which by the looks of the table above performs just as poorly as the Bubble Sort, however, in practice you will find this algorithm a bit more efficient. Also, similar to the Selection Sort, it is a good example for understanding the algorithm from a real-world application.

For example, when asked to sort a deck of cards you will take the next unsorted card and place it at the front of the stack. You will then take the next unsorted card and moving backwards through the sorted stack decide if the card should continue to move down, or stop at the current location. Once it finds it's home, you move onto the next unsorted card. In order to place the card you are looking backwards through a sorted list and terminating your search as soon as the right position is found, making this lookup less than the total length of the sorted list. And while Big O notation will have you seeing the worst case scenario, in practice this algorithm performs better than Selection Sort, and significantly better than Bubble Sort.

Here is how we would implement the Insertion Sort in basic JavaScript:

```javascript
function insertionSort(list) {
    // starting point
    var currentIndex = 1;
    while (currentIndex < list.length) {
        var thisIndex = currentIndex - 1;
        // go backwards over the sorted list starting from the current index
        // stop when this item is in the right spot
        while (thisIndex >= 0 && list[thisIndex] > list[thisIndex+1]) {
            var tmp = list[thisIndex];
            list[thisIndex] = list[thisIndex+1];
            list[thisIndex+1] = tmp;
            // move backwards through the sorted list
            thisIndex--;
        }
        // go to the next element in the list
        currentIndex++;
    }
    // return sorted list
    return list;
}
```

Once again we can observe the script in action by running:

```javascript
console.log("Sorted: ", insertionSort([7, 5, 9, 4, 8, 2, 1, 6, 3]));
```

We can add logging to the script to see how long the script takes and how many operations it performs.

```javascript
function insertionSort(list) {
    // starting point
    var start = new Date();
    var counterA = 0;
    var counterB = 0;
    var currentIndex = 1;
    while (currentIndex < list.length) {
        var thisIndex = currentIndex - 1;
        // loop over the sorted list starting from the current index
        // stop when this item is in the right spot
        while (thisIndex >= 0 && list[thisIndex] > list[thisIndex+1]) {
            counterA++;
            counterB++;
            var tmp = list[thisIndex];
            list[thisIndex] = list[thisIndex+1];
            list[thisIndex+1] = tmp;
            // move backwards through the sorted list
            thisIndex--;
        }
        counterA++;
        // go to the next element in the list
        currentIndex++;
    }
    // return sorted list
    var end = new Date();
    var time = end - start;
    console.log("Finished in ", time, "ms with ", counterA, " comparisons and ", counterB, " swaps");
    return list;
}
```

You'll notice both counters A and B are in the same spot because this algorithm only compares items it's going to swap, plus one extra comparison that terminates the inner while loop. We can once again run the script against these 3 data sets:

```javascript
console.log("Sorted: ", insertionSort([7, 5, 9, 4, 8, 2, 1, 6, 3]));
console.log("Sorted: ", insertionSort([9, 8, 7, 6, 5, 4, 3, 2, 1]));
console.log("Sorted: ", insertionSort([1, 2, 3, 4, 5, 6, 7, 8, 9]));
```

We can see in the first run **33** comparisons and **25** swaps; in the second run **44** comparisons and **36** swaps; and in the final run **8** comparisons and **0** swaps. Similar to the Selection Sort we have a high number of comparisons, and a significantly higher number of swaps since we swap each time the item moves backwards through the sorted list. However, overall we compare fewer times as we terminate the search once the item finds it's place in the sorted list.

We can validate this by comparing performance of the algorithms against a large dataset. This comparison can be found on [this page](../comparison/).

