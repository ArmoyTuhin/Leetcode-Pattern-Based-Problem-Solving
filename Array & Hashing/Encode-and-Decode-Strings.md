# Encode and Decode Strings

**Difficulty:** Medium  

## Problem Statement

Design an algorithm to encode a list of strings to a string. The encoded string is then sent over the network and is decoded back to the original list of strings.

Machine 1 (sender) has the function:
```
string encode(vector<string> strs) {
  // ... your code
  return encoded_string;
}
```

Machine 2 (receiver) has the function:
```
vector<string> decode(string s) {
  // ... your code
  return strs;
}
```

So Machine 1 does:
```
string encoded_string = encode(strs);
```

and Machine 2 does:
```
vector<string> strs2 = decode(encoded_string);
```

`strs2` in Machine 2 should be the same as `strs` in Machine 1.

Implement the `encode` and `decode` methods.

You are not allowed to solve the problem using any serialize methods (such as `eval`).

**Example 1:**
```
Input: dummy_input = ["Hello","World"]
Output: ["Hello","World"]
Explanation:
Machine 1:
Codec encoder = new Codec();
String msg = encoder.encode(strs);
Machine 1 ---msg---> Machine 2

Machine 2:
Codec decoder = new Codec();
String[] strs = decoder.decode(msg);
```

**Example 2:**
```
Input: dummy_input = [""]
Output: [""]
```

## Brief Explanation

We need to encode a list of strings into a single string and decode it back. The challenge is handling edge cases like empty strings, strings containing delimiters, and ensuring we can uniquely identify where each string starts and ends.

## Approach 1: Length Prefix with Delimiter

### Explanation
Encode each string by prefixing its length followed by a delimiter. This allows us to know exactly how many characters belong to each string when decoding.

### Pseudocode
```
encode(strs):
    result = ""
    for each str in strs:
        result += length(str) + "#" + str
    return result

decode(s):
    result = []
    i = 0
    while i < length(s):
        length = 0
        while s[i] != '#':
            length = length * 10 + (s[i] - '0')
            i++
        i++ // skip '#'
        result.add(s.substring(i, i + length))
        i += length
    return result
```

