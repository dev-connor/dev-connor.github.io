---
title: 6' Sorting&Searching 강의
category: algorithm
---

# 정렬

정렬알고리즘 애니메이션

<https://www.toptal.com/developers/sorting-algorithms>{:target="_blank"}

## 1. 선택정렬

![image-20210707204811224](../../assets/images/image-20210707204811224.png)

가장 작은 수를 찾아 앞에서부터 정렬한다.

비교정렬이자 제자리정렬

시간복잡도는 $$O(N^2)$$ 이다.

항상 비슷한 시간이 걸린다.

**입력**

```
6
13 5 11 7 23 15
```

**출력**

```
13과 5을 바꿉니다.
[5, 13, 11, 7, 23, 15]
13과 7을 바꿉니다.
[5, 7, 11, 13, 23, 15]
11과 11을 바꿉니다.
[5, 7, 11, 13, 23, 15]
13과 13을 바꿉니다.
[5, 7, 11, 13, 23, 15]
23과 15을 바꿉니다.
[5, 7, 11, 13, 15, 23]
```

**swap 메서드사용 코드** 

```java
public class Main {
	public static void main(String[] args) {
		int n = 10;
		int[] arr = new int[n];

		for (int i = 0; i < n; i++) {
			arr[i] = (int) (Math.random() * 50);
		}

		for (int i = 0; i < n - 1; i++) {
			int index = i;
			for (int j = i + 1; j < n; j++) {
				if (arr[index] > arr[j]) {
					index = j;
				}
			}
			swap(arr, i, index);
		}
		System.out.println(Arrays.toString(arr));
	}

	static void swap(int[] arr, int i, int j) {
		int temp = arr[i];
		arr[i] = arr[j];
		arr[j] = temp;
	}
}
```

**메소드없는 코드**

```java
public class Main {
	public static void main(String[] args) {
		Scanner sc = new Scanner(System.in);
		int n = sc.nextInt();
		int[] arr = new int[n];

		for (int i = 0; i < n; i++) {
			arr[i] = sc.nextInt();
		}

		for (int i = 0; i < n - 1; i++) {
			int index = i;
			for (int j = i + 1; j < n; j++) {
				if (arr[index] > arr[j]) {
					index = j;
				}
			}
			int temp = arr[i];
			arr[i] = arr[index];
			arr[index] = temp;
		}
		System.out.println(Arrays.toString(arr));
	}
}
```

**설명포함 코드**

```java
public class Main {
	public static void main(String[] args) {
		int n = 10;
		int[] arr = new int[n];

		for (int i = 0; i < n; i++) {
			arr[i] = (int) (Math.random() * 50);
		}

		for (int i = 0; i < n - 1; i++) {
			int index = i;
			for (int j = i + 1; j < n; j++) {
				if (arr[index] > arr[j]) {
					index = j;
				}
			}
			int temp = arr[i];
			arr[i] = arr[index];
			arr[index] = temp;
			System.out.println(temp + "과 " + arr[i] + "을 바꿉니다.");
			System.out.println(Arrays.toString(arr));
		}
	}
}
```

## 2. 버블정렬


![bubble-sort](../../assets/images/bubble-sort-1625845909357.png)

이웃한 것 끼리 비교하여 정렬

이미 거의 정렬된 상태에서 빠르다.

```java
public class Main {
	public static void main(String[] args) {
		Scanner sc = new Scanner(System.in);
		int n = sc.nextInt();
		int[] arr = new int[n];
		
		for (int i = 0; i < n; i++) {
			arr[i] = sc.nextInt();
		}
		
		for (int i = 1; i < n; i++) {
			for (int j = 0; j < n - i; j++) {
				if (arr[j] > arr[j + 1]) {
					int temp = arr[j];
					arr[j] = arr[j + 1];
					arr[j + 1] = temp;
				}
			}
		}
		System.out.println(Arrays.toString(arr));
	}
}
```

## 3. 삽입정렬

  ![그림입니다.  원본 그림의 이름: CLP000050e05703.bmp  원본 그림의 크기: 가로 1280pixel, 세로 1718pixel](../../assets/images/EMB000050e05704.bmp)  

이미 거의 정렬된 상태에서 빠르다.

i 는 1부터 증가, j 는 감소하며 i 값을 끼워넣는다.

