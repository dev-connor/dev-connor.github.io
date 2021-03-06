---
title: 프로그래머스 Lv1 1페이지-2
category: programmers
---

## 1. 1주차_부족한 금액 계산하기

**문제 설명**

새로 생긴 놀이기구는 인기가 매우 많아 줄이 끊이질 않습니다. 이 놀이기구의 원래 이용료는 price원 인데, 놀이기구를 N 번 째 이용한다면 원래 이용료의 N배를 받기로 하였습니다. 즉, 처음 이용료가 100이었다면 2번째에는 200, 3번째에는 300으로 요금이 인상됩니다.
놀이기구를 count번 타게 되면 현재 자신이 가지고 있는 금액에서 얼마가 모자라는지를 return 하도록 solution 함수를 완성하세요.
단, 금액이 부족하지 않으면 0을 return 하세요.

**입출력 예**

| price | money | count | result |
| ----- | ----- | ----- | ------ |
| 3     | 20    | 4     | 10     |

**코드**

```java
public class Solution {
    public long solution(int price, int money, int count) {
        long sum = 0;
        
        for (int i = 1; i <= count; i++) {
			sum += i * price;
		}
        return sum > money ? sum - money : 0;
    }
}
```

> 해설: 1* 2500 + 2* 2500 + 3* 2500 + ... +2499* 2500 + 2500* 2500 > 21억이므로 sum 을 long 형으로 선언해야한다.

**등차수열의 합 공식**



## 2. 2주차_상호평가

10점

**문제 설명**

대학 교수인 당신은, 상호평가를 통하여 학생들이 제출한 과제물에 학점을 부여하려고 합니다. 아래는 0번부터 4번까지 번호가 매겨진 5명의 학생들이 자신과 다른 학생의 과제를 평가한 점수표입니다.

|          |             |            |        |        |        |
| -------- | ----------- | ---------- | ------ | ------ | ------ |
| No.      | **0**       | **1**      | **2**  | **3**  | **4**  |
| **0**    | ~~**100**~~ | 90         | 98     | 88     | 65     |
| **1**    | 50          | ~~**45**~~ | 99     | 85     | 77     |
| **2**    | 47          | 88         | **95** | 80     | 67     |
| **3**    | 61          | 57         | 100    | **80** | 65     |
| **4**    | 24          | 90         | 94     | 75     | **65** |
| **평균** | 45.5        | 81.25      | 97.2   | 81.6   | 67.8   |
| **학점** | F           | B          | A      | B      | D      |

위의 점수표에서, i행 j열의 값은 i번 학생이 평가한 j번 학생의 과제 점수입니다.

- 0번 학생이 평가한 점수는 0번 `행` 에담긴 [100, 90, 98, 88, 65]입니다.
  - 0번 학생은 자기 자신에게 100점, 1번 학생에게 90점, 2번 학생에게 98점, 3번 학생에게 88점, 4번 학생에게 65점을 부여했습니다.
- 2번 학생이 평가한 점수는 2번 `행` 에담긴 [47, 88, 95, 80, 67]입니다.
  - 2번 학생은 0번 학생에게 47점, 1번 학생에게 88점, 자기 자신에게 95점, 3번 학생에게 80점, 4번 학생에게 67점을 부여했습니다.

당신은 각 학생들이 받은 점수의 평균을 구하여, 기준에 따라 학점을 부여하려고 합니다.
만약, 학생들이 자기 자신을 평가한 점수가 **유일한 최고점** 또는 **유일한 최저점**이라면 그 점수는 제외하고 평균을 구합니다.

- 0번 학생이 받은 점수는 0번`열` 에 담긴 [100, 50, 47, 61, 24]입니다. 자기 자신을 평가한 100점은 자신이 받은 점수 중에서 유일한 최고점이므로, 평균을 구할 때 제외합니다.
  - 0번 학생의 평균 점수는 (50+47+61+24) / 4 = 45.5입니다.
