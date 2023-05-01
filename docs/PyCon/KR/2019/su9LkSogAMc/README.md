---
title: [2019 PyCon KR] 파이썬의 변수 - 조인석
subtitle: [2019년 PyCon KR] 파이썬의 변수 (강한 타입, Immutable vs Mutable, Class vs Instance) - 조인석
description: 2019년 PyCon 한국에서 진행한 파이썬의 변수에 대한 조인석님의 발표 요약 정리
comments: true
---

# [2019 PyCon KR] 파이썬의 변수 - 조인석

## 동적 바인딩과 강한 타입 언어

### 동적 바인딩(Dynamic Binding)

동적 바인딩이란 런타임에 바인딩하는 것을 의미한다. 다시 말해 컴파일 시점이 아닌 프로그램을 실행하는 런타임 시점에 값이 확정되는 것을 의미한다. 덕 타이핑(Duck Typing)은 이러한 동적 바인딩의 종류 중 하나로 그 이름에서 알 수 있듯 오리처럼 걷고, 오리처럼 소리내면 그것을 오리라고 인지하는 걸 의미한다. 예를 들어 아래와 같은 코드에서 변수 `what`은 정수, 문자열, 리스트가 된다.

```Python
what = 10   # 정수형
what = "10" # 문자열
what = [10] # 리스트
```

결국 변수 `what`의 자료형은 컴파일 시점이 아닌 런타임 시점에 적용되며, 결국 덕 타이핑 또한 동적 바인딩의 한 종류라 할 수 있다. 여담이지만 파이썬 또한 내부적으로 컴파일 단계가 수행되며, `.pyc` 확장자로 끝나는 파일을 활용해 매번 소스 코드를 빌드하는 것이 아닌 변경된 부분만 빌드해서 프로그램을 더 효율적으로 실행한다.

### 강한 타입 언어

자바스크립트의 경우 아래 소스 코드와 같이 자료형 간에 암시적인 변화를 허용하기 때문에 정수형 값인 `10`과 문자열 값인 `"10"`은 동일한 값으로 취급된다. 만약 자료형까지 고려하여 비교를 더 제한적으로 하고 싶은 경우 `==` 비교 연산자가 아닌 `===` 비교 연산자를 사용해야 한다. 그리고 이러한 언어, 다시 말해 자료형 간에 암시적 변환을 쉽게 허용하는 언어를 약한 타입 언어라 한다.

```JavaScript
console.log(10 == "10") // > true
console.log(10 === "10") // > false
```

반대로 파이썬의 경우 아래와 같이 자료형 간에 암시적 변환을 허용하지 않기 때문에 자바스크립트처럼 두 개의 비교 연산자가 존재하지도 않을 뿐더러 정수형 `10`과 문자열 `"10"`의 값을 명확하게 다른 것으로 취급한다. 그리고 이러한 언어, 다시 말해 자료형 간에 암시적 변화를 허용하지 않는 언어를 강한 타입 언어라 한다.

```Python
print(10 == "10") # > False
```

결론적으로 파이썬의 경우 강한 타입 언어이면서 동시에 동적 바인딩을 통해 덕 타이핑을 구현한 프로그래밍 언어이기 때문에 개발자들에게 강한 제약을 부여하지 않고, 오히려 명명 규칙(Naming Convention)을 활용해 이를 보완한다.


## Immutable vs Mutable

### Immutable

파이썬에 존재하는 자료형은 크게 Immutable한 자료형과 Mutable한 자료형으로 나뉜다. 이때 Immutable한 자료형에는 크게 정수형, 문자열, 튜플이 존재하며 Mutable한 자료형에는 크게 리스트, 딕셔너리 등이 존재한다. 예를 들어, 아래와 같은 소스 코드에서 변수 `abcde`는 런타임 시점에 문자열 자료형이며, 이는 Immutable한 자료형이기 때문에 객체에 값을 추가하더라도 추가되지 않고, 이전과 동일한 객체로 존재한다

```Python
abcde = "abcd"
print(id(abcde)) # > 4522087288

abcde + "e"
print(abcde) # > "abcd"
print(id(abcde)) # > 4522087288
```

이때 변수 `abcde`에 값을 추가하고 싶을 경우 Immutable한 자료형이기 때문에 새로운 객체에 이를 대입해줘야 하며, 이를 위해서 변수 `abcde`에 새로운 값을 추가한 결괏값을 할당해주었다. 따라서 이는 완전히 새로운 객체가 되어 이전 변수 `abcde`와는 다른 메모리 주소값을 가리키게 된다.

