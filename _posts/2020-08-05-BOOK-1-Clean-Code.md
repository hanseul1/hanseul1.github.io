---
title: "BOOK 1 - Clean Code"
date: 2020-08-05 15:20:00 +0900
layout: single
classes: wide
categories: book development
tags: clean-code development code
toc: true
toc_sticky: true
---

# Clean Code

## 의미 있는 이름

##### 의도를 분명히 밝혀라

- 변수(또는 함수, 클래스)의 이름은 다음 질문에 모두 답할 수 있어야 한다.
  - 변수의 존재 이유는? 수행 기능은? 사용 방법은?
- 예를 들어, 시간을 나타내는 변수라면 시간 단위, 범위 등을 포함해야 한다.

##### 의미 있게 구분하라

- 비슷한 개념의 변수에 대해서 noise word를 추가하는 방식의 네이밍은 적절하지 않다.
  - 예를 들어, `product` 라는 클래스가 있다고 가정했을 때, `productInfo` 혹은 `productData` 라는 클래스를 만든다면, 이는 개념을 구분하지 않은 채 이름만 다르게 한 경우이다.
  - `info` , `data` , `a` , `the` 같은 noise word들은 의미가 불분명하다.

- 역시 같은 예로 `customer` 와 `customerInfo` , `money` 와 `moneyAmount` , `message` 와 `theMessage` 는 구분이 되지 않는다.

##### 발음하기 쉬운 이름을 사용하라

- 너무 긴 변수 이름을 무리하게 줄이기 위해서 발음이 힘든 변수를 사용해서는 안된다.
  - 예를 들어, `genymdhms` 는 `generate date year, month, day, hour, minute, second` 라는 이름을 간편하게 줄인 이름이지만, 발음이 힘들어 개발자들 간 커뮤니케이션이 어렵다.
  - 이는 `generationTimestamp` 라고 발음하기 쉬운 이름으로 바꿀 수 있다.

##### 검색하기 쉬운 이름을 사용하라

- 문자 하나, 두개짜리 이름을 가진 변수는 검색이 어렵다. 비슷한 검색 결과가 너무 많이 나오기 때문이다.
- 이름 길이는 범위 크기에 비례해야 한다.
  - 즉, 변수나 상수를 코드 여러 곳에서 사용한다면 검색하기 쉬운 이름이 바람직하다.
  - 변수가 사용되는 범위 크기가 커질 수록 이름이 더 구체적으로 지어져야 한다.
- 상수 값 역시 단순히 `5` 라고 지정하는 것 보다, `WORK_DAYS_PER_WEEK` 이라고 이름을 지정했을 때 검색이 훨씬 쉬워진다.

##### 메서드 이름

- 메서드 이름은 동사나 동사구가 적합하다.

- class constructor를 override 할 때는 정적 팩토리 메서드를 사용한다.

  - 메서드는 parameter를 설명하는 이름을 사용한다.

  ```java
  Complex fulcrumPoint = Complex.FromRealNumber(23.0);  // GOOD
  Complex fulcrumPoint = new Complex(23.0);							// BAD
  ```

  - 아래 생성자 사용을 제한하려면 private으로 선언한다.

##### 한 개념에 한 단어를 사용하라

- 추상적인 개념 하나에 단어 하나를 선택해 이를 고수한다.
  - 예를 들어, `fetch`, `retrieve` , `get` 은 `get` 으로 통일하여 사용한다.

##### 말장난을 하지 마라

- '한 개념에 한 단어를 사용하라'는 규칙을 따르기 위해서 여러 클래스에 `add` 라는 메서드가 생겼다.
  - 모두 같은 맥락으로 일관성을 가지고 있다면 문제 없다.
  - 그러나 지금까지 구현한 `add` 라는 메서드는 기존 값 두개를 더하는 메서드였고, 새로 작성하는 메서드는 집합에 값 하나를 추가하는 메서드라고 해보자.
    - 새로 추가되는 메서드는 기존 `add` 메서드와 맥락이 다르다.
    - 새로 추가되는 메서드는 `add` 보다는 `append` , `insert` 같은 이름이 더 적절할 것이다.

##### 불필요한 맥락을 없애라

- `customerAddress` 나 `accountAddress` 는 `Address` 클래스의 인스턴스 이름으로는 좋은 이름이나 클래스 이름으로는 적합하지 않다.
- 의미가 분명한 경우에 한해서 이름에 불필요한 맥락을 추가하지 않도록 주의한다.



## 함수

##### 작게 만들어라

- `if/else` 문, `while` 문 등에 들어가는 블록은 한 줄이어야 좋다.
  - 즉, 블록 안에서 다른 함수를 호출한다.
- 중첩 구조가 생길만큼 함수가 커져서는 안된다.
  - 들여쓰기 수준은 1단이나 2단을 넘어서지 않는 것이 좋다.

##### 한 가지만 해라

- 함수는 한 가지를 해야 한다. 그 한 가지를 잘 해야 한다. 그 한 가지만을 해야 한다.

- 여기서 한 가지 일이란, 추상화 수준이 하나인 단계만 수행하는 것을 의미한다.

  - 한 함수 내에 추상화 수준을 섞으면 특정 표현이 근본 개념인지 아니면 세부사항인지 구분하기 어려워진다.

  ```java
  public static String testableHtml(PageData pageData, boolean includeSuiteSetup) {
    ...
    String pagePathNAme = pathParser.render(pagePath);  // 추상화 수준: 중간
    buffer.append("!include -setup .")									// 추상화 수준: 낮음
      ...
      
    return pageData.getHtml();													// 추상화 수준: 높음
  }
  ```

- 코드는 위에서 아래로 이야기처럼 읽혀야 좋다.
  - 한 함수 다음에는 추상화 수준이 한 단계 낮은 함수가 온다.
  - 즉, 위에서 아래로 읽으면 함수 추상화 수준이 한번에 한 단계씩 낮아진다.

##### switch문

- 본질적으로 `switch` 문은 N가지 작업을 처리한다.
- `switch` 문을 완전히 피할수는 없지만 `switch` 문을 저차원 클래스에 숨기고 절대로 반복하지 않는 방법이 있다.
  - 이는 polymorphism을 이용한다.

```java
public Money calculatePay(Employee e) throws InvalidEmployeeType {
  switch(e.type) {
    case COMMISIONED:
      return calculateCommissionedPay(e);
    case HOURLY:
      return calculateHourlyPay(e);
    case SALARIED:
      return calculateSalariedPay(e);
    default:
      throw new InvalidEmployeeType(e.type);
  }
}
```

- 위 함수는 새 직원 유형을 추가했을 때 끊임없이 길어질 여지를 가지고 있다.
  - 또한, 한 가지 작업만 수행하지 않는다.
  - `isPayday(...)` 나 `deliverPay(...)` 같은 위 함수와 동일한 구조의 함수가 무한정 존재할 수 있다는 위험이 있다.
- 위 문제를 해결하기 위해서 `switch` 문을 abstract factory에 숨겨 Employee 자식 클래스의 인스턴스를 생성하도록 한다.
  - 그리고 `calculatePay(...)` 같은 함수들은 Employee 인터페이스를 거쳐 호출되도록 한다.

```java
public abstract class Employee {
  public abstract boolean isPayDay();
  public abstract Money calculatePay();
  public abstract void deliverPay(Money pay);
}

public interface EmployeeFactory {
  public Employee makeEmployee(EmployeeRecord r) throws InvalidEmployeeType;
}

public class EmployeeFactoryImpl implements EmployeeFactory {
  public Employee makeEmployee(EmployeeRecord r) throws InvalidEmployeeType {
    switch(r.type) {
      case COMMISIONED:
        return new CommissionedEmployee(r);
      case HOURLY:
        return new HourlyEmployee(r);
      case SALARIED:
        return new SalariedEmployee(r);
      default:
        throw new InvalidEmployeeType(e.type);
    }
  }
}
```

##### 함수 인수

- 함수에서 parameter는 적을수록 좋다.

- 인스턴스 변수를 선언하는 대신 함수 인수로 변수를 넘기게 되면, 코드를 읽는 사람은 현 시점에서 별로 중요하지 않은 해당 인수의 의미를 해석해야 한다.

- 테스트 관점에서도 인수가 많은 함수는 갖가지 인수 조합으로 함수를 검증해야 하기 때문에 까다롭다.

- 변환 함수에서 출력 인수를 사용하지 않도록 한다.

  ```java
  void transform(StringBuffer out);						// BAD
  StringBuffer transform(StringBuffer in);		// GOOD
  ```

  - 입력 인수를 변환하는 함수라면 변환 결과는 return 값으로 돌려준다.

- 함수로 `boolean` 값을 넘기는 것은 함수가 한꺼번에 여러 가지 일을 처리한다는 셈이므로 가급적 쓰지 않도록 한다.

- `writeField(name)` 이 `writeField(outputStream, name)` 보다 이해하기 쉽다.

  - `outputStream` 과 `name` 은 한 값을 표현하지도, 자연적인 순서가 있지도 않다.
  - 이는 `writeField(...)` 함수를 `OutputStream` 클래스의 메서드로 만들어 `outputStream.writeField(name)` 같은 형태로 수정할 수 있다.
  - 또는 `outputStream` 을 현재 클래스의 member field로 만들어 인수로 넘기지 않도록 수정할 수도 있다.

##### 부수 효과를 일으키지 마라

- 부수적인 일은 예상치 못하게 클래스 변수나 함수의 인수, 시스템 전역 변수를 수정한다.
  - 이는 많은 경우 temporal coupling이나 order dependency를 초래한다.
- 예를 들어, `checkPassword(name, password)` 라는 함수에서 비밀번호를 체크하는 작업 외에 세션을 초기화하는 작업을 부수적으로 해서는 안된다.
  - 이런 부수 효과는 해당 함수를 세션을 초기화해도 괜찮은 경우에만 호출해야 한다는 시간적인 결합(`temporal coupling`)을 초래한다.