- 4번 학생이 받은 점수는 4번`열` 에 담긴 [65, 77, 67, 65,65]입니다. 자기 자신을 평가한 65점은 자신이 받은 점수 중에서 최저점이지만 같은 점수가 2개 더 있으므로, 유일한 최저점이 아닙니다. 따라서, 평균을 구할 때 제외하지 않습니다.
  - 4번 학생의 평균 점수는 (65+77+67+65+65) / 5 = 67.8입니다.

제외할 점수는 제외하고 평균을 구한 후, 아래 기준에 따라 학점을 부여합니다.

| 평균                | 학점 |
| ------------------- | ---- |
| 90점 이상           | A    |
| 80점 이상 90점 미만 | B    |
| 70점 이상 80점 미만 | C    |
| 50점 이상 70점 미만 | D    |
| 50점 미만           | F    |

학생들의 점수가 담긴 정수형 2차원 배열 scores가 매개변수로 주어집니다. 이때, 학생들의 학점을 구하여 하나의 문자열로 만들어서 return 하도록 solution 함수를 완성해주세요.

**개선된 코드**

```java
public class Solution {
    public String solution(int[][] scores) {
        String answer = "";
        double avg;
        
        for (int i = 0; i < scores.length; i++) {
        	int sum = 0;
        	int max = 0;
        	int min = 100;
        	
        	for (int j = 0; j < scores.length; j++) {
        		int score = scores[j][i];
        		
        		sum += score;
        		if (i != j) {
        			if (min > score) min = score;
        			if (max < score) max = score;
        		}
			}
        	avg = selfRating(scores[i][i], max, min) ? (double) sum / scores.length :
        		(double) (sum - scores[i][i]) / (scores.length - 1);
        	answer += getGrade(avg);
		}
        return answer;
    }

	private boolean selfRating(int mine, int max, int min) {
		return mine > max || mine < min ? false : true;
	}					

	public String getGrade(double avg) {
		return avg >= 90 ? "A" : avg >= 80 ? "B" : avg >= 70 ? "C" : avg >= 50 ? "D" : "F";
	}
}
```

## 3. 가운데 글자 가져오기

1점

**문제 설명**

단어 s의 가운데 글자를 반환하는 함수, solution을 만들어 보세요. 단어의 길이가 짝수라면 가운데 두글자를 반환하면 됩니다.

**재한사항**

- s는 길이가 1 이상, 100이하인 스트링입니다.

**입출력 예** 

| s       | return |
| ------- | ------ |
| "abcde" | "c"    |
| "qwer"  | "we"   |

**개선된 코드**

```java
public class Solution {
    public String solution(String s) {
        return s.substring((s.length()-1) / 2, (s.length()+2) / 2);
    }
}
```

**코드** 

```java
class Solution {
    public String solution(String s) {
        int half = s.length() / 2;
        
        return s.length() % 2 == 0 ? s.substring(half - 1, half + 1) :
        s.charAt(half) + "";
    }
}
```

## 

## 4. \[1차] 다트게임

7점

**문제 설명**

카카오톡 게임별의 하반기 신규 서비스로 다트 게임을 출시하기로 했다. 다트 게임은 다트판에 다트를 세 차례 던져 그 점수의 합계로 실력을 겨루는 게임으로, 모두가 간단히 즐길 수 있다.
갓 입사한 무지는 코딩 실력을 인정받아 게임의 핵심 부분인 점수 계산 로직을 맡게 되었다. 다트 게임의 점수 계산 로직은 아래와 같다.

