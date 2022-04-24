# Searching And sorting

# Topic : Binary Search

<aside>
💡 It can be applied to sorted array as well as unsorted array ( ex: mountain array). In bs we have to check if the **mid has the answer if it doesn’t have just discard the left or the right part on the condition**

</aside>

## Questions done

- Floor and ceil
    
    ## Approach
    
    To find floor of a element just apply simple binary search and we observe one thing that after loop ends `low>high` so low will be the ceil and `high` will be the floor
    
    Explanation
    
    ![Untitled](Searching%20And%20Sorting%20e8359abc6e464fa0a9df47225d82a178/Untitled.png)
    
    ```java
    static int search(int[] arr,int target){
    	int low = 0 , high = arr.length-1;
    		while(low<=high){
    			int mid = low + (high-low)/2;
    			if(arr[mid]==target) return mid;
    			else if(arr[mid]<target) low = mid+1;
    			else high = mid-1;
    			}
    		return low // if to find ceil else return ceil
    		
    }
    ```
    
    ---
    
- First and last position of an array
    
    ## Approach
    
    simply apply binary Search and if we encounter a element which can be a possible answer just **discard the right part if we have to find first occurrence else discard the left part .** 
    
    Explanation
    
    ```java
    static int binarySearch(int[] arr,int target,boolean isFisrtOccurence){
    	int low = 0, high = arr.length-1;
            int ans = -1;
            while(low<=high){
    			           int mid = low+(high-low)/2;
                    if(arr[mid]<target) low = mid+1;
                    else if(arr[mid]>target) high = mid-1;
                    else{
                        ans = mid;
                        if(isFirstOccurence) high = mid-1;
                        else low = mid+1;
                    }
                }
            return ans;
            }
    }
    ```
    
    ---
    
- Find in an infinitely ♾️ sorted array
    
    ## Approach
    
    The array has infinite size so we don’t know the size of the array hence we have to find the range in which the element lies and then apply `binarySearch.` To find range we will check if `arr[high]>=target` 
    
    then we can say that element is between `low` and `high`.
    
    Explanation
    
    ```java
    public static int findRange(int[] arr,int target){
         int low = 0, high = arr.length-1;
         while (target>arr[high]){
             int temp = high;
             high += (high-low+1)*2;
             low = high+1;
         }
          return binarySearch(arr,low,high,target);
        }
    ```
    
    ---
    
- Index of first 1 in ♾️ sorted binary array
    
    ## Approach
    
    The idea will be same just find the range where 1 lies than apply binary search
    
    Explanation
    
    ```java
    public static int findPos(int[] arr) {
       int low = 0, high = arr.length - 1;
       while (arr[high] == 0) {
          int temp = high;
          high += (high - low + 1) * 2;
          low = high + 1;
       }
       // we got the range just apply binarySearch by finding the first occurence of 1
       while (low <= high) {
          int pos = -1;
          int mid = (low + high) / 2;
          if (arr[mid] == 1) {
             pos = mid;
             high = mid - 1;
          } else low = mid + 1;
       }
       return pos;
    }
    ```
    
    ---
    
- Peak of an Mountain array
    
    ## Approach
    
    The array is first increasing then decreasing after the peak element so we know that peak element has the unique property of `mid>mid+1` but it is also true for the decreasing part so we have to find the breakpoint from where array start decreasing so when `low==high` it is where peak element lies
    
    Explanation
    
    ```java
    static int findPeakIndex(int[] arr,int n){
            int low = 0 , high = n-1;
            while (low<high){
                int mid = low + (high-low)/2;
                if(arr[mid]<=arr[mid+1]) low = mid+1;
                else high = mid; 
    					// it can be the peak but we have to find the greatest possible peak
            }
    			// Here low==high and we know that this will be the peak
            return low;
        }
    ```
    
    ---
    
