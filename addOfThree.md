# 三数之和问题分析和总结
## 问题重述
给你一个包含 n 个整数的数组 nums，判断 nums 中是否存在三个元素 a，b，c ，使得 a + b + c = 0 ？请你找出所有和为 0 且不重复的三元组。

注意：答案中不可以包含重复的三元组。

输入：nums = [-1,0,1,2,-1,-4]
输出：[[-1,-1,2],[-1,0,1]]

链接：https://leetcode-cn.com/problems/3sum

## 问题分析
1. 个人分析
   
   1. 看见题目中结果为三个数字的相加，考虑了采用动态规划的算法，尝试是否可以通过计算出两数相加的表格，再有表格得出三数相加的结果，但是发现去除重复的在三维坐标下过于繁琐，遂放弃。
   2. 有考虑了回溯的方法 本题目可以简单的得到基础解空间，在每次操作时，可以只按照顺序考虑，由于需要去除重复，需要需之前用过的数字来进行比较，有较大的时间和空间的复杂度。但是这种方法在考虑时还是没有办法规避掉类似 $nums = [-1,0,1,2,-1,-4]$ 这种情况下出现结果是 $[[-1,0,1],[0,1,-1]]$ 的情况，因为第一个数仅仅考虑到了不要在第一个数可能取值的位置处重复，但是无法规避后面出现的重复情况。
2. 官方解答

    https://leetcode-cn.com/problems/3sum/solution/san-shu-zhi-he-by-leetcode-solution/

    重点：排序去重复 双指针并列搜索加快速度

3. 我的源码
   
   *错误的回溯*
``` c++
    class Solution2 {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
        number = nums.size();
        for(int i = 0;i<=number-3;i++)
        {
            if(!check(nums[i],usedI)) continue;
            usedI.push_back(nums[i]);
            for(int j = i+1;j<=number-2;j++)
            {
                if(!check(nums[j],usedJ)) continue;
                usedJ.push_back(nums[j]);
                for(int k = j+1;k<=number-1;k++)
                {
                    if(!check(nums[k],usedK)) continue;
                    usedK.push_back(nums[k]);
                    if(nums[i]+nums[j]+nums[k] == 0) 
                    {
                        vector<int> elem = {nums[i],nums[j],nums[k]};
                        result.push_back(elem);
                    }
                }
                usedK.clear();
            }
            usedJ.clear();
        }
        print();
        return result;
    }
    bool check(int &e, vector<int> &used)
    {
        int size = used.size();
        for(int i = 0;i<size;i++)
        {
            if(e == used[i]) return false; //have been used
        }
        return true;
    }
    void print()
    {
        int i = 0;
        int j = 0;
        cout<<"[";
        for (vector<vector<int>>::iterator iter = result.begin(); iter != result.end(); iter++)
        {
            cout << "[" ;
            for(vector<int>::iterator elem = iter->begin();elem != iter->end();elem++)
            {
                cout<<*elem;
                i++;
                if (i!=3) cout<<",";
                i = 0;
            }
            cout<<"],";
        }
        cout<<"]";
    }
private:
    vector<vector<int>> result;
    int number;
    vector<int> usedI;
    vector<int> usedJ;
    vector<int> usedK;
};
```
**按照官方方法的代码**
```c++
class Solution
{
public:
    vector<vector<int>> threeSum(vector<int>& nums)
    {
        sort(nums);
        int number = nums.size();
        int k = number-1;
        int addres;
        for(int i = 0; i<number-2;i++)
        {
            if(i!=0 && nums[i] == nums[i-1]) continue;
            for(int j = i+1;j<number-1;j++)
            {
                if(j>=k) break;
                if(j!=i+1 && nums[j] == nums[j-1]) continue;
                addres = nums[i]+nums[j]+nums[k];
                if(addres == 0)
                {
                    tempelem.clear();
                    tempelem = {nums[i],nums[j],nums[k]};
                    result.push_back(tempelem);
                }
                else if(addres<0) continue;
                else {k--; j--;}
            }
            k = number-1;
        }
        return result;
    }
    void sort(vector<int>& nums)
    {
        int len = nums.size();
        int temp;
        for(int i= 0;i<len-1;i++)
        {
            for(int j = 0;j<len-1-i;j++)
            {
                if(nums[j] > nums[j+1])
                {
                    temp = nums[j];
                    nums[j] = nums[j+1];
                    nums[j+1] = temp;
                }
            }  
        }
    }
    void print()
    {
        int i = 0;
        int lenOfResult = result.size();
        cout<<"[";
        for (vector<vector<int>>::iterator iter = result.begin(); iter != result.end(); iter++)
        {
            i++;
            cout << "[" << (*iter)[i] <<","<< (*iter)[1]<<","<< (*iter)[2]<<"]";
            if(iter != result.end())
            if(i != lenOfResult) cout<<",";
        }
        cout<<"]";
    }
private:
    vector<vector<int>> result;
    vector<int> tempelem;
};
```