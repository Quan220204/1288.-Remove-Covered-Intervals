# 1288.-Remove-Covered-Intervals
Leetcode exercise

Given an array intervals where intervals[i] = [li, ri] represent the interval [li, ri), remove all intervals that are covered by another interval in the list.The interval [a, b) is covered by the interval [c, d) if and only if c <= a and b <= d.Return the number of remaining intervals.

Example 1:
Input: intervals = [[1,4],[3,6],[2,8]]
Output: 2
Explanation: Interval [3,6] is covered by [2,8], therefore it is removed.

Example 2:
Input: intervals = [[1,4],[2,3]]
Output: 1
 
Constraints:
1 <= intervals.length <= 1000
intervals[i].length == 2
0 <= li < ri <= 105

All the given intervals are unique.


    This exercise is quite interesting, the idea is to sort for ascending left limit(l) and descending right(r) limit, 
    this time I'm a bit regretful because at first I thought of this idea but thought I couldn't sort like that so i missed it.
    
    After a while I came up with a strict explanation for my idea:
    
    The array is sorted so that l increases and r decreases, then we have intervals[i] -> intervals[j], each of these 
    segments has l respectively l = k0, k1, ..., km(k[i]<k[i+p) ], p>0) infer that intervals with l=k[i+p] cannot cover 
    intervals with l=k[i] (1)
    
    Let's consider intervals[i] -> intervals[q] has l = k0 and r is decreasing and since all the given intervals are 
    unique , none of the r in this interval is equal associated with interval [a, b) condition is covered by the interval
    [c, d) if and only if c <= a and b <= d so intervals[i+p] cannot cover intervals[i](p>1) (2)
    
    Combine (1),(2) we conclude intervals[i] cannot be covered by intervals[i+p](p>0).
    
    I sort as follows using lambda function, I admit that sort in python is very convenient, I add a minus sign to 
    sort descending.
    
    class Solution:
        def removeCoveredIntervals(self, intervals: List[List[int]]) -> int:
              intervals.sort(key = lambda i:(i[0], -i[1]))
              # I create a stack containing intervals that cannot be covered
              stack = [intervals[0]]
              for l,r in intervals:
              if l >= stack[-1][0] and r <= stack[-1][1]:
                continue
              # If previous intervals can't cover it, I put it on the stack
              else:
                stack.append([l,r])
              # Finally I return the length of the stack
              return len(stack)
