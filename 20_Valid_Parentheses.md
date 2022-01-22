# Description (Easy)
Given a string s containing just the characters `'('`, `')'`, `'{'`, `'}'`, `'['` and `']'`, determine if the input string is valid.

An input string is valid if:

1. Open brackets must be closed by the same type of brackets.
2. Open brackets must be closed in the correct order.

### Example1
```
Input: s = "()"
Output: true
```

### Example2
```
Input: s = "()[]{}"
Output: true
```

### Example3
```
Input: s = "(]"
Output: false
```

# Solution1
```java
public static boolean solve1(String str) {
        Stack<Character> workList = new Stack<>();

        for (Character c : str.toCharArray()) {
            switch (c) {
                case ('('):
                    workList.push(')');
                    break;
                case ('{'):
                    workList.push('}');
                    break;
                case ('['):
                    workList.push(']');
                    break;
                case (')'):
                case ('}'):
                case (']'):
                    if (!workList.isEmpty() && workList.peek() == c) {
                        workList.pop();
                    } else {
                        return false;
                    }
            }
        }
        return workList.isEmpty();
    }
```

# Solution2
```java
public static boolean solve2(String str) {
        Stack<Character> stack = new Stack<>();
        for (char c : str.toCharArray()) {
            if (c == '(') {
                stack.push(')');
            } else if (c == '{') {
                stack.push('}');
            } else if (c == '[') {
                stack.push(']');
            } else if (stack.isEmpty() || stack.pop() != c) {
                return false;
            }
        }
        return stack.isEmpty();
    }
```
