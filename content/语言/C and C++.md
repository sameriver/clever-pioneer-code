# 标准输入输出

## 格式化输出

### cout

- oct/dec/hex

```cpp
//输出8 10 16进制数字->对后续输出依然有效
//<iomanip>
cout<<oct<<10<<endl;
cout<<dec<<10<<endl;
cout<<hex<<10<<endl;
```

- setw( )

```cpp
//设置输出宽度 仅可在开头使用->**仅对下一次输出有效**
//需要<iomanip>头文件
//长度超过宽度 则直接输出
cout<<setw(3)<<"string"<<endl;
//"string"
```

- setfill( ‘ ‘ )

```cpp
//对**后续输出依然有效**
cout<<setfill('*')<<setw(10)<<"string"<<endl;
//"****string"
```

- left/right 对齐方式

```cpp
//对后续输出依然有效
cout<<left<<setw(5)<<setfill('0')<<7<<endl;
//70000
cout<<right<<setw(5)<<setfill('0')<<7<<endl;
//00007
```

- fixed/setprecision()浮点数格式
    - 含有fixed→保留小数点后n位
    - 不含fixed→保留n位有效数字
    - n由setprecision（n）决定

```cpp
//对后续输出依然有效
cout<<fixed<<setprecision(1)<<1.42<<endl;
```

- scientific科学计数法
- 科学计数法触发条件
    - 6≤指数部分 || -5≥指数部分
    - 显示使用scientific操纵符强制使用科学计数法

```cpp
cout<<scientific<<123.456;
//1.234560e+02
cout<<scientific<<0.00123;
//1.230000e-03
```

- cout类方法的设置

```cpp
//恢复默认浮点格式
cout<<resetiosflags(ios_base::fixed);
cout<<defaultfloat;
cout.unsetf(ios::fixed);
cout.unsetf(ios::scientific);
//返回默认精度**(6)**
int precision = cout.precision();
//设置精度
cout<<precision(7);
//设置填充字符
cout.fill(' ');
//清除fixed/scientific值
cout.unsetf(ios_base::floatfield);
```

# 运算符重载

## 自增自减

# virtual

1. 虚函数

- 基类中使用_**virtual关键字**_声明
- 派生类中使用_**override关键字**_显示标记

基类未添加virtual前，根据指针或引用类型执行函数

基类添加virtual后，则变为根据实际赋值类型执行函数

```sql
class Base {
public:
    virtual void print() { 
        cout << "Base class" << endl; 
    }
};

class Derived : public Base {
public:
    void print() override {  // override 关键字（C++11+）显式声明重写
        cout << "Derived class" << endl;
    }
};

int main() {
    Base* obj = new Derived();
    obj->print();  // 输出 "Derived class"（动态绑定）
    delete obj;
    return 0;
}
```

1. 虚析构函数
    
    基类析构函数添加virtual从而实现态
    
    ```sql
    class Base {
    public:
        virtual ~Base() { 
            cout << "Base destructor" << endl; 
        }
    };
    
    class Derived : public Base {
    public:
        ~Derived() { 
            cout << "Derived destructor" << endl; 
        }
    };
    
    int main() {
        Base* obj = new Derived();
        delete obj;  // 先调用 ~Derived()，再调用 ~Base()
        return 0;
    }
    ```
    
2. 纯虚函数（抽象类）
    
    定义接口规范化→强制派生类实现方法
    
    ```sql
    class Shape {
    public:
        virtual void draw() = 0;  // 纯虚函数
    };
    
    class Circle : public Shape {
    public:
        void draw() override {
            cout << "Drawing a circle" << endl;
        }
    };
    
    int main() {
        // Shape shape;  // 错误：抽象类不能实例化
        Shape* obj = new Circle();
        obj->draw();  // 输出 "Drawing a circle"
        delete obj;
        return 0;
    }
    ```
    
3. 虚继承
    
    用于解决菱形继承
    
    ```cpp
    class 派生类名 : virtual [继承方式] 基类名 {
        // 类定义
    };
    ```
    

# 模板

## 函数模板

- 每一个参数类型在函数参数表中必须至少使用一次
- 范型命名后在作用域内视作关键字
- 范型命名作为形参地位（前向声明与定义可不同）

```cpp
template<class/typename T，class/typename Y> T func(T value,....)｛
    //函数体
｝
```

## 隐式实例化

- 编译器根据传入参数，生成对应的模板函数

## 显式实例化

```cpp
//定义时
template 返回值 函数名<类型>（参数列表）
｛
    //函数体
｝

//调用时
//表示将范型转换为相应的模板类型
func<type>（value，value）
```

## 显式具体化

- 应对特殊情况

```cpp
template<>
返回值 函数名 <特定类型>（参数列表）｛
    //函数体
｝
```

## 函数模板重载

- 函数模板进行重载，处理特殊情况
- 优先调用非模板函数

## 类模板

```cpp
template<class T>
class 类名｛
    public：
    T a；
    T func（T a）；
｝；
```

# 数据类型使用