1. 다트 게임은 총 3번의 기회로 구성된다.
2. 각 기회마다 얻을 수 있는 점수는 0점에서 10점까지이다.
3. 점수와 함께 Single(`S`), Double(`D`), Triple(`T`) 영역이 존재하고 각 영역 당첨 시 점수에서 1제곱, 2제곱, 3제곱 (점수1 , 점수2 , 점수3 )으로 계산된다.
4. 옵션으로 스타상(`*`) , 아차상(`#`)이 존재하며 스타상(`*`) 당첨 시 해당 점수와 바로 전에 얻은 점수를 각 2배로 만든다. 아차상(`#`) 당첨 시 해당 점수는 마이너스된다.
5. 스타상(`*`)은 첫 번째 기회에서도 나올 수 있다. 이 경우 첫 번째 스타상(`*`)의 점수만 2배가 된다. (예제 4번 참고)
6. 스타상(`*`)의 효과는 다른 스타상(`*`)의 효과와 중첩될 수 있다. 이 경우 중첩된 스타상(`*`) 점수는 4배가 된다. (예제 4번 참고)
7. 스타상(`*`)의 효과는 아차상(`#`)의 효과와 중첩될 수 있다. 이 경우 중첩된 아차상(`#`)의 점수는 -2배가 된다. (예제 5번 참고)
8. Single(`S`), Double(`D`), Triple(`T`)은 점수마다 하나씩 존재한다.
9. 스타상(`*`), 아차상(`#`)은 점수마다 둘 중 하나만 존재할 수 있으며, 존재하지 않을 수도 있다.

0~10의 정수와 문자 S, D, T, *, #로 구성된 문자열이 입력될 시 총점수를 반환하는 함수를 작성하라.

**입력 형식**

"점수|보너스|[옵션]"으로 이루어진 문자열 3세트.
예) `1S2D*3T`

- 점수는 0에서 10 사이의 정수이다.
- 보너스는 S, D, T 중 하나이다.
- 옵선은 *이나 # 중 하나이며, 없을 수도 있다.

**출력 형식**

3번의 기회에서 얻은 점수 합계에 해당하는 정수값을 출력한다.
예) 37

**입출력 예제**

| 예제 | dartResult | answer | 설명                        |
| ---- | ---------- | ------ | --------------------------- |
| 1    | `1S2D*3T`  | 37     | 11 * 2 + 22 * 2 + 33        |
| 2    | `1D2S#10S` | 9      | 12 + 21 * (-1) + 101        |
| 3    | `1D2S0T`   | 3      | 12 + 21 + 03                |
| 4    | `1S*2T*3S` | 23     | 11 * 2 * 2 + 23 * 2 + 31    |
| 5    | `1D#2S*3S` | 5      | 12 * (-1) * 2 + 21 * 2 + 31 |
| 6    | `1T2D3D#`  | -4     | 13 + 22 + 32 * (-1)         |
| 7    | `1D2S3T*`  | 59     | 12 + 21 * 2 + 33 * 2        |

**for-each 코드**

> 프로그래머스에서 다른사람의 풀이를 봤는데 내가 짠 코드보다 더 좋은 코드는 없어보였다..

```java
public class 다트게임_1차 {
    public int solution(String dartResult) {
        int n = 0;
        int[] scores = new int[3];
        int idx = -1;
        boolean isTen = false;
        
        for (char c : dartResult.toCharArray()) {
        	if (Character.isDigit(c)) {
        		n = isTen ? 10 : c - '0';
        		if (!isTen) idx++;
        		isTen = true;
        	} else if (Character.isAlphabetic(c)) {
        		scores[idx] = (int) Math.pow(n, c == 'S' ? 1 : c == 'D' ? 2 : 3);
        		isTen = false;
        	}
        	else {
        		scores[idx] *= c == '*' ? 2 : -1;
        		if (c == '*' && idx != 0) scores[idx - 1] *= 2;
        	}
		}
        return Arrays.stream(scores).sum();
    }
}
```

**게임별 문자열배열 코드**

입력

```java
String dartResult = "1D2S#10S";
```

배열 출력

```
[1D, 2S#, 10S]
```

코드

