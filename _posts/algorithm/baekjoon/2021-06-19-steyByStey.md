---
title: 단계별 문제
category: baekjoon
---

# for 문

**X보다 작은 수**

```java
import java.util.ArrayList;
import java.util.Scanner;

public class Main {
	public static void main(String[] args) {
		Scanner sc = new Scanner(System.in);
		int n = sc.nextInt();
		int x = sc.nextInt();
		ArrayList<Integer> arr = new ArrayList<>();
		
		for (int i = 0; i < n; i++) {
			int m = sc.nextInt();
			if (m < x) {
				arr.add(m);
			}
		}
        
		for (Integer integer : arr) {
			System.out.print(integer + " ");
		}		
	}
}
```

> arrayList 를 쓰지 않아도 된다.

# while 문

##  A + B

```java
import java.util.Scanner;

public class Main {
	public static void main(String[] args) {
		Scanner sc = new Scanner(System.in);
        
		while (true) {
			int a = sc.nextInt();
			int b = sc.nextInt();
			if ((a == 0) & (b == 0)) {
				break;
			}
			System.out.println(a + b);
		}
	}
}
```

## A + B (EOF)

```java
import java.util.Scanner;

public class Main {
	public static void main(String[] args) {
		Scanner sc = new Scanner(System.in);
        
		while (sc.hasNextInt()) {
			int a = sc.nextInt();
			int b = sc.nextInt();
	
			System.out.println(a + b);
		}
	}
}
```

> EOF 의 구현은 입력이 없을 때 while 문을 반복하지 않는 것!!

## 더하기 사이클

```java
import java.util.Scanner;

public class Main {
	public static void main(String[] args) {
		Scanner sc = new Scanner(System.in);
		int n = sc.nextInt();
		int cycle = 0;
		int ten;
		int one;
		int next = n;
		
		while (true) {
			ten = next / 10;
			one = next % 10;
			next = one * 10 + (ten + one) % 10;
			cycle++;
			
			if (n == next) {
				System.out.println(cycle);
				break;
			}
		}		
	}
}
```

# 1차원 배열

## 최소, 최대

```java
import java.util.Scanner;

public class Main {
	public static void main(String[] args) {
		Scanner sc = new Scanner(System.in);
		int n = sc.nextInt();
		int min;
		int max;
		int next;

		next = sc.nextInt();
		min = next;
		max = next;
		
		for (int i = 0; i < n - 1; i++) {
			next = sc.nextInt();
			if (min > next) {
				min = next;
			}
			if (max < next) {
				max = next;
			}	
		}
		System.out.println(min + " " + max);
	}
}
```

---

> 스트림을 공부 중이므로 이제부터 스트림을 적극 활용해보려고 한다.
>
> 그리고 메서드 자동완성 없이 손코딩도 연습중이다.

## 최댓값 -2562

```java
public class Main {
	public static void main(String[] args) {
		Scanner sc = new Scanner(System.in);
		ArrayList<Integer> list = new ArrayList<>();

		for (int i = 0; i < 9; i++) 
			list.add(sc.nextInt());

		int max = list.stream().mapToInt(i -> i).max().getAsInt();
		System.out.print(max + "\n" + (list.indexOf(max) + 1));
	}
}
```

## 나머지 -3052

```java
public class Main {
	public static void main(String[] args) {
		Scanner sc = new Scanner(System.in);
		HashSet<Integer> set = new HashSet<>();

		for (int i = 0; i < 10; i++) 
			set.add(sc.nextInt() % 42);

		System.out.println(set.size());
	}
}
```

set 은 add. map 은 put 이다.

**평균 -1546**

**문제**

세준이는 기말고사를 망쳤다. 세준이는 점수를 조작해서 집에 가져가기로 했다. 일단 세준이는 자기 점수 중에 최댓값을 골랐다. 이 값을 M이라고 한다. 그리고 나서 모든 점수를 점수/M*100으로 고쳤다.

예를 들어, 세준이의 최고점이 70이고, 수학점수가 50이었으면 수학점수는 50/70*100이 되어 71.43점이 된다.

세준이의 성적을 위의 방법대로 새로 계산했을 때, 새로운 평균을 구하는 프로그램을 작성하시오.

**입력**

