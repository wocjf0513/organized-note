## Mock이란?
가짜 객체
- 실제 객체와 동일한 모의 객체를 만들어 테스트의 효용성을 높인다.

## Mockito란?
mock을 쉽게 만들고 mock의 행동을 정하는 stubbing, 정상적으로 작동하는지에 대한 verify 등 다양한 기능을 제공해주는 프레임워크


### Annotation
* @Mock
@Mock으로 만든 **mock 객체는 가짜 객체이며 그 안에 메소드 호출해서 사용하려면 반드시 스터빙(stubbing)**을 해야합니다.

	mockinject 사용시, mock으로 만든 필드 타입들이 같아서 구별못하는 경우 mock(name=필드네임) 으로 구별해야 된다.

	autowired사용하고 싶다면, mockbean을 통해 mock을 주입할 수 있따.

* @Spy
@Spy로 만든 **mock 객체는 진짜 객체이며 메소드 실행 시 스터빙을 하지 않으면 기존 객체의 로직을 실행한 값을, 스터빙을 한 경우엔 스터빙 값을 리턴**합니다.

```java
@ExtendWith(MockitoExtension.class)
public class SpyAnnotation {
 
    @Spy
    ProductService productService;
 
    @Test
    void testSpy_스터빙X() {
        Product product = productService.getProduct();
 
        assertEquals("A001", product.getSerial());
    }
 
    @Test
    void testSpy_스터빙O() {
        Product productDummy = new Product("B001", "keyboard");
 
        when(productService.getProduct()).thenReturn(productDummy);
 
        Product product = productService.getProduct();
 
        assertEquals(productDummy.getSerial(), product.getSerial());
    }
}
```

* **@InjectMock**
@InjectMock은 DI를 **@Mock이나 @Spy로 생성된 mock 객체를 자동으로 주입**해주는 어노테이션입니다.



### 스터빙(Stubbing) 이란?
테스트 스텁(Test Stub)은 테스트 호출 중 테스트 스텁은 테스트 중에 만들어진 호출에 대해 **미리 준비된 답변**을 제공하는 것

	 Mockito에선 when 메소드를 이용해서 스터빙을 지원하고 있습니다.when에 스터빙할 메소드를 넣고 그 이후에 어떤 동작을 어떻게 제어할지를 메소드 체이닝 형태로 작성하면됩니다.

#### 스터빙 방법
```
when({스터빙할 메소드}).{OngoingStubbing 메소드};
```
![[Pasted image 20231113143429.png]]

```
{Stubber 메소드}.when({스터빙할 클래스}).{스터빙할 메소드}
```

![[Pasted image 20231113143440.png]]


### 검증
```
verify(T mock, VerificationMode mode)
	 verify(userService, atLeastOnce()).getUser();
```
![[Pasted image 20231113143559.png]]


Juit Assert Methods ()

ctrl + shift + t 를 눌러, 기존 작업 클래스에서 test를 만들거나 
```java
import static org.junit.jupiter.api.Assertions.*;
```
추가해야  된다.

### assertThrows()

- 주로 테스트 방법에 의해 던진 이상을 테스트하고 던진 이상이 기대에 부합되는지 테스트한다. -> 내가 의도한 오류가 제대로 던져졌는지를 테스트

### assertDoesNotThrow()

- 주로 테스트 방법에 이상이 던져졌는지, 이상이 던져지면 실패했다고 단언하고, 이상이 없으면 성공했다고 단언하는 데 쓰인다. -> 내가 테스트하는 코드가 오류가 있는지 없는지를 테스트



## 추가
given -when - then 패턴을 사용해 테스트를 하는데

각각 [준비 - 실행 - 검증]
이고 
준비단계에서는 stubbing이
실행에서는 테스트할 함수가
검증에서는 그 결과값에 대한 검증이 이루어진다.