```Python
abcde = "abcd"
print(id(abcde)) # > 4522087288

abcde = abcde + "e"
print(abcde) # > "abcde"
print(id(abcde)) # > 4574089144
```

### Mutable

반대로 아래 리스트 변수 `abcde`의 경우 Mutable한 자료형이기 때문에 `append` 메서드를 활용해 새로운 요소(Element)를 추가하면 기존 객체에 새로운 값이 추가되고, 메모리 주소값 또한 유지된다.

```Python
abcde = ["a", "b", "c", "d"]
print(id(abcde)) # 4574096456

abcde.append("e")
print(abcde) # > ["a", "b", "c", "d", "e"]
print(id(abcde)) # > # 4574096456
```

이러한 Immutable한 자료형과 Mutable한 자료형의 차이점을 메모리 관점에서 생각한다면, Immutable한 자료형의 경우 빈번하게 값을 갱신할 경우 매번 새로운 객체를 생성하고 이전 객체는 가비지 컬렉터에 의해 해제되어야 하기 때문에 Mutable한 자료형을 사용하는 경우보다 과도한 메모리를 사용하게 될 것이다. 반대로 한 번 정의된 이후 별도의 갱신이 필요하지 않은 경우 Immutable한 자료형으로 객체를 정의하는 것이 메모리에 있어 훨씬 효율적일 것이다.

## 클래스 변수 vs 인스턴스 변수

### Mutable한 클래스 변수와 인스턴스 변수

클래스 변수의 경우, 해당 클래스를 사용하는 모든 객체에서 공유할 수 있는 변수를 의미한다. 아래 소스 코드 예시에서는 리스트 변수 `languages`가 클래스 변수에 해당한다.

```Python
class Programmer:
    languages = []

    def __init__(self, name):
        self.name = name
    
    def add_language(self, language):
        self.languages.append(language)
```

클래스 변수를 사용할 때의 문제점은 해당 클래스를 사용하는 모든 객체가 해당 변수를 공유한다는 점이다. 따라서 아래와 같이 `chris` 인스턴스에서 `add_language` 메서드를 사용해 클래스 변수에 값을 갱신할 경우 `ujin` 인스턴스에서도 똑같이 갱신된 값을 가진 클래스 변수를 공유하게 된다. 이는 해당 클래스 변수 `languages`가 Mutable한 객체은 리스트라 발생한 문제점이기도 하다. 결국 새로운 값을 추가하여 객체를 갱신할 경우 Mutable한 객체인 리스트의 경우 기존 메모리 주소를 유지한 상태로 값을 변경 할 수 있기 때문이다.

```Python
chris = Programmer("Chris")
chris.add_language("Python")
chris.add_language("Java")
print(chris.languages) # > ["Python", "Java"]

ujin = Programmer("Ujin")
print(ujin.languages) # > ["Python", "Java"]
```

따라서 클래스끼리 공유하고 싶지 않은, 리스트와 같은 Mutable한 자료형을 클래스 내부에서 사용하고 싶을 때는 아래 소스 코드와 같이 인스턴스 변수로 정의하는 게 오류를 방지할 수 있다.

```Python
class Programmer:
    def __init__(self, name):
        self.name = name
        self.languages = []
    
    def add_language(self, language):
        self.languages.append(language)
```

결론적으로 클래스에 의해 생성된 모든 객체가 같은 값을 조회할 때는 클래스 변수를 사용하고, 인스턴스화 될 때마다 새로운 값이 할당되며 서로 다른 객체 간에는 값을 공유할 수 없게 할 때는 인스턴스 변수를 사용해야 한다. 객체 단위로 값이 따로 관리되어야 하는 지, 혹은 클래스 단위로 값이 함께 관리되어야 하는 지에 따라 변수 사용 방법을 결정해야 한다.

### Immutable한 클래스 변수와 인스턴스 변수

이제 클래스끼리 값을 공유해야 하는, 다시 말해 클래스 변수로 문자열 변수 `country`를 선언하고 그 값으로 `"South Korea"`를 할당한 상황을 가정해보자.

```Python
class Programmer:
    country = "South Korea"

    def __init__(self, name):
        self.name = name
        self.languages = []
    
    def add_language(self, language):
        self.languages.append(language)

    def introduce(self):
        print(f"{self.name} is in {self.country}, {id(self.country)}")
```

