>a type of [[dynamic-programming]]

[Good Explanation](https://medium.com/@rsinghal757/kadanes-algorithm-dynamic-programming-how-and-why-does-it-work-3fd8849ed73d)

>local_maximum[i] = max(A[i], A[i] + local_maximum(i - 1))

find largest subarray sum from array
```go
func solution(arr []int) int {
	gMax, lMax := 0, 0

	for i := range {
		lMax = math.Max(arr[i], arr[i] + lMax)
		if lMax > gMax {
			gMax = lMax
		}
	}

	return gMax
}
```
