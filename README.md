## [프로그래머스 - 아방가르드 타일링](https://school.programmers.co.kr/learn/courses/30/lessons/181186)

```java
class Solution {
    public int solution(int n) {
        long[] base = new long[4];
        long[] dp = new long[100_001];
        
        dp[0] = 1;
        dp[1] = 1;
        dp[2] = 3;
        dp[3] = 10;
        base[0] = 1;
        base[1] = 1;
        base[2] = 2;
        base[3] = 5;
   
        // save the sum of dp[] for cases
        long[] patternsOfFourTwo = new long[6];
        
        for (int i = 4; i <= n; i++) {
            long four = 0;
            long two = 0;
            
            for (int j = 1; j < 4; j++) {
                dp[i] += dp[i - j] * base[j];
            }
            
            if (i > 3) {
                
                if (i % 3 == 1) {
                    patternsOfFourTwo[0] += dp[i - 4];
                    patternsOfFourTwo[2] += dp[i - 4];
                    patternsOfFourTwo[5] += dp[i - 4];

                    four = patternsOfFourTwo[1];
                    two = patternsOfFourTwo[0];
                } else if (i % 3 == 2) {
                    patternsOfFourTwo[1] += dp[i - 4];
                    patternsOfFourTwo[2] += dp[i - 4];
                    patternsOfFourTwo[4] += dp[i - 4];

                    four = patternsOfFourTwo[3];
                    two = patternsOfFourTwo[2];
                } else {
                    patternsOfFourTwo[0] += dp[i - 4];
                    patternsOfFourTwo[3] += dp[i - 4];
                    patternsOfFourTwo[4] += dp[i - 4];

                    four = patternsOfFourTwo[5];
                    two = patternsOfFourTwo[4];
                }
                
            }
            
            dp[i] += four * 4 + two * 2;
            dp[i] %= 1_000_000_007;
        }
        
        return (int) dp[n];
    }
}
```