아래 소스 코드와 같이 `chris` 인스턴스의 클래스 변수의 값을 변경하게 되면, 이번에는 `ujin` 인스턴스의 클래스 변수 값은 그대로인데, `chris` 인스턴스의 클래스 변수 값만 변경된 것을 볼 수 있다. 앞서 살펴봤던 Mutable한 자료형이었던 리스트 변수 `languages` 사례와는 다르게, 변수 `country`의 경우 Immutable한 자료형이기 때문에 클래스 변수에 접근해서 그 값을 변경하는 순간 새로운 객체로 변경되어 `chris` 인스턴스와 `ujin` 인스턴스가 가리키는 객체의 메모리 주소값이 달라졌기 때문이다. 이는 `id` 메서드를 통해 두 인스턴스의 클래스 변수 메모리 주소값을 출력해보면 알 수 있다.

```Python
chris = Programmer("Chris")
chris.country = "Japan"
chris.introduce() # > Chris is in Japan, 4356463536

ujin = Programmer("Ujin")
ujin.introduce() # > Ujin is in South Korea, 4356264624
```

이를 예방하기 위해서는 아래 소스 코드와 같이 명명 규칙을 활용해 언더스코어(`_`) 및 대문자로 변수명을 지정해서 갱신되면 안 되는 클래스 변수임을 나타내고 실제 메서드 등에서 해당 클래스 변수를 사용할 때도 `self` 키워드를 활용해서 인스턴스로 접근하는 것이 아닌, 클래스 자체로 접근해서 사용하면 된다.

```Python
class Programmer:
    _COUNTRY = "South Korea"

    def __init__(self, name):
        self.name = name
        self.languages = []
    
    def add_language(self, language):
        self.languages.append(language)

    def introduce(self):
        print(f"{Programmer._COUNTRY}, {self._COUNTRY}")
        print(f"{self.name} is in {Programmer._COUNTRY}, {id(Programmer._COUNTRY)}")
```

그러면 아래 소스 코드와 같이 `chris` 인스턴스를 통해서 클래스 변수명에 접근하더라도 `introduce` 메서드에서 가리키는 클래스 변수 `_COUNTRY`는 결국 클래스를 통해 가리키기 때문에 여전히 동일한 객체를 가리키게 되어 변경되지 않는다. 물론 인스턴스를 통해 접근하여 변경한 클래스 변수 `_COUNTRY`는 변경이 되었기 때문에 `chirs._COUNTRY`와 같이 인스턴스로 접근해 그 값과 메모리 주소값을 출력해보면 변경된 값과 메모리 주소값을 보여준다.

```Python
chris = Programmer("Chris")
chris._COUNTRY = "Japan"
chris.introduce() # > Chris is in South Korea, 4557542064
print(chris._COUNTRY, id(chris._COUNTRY)) # > Japan 4362050544

ujin = Programmer("Ujin")
ujin.introduce() # > Ujin is in South Korea, 4557542064
```

물론 인스턴스가 아닌 클래스를 통해 접근해서 클래스 변수의 값을 변경하면 아래 소스 코드와 같이 완전히 바뀐 값을 모든 인스턴스가 가리키게 되기 때문에 유의해야 한다.

```Python
chris = Programmer("Chris")
Programmer._COUNTRY = "Japan"
chris.introduce() # > Chris is in Japan, 4410858480
print(chris._COUNTRY, id(chris._COUNTRY)) # > Japan 4410858480

ujin = Programmer("Ujin")
ujin.introduce() # > Ujin is in Japan, 4410858480
```

이러한 사실로부터 클래스 변수와 인스턴스 변수 사용 방법을 다시 한 번 정리해보면, 클래스 변수는 객체 이름이 아닌 클래스 이름으로 접근하는 것이 안전하며 되도록 변경되지 않는 값을 정의해주는 것이 좋다는 것을 알 수 있다.

## 변수의 범위

변수의 범위는 크게 로컬(Local), 논로컬(Nonlocal), 글로벌(Global)이 존재한다. 논로컬의 경우 `nonlocal` 키워드로 접근해서 조회 및 갱신 가능하며, 글로벌의 경우 `global` 키워드로 접근해서 조회 및 갱신 가능하다. 아래와 같은 소스 코드를 실행하여 각각의 범위에 대해 출력해보면, `global` 키워드를 활용해 문자열 변수 `spam`을 출력하는 순간, 글로벌 범위의 문자열 변수 `spam`이 존재하지 않기 때문에 앞서 `nonlocal` 키워드로 변경하게 된 값을 출력하게 된다. 그러나 `scope_test` 함수를 수행한 뒤에 변수 `spam`을 출력하게 되면, 이때의 변수는 글로벌 범위의 변수 `spam`이기 때문에 `do_global` 함수에 의해 그 값이 `"global spam"`으로 할당되어 `"global_spam"`을 출력하게 된다.

