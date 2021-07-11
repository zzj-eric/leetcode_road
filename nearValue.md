# 最接近三数之和
## 问题重述
给定一个包括 n 个整数的数组 nums 和 一个目标值 target。找出 nums 中的三个整数，使得它们的和与 target 最接近。返回这三个数的和。假定每组输入只存在唯一答案。

输入：nums = [-1,2,1,-4], target = 1
输出：2
解释：与 target 最接近的和是 2 (-1 + 2 + 1 = 2) 。

链接：https://leetcode-cn.com/problems/3sum-closest

## 问题分析
1. 个人分析
    
    分析：采用和AddThreeTree相似的方法 排序 一重循环加二重指针 （循环中剔除相同值的遍历）
2. 代码
```c++
class Solution {
public:
    int threeSumClosest(vector<int>& nums, int target) {
        result = nums[0] + nums[1] + nums[2];
        mingap = fabs(target - result);
        sort(nums); //sort over
        // the first rotate
        int number = nums.size();
        int k = number-1;
        int tempgap;
        for(int i = 0;i<number-2;i++)
        {
            if(i!=0 && nums[i] == nums[i-1]) continue;
            for(int j = i+1;j<number-1;j++)
            {
                if(j>=k) break;
                if(j != i+1 && nums[j] == nums[j-1]) continue;
                if(k != number-1 && nums[k] == nums[k+1]) {k--;j--;continue;}
                nowres = nums[i] + nums[j] + nums[k];
                tempgap = target - nowres;
                nowgap = fabs(tempgap);
                if(nowgap < mingap) {result = nowres; mingap = nowgap; if(mingap == 0) return target;}
                if(tempgap > 0) continue;
                else {k--;j--;}
                
            }
            k = number-1;
        }
        return result;
    }
    void sort(vector<int> & nums) //using n^2 temply
    {
        int num = nums.size();
        int temp;
        for(int i = 0;i<num-1;i++)
        {
            for(int j = 0; j<num-1-i;j++)
            {
                if(nums[j]>nums[j+1]) {temp = nums[j];nums[j] = nums[j+1]; nums[j+1] = temp;}
            }
        }
    }
private:
    int mingap;
    int nowgap;
    int nowres;
    int result;
};
```
