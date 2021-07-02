# 1、[ A+B(1)](https://ac.nowcoder.com/acm/contest/5652/A)

- 题目描述

  - 计算a+b

- 输入描述:

  - 输入包括两个正整数a,b(1 <= a, b <= 10^9)，**输入数据包括多组**。

- 输出描述:

  - 输出a+b的结果

- 示例1

  - 输入

    - ```c++
      1 5  //第一行上来就是数据
      10 20
      ```

  - 输出

    - ```
      6
      30
      ```

- 代码

  - ```c++
    #include<iostream>
    using namespace std;
    
    int main(){
        int a,b;
        while(cin>>a>>b)//输入数据包括多组
            cout<<a+b<<endl;
        return 0;
    }
    ```

# 2、[A+B(2)](https://ac.nowcoder.com/acm/contest/5657/B)

- 示例1

  - 输入

    - ```c++
      2  //第一行的数字代表数据个数
      1 5
      10 20
      ```

  - 输出

    - ```
      6
      30
      ```

- 代码

  - ```c++
    #include<iostream>
    using namespace std;
    
    int main(){
        int t,a,b;
        cin>>t;//第一行的数字代表数据个数
        while(t--){
            cin>>a>>b;  
            cout<<a+b<<endl;
        }
        return 0;
    }
    ```

# 3、[ A+B(3)](https://ac.nowcoder.com/acm/contest/5657/C)

- 示例1

  - 输入

    - ```c++
      1 5
      10 20
      0 0 //输入数据有多组, 如果输入为0 0则结束输入
      ```

  - 输出

    - ```c++
      6
      30
      ```

- 代码

  - ```c++
    #include<iostream>
    using namespace std;
    
    int main(){
        int a,b;
        while(cin>>a>>b){//输入数据包括多组
            if(a==0 && b==0)//输入为0 0则结束输入
                break;
            cout<<a+b<<endl;
        }
        return 0;
    }
    ```

# 4、[A+B(4)](https://ac.nowcoder.com/acm/contest/5657/D)

- 示例1

  - 输入

    - ```c++
      4 1 2 3 4      //每组数据一行,每行的第一个整数为整数的个数n
      5 1 2 3 4 5
      0              //n为0的时候结束输入
      ```

  - 输出

    - ```c++
      10
      15
      ```

- 代码

  - ```c++
    #include<iostream>
    using namespace std;
    int main(){
        int n;
        int cur;
        while(cin>>n){//每组数据一行,每行的第一个整数为整数的个数n
            if(n==0)//n为0的时候结束输入
                break;
            int sum=0;
            while(n--){//本行输入n次
                cin>>cur;
                sum+=cur;
            }
            cout<<sum<<endl;
        }
            
        return 0;
    }
    ```

# 5、[A+B(5)](https://ac.nowcoder.com/acm/contest/5657/E)

- 示例1

  - 输入

    - ```c++
      2              //输入的第一行， 表示数据组数
      4 1 2 3 4	   //接下来t行, 每行一组数据。每行的第一个整数为整数的个数n
      5 1 2 3 4 5
      ```

  - 输出

    - ```c++
      10
      15
      ```

- 代码

  - ```c++
    #include<iostream>
    using namespace std;
    int main(){
        int t; //t行。输入的第一行， 表示数据组数
        int n; //本行有几个数输入
        int cur; //当前输入值
        cin>>t;
        while(t--){//每组数据一行,每行的第一个整数为整数的个数n
            cin>>n;
            int sum=0;
            while(n--){//本行输入n次
                cin>>cur;
                sum+=cur;
            }
            cout<<sum<<endl;
        }
            
        return 0;
    }
    ```

# 6、[A+B(6)](https://ac.nowcoder.com/acm/contest/5657/F)

- 示例1

  - 输入

    - ```c++
      4 1 2 3 4	  //每组数据一行,每行的第一个整数为整数的个数n
      5 1 2 3 4 5  //相比第4题只是少了最后一行输入为0时停止
      ```

  - 输出

    - ```c++
      10
      15
      ```

- 代码

  - ```c++
    #include<iostream>
    using namespace std;
    int main(){
        int n;
        int cur;
        while(cin>>n){//每组数据一行,每行的第一个整数为整数的个数n
            int sum=0;
            while(n--){//本行输入n次
                cin>>cur;
                sum+=cur;
            }
            cout<<sum<<endl;
        }
            
        return 0;
    }
    ```

# 7、[A+B(7)](https://ac.nowcoder.com/acm/contest/5657/G)

- 示例1

  - 输入

    - ```c++
      1 2 3       //不定长输入
      4 5
      0 0 0 0 0
      ```

  - 输出

    - ```c++
      6
      9
      0
      ```

- 代码

  - ```c++
    #include<iostream>
    using namespace std;
    int main(){
        int cur;
        int sum=0;
        while(cin>>cur){//cin读不到空格和回车
            sum+=cur;
            if (cin.get() == '\n') {//但是cin.get()可以接收到包括空格和回车在内的所有字符。
                                    //在这个代码里，每次运行到这儿时，cin.get()到的要么是空格要么是回车。
                 cout<<sum<<endl;
                 sum=0;
            }
        }
            
        return 0;
    }
    ```

# 8、[字符串排序(1)](https://ac.nowcoder.com/acm/contest/5652/H)

- 实例1

  - 输入

    - ```c++
      5         //第一行n
      c d a bb e//第二行是n个空格隔开的字符串
      ```

  - 输出

    - ```c++
      a bb c d e//输出一行排序后的字符串，空格隔开，无结尾空格
      ```