### Java Code
<div style="background-color: #1e1e1e; padding: 15px; border-radius: 5px; overflow-x: auto; margin: 10px 0;">
<pre style="background-color: #1e1e1e; color: #d4d4d4; margin: 0; padding: 0; font-family: 'Consolas', 'Monaco', 'Courier New', monospace; font-size: 14px; line-height: 1.5;"><code style="background-color: #1e1e1e; color: #d4d4d4;">public class Codec {
    public String encode(List&lt;String&gt; strs) {
        StringBuilder sb = new StringBuilder();
        for (String str : strs) {
            sb.append(str.length()).append(&quot;#&quot;).append(str);
        }
        return sb.toString();
    }
    public List&lt;String&gt; decode(String s) {
        List&lt;String&gt; result = new ArrayList&lt;&gt;();
        int i = 0;
        while (i &lt; s.length()) {
            int length = 0;
            while (s.charAt(i) != '#') {
                length = length * 10 + (s.charAt(i) - '0');
                i++;
            }
            i++; // skip '#'
            result.add(s.substring(i, i + length));
            i += length;
        }
        return result;
    }
}</code></pre>
</div>
</div>
### C++ Code
<div style="background-color: #1e1e1e; padding: 15px; border-radius: 5px; overflow-x: auto; margin: 10px 0;">
<pre style="background-color: #1e1e1e; color: #d4d4d4; margin: 0; padding: 0; font-family: 'Consolas', 'Monaco', 'Courier New', monospace; font-size: 14px; line-height: 1.5;"><code style="background-color: #1e1e1e; color: #d4d4d4;">#include &lt;vector&gt;
#include &lt;string&gt;
class Codec {
public:
    string encode(vector&lt;string&gt;&amp; strs) {
        string result = &quot;&quot;;
        for (string str : strs) {
            result += to_string(str.length()) + &quot;#&quot; + str;
        }
        return result;
    }
    vector&lt;string&gt; decode(string s) {
        vector&lt;string&gt; result;
        int i = 0;
        while (i &lt; s.length()) {
            int length = 0;
            while (s[i] != '#') {
                length = length * 10 + (s[i] - '0');
                i++;
            }
            i++; // skip '#'
            result.push_back(s.substr(i, length));
            i += length;
        }
        return result;
    }
};</code></pre>
</div>
</div>
**Time Complexity:** O(n) where n is total characters  
**Space Complexity:** O(n)
## Approach 2: Escape Character
### Explanation
Use an escape character to handle special cases. Choose a delimiter and escape it when it appears in the original strings. This approach is more complex but handles any character.
### Pseudocode
<pre style="background-color: #1e1e1e; color: #d4d4d4; margin: 0; padding: 0; font-family: 'Consolas', 'Monaco', 'Courier New', monospace; font-size: 14px; line-height: 1.5;"><code style="background-color: #1e1e1e; color: #d4d4d4;">
encode(strs):
    result = &quot;&quot;
    for each str in strs:
        for each char in str:
            if char == DELIMITER:
                result += ESCAPE + DELIMITER
            else:
                result += char
        result += DELIMITER
    return result

decode(s):
    result = []
    current = &quot;&quot;
    i = 0
    while i &lt; length(s):
        if s[i] == ESCAPE and s[i+1] == DELIMITER:
            current += DELIMITER
            i += 2
        else if s[i] == DELIMITER:
            result.add(current)
            current = &quot;&quot;
            i++
        else:
            current += s[i]
            i++
    return result
</code></pre>
### Java Code
<div style="background-color: #1e1e1e; padding: 15px; border-radius: 5px; overflow-x: auto; margin: 10px 0;">
<pre style="background-color: #1e1e1e; color: #d4d4d4; margin: 0; padding: 0; font-family: 'Consolas', 'Monaco', 'Courier New', monospace; font-size: 14px; line-height: 1.5;"><code style="background-color: #1e1e1e; color: #d4d4d4;">import java.util.*;
public class Codec {
    private static final char DELIMITER = '|';
    private static final char ESCAPE = '\\';
    public String encode(List&lt;String&gt; strs) {
        StringBuilder sb = new StringBuilder();
        for (String str : strs) {
            for (char c : str.toCharArray()) {
                if (c == DELIMITER) {
                    sb.append(ESCAPE).append(DELIMITER);
                } else {
                    sb.append(c);
                }
            }
            sb.append(DELIMITER);
        }
        return sb.toString();
    }
    public List&lt;String&gt; decode(String s) {
        List&lt;String&gt; result = new ArrayList&lt;&gt;();
        StringBuilder current = new StringBuilder();
        int i = 0;
        while (i &lt; s.length()) {
            if (i &lt; s.length() - 1 &amp;&amp; s.charAt(i) == ESCAPE &amp;&amp; s.charAt(i + 1) == DELIMITER) {
                current.append(DELIMITER);
                i += 2;
            } else if (s.charAt(i) == DELIMITER) {
                result.add(current.toString());
                current = new StringBuilder();
                i++;
            } else {
                current.append(s.charAt(i));
                i++;
            }
        }
        return result;
    }
}</code></pre>
</div>
</div>
### C++ Code
<div style="background-color: #1e1e1e; padding: 15px; border-radius: 5px; overflow-x: auto; margin: 10px 0;">
<pre style="background-color: #1e1e1e; color: #d4d4d4; margin: 0; padding: 0; font-family: 'Consolas', 'Monaco', 'Courier New', monospace; font-size: 14px; line-height: 1.5;"><code style="background-color: #1e1e1e; color: #d4d4d4;">#include &lt;vector&gt;
#include &lt;string&gt;
class Codec {
private:
    const char DELIMITER = '|';
    const char ESCAPE = '\\';
public:
    string encode(vector&lt;string&gt;&amp; strs) {
        string result = &quot;&quot;;
        for (string str : strs) {
            for (char c : str) {
                if (c == DELIMITER) {
                    result += ESCAPE;
                    result += DELIMITER;
                } else {
                    result += c;
                }
            }
            result += DELIMITER;
        }
        return result;
    }
    vector&lt;string&gt; decode(string s) {
        vector&lt;string&gt; result;
        string current = &quot;&quot;;
        int i = 0;
        while (i &lt; s.length()) {
            if (i &lt; s.length() - 1 &amp;&amp; s[i] == ESCAPE &amp;&amp; s[i + 1] == DELIMITER) {
                current += DELIMITER;
                i += 2;
            } else if (s[i] == DELIMITER) {
                result.push_back(current);
                current = &quot;&quot;;
                i++;
            } else {
                current += s[i];
                i++;
            }
        }
        return result;
    }
};</code></pre>
</div>
</div>
**Time Complexity:** O(n) where n is total characters  
**Space Complexity:** O(n)
## Approach 3: Length Prefix (Optimal)
### Explanation
Similar to Approach 1 but optimized. Use length prefix with a fixed-width format or delimiter. This is the most efficient and handles all edge cases including empty strings and strings containing any characters.
### Pseudocode
<pre style="background-color: #1e1e1e; color: #d4d4d4; margin: 0; padding: 0; font-family: 'Consolas', 'Monaco', 'Courier New', monospace; font-size: 14px; line-height: 1.5;"><code style="background-color: #1e1e1e; color: #d4d4d4;">
encode(strs):
    result = &quot;&quot;
    for each str in strs:
        length = str.length()
        result += formatLength(length) + DELIMITER + str
    return result

decode(s):
    result = []
    i = 0
    while i &lt; length(s):
        length = parseInt(s, i, DELIMITER_POS)
        i = DELIMITER_POS + 1
        result.add(s.substring(i, i + length))
        i += length
    return result
</code></pre>
### Java Code
<div style="background-color: #1e1e1e; padding: 15px; border-radius: 5px; overflow-x: auto; margin: 10px 0;">
<pre style="background-color: #1e1e1e; color: #d4d4d4; margin: 0; padding: 0; font-family: 'Consolas', 'Monaco', 'Courier New', monospace; font-size: 14px; line-height: 1.5;"><code style="background-color: #1e1e1e; color: #d4d4d4;">import java.util.*;
public class Codec {
    public String encode(List&lt;String&gt; strs) {
        StringBuilder sb = new StringBuilder();
        for (String str : strs) {
            sb.append(str.length()).append(&quot;#&quot;).append(str);
        }
        return sb.toString();
    }
    public List&lt;String&gt; decode(String s) {
        List&lt;String&gt; result = new ArrayList&lt;&gt;();
        int i = 0;
        while (i &lt; s.length()) {
            int j = i;
            while (s.charAt(j) != '#') {
                j++;
            }
            int length = Integer.parseInt(s.substring(i, j));
            i = j + 1;
            result.add(s.substring(i, i + length));
            i += length;
        }
        return result;
    }
}</code></pre>
</div>
</div>
### C++ Code
<div style="background-color: #1e1e1e; padding: 15px; border-radius: 5px; overflow-x: auto; margin: 10px 0;">
<pre style="background-color: #1e1e1e; color: #d4d4d4; margin: 0; padding: 0; font-family: 'Consolas', 'Monaco', 'Courier New', monospace; font-size: 14px; line-height: 1.5;"><code style="background-color: #1e1e1e; color: #d4d4d4;">#include &lt;vector&gt;
#include &lt;string&gt;
class Codec {
public:
    string encode(vector&lt;string&gt;&amp; strs) {
        string result = &quot;&quot;;
        for (string str : strs) {
            result += to_string(str.length()) + &quot;#&quot; + str;
        }
        return result;
    }
    vector&lt;string&gt; decode(string s) {
        vector&lt;string&gt; result;
        int i = 0;
        while (i &lt; s.length()) {
            int j = i;
            while (s[j] != '#') {
                j++;
            }
            int length = stoi(s.substr(i, j - i));
            i = j + 1;
            result.push_back(s.substr(i, length));
            i += length;
        }
        return result;
    }
};</code></pre>
</div>
</div>
**Time Complexity:** O(n) where n is total characters  
**Space Complexity:** O(n)
## Comparison Table
| Approach | Time Complexity | Space Complexity | When to Use |
|----------|----------------|------------------|-------------|
| Length Prefix with Delimiter | O(n) | O(n) | Simple and efficient. Handles all edge cases. Easy to implement. Best for most cases |
| Escape Character | O(n) | O(n) | More complex but allows using any delimiter. Useful when you need more control over encoding format |
| Length Prefix (Optimal) | O(n) | O(n) | Most straightforward and efficient. Doesn't require escaping. Handles all characters naturally. Recommended approach |