```java
public class Main {
	public static void main(String[] args) {
		Scanner sc = new Scanner(System.in);
		int n = sc.nextInt();
		int[] arr = new int[n];

		for (int i = 0; i < n; i++) {
			arr[i] = sc.nextInt();
		}

		for (int i = 1; i < n; i++) {
			int target = arr[i];
			int j = i - 1;
			while (j >= 0 && arr[j] > target) {
				arr[j + 1] = arr[j];
				j--;
			}
			arr[j + 1] = target;
		}
		System.out.println(Arrays.toString(arr));
	}
}
```

| 정렬     | 초기값 | 최종값 | 초기값 | 최종값      |
| -------- | ------ | ------ | ------ | ----------- |
| 선택정렬 | 0      | n-1    | i+1    | n           |
| 버블정렬 | 1      | n      | 0      | n-i         |
| 삽입정렬 | 1      | n      | i-1    | 0 보다 크다 |

## 4. LRU: Least Recently Used

![Image1.jpg](../../assets/images/c366c701c2.jpg)

캐시, 카카오 변형

**입력**

```
5 9
1 2 3 2 6 2 3 5 7
```

**출력** 

```
Cache Miss
[1, 0, 0, 0, 0]: 1
Cache Miss
[1, 1, 0, 0, 0]: 1을 밀었습니다.
[2, 1, 0, 0, 0]: 2
Cache Miss
[2, 1, 1, 0, 0]: 1을 밀었습니다.
[2, 2, 1, 0, 0]: 2을 밀었습니다.
[3, 2, 1, 0, 0]: 3
Cache Hit
2가 1위치에 있습니다.
Cache Miss
[3, 3, 1, 0, 0]: 3을 밀었습니다.
[2, 3, 1, 0, 0]: 2
Cache Miss
[2, 3, 1, 1, 0]: 1을 밀었습니다.
[2, 3, 3, 1, 0]: 3을 밀었습니다.
[2, 2, 3, 1, 0]: 2을 밀었습니다.
[6, 2, 3, 1, 0]: 6
Cache Hit
2가 1위치에 있습니다.
Cache Miss
[6, 6, 3, 1, 0]: 6을 밀었습니다.
[2, 6, 3, 1, 0]: 2
Cache Hit
3가 2위치에 있습니다.
Cache Miss
[2, 6, 6, 1, 0]: 6을 밀었습니다.
[2, 2, 6, 1, 0]: 2을 밀었습니다.
[3, 2, 6, 1, 0]: 3
Cache Miss
[3, 2, 6, 1, 1]: 1을 밀었습니다.
[3, 2, 6, 6, 1]: 6을 밀었습니다.
[3, 2, 2, 6, 1]: 2을 밀었습니다.
[3, 3, 2, 6, 1]: 3을 밀었습니다.
[5, 3, 2, 6, 1]: 5
Cache Miss
[5, 3, 2, 6, 6]: 6을 밀었습니다.
[5, 3, 2, 2, 6]: 2을 밀었습니다.
[5, 3, 3, 2, 6]: 3을 밀었습니다.
[5, 5, 3, 2, 6]: 5을 밀었습니다.
[7, 5, 3, 2, 6]: 7
```

**설명없는 코드**

```java
public class Main {
	public static void main(String[] args) {
		Scanner sc = new Scanner(System.in);
		int c = sc.nextInt();
		int w = sc.nextInt();
		int[] cash = new int[c];
		int[] work = new int[w];
		
		for (int i = 0; i < w; i++) {
			work[i] = sc.nextInt();
		}
		
		for (int i = 0; i < w; i++) {
			int index = c - 1;
			for (int j = 0; j < c; j++) {
				if (work[i] == cash[j]) {
					index = j;
					break;
				}
			}
			for (int j = index; j > 0; j--) {
				if (cash[j] != cash[j - 1]) {
					cash[j] = cash[j - 1];
				}
			}
			cash[0] = work[i];
		}
		System.out.println(Arrays.toString(cash));
	}
}
```

**설명있는 코드**

