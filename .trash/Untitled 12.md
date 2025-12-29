

```
/var/folders/xw/6417bdnn7rl_smjvfm8mkdz00000gn/T/tmpfupe15c9/main.cpp:27:10: error: invalid operands to binary expression ('std::ofstream' (aka 'basic_ofstream<char>') and 'std::vector<int>')  
   27 |     fout << arr;  
      |     ~~~~ ^  ~~~  
/Library/Developer/CommandLineTools/SDKs/MacOSX.sdk/usr/include/c++/v1/__ostream/basic_ostream.h:346:55: note: candidate function template not viable: no known conversion from 'std::vector<int>' to 'char' for 2nd argument  
  346 | _LIBCPP_HIDE_FROM_ABI basic_ostream<_CharT, _Traits>& operator<<(basic_ostream<_CharT, _Traits>& __os, char __cn) {  
      |                                                       ^                                                ~~~~~~~~~  
/Library/Developer/CommandLineTools/SDKs/MacOSX.sdk/usr/include/c++/v1/__ostream/basic_ostream.h:373:53: note: candidate function template not viable: no known conversion from 'std::vector<int>' to 'char' for 2nd argument  
  373 | _LIBCPP_HIDE_FROM_ABI basic_ostream<char, _Traits>& operator<<(basic_ostream<char, _Traits>& __os, char __c) {  
      |
```