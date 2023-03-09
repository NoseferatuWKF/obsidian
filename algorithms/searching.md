## Basics

Linear searching (O(N))
```cpp
bool needleInHaystack(vector<int> *haystack, int needle)
{
	for (int i = 0; i < haystack.size(); ++i)
	{
		if (haystack[i] == needle)
		{
			return true;
		}
	}
	return false;
}
```

Binary searching (O(logN))
>Binary here means it can only be two outcomes, so no need to 'scan' the whole input
```cpp
bool needleInHaystack(vector<int> *haystack, int needle)
{
	int low = 0;
	int high = haystack.size();
	do 
	{
		int middle = low + (high - low) / 2; // relative to high & low
		if (haystack[middle] == needle) 
		{
			return true;
		} else if (haystack[middle] > needle)
		{
			high = middle;
		} else
		{
			low = middle;
		}
	} while (low < high)
	return false;
}
```

## Case

2 crystal balls
```cpp
#include <cmath>
// O(sqrt(N)) solution
int twoCrystalBalls(vector<bool> breaks*)
{
	int jump = floor(sqrt(breaks.size()));
	int i = jump;
	for (; i < breaks.size(); i += jump)
	{
		if (breaks[i])
		{
			break;
		}
	}
	i -= jump; // return to last succesful test
	for (int j = 0; j < jump && i < breaks.size(); ++j, ++i)
	{
		if (breaks[i])
		{
			return i;
		}
	}
	return -1;
}
```