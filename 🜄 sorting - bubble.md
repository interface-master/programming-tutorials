---
permalink: sorting/bubble/
---

## Bubble Sort

|                       |            |
| --------------------- | :--------: |
| Type:                 | Exchanging |
| Best Running Time:    |      n     |
| Average Running Time: |      n²    |
| Worst Running Time:   |      n²    |
| Memory consumption:   |      1     |

We will start with the simplest sort to lay the groundwork for understanding sorting algorithms. The bubble sort is only here for educational purposes to better understand algorithms and should never be used in production. Its performance is terrible, and the code is too simple to be of any educational value. The only thing useful in this section is the concept of understanding algorithms.

The bubble sort algorithm works by looking through the list and comparing two neighbouring elements. If they are out of order, they are swapped. Once the algorithm reaches the end of the list, it starts again. Once it has made a single pass through the list without swapping any items - the list is sorted. Here it is illustrated in basic JavaScript:

```javascript
function bubbleSort(list) {
    // default value to kick off the loop
    var changed = true;
    // keep looping while any changes are made
    while (changed) {
        changed = false;
        // loop over all the elements in the list
        for (var i = 0; i < list.length - 1; i++) {
            // compare two items
            if (list[i] > list[i+1]) {
                // swap them
                var tmp = list[i];
                list[i] = list[i+1];
                list[i+1] = tmp;
                changed = true;
            }
        }
    }
    return list;
}
```

You can test this script by running it in your browser or node with a simple array:

```javascript
console.log("Sorted: ", bubbleSort([7, 5, 9, 4, 8, 2, 1, 6, 3]));
```

You should observe the output is a sorted list from 1 to 9.

As you can see, the code footprint is tiny, which makes it fairly straightforward to read and understand, and an ideal place to learn about algorithms. You should also be able to see that it requires minimal additional memory - only one extra unit to hold a temporary item as we swap two neighbours. If we are concerned about memory consumption but don't care about time or energy, then perhaps this type of sort might be appropriate _(hint: it probably isn't)_.

Now let's look at the complexity in terms of computational operations performed. You should be able to see now why the average running time of this algorithm is n² - as it will loop over the list again and again until no changes are made. In the above example, let's add some logging to the function to see how many times the loop runs:

```javascript
function bubbleSort(list) {
    // default value to kick off the loop
    var changed = true;
    var start = new Date();
    var counterA = 0;
    var counterB = 0;
    // keep looping while any changes are made
    while (changed) {
        changed = false;
        // loop over all the elements in the list
        for (var i = 0; i < list.length - 1; i++) {
            // compare two items
            counterA++;
            if (list[i] > list[i+1]) {
                // swap them
                counterB++;
                var tmp = list[i];
                list[i] = list[i+1];
                list[i+1] = tmp;
                changed = true;
            }
        }
    }
    // return the sorted list
    var end = new Date();
    var time = end - start;
    console.log("Finished in ", time, "ms with ", counterA, " comparisons and ", counterB, " swaps");
    return list;
}
```

Let's run the sort on 3 different arrays -- first one will be the same as before, the next one will be sorted in reverse order, and the last will be a sorted array. Let's observe how many times the algorithm compares and swaps elements:

```javascript
console.log("Sorted: ", bubbleSort([7, 5, 9, 4, 8, 2, 1, 6, 3]));
console.log("Sorted: ", bubbleSort([9, 8, 7, 6, 5, 4, 3, 2, 1]));
console.log("Sorted: ", bubbleSort([1, 2, 3, 4, 5, 6, 7, 8, 9]));
```

The first run performed **56** comparisons and **25** swaps; the second run performed **72** comparisons and **36** swaps; and finally the third run performed only **8** comparisons and **0** swaps to sort 9 items. As you can imagine, the larger the list and the more operations and swaps that will have to be performed - the longer the script will take.