첫째 줄에 시험 본 과목의 개수 N이 주어진다. 이 값은 1000보다 작거나 같다. 둘째 줄에 세준이의 현재 성적이 주어진다. 이 값은 100보다 작거나 같은 음이 아닌 정수이고, 적어도 하나의 값은 0보다 크다.

**출력**

첫째 줄에 새로운 평균을 출력한다. 실제 정답과 출력값의 절대오차 또는 상대오차가 10-2 이하이면 정답이다.

| 입력                  | 출력      |
| --------------------- | --------- |
| 3 <br />40 80 60      | 75.0      |
| 3 <br />10 20 30      | 66.666667 |
| 4 <br />1 100 100 100 | 75.25     |
| 5 <br />1 2 4 8 16    | 38.75     |
| 2 <br />3 10          | 65.0      |

**코드**

```java
public class Main {
	public static void main(String[] args) {
		Scanner sc = new Scanner(System.in);
		ArrayList<Integer> list = new ArrayList<>();
		int n = sc.nextInt();

		for (int i = 0; i < n; i++) 
			list.add(sc.nextInt());

		int max = list.stream().mapToInt(i -> i).max().getAsInt();
		double avg = list.stream().mapToDouble(i -> (double) i / max * 100).average().getAsDouble();
		System.out.println(avg);
	}
}
```

`mapToInt()` 에는 i -> i 를 꼭 넣는다.

계산은 mapToInt 혹은 mapToDouble 에서 한다.

평균은 `average()` - `getAsDouble()` 로 계산한다.

**OX퀴즈 -8958**

**문제**

"OOXXOXXOOO"와 같은 OX퀴즈의 결과가 있다. O는 문제를 맞은 것이고, X는 문제를 틀린 것이다. 문제를 맞은 경우 그 문제의 점수는 그 문제까지 연속된 O의 개수가 된다. 예를 들어, 10번 문제의 점수는 3이 된다.

"OOXXOXXOOO"의 점수는 1+2+0+0+1+0+0+1+2+3 = 10점이다.

OX퀴즈의 결과가 주어졌을 때, 점수를 구하는 프로그램을 작성하시오.

입력

```
5
OOXXOXXOOO
OOXXOOXXOO
OXOXOXOXOXOXOX
OOOOOOOOOO
OOOOXOOOOXOOOOX
```

출력

```
10
9
7
55
30
```

코드

```java
public class Main {
	public static void main(String[] args) {
		Scanner sc = new Scanner(System.in);
		int n = sc.nextInt();

		for (int i = 0; i < n; i++) {
			String quiz = sc.next();
			int point = 0;
			int sum = 0;

			for (char q : quiz.toCharArray()) {
				if (q == 'O') {
					point++;
					sum += point;
				}
				else point = 0;
			}
			System.out.println(sum);
		} 
	}
}
```

## 평균은 넘겠지 -4344

작성날짜: 10/9

**문제**

대학생 새내기들의 90%는 자신이 반에서 평균은 넘는다고 생각한다. 당신은 그들에게 슬픈 진실을 알려줘야 한다.

**입력**

첫째 줄에는 테스트 케이스의 개수 C가 주어진다.

둘째 줄부터 각 테스트 케이스마다 학생의 수 N(1 ≤ N ≤ 1000, N은 정수)이 첫 수로 주어지고, 이어서 N명의 점수가 주어진다. 점수는 0보다 크거나 같고, 100보다 작거나 같은 정수이다.

```
5
5 50 50 70 80 100
7 100 95 90 80 70 60 50
3 70 90 80
3 70 90 81
9 100 99 98 97 96 95 94 93 91
```

**출력**

각 케이스마다 한 줄씩 평균을 넘는 학생들의 비율을 반올림하여 소수점 셋째 자리까지 출력한다.

```
40.000%
57.143%
33.333%
66.667%
55.556%
```

```java
public class Main {
	public static void main(String[] args) {
		Scanner sc = new Scanner(System.in);
		int n = sc.nextInt();	

		for (int i = 0; i < n; i++) {
			int student = sc.nextInt();
			int[] arr = new int[student];

			for (int j = 0; j < student; j++) {
				arr[j] = sc.nextInt();
			}
			
			double avg = Arrays.stream(arr).average().getAsDouble();
			int count = (int) Arrays.stream(arr).filter(it -> it > avg).count();
			double percnt = (double) count / student;
			System.out.printf("%.3f%%\n", percnt * 100);
		} 
	}
}
```