```java
public class Main {
	public static void main(String[] args) {
		Scanner sc = new Scanner(System.in);
		int c = sc.nextInt();
		int w = sc.nextInt();
		int[] cash = new int[c];
		int[] work = new int[w];

		for (int i = 0; i < w; i++) {
			work[i] = sc.nextInt();
		}

		for (int i = 0; i < w; i++) {
			int index = c - 1;
			boolean miss = false;
			for (int j = 0; j < c; j++) {
				if (work[i] == cash[j]) {
					System.out.println("Cache Hit");
					System.out.println(work[i] + "가 " + j + "위치에 있습니다.");
					index = j;
					break;
				} else {
					miss = true;
				}
			}
			if (miss) {
				System.out.println("Cache Miss");
			}
			for (int j = index; j > 0; j--) {
				if (cash[j] != cash[j - 1]) {
					cash[j] = cash[j - 1];
					System.out.println(Arrays.toString(cash) + ": " + cash[j - 1] + "을 밀었습니다.");
				}
			}
			cash[0] = work[i];
			System.out.println(Arrays.toString(cash) + ": " + cash[0]);
		}
	}
}
```

## 5. 중복확인

이 문제는 HashMap 으로도 풀 수 있을 것 같다.

```java
public class Main {
	public static void main(String[] args) {
		Scanner sc = new Scanner(System.in);
		int n = sc.nextInt();
		int[] numbers = new int[n];
		char answer = 'U';

		for (int i = 0; i < n; i++) {
			numbers[i] = sc.nextInt();
		}
		
		loop:
		for (int i = 0; i < n - 1; i++) {
			for (int j = i + 1; j < n; j++) {
				if (numbers[i] == numbers[j]) {
					answer = 'D';
					break loop;
				}
			}
		}
		System.out.println(answer);
	}
}
```

## 6. 장난꾸러기

**입력**

```
9
120 125 152 130 135 135 143 127 160
```

**출력** 

```
3 8 
```

**코드 ** 

```java
public class Main {
	public static void main(String[] args) {
		Scanner sc = new Scanner(System.in);
		int n = sc.nextInt();
		int[] arr = new int[n];
		int[] sorted = new int[n];
		int number = 1;

		for (int i = 0; i < n; i++) {
			arr[i] = sc.nextInt();
		}
		sorted = arr.clone();
		
		for (int i = 1; i < n; i++) {
			int target = sorted[i];
			int j = i - 1;
			while (j >= 0 && sorted[j] > target) {
				sorted[j + 1] = sorted[j];
				j--;
			}
			sorted[j + 1] = target;
		}
		
		for (int i = 0; i < n; i++) {
			if (arr[i] != sorted[i]) {
				System.out.print(number + " ");
			}
			number++;
		}
	}
}
```

## 7. 객체정렬: compareTo

x, y 좌표를 정렬하는 문제입니다.

**compareTo 메서드구현 코드**

```java
class Point implements Comparable<Point> {
	int x;
	int y;
	
	Point(int x, int y) {
		this.x = x;
		this.y = y;
	}

	@Override
	public int compareTo(Point o) {
		if (this.x == o.x) {
			return this.y - o.y;
		}
		return this.x - o.x;
	}
}
```

```java
public class Main {
	public static void main(String[] args) {
		Scanner sc = new Scanner(System.in);
		int n = sc.nextInt();
		ArrayList<Point> arr = new ArrayList<>();
		
		for (int i = 0; i < n; i++) {
			int x = sc.nextInt();
			int y = sc.nextInt();
			arr.add(new Point(x, y));
		}
		
		Collections.sort(arr);
		for (Point point : arr) {
			System.out.println(point.x + " " + point.y);
		}
	}
}
```

**입력**

```
5
2 7
1 3
1 2
2 5
3 6
```

**출력**

```
[2 7, 2 7, 1 2, 2 5, 3 6]: 2 7를 밉니다.
[1 3, 2 7, 1 2, 2 5, 3 6]
[1 3, 2 7, 2 7, 2 5, 3 6]: 2 7를 밉니다.
[1 3, 1 3, 2 7, 2 5, 3 6]: 1 3를 밉니다.
[1 2, 1 3, 2 7, 2 5, 3 6]
[1 2, 1 3, 2 7, 2 7, 3 6]: 2 7를 밉니다.
[1 2, 1 3, 2 5, 2 7, 3 6]
[1 2, 1 3, 2 5, 2 7, 3 6]
```

**삽입정렬구현 코드**

