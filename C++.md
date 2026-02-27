#programminglang 

---

``` cpp
#include <bits/stdc++.h>
//#include <iostream>
//#include <vector>
//#include <numeric>

int main()
{
	int t{};
	std::cin >> t;

	for (int i = 0; i < t; ++i)
	{
		int n{};
		std::cin >> n;

		std::vector<int> a(n);

		for (int j = 0; j < n; ++j)
		{
			std::cin >> a[j];
		}

		std::vector<bool> win(n, false);
	
		for (int first = 0; first < n; ++first)
		{
			std::vector<int> b = a;

			int total = accumulate(b.begin(), b.end(), 0);
			int i = first;
			int winner = -1;

			while (total > 0)
			{
				if (b[i] > 0)
				{
					b[i]--;
					total--;
					if (total == 0)
					{
						winner = i;
						break;
					}
				}
				i = (i + 1) % n;
			}
			if (winner != -1)
			{
				win[winner] = true;
			}
		}
		int answer = 0;
		for (bool f : win)
		{
			if (f) answer++;
		}
		std::cout << answer << std::endl;
	}

	return EXIT_SUCCESS;
}
```

