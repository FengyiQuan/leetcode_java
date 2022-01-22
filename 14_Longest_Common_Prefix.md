# Description (Easy)
Write a function to find the longest common prefix string amongst an array of strings.

If there is no common prefix, return an empty string `""`.
### Example1
```
Input: strs = ["flower","flow","flight"]
Output: "fl"
```
### Example2
```
Input: strs = ["dog","racecar","car"]
Output: ""
Explanation: There is no common prefix among the input strings.
```
# Tips
- `public static short[] copyOfRange(short[] original, int from, int to)`
- `boolean startsWith(String str)`
- `boolean startsWith(String str, index fromIndex)`
- `int indexOf(String searchValue)` The index of the first occurrence of searchValue, or -1 if not found.
# Solution1
```java
public static String solve1(String[] strs) {
        StringBuilder res = new StringBuilder();
        int shortestLength = getShortestLength(strs);

        for (int j = 0; j < shortestLength; j++) {
            char current = strs[0].charAt(j);
            for (int i = 1; i < strs.length; i++) {

                if (strs[i].charAt(j) != current) {
                    return res.toString();
                }
            }
            res.append(current);
        }
        return res.toString();
    }
private static int getShortestLength(String[] strs) {
      int res = Integer.MAX_VALUE;
      for (String s : strs) {
          if (s.length() < res) {
              res = s.length();
          }
      }
      return res;
  }
```
# Solution2
```java
public static String solve2(String[] strs) {
    if (strs == null || strs.length == 0) {
        return "";
    }
    String res = strs[0];

    for (int i = 1; i < strs.length; i++) {
        res = findCommonPrefix(res, strs[i]);
    }
    return res;
}
private static String findCommonPrefix(String s1, String s2) {
    StringBuilder res = new StringBuilder();
    int shorterLength = Math.min(s1.length(), s2.length());
    for (int i = 0; i < shorterLength; i++) {
        if (s1.charAt(i) == s2.charAt(i)) {
            res.append(s1.charAt(i));
        } else {
            return res.toString();
        }
    }
    return res.toString();
}
```

# Solution3
divide and conquer

`Arrays.copyOfRange(short[] original, int from, int to)`

`from` inclusive

`to` exclusive

```java
public static String solve3(String[] strs) {
      if (strs == null || strs.length == 0) {
          return "";
      }
      if (strs.length == 1) {
          return strs[0];
      } else if (strs.length == 2) {
          return findCommonPrefix(strs[0], strs[1]);
      } else {
          String[] leftArray = Arrays.copyOfRange(strs, 0, (strs.length + 1) / 2);
          String[] rightArray = Arrays.copyOfRange(strs, (strs.length + 1) / 2, strs.length);
          String leftPrefix = solve3(leftArray);
          String rightPrefix = solve3(rightArray);
          return findCommonPrefix(leftPrefix, rightPrefix);
      }
  }
```

# Solution4: Horizontal scanning
```java
public static String solve4(String[] strs) {
      if (strs == null || strs.length == 0) {
        return "";
      }
      String prefix = strs[0];
      for (int i = 1; i < strs.length; i++)
          while (strs[i].indexOf(prefix) != 0) {
              prefix = prefix.substring(0, prefix.length() - 1);
              if (prefix.isEmpty()) return "";
          }
      return prefix;
  }
```

# Solution5: Vertical scanning
```java
public static String solve5(String[] strs) {
        if (strs == null || strs.length == 0) {
            return "";
        }
        for (int i = 0; i < strs[0].length(); i++) {
            char c = strs[0].charAt(i);
            for (int j = 1; j < strs.length; j++) {
                if (i == strs[j].length() || strs[j].charAt(i) != c)
                    return strs[0].substring(0, i);
            }
        }
        return strs[0];
    }
```

# Solution6: Divide and conquer
```java
public String longestCommonPrefix(String[] strs) {
    if (strs == null || strs.length == 0) {
      return "";
    }
  return longestCommonPrefix(strs, 0 , strs.length - 1);
}

private String longestCommonPrefix(String[] strs, int l, int r) {
    if (l == r) {
        return strs[l];
    }
    else {
        int mid = (l + r)/2;
        String lcpLeft =   longestCommonPrefix(strs, l , mid);
        String lcpRight =  longestCommonPrefix(strs, mid + 1,r);
        return commonPrefix(lcpLeft, lcpRight);
   }
}

String commonPrefix(String left,String right) {
    int min = Math.min(left.length(), right.length());       
    for (int i = 0; i < min; i++) {
        if ( left.charAt(i) != right.charAt(i) )
            return left.substring(0, i);
    }
    return left.substring(0, min);
}
```

# Solution7: Binary search
```java
public static String solve7(String[] strs) {
        if (strs == null || strs.length == 0) {
            return "";
        }
        int minLen = getShortestLength(strs);
        int low = 1;
        int high = minLen;
        while (low <= high) {
            int middle = (low + high) / 2;
            if (isCommonPrefix(strs, middle))
                low = middle + 1;
            else
                high = middle - 1;
        }
        return strs[0].substring(0, (low + high) / 2);
    }
// len is to indicate if index of 0-len is a common prefix in all strings
private static boolean isCommonPrefix(String[] strs, int len) {
        String str1 = strs[0].substring(0, len);
        for (int i = 1; i < strs.length; i++)
            if (!strs[i].startsWith(str1)) {
                return false;
            }
        return true;
    }
```