```java
class Coordinate implements Cloneable {
	int x;
	int y;

	Coordinate(int x, int y) {
		this.x = x;
		this.y = y;
	}

	@Override
	public String toString() {
		return x + " " + y;
	}
	
	@Override
	protected Coordinate clone() throws CloneNotSupportedException {
		return (Coordinate) super.clone();
	}
}
```

```java
public class Main {
	public static void main(String[] args) throws CloneNotSupportedException {
		Scanner sc = new Scanner(System.in);
		int n = sc.nextInt();
		Coordinate[] coordinates = new Coordinate[n];

		for (int i = 0; i < n; i++) {
			int x = sc.nextInt();
			int y = sc.nextInt();
			coordinates[i] = new Coordinate(x, y);
		}

		for (int i = 1; i < n; i++) {
			Coordinate target = coordinates[i];
			int j = i - 1;
			while (j >= 0 && coordinates[j].x >= target.x) {
				if (coordinates[j].x > target.x) {
					coordinates[j + 1] = coordinates[j].clone();
					System.out.println(Arrays.toString(coordinates) + ": " + coordinates[j].x + ", " + coordinates[j].y + "를 밉니다.");
					j--;
				} else if (coordinates[j].y > target.y) {
					coordinates[j + 1] = coordinates[j].clone();
					System.out.println(Arrays.toString(coordinates) + ": " + coordinates[j].x + ", " + coordinates[j].y + "를 밉니다.");
					j--;
				}
			}
			coordinates[j + 1] = target.clone();
			System.out.println(Arrays.toString(coordinates));
		}
	}
}
```



## 8. 이분검색: Binary search

![img](../../assets/images/img.png)

이 교수님이 페이지를 찾는데 책을 찢는 것과 같은 방법이다.