- Search in a Bitonic/Mountain array
    
    ### Approach 1 (find bitonic point then apply binary search)
    
    It is simple just copy above code to find bitonic/peak point than apply binary search in both half as they are sorted
    
    Explanation
    
    ```java
    static int search(int[] arr,int target){
    		int bitonicPoint = findPeakIndex(arr);
    		if(arr[bitonicPoint]==target) return idx;
    		int idx = binarySearch(0,bitonicPoint-1);
    		if(idx!=-1) idx = binarySearch(arr,bitonicPoint+1,arr.length-1);
    		return idx;
    }
    ```
    
    ### Approach 2 (binary seach in one pass)
    
    It is just based on the idea that we know that array is sorted until bitonic point in ascending order than in descending order so we take advantage of this and apply **`binarySearch`** 
    
    ```java
    public int solve(int[] arr, int target) {
            int low = 0 , high = arr.length-1;
            while(low<=high){
                int mid = low+(high-low)/2;
                if(arr[mid]==target) return mid;
                else if(mid+1<arr.length && arr[mid]<arr[mid+1]){
                    // we are in increasing part of the array
                     if(arr[mid]<target) low = mid+1;
                    else high = mid-1;
                }
                else{
    								// we are in decreasing part of array
                    if(arr[mid]<target) high = mid-1;
                    else low = mid+1;
                }
            }
            return -1;
        }
    ```
    
    ---
    
- Min element or no. of rotations in a rotated sorted array
    
    Well the index 💥 where the minimum element lies is equal to the no. of rotations so that’s why these questions are identical 😀. 
    
    ## Approach
    
    We have to search 🔍 for the minimum element and there is a unique property which we can observe that 
    
    - `min` element is always lesser than it’s neighbouring elements
    - The pivot element always lies in the unsorted part of the array just discard the part which is sorted.
    
    ## Explanation
    
    ```java
    static int find(int[] arr,int n){
            int low = 0 , high=n-1;
            if(arr[low]<=arr[high]) return arr[low];
    				//if array is sorted than the min element is present at 0th index
            while (low<=high){
                int mid = low + (high-low)/2;
                if (mid<n && arr[mid]>arr[mid+1]){
                    return arr[mid+1];
                }
                if(mid>0 && arr[mid]<arr[mid-1]){
                    return arr[mid];
                }
                else if(arr[mid]>=arr[low]) high = mid-1;
    							// the right part is sorted so we will check in left unsorted part
                else low = mid+1;
            }
            return -1;
        }
    ```
    
    ---
    
- Search in Rotated Sorted Array
    
    ## Approach 1
    
    The first approach would be to find the pivot element then apply `**binarySearch`** in the both half `arr[] = {4,5,6,0,1,2}`In the above example the pivot is 0 so find it then apply binarySearch
    
    Explanation
    
    ```java
    public int search(int[] nums, int target) {
    	int pivot = findPivot(nums, nums.length);
    	// find the pivot/min element then we will simply apply bs
    	if (nums[pivot] == target) return pivot;
    	int ans = binarySearch(nums, 0, pivot - 1, target);
    	if (ans == -1) ans = binarySearch(nums, pivot + 1, nums.length - 1, target);
    	return ans;
    }
    ```
    
    ## Approach 2 (binarySearch in one pass)
    
    just apply binarySearch  in the sorted part of the array and check
    
    - which part is sorted
    - if target is in range of sorted part than compress search space to that range
    
    Explanation
    
    ```java
    public int search(int[] nums, int target)
     {
     	int low = 0, high = nums.length - 1;
     	while (low <= high)
     	{
     		int mid = low + (high - low) / 2;
     		if (nums[mid] == target) return mid;
     		if (nums[mid] >= nums[low])
     		{
    			// if we come here this means that left part is sorted
     			if (target >= nums[low] && target <= nums[mid]) high = mid - 1;
    			// we are checking that if element lies in range or not
     			else low = mid + 1;
     		}
     		else
     		{
     			if (target >= nums[mid] && target <= nums[high]) low = mid + 1;
     			else high = mid - 1;
     		}
     	}
    
     	return -1;
     }
    ```
    
    ---
    
- Search in a ****Rotated Sorted Array with duplicates
    
    ## Approach
    
    The logic will always be same as the above variation of this question just check that our low, mid and high are pointing to same element or not if they are just do `low++` and `high—` .
    
    Explanation
    
    ```java
    public boolean search(int[] nums, int target)
    	{
    		int low = 0, high = nums.length - 1;
    		while (low <= high)
    		{
    			int mid = (low + high) / 2;
    			if (nums[mid] == target) return true;
    			else if (nums[mid] == nums[low] && nums[mid] == nums[high])
    			{
    				low++;
    				high--;
    			}
    			else if (nums[mid] >= nums[low])
    			{
    				// if we come here this means that left part is sorted
    				if (target >= nums[low] && target <= nums[mid]) high = mid - 1;
    				// we are checking that if element lies in range or not
    				else low = mid + 1;
    			}
    			else
    			{
    				if (target >= nums[mid] && target <= nums[high]) low = mid + 1;
    				else high = mid - 1;
    			}
    		}
            return false;
    	}
    ```
    
    ---
    
