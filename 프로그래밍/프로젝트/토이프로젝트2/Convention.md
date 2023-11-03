## 디렉토리 구조

<aside> ⚠️ 디렉토리 안에 `.gitkeep` 파일 외에 **다른 파일이 있다면** `.gitkeep`은 지우셔도 됩니다.

</aside>

```
Project
├── HELP.md
├── build.gradle
├── gradle
│   └── wrapper
│       ├── gradle-wrapper.jar
│       └── gradle-wrapper.properties
├── gradlew
├── gradlew.bat
├── readme.md
├── settings.gradle
└── src
    ├── main
    │   ├── java
    │   │   └── com
    │   │       └── kdt_y_be_toy_project2
    │   │           ├── KdtYBeToyProject2Application.java
    │   │           ├── domain
    │   │           │   ├── itinerary
    │   │           │   │   ├── controller
    │   │           │   │   ├── domain
    │   │           │   │   ├── dto
    │   │           │   │   ├── exception
    │   │           │   │   ├── repository
    │   │           │   │   └── service
    │   │           │   ├── model
    │   │           │   │   ├── PlaceInfo.java
    │   │           │   │   └── TimeScheduleInfo.java
    │   │           │   └── trip
    │   │           │       ├── controller
    │   │           │       ├── domain
    │   │           │       ├── dto
    │   │           │       ├── exception
    │   │           │       ├── repository
    │   │           │       └── service
    │   │           └── global
    │   │               ├── config
    │   │               └── error
    │   └── resources
    │       ├── application.properties
    │       ├── static
    │       └── templates
    └── test
        └── java
            └── com
                └── kdt_y_be_toy_project2
                    └── KdtYBeToyProject2ApplicationTests.java
```

## 컨트롤러

```java
@RestController
@RequestMapping("/v1")
@RequiredArgsConstructor
public class ExampleApi {
    private final ExampleRepository exampleRepository;
    private final ExampleService exampleService;

    @GetMapping("/example/{example_id}")
    public ResponseEntity<GetResponse> getExample(
         @PathVariable(name="example_id") final Long id
    ) {
      return ResponseEntity.status(HttpStatus.OK)
						 .contentType(MediaType.APPLICATION_JSON)
		         .body(exampleService.getExample(id));
    }

    @PostMapping("/example")
    public ResponseEntity<AddResponse> addExample(
         @RequestBody final ExampleDTORequest request
    ) {
      return ResponseEntity.status(HttpStatus.OK)
						 .contentType(MediaType.APPLICATION_JSON)
		         .body(exampleService.addExamples(ExampleDTO.of(request)));
    }
}
```

## 서비스

```java
@Service
@Transactional
@RequiredArgsConstructor
public class ExampleService {

    private final ExampleRepository exampleRepository;

    @Transactional(readOnly = true)
    public GetResponse getExample(final long exampleId) {
        final ExampleEntity exampleEntity = exampleRepository.findById(memberId);
	      return GetResponse.from(exampleEntity);
    }

    public AddResponse add(final long memberId, final MemberProfileUpdate dto) {
        final ExampleEntity exampleEntity = exampleRepository.save(memberId);
        return AddResponse.from(exampleEntity);
    }
}
```

- 클래스 어노테이션에 `Transactional`을 붙인다.

## RequestDTO

```java
@Builder
public record ExampleRequest (
  @NotNull int a,
  @NotNull String b
) {
    public ExampleEntity to(final ExampleResponse response) {
        return ExampleEntity.builder()
                .a(response.a)
                .b(response.b)
                .build();
    }
}
```

## ResponseDTO

```java
@Builder
public record ExampleResponse (
  int a,
  String b
) {
    public ExampleResponse from(final ExampleEntity entity) {
        return ExampleResponse.builder()
                .a(a)
                .b(b)
                .build();
    }
}
```

## Entity

```java
@Entity
@Table(name = "example")
@NoArgsConstructor(access = AccessLevel.PROTECTED)
@Getter
public class Example {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    @Column(name = "id", updatable = false)
    private Long id;

    @Embedded
    @AttributeOverride(name = "value", column = @Column(name = "email", nullable = false, unique = true, updatable = false, length = 50))
    private Email email;

    @Embedded
    @AttributeOverrides({
            @AttributeOverride(name = "first", column = @Column(name = "first_name", nullable = false)),
            @AttributeOverride(name = "middle", column = @Column(name = "middle_name")),
            @AttributeOverride(name = "last", column = @Column(name = "last_name", nullable = false))
    })
    private Name name;

		@Column(name = "used", nullable = false)
    private Boolean used;

    @Column(name = "discount", nullable = false)
    private Double discount;

    @Builder
    private Example(Email email, Name name, boolean used, double discount) {
        this.email = email;
        this.name = name;
				this.used = used;
				this.discount = discount;
    }

		// 상황에 따라 적절하게 메소드 만들어서 사용
    public void updateProfile(final Name name) {
        this.name = name;
    }

		public void verifyUsed() {
        if (used) throw new CouponAlreadyUseException();
    }

	 public Example to(final ExampleDTO exampleDTO) {
        return Example.builder()
                .a(a)
                .b(b)
                .build();
    }

}
```

## 에러처리

### Exception

- 공통 Exception 코드

```java
@Getter
public class ExampleException extends RuntimeException {

  private final ErrorCode errorcode;
  private final String message;

  public ExampleException(ErrorCode errorcode, String message) {
    super(errorcode.getValue());
    this.errorcode = errorcode;
    this.message = message;
  }

  public ExampleException(ErrorCode errorcode) {
    super(errorcode.getValue());
    this.errorcode = errorcode;
    this.message = errorcode.getValue();
  }
}
```

- Exception 사용 코드

```java
public class ExampleErrorException extends ExampleException {

    public ExampleErrorException(String value) {
        super(value, ErrorCode.EXAMPLE_ERROR_CODE);
    }

    public ExampleErrorException(String value, ErrorCode errorCode) {
        super(value, errorCode);
    }
}
```

### ErrorCode

```java
@JsonFormat(shape = JsonFormat.Shape.OBJECT)
public enum ErrorCode {

    // Common
    INVALID_INPUT_VALUE(400, "C001", " Invalid Input Value"),
    METHOD_NOT_ALLOWED(405, "C002", " Invalid Input Value"),
    ENTITY_NOT_FOUND(400, "C003", " Entity Not Found"),
    INTERNAL_SERVER_ERROR(500, "C004", "Server Error"),
    INVALID_TYPE_VALUE(400, "C005", " Invalid Type Value"),
    HANDLE_ACCESS_DENIED(403, "C006", "Access is Denied"),

    // Member
    EMAIL_DUPLICATION(400, "M001", "Email is Duplication"),
    LOGIN_INPUT_INVALID(400, "M002", "Login input is invalid"),

    // Coupon
    COUPON_ALREADY_USE(400, "CO001", "Coupon was already used"),
    COUPON_EXPIRE(400, "CO002", "Coupon was already expired");

    private final String code;
    private final String message;
    private int status;

    ErrorCode(final int status, final String code, final String message) {
        this.status = status;
        this.message = message;
        this.code = code;
    }

    public String getMessage() {
        return this.message;
    }

    public String getCode() {
        return code;
    }

    public int getStatus() {
        return status;
    }

}
```