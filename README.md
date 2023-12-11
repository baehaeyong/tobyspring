# tobyspring
토비스프링 정리<br>
컴파일러는 소스 코드를 CPU의 기계어로 변환해주는 프로그램입니다. 일반적으로 하이 레벨 프로그래밍 언어의 소스 코드는 인간이 이해하기 쉽도록 작성되어 있고, 기계어는 컴퓨터가 이해하기 쉬운 형태입니다. 컴파일러는 이 두 가지 형태의 코드를 변환하여 소스 코드를 실행 가능한 형태로 만들어줍니다.

 

컴파일러는 일반적으로 소스 코드를 읽어들여서 문법에 맞는지 검사하고, 코드를 분석하고, 중간 코드를 생성하고, 최종적으로 기계어 코드를 생성합니다. 이 과정을 컴파일링이라고 합니다.

 

컴파일러는 소스 코드를 기계어로 변환하는 과정에서 성능 최적화나 보안 검사 등의 기능을 수행할 수 있습니다. 이러한 기능들을 통해 컴파일러는 프로그램의 실행 속도를 높이거나 보안성을 높일 수 있습니다.

 

컴파일러는 다양한 프로그래밍 언어에 대해 지원됩니다. 예를 들어 C, C++, Java, Python 등의 언어에 대한 컴파일러가 존재합니다.

※ 기계어(인텔 CPU)<br>
![기계어](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FchLUd3%2FbtscGTtVpVL%2FaM94CplOAph8ZkjBeVRNZk%2Fimg.jpg)

|Opcode |정의 |
|-------|----|
|MOV | 데이터를 복사하는 명령어입니다.|
|ADD | 더하기 연산을 수행하는 명령어입니다.|
|SUB |	빼기 연산을 수행하는 명령어입니다. |
|CMP |	비교 연산을 수행하는 명령어입니다.|
|JMP |	분기 연산을 수행하여 프로그램 실행 흐름을 변경하는 명령어입니다.|
|CALL |	서브루틴 호출을 수행하는 명령어입니다.|
|RET |	서브루틴 종료를 나타내는 명령어입니다.|
|PUSH |	스택에 데이터를 삽입하는 명령어입니다.|
|POP |	스택에서 데이터를 추출하는 명령어입니다.|
|AND,OR,XOR |	비트 연산을 수행하는 명령어입니다.|

이외에도 인텔 CPU의 기계어는 다양한 명령어들이 존재하며, CPU 모델에 따라 지원되는 명령어가 다를 수 있습니다.

 

아래 이미지는 컴파일러의 컴파일 단계를 보여 주고 있습니다.

![컴파일단계](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbbNBTn%2FbtscyZ2DKYy%2FcYpUaZAwkUw2WKNSWMYYx0%2Fimg.png)

- 일반적으로 프런트 엔드(front end)는 소스 코드를 분석하여 프로그램의 내부 표현인 중간 표현(Intermediate Representation, IR)을 구축합니다. 또한 심볼 테이블(symbol table)을 관리합니다. 심볼 테이블은 소스 코드의 각 심볼(symbol)을 위치, 유형 및 범위와 같은 관련 정보에 매핑하는 데이터 구조입니다.
- 미들 엔드(middle end), 또는 최적화기(optimizer)는 중간 표현(Intermediate Representation, IR)을 최적화하여 생성된 기계 코드의 성능과 품질을 향상시킵니다. 미들 엔드에는 대상 CPU 아키텍처와 관련이 없는 최적화가 포함됩니다.
- 백 엔드(Back end)는 CPU 아키텍처별 최적화와 코드 생성을 담당합니다.

아래의 소스코드는 C언어로 작성된 코드입니다.
```
if (net > 0.0) 
	total += net * (1.0 + tax / 100.0);
```

