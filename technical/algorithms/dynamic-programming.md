## Example

- [how-to-solve-a-dynamic-programming-problem](https://www.codingninjas.com/studio/library/how-to-solve-a-dynamic-programming-problem)

>the idea is to solve a larger problem with a subset of the larger problem if it exists. ie; Fibonacci sequence where fib(n - 1) and fib(n - 2) are subsets of the problem

>dynamic programming relies on memoization where the result of the subset problem is stored in a variable

>bottom up approach is where the memoization part is done first

```c
#include <stdio.h>

#define MAX_SIZE 32

static int memo[MAX_SIZE];

int dynamic(int sum);

int main() {

	int result = dynamic(MAX_SIZE);

	printf("%d\n", result);

	return 0;
}

int dynamic(int sum) {

	// base case
	if (sum <= 0) {
		return sum;
	}

	// if calculated before then use the same result
	if (memo[sum - 1] != 0) {
		return memo[sum - 1];
	}

	// problem where the branching of result converges can be cached
	return memo[sum - 1] =
		dynamic(sum - 1) -
		dynamic(sum - 2) -
		dynamic(sum - 3);
}
```