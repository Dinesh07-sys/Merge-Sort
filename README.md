# Merge Sort Algorithm

Merge Sort approaches sorting from a different direction: instead of trying to place elements into their final positions immediately, it repeatedly breaks the problem into smaller pieces.

Once the individual sections are small enough, the algorithm rebuilds them in sorted order by comparing elements from two halves.

## Example

Given:

```text
5 2 8 11 3
```

The final sorted array is:

```text
2 3 5 8 11
```

## Implementation

```python
class Solution:
    def merge(self, arr, low, mid, high):
        temp = []
        left, right = low, mid + 1

        while left <= mid and right <= high:
            if arr[left] <= arr[right]:
                temp.append(arr[left])
                left += 1
            else:
                temp.append(arr[right])
                right += 1

        while left <= mid:
            temp.append(arr[left])
            left += 1

        while right <= high:
            temp.append(arr[right])
            right += 1

        for i in range(low, high + 1):
            arr[i] = temp[i - low]

    def mergeSort(self, arr, low, high):
        if low >= high:
            return

        mid = (low + high) // 2

        self.mergeSort(arr, low, mid)
        self.mergeSort(arr, mid + 1, high)

        self.merge(arr, low, mid, high)


arr = [5, 2, 8, 11, 3]

sol = Solution()
sol.mergeSort(arr, 0, len(arr) - 1)

print(*arr)
```

## The Divide-and-Conquer Idea

Merge Sort follows three fundamental stages:

```text
Divide
  ↓
Solve smaller sections
  ↓
Merge
```

For the array:

```text
5 2 8 11 3
```

the recursive division looks approximately like:

```text
             [5 2 8 11 3]
              /        \
          [5 2 8]      [11 3]
           /   \        /  \
        [5 2]  [8]    [11] [3]
         / \
       [5] [2]
```

The process continues until every section contains only one element.

A single element is already sorted relative to itself.

## Base Case

The recursive function stops when:

```python
if low >= high:
    return
```

This means the current section contains either one element or no elements.

There is nothing left to divide.

## Finding the Middle

The array section is divided using:

```python
mid = (low + high) // 2
```

This produces two ranges:

```text
low → mid
mid + 1 → high
```

The function then recursively sorts both:

```python
self.mergeSort(arr, low, mid)
self.mergeSort(arr, mid + 1, high)
```

Only after both sections have been processed does the merging stage begin.

## The Merge Operation

The `merge()` function receives two already-sorted sections:

```text
Left half  | Right half
```

Two pointers are maintained:

```python
left, right = low, mid + 1
```

The values pointed to by `left` and `right` are compared.

The smaller value is placed into:

```python
temp
```

This continues until one of the two sections is exhausted.

## Example of Merging

Suppose the two sorted sections are:

```text
[2 5]    [3 8]
 ↑         ↑
left      right
```

Compare:

```text
2 < 3
```

So `2` enters `temp`.

```text
temp = [2]
```

Next:

```text
5 > 3
```

So `3` is added.

```text
temp = [2, 3]
```

Then:

```text
5 < 8
```

giving:

```text
temp = [2, 3, 5]
```

Finally:

```text
8
```

is added.

Result:

```text
[2 3 5 8]
```

## Handling Remaining Elements

The main comparison loop ends when one half has no remaining elements:

```python
while left <= mid and right <= high:
```

The remaining values from either side are then copied using separate loops:

```python
while left <= mid:
    temp.append(arr[left])
    left += 1
```

and:

```python
while right <= high:
    temp.append(arr[right])
    right += 1
```

This is necessary because the remaining elements are already sorted within their respective halves.

## Copying Back to the Array

The merged values initially exist inside:

```python
temp
```

They are copied back into the original array:

```python
for i in range(low, high + 1):
    arr[i] = temp[i - low]
```

This replaces the current section with its newly sorted version.

## Complete Flow

For:

```text
5 2 8 11 3
```

the algorithm conceptually performs:

```text
                    Divide
                      ↓
             [5 2 8 11 3]
               /       \
          [5 2 8]     [11 3]
            ↓           ↓
        Smaller       Smaller
        divisions     divisions
            ↓           ↓
          Sorted       Sorted
          halves       halves
               \       /
                 Merge
                   ↓
            [2 3 5 8 11]
```

The important point is that **division and merging happen at different stages**.

## Complexity Analysis

| Case | Time Complexity |
|---|---|
| Best Case | `O(N log N)` |
| Average Case | `O(N log N)` |
| Worst Case | `O(N log N)` |

The recursive division creates approximately `log N` levels.

At each level, the elements are processed during merging, resulting in `O(N)` work per level.

Therefore:

```text
O(N) × O(log N)
= O(N log N)
```

### Auxiliary Space

```text
O(N)
```

The temporary list:

```python
temp = []
```

is used during the merge operation, requiring additional memory proportional to the number of elements being processed.

## Why Merge Sort Is Different

Selection Sort repeatedly searches for an element to place.

Insertion Sort repeatedly inserts an element into an already sorted section.

Merge Sort takes another route:

```text
Break the problem
       ↓
Solve smaller problems
       ↓
Combine their solutions
```

The sorted result emerges during the merging phase rather than through repeated selection or insertion.

## Key Takeaway

The strength of Merge Sort comes from separating a difficult task into manageable pieces.

```text
Large problem
     ↓
Smaller problems
     ↓
Small sorted sections
     ↓
Controlled merging
     ↓
Complete sorted array
```

The recursive division makes the problem smaller; the merge operation is responsible for putting those solutions back together correctly.
