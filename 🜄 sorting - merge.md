---
permalink: sorting/merge/
---

## Merge Sort

|                       |           |
| --------------------- | :-------: |
| Type:                 |  Merging  |
| Best Running Time:    |  n log n  |
| Average Running Time: |  n log n  |
| Worst Running Time:   |  n log n  |
| Memory consumption:   |     n     |

The Merge Sort is very similar to the Quick Sort in that it recursively divides the unsorted list in half and processes each part separately, making it ideal for parallelization. The one improvement over the Quick Sort is that as the arrays are merged back together they are compared with each other to place the items in the right order.

Here is a basic JavaScript implementation:

```javascript
function mergeSort(list, leftIndex = 0) {
    // terminates the recursion
    if (list.length <= 1) {
        return list;
    }

    // find midpoint
    var pivot = Math.floor(list.length / 2);

    // split into two halves
    var listLeft = list.slice(0, pivot);
    var listRight = list.slice(pivot);

    // recursively sort the halves
    var mergedList = merge(
        mergeSort(listLeft, leftIndex),
        mergeSort(listRight, leftIndex + pivot)
    )

    // return
    return mergedList;
}

function merge(left, right) {
    var merged = [];
    var leftIndex = 0;
    var rightIndex = 0;

    while (leftIndex < left.length && rightIndex < right.length) {
        if (left[leftIndex] < right[rightIndex]) {
            merged.push(left[leftIndex]);
            leftIndex++;
        } else {
            merged.push(right[rightIndex]);
            rightIndex++;
        }
    }

    merged.push(...left.slice(leftIndex));
    merged.push(...right.slice(rightIndex));

    return merged;
}
```

You can see how recursion is used to continuously divide the array into smaller and smaller pieces until they can not be split any further. Additionally when merging the arrays back together we pluck items from each half based on which item should be the next in sequence.

Once again let's add some observability and check out the results.

```javascript
function mergeSort(list, leftIndex = 0) {
    // terminates the recursion
    if (list.length <= 1) {
        return list;
    }

    // find midpoint
    var pivot = Math.floor(list.length / 2);

    // split into two halves
    var listLeft = list.slice(0, pivot);
    var listRight = list.slice(pivot);

    // recursively sort the halves
    var mergedList = merge(
        mergeSort(listLeft, leftIndex),
        mergeSort(listRight, leftIndex + pivot)
    )

    // return
    return mergedList;
}

function merge(left, right) {
    var merged = [];
    var leftIndex = 0;
    var rightIndex = 0;

    while (leftIndex < left.length && rightIndex < right.length) {
        if (left[leftIndex] < right[rightIndex]) {
            counterA++;
            merged.push(left[leftIndex]);
            leftIndex++;
        } else {
            counterA++;
            merged.push(right[rightIndex]);
            rightIndex++;
        }
    }

    merged.push(...left.slice(leftIndex));
    merged.push(...right.slice(rightIndex));

    return merged;
}
```

Because this is a recursive function it isn't possible to add logging to the `quickSort` method - instead we'll add the timer outside the function and measure how long it took to return the final result.

```javascript
var counterA = 0;
var start = new Date();
console.log("Sorted: ", mergeSort([7, 5, 9, 4, 8, 2, 1, 6, 3]));
// console.log("Sorted: ", mergeSort([9, 8, 7, 6, 5, 4, 3, 2, 1]));
// console.log("Sorted: ", mergeSort([1, 2, 3, 4, 5, 6, 7, 8, 9]));
var end = new Date();
var time = end - start;
console.log("Finished in ", time, "ms with ", counterA, " comparisons and swaps");
```

You will notice we only have one counter in this example. The algorithm doesn't perform a comparison task that is separate from the swapping task. As we make a comparison we pluck the elements from the relevant sub-arrays and put them in order, therefore we can assume that any work the script has to perform is counted when the elements are compared and placed into the sorted array.

Running the above code on the 3 different arrays (by uncommenting one-at-a-time) we should observe that the first one executed in **20** steps, the second in **16** steps, and the last in **13** steps. This is a significant improvement over the previous algorithms.

However, to really see the impact we can analyze how this algorithm performs with a larger dataset. This comparison can be found on [this page](../comparison/).
