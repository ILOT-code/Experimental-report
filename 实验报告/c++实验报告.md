

[TOC]

完整的代码在[这里]( https://github.com/ILOT-code/Experimental-report )

## 上机实验一

### 1.1：配置编程环境

按照文档配置好VS2010；

按照网上教程配置好VS code;

进行实验，IDE能够正常运行。

## 上机实验二

### 2.1：质因数分解：

思路：

由数论的知识可知，任何自然数都可以分解为质因数相乘。尝试先设计一个函数，让它判断一个数是否为质数，在主函数中，设计一个while循环对$x$进行递归计算；

核心代码：

```c++
bool ss(int t){ 
    bool flag=true;
    if(t==1) return false;
    for(int i=2;i*i<=t;i+=2)
        if(t%i==0)
        {
            flag=false;
            break;
        }
    return flag;
}
```

```c++
int x,t=2;                           //从最小的质数2开始判断；
scanf("%d",&x);
while(x!=1){                         //进行递归
       if(ss(t)&&x%t==0){
           printf("%d ",t);
           x=x/t;
       }
       else t++;
};
```

实验结果：

该程序通过了平台的测试，在特殊情况$(x=0,x=1)$下也能给出正确结果。

实验分析：

判断函数已进行了优化。只有奇数能进入循环体，并在$i*i>t$后退出；递归的复杂度小于$log_2^n$ .

### 2.2：矩阵旋转：

思路：

设旋转操作之前一元素坐标为$(i,j)$,旋转后为$(i^1,j^1)$,分析可知，这两者存在一个很简单的双射函数关系，

于是可以根据这一关系在常数时间内求得一点的变换后坐标；

核心代码：

```c++
int a[M][N],m,n;
    scanf("%d%d",&m,&n);
    for(int i=1;i<=m;++i)
        for(int j=1;j<=n;++j)
            scanf("%d",&a[i][j]);        //输入原矩阵
    printf("%d %d\n",n,m);
    for(int i=1;i<=n;++i)
    {
        for(int j=1;j<=m;++j)
            printf("%d ",a[1+m-j][i]);   //这就是对应的函数关系
        printf("\n");
    }
```

实验结果：

该程序通过了平台的测试；

实验分析：

设元素个数为$n$，则该算法的复杂度为$O(n)$ 。

### 2.3：字符串基本操作：

思路：

其实，可以先去掉非字母非数字的字符，在互换大小写，然后编写一个比较函数，利用STL中的$sort()$函数对它进行排序，在利用STL中的$unique()$函数或则重新编写一个去重函数便可以完成要求。

核心代码：

```c++
bool cmp(char a,char b)                        //自定义比较
{return a>b;}
i=0;
    while(s[i]!=0){                           //这是去重函数
        int t=i+1;
        while(s[t]!=0){
            if(s[t]==s[i]){
                 int j=t;
                while(s[j]!=0){              //找到重复元素后删除它
                    s[j]=s[j+1];
                    ++j;

                };
            }
            else ++t;
        };
        ++i;
    };
    sort(s,s+i,cmp);                        //排序后输出          
    for(;n>0;--n){
       for(int j=0;j<i;++j)
        printf("%c",s[j]);
   }
```

实验结果：

该程序通过了平台的测试。

实验分析：

删除函数的复杂度为$O(n)$,去重函数的复杂度为$O(n^2)$,排序函数的复杂度为$O(nlog^n)$,复杂度是可以接受的。

### 2.4:   整数加一：

思路：

输入数据远超```int```与```long long int ```,可以用一个数组来模拟四则运算。由于只是加一，实现起来并不复杂。

核心代码：

```c++
    gets(s);
    while(s[len]) ++len;                         //计算长度
    for(int i=0;i<len;++i)                       //模拟加一过程
    {
        if(s[len-1-i]-'0'==9) s[len-1-i]='0';
        else {s[len-1-i]++;break;}
    }
    if(s[0]!='0') puts(s);                      //输出
    else {putchar('1');puts(s);}
```

实验结果：

该程序通过了平台的测试。

实验分析：

利用数组来模拟四则运算可以大大增加计算机的运算能力，使计算机在面对巨大的数字时依旧可以发挥作用。

### 2.5：回文字符串:

思路：

分析可知，回文字符串只有两种类型，一种是$**aa**$ 型的，一种是$**bab**$型的，因此只需找到满足`a[i]==a[i+1] or a[i]==a[i+2]`的位置，在向两头进行寻找即可。可以用优先队列存储所有回文字符串的位置，最后输出即可。

核心代码：

```c++
struct tel                             //该结构体用来存储回文字符串                     
{
    int pos;
    int length;
    bool operator< (tel x) const
    {
        if(length!=x.length) return length<x.length;
        else return pos>x.pos;
    }
};
priority_queue<tel>q;
void find1(int t)                      //第一种情况的find函数
{
    int i=1;
    tel p;
    while(t-i>=0&&t+1+i<len)
    {
        if(s[t-i]==s[t+1+i]) ++i;
        else break;
    };
    p.pos=t-i+1; p.length=2*i;
    q.push(p);
} 
void find2(int t)                    //第二种情况的find函数
{
    int i=1;
    tel p;
    while(t-i>=0&&t+2+i<len)
    {
        if(s[t-i]==s[t+2+i]) ++i;
        else break;
    }
    p.pos=t-i+1;p.length=2*i+1;
    q.push(p);
}
int main()
{
    gets(s);
    while(s[len]!=0) len++;
    for(int i=0;i<len;++i)
    {
        if(s[i]==s[i+1]) find1(i);
        if(s[i]==s[i+2]) find2(i);
    }
    if(q.empty()) printf("0");                //输出
    else
    {
        tel jie=q.top();
        printf("%d\n",jie.length);
        for(int i=0;i<jie.length;++i)
           printf("%c",s[jie.pos+i]);
    }
```

实验结果：

该程序通过了平台的测试。

实验分析：

极端情况是所有字符都相同，在此情况下，复杂度为$O(\frac{1}{2}n^2)$.



## 上机实验三

### 3.1：摄氏华氏温度转换：

思路：

按部就班，实现要求即可。

核心代码：

```c++
class ecg{
	private:
		int c;
		double f;
	public:
		void set_value(int t){
			c=t;f=c*9/5.0+32;
		}
		void put_out(){
			printf("%.1lf",f);
		}
	};
```

实验结果：

该程序通过了平台的测试。

### 3.2：简易计算器

思路：

读取字符，根据该字符选择不同的计算方式。

核心代码：

```c++
class cal{
		int date1,date2;
	public:
		void set_num(int a,int b){
			date1=a;date2=b;
		}
		void run(char s){
			if(s=='+') cout<<date1+date2<<endl;
			if(s=='-') cout<<date1-date2<<endl;
			if(s=='*') cout<<date1*date2<<endl;
			if(s=='/'){
				if(date1%date2) printf("%.2lf",(double)date1/date2);
				else           printf("%d",date1/date2);
			}
		}
	};
```

实验结果：

该程序通过了平台的测试。

实验分析：

在这里用`void set_num(int,int)`来实现数据的输入，在之后可以用构造函数替代。`date1 date2`默认为`private`类型。

### 3.3：时间类Time1的编写：

思路：

构建好`+= -=`运算符后，可以利用它们方便的实现`++ --`运算符。并利用`friend`声明进行`<<`运算符的重载。

核心代码：

```c++
class Time{
private:
	int hour,minute,second;
public:
	Time(int a,int b,int c){
		hour=a;
		minute=b;
		second=c;
	}
	void operator+=(const Time &t1){
		int pos1,pos2;
		pos1=(second+t1.second)/60;
		second=(second+t1.second)%60;
		pos2=(minute+t1.minute+pos1)/60;
		minute=(minute+t1.minute+pos1)%60;
		hour=(hour+t1.hour+pos2)%24;
	}
	void operator-=(const Time &t2){
		int pos1=0,pos2=0;
		if (second>=t2.second     )     second-=t2.second;
		else { pos1=1;                  second=second+60-t2.second;}
		if (minute-pos1>=t2.minute)     minute=minute-pos1-t2.minute;
		else { pos2=1;                  minute=minute-pos1+60-t2.minute;} 
		if (hour-pos2>=t2.hour    )     hour=hour-pos2-t2.hour;
		else {                          hour=hour-pos2+24-t2.hour;}
	}
	Time& operator++(){
		Time t0(0,0,1);
		*this+=t0;
		return *this;
	}
	Time& operator++(int){
		static Time tt(*this),t0(0,0,1);
		*this+=t0;
		return tt;
	}
	Time& operator--(){
		Time t0(0,0,1);
		*this-=t0;
		return *this;
	}
	Time& operator--(int){
		static Time tt(*this),t0(0,0,1);
		*this-=t0;
		return tt;
	}
	friend ostream& operator<<(ostream& output,const Time &p){
		output<<setfill('0')<<setw(2)<<p.hour<<':'<<setfill('0')<<setw(2)<<p.minute<<':'<<setfill('0')<<setw(2)<<p.second<<endl;
		return output;
	}
};
```

实验结果：

该程序通过了平台的测试。

实验分析：

不足：`+= -=`运算符的返回类型设置为了`void`,实际上这并不好，但在该题的背景下并无冲突之处。



## 上机实验四

### 4.1：矩形相交：

思路：

$x$ 与 $ y$ 实际是对称的，只需设计一个函数能求出两条线段的重合长度，在分别把两个长方形的长与宽输入即可。

核心代码：

```c++
class rectangle{
    public:
    int a[2],b[2];
};
int calculate(int x1,int x2,int x3,int x4){
    if(x3<x1)        return calculate(x3,x4,x1,x2); //让x1小于x2;
    if(x3>=x2)       return 0;
    return min(x4,x2)-x3;
}
//输入数据后，直接打印即可
cout<<calculate(a.a[0],a.b[0],b.a[0],b.b[0])*calculate(a.b[1],a.a[1],b.b[1],b.a[1]);
```

实验结果：

该程序通过了平台的测试。

实验分析:

把一个矩形的坐标设置为了`public`,这方便了对其内容的访问，不用设置专门的访问函数。当然也可以把`calculate`设置为`rectangle`的友元函数，`calculate`的形参设置为`rectangle `.

### 4.2：成绩表里找同学：

思路：

`string`类可以根据字典序比较大小，所以把姓名设置为`string`类很方便；并且可以设置一个`com`函数，利用`sort`即可完成排序。

核心代码：

```c++
class stu{
    public:
    string name;
    int a,b,c;
};
bool com( stu a,stu b){
    if(a.a+a.b+a.c!=b.a+b.b+b.c) return a.a+a.b+a.c>b.a+b.b+b.c;
    if(a.a!=b.a)                 return a.a>b.a;
    if(a.b!=b.b)                 return a.b>b.b;
    else                         return a.name<b.name;
}
stu a[1024+5];
//输入数据后，直接排序输出即可；
 sort(a+1,a+1+N,com);
 cout<<a[k].name<<' '<<a[k].a+a[k].b+a[k].c;
```

实验结果：

该程序通过了平台的测试。

实验分析:

可以充分利用`c++`中已有的类以及`STL`来简化代码，并提升程序效率。

### 4.3：动态二维double型数组类Matrix的编写

思路：

在构造函数中利用`new`来实现动态二维数组。利用友元函数实现`<<`与`>>`运算符的重载。并且，在函数中设置一个`flag`,当运算不能进行时，`cout`能直接输出`ERROR`

核心代码：

```c++
class Matrix{
    private:
    int row,col;
    bool flag=1;                              //标记是否合法，不合法输出ERROR
    double** data;
    public:
    Matrix(int a,int b){
        row=a;col=b;
        data=new double*[row];
        for(int i=0;i<row;++i) data[i]=new double[col];
    }
    ~Matrix(){
        for(int i=0;i<row;++i) delete []data[i];
        delete []data;
    }
    double table(int i,int j) { return data[i][j];}
    Matrix& operator+=(const Matrix& p){
        if(row!=p.row||col!=p.col){ flag=0; return *this; }
        for(int i=0;i<row;++i)
        for(int j=0;j<col;++j)
        data[i][j]+=p.data[i][j];
        return *this;
    }
    Matrix& operator*=(const Matrix& p){
        if(col!=p.row) { flag=0; return *this; }
        Matrix pp(row,p.col);
        for(int i=0;i<row;++i)
        for(int j=0;j<p.col;++j)
        for(int k=0;k<p.row;++k)
            pp.data[i][j]+=data[i][k]*p.data[k][j];
        *this=pp;
        return *this;
    }
    Matrix& operator=(const Matrix&p){
        row=p.row;
        col=p.col;
        data=new double*[row];
        for(int i=0;i<row;++i) data[i]=new double[col];
        for(int i=0;i<row;++i)
        for(int j=0;j<col;++j)
        data[i][j]=p.data[i][j];
        return *this;
    }
    friend istream& operator>>(istream& input,Matrix &p){
        for(int i=0;i<p.row;++i)
        for(int j=0;j<p.col;++j)
        input>>p.data[i][j];
        return input;
    }
    friend ostream& operator<<(ostream& output,Matrix &p){
        if(p.flag){
            for(int i=0;i<p.row;++i){
            for(int j=0;j<p.col;++j)
            output<<p.data[i][j]<<' ';
            output<<endl;
        }}
        else {p.flag=1;output<<"ERROR!"<<endl;}
        return output;
    }
};
```

实验结果：

该程序通过了平台的测试。

实验分析:

关于动态二维数组的创建，这里是先创建`row`个一维动态数组，再为每个一位动态数组分配`col`个空间，但是其实`new`可以直接新创建一个二维动态数组，所以这里可以简化一下。

## 第五次上机实验

### 5.1：日期推算：

思路：

日进位到月的过程比较复杂，因此可以专门设置一个函数`fulld`来检测是否需要进位，再依次向上检测是否进位。

核心代码：

```c++
int  id(char s){return s-'0';}                       //把字符变成int型数据
bool big_m(int m){return m==1||m==3||m==5||m==7||m==8||m==10||m==12;}
bool fulld(int run,int m,int d){                    //run表示是否为闰年
    if(big_m(m)&&d==31)             return true;
    if(m==2&&d==28+run)             return true;
    if(!big_m(m)&&m!=2&&d==30)      return true;
    else                            return false;
}
class Data{
    private:
    int y,m,d;
    bool flag=1;                                   / /flag检测越界
    public:
    Data(char *s){
        y=id(s[0])*1000+id(s[1])*100+id(s[2])*10+id(s[3]);
        m=id(s[4])*10+id(s[5]);
        d=id(s[6])*10+id(s[7]);
    }
    void out(){
        if(flag){
             cout<<setfill('0')<<setw(4)<<y<<setw(2)<<m<<setw(2)<<d<<endl;
        }
        else cout<<"out of limitation!";
    }
    void change(int n);
};
void Data::change(int n){
    for(int i=0;i<n;++i){
        int run=!(y%400)||(!(y%4)&&(y%100));     //闰年的判断式
        fulld(run,m,d)?(d=1,m++):(d++);
        if(m==13) {m=1;y++;}
        if(y==10000){flag=0;break;}
    }
}
int main(){
    char s[9];
    int n;
    cin>>s>>n;
    Data a(s);
    a.change(n);
    a.out();
    return 0;
}
```

实验结果：

该程序通过了平台的测试。

实验分析:

`change(int n)` 函数是通过循环加1，每次循环的时间都是常数项，所以复杂度为$O(n)$.

### 5.2：矩形：

思路：

仔细按照题目要求的格式进行输出即可。

核心代码：

```c++
class Rectangle{
    private:
    double m_length=1,m_width=1;
    public:
    double Perimeter(){
        return 2*(m_length+m_width);
    }
    double Area(){
        return m_length*m_width;
    }
    void SetWidth(double w){
        m_width=w;
    }
    double GetWidth(){
        return m_width;
    }
    void SetLength(double l){
        m_length=l;
    }
    double GetLength(){
        return m_length;
    }
    void SetRange(){
        if(m_length<20.0&&m_width<20.0)
            cout<<"length and width are both in 0.0 - 20.0\n";
        else if(m_length>=20.0&&m_width>=20.0)
            cout<<"length and width are not both in 0.0 - 20.0\n";
        else if(m_length>=20.0)
            cout<<"length is not in 0.0 - 20.0\n";
        else 
            cout<<"width is not in 0.0 - 20.0\n";
    }
};
```

实验结果：

该程序通过了平台的测试。

实验分析：

不足：参数的赋值可以用构造函数来给定，能够简化很多。另外，`m_length`与`m_width`都设置为了`private`,需要专门的函数来进行访问。

### 5.3：人员信息：

思路：

按要求定义无参数构造函数、有参数构造函数、拷贝构造函数，再每个构造函数里分别打印对应的信息。

核心代码：

```c++
class Person{
    public:
    char *name;
    int age,female;
    Person(){
        cout<<"Parameterless constructor\n";
    }
    Person(char *s,int age,int female):name(s),age(age),female(female){
        cout<<"Parameter constructor\n";
    }
    Person(const Person &a):name(a.name),age(a.age),female(a.female){
        cout<<"Copy constructor\n";
    }
    void display(){
        cout<<"Information:\n";
        cout<<"Name: "<<name<<"    "<<"Sex: ";
        if(female)  cout<<"female    ";
        else        cout<<"male    ";
        cout<<"Age: "<<age<<endl;
    }
};
int main(){
    char s[10];
    int age,female;
    cin>>s>>female>>age;
    Person P1;
    P1.name=s; P1.age=age; P1.female=female;
    P1.display();
    cin>>s>>female>>age;
    Person P2(s,age,female);
    P2.display();
    Person P3=P2;
    P3.display();
    return 0;
}
```

实验结果：

该程序通过了平台的测试。

实验分析：

由于构造函数在类声明时会自动进行，只要在构造函数中打印对应的信息，就能知道每一个对象是采取何种方式构造的。另外，复制构造函数在 `P1=P2` 与 `P1(P2)` 情况下都会调用，对它采用了以列表形式赋值的办法。

## 第六次上机实验

### 6.1：动态顺序表

思路：

利用`new`函数来实现动态表的创建，利用`sort`来进行排序，要定义复制构造函数，最后用`delete`函数释放内存。

核心代码：

```c++
class List{
    private:
    int *a;
    int length;
    public:
    List(){             
        a=new int[6];
        length=0;
    }
    List(const List &L){
        for(int i=0;i<6;++i)
            a[i]=L.find(i);
        length=L.length;
    }
    void input(){
        for(int i=0;i<5;++i)
            cin>>a[i];
        length=5;
    }
    void insert(int i=8){
        a[length]=i;
        length++;
    }
    void display(){
        sort(a,a+length);
        for(int i=0;i<length;++i)
            cout<<a[i]<<" ";
        cout<<endl;
    }
    int find(int i)const{
        return a[i];
    }
};
int main(){
    List a,b;
    a.input();
    a.display();
    a.insert();
    a.display();
    b=a;
    b.display();
    return 0;
}
```

实验结果：

该程序通过了平台的测试。

实验分析：

由于题目中要求插入值为$8$,因此`insert` 函数形参默认为$8$；利用`find`函数来进行对对象数据的访问。

不足：复制构造函数中并没有利用`new`来为数据分配空间，但实际上在这道题中，两个对象都事先利用无参构造函数来分配了空间，因此并无影响。

### 6.2：由圆派生出圆柱：

思路：

在`circle`中定义半径`r`与计算圆面积的函数`area`,采用`public`继承来派生出`Cylinder`类，于是`Cylinder`可以访问`area`函数来简化`area1`与`volume`函数。

核心代码：

```c++
class circle{
    private:
    double r;
    public:
    circle(double r=0):r(r){}
    double area(){
        return pi*r*r;
    }
    double get_r(){
        return r;
    }
};
class Cylinder:public circle{
    private:
    double h;
    public:
    Cylinder(double r,double h):circle(r),h(h){}     //构造父类
    double area1(){
        return 2*area()+2*pi*get_r()*h;
    }
    double volume(){
        return area()*h;
    }
};
```

实验结果：

该程序通过了平台的测试。

实验分析：

`Cylinder` 类的构造函数中，采用以列表形式赋值，先构造出其父类`circle`,在给`h`赋值。由于是以`public`方式继承，`Cylinder`的对象可以直接访问`circle`中的`public`成员。

### 6.3：虚函数--交通工具：

思路：

由基类`vehicle`派生出`car` `truck` `boat`类，每个类中都定义虚函数`virtual void display()` ,在外部定义一个接口`void fun(vehicle &p)`,来对这些类进行统一的操作。

核心代码：

```c++
class vehicle{
    private:
    int speed,mount,weight;
    public:
    vehicle(int speed=0,int mount=0,int weight=0):speed(speed),mount(mount),weight(weight){}
    virtual void display(){
        cout<<"vehicle messgae\n";
    }
    int get_speed(){
        return speed;
    }
    int get_mount(){
        return mount;
    }
    int get_weight(){
        return weight;
    }
};
class car:public vehicle{
    private:
    int capacity;
    public:
    car(int speed=0,int mount=0,int weight=0,int capacity=0):
                vehicle(speed,mount,weight),capacity(capacity){}
    virtual void display(){
        cout<<"car message\n";
        cout<<get_speed()<<" "<<get_mount()<<" "<<get_weight()<<" "<<capacity<<endl;
    }
};
class truck:public vehicle{
    private:
    int load;
    public:
    truck(int speed=0,int mount=0,int weight=0,int load=0):
                vehicle(speed,mount,weight),load(load){}
    virtual void display(){
        cout<<"truck message\n";
        cout<<get_speed()<<" "<<get_mount()<<" "<<get_weight()<<" "<<load<<endl;
    }
};
class boat:public vehicle{
    private:
    char s;
    public:
    boat(int speed=0,int mount=0,int weight=0,char s='k'):
                vehicle(speed,mount,weight),s(s){}
    virtual void display(){
        cout<<"boat message\n";
        cout<<get_speed()<<" "<<get_mount()<<" "<<get_weight()<<" "<<s<<endl;
    }
};
void fun(vehicle &p){
    p.display();
}
int main(){
    car   c(80,4,1000,4);
    truck t(80,4,2500,3000);
    boat  b(30,0,12000);
    fun(c);
    fun(t);
    fun(b);
    return 0;
}
```

实验结果：

该程序通过了平台的测试。

实验分析：

接口`void fun(vehicle &p)`的形参设置为基类`vehicle`,这样，就能对由`vehicle`派生出的类进行统一的操作。每个派生类的构造函数中，都先构造了基类。

