# [C++ STL] string

## 1. Constructor

| 描述             | 函数原型                                                  |
| ---------------- | --------------------------------------------------------- |
| default(1)       | string();                                                 |
| copy(2)          | string(const string& str);                                |
| substring(3)     | string(const string& str, size_t pos, size_t len = npos); |
| from c-string(4) | string(const char* s);                                    |
| from sequence(5) | string(const char* s, size_t n);                          |
| fill(6)          | string (size_t n, char c);                                |
| range(7)         | string (InputIterator first, InputIterator last);         |



示例代码：

```c++
// string constructor
#include <iostream>
#include <string>

int main ()
{
  std::string s0 ("Initial string");

  // constructors used in the same order as described above:
  std::string s1;
  std::string s2 (s0);
  std::string s3 (s0, 8, 3);
  std::string s4 ("A character sequence");
  std::string s5 ("Another character sequence", 12);
  std::string s6a (10, 'x');
  std::string s6b (10, 42);      // 42 is the ASCII code for '*'
  std::string s7 (s0.begin(), s0.begin()+7);

  std::cout << "s1: " << s1 << "\ns2: " << s2 << "\ns3: " << s3;
  std::cout << "\ns4: " << s4 << "\ns5: " << s5 << "\ns6a: " << s6a;
  std::cout << "\ns6b: " << s6b << "\ns7: " << s7 << '\n';
  return 0;
}
```

输出：

```
s1: 
s2: Initial string
s3: str
s4: A character sequence
s5: Another char
s6a: xxxxxxxxxx
s6b: **********
s7: Initial
```

## 2. Iterators