[책 예시](../../cs50/Computational-thinking/#알고리즘--algorithms){:target="_brank"}

이분검색은 정렬이 되어있어야 합니다.

```java
public class Main {
	public static void main(String[] args) {
		Scanner sc = new Scanner(System.in);
		int n = sc.nextInt();
		int m = sc.nextInt();
		int[] arr = new int[n];
		int left = 0;
		int right = n - 1;
		
		for (int i = 0; i < n; i++) {
			arr[i] = sc.nextInt();
		}
		
		Arrays.sort(arr);
		while (left <= right) {
			int mid = (left + right) / 2;
			if (m == arr[mid]) {
				System.out.println(mid + 1);
				break;
			} else if (m < arr[mid]) {
				right = mid - 1;
			} else {
				left = mid + 1;
			}
		}
	}
}
```

## 9. 결정알고리즘: 뮤직비디오

`Arrays.stream(arr)`

- `.max().getAsInt()` 
- `.sum()` 

**입력**

```
9 3
1 2 3 4 5 6 7 8 9
```

**출력** 

```
left: 9, right: 45, mid: 27
2<=3
answer: 27, right: 26
left: 9, right: 26, mid: 17
3<=3
answer: 17, right: 16
left: 9, right: 16, mid: 12
5>3
answer: 17, left: 13
left: 13, right: 16, mid: 14
5>3
answer: 17, left: 15
left: 15, right: 16, mid: 15
4>3
answer: 17, left: 16
left: 16, right: 16, mid: 16
4>3
answer: 17, left: 17
```

**설명없는 코드**

```java
public class Main {
	public static void main(String[] args) {
		Scanner sc = new Scanner(System.in);
		int n = sc.nextInt();
		int m = sc.nextInt();
		int[] arr = new int[n];
		int answer = 0;

		for (int i = 0; i < n; i++) {
			arr[i] = sc.nextInt();
		}

		int left = Arrays.stream(arr).max().getAsInt();
		int right = Arrays.stream(arr).sum();

		while (left <= right) {
			int mid = (left + right) / 2;
			if (count(arr, mid) <= m) {
				answer = mid;
				right = mid - 1;
			} else {
				left = mid + 1;
			}
		}
		System.out.println(answer);
	}

	static int count(int[] arr, int DVD) {
		int sum = 0;
		int count = 1;
		
		for (int minute : arr) {
			if (sum + minute > DVD) {
				count++;
				sum = 0;
			}
			sum += minute;
		}
		return count;
	}
}
```

**설명있는 코드**

```java
public class Main {
	public static void main(String[] args) {
		Scanner sc = new Scanner(System.in);
		int n = sc.nextInt();
		int m = sc.nextInt();
		int[] arr = new int[n];
		int answer = 0;

		for (int i = 0; i < n; i++) {
			arr[i] = sc.nextInt();
		}

		int left = Arrays.stream(arr).max().getAsInt();
		int right = Arrays.stream(arr).sum();

		while (left <= right) {
			int mid = (left + right) / 2;
			System.out.println("left: " + left + ", right: " + right + ", mid: " + mid);
			if (count(arr, mid) <= m) {
				answer = mid;
				right = mid - 1;
				System.out.println(count(arr, mid) + "<=" + m);
				System.out.println("answer: " + answer + ", right: " + right);
			} else {
				left = mid + 1;
				System.out.println(count(arr, mid) + ">" + m);
				System.out.println("answer: " + answer + ", left: " + left);
			}
		}
	}

	static int count(int[] arr, int DVD) {
		int sum = 0;
		int count = 1;
		
		for (int minute : arr) {
			if (sum + minute > DVD) {
				count++;
				sum = 0;
			}
			sum += minute;
		}
		return count;
	}
}
```

## 10. 결정알고리즘: 마구간 정하기

N개의 마구간이 수직선상에 있습니다. 각 마구간은 x1, x2, x3, ......, xN의 좌표를 가지며, 마구간간에 좌표가 중복되는 일은 없습니다.

현수는 C마리의 말을 가지고 있는데, 이 말들은 서로 가까이 있는 것을 좋아하지 않습니다. 각 마구간에는 한 마리의 말만 넣을 수 있고,

가장 가까운 두 말의 거리가 최대가 되게 말을 마구간에 배치하고 싶습니다.

C마리의 말을 N개의 마구간에 배치했을 때 가장 가까운 두 말의 거리가 최대가 되는 그 최대값을 출력하는 프로그램을 작성하세요.

**입력**

```
5 3
1 2 8 4 9
```

**출력**

```
[1, 2, 4, 8, 9]
left: 1, right: 8, mid: 4
5>=8, count: 2
left: 1, right: 3, mid: 2
3>=4, count: 2
6>=8, count: 3
left: 3, right: 3, mid: 3
4>=4, count: 2
7>=8, count: 3
```

**설명없는 코드**

```java
import java.util.Arrays;
import java.util.Scanner;

public class Main {
	public static void main(String[] args) {
		Scanner sc = new Scanner(System.in);
		int n = sc.nextInt();
		int c = sc.nextInt();
		int[] arr = new int[n];
		int answer = 0;

		for (int i = 0; i < n; i++) {
			arr[i] = sc.nextInt();
		}

		Arrays.sort(arr);
		int left = 1;
		int right = arr[n - 1] - arr[0];
		while (left <= right) {
			int mid = (left + right) / 2;
			if (horse(arr, mid) >= c) {
				answer = mid;
				left = mid + 1;
			} else {
				right = mid - 1;
			}
		}
		System.out.println(answer);
	}

	private static int horse(int[] arr, int mid) {
		int recent = arr[0];
		int count = 1;
		
		for (int point : arr) {
			if (recent + mid <= point) {
				recent = point;
				count++;
			}
		}
		return count;
	}
}
```

**설명있는 코드**

```java
public class Main {
	public static void main(String[] args) {
		Scanner sc = new Scanner(System.in);
		int n = sc.nextInt();
		int c = sc.nextInt();
		int[] arr = new int[n];
		int answer = 0;

		for (int i = 0; i < n; i++) {
			arr[i] = sc.nextInt();
		}

		Arrays.sort(arr);
		System.out.println(Arrays.toString(arr));
		int left = 1;
		int right = arr[n - 1] - arr[0];
		while (left <= right) {
			int mid = (left + right) / 2;
			System.out.println("left: " + left + ", right: " + right + ", mid: " + mid);
			if (horse(arr, mid) >= c) {
				answer = mid;
				left = mid + 1;
			} else {
				right = mid - 1;
			}
		}
	}

	private static int horse(int[] arr, int mid) {
		int recent = arr[0];
		int count = 1;
		
		for (int point : arr) {
			if (recent + mid <= point) {
				System.out.print(recent + mid + ">=" + point + ", count: ");
				recent = point;
				count++;
				System.out.println(count);
			}
		}
		return count;
	}
}
```

