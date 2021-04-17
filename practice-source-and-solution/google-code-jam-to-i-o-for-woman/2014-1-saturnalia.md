# 2014 \(1\) Saturnalia

## Description 

[**Code Jam to I/O for Woman 2014 \(1\) Saturnalia \(15 pts\)**](https://codingcompetitions.withgoogle.com/codejamio/round/0000000000050ed0/0000000000050df2)\*\*\*\*

> ### Problem \(Simplified\)
>
> Caterina needs a computer program that reads a message and outputs it back, decorated with a box.
>
> ### Input
>
> The first line of the input gives the number of test cases, **T**. **T** lines follow. Each line contains a text message.
>
> ### Output
>
> For each test case, output four lines. The first one should contain "Case \#x:", where x is the test case number \(starting from 1\). The next 3 lines should contain the original message surrounded by a box of '+', '-', and '\|' characters, with a space character added on each side of the message. See examples below for the exact formatting requirements. Pay special attention to the spaces.
>
> ### Sample
>
> Input:
>
> ```text
> 5
> Merry Saturnalia, Giovanni!
> Equus, you're the best!
> Caballus, you try really hard!
>    
> w
> ```
>
> Output:
>
> ```text
> Case #1:
> +-----------------------------+
> | Merry Saturnalia, Giovanni! |
> +-----------------------------+
> Case #2:
> +-------------------------+
> | Equus, you're the best! |
> +-------------------------+
> Case #3:
> +--------------------------------+
> | Caballus, you try really hard! |
> +--------------------------------+
> Case #4:
> +-----+
> |     |
> +-----+
> Case #5:
> +---+
> | w |
> +---+
> ```

## Solution 

We just need to output the required characters at the right position

```java
import java.util.*;
import java.io.*;

public class Solution {
    public static void main(String[] args) {
        Scanner in = new Scanner(new BufferedReader(new InputStreamReader(System.in)));
        int t = in.nextInt(); 
        String s = in.nextLine(); // jump to the next line
        for (int i = 1; i <= t; ++i) {
            s = in.nextLine(); // read the string
            int len = s.length();
            System.out.print("Case #" + i + ":\n+-");
            for(int j=0; j<len; j++) {
                System.out.print("-");
            }
            System.out.print("-+\n| " + s + " |\n+-");
            for(int j=0; j<len; j++) {
                System.out.print("-");
            }
            System.out.println("-+");
        }
    }
}
```

* **Time Complexity: O\(n\)** n is the length of the input string
* **Space Complexity: O\(n\)**

## Summary

* As the very first task of Code Jam to I/O, it is quite simple

