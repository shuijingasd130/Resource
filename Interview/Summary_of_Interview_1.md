
# 联发科

## 笔试

时间 2018年6月13日10点

- **宏定义比较ab大小，不能用if > <**
    - 答： #define comp(a, b) bool(a/b)  bug b为0
    - 网：：https://www.nowcoder.com/questionTerminal/9240327b0f184184984deb3e387c0e24
        建议使用左移；
        a和b相减，如果结果大于0，最高位为0；否则，为负数最高位为1；
        因为 在计算机中是使用补码存储的；下面的1<<31位，就是用1去与上最高位； */
        	#define cmp(a,b) (((a)-(b))&(1<<31))==1?-1:1;
        //也可以这样写
        	#define cmp(a,b) (((a)-(b))&(0x80000000))==1?-1:1;

        	((a+b)+abs(a-b))/2  https://blog.csdn.net/bingdian37/article/details/1476791 
        	
			#define max(a,b)   (((a)+(b))+(((((a)-(b))>>31)&1)?(~((a)-(b))+1):((a)-(b))))/2
        

``` cpp
    #include <iostream>
    using namespace std;

    int main()
    {
        int a=-20;
        unsigned int b = 10;
        unsigned int c = 20;
        int d = a + b > 0 ? 1 : 0;
        int e = b - c > 0 ? 1 : 0;
        cout << "d=" << d <<"  e=" << e << endl;
        return 0;
    }
```

- 输出  d=1  e=1



- 两个优化题
    - 一个是十六进制转十进制
``` cpp
    #include <iostream>
    using namespace std;

    int main()
    {
        int a;
        while(cin>>hex>>a){
        cout<<a<<endl;
        }
    }
```
- 链接：https://www.nowcoder.com/questionTerminal/8f3df50d2b9043208c5eed283d1d4da6  

    ``` cpp
        #include 〈iostream〉  
        #include 〈list〉  
        #include 〈bitset〉  
        using namespace std; 

    //递归输出二进制函数  
        void BinaryRecursion(int n)  
        {  
         int a;  
         a=n%2;  
         n=n〉〉1;  
         if (n==0)  
          ;  
         else  
          BinaryRecursion(n);  
         cout〈〈a;  
        } 
    ```
    参考：http://blog.csdn.net/xuyongbeijing2008/article/details/7891148


## 程序

- **数独解法 C++实现**

``` cpp
    #include <iostream>

    using namespace std;

    /* 构造完成标志 */
    bool sign = false;

    /* 创建数独矩阵 */
    int num[9][9];

    /* 函数声明 */
    void Input();
    void Output();
    bool Check(int n, int key);
    int DFS(int n);

    /* 主函数 */
    int main()
    {
        //cout << "请输入一个9*9的数独矩阵，空位以0表示:" << endl;
        //Input();

        int array_input[9][9]= { {8,0,0,0,0,0,0,0,0}, \
        {0,0,3,6,0,0,0,0,0}, \
        {0,7,0,0,9,0,2,0,0}, \
        {0,5,0,0,0,7,0,0,0}, \
        {0,0,0,0,4,5,7,0,0}, \
        {0,0,0,1,0,0,0,3,0}, \
        {0,0,1,0,0,0,0,6,8}, \
        {0,0,8,5,0,0,0,1,0}, \
        {0,9,0,0,0,0,4,0,0} };
        for (size_t i = 0; i < 9; i++)
        {
            for (size_t j = 0; j < 9; j++)
            {
                num[i][j] = array_input[i][j];
            }
        }
        DFS(0);
        Output();
        system("pause");
    }

    /* 读入数独矩阵 */
    void Input()
    {
        char temp[9][9];
        for (int i = 0; i < 9; i++)
        {
            for (int j = 0; j < 9; j++)
            {
                cin >> temp[i][j];
                num[i][j] = temp[i][j] - '0';
            }
        }
    }

    /* 输出数独矩阵 */
    void Output()
    {
        cout << endl;
        for (int i = 0; i < 9; i++)
        {
            for (int j = 0; j < 9; j++)
            {
                cout << num[i][j] << " ";
                if (j % 3 == 2)
                {
                    cout << "   ";
                }
            }
            cout << endl;
            if (i % 3 == 2)
            {
                cout << endl;
            }
        }
    }

    /* 判断key填入n时是否满足条件 */
    bool Check(int n, int key)
    {
        /* 判断n所在横列是否合法 */
        for (int i = 0; i < 9; i++)
        {
            /* j为n竖坐标 */
            int j = n / 9;
            if (num[j][i] == key) return false;
        }

        /* 判断n所在竖列是否合法 */
        for (int i = 0; i < 9; i++)
        {
            /* j为n横坐标 */
            int j = n % 9;
            if (num[i][j] == key) return false;
        }

        /* x为n所在的小九宫格左顶点竖坐标 */
        int x = n / 9 / 3 * 3;

        /* y为n所在的小九宫格左顶点横坐标 */
        int y = n % 9 / 3 * 3;

        /* 判断n所在的小九宫格是否合法 */
        for (int i = x; i < x + 3; i++)
        {
            for (int j = y; j < y + 3; j++)
            {
                if (num[i][j] == key) return false;
            }
        }

        /* 全部合法，返回正确 */
        return true;
    }

    /* 深搜构造数独 */
    int DFS(int n)
    {
        /* 所有的都符合，退出递归 */
        if (n > 80)
        {
            sign = true;
            return 0;
        }
        /* 当前位不为空时跳过 */
        if (num[n / 9][n % 9] != 0)
        {
            DFS(n + 1);
        }
        else
        {
            /* 否则对当前位进行枚举测试 */
            for (int i = 1; i <= 9; i++)
            {
                /* 满足条件时填入数字 */
                if (Check(n, i) == true)
                {
                    num[n / 9][n % 9] = i;
                    /* 继续搜索 */
                    DFS(n + 1);
                    /* 返回时如果构造成功，则直接退出 */
                    if (sign == true) return 0;
                    /* 如果构造不成功，还原当前位 */
                    num[n / 9][n % 9] = 0;
                }
            }
        }
    }
```

## 面试

时间：2018年6月14日8:40，下雨，迟到，貌似没啥影响

- 自我介绍：  
    - 简单介绍一下，下回可以重点介绍自己的技能/所学知识  
- 笔试题重新提问：  
    - 改错题的 malloc后需要释放，strlen，还有什么错误忘了  
    - 两个优化题，第一个没给答案，第二个十六进制转十进制，采用移位的方案优化  
			#define cmp(a,b) (((a)-(b))&(1<<31))==1?-1:1;存在无法适应64位计算机问题
- 项目介绍：  
    - 提问了写过最长代码是什么，做过什么大型项目，担任的角色，做了什么  
    - 回答上应该参考答辩，突出项目的难点，细节要稍微详细一下，答得要有些逻辑；大型项目介绍上，面试官可能并不了解你做的什么，在回答时尽量详细些，引起面试官的兴趣，在交互过程中体现你的能力，担任角色可能想问的是你的团队协作意识，怎么交流，工作交叉之类的。  
- 向面试官提问：  
    - 问了下做软件是干什么的，期间问了下在公司做软件前途，这个问题有些糊涂，回答肯定心里有数，问了还显得自己low，下回可以旁敲侧击，了解公司软件各个都是做什么的，哪个人多之类的，来判断做软件怎么样  
    - 其他就是与面试无关的问题，忽略了个加班问题，面试最好可以把握下时间，这次估计是所有面试的时间最短的一个，毫无疑问肯定凉了，可以趁着最后机会补救下，或者问问面试官自己再哪方面应该提升下  


