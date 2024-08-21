---
title: "[C#] const vs readonly 무슨 차이??"
description: "const와 readonly의 차이"
date: 2024-08-08
tags:
  - C#
---

 const와 readonly는 C#에서 변수를 선언할 때 사용되는 키워드로 대표적인 공통점은 **변수의 값을 변경할 수 없게 만든다.**

하지만 자세히 파고들면 두 가지 키워드의 차이가 있다

---

### **각자의 특징**

#### **const**

-   컴파일 타입 상수로, 값은 컴파일 시간에 결정된다
-   정적(static) 환경에서만 사용 가능하다
-   기본 데이터 형식(int, double, bool 등)에만 사용가능
-   예: `const double PI = 3.141592;`

**장점**

-   컴파일 시간에 값이 결정되므로 메모리 사용이 효율적이다
-   값이 고정되어 있어 실수로 변경되는 것을 방지할 수 있다

**단점**

-   컴파일 시간에 값이 고정되므로 런타임에 값을 변경할 수 없다
-   기본 데이터 형식에만 사용할 수 있어 유연성이 부족하다

---

#### **readonly**

-   런타임 상수로, 값은 생성자에서 초기화된다
-   모든 참조 및 값 형식에서 사용할 수 있다
-   정적 또는 인스턴스 수준에서 선언할 수 있다
-   예: `public readonly string Name;`

**장점**

-   런타임에 값을 초기화 할 수 있어 유연성이 높다
-   모든 참조 형식과 값 형식에 사용할 수 있다
-   인스턴스 수준에서 사용할 수 있어 객체 간 상수 값을 다르게 할 수 있다

**단점**

-   메모리 사용이 const보다 비효율적일 수 있다
-   초기화 이후에는 값을 변경할 수 없어 불편할 수 있다

---

#### **공통점**

-   변수의 값을 변경할 수 없도록 한다
-   상수 또는 불변 값을 정의할 때 사용된다

#### **차이점**

**1\. 초기화 시점**

-   const : 컴파일 시간에 초기화되며, 값은 컴파일 시간에 결정됨
-   readonly : 런타임 시간에 초기화되며, 값은 생성자나 정적 생성자에서 설정됨

**2\. 사용 가능한 형식**

-   const : 기본 데이터 형식에서만 사용할 수 있음
-   readonly : 모든 참조 형식과 값 형식에 사용할 수 있음

**3\. 선언 가능한 위치**

-   const : 클래스, 구조체, 인터페이스 내부에서만 정적 필드로 선언할 수 있음
-   readonly : 클래스, 구조체, 인터페이스 내부에서 정적 필드 또는 인스턴스 필드로 선언할 수 있음

**4\. 사용 가능한 위치**

-   const : 클래스, 구조체, 인터페이스 내부에서만 사용할 수 있음
-   readonly : 클래스, 구조체, 인터페이스 내부뿐만 아니라 지역 변수로도 사용할 수 있음

**5\. 메모리 효율성**

-   const : 컴파일 시간에 값이 결정되므로 메모리 사용이 더 효율적임
-   readonly : 런타임에 값이 초기화되므로 메모리 사용이 const보다 비효율적임

**6\. 값 변경 가능성**

-   const : 값을 변경할 수 없음
-   readonly : 값을 변경할 수 없지만, 참조 형식의 경우 객체 내부의 필드는 변경할 수 있음

---

#### **예제 코드**

```
namespace ConsoleApp1
{

    public class MyClass
    {
        public const int constValue = 10;
        public readonly int readonlyValue;

        public MyClass(int readonly_Value)
        {
            readonlyValue = readonly_Value;
        }

        public void PrintValue()
        {
            Console.WriteLine($"ConstValue: {constValue}");
            Console.WriteLine($"ReadonlyValue: {readonlyValue}");

        }
    }
    internal class Program
    {
        static void Main(string[] args)
        {
            MyClass obj = new MyClass(20);
            obj.PrintValue();

            MyClass obj2 = new MyClass(30);
            obj2.PrintValue();
        }
    }
}
```

**결과**

```
ConstValue: 10
ReadonlyValue: 20
ConstValue: 10
ReadonlyValue: 30
```

위 예제를 통해 const와 readonly의 차이점을 알 수 있다.

1\. `const` 필드 `constValue`는 컴파일 시간에 값이 결정되므로, 모든 인스턴스에 대해 동일한 값(10)을 갖는다.

2\. `readonly` 필드 `readonlyValue`는 생성자에서 초기화되므로, 인스턴스마다 다른 값(20, 30)을 가질  수 있다.

3\. `const`와 `readonly`필드 모두 값을 변경할 수 없다.

#### **그래서 뭘 써야하는가?**

**1\. 유연성**

-   readonly는 값 형식뿐만 아니라 참조 형식에도 사용할 수 있어서 더 유연하다
-   const는 기본 데이터 형식에만 사용할 수 있다

**2\. 초기화 시점**

-   readonly는 생성자나 정적 생성자에서 초기화되므로 런타임에 다양한 방식으로 값을 설정할 수 있다
-   const는 컴파일 시간에 값이 결정되므로 유연성이 떨어진다

**3\. 인스턴스 수준 상수**

-   readonly는 인스턴스 수준에서 상수를 정의할 수 있다
-   const는 정적 환경에서만 사용할 수 있다

**4\. 지역 변수 불변성**

-   readonly는 지역 변수로 사용할 수 있어서 코드 블럭 내에서 불변 변수를 만들 수 있다
-   const는 지역 변수로 사용할 수 없다

**5\. 참조 형식 불변성**

-   readonly 참조 형식은 객체 자체는 불변이지만, 내부 필드는 변경할 수 있다
-   const는 값 형식에만 사용할 수 있기 때문에 참조 형식의 불변성을 보장하지 못한다

**6\. 메모리 효율성**

-   const가 readonly보다 조금 더 메모리 효율적이지만, 실제로는 큰 차이가 없는 경우가 많다

따라서 readonly가 const보다 조금 더 유연하고, 다양한 상황에서 사용할 수 있기 때문에 대부분의 경우 readonly를 사용하길 권장한다.

단 간단한 값 형식의 상수를 정의할 때는 const를 사용해도 좋다.