- [**begin**](http://www.cplusplus.com/reference/string/string/begin/)

  Return iterator to beginning (public member function )

- [**end**](http://www.cplusplus.com/reference/string/string/end/)

  Return iterator to end (public member function )

- [**rbegin**](http://www.cplusplus.com/reference/string/string/rbegin/)

  Return reverse iterator to reverse beginning (public member function )

- [**rend**](http://www.cplusplus.com/reference/string/string/rend/)

  Return reverse iterator to reverse end (public member function )

- [**cbegin** ](http://www.cplusplus.com/reference/string/string/cbegin/)

  Return const_iterator to beginning (public member function )

- [**cend** ](http://www.cplusplus.com/reference/string/string/cend/)

  Return const_iterator to end (public member function )

- [**crbegin** ](http://www.cplusplus.com/reference/string/string/crbegin/)

  Return const_reverse_iterator to reverse beginning (public member function )

- [**crend** ](http://www.cplusplus.com/reference/string/string/crend/)

  Return const_reverse_iterator to reverse end (public member function )

示例代码：

```c++
// string iterators
#include <iostream>
#include <string>

int main ()
{
  std::string str ("Test string");
  for ( std::string::iterator it=str.begin(); it!=str.end(); ++it)
    std::cout << *it;
  std::cout << '\n';

  return 0;
}
```

输出：

```
Test string
```

## 3. Capacity

- [**size**](http://www.cplusplus.com/reference/string/string/size/)

  Return length of string (public member function )

- [**length**](http://www.cplusplus.com/reference/string/string/length/)

  Return length of string (public member function )

- [**max_size**](http://www.cplusplus.com/reference/string/string/max_size/)

  Returns the maximum length the string can reach (public member function )

- [**resize**](http://www.cplusplus.com/reference/string/string/resize/)

  Resize string (public member function )

  void resize(size_t n);

  void resize(size_t char c);

  调整string的长度为n个字符。如果n小于string的长度，保留前n个字符，将后面的字符移除；如果n大于string的长度，用最后一个字符或c(指定的字符)扩展string。 

- [**capacity**](http://www.cplusplus.com/reference/string/string/capacity/)

  Return size of allocated storage (public member function )

- [**reserve**](http://www.cplusplus.com/reference/string/string/reserve/)

  Request a change in capacity (public member function )

  如果n大于当前字符串的容量，则该函数使容器将其容量增大到n个字符（或更大）。**此函数对字符串长度没有影响，且不能更改其内容**。

- [**clear**](http://www.cplusplus.com/reference/string/string/clear/)

  Clear string (public member function )

- [**empty**](http://www.cplusplus.com/reference/string/string/empty/)

  Test if string is empty (public member function )

- [**shrink_to_fit** ](http://www.cplusplus.com/reference/string/string/shrink_to_fit/)

  Shrink to fit (public member function )

## 4. Element access

- [**operator[]**](http://www.cplusplus.com/reference/string/string/operator[]/)

  Get character of string (public member function )

- [**at**](http://www.cplusplus.com/reference/string/string/at/)

  Get character in string (public member function )

- [**back** ](http://www.cplusplus.com/reference/string/string/back/)

  Access last character (public member function )

- [**front** ](http://www.cplusplus.com/reference/string/string/front/)

  Access first character (public member function )

## 5. Modifiers

- [**operator+=**](http://www.cplusplus.com/reference/string/string/operator+=/)

  Append to string (public member function )

  | 描述          | 原型                                       |
  | :------------ | ------------------------------------------ |
  | string (1)    | `string& operator+= (const string& str); ` |
  | c-string (2)  | `string& operator+= (const char* s); `     |
  | character (3) | `string& operator+= (char c);`             |

- [**append**](http://www.cplusplus.com/reference/string/string/append/)

  Append to string (public member function )

  | 描述          | 原型                                                         |
  | :------------ | ------------------------------------------------------------ |
  | string (1)    | `string& append (const string& str); `                       |
  | substring (2) | `string& append (const string& str, size_t subpos, size_t sublen); ` |
  | c-string (3)  | `string& append (const char* s); `                           |
  | buffer (4)    | `string& append (const char* s, size_t n); `                 |
  | fill (5)      | `string& append (size_t n, char c); `                        |
  | range (6)     | `template <class InputIterator>   string& append (InputIterator first, InputIterator last);` |

- [**push_back**](http://www.cplusplus.com/reference/string/string/push_back/)

  Append character to string (public member function )

- [**assign**](http://www.cplusplus.com/reference/string/string/assign/)

  Assign content to string (public member function )

  | 描述          | 原型                                                         |
  | :------------ | ------------------------------------------------------------ |
  | string (1)    | `string& assign (const string& str); `                       |
  | substring (2) | `string& assign (const string& str, size_t subpos, size_t sublen); ` |
  | c-string (3)  | `string& assign (const char* s); `                           |
  | buffer (4)    | `string& assign (const char* s, size_t n); `                 |
  | fill (5)      | `string& assign (size_t n, char c); `                        |
  | range (6)     | `template <class InputIterator>   string& assign (InputIterator first, InputIterator last);` |

- [**insert**](http://www.cplusplus.com/reference/string/string/insert/)

  Insert into string (public member function )

  | 描述                 | 原型                                                         |
  | :------------------- | ------------------------------------------------------------ |
  | string (1)           | ` string& insert (size_t pos, const string& str); `          |
  | substring (2)        | ` string& insert (size_t pos, const string& str, size_t subpos, size_t sublen); ` |
  | c-string (3)         | ` string& insert (size_t pos, const char* s); `              |
  | buffer (4)           | ` string& insert (size_t pos, const char* s, size_t n); `    |
  | fill (5)             | ` string& insert (size_t pos, size_t n, char c);    void insert (iterator p, size_t n, char c); ` |
  | single character (6) | `iterator insert (iterator p, char c); `                     |
  | range (7)            | `template <class InputIterator>   void insert (iterator p, InputIterator first, InputIterator last);` |

- [**erase**](http://www.cplusplus.com/reference/string/string/erase/)

  Erase characters from string (public member function )

  | 描述          | 原型                                                   |
  | :------------ | ------------------------------------------------------ |
  | sequence (1)  | ` string& erase (size_t pos = 0, size_t len = npos); ` |
  | character (2) | `iterator erase (iterator p); `                        |
  | range (3)     | `     iterator erase (iterator first, iterator last);` |

- [**replace**](http://www.cplusplus.com/reference/string/string/replace/)

  Replace portion of string (public member function )

  | 描述          | 原型                                                         |
  | :------------ | ------------------------------------------------------------ |
  | string (1)    | `string& replace (size_t pos,  size_t len,  const string& str);                                                                                                               string& replace (iterator i1, iterator i2, const string& str); ` |
  | substring (2) | `string& replace (size_t pos,  size_t len,  const string& str, size_t subpos, size_t sublen); ` |
  | c-string (3)  | `string& replace (size_t pos,  size_t len,  const char* s);                                                string& replace (iterator i1, iterator i2, const char* s); ` |
  | buffer (4)    | `string& replace (size_t pos,  size_t len,  const char* s, size_t n);                                     string& replace (iterator i1, iterator i2, const char* s, size_t n); ` |
  | fill (5)      | `string& replace (size_t pos,  size_t len,  size_t n, char c);                                          string& replace (iterator i1, iterator i2, size_t n, char c); ` |
  | range (6)     | `template <class InputIterator>  string& replace (iterator i1, iterator i2,                   InputIterator first, InputIterator last);` |

  示例代码：

  ```c++
  // replacing in a string
  #include <iostream>
  #include <string>
  
  int main ()
  {
    std::string base="this is a test string.";
    std::string str2="n example";
    std::string str3="sample phrase";
    std::string str4="useful.";
  
    // replace signatures used in the same order as described above:
  
    // Using positions:                 0123456789*123456789*12345
    std::string str=base;           // "this is a test string."
    str.replace(9,5,str2);          // "this is an example string." (1)
    str.replace(19,6,str3,7,6);     // "this is an example phrase." (2)
    str.replace(8,10,"just a");     // "this is just a phrase."     (3)
    str.replace(8,6,"a shorty",7);  // "this is a short phrase."    (4)
    str.replace(22,1,3,'!');        // "this is a short phrase!!!"  (5)
  
    // Using iterators:                                               0123456789*123456789*
    str.replace(str.begin(),str.end()-3,str3);                    // "sample phrase!!!"      (1)
    str.replace(str.begin(),str.begin()+6,"replace");             // "replace phrase!!!"     (3)
    str.replace(str.begin()+8,str.begin()+14,"is coolness",7);    // "replace is cool!!!"    (4)
    str.replace(str.begin()+12,str.end()-4,4,'o');                // "replace is cooool!!!"  (5)
    str.replace(str.begin()+11,str.end(),str4.begin(),str4.end());// "replace is useful."    (6)
    std::cout << str << '\n';
    return 0;
  }
  ```

- [**swap**](http://www.cplusplus.com/reference/string/string/swap/)

  Swap string values (public member function )

- [**pop_back** ](http://www.cplusplus.com/reference/string/string/pop_back/)

  Delete last character (public member function )

## 6. String operations

- [**c_str**](http://www.cplusplus.com/reference/string/string/c_str/)

  Get C string equivalent (public member function )

- [**data**](http://www.cplusplus.com/reference/string/string/data/)

  Return a pointer to the c-string representation of the string object's value.

- [**get_allocator**](http://www.cplusplus.com/reference/string/string/get_allocator/)

  Get allocator (public member function )

- [**copy**](http://www.cplusplus.com/reference/string/string/copy/)

  Copies a substring of the current value of the string object into the array pointed by *s*.

  size_t copy (char* s, size_t len, size_t pos = 0) const;

- [**find**](http://www.cplusplus.com/reference/string/string/find/)

  Searches the string for the first occurrence of the sequence specified by its arguments. When *pos* is specified, the search only includes characters at or after position *pos*.

  | 描述          | 原型                                                        |
  | :------------ | ----------------------------------------------------------- |
  | string (1)    | `size_t find (const string& str, size_t pos = 0) const; `   |
  | c-string (2)  | `size_t find (const char* s, size_t pos = 0) const; `       |
  | buffer (3)    | `size_t find (const char* s, size_t pos, size_t n) const; ` |
  | character (4) | `size_t find (char c, size_t pos = 0) const;`               |

  示例代码：

  ```c++
  // string::find
  #include <iostream>       // std::cout
  #include <string>         // std::string
  
  int main ()
  {
    std::string str ("There are two needles in this haystack with needles.");
    std::string str2 ("needle");
  
    // different member versions of find in the same order as above:
    std::size_t found = str.find(str2);
    if (found!=std::string::npos)
      std::cout << "first 'needle' found at: " << found << '\n';
  
    found=str.find("needles are small",found+1,6);
    if (found!=std::string::npos)
      std::cout << "second 'needle' found at: " << found << '\n';
  
    found=str.find("haystack");
    if (found!=std::string::npos)
      std::cout << "'haystack' also found at: " << found << '\n';
  
    found=str.find('.');
    if (found!=std::string::npos)
      std::cout << "Period found at: " << found << '\n';
  
    // let's replace the first needle:
    str.replace(str.find(str2),str2.length(),"preposition");
    std::cout << str << '\n';
  
    return 0;
  }
  ```

  输出结果：

  ```
  first 'needle' found at: 14
  second 'needle' found at: 44
  'haystack' also found at: 30
  Period found at: 51
  There are two prepositions in this haystack with needles
  ```

- [**rfind**](http://www.cplusplus.com/reference/string/string/rfind/)

  Find last occurrence of content in string (public member function )

- [**find_first_of**](http://www.cplusplus.com/reference/string/string/find_first_of/)

  Searches the string for the first character that matches **any** of the characters specified in its arguments. When *pos* is specified, the search only includes characters at or after position *pos*.

  | 描述          | 原型                                                         |
  | :------------ | ------------------------------------------------------------ |
  | string (1)    | `size_t find_first_of (const string& str, size_t pos = 0) const; ` |
  | c-string (2)  | `size_t find_first_of (const char* s, size_t pos = 0) const; ` |
  | buffer (3)    | `size_t find_first_of (const char* s, size_t pos, size_t n) const; ` |
  | character (4) | `size_t find_first_of (char c, size_t pos = 0) const;`       |

  示例代码：

  ```c++
  // string::find_first_of
  #include <iostream>       // std::cout
  #include <string>         // std::string
  #include <cstddef>        // std::size_t
  
  int main ()
  {
    std::string str ("Please, replace the vowels in this sentence by asterisks.");
    std::size_t found = str.find_first_of("aeiou");
    while (found!=std::string::npos)
    {
      str[found]='*';
      found=str.find_first_of("aeiou",found+1);
    }
  
    std::cout << str << '\n';
  
    return 0;
  }
  ```

  输出结果：

  ```
  Pl**s*, r*pl*c* th* v*w*ls *n th*s s*nt*nc* by *st*r*sks.
  ```

- [**find_last_of**](http://www.cplusplus.com/reference/string/string/find_last_of/)

  Find character in string from the end (public member function )

- [**find_first_not_of**](http://www.cplusplus.com/reference/string/string/find_first_not_of/)

  Find absence of character in string (public member function )

- [**find_last_not_of**](http://www.cplusplus.com/reference/string/string/find_last_not_of/)

  Find non-matching character in string from the end (public member function )

- [**substr**](http://www.cplusplus.com/reference/string/string/substr/)

  Returns a newly constructed string object with its value initialized to a copy of a substring of this object.

  string substr (size_t pos = 0, size_t len = npos) const;

- [**compare**](http://www.cplusplus.com/reference/string/string/compare/)

  Compare strings (public member function )

  | 描述           | 原型                                                         |
  | :------------- | ------------------------------------------------------------ |
  | string (1)     | `int compare (const string& str) const; `                    |
  | substrings (2) | `int compare (size_t pos, size_t len, const string& str) const;                                                  int compare (size_t pos, size_t len, const string& str,             size_t subpos, size_t sublen) const; ` |
  | c-string (3)   | `int compare (const char* s) const;                                                                            int compare (size_t pos, size_t len, const char* s) const; ` |
  | buffer (4)     | `int compare (size_t pos, size_t len, const char* s, size_t n) const;` |

  返回值：

  | value | relation between *compared string* and *comparing string*    |
  | ----- | :----------------------------------------------------------- |
  | `0`   | They compare equal                                           |
  | `<0`  | Either the value of the first character that does not match is lower in the *compared string*, or all compared characters match but the *compared string* is shorter. |
  | `>0`  | Either the value of the first character that does not match is greater in the *compared string*, or all compared characters match but the *compared string* is longer. |

## 7. Non-member function overloads

- [**operator+**](http://www.cplusplus.com/reference/string/string/operator+/)

  Concatenate strings (function )

  | 描述          | 原型                                                         |
  | :------------ | ------------------------------------------------------------ |
  | string (1)    | `string operator+ (const string& lhs, const string& rhs); `  |
  | c-string (2)  | `string operator+ (const string& lhs, const char*   rhs);                                                 string operator+ (const char*   lhs, const string& rhs); ` |
  | character (3) | `string operator+ (const string& lhs, char          rhs);                                                  string operator+ (char          lhs, const string& rhs);` |

- [**relational operators**](http://www.cplusplus.com/reference/string/string/operators/)

  Relational operators for string (function )

- [**swap**](http://www.cplusplus.com/reference/string/string/swap-free/)

  Exchanges the values of two strings (function )

  void swap (string& x, string& y);

- [**operator>>**](http://www.cplusplus.com/reference/string/string/operator>>/)

  Extract string from stream (function )

- [**operator<<**](http://www.cplusplus.com/reference/string/string/operator<)

  Insert string into stream (function )

- [**getline**](http://www.cplusplus.com/reference/string/string/getline/)

  Get line from stream into string (function )