##### 오류 코드보다 예외를 사용하라

- 함수마다 실행 결과를 if 문으로 오류 코드와 비교하는 대신, 예외를 던지도록 해서 `try/catch` 문을 사용한다.
  - 오류 코드 대신 예외를 사용하면 오류 처리 코드가 원래 함수가 하려던 코드에서 분리되므로 코드가 깔끔해진다.
  - 예를 들어, 아래 예제에서 `if(deletePage(page)) == E_OK` 같은 오류 코드 비교를 굳이 하지 않아도 된다.
- `try/catch` 블록은 별도 함수로 뽑아내는 것이 정상 동작과 오류 처리 동작을 뒤섞지 않는 좋은 방법이다.

```java
public void delete(Page page) {
  try {
    deletePageAndAllReferences(page);
  } catch(Exception e) {
    logError(e);
  }
}

private void deletePageAndAllReferences(Page page) throws Exception {
  deletePage(page);
  registry.deleteReference(page.name);
  configKey.deleteKey(page.name.makeKey());
}

private void logError(Exception e) {
  logger.log(e.getMessage());
}
```



## 주석

##### 주석은 나쁜 코드를 보완하지 못한다

- 코드에 주석을 추가하는 일반적인 이유는 코드 품질이 나쁘기 때문이다.
- 표현력이 풍부하고 깔끔하며 주석이 거의 없는 코드가, 복잡하고 어수선하며 주석이 많이 달린 코드보다 훨씬 좋다.

##### 코드로 의도를 표현하라

- 주석으로 달려는 설명을 함수로 만들어 표현할 수 있다.

```java
// 직원에게 복지 혜택을 받을 자격이 있는지 검사한다.
if((employee.flags & HOURLY_FLAG) && (employee.age > 65))
```

```java
if(employee.isEligibleForFullBenefits())
```

- 위의 주석이 있는 코드보다 아래의 함수명으로 의도가 표현된 코드가 훨씬 깔끔하고 이해하기 쉽다.

##### 좋은 주석

###### 법적인 주석

- 소스 파일 첫머리에 주석으로 들어가는 저작권 정보와 소유권 정보는 필요하고도 타당하다.

###### 결과를 경고하는 주석

- 다른 프로그래머에게 결과를 경고할 목적으로 주석을 사용할 수 있다.

- 특정 테스트 케이스를 꺼야하는 이유를 설명하는 목적으로 사용할 수 있다.

  ```java
  // 시간이 충분하지 않다면 이 테스트 케이스를 실행하지 마십시오.
  @Test
  ...
  ```

  - 요즘에는 `@Ignore` 속성을 이용해 테스트 케이스를 꺼버릴 수 있기 때문에 잘 쓰이지 않는다.

  ```java
  @Ignore("실행이 너무 오래 걸림")
  ```

###### TODO 주석

- 프로그래머가 필요하다 여기지만 당장 구현하기 어려운 업무를 기술할 때 사용한다.
- 주기적으로 TODO 주석을 점검해 없애도 괜찮은 주석은 없애는 작업을 수행해야 한다.

###### 중요성을 강조하는 주석

- 자칫 대수롭지 않다고 여겨질 뭔가의 중요성을 강조하기 위해서도 주석을 사용한다.

```java
String listItemContent = match.group(3).trim();
// 여기서 trim은 정말 중요하다. trim 함수는 문자열에서 시작 공백을 제거한다.
// 문자열에 시작 공백이 있으면 다른 문자열로 인식되기 때문이다.
```

##### 함수나 변수로 표현할 수 있다면 주석을 달지 마라

- 앞에서도 설명한 것과 같이 코드의 함수나 변수명으로 의도를 설명할 수 있다면 주석이 필요하지 않다.

```java
// 전역 목록 <smodule>에 속하는 모듈이 우리가 속한 하위 시스템에 의존하는가?
if(smodule.getDependSubSystems().contains(subSysMod.getSubSystem()))
```

- 이 코드에서 주석을 없애고 코드로 표현하면 다음과 같다.

```java
ArrayList moduleDependees = smodule.getDependSubSystems();
String ourSubSystem = subSysMod.getSubSystem();
if(moduleDependees.contains(ourSubSystem))
```



## 형식 맞추기

##### 적절한 행 길이를 유지하라

- 일반적으로 큰 파일보다 작은 파일이 이해하기 쉽다.

###### 신문 기사처럼 작성하라

- 파일 이름은 간단하면서도 설명이 가능하게 짓는다.
  - 이름만 보고도 올바른 모듈을 살펴보고 있는지 아닌지를 판단할 수 있도록 짓는다.
- 소스 파일 첫 부분은 고차원 개념과 알고리즘을 설명하고, 아래로 내려갈수록 의도를 세세하게 묘사한다.
  - 즉, 마지막에는 가장 저차원의 함수와 세부 내역이 나온다.

###### 세로 밀집도

- 서로 밀집한 코드 행은 세로로 가까이 놓여야 한다.
  - 예를 들어, 한 클래스 내의 인스턴스 변수는 서로 가까이 두어 메서드 영역과 분리되어 파악할 수 있게 한다.

##### 변수 선언

- 변수는 사용하는 위치에 최대한 가까이 선언한다.
- 함수 내의 지역 변수는 각 함수 맨 처음에 선언한다.
  - 다소 긴 함수에서는 블록 상단이나 루프 직전에 변수를 선언할 수도 있다.

- 인스턴스 변수는 클래스 맨 처음에 선언한다.

##### 종속 함수

- 한 함수가 다른 함수를 호출한다면 두 함수는 세로로 가까이 배치한다.
  - 되도록 호출되는 함수는 호출하는 함수의 아래쪽에 배치한다.(고차원 => 저차원)
  - 한 함수에서 호출되는 함수가 여러개라면 호출되는 순서대로 아래쪽에 배치한다.(위에서 아래로 읽기)



## 객체와 자료 구조

##### 자료 추상화

```java
public class Point {
  public double x;
  public double y;
}
```

```java
public interface Point {
  double getX();
  double getY();
  void setCartesian(double x, double y);
  double getR();
  double getTheta();
  void setPolar(double r, double theta);
}
```

- 첫번째 클래스는 직교 좌표계를 사용하는 것을 사용자가 바로 알 수 있고, 구현을 노출한다.
  - 인스턴스 변수를 `private` 으로 선언하고 `getter/setter` 함수를 제공하더라도 구현이 노출되는 것은 마찬가지이다.
- 두번째 인터페이스는 구현 내부에서 직교좌표계를 사용하는지, 극좌표계를 사용하는지 사용자가 알 수 없다.
- 변수 사이에 함수라는 계층을 넣는다고 구현이 저절로 감춰지지는 않는다.
  - 추상 인터페이스를 제공해 사용자가 구현을 모른 채 자료의 핵심을 조작할 수 있어야 진정한 의미의 클래스이다.

##### 자료/객체 비대칭

- 객체는 추상화 뒤로 자료를 숨긴 채 자료를 다루는 함수만 공개한다.
- 자료 구조는 자료를 그대로 공개하며 별다른 함수는 제공하지 않는다.

```java
public class Square {
  public Point topLeft;
  public double side;
}

public class Rectangle {
  public Point topLeft;
  public double height;
  public double width;
}

public class Circle {
  public Point center;
  public double radius;
}

public class Geometry {
  public static final double PI = 3.141592;
  
  public double area(Object shape) throws NoSuchShapeException {
    ...
  }
}
```

- 위 코드는 객체 지향에는 맞지 않는 코드이다.

- 각 도형 클래스가 자료 구조로써 각 자료를 그대로 공개하고, 개별로 구현된 함수를 가지고 있지도 않다.

  - 위 코드에서 둘레 길이를 구하는 `perimeter()` 함수를 추가하고 싶다면, 각 도형 클래스는 전혀 영향을 받지 않는다.

  - 그러나 새로운 도형을 추가하고 싶다면, `Geometry` 클래스에 속한 모든 함수를 고쳐야 한다.

```java
public class Square implements Shape {
  private Point topLeft;
  private double side;
  
  public double area() {
    return side * side;
  }
}

public class Rectangle implements Shape {
  private Point topLeft;
  private double height;
  private double width;
  
  public double area() {
    return height * width;
  }
}

public class Circle implements Shape {
  private Point center;
  private double radius;
  public final double PI = 3.141592;
  
  public double area() {
    return PI * radius * radius;
  }
}
```

- 위 코드는 객체 지향 코드이다.
- 각 도형 클래스는 인스턴스 변수를 `private` 으로 감추고, 개별 클래스들이 `area()` 함수를 오버라이드하여 구현하였다.
  - 위 코드에서 새로운 함수를 추가하기 위해서는 모든 도형 클래스들을 수정해야 한다.
  - 그러나 새로운 도형을 추가할 때에는 기존 함수나 클래스에 전혀 영향을 끼치지 않는다.

- 즉, 절차 지향 코드는 새로운 자료 구조를 추가하기 어렵다. 그리고 객체 지향 코드는 새로운 함수를 추가하기 어렵다.
  - 시스템에 따라 적합한 코드를 지향할 수 있어야 한다.

##### 디미터 법칙

- 모듈은 자신이 조작하는 객체의 속사정을 몰라야 한다는 법칙
  - 즉, 객체는 조회 함수로 내부 함수를 공개하면 안된다는 의미이다.
- 클래스 C의 메서드 f는 다음과 같은 객체의 메서드만 호출해야 한다.
  - 클래스 C
  - f가 생성한 객체
  - f 인수로 넘어온 객체
  - C 인스턴스 변수에 저장된 객체

```java
final String outputDir = ctxt.getOptions().getScratchDir().getAbsolutePath();
```

- 위 코드는 흔히 `train wreck` 이라 부른다.