- 代码

  - ```c++
    #include<iostream>
    #include<vector>
    #include<algorithm>
    using namespace std;
    int main(){
        int n;
        string str;
        vector<string> data;
        cin>>n;
        while(n--){
            cin>>str;
            data.push_back(str);
        }
        sort(data.begin(),data.end());
        for (int i = 0; i < data.size() - 1; i++) {
            cout << data[i] << ' ';
        }
        cout << data[data.size() - 1];
        return 0;
    }
    ```

# 9、[字符串排序(2)](https://ac.nowcoder.com/acm/contest/5652/I)

- 实例1

  - 输入

    - ```c++
      a c bb//多个测试用例，每个测试用例一行。
      f dddd
      nowcoder
      ```

  - 输出

    - ```c++
      a bb c
      dddd f
      nowcoder
      ```

- 代码

  - ```c++
    //不推荐！
    #include<iostream>
    #include<vector>
    #include<algorithm>
    using namespace std;
    int main(){
        string str;
        vector<string> data;
        while(cin>>str){//cin读不到空格和回车
            data.push_back(str);
            if(cin.get() == '\n'){//cin.get()可以读到空格和回车
                sort(data.begin(),data.end());         
                for (int i = 0; i < data.size() - 1; i++)
                        cout << data[i] << ' ';
                cout << data[data.size() - 1]<<endl;
                data.clear();
            }
        }
        return 0;
    }
    ```

  - ```c++
    //推荐！
    #include<iostream>
    #include<vector>
    #include<algorithm>
    #include<sstream>
    using namespace std;
    int main(){
        string line;
        vector<string> data;
        while(getline(cin, line)){//以回车为界，读出一整行先
            string buf;
            istringstream iss(line);
            while(getline(iss,buf,' '))//以空格切割字符串
    	        data.push_back(buf);
            sort(data.begin(),data.end());
            for (int i = 0; i < data.size() - 1; i++)
                cout << data[i] << ' ';
            cout << data[data.size() - 1]<<endl;
            data.clear();
        }
        return 0;
    }
    ```

    

# 10、[字符串排序(3)](https://ac.nowcoder.com/acm/contest/5652/J)

- 实例1

  - 输入

    - ```c++
      a,c,bb
      f,dddd
      nowcoder
      ```

  - 输出

    - ```c++
      a,bb,c
      dddd,f
      nowcoder
      ```

- 代码

  - ```c++
    #include<iostream>
    #include<vector>
    #include<algorithm>
    #include<sstream>
    using namespace std;
    int main(){
        string line;
        vector<string> data;
        while(getline(cin, line)){//以回车为界，读出一整行先
            string buf;
            istringstream iss(line);
            while(getline(iss,buf,','))//以','切割字符串
    	        data.push_back(buf);
            sort(data.begin(),data.end());
            for (int i = 0; i < data.size() - 1; i++)
                cout << data[i] << ',';
            cout << data[data.size() - 1]<<endl;
            data.clear();
        }
        return 0;
    }
    ```

    

# 注意

- cin和getline混用时，使用cin.ignore()来消除回车

  - ```c++
    int C;
    cin>>C;
    cin.ignore();//消除回车
    ```

- cin 不会主动删除输入流内的换行符，这样换行（即回车）符就被 getline 读取到，getline 遇到换行符返回，因此程序不会等待用户输入。

  - 解决的办法是手动清除换行符，在cin>>后加上 **cin.ignore();**，这样即可得到正确结果。

# 总结

- cin
  - 自动跳过空格和回车读入数据
- cin.get()
  - 读取一个char，可以是空格 回车 等等
- getline(cin, line)
  - 读取一整行到string "line"中

# 建议

- **建议不要混用cin和getline**

  - ```c++
    #include<iostream>
    #include<vector>
    #include<sstream>
    using namespace std;
    int main(){    
    	int C;
        cin>>C;
        
        string w;
        cin>>w;
        vector<int> W;
        string buf;
        istringstream iss(w);
        while(getline(iss,buf,','))
            W.push_back(stoi(buf));
    
        string v;
        cin>>v;
        vector<int> V;
        iss=istringstream(v);
        while(getline(iss,buf,','))
            V.push_back(stoi(buf));
        
    }
    ```

  - 一行直接cin到一个string里（不要使用getline到一个string里）

- 如果在cin之后还有getline，切记cin之后加一句cin.ignore();消除回车

  - ```c++
    int main(){
        int C;
        cin>>C;
        cin.ignore();//cin之后还有getline，切记cin之后加一句cin.ignore();消除回车
        
        vector<int> W;
        string line;
        getline(cin,line);
        string buf;
        istringstream iss(line);
        while(getline(iss,buf,','))
            W.push_back(stoi(buf));
        
        vector<int> V;
        getline(cin,line);
        iss= istringstream(line);
        while(getline(iss,buf,','))
            V.push_back(stoi(buf));
    }
    ```


# 最终版

```c++
//split函数
vector<int> split(string str, char pattern) {
	vector<int> result;
	string buf;
	istringstream iss(str);
	while (getline(iss, buf, pattern))
		result.push_back(stoi(buf));
	return result;
}


int main() {
	int bagweight = 0;
	string input1;
	string input2;
	cin >> bagweight;
	cin >> input1;
	cin >> input2;
	vector<int> weight = split(input1, ',');
	vector<int> value = split(input2, ',');
}
//4
//2,1,3
//4,2,3
```

