bubble sort
```C
#include <time.h>
#include <stdlib.h>
#include <stdio.h>

#define SIZE 100000

void bubblesort(int arr[], int size);

int main() {
    srand(time(NULL));

    int arr[SIZE];

    for (int i = 0; i < SIZE; i++) {
        int r = rand();
        arr[i] = r;
    }

    bubblesort(arr, sizeof(arr) / sizeof(*arr));

    return 0;
}

void bubblesort(int arr[], int size) {
    for (int i = 0; i < size; i++) {
        for (int j = 0; j < size; j++) {
            if (arr[i] < arr[j]) {
                int temp = arr[i];
                arr[i] = arr[j];
                arr[j] = temp;
            }
        }
    }
}
```

quick sort
```c
#include <time.h>
#include <stdlib.h>
#include <stdio.h>

#define SIZE 100000

void quicksort(int arr[], int lo, int hi);
int quicksort_helper(int arr[], int lo, int hi);

int main() {
    srand(time(NULL));

    int arr[SIZE];

    for (int i = 0; i < SIZE; i++) {
        int r = rand();
        arr[i] = r;
    }

    quicksort(arr, 0, sizeof(arr) / sizeof(*arr) - 1);

    return 0;
}

void quicksort(int arr[], int lo, int hi) {
    if (lo >= hi) {
        return;
    }

    const int pivotIdx = quicksort_helper(arr, lo, hi);

    quicksort(arr, lo, pivotIdx - 1);
    quicksort(arr, pivotIdx + 1, hi);
}

int quicksort_helper(int arr[], int lo, int hi) {
    const int pivot = arr[hi];

    int idx = lo - 1;

    for (int i = lo; i < hi; i++) {
        if (arr[i] <= pivot) {
            idx++;
            int temp = arr[i];
            arr[i] = arr[idx];
            arr[idx] = temp;
        }
    }

    idx++;
    arr[hi] = arr[idx];
    arr[idx] = pivot;

    return idx;
}
```