- 위 코드가 포함된 메서드에서는 ctxt 객체가 Options 를 포함하며, Options가 ScratchDir을 포함하고, ScratchDir이 AbsolutePath를 포함한다는 사실을 알게 된다.

  - 위 코드를 사용하는 메서드는 많은 객체를 탐색할 줄 안다는 의미이다.

- 위 코드의 `ctxt` , `Options` , `ScratchDir` 이 객체인지 아니면 자료 구조인지에 따라 디미터 법칙을 위반하는지 여부가 판단된다.

  - 자료 구조라면 내부 구조를 노출하는게 당연하므로 디미터 법칙이 적용되지 않는다.
  - 자료 구조일 때 코드를 다음과 같이 구현했다면 디미터 법칙을 거론할 필요가 없어진다.

  ```java
  final String outputDir = ctxt.options.scratchDir.absolutePath;
  ```

- `ctxt` 가 객체였다면 위 코드는 디미터 법칙을 위반한 것이다.

  - 디미터 법칙을 위반하지 않고 임시 디렉토리의 절대 경로는 어떻게 얻을 수 있을까?
  - `ctxt` 가 객체라면 뭔가를 하라고 작업을 지시해야지 자료를 드러내라고 하면 안된다.
  - 임시 디렉토리의 절대 경로가 왜 필요한지 판단하고, 그 일을 시키면 된다.

  ```java
  String outFile = outputDir + "/" + className.replace('.', '/') + ".class";
  FileOutputStream fout = new FileOutputStream(outFile);
  BufferedOutputStream bos = new BufferedOutputStream(fout);
  ```

  - 임시 파일을 생성하기 위해 임시 디렉토리의 절대 경로가 필요했다.
    - 그러면 `ctxt` 객체에 임시 파일을 생성하라고 작업을 지시하면 된다.

  ```java
  BufferedOutputStream bos = ctxt.createScratchFileStream(classFileName);
  ```



## 오류 처리

##### 예외에 의미를 제공하라

- 오류 메시지에 실패한 연산 이름과 실패 유형 같은 정보를 담아 예외를 던진다.
- 로깅 기능을 사용한다면 catch 블록에서 오류를 기록하도록 충분한 정보를 넘겨준다.

##### 호출자를 고려해 예외 클래스를 정의하라

- 오류를 정의할 때 가장 중요한 관심사는 오류를 잡아내는 방법이 되어야 한다.

```java
ACMEPort port = new ACMEPort(12);

try {
  port.open();
} catch(DeviceResponseException e) {
  reportPortError(e);
  logger.log("Device response exception", e);
} catch(ATM1212UnlockedException e) {
  reportPortError(e);
  logger.log("Unlock exception", e);
} catch(GMXError e) {
  reportPortError(e);
  logger.log("Device response exception", e);
} finally {
  ...
}
```

- 위 코드는 중복도 심하고 예외에 대응하는 방식이 예외 유형과 무관하게 거의 동일하다.
- 외부 라이브러리를 호출하는 곳에서 외부 라이브러리가 던지는 예외를 모두 잡아 처리하게 되면서 복잡도가 증가하게 되었다.
- 이 경우 외부 라이브러리가 던지는 예외를 잡아 변환하는 wrapper 클래스를 정의하여 문제를 해결할 수 있다.
  - 즉, 호출하는 라이브러리 API를 감싸 하나의 예외 유형을 반환하도록 한다.

```java
public class LocalPort {
  private ACMEPort innerPort;
  
  public LocalPort(int portNumber) {
    innerPort = new ACMEPort(portNumber);
  }
  
  public void open() {
    try {
      innerPort.open();
    } catch(DeviceResponseException e) {
      throw new PortDeviceFailure(e);
    } catch(ATM1212UnlockedException e) {
      throw new PortDeviceFailure(e);
    } catch(GMXError e) {
      throw new PortDeviceFailure(e);
    }
  }
}

LocalPort port = new LocalPort(12);

try {
  port.open();
} catch(PortDeviceFailure e) {
  reportError(e);
  logger.log(e.getMessage(), e);
} finally {
  ...
}
```

- 외부 API를 감싸면 외부 라이브러리와 프로그램 사이에 의존성이 크게 줄어든다.
  - 또한, 프로그램이 사용하기 편리한 API를 새로 정의함으로써 특정 업체가 API를 설계한 방식에 발목 잡히지 않아도 된다.

##### null을 반환하지 마라

- 메서드에서 `null` 을 반환하고 싶은 유혹이 든다면 그 대신 예외를 던지거나 특수 사례 객체(`Special case pattern`) 를 반환한다.

```java
List<Employee> employees = getEmployees();
if(employees != null) {
  for(Employee e : employees) {
    totalPay += e.getPay();
  }
}
```

- 위 코드에서 `getEmployees()` 가 꼭 `null` 을 반환할 필요 없이 빈 리스트를 반환한다면 굳이 `null` 확인을 하지 않아도 된다.

  ```java
  public List<Employee> getEmployees() {
    if( ...직원이 없다면 ) {
      return Collections.emptyList();
    }
  }
  ```

##### null을 전달하지 마라

- 메서드로 null을 전달하는 방식은 최대한 피한다.



## 경계

##### 외부 코드 사용하기

- 외부 라이브러리는 사용자에게 필요하지 않은 기능까지 제공한다.
  - 즉, 프로그램 내에서 외부 라이브러리 인터페이스가 여기저기 사용된다고 하였을 때 위험성이 따른다.
  - 예를 들어, `Map` 인터페이스의 사용자는 해당 객체의 내용을 삭제하길 바라지 않지만, `Map` 인터페이스는 `clear()` 함수를 제공하여 누구든지 그 내용을 삭제시킬 수 있다.
- 외부 라이브러리 인터페이스에 변경 사항이 생겼을 때 해당 인터페이스가 여기저기 사용되었다면 역시 대규모의 수정이 필요하게 된다.

```java
public class Sensors {
  private Map sensors = new HashMap();
  
  public Sensor getById(String id) {
    return (Sensor) sensors.get(id);
  }
  
  ...
}
```

- 위 코드처럼 경계 인터페이스인 `Map` 인터페이스를 `Sensors` 클래스(Wrapper 클래스) 안으로 숨기게 되면 `Map` 인터페이스가 프로그램 여기저기 사용되는 것을 방지한다.
  - 즉, `Map` 인터페이스가 변하더라도 나머지 프로그램에는 영향을 미치지 않는다.
- 또한 `Sensors` 클래스는 프로그램에 필요한 인터페이스만 제공한다.
  - 사용자가 이해하기 쉽고, 오용하기는 어렵다.
  - 나머지 프로그램이 설계 규칙과 비즈니스 규칙을 따르도록 강제할 수 있다.

##### 경계 살피고 익히기

- 외부 코드를 사용하고자 할 때, 곧바로 우리 프로그램 코드를 작성해 외부 코드를 호출하는 대신 먼저 간단한 테스트 케이스를 작성해 외부 코드를 익힌다.
  - 이를 `학습 테스트` 라 부른다.
  - 프로그램에서 사용하려는 방식대로 API를 호출하고, 통제된 환경에서 API를 제대로 이해하는지를 확인한다.
- 학습테스트로 외부 코드를 사용하는 방법을 익혔다면 독자적인 클래스를 정의하여 캡슐화 한다.
  - 나머지 프로그램은 해당 경계 인터페이스를 모르고도 사용할 수 있게 된다.

##### 아직 존재하지 않는 코드를 사용하기

- 우리가 바라는 인터페이스를 정의하여 우리가 인터페이스를 전적으로 통제하도록 한다.
- API가 구현되었으면 `Adapter` 로 우리가 정의한 인터페이스와의 간극을 메우도록 한다.
  - ADAPTER 패턴의 사용은 API 사용을 캡슐화해 API가 바뀔 때 수정할 코드를 한 곳으로 모으도록 한다.



## 단위 테스트

##### 깨끗한 테스트 코드 유지하기

- 테스트는 유연성, 유지보수성, 재사용성을 제공한다.
  - 테스트 케이스가 없다면 모든 변경이 잠정적인 버그다.
- 테스트 코드가 지저분할수록 변경하기 어려워진다.
  - 새 버전을 출시할 때마다 팀이 테스트 케이스를 유지하고 보수하는 비용도 늘어난다.

##### 깨끗한 테스트 코드

- `BUILD-OPERATE-CHECK` 패턴
  - 각 테스트는 세 부분으로 나누어진다.
  - 첫 부분은 테스트 자료를 만든다.
  - 두 번째 부분은 테스트 자료를 조작한다.
  - 세 번째 부분은 조작한 결과가 올바른지 확인한다.
- 테스트 코드는 본론에 돌입해 진짜 필요한 자료 유형과 함수만 사용한다.

##### 테스트 당 assert 하나

- `assert` 문이 단 하나인 함수는 결론이 하나라서 코드를 이해하기 쉽다.
- `given-when-then` 관례를 사용하면 테스트 코드를 읽기 쉬워진다.
  - `@Before` 함수에 `given/when` 부분을 넣고 `@Test` 함수에 `then` 부분을 넣어도 된다.
- 그러나 부득이한 경우 테스트 당 개념 하나 라는 규칙을 따르도록 한다.
- 그러나 부득이한 경우 `테스트 당 개념 하나` 라는 규칙을 따르도록 한다.

##### F.I.R.S.T

- `Fast` : 테스트는 빨리 돌아야 한다. 테스트가 느리면 자주 돌릴 엄두를 못내고, 코드 품질이 망가지기 시작한다.
- `Independent` : 각 테스트는 서로 의존하면 안 된다. 각 테스트는 독립적으로 그리고 어떤 순서로 실행해도 괜찮아야 한다.
- `Repeatable` : 테스트는 어떤 환경에서도 반복 가능해야 한다.
- `Self-Validating` : 테스트는 bool 값으로 결과를 내야 한다. 테스트 통과 여부를 알기 위해서 로그 파일을 직접 확인해야 하는 경우는 없어야 한다.
- `Timely` : 테스트는 적시에 작성해야 한다. 단위 테스트는 테스트하려는 실제 코드를 구현하기 직전에 구현한다.



