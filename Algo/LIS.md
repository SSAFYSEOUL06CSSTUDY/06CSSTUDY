# 최장 증가 수열 (LIS, Longest Increasing Subsequence) 알고리즘


최장 증가 부분 수열은 말 그대로 가장 길게 증가하는 부분 수열.
즉, 어떤 수열에서 만들 수 있는 부분 수열 중에서 가장 길면서 오름차순을 유지하는 수열을 LIS라고 한다. 
예시로, 수열 A = {10, 20, 10, 30, 20, 50}이 있다고 하면 해당 수열의 LIS는 {10, 20, 30, 50} 

풀이 방법 : 1. DP를 활용한 LIS 2. 이진탐색을 활용한 LIS

* DP는 단순하지만 시간복잡도가 O(n^2)을 가진다.
* 이진탐색을 활용하면 시간복잡도를 O(nlogn)을 가진다.

## 1. DP로 푸는 법 

<img width="662" alt="스크린샷 2023-12-12 오후 11 41 34" src="https://github.com/Youth787/SSAFY_CS_Study/assets/90955152/4a69279c-c481-4b10-9e8d-0ae50bfd7200">

    int N = 13;
		int[] arr = new int[N];
		int[] dp = new int[N];

		// dp로 풀 경우
		for (int i = 0; i < N; i++) {
			dp[i] = 1;
			for (int j = 0; j < i; j++) {
				if (arr[i] > arr[j]) {
					dp[i] = Math.max(dp[i], dp[j] + 1);
				}
			}
		}
