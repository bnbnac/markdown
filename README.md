## [리트코드 - 15. 3Sum](https://leetcode.com/problems/3sum/description/)

int array `nums`가 주어질 때, 더해서 0이 되는 세 개의 원소 조합들을 return하는 문제입니다. **솔루션을 참고하여 작성했습니다.*

중복이 없도록 return 하라는 요구사항을 보고 set을 이용해 브루트포스로 풀면 통과는 되지만 매우 느립니다. 아예 중복을 건너뛸 수 있는 방법을 쓰니 실행속도가 6배 이상 빨라졌습니다.

우선 `nums`를 정렬해둡니다. 앞에서부터 각 `nums[i]`에 대해, 더해서 `-nums[i]`가 되는 두 쌍을 `twoSumSorted`를 이용하여 찾을겁니다. 그러면 세 개 더했을 때 0이니까요. 이 때 `nums[i]`와 `nums[i + 1]`이 같다면 `i`번째 시행에서 이미 짝들을 찾아놨기 때문에 `i + 1`번째 시행을 반복하면 안됩니다.

`{a, b1, b2, c, d, e, ...}`에서 `b1 == b2`일 때,
앞의 `b1`으로 이미 짝 `[b1, p, q]`를 찾아놨는데 뒤의 `b2`로도 `twoSumSorted`를 돌리면 `[b2, p, q]`가 또 발견될테니 이를 방지하는 것입니다.

그리고 `twoSumSorted` method 내부에서는, `target`을 찾을 때 양 끝에서 two pointers를 좁혀오며 찾습니다. 비슷한 개념으로 주석표시된 while문을 통해 중복을 방지합니다.
마찬가지로 `{a, b1, b2, c, d, e, ...}`에서 `b1 == b2`일 때,
더해서 `-nums[i]`가 되는 짝 `[b1, k]`를 찾아놨는데 `[b2, k]`가 또 발견되지 않도록 합니다.


```java
class Solution {
    List<List<Integer>> res = new ArrayList();

    public List<List<Integer>> threeSum(int[] nums) {
        Arrays.sort(nums);

        for (int i = 0; i < nums.length; i++){
            if (i == 0 || nums[i - 1] != nums[i]) {
                twoSumSorted(i + 1, nums);
            }
        }

        return res;
    }

    public void twoSumSorted(int i, int[] nums){
        int cur = nums[i - 1];
        int target = -1 * cur;
        int j = nums.length - 1;

        while (i < j) {
            if (nums[i] + nums[j] > target){
                j--;
                continue;
            }
            if (nums[i] + nums[j] < target) {
                i++;
                continue;
            }

            res.add(new ArrayList(Arrays.asList(cur, nums[i], nums[j])));
                
            // 중복 방지
            while (i < j && nums[i] == nums[i + 1]) {
                i++;
            }
            
            // 중복 방지
            while (i < j && nums[j] == nums[j - 1]) {
                j--;
            }
            
            i++;
            j--; 
        }
    }
}
```
