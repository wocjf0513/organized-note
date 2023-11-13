SpringBoot 테스트는 **@DataJpaTest** Annottation을 제공하는데, 이것을 통해 Repository의 단위 테스트가 가능하다.

@DataJpaTest을 사용할경우 아래와 같은 기능이 수행된다.

- JPA 관련된 설정만 로드한다. (WebMVC와 관련된 Bean이나 기능은 로드되지 않는다)
- JPA를 사용해서 생성/조회/수정/삭제 기능의 테스트가 가능하다.
- @Transactional을 기본적으로 내장하고 있으므로, 매 테스트 코드가 종료되면 자동으로 DB가 롤백된다.
- 기본적으로 내장 DB를 사용하는데, 설정을 통해 실제 DB로 테스트도 가능하다. (권장하지 않는다)
- @Entity가 선언된 클래스를 스캔하여 저장소를 구성한다.

```java
@DataJpaTest  
class ItineraryRepositoryTest {  
  
    @Autowired  
    private TripRepository tripRepository;  
    @Autowired  
    private ItineraryRepository itineraryRepository;
```

---

Service 단위 테스트는

Repository를 mock 가짜 객체로 만든 후 이를 Service에 주입해서 테스트를 진행할 수 있다.

```java

@ExtendWith(MockitoExtension.class)  
class ItineraryServiceTest {  
  
    @Mock  
    private ItineraryRepository itineraryRepository;  
  
    @InjectMocks  
    private ItineraryService itineraryService;
    
```

---

Controller 단위 테스트는

@SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT)  
@Transactional  
@AutoConfigureMockMvc


SpringBootTest를 통해 bean들을 등록하고, 
AutoConfigureMockMvc를 통해 Mockmvc에 대한 자동 설정을 하고 이용할 수 있다.

```java
@SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT)  
@Transactional  
@AutoConfigureMockMvc  
@ActiveProfiles("test")  
class ItineraryControllerTest {  
  
    @Autowired  
    private MockMvc mockMvc;  
  
    @Autowired  
    private ObjectMapper objectMapper;  
  
    @Autowired  
    private ItineraryService itineraryService;  
  
    @Autowired  
    private TripRepository tripRepository;  
  
    @Autowired  
    private ItineraryRepository itineraryRepository;
```