- Find in a nearly sorted array
    
    Given a nearly sorted array such that each of its elements may be misplaced by no more than one position from the correct sorted order. An element at index `i` in a correctly sorted order can be misplaced by the ±1 position, i.e., it can be present at index `i-1` or `i` or `i+1` in the input array.
    
    ## Approach
    
    We know that element can differ in ±1 position so we will always check in that range . This will make it this array as normal sorted array
    
    Explanation
    
    ```java
    public static int findIndex(int[] nums, int target) {
    		int n = nums.length;
    		int low = 0, high = n - 1;
    		while (low <= high) {
    			int mid = low + (high - low) / 2;
    			if (nums[mid] == target) return mid;
    			else if (mid + 1 < n && nums[mid + 1] == target) return mid + 1;
    			else if (mid - 1 >= 0 && nums[mid - 1] == target) return mid - 1;
    			else if (nums[mid]<target) low = mid + 2;
    			else high = mid - 2;
    		}
    		return -1;
    	}
    ```
    
    ---
    
- Min difference element in a sorted array
    
    we have to find the element which has the min absolute difference with target
    
    ## Approach
    
    Observe one thing that the element will have min diff with itself which is 0. The difference can’t go **-ve** as ****it is absolute and whenever we move left and right in the array difference increases due to it’s sorted nature. 
    
    - find the target itself in the array if it doesn’t exist do the below thing
        - find the floor and ceil of the target and the answer will be the min diff of both of them
    
    Explanation
    
    ```java
    public int findMinDiff(int[] arr, int target) {
       int low = 0, high = arr.length;
       while (low <= high) {
          int mid = low + (high - low) / 2;
          if (arr[mid] == target) return 0;
          else if (arr[mid] < target) low = mid + 1;
          else high = mid - 1;
       }
       int diff1 = Math.abs(arr[low] - target);
       int diff2 = Math.abs(arr[high] - target);
       return Math.min(diff1, diff2);
    }
    ```
    
    ---
    
- Allocate Minimum number of pages 📘
    
    There are **n** books and **m** students we are given an array in which `a[i]` represents the pages of the book . We have to give atleast one book to each student and minimise the maximum no. of pages one student read so that he doesn’t have much burden. In other words the maximum pages one student read has to be minimised.
    
    ## Approach
    
    We can apply **bs** the range would be low = min(array) high = sum(array) and then check if mid can be the possible ans or not if it is shrink the range to left else if it isn’t the answer we have to increase the capacity of pages one can read.
    
    Explanation
    
    ```java
    public class Solution {
      public int books(int[] arr, int m) {
        if (m > arr.length) {
          // The books are less than the students
          return -1;
        }
        int min = arr[0], sum = arr[0];
        for (int i = 1; i < arr.length; i++) {
          min = Math.min(min, arr[i]);
          sum += arr[i];
        }
        int low = min, high = sum;
        int ans = -1;
        while (low <= high) {
          int mid = low + (high - low) / 2;
          if (isValid(arr, mid, m)) {
            high = mid - 1;
            ans = mid;
          } else low = mid + 1;
        }
        return ans;
      }
      public boolean isValid(int[] arr, int max, int m) {
        int students = 1;
        int sum = 0;
        for (int i = 0; i < arr.length; i++) {
          if (arr[i] > max) return false;
          sum += arr[i];
          if (sum > max) {
            students++;
            if (students > m) return false;
            sum = arr[i];
          }
        }
        return true;
      }
    }
    ```
    
    ---
    
- Painter’s🎨 Partition Problem
    
    ### same as above problem so I don’t think I should explain again
    
    ---
    