## 클래스

##### 클래스 체계

- 표준 자바 관례에 따르면, 클래스의 가장 먼저 변수 목록이 나온다.
  - `public static` 상수가 맨 처음 나오고, 그 다음 `private static` 상수가 나오며, 이어서 `private` 인스턴스 변수가 나온다.
  - `public` 인스턴스 변수가 필요한 경우는 거의 없다.
- 변수 목록 다음에는 `public` 함수가 나온다. 
  - `private` 함수는 자신을 호출하는 `public` 함수 직후에 넣는다.

##### 클래스는 작아야 한다

- 클래스 설계할 때도 함수와 마찬가지로 '작게' 가 규칙이다.
  - 함수는 물리적인 행 수로 크기를 측정했다면, 클래스는 클래스가 맡은 책임으로 크기를 측정한다.
- 클래스 이름을 명명할 때 간결한 이름이 떠오르지 않거나 모호하다면, 클래스의 책임이 너무 많아서이다.
  - 클래스 이름에 `Processor` , `Manager` , `Super` 등과 같은 모호한 단어가 있다면 클래스가 여러 책임을 맡았다는 증거다.

###### SRP (Single Responsibility Principle)

- 클래스나 모듈을 변경할 이유가 단 하나뿐이어야 한다는 원칙이다.

###### Cohesion

- 클래스는 인스턴스 변수가 적어야 한다. 각 클래스 메서드는 인스턴스 변수를 하나 이상 사용해야 한다.
  - 모든 인스턴스 변수를 메서드마다 사용하는 클래스는 응집도가 가장 높다.
- 큰 함수를 작은 함수 여러개로 분리하다보면 클래스의 응집도가 낮아지게 된다.
  - 몇몇 함수만 사용하는 인스턴스 변수가 늘어나기 때문이다.
  - 그러면 몇몇 변수만 사용하는 몇몇 함수를 독자적인 클래스로 분리하면 응집력이 높은 클래스를 만들 수 있다.

##### 변경하기 쉬운 클래스

```java
public class Sql {
  public Sql(String table, Column[] columns)
  public String create()
  public String insert(Object[] fields)
  public String selectAll()
  public String findByKey(String keyColumn, String keyValue)
  public String select(Column column, String pattern)
  public String select(Criteria criteria)
  public String preparedInsert()
  private String columnList(Column[] columns)
  private String valuesList(Object[] fields, final Column[] columns)
  private String selectWithCriteria(String criteria)
  private String placeholderList(Column[] columns)
}
```

- 위 클래스는 SRP를 위반한다.
  - 기존 SQL문 하나를 수정할 때도 반드시 Sql 클래스를 수정해야 한다. 또한, 새로운 SQL문 (예를 들어 update문)을 지원하려면 역시 Sql 클래스를 수정해야 한다.
  - 구조적인 관점에서도 `selectWithCriteria` 라는 `private` 함수는 select문을 처리할 때만 사용하기 때문에 SRP를 위반한다고 볼 수 있다.
    - 클래스 일부에서만 사용되는 `private` 함수는 대체로 코드를 개선할 여지를 시사한다.

```java
abstract public class Sql {
  public Sql(String table, Column[] columns)
  abstract public String generate();
}

public class CreateSql extends Sql {
  public CreateSql(String table, Column[] columns)
  @override public String generate()
}

public class SelectSql extends Sql {
  public SelectSql(String table, Column[] columns)
  @override public String generate()
}

public class InsertSql extends Sql {
  public InsertSql(String table, Column[] columns, Object[] fields)
  @override public String generate()
  private String valuesList(Object[] fields, fianl Column[] columns)
}

...
```

- 위와 같이 각 SQL문을 생성하는 메서드를 Sql 추상 클래스를 상속받는 클래스를 만들어 옮겼다.
  - `private` 메서드들은 각 책임에 맞게 자식 클래스로 옮겼다.
  - 모든 자식 클래스가 공통으로 사용하는 `private` 클래스는 유틸리티 클래스를 만들어 사용할 수 있다.
- 클래스가 서로 분리되어 update문을 추가할 때 기존 클래스를 변경할 필요가 없다.
- 위처럼 변경했을 때 SRP를 지원하는 것 뿐만 아니라 OCP도 지원한다.
  - OCP (Open-Closed Principle)란 클래스는 확장에 개방적이고 수정에 폐쇄적이어야 한다는 원칙이다.
- 새 기능을 추가하거나 기존 기능을 변경할 때 건드릴 코드가 최소인 시스템 구조가 바람직하다.

###### 변경으로부터 격리

- 요구사항이 변하면서 코드는 계속해서 변하기 마련이다.

- 상세한 구현에 의존하는 클라이언트 클래스는 구현이 바뀌면 위험에 빠진다.

  - 그래서 인터페이스와 추상 클래스를 사용해 구현이 미치는 영향을 격리한다.

- 특히 외부 API는 클라이언트 클래스에서 변화에 대응하기 어려우므로 API를 직접 호출하는 대신 인터페이스를 생성한 후 메서드를 하나 선언한다.

  - 예를 들어, `Portfolio` 클래스를 만든다고 가정한다. 이 클래스는 외부 `TokyoStockExchange` API를 사용해 포트폴리오 값을 계산한다.

    - 그런데 이 API는 시세 변화에 영향을 받기 때문에 5분마다 값이 바뀐다.
    - 이 경우 테스트 코드를 짜는 것이 쉽지 않다.

  - `Portfolio` 클래스에서 직접 `TokyoStockExchange` API를 호출하는 대신 `StockExchange` 라는 인터페이스를 생성한다.

    ```java
    public interface StockExchange {
      Money currentPrice(String symbol);
    }
    ```

  - 클라이언트 클래스에서는 해당 인터페이스를 의존하도록 한다.

    ```java
    public Portfolio {
      private StockExchange exchange;
      public Portfolio(StockExchange exchange) {
        this.exchange = exchange;
      }
      ...
    }
    ```

  - 이제 `TokyoStockExchange` 클래스를 흉내내는 테스트용 `FixedStockExchangeStub` 클래스를 만들어 테스트 코드를 작성할 수 있다.

    - 해당 테스트용 클래스는 고정된 주가를 반환한다.
    - 물론 실제 코드에서는 `TokyoStockExchange` API를 호출하는 구현 클래스를 따로 만들어 사용한다.

    ```java
    public class PortfolioTest {
      private FixedStockExchangeStub exchange;
      private Portfolio portfolio;
      
      @Before
      protected void setup() throws Exception {
        exchange = new FixedStockExchangeStub();
        exchange.fix("MSFT", 100);
        portfolio = new Portfolio(exchange);
      }
      
      @Test
      public void givenFiveMSFTTotalShouldBe500() throws Exception {
        portfolio.add(5, "MSFT");
        Assert.assertEquals(500, portfolio.value());
      }
    }
    ```

- 시스템의 결합도를 낮추면 유연성과 재사용성도 높아진다.

  - 이로써 DIP도 지원하는 클래스를 만들 수 있다.
  - DIP (Dependency Inversion Principle)란 상세한 구현이 아니라 추상화에 의존해야 한다는 원칙이다.



## 시스템

##### 시스템 제작과 시스템 사용을 분리하라

- 소프트웨어 시스템은 어플리케이션 객체를 제작하고 의존성을 서로 연결하는 준비 과정과 준비 과정 이후에 이어지는 런타임 로직을 분리해야 한다.

```java
public Service getService() {
  if(service == null) {
    service = new MyServiceImpl();
  }
  return service;
}
```

- 위 코드는 전형적인 Lazy Initialization 이다. 
  - 실제로 필요할 때까지 객체를 생성하지 않으므로 불필요한 부하를 줄일 수 있고, 어떤 경우에도 null 포인터를 반환하지 않는다는 장점이 있다.
  - 그러나 `getService()` 메서드가 `MyServiceImpl` 클래스와 해당 클래스의 생성자 인수에 명시적으로 의존한다.
    - 즉, 런타임 로직에서 `MyServiceImpl` 객체를 전혀 사용하지 않더라도 의존성을 해결하지 않으면 컴파일이 되지 않는다.
  - 또한 `MyServiceImpl` 이 모든 문맥에 적합한 객체인지 알 수 없다.
- 또한 일반 런타임 로직에 객체 생성 로직을 섞어놓아 작게나마 SRP를 깬다고 할 수 있다.
- 설정 논리는 일반 실행 논리와 분리해야 모듈성이 높아진다.

###### Main 분리

- 생성과 관련한 코드는 모두 `main` 이나 main이 호출하는 모듈로 옮기고, 나머지 시스템은 모든 객체가 생성되었고 모든 의존성이 연결되었다고 가정한다.
- 즉, `main` 함수에서 시스템에 필요한 객체를 생성한 후 이를 application에 넘긴다.
  - application은 `main` 이나 객체가 생성되는 과정을 전혀 모른다.

###### Factory

- 때로는 객체가 생성되는 시점을 application이 결정할 필요도 생긴다.
- 이 때 `ABSTRACT FACTORY` 패턴을 사용한다.
- 예를 들어, `OrderProcessing` application이 `LineItem` 을 생성하는 시점을 결정한다고 할 때, Factory interface를 구현하여 `LineItem` 인스턴스가 생성되는 시점을 통제할 수 있도록 한다.
  - `main` 쪽에 있는  `LineItemFactoryImpl` 클래스(`LineItemFactory` 인터페이스를 구현한 클래스) 가 `LineItem` 이 생성되는 구체적인 방법을 알고, `OrderProcessing` application은 단지 시점만 통제할 뿐 구체적인 구현은 모른다.

###### Dependency Injection