```Python
def scope_test():
    def do_local():
        spam = "local spam"
    
    def do_nonlocal():
        nonlocal spam
        spam = "nonlocal spam"
    
    def do_global():
        global spam
        spam = "global spam"
    

    spam = "test spam"
    do_local()
    print(f"After Local Assignment: {spam}") # > After Local Assignment: test spam
    do_nonlocal()
    print(f"After Nonlocal Assignment: {spam}") # > After Nonlocal Assignment: nonlocal spam
    do_global()
    print(f"After Global Assignment: {spam}") # > After Global Assignment: nonlocal spam

scope_test()
print(f"In Global Scope: {spam}") # > In Global Scope: global spam
```

따라서 값이 유효 범위에 따라 예상하지 못한 상태로 변화될 수 있기 때문에 가급적 `nonlocal` 및 `global` 키워드를 활용해 변수에 접근해서 갱신하는 것은 삼가야 한다.

## 코드 스타일 가이드

이러한 파이썬의 유연성 때문에 프로그램은 예상하지 못한 오류를 발생할 가능성이 높다. 따라서 코드 스타일 가이드를 통해 사전에 방지하는 것이 좋은데 앞서 이야기한 것처럼 보통 언더스코어(`_`) 또는 던더라 불리는 더블 언더스코어(`__`)을 사용해 변수명을 지어서 개발자가 범할 실수를 예방한다. 예를 들어, 외부에서 접근이 불가능해야 할 클래스 변수명의 경우 더블 언더스코어을 활용해 `__COUNTRY`와 같이 그 변수명을 짓게 되면, 파이썬 내부 맹글링 규칙(Mangling Rules)에 의해 해당 클래스명은 실제 이름이 바뀌어 `Programmer._Programmer__COUNTRY`와 같이 접근해야 한다. 즉, 본인이 정의한 클래스 변수명 앞에 클래스명이 추가로 붙게 되는 것이다.

예를 들어 아래 소스 코드와 같이 `dir` 메서드를 통해 해당 클래스로 접근할 수 있는 객체와 메서드를 출력해보면 정의한 `__COUNTRY` 변수명이 아닌 맹글링 규칙에 의해 변경된 `_Programmer__COUNTRY` 변수명으로 접근해야 `"South Korea"`라는 할당된 값을 출력할 수 있음을 알 수 있다. 정의한 변수명으로 접근할 경우 `AttributeError`를 반환한다. 물론 이 경우에도 완벽하게 접근을 막을 수 있는 것은 아니기 때문에, 여전히 `_Programmer__COUNTRY` 변수명으로 접근하여 클래스 변수를 갱신하게 되면 그 값을 변경할 수 있게 된다.

```Python
class Programmer:
    __COUNTRY = "South Korea"


print(Programmer.__COUNTRY) # > AttributeError: type object 'Programmer' has no attribute '_Programmer'
print(dir(Programmer)) # ['_Programmer__COUNTRY', ...]
print(Programmer._Programmer__COUNTRY) # > South Korea
```

추가로 구글에서 제공하는 구글 파이썬 코드 스타일 가이드를 볼 경우 개발 생산성 등을 위해 더블 언더스코어보다는 단순히 싱글 언더스코어만 사용할 것을 추천하고 있다. 추가로 앞서 변수의 범위 때 언급했던 것처럼 글로벌 변수를 사용하지 말라고 추천하고 있기도 하다.

## 개인 의견

파이썬을 활용해 프로젝트를 진행하면서 클래스끼리 공통적으로 사용하게 되는 설정값을 어떻게 정의할 것인지에 대해 고민이 많았는데 이 컨퍼런스를 통해 여러 가지를 생각할 수 있게 되어 좋았다. 예를 들어, `boto3` 라이브러리를 활용하여 AWS S3에 데이터를 업로드하는 메서드를 구현한 객체를 구현할 경우, 클래스 변수로 `AWS_REGION_NAME` 등의 설정값을 정의하면 좋을 것 같았다. 이는 잘 변하지 않기 때문이기도 하다. 추가로 세션 관리는 인스턴스 변수로 하는 것이 좋을 지 등에 대해서는 조금 더 고민이 필요할 것 같았다. 또한 Mutable한 자료형과 Immutable한 자료형에서 선택할 때, 값이 자주 갱신되는 지 여부를 고민해야 하는 부분을 확장해서 생각해 볼 경우 결국 가비지 컬렉터의 작동 방식과 함께 고민해봐야 하는 부분처럼 느껴지기도 했다. CPython 내부 작동에 대해서 한 번쯤 살펴봐야겠다는 생각이 다시금 들었다.
