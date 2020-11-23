学习笔记
作业题：

三数之和。leetcode 15题

给定数组 nums = [-1, 0, 1, 2, -1, -4]，[-4,-1,-1,0,1,2]
//
//满足要求的三元组集合为：
//[
//  [-1, 0, 1],
//  [-1, -1, 2]
//]

思考：

首先 先将数据有序。数据有序并不影响最终的输出。

如果数据有序，双指针遍历，如果最左边的都比0大，那没有这样的三元组存在。

如果当前值 + array[left指针] + array[right指针] > 0; right 指针左移。

如果当前值 + array[left指针] + array[right指针] < 0; left 指针右移。

public static List<List<Integer>> threeSum(int[] nums) {


        if(nums==null || nums.length==0) return null;


        List<List<Integer>> resultList = new ArrayList<List<Integer>>();

        Arrays.sort(nums);
        //Arrays.sort(nums);


        for(int i=0;i<nums.length - 2;i++){

            //if(nums[i] > 0){
            //    break;
            //}

            if(i==0||nums[i] > nums[i-1]){
                int left = i+1;

                int right =nums.length - 1;



                if(nums[i]+nums[left]+nums[left+1]>0 || nums[i]+nums[right-1]+nums[right]<0){
                    continue;
                }

                while(right > left){
                    int result = nums[i] + nums[left] + nums[right];


                    if(result == 0){

                        List<Integer> list =new ArrayList();


                        list.add(nums[i]);
                        list.add(nums[left]);
                        list.add(nums[right]);


                        resultList.add(list);

                        //去重

                        left+=1;

                        right -=1;


                        while(right>left && nums[left] == nums[left-1]){

                            left+=1;
                        }


                        while (right>left && nums[right] == nums[right +1])
                        {
                            right-=1;
                        }

                    }

                    if(result > 0){

                        right--;

                    }

                    if(result <0){
                        left++;
                    }
                }
            }



        }

        return resultList;
}

            
第二：求在该柱状图中，能够勾勒出来的矩形的最大面积。84题。

首先想到的就是暴力解法。
       /**
         * 暴力解法  矩形面积是 高*宽
         *
         * 固定高度，枚举宽度
         *
         * 时间复杂度为 O（n^2) 空间复杂度为O(1)
         */     

      for(int i = 0;i<heights.length;i++){

            left = i;

            right = i;

            //找到左边严格小于当前柱状的高度的坐标
            while(left - 1 >=0 && heights[left - 1] >= heights[i]){
                left -- ;
            }

            while(right + 1<heights.length && heights[right + 1] >= heights[i]){
                right ++;
            }

            area = Math.max(area,(right - left  + 1) * heights[i])
        }
        
        
每次的遍历，都没与记录任何有用信息。优化点，我们需要拿空间来换时间。

单调栈+哨兵，见识到了哨兵的威力。单链表常用的思想 dummyHead 也是哨兵的思想。        