- DI는 IoC (Inversion of Control) 기법을 의존성 관리에 적용한 매커니즘이다.

  - 한 객체가 맡은 보조 책임을 새로운 객체에게 전적으로 떠넘긴다.

  - 객체는 의존성 자체를 인스턴스로 만드는 책임을 지지 않는다.
  - 대개 의존성을 설정하는 책임을 맡는 매커니즘으로 `main` 루틴이나 특수 `container` 를 사용한다.
    - 스프링 프레임워크는 자바 DI container를 제공한다.

- 클래스는 의존성을 직접 해결하려 시도하지 않는다. 

  - 대신 의존성을 주입하는 방법으로 `setter` 메서드나 `constructor` parameter를 제공한다.
  - DI container가 필요한 객체의 인스턴스를 만든 후 생성자 인수나 setter 메서드를 사용해 의존성을 설정한다.

- 대다수 DI container는 필요할 때까지는 객체를 생성하지 않고, 대부분은 Lazy initialization을 지원하여 `Factory` 를 호출하거나 proxy를 생성하는 방법을 제공한다.

##### 확장

###### Cross-Cutting concerns

- 트랜잭션, 보안, 영속성과 같은 관심사는 어플리케이션의 자연스러운 객체 경계를 넘나드는 경향이 있다.
  - 따라서 모든 객체가 전반적으로 동일한 방식을 이용하게 만들어야 한다.
- AOP (Aspect Oriented Programming) 는 횡단 관심사에 대처해 모듈성을 확보하는 일반적인 방법론이다.
  - 특정 관심사를 지원하려면 시스템에서 특정 지점들이 동작하는 방식을 일관성 있게 바꿔야 한다.
  - 영속성을 예로 들면, 영속적으로 저장할 객체와 속성을 선언한 후 영속성 책임을 영속성 프레임워크에 위임한다.
    - AOP 프레임워크는 대상 소스 코드를 일일이 수정하지 않고 동작 방식을 변경한다.

##### 자바 프록시

- Proxy Pattern은 실제 기능을 수행하는 객체 대신 가상의 객체를 사용해 로직의 흐름을 제어하는 패턴이다.
  - 원래 객체의 기능에 더해 부가적인 작업(로깅, 인증, 트랜잭션 등) 을 수행하기에 좋다.
  - 사용자(client) 입장에서는 프록시 객체나 실제 객체나 사용법이 유사하므로 사용성이 좋다.
- 즉, 개별 객체나 클래스에서 메서드 호출을 감싸는 경우에 사용한다.

```java
public interface Bank {
  Collection<Account> getAccounts();
  void setAccounts(Collection<Account> accounts);
}

// Bank 인터페이스를 구현하는 POJO
public class BankImpl implements Bank {
  private List<Account> accounts;
  
  public Collection<Account> getAccounts() {
    return accounts;
  }
  
  public void setAccounts(Collection<Account> accounts) {
    this.accounts = new ArrayList<Account>();
    for(Account account : accounts) {
      this.accounts.add(account);
    }
  }
}

public class BankProxyHandler implements InvocationHandler {
  private Bank bank;
  
  public BankProxyHandler(Bank bank) {
		this.bank = bank;
  }
  
  public Object invoke(Object proxy, Method method, Object[] args) {
    String methodName = method.getName();
    if(methodName.equals("getAccounts")) {
      bank.setAccounts(getAccountsFromDatabase());
      return bank.getAccounts();
    } else if(methodName.equals("setAccounts")) {
      bank.setAccounts((Collection<Account>) args[0]);
      setAccountsToDatabase(bank.getAccounts());
      return null;
    } else {
      ...
    }
  }
  
  // 영속성 관련 메서드 세부 구현
  protected Collection<Account> getAccountsFromDatabase() { ... }
  protected void setAccountsToDatabase(Collection<Account> accounts) { ... }
}
```

- 위 코드는 `Bank` 인터페이스에 영속성을 지원하는 예제이다.

- `Bank` 인터페이스에 대해 비즈니스 논리는 `BankImpl` POJO 클래스에 구현된다.
- `BankProxyHandler` 에서는 Java Reflection API를 이용해 메서드를 상응하는 `BankImpl` 메서드로 매핑한다.
  - 그와 동시에 영속성 메서드를 호출하여 영속성 작업도 진행한다.

##### 순수 자바 AOP 프레임워크

- 스프링 AOP와 같은 여러 자바 프레임워크는 내부적으로 프록시를 사용한다.
  - 스프링은 비즈니스 논리를 POJO(Plain Old Java Object)로 구현한다.
  - 어플리케이션 기반 구조는 설정 파일이나 API를 사용해 구현한다.
    - 영속성, 트랜잭션, 보안, 캐시 등과 같은 횡단 관심사가 여기에 속한다.
    - 프레임워크는 사용자가 모르게 프록시나 바이트코드 라이브러리를 사용해 이를 구현한다.
- 즉, 위의 예제에서 `Bank` 도메인 객체는 DAO(Data Access Object) 로 프록시되며, DAO 객체는 JDBC 드라이버 dataSource로 프록시 된다.
  - 따라서 client는 `Bank` 객체에서 `getAccount()` 메서드를 호출한다고 믿지만, 실제로는 `Bank` POJO의 기본 동작을 확장한 중첩 DECORATOR 객체 집합의 가장 외곽과 통신한다.
- EJB3는 XML 설정 파일과 자바 5 annotation 기능을 사용해 횡단 관심사를 선언적으로 지원하는 스프링 모델을 따른다.

```java
@Entity
@Table(name = "BANKS")
public class Bank implements Serializable {
  @Id
  private int id;
  
  @OneToMany(cascade=CascadeType.ALL, fetch=FetchType.EAGER, mappedBy="bank")
  private Collection<Account> accounts = new ArrayList<Account>();
  
  ...
}
```

- 위 코드는 annotation으로 영속성 정보를 나타낸 `Bank` 객체 코드이다.
- 이처럼 프록시를 직접 생성하는 것 보다 스프링에서 제공하는 XML 설정파일 또는 annotation을 활용하여 자동으로 프록시를 생성하는 것이 코드를 더 깔끔하게 만든다.



## 창발성

##### 창발적 설계로 깔끔한 코드를 구현하자

- 다음 네 가지 규칙을 따르면 소프트웨어 설계 품질이 크게 올라간다.
  - 모든 테스트를 실행한다.
  - 중복을 없앤다.
  - 프로그래머 의도를 표현한다.
  - 클래스와 메서드 수를 최소로 줄인다.

##### 모든 테스트를 실행하라

- 설계는 의도한 대로 돌아가는 시스템을 내놓아야 한다.
- 테스트가 가능한 시스템을 만들려고 애쓰면 설계 품질이 더불어 높아진다.
  - 결합도가 높으면 테스트 케이스를 작성하기 어렵다. 그러므로 개발자는 DIP를 적용하고 결합도를 낮춘다.
  - 크기가 작고 목적 하나만 수행하는 클래스가 나온다.

##### 리팩터링

- 테스트 케이스를 모두 작성했다면 코드와 클래스를 정리해도 괜찮다.
- 응집도를 높이고, 결합도를 낮추고, 관심사를 분리하고, 시스템 관심사를 모듈로 나누고, 함수와 클래스 크기를 줄이고, 더 나은 이름을 선택하는 등 다양한 기법을 동원한다.
- 리팩터링 후에는 테스트 케이스를 돌려 기존 기능을 깨뜨리지 않았다는 사실을 확인한다.

##### 중복을 없애라

```java
public void scaleToOneDimension(float desiredDimension, float imageDemension) {
  ...
  RenderedOp newImage = ImageUtilities.getScaledImage(image, scalingFactor, scalingFactor);
  image.dispose();
  System.gc();
  image = newImage;
}

public synchronized void rotate(int degrees) {
  RenderedOp newImage = ImageUtilities.getRotatedImage(image, degrees);
  image.dispose();
  System.gc();
  image = newImage;
}
```

- 위 코드의 두 메서드에는 중복되는 부분이 있다.
- 적은 양이지만 중복되는 코드를 새로운 메서드로 분리할 수 있다.

```java
public void scaleToOneDimension(float desiredDimension, float imageDemension) {
  ...
  replaceImage(ImageUtilities.getScaledImage(image, scalingFactor, scalingFactor));
}

public synchronized void rotate(int degrees) {
  replaceImage(ImageUtilities.getRotatedImage(image, degrees));
}

public void replaceImage(RenderedOp newImage) {
  image.dispose();
  System.gc();
  image = newImage;
}
```

- 메서드를 분리하고 보니 클래스가 SRP를 위반한다.
  - 새로 만든 `replaceImage` 메서드를 다른 클래스로 옮길 수 있다.
  - 그러면 새 메서드를 좀 더 추상화 해 다른 맥락에서 재사용하게 될 수도 있다.



## 동시성

##### 동시성이 필요한 이유?

- 동시성은 coupling을 없애는 전략이다. 즉, 무엇과 언제를 분리하는 전략이다.
  - 무엇과 언제를 분리하면 어플리케이션 구조와 효율이 극적으로 나아진다.
- 단일 스레드 시스템과 다중 스레드 시스템은 설계가 판이하게 다르다.
- 웹 또는 EJB 컨테이너를 사용해도 컨테이너가 실제 어떻게 동작하는지, 어떻게 동시 수정, 데드락 등과 같은 문제를 피할 수 있는지 알아야 한다.

##### 동시성 방어 원칙

###### SRP

- 동시성은 복잡성 하나만으로도 따로 분리할 이유가 충분하다.
- 동시성 관련 코드는 독자적인 개발, 변경, 조율 주기가 있으므로 다른 코드와 분리해야 한다.

###### 자료 범위를 제한하라

- 객체 하나를 공유하는 두 스레드가 동일 필드를 수정하여 서로 간섭하게 되는 결과가 생길 수 있다.
  - 이 문제를 해결하기 위해 공유 객체를 사용하는 코드 내 critical section을 `synchronized` 키워드로 보호한다.
  - critical section을 줄이는 기술이 중요하다.
