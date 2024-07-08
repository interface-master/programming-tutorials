---
permalink: sorting/selection/
---

## Selection Sort

|                       |           |
| --------------------- | :-------: |
| Type:                 | Selection |
| Best Running Time:    |     n²    |
| Average Running Time: |     n²    |
| Worst Running Time:   |     n²    |
| Memory consumption:   |     1     |

We will now look at the Selection Sort algorithm, once again, for educational purposes only. As you can see from the table above this algorithm isn't much better than the Bubble Sort in terms of complexity. In fact the Best Running Time for the Selection Sort is worse than for the Bubble Sort.

So why are we looking at this algorithm? As the next natural step in progressing our understanding of algorithms, and as a way to relate to an intuitive way we may sort things in the real world.

For example, when presented with a random pile of coins and asked to sort them, some people might pluck out all the dollars first (as they are large, and easy to spot), then the quarters, as they are the next most visible coin, perhaps nickels or dimes next, and so on. Instead of picking up one random coin at a time, we choose to select the coins that we want by scanning the available dataset as that reduces the cognitive load of making a decision every time for which bucket the coin would go into.

Similarly, when presented with an unsorted list of data the Selection Sort would scan the whole list and identify the smallest (or largest) value, and place it at the front of the list. It would repeat this process until all values have been exhausted.

It is similar to the bubble sort in that we look over the entire list every time and determine the value to swap with the front of the list, however, in the Selection Sort the list of available items decreases with each iteration, therefore making our algorithm potentially faster than the Bubble Sort.

Here is how we could implement it in JavaScript:

```javascript
function selectionSort(list) {
    // starting point
    var currentIndex = 0;
    while (currentIndex < list.length-1) {
        // looking for global minima
        var minIndex = -1;
        // loop over the list starting from the current index
        for (var i = currentIndex; i < list.length; i++) {
            // if this item is smaller than the one at the minIdex, then
            if (minIndex < 0 || list[i] < list[minIndex]) {
                // remember this index
                minIndex = i;
            }
        }
        // if we found an element 
        if (minIndex > -1 && minIndex != currentIndex) {
            var tmp = list[currentIndex];
            list[currentIndex] = list[minIndex];
            list[minIndex] = tmp;
        }
        // go to the next element
        currentIndex++;
    }
    // return the sorted list
    return list;
}
```

You can test this script by running it in your browser or node with a simple array:

```javascript
console.log("Sorted: ", selectionSort([7, 5, 9, 4, 8, 2, 1, 6, 3]));
```

You should observe the output is a sorted list from 1 to 9.

Now, similar to the Bubble Sort you will notice that we didn't add much more code. Except in this algorithm instead of performing swaps as we go, we first find the item we want to swap, and then perform the operation. This reduces our list by one, and we start the loop over again. We can add log statements to the function to see how many lookup operations and swap operations the script performs.

```javascript
function selectionSort(list) {
    // starting point
    var currentIndex = 0;
    var start = new Date();
    var counterA = 0;
    var counterB = 0;
    while (currentIndex < list.length-1) {
        // looking for global minima
        var minIndex = -1;
        // loop over the list starting from the current index
        for (var i = currentIndex; i < list.length; i++) {
            counterA++;
            // if this item is smaller than the one at the minIdex, then
            if (minIndex < 0 || list[i] < list[minIndex]) {
                // remember this index
                minIndex = i;
            }
        }
        // if we found an element 
        if (minIndex > -1 && minIndex != currentIndex) {
            counterB++;
            var tmp = list[currentIndex];
            list[currentIndex] = list[minIndex];
            list[minIndex] = tmp;
        }
        // go to the next element
        currentIndex++;
    }
    // return the sorted list
    var end = new Date();
    var time = end - start;
    console.log("Finished in ", time, "ms with ", counterA, " comparisons and ", counterB, " swaps");
    return list;
}
```

Same as before, we can run this sort with the same three lists and note down the values of the counters:

```javascript
console.log("Sorted: ", selectionSort([7, 5, 9, 4, 8, 2, 1, 6, 3]));
console.log("Sorted: ", selectionSort([9, 8, 7, 6, 5, 4, 3, 2, 1]));
console.log("Sorted: ", selectionSort([1, 2, 3, 4, 5, 6, 7, 8, 9]));
```

The first run completed with **44** comparisons and **5** swaps; the second with **44** comparisons and **4** swaps; and the third with **44** comparisons and **0** swaps to sort the same 9 items. As you can see, regardless of the state of the data - we still had to run through the list a constant number of times to determine if it's sorted or not, the saving grace for the Selection Sort is that it had to perform significantly fewer swaps to achieve the same result. This means that in theory if a lookup operation is less expensive than the swap operation, this algorithm should perform better than the bubble sort, even though its complexity appears to be worse.

We can validate this by adding another tiny bit of code to both algorithms to time how long the process took, and comparing the results for different datasets. This comparison can be found on [this page](../comparison/).

