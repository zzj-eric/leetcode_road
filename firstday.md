最大装水问题

给你 n 个非负整数 a1，a2，...，an，每个数代表坐标中的一个点 (i, ai) 。在坐标内画 n 条垂直线，垂直线 i 的两个端点分别为 (i, ai) 和 (i, 0) 。找出其中的两条线，使得它们与 x 轴共同构成的容器可以容纳最多的水。

链接：https://leetcode-cn.com/problems/container-with-most-water

这里用到了动态规划，基本的表达式: area = min(height[i], height[j]) * (j - i) 使用两个指针，值小的指针向内移动，这样就减小了搜索空间 因为面积取决于指针的距离与值小的值乘积，如果值大的值向内移动，距离一定减小，而求面积的另外一个乘数一定小于等于值小的值，因此面积一定减小，而我们要求最大的面积，因此值大的指针不动，而值小的指针向内移动遍历

#include <iostream>
#include <vector>
using namespace std;
class Solution2 {   //the complex is n^2
public:
    int maxArea(vector<int>& height) {
        maxWater = 0;
        maxTempHeight = 0;
        tempWater = 0;
        number = height.size();

        //cout<<"number:"<<number<<endl;
        for(int i = 0;i<number;i++)
        {
            // if maxheight mul maxlength less then maxWater -> break
            vector<int>::const_iterator First = height.begin() + 1+i;
            vector<int>::const_iterator Second = height.end();
            vector<int> tempV(First, Second); 
            if((number - i -1)*maxInVector(tempV) < maxWater) 
            {
                tempV.clear();
                break;
            }
            for(int j = number-1;j>=i+1;j--)
            {
                if(height[j]>maxTempHeight)
                {
                    maxTempHeight = height[j];
                    tempWater = min(maxTempHeight, height[i]) * (j-i);
                    if(tempWater>maxWater) maxWater = tempWater;
                }
            }
            maxTempHeight= 0;
        }
        return maxWater;
    }
    int min(int a, int b)
    {
        return (a>b)?b:a;
    }
    int maxInVector(vector<int> &list)
    {
        int max = 0;
        for(int i = 0;i<list.size();i++)
        {
            if(list[i] > max) max = list[i];
        }
        return max;
    }
private:
    int maxWater;
    int tempWater;
    int maxTempHeight;
    int number;
};

class Solution   //n的复杂度
{
public:
    int maxArea(vector<int>& height) 
    {
        maxWater = 0;
        int begin = 0;
        int end = height.size()-1;
        int tempW;
        while(begin != end)
        {
            tempW = getV(begin,end,height);
            if(tempW > maxWater) maxWater = tempW;
        }
        return maxWater;
    }
    int getV(int & begin,int & end, vector<int> & height)
    {
        int minH = min(height[begin],height[end]);
        int tempV = minH * (end - begin);
        //cout<<"begin:"<<begin<<"       end:"<<end<<"        minH:"<<minH<<"        tempV:"<<tempV<<endl;
        if(minH == height[begin]) begin++;
        else end--;
        return tempV;
    }
    int min(int &a, int &b)
    {
        return (a>b)?b:a;
    }
private:
    int maxWater;
};
int main(void)
{
    vector<int> L = {1,8,6,2,5,4,8,3,7};
    Solution A;
    cout<<A.maxArea(L)<<endl;;
    return 0;
}