1. forEach 와 map 에는 조건문과 식이 들어가지 않으므로 조건문은 꼭 filter 에 넣는다.
2. for 문 안에서 stream 을 쓸 경우 i 대신 it 을 사용한다.
3. `count()` 는 long (정수) 형을 반환하므로 후에 꼭 캐스팅한다.
4. int 끼리의 나눗셈은 꼭 double 형으로 캐스팅한다.
5. printf 에서 % 의 출력은 `\%` 가 아닌 `%%` 로 한다.

# 함수

## 정수 N개의 합 -15596

```java
class Test {
	long sum;
	
	long sum(int[] a) {
		Arrays.stream(a).forEach(i -> sum += i);
		return sum;
	}
}
```

1. `average()` `max()` `min()` 이 optional 을 반환하는 것과 다르게 `sum()` 은 int 를 반환하므로 캐스팅이 필요없다.
2. sum 에 더하는 것은 sum 을 멤버변수로 선언할 것
3. forEach 문을 사용할 것

## 셀프넘버 -4673

**문제**

셀프 넘버는 1949년 인도 수학자 D.R. Kaprekar가 이름 붙였다. 양의 정수 n에 대해서 d(n)을 n과 n의 각 자리수를 더하는 함수라고 정의하자. 예를 들어, d(75) = 75+7+5 = 87이다.

양의 정수 n이 주어졌을 때, 이 수를 시작해서 n, d(n), d(d(n)), d(d(d(n))), ...과 같은 무한 수열을 만들 수 있다. 

예를 들어, 33으로 시작한다면 다음 수는 33 + 3 + 3 = 39이고, 그 다음 수는 39 + 3 + 9 = 51, 다음 수는 51 + 5 + 1 = 57이다. 이런식으로 다음과 같은 수열을 만들 수 있다.

33, 39, 51, 57, 69, 84, 96, 111, 114, 120, 123, 129, 141, ...

n을 d(n)의 생성자라고 한다. 위의 수열에서 33은 39의 생성자이고, 39는 51의 생성자, 51은 57의 생성자이다. 생성자가 한 개보다 많은 경우도 있다. 예를 들어, 101은 생성자가 2개(91과 100) 있다. 

생성자가 없는 숫자를 셀프 넘버라고 한다. 100보다 작은 셀프 넘버는 총 13개가 있다. 1, 3, 5, 7, 9, 20, 31, 42, 53, 64, 75, 86, 97

10000보다 작거나 같은 셀프 넘버를 한 줄에 하나씩 출력하는 프로그램을 작성하시오.

출력

```
1
3
5
7
9
20
31
42
53
64
 |
 |       <-- a lot more numbers
 |
9903
9914
9925
9927
9938
9949
9960
9971
9982
9993
```

코드

```java
public class Main {
	public static void main(String[] args) {
		HashSet<Integer> set = new HashSet<>();

		for (int i = 1; i < 10000; i++) {
			int sum = i;

			for (char c : String.valueOf(i).toCharArray()) 
				sum += c - '0';
			set.add(sum);
		}

		for (int i = 1; i <= 10000; i++) 
			if (!set.contains(i)) System.out.println(i);
	}
}
```

## 한수 -1065

**문제**

어떤 양의 정수 X의 각 자리가 등차수열을 이룬다면, 그 수를 한수라고 한다. 등차수열은 연속된 두 개의 수의 차이가 일정한 수열을 말한다. N이 주어졌을 때, 1보다 크거나 같고, N보다 작거나 같은 한수의 개수를 출력하는 프로그램을 작성하시오. 

| 입력 | 출력 |
| ---- | ---- |
| 110  | 99   |
| 1    | 1    |
| 210  | 105  |
| 1000 | 144  |

```java
public class Main {
	public static void main(String[] args) {
		Scanner sc = new Scanner(System.in);
		int n = sc.nextInt();
		int count = 99;

		for (int i = 100; i <= n; i++) {
			char[] arr = String.valueOf(i).toCharArray();
			int d = arr[1] - arr[0];

			for (int j = 1; j < arr.length - 1; j++) {
				if (d != arr[j + 1] - arr[j]) break;
				if (j == arr.length - 2) count++;
			}
		}
		System.out.println(n >= 100 ? count : n);
	}
}
```