- Aggressive Cows 🐮
    
    We are given N cows where N≥2 and m stalls we have to place them in such a manner that their minimum distance is maximized.
    
    ## Approach
    
    We will first sort the array than low will become `a[1]-a[0]` and high will be `a[n-1]-a[0]` **Placing the  cows are quite simple just** always place first cow at 0th position and check if we find a suitable position where cow can be placed ***increment the cows and if we can’t place cow search in high = mid-1 as if we go in right we will be never be able to place as they are all greater than mid***  
    
    Explanation
    
    ```java
    public static int findMin(int[] arr,int C){
            Arrays.sort(arr);
            int ans = -1;
            int low = arr[1]-arr[0];
            int high = arr[arr.length-1]-arr[0];
            while (low<=high){
                int mid = low + (high-low)/2;
                if (isValid(arr,C,mid)) {
                    ans = mid;
                    low = mid+1;
                }
                else high =  mid-1;
            }
            return ans;
        }
        public static boolean isValid(int[] arr,int C,int min){
            int lastPlace = arr[0];
            int cows = 1;
            for (int i=1;i<arr.length;i++){
                int diff = arr[i]-lastPlace;
                if (diff>=min){
                    cows++;
                    if (cows==C) return true;
                    lastPlace = arr[i];
                }
            }
            return false;
        }
    ```
    
    ---
    

## Searching questions

- Square root of an integer
    
    ## Approach
    
    We have to find the number of perfect squares less than n 
    
    This is always the `sqrt(N-1)` number of perfect squares less than n 
    
- Missing and repeating numbers
    
    > a set {1,2,3,...,n} from this an array is derived which contains element with one missing and one duplicate number
    > 
    
    ## Approach
    
    Take one extra array which stores the frequency of the number if we visit one number just increment that index . the answer will be in the extra array the position with value 0 is unique and pos with value 2 is repeating element.
    
    Explanation
    
    ```java
    int[] findTwoElement(int arr[], int n)
     {
       int[] dp = new int[n + 1];
       for (int i = 0; i < n; i++)
       {
         dp[arr[i]]++;
       }
       int[] ans = new int[2];
       for (int i = 1; i <= n; i++)
       {
         if (dp[i] == 0) ans[1] = i;
         if (dp[i] == 2) ans[0] = i;
       }
       return ans;
     }
    ```
    
- Find majority element
    
    > Find element which occur more than n/2 times in the array.
    > 
    
    ## Approach
    
    Refer to copy notes for this
    
    ## Explanation
    
    ```java
    public static int majorityElement(int a[], int size) {
        int elm = 0, cnt = 0;
        for (int i: a) {
           if (cnt == 0) {
              elm = i;
              cnt++;
           } else if (elm == i) cnt++;
           else cnt--;
        }
        cnt = 0;
        for (int i: a) {
           if (i == elm) cnt++;
        }
        if (cnt > size / 2) return elm;
        return -1;
     }
    ```
    
- Find Pair Which has Given Difference
    
    ## Approach (Bruteforce)
    
    Just simply itereate the array and find a valid pair
    
    ## Explanation
    
    ```java
    public boolean findPair(int arr[], int size, int n)
        {
            for(int i=0;i<size;i++){
                for(int j=i+1;j<size;j++){
                    int diff = Math.abs(arr[i]-arr[j]);
                    if(diff==n) return true;
                }
            }
            return false;
        }
    ```
    
    ## Approach2 (Binary Search)
    
    ## Explanation
    
    - Sort the array
    - Linearly traverse the array and apply binary search on the rest half of the array for finding the other element
    
    ```java
    public boolean findPair(int arr[], int size, int n)
        {
            Arrays.sort(arr);
            for(int i=0;i<size;i++){
                int target = n+arr[i];
                int low = i+1, high = size-1;
                while(low<=high){
                    int mid = (low+high)/2;
                    if(arr[mid]==target) return true;
                    else if(arr[mid]<target) low = mid+1;
                    else high = mid-1;
                }
            }
            return false;
        }
    ```
    
