# 최장 증가 수열 (LIS, Longest Increasing Subsequence) 알고리즘


최장 증가 부분 수열은 말 그대로 가장 길게 증가하는 부분 수열.
즉, 어떤 수열에서 만들 수 있는 부분 수열 중에서 가장 길면서 오름차순을 유지하는 수열을 LIS라고 한다. 
예시로, 수열 A = {10, 20, 10, 30, 20, 50}이 있다고 하면 해당 수열의 LIS는 {10, 20, 30, 50} 

[풀이 방법] 
1. DP를 활용한 LIS
2. 이진탐색을 활용한 LIS

* DP는 단순하지만 시간복잡도가 O(n^2)을 가진다.
* 이진탐색을 활용하면 시간복잡도를 O(nlogn)을 가진다.

## 1. DP로 푸는 법 

<img width="662" alt="스크린샷 2023-12-12 오후 11 41 34" src="https://github.com/Youth787/SSAFY_CS_Study/assets/90955152/4a69279c-c481-4b10-9e8d-0ae50bfd7200">
<br>

```java

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

```

 - 1에서 끝나는 LIS 길이 : 1 (1)
 - 5에서 끝나는 LIS 길이 : 2 (1, 5)
 - 4에서 끝나는 LIS 길이 : 2 (1, 4)
 - 2에서 끝나는 LIS 길이 : 2 (1, 2)
 - 3에서 끝나는 LIS 길이 : 3 (1, 2, 3)
   
이들 중 가장 긴 (1, 2, 3)에 현재 수 8을 더한 (1, 2, 3, 8)이 8에서 끝나는 최장 증가 수열이 된다. <br>
즉, 앞 순서의 모든 원소에서 끝나는 최장 증가 수열들의 길이 중 가장 긴 것을 골라 1을 더한 것이 곧 현재 수에서 끝나는 최장 증가 수열의 길이이다.<br>

<br>

## 2. 이진탐색으로 푸는 법 

<img width="600" alt="스크린샷 2023-12-13 오전 12 09 07" src="https://github.com/Youth787/SSAFY_CS_Study/assets/90955152/47c33333-b72e-47bd-891e-9c87787899ed">

<img width="600" alt="스크린샷 2023-12-13 오전 12 09 25" src="https://github.com/Youth787/SSAFY_CS_Study/assets/90955152/1cddad5f-d24c-40cc-abc0-4c38a55d3eac">

<img width="600" alt="스크린샷 2023-12-13 오전 12 09 39" src="https://github.com/Youth787/SSAFY_CS_Study/assets/90955152/d869c7c8-c3dc-4b1d-a35d-6dddda07eba6">

<img width="600" alt="스크린샷 2023-12-13 오전 12 09 55" src="https://github.com/Youth787/SSAFY_CS_Study/assets/90955152/e48a881e-a482-451b-bbd2-371d0f0306b2">

<img width="600" alt="스크린샷 2023-12-13 오전 12 10 15" src="https://github.com/Youth787/SSAFY_CS_Study/assets/90955152/58cd391e-f4ac-47e7-9d86-c10beae6af42">

<img width="600" alt="스크린샷 2023-12-13 오전 12 10 28" src="https://github.com/Youth787/SSAFY_CS_Study/assets/90955152/0a6799ce-d33e-4795-8b06-7c543c2de5f1">

<img width="600" alt="스크린샷 2023-12-13 오전 12 10 44" src="https://github.com/Youth787/SSAFY_CS_Study/assets/90955152/32939b0f-bb60-4971-95b6-6724ba0a13a0">

<img width="600" alt="스크린샷 2023-12-13 오전 12 10 57" src="https://github.com/Youth787/SSAFY_CS_Study/assets/90955152/9efc6349-109f-44e7-a479-24f4d27b150e">

<img width="600" alt="스크린샷 2023-12-13 오전 12 11 11" src="https://github.com/Youth787/SSAFY_CS_Study/assets/90955152/222f08a7-fd63-4643-b87f-5f7ede1715d2">

```java

// LIS 알고리즘

	int N = 13;
	int[] arr = new int[N];
	int[] dp = new int[N];

	// 이분 탐색을 이용한 방법
	memo = new int[N+1];
	int len=0;
	int idx=0;
	for(int i=0; i<N; i++) {
		if(arr[i] > memo[len]) {
			len +=1;
			memo[len] = arr[i];
		}else {
			idx = binarySearch(0,len, arr[i]);
			memo[idx] = arr[i];
		}
	}
	}

	public static int binarySearch(int left, int right, int key) {
		int mid = 0;
		while (left < right) {
			mid = (left + right) / 2;
			if (memo[mid] < key) left = mid + 1;
			else right = mid;
		}
		return right;
	}

```
참고 자료 https://4legs-study.tistory.com/106
