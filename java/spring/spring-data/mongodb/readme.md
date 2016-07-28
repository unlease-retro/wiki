# mongoDB


## Custom Repository

Spring-data mongoDB provide a very nice QueryDSL interface which can be used in most of the cases, however some times when querys gets to complicated, it is more readable and maintainable to use mongoTemplate instead.

In order to have the benefit of both QueryDSL and mongoTemplate, we could extend custom repository on top of MongoRepository.

#### Example

- create a DAO

```java
@Document('example')
public Example {
  @id
  private String id;

  @Indexed
  private Boolean visible;

  @Indexed
  private Date createdAt;

  // .... getter and setters
}
```
- Create custom MongoRepository first
- **The naming is very important, (http://stackoverflow.com/questions/17035419/spring-data-mongodb-custom-implementation-propertyreferenceexception)**

```java
public interface ExampleRepositoryCustom {
  List<Example> myCustomQuery
}

```

- Create your normal MongoRepository

```java

public interface ExampleRepository extends MongoRepository<Example, String>,ExampleRepositoryCustom {
  // ... some out of box DSL querys
  // i.e
  List<Example> findByVisibleIsTrue();
  Example findById(String id);
}

```
- Implement custom MongoRepository

```java
public class ExampleRepositoryCustomImpl implements ExampleRepositoryCustom {
   @Autowired
   MongoTemplate mongoTemplate;

   @Override
   public void someMethod(String id, Boolean visible) {
       Update update = new Update();
       update.set("visible",visible);
       mongoTemplate.updateFirst(
               new Query(Criteria.where("id").is(id)),
               update,
               Example.class
       );
   }
}

```
- use it in some service
```java
@Service
public SomeService implements ISomeService {
  @Autowired
  ExampleRepository exampleRepository;

  /// then you can use all the Standard and custom method under the same injection
  /// exampleRepository.someMethod
  /// exampleRepository.findByVisibleIsTrue
  /// and exampleRepository.findById
}
```
-