- 4 sum
    
    ## Approach 1 (BruteForce) O($n^3logn$)
    
    As we have to find the 4 numbers equal to sum we will take 3 pointers `i` , `j` and `k`  where:
    
    - i = 0 to n
    - j = `i+1` to n
    - k = `j+1` to n
    
    and apply binary search to find the 4th element 
    
    > We have to find unique quads so we will take set to store ans
    > 
    
    ## Explanation
    
    ```java
    public List<List < Integer>> fourSum(int[] nums, int target)
    {
      List<List < Integer>> lst = new ArrayList < > ();
      Set<List < Integer>> sv = new HashSet < > ();
      Arrays.sort(nums);
      int n = nums.length;
      for (int i = 0; i < n; i++)
        for (int j = i + 1; j < n; j++)
          for (int k = j + 1; k < n; k++)
          {
            int low = k + 1, high = n - 1;
            int sum = nums[i] + nums[j] + nums[k];
            while (low <= high)
            {
              int mid = low + (high - low) / 2;
              if (nums[mid] == target - sum)
              {
                List<Integer> temp = new ArrayList < > ();
                temp.add(nums[i]);
                temp.add(nums[j]);
                temp.add(nums[k]);
                temp.add(nums[mid]);
                sv.add(temp);
                break;
              }
              else if (nums[mid] < target - sum) low = mid + 1;
              else high = mid - 1;
            }
          }
    
      for (List<Integer> l: sv) lst.add(l);
      return lst;
    }
    ```
    
    ## Approach 2 (2 pointer)
    
    Explanation
    
    ```java
    class Solution
    {
    	public List<List < Integer>> fourSum(int[] nums, int target)
    	{
    		List<List < Integer>> ans = new ArrayList < > ();
    		Arrays.sort(nums);
    		int n = nums.length;
    		for (int i = 0; i < n; i++)
    		{
    			for (int j = i + 1; j < n; j++)
    			{
    				int left = j + 1, right = n - 1;
    				int sum = nums[i] + nums[j];
    				while (left < right)
    				{
    					int total = sum + nums[left] + nums[right];
    					if (total == target)
    					{
    						List<Integer> lst = new ArrayList < > ();
    						lst.add(nums[i]);
    						lst.add(nums[j]);
    						lst.add(nums[left]);
    						lst.add(nums[right]);
    						ans.add(lst);
    						while (left < right && nums[left] == lst.get(2)) left++;
    						while (left < right && nums[right] == lst.get(3)) right--;
    					}
    					else if (total < target) left++;
    					else right--;
    				}
    
    				while (j + 1 < n && nums[j] == nums[j + 1]) j++;
    			}
    
    			while (i + 1 < n && nums[i] == nums[i + 1]) i++;
    		}
    
    		return ans;
    	}
    }
    ```
    
- Searching in an array where adjacent differ by at most k
    
    ## Approach
    
    We know here that difference is always differ atmost k so we can take advantage of it by fist taking difference with the target than as we know each step is of k difference so divide the `diff` with k.
    
    ***Formula*** = `abs(a[i]-target)/k`
    
    ## Explanation
    
    ```java
    public static int search(int arr[], int n, int x, int k)
    {
    	int i = 0;
    	while (i < n)
    	{
    		if (arr[i] == x) return i;
    		i += Math.max(1, Math.abs(arr[i] - x) / k);
    	}
    	return -1;
    }
    ```
    
- House Robber 💰
    
    > We have to find the max amount we can get without going on consecutive index
    > 
    
    ## Approach 1 (Recursion)
    
    Explanation
    
    We have 2 choices
    
    1. select the **`i+1`** element
    2. select `i+2` element in this case we have to add current element in the answer
    
    We will take max of both choices
    
    ```java
    int rob(int[] nums, int i)
    {
       if (i >= nums.length) return 0;
       return Math.max(rob(nums, i + 1), rob(nums, i + 2) + nums[i]);
    }
    ```
    
    ## Approach 2 (dp)
    
    ```java
    public int rob(int[] nums)
     {
        int n = nums.length;
        if (n == 1) return nums[0];
        int[] dp = new int[n];
        dp[0] = nums[0];
        dp[1] = Math.max(dp[0], nums[1]);
        for (int i = 2; i < n; i++)
        {
           dp[i] = Math.max(dp[i - 1], dp[i - 2] + nums[i]);
        }
    
        return dp[n - 1];
     }
    ```
    