- 즉, critical section이 늘어나서 보호할 임계영역을 빼먹거나 버그를 찾아내기 어려워지는 상황이 되기보다는 자료를 캡슐화하고 공유 자료를 최대한 줄이는 것이 좋다.

###### 자료 사본을 사용하라

- 공유 자료를 줄이기 위해서 객체를 복사해 읽기 전용으로 사용하는 방법을 쓸 수 있다.
- 또는 각 스레드가 객체를 복사해 사용한 후 한 스레드가 해당 사본에서 결과를 가져오는 방법도 가능하다.
- 객체 복사 비용(시간과 부하)은 공유 객체 내부 lock을 없애 절약한 수행시간으로 커버될 가능성이 높다.

###### 스레드는 가능한 독립적으로 구현하라

- 각 스레드는 클라이언트 요청 하나를 처리하여 다른 스레드와 자료를 공유하지 않는다.
  - 모든 정보는 비공유 출처에서 가져오며 로컬 변수에 저장한다.
- 예를 들어, 각 서블릿은 모든 정보를 `doGet` , `doPost` 매개변수로 받아 마치 자신이 독자적인 시스템에서 동작하는 양 요청을 처리한다.
  - 데이터베이스 연결과 같은 자원을 공유하는 상황은 어쩔 수 없이 동기화 문제에 처하기도 한다.

##### 실행 모델을 이해하라

- `Bound Resource` : 다중 스레드 환경에서 사용하는 자원은 크기나 숫자가 제한적이다. 데이터베이스 연결, 길이가 일정한 read/write 버퍼 등이 그 예이다.
- `Mutual Exclusion` : 한 번에 한 스레드만 공유 자료나 공유 자원을 사용할 수 있다.
- `Starvation` : 스레드가 굉장히 오랫동안 혹은 영원히 자원을 기다린다.
- `Deadlock` : 여러 스레드가 서로가 끝나기를 기다린다. 모든 스레드가 각기 필요한 자원을 기다리며 자원을 점유하는 바람에 어느 쪽도 더이상 진행하지 못한다.
- `Livelock` : lock을 거는 단계에서 스레드가 서로를 방해한다.

###### Producer-Consumer

- 생산자 스레드가 정보를 생성해 버퍼나 대기열에 넣는다. 소비자 스레드가 대기열에서 정보를 가져와 사용한다.
- 여기서 버퍼나 큐는 한정된 자원이다.
  - 생산자 스레드는 대기열에 빈 공간이 있어야 정보를 채우고, 소비자 스레드는 정보가 있어야 가져온다.
- 대기열을 올바로 사용하고자 생산자 스레드와 소비자 스레드는 서로에게 시그널을 보낸다.
  - 생산자 스레드는 대기열에 정보를 채운 다음 소비자 스레드에게 '정보가 있다'는 시그널을 보내고, 소비자 스레드는 대기열에서 정보를 읽어들인 후 '빈 공간이 있다'는 시그널을 보낸다.
  - 잘못하면 생산자 스레드와 소비자 스레드 둘 다 진행 가능함에도 불구하고 동시에 서로에게서 시그널을 기다릴 가능성이 존재한다.

###### Readers-Writers

- 읽기 스레드가 공유 자원에서 정보를 읽고, 쓰기 스레드가 이따금씩 공유 자원을 갱신한다.
- 쓰기 스레드가 버퍼를 오랫동안 점유하게 되면 throughput이 떨어지게 될 수 있으므로 throughput이 문제의 핵심이다.
  - throughput을 강조하면 쓰기 스레드는 기아 상태에 빠질 수 있고, 오래된 정보가 쌓이게 된다.
  - 반면, 쓰기 스레드에게 우선권을 주어 쓰기 스레드가 계속 이어진다면 읽기 스레드가 기아 상태에 빠지고 throughput이 떨어지게 된다.
- 양쪽 균형을 잡으면서 동시 갱신 문제를 피하는 해법이 필요하다.

###### Dining Philosophers

- 여러 프로세스가 자원을 얻으려 경쟁한다.
  - 주의해서 설계하지 않으면 데드락, 라이브락, 처리율 저하, 효율성 저하 등을 겪는다.

##### 동기화하는 메서드 사이에 존재하는 의존성을 이해하라

- 동기화하는 메서드(`synchronized`) 사이에 의존성이 존재하면 동시성 코드에 찾아내기 어려운 버그가 생긴다.
- 공유 클래스 하나에 동기화된 메서드가 하나여야 한다.
  - lock 영역에서 다른 lock 영역을 호출하지 않는다.
- 만약 공유 객체 하나에 여러 메서드가 필요한 경우에 다음 방법을 고려한다.
  - client에서 lock : 클라이언트에서 첫 번째 메서드를 호출하기 전에 서버를 잠근다. 마지막 메서드를 호출할 때까지 잠금을 유지한다.
  - server에서 lock : 서버에 '서버를 잠그고 모든 메서드를 호출한 후 잠금을 해제'하는 메서드를 구현한다. 클라이언트는 이 메서드를 호출한다.
  - Adapted 서버 : 잠금을 수행하는 중간 단계를 생성한다. 원래 서버를 변경하지 않고 서버에서 lock하는 방식이다.

##### 동기화하는 부분을 작게 만들어라

- `synchronized` 키워드를 사용하면 락을 설정하고 해당 영역은 한 번에 한 스레드만 실행이 가능하다.
  - 락은 스레드를 지연시키고 부하를 가중시킨다.
- critical section을 최대한 줄이고 `synchronized` 문을 남발하지 않아야 한다.



## 냄새와 휴리스틱

##### 주석

###### 부적절한 정보

- 다른 시스템(소스 코드 관리 시스템, 버그 추척 시스템, 이슈 추적 시스템 등)에 저장할 정보는 주석으로 적절하지 못하다.
  - 예를 들어, 변경 이력은 소스 코드 관리 시스템을 통해 알 수 있는 정보이기 때문에 넣을 필요가 없다.
- 일반적으로 작성자, 최종 수정일과 같은 메타 정보만 주석으로 넣는다.

###### 쓸모 없는 주석

- 쓸모 없어질 주석은 아예 달지 않는 편이 좋다.
- 쓸모 없어진 주석은 재빨리 삭제하는 편이 가장 좋다.

###### 중복된 주석

- 코드만으로 충분한데 구구절절 설명하는 중복된 주석은 필요하지 않다.

###### 성의 없는 주석

- 주석을 달아야 하는 상황이라면 단어를 신중하게 선택하여 간결하고 명료하게 작성한다.
  - 당연한 소리를 구구절절 반복하지 않는다.

###### 주석 처리된 코드

- 주석으로 처리된 코드는 즉각 지워버린다.
  - 주석으로 처리되어 오래 지난 코드는 더 이상 존재하지 않는 함수를 호출하고 이름이 바뀐 변수를 사용한다.
  - 소스 코드 관리 시스템이 기억하기 때문에 정말 필요하다면 누군가 이전 버전을 가져올 것이다.

##### 환경

###### 여러 단계의 빌드

- 빌드는 간단히 한 단계로 끝나야 한다.

  - 즉, 불가해한 명령이나 스크립트를 잇달아 실행해 각 요소를 빌드할 필요가 없어야 한다.

  - 한 명령으로 전체를 체크아웃해서 한 명령으로 빌드할 수 있어야 한다.

###### 여러 단계의 테스트

- 모든 단위 테스트는 한 명령으로 돌려야 한다.
  - shell에서 명령 하나로 가능해야 한다.
- 모든 테스트를 한 번에 실행하는 능력은 아주 근본적이고 아주 중요하다.

##### 함수

###### 너무 많은 인수

- 함수에서 인수 개수는 작을수록 좋다.
  - 아예 없으면 가장 좋고, 그 다음으로 하나, 둘, 셋이 차례로 좋다.
  - 넷 이상은 그 가치가 아주 의심스러우므로 최대한 피한다.

###### 출력 인수

- 일반적으로 독자는 인수를 입력으로 간주한다.
- 함수에서 뭔가를 변경해야 한다면 출력 인수보다는 함수가 속한 객체의 상태를 변경한다.

###### 플래그 인수

- `boolean` 인수는 함수가 여러 기능을 수행한다는 명백한 증거이므로 피하는 것이 좋다.

###### 죽은 함수

- 아무도 호출하지 않는 함수는 삭제한다.
  - 이 역시 소스 코드 관리 시스템이 기억하기 때문에 걱정할 필요 없다.

##### 일반

###### 한 소스 파일에 여러 언어를 사용한다

- 예를 들어, 어떤 자바 소스 파일은 XML, HTML, Javadoc, Javascript 등을 포함한다.
- 이상적으로는 소스 파일 하나에 언어 하나만 사용하는 방식이 가장 좋다.
  - 현실적으로 불가능하다면 소스 파일에서 언어 수와 범위를 최대한 줄이도록 해야 한다.

###### 당연한 동작을 구현하지 않는다

- 함수나 클래스는 다른 프로그래머가 당연하게 여길 만한 동작과 기능을 제공해야 한다.

```java
Day day = DayDate.stringToDay(String dayName);
```

- 위 함수가 'Monday' 라는 문자열을 `Day.MONDAY` 로 변환해 줄 것을 기대한다.
  - 일반적으로 사용하는 'mon' 같은 약어도 올바로 변환하리라 기대한다.
  - 또한 대소문자는 구분하지 않을 것을 기대한다.
- 당연한 동작을 구현하지 않으면 코드를 읽거나 사용하는 사람이 함수 기능을 직관적으로 예상하기 어렵다.
  - 저자를 신뢰하지 못한다.

###### 경계를 올바로 처리하지 않는다

- 흔히 개발자들은 머릿속에서 코드를 돌려보고 끝낸다.
- 스스로의 직관에 의존하지 말고 모든 경계 조건을 찾아내고, 모든 경계 조건을 테스트하는 테스트 케이스를 작성한다.