```java
public class Solution {
    public int solution(String dartResult) {
    	String[] results = new String[3];
    	int[] scores = new int[3];
    	int len = 0;
    	
    	for (int i = 0; i < 3; i++) {
    		results[i] = dartResult.substring(len, len + 2) + dartResult.substring(len + 2).split("\\d")[0];
    		len += results[i].length();
		}
    	
    	for (int i = 0; i < 3; i++) {
    		String point = results[i].split("\\D")[0];
    		char bonus = results[i].charAt(point.length());
    		
    		scores[i] = (int) Math.pow(Integer.parseInt(point), bonus == 'S' ? 1 : bonus == 'D' ? 2 : 3);
    		if (results[i].endsWith("#")) scores[i] *= -1;
    		else if (results[i].endsWith("*")) {
    			scores[i] *= 2;
    			if (i != 0) scores[i - 1] *= 2;
    		}
		}
    	return Arrays.stream(scores).sum();
    }
}
```

## 5. 신규 아이디 추천

작성날짜: 10/10

3점

**문제 설명**

카카오에 입사한 신입 개발자 `네오`는 "카카오계정개발팀"에 배치되어, 카카오 서비스에 가입하는 유저들의 아이디를 생성하는 업무를 담당하게 되었습니다. "네오"에게 주어진 첫 업무는 새로 가입하는 유저들이 카카오 아이디 규칙에 맞지 않는 아이디를 입력했을 때, 입력된 아이디와 유사하면서 규칙에 맞는 아이디를 추천해주는 프로그램을 개발하는 것입니다.
다음은 카카오 아이디의 규칙입니다.

- 아이디의 길이는 3자 이상 15자 이하여야 합니다.
- 아이디는 알파벳 소문자, 숫자, 빼기(`-`), 밑줄(`_`), 마침표(`.`) 문자만 사용할 수 있습니다.
- 단, 마침표(`.`)는 처음과 끝에 사용할 수 없으며 또한 연속으로 사용할 수 없습니다.

"네오"는 다음과 같이 7단계의 순차적인 처리 과정을 통해 신규 유저가 입력한 아이디가 카카오 아이디 규칙에 맞는 지 검사하고 규칙에 맞지 않은 경우 규칙에 맞는 새로운 아이디를 추천해 주려고 합니다.
신규 유저가 입력한 아이디가 `new_id` 라고 한다면,

```
1단계 new_id의 모든 대문자를 대응되는 소문자로 치환합니다.
2단계 new_id에서 알파벳 소문자, 숫자, 빼기(-), 밑줄(_), 마침표(.)를 제외한 모든 문자를 제거합니다.
3단계 new_id에서 마침표(.)가 2번 이상 연속된 부분을 하나의 마침표(.)로 치환합니다.
4단계 new_id에서 마침표(.)가 처음이나 끝에 위치한다면 제거합니다.
5단계 new_id가 빈 문자열이라면, new_id에 "a"를 대입합니다.
6단계 new_id의 길이가 16자 이상이면, new_id의 첫 15개의 문자를 제외한 나머지 문자들을 모두 제거합니다.
     만약 제거 후 마침표(.)가 new_id의 끝에 위치한다면 끝에 위치한 마침표(.) 문자를 제거합니다.
7단계 new_id의 길이가 2자 이하라면, new_id의 마지막 문자를 new_id의 길이가 3이 될 때까지 반복해서 끝에 붙입니다.
```

------

예를 들어, new_id 값이 "...!@BaT#*..y.abcdefghijklm" 라면, 위 7단계를 거치고 나면 new_id는 아래와 같이 변경됩니다.

1단계 대문자 'B'와 'T'가 소문자 'b'와 't'로 바뀌었습니다.
`"...!@BaT#*..y.abcdefghijklm"` → `"...!@bat#*..y.abcdefghijklm"`