- Count triplets with sum smaller than X
    
    ## Approach O($n^2$)
    
    We can first sort the array than iterate through it and if we get a sum which is less tha targetSum than we know that if we move left through `**k**` all the sum will be less than targetSum till `**j`** 
    
    Explanation 
    
    ```java
    long countTriplets(long arr[], int n, int sum)
       {
         long ans = 0;
         Arrays.sort(arr);
         for (int i = 0; i < n - 1; i++)
         {
           int j = i + 1, k = n - 1;
           while (j < k)
           {
             long curr_sum = arr[i] + arr[j] + arr[k];
             if (curr_sum < sum)
             {
               ans += (k - j);
               j++;
             }
             else
             {
               k--;
             }
           }
         }
         return ans;
    ```
    
- Count subarrays with 0 sum ➕
    
    ## Approach
    
    We will take a sum which will store contiginous sum and whenever a sum value is repeated we got one subarray with 0 sum
    
    Explanation
    
    ```java
    public static long findSubarray(long[] arr ,int n) 
        {
            long ans = 0;
            Map<Long,Integer> map = new HashMap<>();
    				// we are storing frequency of sum
            map.put(0l,1);
            long sum = 0;
            for(int i=0;i<n;i++){
                sum += arr[i];
                if(map.containsKey(sum)){
                    ans += map.get(sum);
                }
                map.put(sum,map.getOrDefault(sum,0)+1);
            }
            return ans;
        }
    ```
    
- Product 📏 of Array Except Self
    
    ## Approach 1 (Bruteforce)
    
    iterate the array and find product of each term
    
    ## Approach 2 (Dividing)
    
    take product of the whole array and divide by each element to get the product of individual element But there is a catch 
    
    - if array contains 2 or more zeros than whole product array is 0
    - if array has 1 zero than all the elements will have 0 product except 0 which will have product of each term
    
    ## Approach 3 (using prefix arrays)
    
    we will take 2 arrays left and right these will store product of each term from left and right and the ith element
    
    - **`ans[i] = left[i-1] * right[i+1]`**
    
    ## Approach 4 (using constant space)
    
    In the answer array store the left product than take one variable pro which stores the product from right than do the same thing
    
    Explanation
    
    ```java
    public int[] productExceptSelf(int[] nums) {
            int n = nums.length;
            int[] ans = new int[n];
            ans[0] = nums[0];
            for(int i=1;i<n;i++){
                ans[i] = ans[i-1]*nums[i];
            }
            int pro = 1;
            for(int i=n-1;i>=0;i--){
                   ans[i] = i-1>=0?ans[i-1]:1;
                   ans[i] *= pro;
                   pro *= nums[i];
            }
            return ans;
        }
    ```
    
- Sort array according to count of set bits 1️⃣
    
    ## Approach
    
    Just sort the array and comparaor function will compare on basis of setBit counts
    
- Minimum Swaps to Sort
    
    ## Approach
    
    Make one arraylist of type pair and add all the elements of the input array with it index than sort the arrayList and compare it with the input array if index are same than no swap required else swap is required don’t increment i as the the swapped may also not be in it’s correct position
    
    Explanation
    
    ```java
    public int minSwaps(int nums[])
     {
       ArrayList<Pair> arr = new ArrayList < > ();
       int n = nums.length;
       for (int i = 0; i < n; i++) arr.add(new Pair(nums[i], i));
       arr.sort(Comparator.comparingInt((Pair x)->x.first));
       int swaps = 0;
       for (int i = 0; i < n; i++)
       {
         if (arr.get(i).second != i)
         {
           Collections.swap(arr, i, arr.get(i).second);
           swaps++;
           i--;
         }
       }
       return swaps;
     }
    ```
    
- Check if number exists in **AP** or not
    
    ## Approach
    
    simply check if (C-A)%d == 0 && $(C-A)/d≥0$
    
- Smallest factorial number with n trailing 0️⃣s
    
    We know that the number of trailing zeros depend on how much 2s and 5s are there in the number as $2*5 = 10$ . The number of 2s in a number is always greater than number of 5 so counting no. of 5 will tell us the answer. But there is a catch if we count only 5 we will miss numbers which contains exponents of 5 such as 25 it contains two 5s so we will apply the below formula:
    
    $$
    ans = |x/5| + |x/25| +  |x/125| + ... +  |x/25^n|
    $$
    
    In the above formula we are counting exponents of 5 appearing in the number and this will give our answer 
    
    Now for this question we have to find a range and apply binarySearch in it range will be from 0 to 5*n as a the max number which can contain n trailing zeros is 5n.