다음은 위 코드를 컴파일러의 프런트엔드부가 코드를 파싱하는 과정을 보여줍니다.
![파싱과정](https://blog.kakaocdn.net/dn/xWnFg/btscwooB70P/qOqKnFhoiVtcMd03xQieKk/img.gif)

1. "if(net>0.0)total+=net*(1.0+tax/100.0);" 문자열에서 먼저 lexer (스캐너)가 토큰의 순서를 구성하고, 각각을 식별자, 예약어, 숫자 리터럴 또는 연산자 등의 카테고리로 분류합니다.
2. 이렇게 구성된 토큰 순서는 parser (파서)에 의해 구문 트리로 변환되며, 나머지 컴파일러 단계에서 처리됩니다.

다음 코드는 컴파일러의 프런트엔드가 생성한 중간 코드를 백엔드가 해당 CPU(인텔계열)의 기계어로 변환된 코드를 어셈블리어로 보여줍니다.

```
fld QWORD PTR [net] ; load net into floating point stack
fcomp QWORD PTR [ZERO] ; compare net with 0.0
fstsw ax ; store FPU status word in AX register
sahf ; set flags from FPU status word
ja LABEL1 ; jump if above (i.e., if net > 0.0)

jmp LABEL2 ; jump over the "then" block

LABEL1:
fld QWORD PTR [net] ; load net into floating point stack
fld QWORD PTR [TAX] ; load tax into floating point stack
fdiv QWORD PTR [HUNDRED] ; divide tax by 100.0
fadd QWORD PTR [ONE] ; add 1.0 to the result
fmul ; multiply net by the result
fstp QWORD PTR [TMP1] ; store the result in TMP1
fld QWORD PTR [total] ; load total into floating point stack
fadd QWORD PTR [TMP1] ; add TMP1 to the result
fstp QWORD PTR [total] ; store the result in total

LABEL2:
```
```
DD 05 00 00 00 00 ; net 주소를 스택에 로드
D8 2D 00 00 00 00 ; net와 0.0을 비교
DF E0 ; FPU 상태 워드를 AX 레지스터에 저장
9E ; FPU 상태 워드에서 플래그를 설정
77 08 ; 위의 조건이 참일 경우 LABEL1로 점프

EB 05 ; "then" 블록을 건너뛰기 위해 점프

LABEL1:
DD 05 00 00 00 00 ; net 주소를 스택에 로드
DD 15 00 00 00 00 ; TAX 주소를 스택에 로드
DC 25 00 00 00 00 ; 100.0으로 tax를 나눔
D8 05 00 00 00 00 ; 1.0을 더함
DE C9 ; net에 결과를 곱함
DD 1D 00 00 00 00 ; total 주소를 스택에 로드
D8 05 00 00 00 00 ; 결과에 total을 더함

LABEL2:
```

일반적인 컴파일러의 백엔드는 기계어를 생성하는 것이 일반적이지만, 자바의 경우에는 바이트코드를 생성합니다.

기계어는 특정한 프로세서 아키텍처에서 직접 실행할 수 있는 형식의 코드입니다. 이에 비해, 바이트코드는 자바 가상 머신에서 실행 가능한 중간 코드입니다. 자바 가상 머신은 바이트코드를 인터프리팅(interpreting)하거나 JIT(just-in-time) 컴파일러를 통해 기계어로 변환하여 실행합니다.

바이트코드는 자바의 플랫폼 독립성(platform independence)을 제공하는 데에 큰 역할을 합니다. 바이트코드는 자바 가상 머신에서 실행되기 때문에, 어떤 운영체제나 CPU 아키텍처에서도 동일한 실행 결과를 보장합니다.

또한, 바이트코드는 자바 애플리케이션의 보안성을 높이는 데에도 사용됩니다. 바이트코드는 바이트 단위로 작성되기 때문에, 기계어보다는 해석하기 어렵습니다. 이를 통해, 바이트코드를 디컴파일(decompile)하여 원본 소스 코드를 추출하는 것이 어렵습니다.

따라서, 자바는 기계어 대신 바이트코드를 생성하는 특징을 가지고 있으며, 이를 통해 플랫폼 독립성과 보안성을 보장합니다.

 

자바의 바이트코드는 컴파일러의 프런트엔드(frontend)가 컴파일 시 생성하는 중간 코드와 비슷합니다.

자바 컴파일러는 소스 코드를 바이트코드로 번역합니다. 이 때, 컴파일러는 소스 코드를 파싱하고, 추상 구문 트리(abstract syntax tree)를 생성합니다. 추상 구문 트리는 컴파일러가 소스 코드를 이해하기 쉬운 구조로 변환한 것입니다.

이후, 추상 구문 트리를 바이트코드로 번역하는 과정에서, 컴파일러는 중간 코드를 생성합니다. 이 중간 코드는 자바 가상 머신이 이해할 수 있는 형식으로, 메모리 상에 존재합니다.
따라서, 자바의 바이트코드는 컴파일러의 프런트엔드가 생성하는 중간 코드와 비슷합니다. 바이트코드는 추상적이고 이해하기 어렵지만, 자바 가상 머신이 이해할 수 있는 형식으로, 컴파일러에 의해 생성됩니다.

※컴파일러의 중간코드는 여러 가지 이유로 필요합니다.<br>
1. 첫째, 중간코드는 컴파일러의 최적화 기능을 구현하는 데에 사용됩니다. 컴파일러는 소스 코드를 바로 기계어로 번역하기보다는, 중간코드로 번역하는 것이 일반적입니다. 이는 컴파일러가 소스 코드를 보다 효율적으로 분석하고 최적화할 수 있도록 하기 때문입니다.
2. 둘째, 중간코드는 다양한 하드웨어와 운영체제에서 실행 가능한 코드를 생성하기 위해 사용됩니다. 컴파일러는 중간코드를 이용하여, 다양한 플랫폼에서 동작하는 실행 파일을 생성할 수 있습니다.
3. 셋째, 중간코드는 다른 프로그램과의 상호운용성(interoperability)을 위해 사용됩니다. 예를 들어, 자바는 자바 가상 머신에서 실행되는 바이트코드를 사용하기 때문에, 다른 언어로 작성된 코드를 자바에서 실행하기 위해서는 해당 코드를 자바 바이트코드로 번역해야 합니다.
4. 넷째, 중간코드는 코드 분석, 디버깅, 코드 변환 등에 사용됩니다. 컴파일러가 소스 코드를 중간코드로 번역하면, 이 중간코드를 분석하거나 변환하여 원하는 코드를 생성할 수 있습니다. 또한, 중간코드는 디버깅 과정에서 사용되기도 합니다.

따라서, 컴파일러의 중간코드는 컴파일러의 최적화, 하드웨어 및 운영체제 호환성, 상호운용성, 코드 분석과 변환, 디버깅 등 다양한 분야에서 사용됩니다.

---

# Reflection
자바에서 리플렉션(Reflection)이란, 실행 중인 자바 프로그램에서 클래스의 정보를 가져오고, 이를 통해 객체를 생성하거나 메서드를 호출하는 등의 동적인 작업을 수행하는 API입니다. 즉, 프로그램이 실행 중일 때도 클래스의 정보를 얻을 수 있으며, 이를 이용해 객체를 생성하고 메서드를 호출할 수 있습니다.

리플렉션은 다음과 같은 기능을 제공합니다.
1. 클래스의 정보 가져오기: 리플렉션은 실행 중인 클래스의 정보(패키지, 클래스명, 상위 클래스, 인터페이스, 생성자, 메서드, 필드 등)를 가져올 수 있습니다.
2. 객체 생성하기: 리플렉션은 클래스의 정보를 바탕으로 객체를 동적으로 생성할 수 있습니다. 즉, 클래스명을 문자열로 입력받아 객체를 생성할 수 있습니다.
3. 메서드 호출하기: 리플렉션은 실행 중인 클래스의 메서드를 호출할 수 있습니다. 메서드명과 매개변수를 지정하여 메서드를 호출할 수 있습니다.
4. 필드 값 접근하기 리플렉션은 실행 중인 클래스의 필드 값을 읽거나 쓸 수 있습니다. 필드명을 지정하여 필드의 값을 읽거나 쓸 수 있습니다.

리플렉션은 객체지향 프로그래밍의 캡슐화(encapsulation) 원칙을 위반하는 경우가 있으므로, 사용에 주의가 필요합니다. 또한, 리플렉션은 성능상의 이슈가 있으므로, 사용 시 성능에 영향을 미칠 수 있습니다. 따라서, 리플렉션을 사용할 때는 적절한 사용처를 고민하고, 성능을 고려하여 사용해야 합니다.

리플렉션을 사용한 코드의 예시입니다.

1. 클래스의 정보 가져오기
```
Class<?> clazz = Class.forName("com.example.MyClass");
String className = clazz.getName(); // 클래스명
Package pkg = clazz.getPackage(); // 패키지 정보
Class<?> superClass = clazz.getSuperclass(); // 상위 클래스 정보
Class<?>[] interfaces = clazz.getInterfaces(); // 인터페이스 정보
Constructor<?>[] constructors = clazz.getConstructors(); // 생성자 정보
Method[] methods = clazz.getDeclaredMethods(); // 메서드 정보
Field[] fields = clazz.getDeclaredFields(); // 필드 정보
```
※ Class.forName(String className) 메서드는 주어진 클래스 이름(className)에 해당하는 클래스를 JVM에서 찾아 로드합니다. 클래스 로드는 JVM 내부의 클래스 로더(class loader)가 수행하며, 클래스 로더는 클래스 파일을 읽어들여 JVM의 메모리에 올립니다.

따라서, Class.forName() 메서드는 로드되지 않은 클래스 파일을 로드할 때 사용됩니다. 예를 들어, 클래스 파일이 로드되지 않은 상태에서 해당 클래스를 사용해야 하는 경우, Class.forName() 메서드를 사용하여 클래스 파일을 로드할 수 있습니다.

그러나, Class.forName() 메서드는 클래스 로딩을 수행할 때 클래스패스(classpath)에서 클래스를 찾습니다. 클래스패스는 JVM에서 클래스를 찾을 때 사용하는 경로를 말하며, 이 경로에 해당하는 디렉토리나 JAR 파일에서 클래스를 찾습니다.

따라서, Class.forName() 메서드를 사용하여 클래스를 로드할 때는 해당 클래스가 클래스패스에 존재해야 합니다. 클래스패스에 존재하지 않는 클래스를 로드하려면, 별도의 클래스 로더를 구현하여 사용해야 합니다.

로드되어 있는 클래스는 Class.forName(String className) 대신에 Class<T>.class를 사용하여 가져올 수 있습니다. 이 방법은 컴파일 시점에 클래스가 이미 로드되어 있을 때 사용할 수 있습니다.

예를 들어, java.lang.String 클래스를 로드하는 경우, 다음과 같이 String.class를 사용하여 가져올 수 있습니다.
```
Class<?> clazz = String.class;
```
이 방법은 Class.forName() 메서드를 사용하는 것보다 더욱 간편하고 안전합니다. Class.forName() 메서드를 사용하는 경우, 클래스 이름을 문자열로 전달해야 하므로 오타 등의 문제가 발생할 가능성이 있습니다. 반면에 Class<T>.class를 사용하는 경우, 컴파일러가 클래스 이름을 검사하여 오타 등의 문제를 사전에 방지할 수 있습니다.

특정 클래스가 로드되어 있는지 아닌지를 확인하기 위해서는 Class.forName(String className) 대신 ClassLoader의 loadClass(String className) 메서드를 사용할 수 있습니다. 이 메서드는 해당 클래스가 이미 로드되어 있는 경우, 로드된 클래스를 반환합니다. 만약 해당 클래스가 아직 로드되어 있지 않은 경우, 클래스 로더는 클래스를 로드한 후 반환합니다.

예를 들어, com.example.MyClass 클래스가 이미 로드되어 있는지 확인하는 경우, 다음과 같이 ClassLoader의 loadClass() 메서드를 사용할 수 있습니다.
```
try {
    ClassLoader classLoader = getClass().getClassLoader(); // 클래스 로더 가져오기
    Class<?> clazz = classLoader.loadClass("com.example.MyClass"); // 클래스 로드
    // 클래스가 로드되어 있으면 clazz에 로드된 클래스가 할당됩니다.
} catch (ClassNotFoundException e) {
    // 클래스가 로드되어 있지 않으면 ClassNotFoundException 예외가 발생합니다.
}
```
위 코드에서 getClass().getClassLoader()는 현재 클래스의 클래스 로더를 가져오는 코드입니다. 이 코드에서 가져온 클래스 로더는 클래스가 로드되어 있는지 확인하는 데 사용됩니다.

loadClass() 메서드는 클래스를 찾을 수 없는 경우 ClassNotFoundException 예외가 발생합니다. 따라서, try-catch 문을 사용하여 예외 처리를 해주어야 합니다.

2. 객체 생성하기
```
Class<?> clazz = Class.forName("com.example.MyClass");
Constructor<?> constructor = clazz.getConstructor(); // 기본 생성자
Object obj = constructor.newInstance(); // 객체 생성
```
※newInstance() 메서드는 해당 클래스의 디폴트 생성자(default constructor)를 호출하여 객체를 생성합니다. 디폴트 생성자는 매개변수를 가지지 않는(public) 생성자를 말합니다.

만약 클래스에 디폴트 생성자가 없는 경우, newInstance() 메서드는 InstantiationException 예외를 발생시킵니다. 이 경우에는 다른 생성자를 사용하여 객체를 생성해야 합니다.

3. 메서드 호출하기
```
Class<?> clazz = Class.forName("com.example.MyClass");
Object obj = clazz.newInstance();
Method method = clazz.getDeclaredMethod("myMethod", String.class);
method.setAccessible(true);
Object result = method.invoke(obj, "hello"); // 메서드 호출
```
Method를 이용해 메소드를 호출하는 방법을 익히기 위해서 다음과 같은 간단한 학습 테스트를 만들수 있습니다
```
package com.kitec.learningtest.jdk;

import static org.junit.jupiter.api.Assertions.assertEquals;
import java.lang.reflect.Method;
import java.util.Date;
import org.junit.jupiter.api.Test;

public class Reflection {
	@Test
	public void invokeMethod() throws Exception {
		String name = "Spring";

		// length
		assertEquals(name.length(), 6);
		
		Method lengthMethod = String.class.getMethod("length");
		assertEquals((Integer)lengthMethod.invoke(name), 6);
		
		// toUpperCase
		assertEquals(name.charAt(0), 'S');
		
		Method charAtMethod = String.class.getMethod("charAt", int.class);
		assertEquals((Character)charAtMethod.invoke(name, 0), 'S');
	}
	
	@Test 
	public void createObject() throws Exception {
		Date now = java.util.Date.class.getDeclaredConstructor().newInstance();
	}
}
```
String 클래스의 length() 메소드롸 charAt() 메소드를 코드에서 직접 호출하는 방법과, Method를 이용해 리플렉션 방식으로 호출하는 방법을 비교한 것입니다.<br>
4. 필드 값 접근하기
```
Class<?> clazz = Class.forName("com.example.MyClass");
Object obj = clazz.newInstance();
Field field = clazz.getDeclaredField("myField");
field.setAccessible(true);
Object value = field.get(obj); // 필드 값 읽기
field.set(obj, "new value"); // 필드 값 쓰기
```
위 코드에서 com.example.MyClass는 예시를 위한 클래스명으로 대체 가능합니다. 또한, 리플렉션을 사용하는 경우 ClassNotFoundException, InstantiationException, IllegalAccessException, NoSuchMethodException, NoSuchFieldException, InvocationTargetException 등 다양한 예외 처리가 필요합니다.