###### 안전 절차 무시

- 컴파일러 경고 일부(또는 전부)를 꺼버리면 빌드가 쉬워질지 모르지만 자칫하면 끝없는 디버깅에 시달린다.
- 실패하는 테스트 케이스를 일단 제껴두고 나중으로 미루지 않는다.

###### 중복

- 코드에서 중복을 발견할 때마다 추상화할 기회로 간주한다.
  - 중복된 코드를 하위 루틴이나 다른 클래스로 분리한다.
- 똑같은 코드가 여러 차례 나오는 중복
  - 간단한 함수로 교체한다.
- 여러 모듈에서 일련의 `switch/case` 나 `if/else` 문으로 똑같은 조건을 거듭 확인하는 중복
  - `polymorphism` 으로 대체한다.
- 알고리즘이 유사하나 코드가 서로 다른 중복
  - `TEMPLATE METHOD` 패턴이나 `STRATEGY` 패턴으로 중복을 제거한다.

###### 추상화 수준이 올바르지 못하다

- 추상화는 저차원 상세 개념에서 고차원 일반 개념을 분리한다.
  - 모든 저차원 개념은 파생 클래스에 넣고, 모든 고차원 개념은 기초 클래스에 넣는다.
- 세부 구현과 관련한 상수, 변수, 유틸리티 함수는 기초 클래스에 넣으면 안된다.
  - 기초 클래스는 구현 정보에 무지해야 한다.

```java
public interface Stack {
  Object pop() throws EmptyException;
  void push(Object o) throws FullException;
  double percentFull();
  class EmptyException extends Exception {}
  class FullException extends Exception {}
}
```

- `percentFull` 함수는 추상화 수준이 올바르지 못하다.
  - 어떤 구현은 '꽉 찬 정도'라는 개념을 알아낼 방법이 없을 수 있다.
  - 이 함수는 `BoundedStack` 과 같은 파생 인터페이스에 넣어야 마땅하다.

###### 기초 클래스가 파생 클래스에 의존한다

- 일반적으로 기초 클래스는 파생 클래스를 아예 몰라야 마땅하다.

- 일반적으로 기초 클래스와 파생 클래스를 다른 JAR 파일로 배포하는 편이 좋다.

  - 기초 JAR 파일이 파생 JAR 파일을 전혀 모른다면, 독립적인 개별 컴포넌트 단위로 시스템을 배치할 수 있다.

  - 만약 컴포넌트를 변경해야 한다면 기초 컴포넌트는 다시 배치할 필요 없이 해당 컴포넌트만 다시 배치하면 된다.

###### 과도한 정보

- 잘 정의된 인터페이스는 많은 함수를 제공하지 않는다.
  - 그래서 coupling이 낮다.
- 클래스가 제공하는 메서드 수와 함수가 아는 변수 수는 작을수록 좋다.
  - 클래스에 들어있는 인스턴스 변수 수도 작을수록 좋다.
- 정보를 제한해 결합도를 낮춰야 한다.

###### 죽은 코드

- 실행되지 않는 코드는 시스템에서 제거한다.
  - 불가능한 조건을 확인하는 `if` 문과 `throw` 문이 없는 `try` 문에서 `catch` 블록이 그 예이다.
  - 아무도 호출하지 않는 유틸리티 함수와 `switch/case` 문에서 불가능한 case 조건도 또 다른 예이다.
- 죽은 코드는 설계가 변해도 제대로 수정되지 않기 때문에 죽은 코드를 발견하면 즉시 삭제하도록 한다.

###### 수직 분리

- 변수와 함수는 사용되는 위치에 가깝게 정의한다.
- 지역 변수는 처음으로 사용하기 직전에 선언하며 수직으로 가까운 곳에 위치해야 한다.
- `private` 함수는 처음으로 호출한 직후에 정의한다.

###### 일관성 부족

- 어떤 개념을 특정 방식으로 구현했다면 유사한 개념도 같은 방식으로 구현한다.
  - 최소 놀람의 원칙(`The Principle of Least Surprise`)
- 표기법은 신중하게 선택하며, 일단 선택한 표기법은 신중하게 따른다.

###### 잡동사니

- 아무도 사용하지 않는 변수, 아무도 호출하지 않는 함수, 정보를 제공하지 못하는 주석은 제거한다.

###### 인위적 결합

- 서로 무관한 개념을 인위적으로 결합하지 않는다.
  - 일반적으로 인위적인 결합은 직접적인 상호작용이 없는 두 모듈 사이에서 일어난다.
  - 예를 들어, `enum` 이 클래스에 속한다면 enum을 사용하는 코드가 특정 클래스를 알아야 한다.
- 뚜렷한 목적 없이 변수, 상수, 함수를 당장 편한 위치에 넣어버리지 않는다.

###### 기능 욕심

- 클래스 메서드는 자기 클래스의 변수와 함수에 관심을 가져야지 다른 클래스의 변수와 함수에 관심을 가져서는 안된다.
- 메서드가 다른 객체의 참조자와 변경자를 사용해 그 객체 내용을 조작한다면 메서드가 그 객체 클래스의 범위를 욕심내는 탓이다.

###### 선택자 인수

- 선택자 인수는 목적을 기억하기 어려울 뿐 아니라 각 선택자 인수가 여러 함수를 하나로 조합한다.

```java
public int calculateWeeklyPay(boolean overtime) {
	int tenthRate = getTenthRate();
  int tenthsWorked = getTenthsWorked();
  int straightTime = Math.min(400, tenthsWorked);
  int overTime = Math.max(0, tenthsWorked-straightTime);
  int straightPay = straightTime * tenthRate;
  double overtimeRate = overtiem? 1.5 : 1.0 * tenthRate;
  int overtimePay = (int)Math.round(overTime * overtimeRate);
  return straightPay + overtimePay;
}
```

- 위 코드에서 초과근무 수당을 1.5배로 지급하면 true이고 아니면 false이다.
  - 이는 독자가 호출 시점에 한 눈에 함수의 기능을 이해하기 어렵게 한다.

```java
public int straightPay() {
  return getTenthsWorked() * getTenthRate();
}

public int overtimePay() {
  int overtimeTenths = Math.max(0, getTenthsWorked() - 400);
  int overtimePay = overTimeBonus(overtimeTenths);
  return stratightPay() + overtimePay;
}

public int overTimeBonus(int overtimeTenths) {
  double bouns = 0.5 * getTenthRate() * overtimeTenths;
  return (int) Math.round(bonus);
}
```

- `boolean` 인수 뿐만 아니라 `enum`, `int` 등 함수 동작을 제어하려는 인수는 모두 바람직하지 않다.
  - 인수를 넘겨 동작을 선택하는 것 보다 새로운 함수를 만드는 편이 좋다.

###### 부적절한 static 함수

- `Math.max(...)` 는 좋은 static 메서드다.
  - 메서드가 사용하는 정보는 인수 두개가 전부이고, 메서드를 소유하는 객체에서 가져오는 정보가 아니다.
  - 또한 메서드를 override할 가능성은 전혀 없다.
- 메서드 내에서 사용하는 정보가 인수가 전부이더라도 함부로 `static` 메서드로 선언해서는 안된다.
  - 해당 기능을 수행하는 알고리즘이 여러 개 이거나 함수를 재정의할 가능성이 존재한다면 클래스의 인스턴스 함수로 정의해야 한다.

###### 서술적 변수

- 계산을 여러 단계로 나누고 중간 값으로 서술적인 변수 이름을 사용한다.

```java
Matcher match = headerPattern.matcher(line);
if(match.find()) {
  String key = match.group(1);
  String value = match.group(2);
  headers.put(key.toLowerCase(), value);
}
```

- 위 코드에서 서술적인 변수 이름을 사용한 덕분에 첫 번째로 일치한 그룹이 key 이고, 두 번째로 일치한 그룹이 value 라는 사실이 드러난다.

###### 이름과 기능이 일치하는 함수

```java
Date newDate = date.add(5);
```

- 위 코드에서 `add` 메서드는 5일을 더하는 함수인지, 5주, 5시간을 더하는 함수인지 알 수 없다.
- 또한, `date` 인스턴스를 변경하는 함수인지, `date` 인스턴스는 그대로 두고 새로운 Date를 반환하는 함수인지도 알 수 없다.

- `date` 인스턴스에 5일을 더해 반환하는 함수라면 : `addDaysTo` , `increaseByDays`

- 새로운 Date를 반환하는 함수라면 : `daysLater` , `daysSince`

###### 논리적 의존성은 물리적으로 드러내라

- 한 모듈이 다른 모듈에 의존한다면 물리적인 의존성도 있어야 한다.
  
- 의존하는 모든 정보를 명시적으로 요청한다.
  
- 예를 들어, 근무 시간 보고서를 출력하는 함수를 가지고 있는 `HourlyReporter` 라는 클래스에서 `HourlyReportFormatter` 클래스에 정보를 넘겨 페이지를 출력하도록 한다고 가정한다.

  ```java
  public class HourlyReporter {
    private HourlyReportFormatter formatter;
    private List<LineItem> page;
    private final int PAGE_SIZE = 55;
    
    public void generateReport(List<HourlyEmployee> employees) {
      for(HourlyEmployee e : employees) {
        addLineItemToPage(e);
        if(page.size == PAGE_SIZE) {
          printAndClearItemList();
        }
      }
      if(page.size() > 0) {
        printAndClearItemList();
      }
    }
    
    private void printAndClearItemList() {
      formatter.format(page);
      page.clear();
    }
    
    ...
  } 
  ```

  - `HourlyReporter` 클래스가 페이지 크기를 나타내는 `PAGE_SIZE` 상수를 알고 있다면 이는 `HourlyReporter` 클래스에 책임을 잘못 지운 것이다.
  - `HourlyReportFormatter` 클래스가 이 페이지 크기를 알 것이라고 가정하고 있기 때문에 논리적인 의존성이 존재한다.
  - 이를 물리적인 의존성으로 바꾸기 위해서는 `HourlyReportFormatter` 클래스에 `getMaxPageSize()` 라는 메서드를 추가하고, `HourlyReporter` 클래스는 `PAGE_SIZE` 상수 대신 `getMaxPageSize()` 메서드를 호출하도록 한다.

