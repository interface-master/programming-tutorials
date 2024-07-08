---
permalink: sorting/quick/
---

## Quick Sort

|                       |              |
| --------------------- | :----------: |
| Type:                 | Partitioning |
| Best Running Time:    |    n log n   |
| Average Running Time: |    n log n   |
| Worst Running Time:   |       n²     |
| Memory consumption:   |     log n    |

The Quick Sort gets its name from the significant improvement on speed and complexity that it offers over its predecessors. It works by dividing the list into sub-lists by pivoting around the middle point of the array, and then sorting items into the two sub-arrays based on whether they're less than or greater than the pivot point.

An important thing to note here is that unlike the previous algorithms which only required one extra block of memory to hold a temporary value while swapping two elements, the Quick Sort does consume a bunch more memory as it creates sub-arrays of all the elements. These arrays are short-lived, but do require this memory to be available - so keep that in mind when considering this algorithm over the others.

Let's have a look at how we can implement this in basic JavaScript:

```javascript
function quickSort(list) {
    // begin by adding the whole list to be processed into the queue
    var queue = [];
    queue.push({ left: 0, right: list.length - 1})
    // process all arrays in the queue
    while (queue.length > 0) {
        // grab the next item to process
        var process = queue.shift();
        // peform the Quick Sort procedure
        var returnValue = quickSortProcedure(list.slice(process.left, process.right + 1));
        // swap items accordingly
        for (var i = process.left; i <= process.right; i++) {
            if (list[i] != returnValue.list[i - process.left]) {
                list[i] = returnValue.list[i - process.left];
            }
        }
        // add the sub-arrays to the processing queue
        if (returnValue.left != null && returnValue.left > 1) {
            queue.push({ left: process.left, right: process.left + returnValue.left});
        }
        if (returnValue.right != null && returnValue.right > 1) {
            if (returnValue.left != null) {
                queue.push({ left: process.left + returnValue.left + 1, right: process.left + returnValue.left + 1 + returnValue.right - 1 })
            } else {
                queue.push({ left: process.left + 1, right: process.left + 1 + returnValue.right - 1 });
            }
        }
    }
    // return sorted array
    return list;
}

// this is the procedure that splits the given array
// into three separate arrays around the pivot point
function quickSortProcedure(list) {
    // return if the array can't be split any further
    if (list.length <= 1) return list;
    // find midpoint
    var pivot = Math.floor(list.length / 2);
    var pivotVal = list[pivot];
    // split into three parts
    var listLeft = new Array();
    var listPivot = new Array();
    var listRight = new Array();
    for (var i = 0; i < list.length; i++) {
        if (i != pivot) {
            if (list[i] < pivotVal) {
                listLeft.push(list[i]);
            } else {
                listRight.push(list[i]);
            }
        } else {
            listPivot.push(list[i]);
        }
    }
    // merge back together
    var mergedList = listLeft.concat(listPivot).concat(listRight);
    var leftLen = listLeft.length > 0 ? listLeft.length : null;
    var rightLen = listRight.length > 0 ? listRight.length : null;
    // return
    return { list: mergedList, left: leftLen, right: rightLen };
}
```

While a traditional implementation of this might use recursion to process sub-arrays, this particular implementation uses a queue and loops over the queue until it is empty. One of the benefits of this approach is that it allows the queue to be processed in parallel. If we have multiple cores available we can send each chunk of the array in the queue to a different processing unit and perform the task in a fraction of the time.

Similarly as before we can enhance the function with observability by adding a timer and counters. Note, the counters are added outside the `quickSort` function as they are also used in the `quickSortProcedure`, so when testing this remember to reset the counters before each test:

```javascript
var counterA = 0;
var counterB = 0;
function quickSort(list) {
    // begin by adding the whole list to be processed into the queue
    var start = new Date();
    var queue = [];
    queue.push({ left: 0, right: list.length - 1})
    // process all arrays in the queue
    while (queue.length > 0) {
        // grab the next item to process
        var process = queue.shift();
        // peform the Quick Sort procedure
        var returnValue = quickSortProcedure(list.slice(process.left, process.right + 1));
        // swap items accordingly
        for (var i = process.left; i <= process.right; i++) {
            if (list[i] != returnValue.list[i - process.left]) {
                counterB++;
                list[i] = returnValue.list[i - process.left];
            }
        }
        // add the sub-arrays to the processing queue
        if (returnValue.left != null && returnValue.left > 1) {
            queue.push({ left: process.left, right: process.left + returnValue.left});
        }
        if (returnValue.right != null && returnValue.right > 1) {
            if (returnValue.left != null) {
                queue.push({ left: process.left + returnValue.left + 1, right: process.left + returnValue.left + 1 + returnValue.right - 1 })
            } else {
                queue.push({ left: process.left + 1, right: process.left + 1 + returnValue.right - 1 });
            }
        }
    }
    // return sorted array
    var end = new Date();
    var time = end - start;
    console.log("Finished in ", time, "ms with ", counterA, " comparisons and ", counterB, " swaps");

    return list;
}

function quickSortProcedure(list) {
    // return if the array can't be split any further
    if (list.length <= 1) return list;
    // find midpoint
    var pivot = Math.floor(list.length / 2);
    var pivotVal = list[pivot];
    // split into three parts
    var listLeft = new Array();
    var listPivot = new Array();
    var listRight = new Array();
    for (var i = 0; i < list.length; i++) {
        if (i != pivot) {
            if (list[i] < pivotVal) {
                counterA++;
                listLeft.push(list[i]);
            } else {
                counterA++;
                listRight.push(list[i]);
            }
        } else {
            counterA++;
            listPivot.push(list[i]);
        }
    }
    // merge back together
    var mergedList = listLeft.concat(listPivot).concat(listRight);
    var leftLen = listLeft.length > 0 ? listLeft.length : null;
    var rightLen = listRight.length > 0 ? listRight.length : null;
    // return
    return { list: mergedList, left: leftLen, right: rightLen };
}
```

We can run three quick tests using the small arrays:

```javascript
console.log("Sorted: ", selectionSort([7, 5, 9, 4, 8, 2, 1, 6, 3]));
console.log("Sorted: ", selectionSort([9, 8, 7, 6, 5, 4, 3, 2, 1]));
console.log("Sorted: ", selectionSort([1, 2, 3, 4, 5, 6, 7, 8, 9]));
```

We should observe **44** comparisons and **5** swaps for the first test; **44** comparisons and **4** swaps for the second test; and **44** comparisons with **0** swaps for the last test. The results are similar to the Selection Sort for this small dataset, however the speed of the Quick Sort is significantly faster.

We can analyze how this algorithm compares to the others using a larger dataset. This comparison can be found on [this page](../comparison/).