2단계 '!', '@', '#', '*' 문자가 제거되었습니다.
`"...!@bat#*..y.abcdefghijklm"` → `"...bat..y.abcdefghijklm"`

3단계 '...'와 '..' 가 '.'로 바뀌었습니다.
`"...bat..y.abcdefghijklm"` → `".bat.y.abcdefghijklm"`

4단계 아이디의 처음에 위치한 '.'가 제거되었습니다.
`".bat.y.abcdefghijklm"` → `"bat.y.abcdefghijklm"`

5단계 아이디가 빈 문자열이 아니므로 변화가 없습니다.
`"bat.y.abcdefghijklm"` → `"bat.y.abcdefghijklm"`

6단계 아이디의 길이가 16자 이상이므로, 처음 15자를 제외한 나머지 문자들이 제거되었습니다.
`"bat.y.abcdefghijklm"` → `"bat.y.abcdefghi"`

7단계 아이디의 길이가 2자 이하가 아니므로 변화가 없습니다.
`"bat.y.abcdefghi"` → `"bat.y.abcdefghi"`

따라서 신규 유저가 입력한 new_id가 "...!@BaT#*..y.abcdefghijklm"일 때, 네오의 프로그램이 추천하는 새로운 아이디는 "bat.y.abcdefghi" 입니다.

**[문제]**

신규 유저가 입력한 아이디를 나타내는 new_id가 매개변수로 주어질 때, "네오"가 설계한 7단계의 처리 과정을 거친 후의 추천 아이디를 return 하도록 solution 함수를 완성해 주세요.

**코드**

```java
public class Solution {
    public String solution(String new_id) {
        new_id = new_id.toLowerCase().replaceAll("[^a-z0-9-_.]", "")
            .replaceAll("[.]{2,}", ".").replaceAll("^[.]|[.]$", "");
        if (new_id.isEmpty()) new_id = "a";
        if (new_id.length() >= 16) {
        	new_id = new_id.substring(0, 15);
        	new_id = new_id.replaceAll("[.]$", "");
        }
        
        while (new_id.length() <= 2) 
            if (!new_id.isEmpty())
                new_id += Character.toString(new_id.charAt(new_id.length() - 1));
        return new_id;
    }
}
```

**StringBuilder 코드**

```java
public class Solution {
    public String solution(String new_id) {
        new_id = new_id.toLowerCase().replaceAll("[^a-z0-9-_.]", "")
            .replaceAll("[.]{2,}", ".").replaceAll("^[.]|[.]$", "");
        
        StringBuilder str = new StringBuilder(new_id);
        if (str.isEmpty()) str.append("a");
        if (str.length() >= 16) {
        	str.setLength(15);
        	if (str.lastIndexOf(".") == str.length() - 1) 
                str.deleteCharAt(str.length() - 1);
        }
        while (str.length() <= 2) 
            str.append(str.charAt(str.length() - 1));    
        return new String(str);
    }
}
```

프로그래머스에서는 이상하게 str.isEmpty() 메서드를 읽지 못한다.



**문자열 메서드 비교**

| method       | String | StringBuilder | 반환   |
| ------------ | ------ | ------------- | ------ |
| chatAt       |        |               |        |
| chars        |        |               |        |
| indexOf      |        |               |        |
| isEmpty      |        |               |        |
| lastIndexOf  |        |               |        |
| length       |        |               |        |
| substring    |        |               | String |
| toString     |        |               |        |
| append       | X      |               | this   |
| delete       | X      |               | this   |
| deleteChatAt | X      |               | this   |
| insert       | X      |               | this   |
| replace      | X      | 다르다.       | this   |
| reverse      | X      |               | this   |
| setCharAt    | X      |               | void   |
| setLength    | X      |               | void   |

String: 찾아 바꾸기, 대소문자 변환에 유리. 

append() 메서드에는 모든 인자가 들어올 수 있으며 `char[ ]` 배열마저 원하는만큼 잘라서 넣을 수 있다.