###### if/else 혹은 switch/case 문보다 다형성을 사용하라

- 같은 선택을 수행하는 다른 코드에서는 다형성 객체를 생성해 `switch` 문을 대신한다.

###### 매직 숫자는 명명된 상수로 교체하라

- 일반적으로 코드에서 숫자를 사용하지 말고, 숫자는 명명된 상수 뒤로 숨긴다.
- 예를 들어, 쪽 당 55줄을 인쇄한다면 숫자 55는 `LINES_PER_PAGE` 상수 뒤로 숨긴다.

###### 조건을 캡슐화하라

- bool 논리는 이해하기 어려우므로 조건의 의도를 분명히 밝히는 함수로 표현한다.

```java
if(shouldBeDelete(timer))
if(timer.hasExpired() && !timer.isRecurrent())
```

- 위 코드가 아래 코드보다 좋다.

###### 부정 조건은 피하라

- 부정 조건은 긍정 조건보다 이해하기 어렵다. 
- 가능하면 긍정 조건으로 표현한다.

###### 함수는 한 가지만 해야 한다

```java
public void pay() {
  for(Employee e : employees) {
    if(e.isPayday()) {
      Money pay = e.calculatePay();
      e.deliverPay(pay);
    }
  }
}
```

- 위 코드에서 `pay` 메서드는 3가지 일을 한다.
  - 직원 목록을 loop로 돌며, 각 직원의 월급일을 확인하고, 해당 직원의 월급을 계산하여 지급한다.

```java
public void pay() {
  for(Employee e : employees) {
    payIfNecessary(e);
  }
}

private void payIfNecessary(Employee e) {
  if(e.isPayday()) {
    calculateAndDeliverPay(e);
  }
}

private void calculateAndDeliverPay(Employee e) {
  Money pay = e.calculatePay();
  e.deliverPay(pay);
}
```

- 위 코드에서는 각 함수는 한 가지 임무만 수행한다.

###### 숨겨진 시각적인 결합

- 때로는 시간적인 결합이 필요하고, 이 시간적인 결합을 숨겨서는 안된다.
- 함수를 짤 때는 함수 인수를 적절히 배치해 함수가 호출되는 순서를 명백히 드러낸다.

```java
public class MoogDiver {
  Gradient gradient;
  List<Spline> splines;
  
  public void dive(String reason) {
    saturateGradient();
    reticulateSplines();
    diveForMoog(reason);
  }
}
```

- 위 코드에서 세 함수가 실행되는 순서가 중요하지만, 위 코드는 시간적인 결합을 강제하지 않는다.

```java
public class MoogDiver {
  Gradient gradient;
  List<Spline> splines;
  
  public void dive(String reason) {
    Gradient gradient = saturateGradient();
    List<Spline> splines = reticulateSplines(gradient);
    diveForMoog(splines, reason);
  }
}
```

- 위 코드는 일종의 연결 소자를 생성해 시간적인 결합을 노출한다.
  - 즉, 각 함수가 내놓는 결과가 다음 함수에 필요하도록 설정하여 시간적 결합을 강제한다.

###### 경계 조건을 캡슐화하라

- 경계 조건은 한 곳에서 별도로 처리한다.
  - 즉, 코드 여기저기에 +1이나 -1을 흩어놓지 않는다.

```java
if(level + 1 < tags.length) {
  parts = new Parse(body, tags, level + 1, offset + endTag);
  body = null;
}
```

- 위 코드에서 `level + 1` 이 두번 나온다.
  - 이런 경계 조건은 변수로 캡슐화하는 것이 좋다.

```java
int nextLevel = level + 1;
if(nextLevel < tags.length) {
  parts = new Parse(body, tags, nextLevel, offset + endTag);
  body = null;
}
```

###### 함수는 추상화 수준을 한 단계만 내려가야 한다

- 함수 내 모든 문장은 추상화 수준이 동일해야 한다.
  - 그 추상화 수준은 함수 이름이 의미하는 작업보다 한 단계만 낮아야 한다.

```java
public String render() throws Exception {
  StringBuffer html = new StringBuffer("<hr");
  if(size > 0) {
    html.append(" size=\"").append(size + 1).append("\"");
  }
  html.append(">");
  return html.toString();
}
```

- 위 코드에서 함수는 추상화 수준이 최소 두개가 섞여 있다.
  - 수평선에 크기가 있다는 개념
  - HR 태그 자체의 문법

```java
public String render() throws Exception {
  HtmlTag hr = new HtmlTag("hr");
  if(extraDashes > 0) {
    hr.addAttribute("size", hrSize(extraDashes));
  }
  return hr.html();
}

private String hrSize(int height) {
  int hrSize = height + 1;
  return String.format("%d", hrSize);
}
```

- 위 코드에서 `render` 함수는 HR 태그의 문법은 전혀 상관없이 HR 태그만 생성한다.
  - HTML 문법은 `HtmlTag` 모듈이 알아서 처리한다.
- 또한 위 코드에서 대시 수에 따라 수평자의 크기를 계산하는 함수(`hrSize`)를 따로 분리하였다.
  -  `render` 함수에서 해당 함수를 호출하여 반환값을 HR 태그 변수에 attribute로 넣어주기만 했다.

###### 설정 정보는 최상위 단계에 둬라

- 추상화 최상위 단계에 둬야 할 기본값 상수나 설정 관련 상수를 저차원 함수에 숨겨서는 안 된다.
  - 대신 고차원 함수에서 저차원 함수를 호출할 때 인수로 넘긴다.

###### 추이적 탐색을 피하라

- 일반적으로 한 모듈은 주변 모듈을 모를수록 좋다. (`Law of Demeter`)
  - 예를 들어, `a.getB().getC().doSomething()` 과 같이 연이어 자신이 아는 모듈을 따라가며 시스템 전체를 휘젓지 않는다.
- 내가 사용하는 모듈이 내게 필요한 서비스를 모두 제공해야 한다.
  - 즉, 원하는 메서드를 찾느라 객체 그래프를 따라 시스템을 탐색할 필요가 없어야 한다.

##### 자바

###### 긴 import 목록을 피하고 와일드카드를 사용하라

- 패키지에서 클래스를 둘 이상 사용한다면 와일드카드를 사용해 패키지 전체를 가져온다.
- 명시적인 import 문은 강한 의존성을 생성하지만 와일드카드는 그렇지 않다.
  - 와일드카드로 패키지를 지정하면 특정 클래스가 존재할 필요가 없기 때문이다.
- 와일드카드 import 문은 때로 이름 충돌이나 모호성을 초래한다.
  - 이름이 같으나 패키지가 다른 클래스는 명시적인 import 문을 사용하거나 코드에서 클래스를 사용할 때 전체 경로를 명시한다.

###### 상수는 상속하지 않는다

```java
public class HourlyEmployee extends Employee {
  private int tenthsWorked;
  private double hourlyRate;
  
  public Money calculatePay() {
    int straightTime = Math.min(tenthsWorked, TENTHS_PER_WEEK);
    int overTime = tenthsWorked - straightTime;
    return new Money(hourlyRate * (tenthsWorked + OVERTIME_RATE * overTime));
  }
}

public abstract class Employee implements PayrollConstants {
  ...
}

public interface PayrollConstants {
  public static final int TENTHS_PER_WEEK = 400;
  public static final double OVERTIME_RATE = 1.5;
}
```

- 위 코드에서 `HourlyEmployee` 클래스에서 사용된 상수 `TENTHS_PER_WEEK` 과 `OVERTIME_RATE` 의 의미를 알기 위해 `PayrollConstants` 인터페이스까지 거슬러 올라가야 한다.

```java
import static PayrollConstants.*;
```

- 대신 `static import` 문을 사용하여 상수를 상속하지 않아도 바로 사용할 수 있도록 한다.

###### 상수 대 Enum

- `public static final int` 는 코드에서 의미를 잃어버릴 수 있다.
  - `enum` 은 이름이 부여된 열거체에 속하기 때문에 그렇지 않다.
  - 또한 메서드와 필드도 사용할 수 있기 때문에 int 보다 훨씬 더 유연하고 서술적인 강력한 도구다.

##### 이름

###### 서술적인 이름을 사용하라

- 이름은 성급하게 정하지 않고 서술적인 이름을 신중하게 고른다.
- 신중하게 선택한 이름은 추가 설명을 포함한 코드보다 강력하다.

###### 가능하다면 표준 명명법을 사용하라

- 예를 들어 DECORATOR 패턴을 활용하는 장식하는 클래스 이름에 Decorator 라는 단어를 사용한다.
- 프로젝트에 유효한 의미가 담긴 이름을 많이 사용할수록 독자가 코드를 이해하기 쉬워진다.

###### 긴 범위는 긴 이름을 사용하라

- 이름 길이는 범위 길이에 비례해야 한다.
  - 범위가 작으면 아주 짧은 이름을 사용해도 괜찮다.

###### 이름으로 부수 효과를 설명하라

- 함수, 변수, 클래스가 하는 일을 모두 기술하는 이름을 사용한다.
  - 실제로 여러 작업을 수행하는 함수에다 동사 하나만 사용하면 곤란하다.

```java
public ObjectOutputStream getOos() throws Exception {
  if(m_oos == null) {
    m_oos = new ObjectOutputStream(m_socket.getOutputStream());
  }
  return m_oos;
}
```

- 위 코드에서 `getOos` 는 함수의 일을 모두 설명해주지 못한다.
  - 함수는 단순히 'oos' 만 가져오지 않고, 기존에 'oos' 가 없으면 생성한다.
- `createOrReturnOos` 라는 이름이 더 적절하다.

