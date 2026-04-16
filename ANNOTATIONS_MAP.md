# E-Commerce Backend - Annotations Map

## Overview
This document provides a comprehensive mapping of all annotations used throughout the e-commerce backend project. Annotations are organized by category and include their usage context and purpose.

---

## 1. Spring Framework Core Annotations

### 1.1 Application & Configuration

| Annotation | Location | Purpose |
|-----------|----------|---------|
| `@SpringBootApplication` | `EcomBackendApplication.java` | Marks the main Spring Boot application class; enables auto-configuration and component scanning |
| `@Configuration` | `CachingConfig.java`, `KafkaConsumerConfig.java` | Indicates a class contains Spring bean definitions |
| `@EnableCaching` | `CachingConfig.java` | Enables Spring's annotation-driven cache management capability |
| `@EnableKafka` | `KafkaConsumerConfig.java` | Enables `@KafkaListener` support for Kafka message consumption |

### 1.2 Dependency Injection

| Annotation | Location | Purpose |
|-----------|----------|---------|
| `@Autowired` | Controllers, Services | Marks a field/constructor for automatic dependency injection by Spring |
| `@Value` | `CachingConfig.java`, `KafkaConsumerConfig.java` | Injects property values from `application.properties` |

### 1.3 Stereotype Annotations

| Annotation | Location | Purpose |
|-----------|----------|---------|
| `@RestController` | All Controller classes | Marks a class as a REST controller; combines `@Controller` and `@ResponseBody` |
| `@Service` | `AnalyticsService.java`, `ProductService.java`, `OrderService.java`, etc. | Marks a class as a service layer component |
| `@Bean` | `CachingConfig.java`, `KafkaConsumerConfig.java` | Indicates a method produces a Spring-managed bean |

---

## 2. Web/REST Annotations

### 2.1 Request Mapping

| Annotation | Location | Purpose |
|-----------|----------|---------|
| `@RequestMapping` | All Controller classes | Maps HTTP requests to controller class/method |
| `@GetMapping` | All Controller classes | Maps HTTP GET requests to specific endpoints |
| `@PostMapping` | All Controller classes | Maps HTTP POST requests to specific endpoints |
| `@PutMapping` | All Controller classes | Maps HTTP PUT requests to specific endpoints |
| `@DeleteMapping` | `ProductController.java` | Maps HTTP DELETE requests to specific endpoints |

### 2.2 Request Parameters & Body

| Annotation | Location | Purpose |
|-----------|----------|---------|
| `@PathVariable` | All Controller classes | Binds URL path variables to method parameters |
| `@RequestParam` | All Controller classes | Binds query string parameters to method parameters |
| `@RequestBody` | All Controller classes | Binds HTTP request body to method parameters |

---

## 3. Validation Annotations (Jakarta Validation)

### 3.1 Field Validation

| Annotation | Location | Purpose |
|-----------|----------|---------|
| `@NotBlank` | `Product.java`, `Orders.java` | Validates that a string is not null and not empty |
| `@NotNull` | `Product.java`, `Orders.java`, `OrderItem.java` | Validates that a field is not null |
| `@PositiveOrZero` | `Product.java` | Validates that a numeric value is positive or zero |
| `@Min` | `OrderItem.java` | Validates that a numeric value is at least the specified minimum |
| `@Valid` | All Controller classes | Triggers validation of nested objects |

---

## 4. JPA/Persistence Annotations (Jakarta Persistence)

### 4.1 Entity & Table Mapping

| Annotation | Location | Purpose |
|-----------|----------|---------|
| `@Entity` | `Product.java`, `Orders.java`, `OrderItem.java` | Marks a class as a JPA entity |
| `@Table` | `Product.java`, `Orders.java`, `OrderItem.java` | Specifies the database table name for an entity |

### 4.2 Primary Key & ID Generation

| Annotation | Location | Purpose |
|-----------|----------|---------|
| `@Id` | All model classes | Marks a field as the primary key |
| `@GeneratedValue` | All model classes | Specifies how the primary key is generated (IDENTITY strategy) |

### 4.3 Column Mapping

| Annotation | Location | Purpose |
|-----------|----------|---------|
| `@Column` | `Product.java`, `Orders.java` | Specifies column properties (length, nullable, updatable) |

### 4.4 Relationships

| Annotation | Location | Purpose |
|-----------|----------|---------|
| `@OneToMany` | `Orders.java` | Defines a one-to-many relationship with `OrderItem` |
| `@ManyToOne` | `OrderItem.java` | Defines a many-to-one relationship with `Orders` and `Product` |
| `@JoinColumn` | `OrderItem.java` | Specifies the foreign key column name |

### 4.5 Relationship Configuration

| Annotation | Location | Purpose |
|-----------|----------|---------|
| `mappedBy` (attribute) | `Orders.java` | Indicates the owning side of a bidirectional relationship |
| `cascade` (attribute) | `Orders.java` | Specifies cascade operations (ALL) to propagate to related entities |
| `fetch` (attribute) | `Orders.java`, `OrderItem.java` | Specifies fetch strategy (LAZY or EAGER) |
| `orphanRemoval` (attribute) | `Orders.java` | Automatically deletes orphaned entities |

### 4.6 Lifecycle Callbacks

| Annotation | Location | Purpose |
|-----------|----------|---------|
| `@PrePersist` | `Product.java`, `Orders.java` | Executes before a new entity is saved to the database |
| `@PreUpdate` | `Product.java`, `Orders.java` | Executes before an existing entity is updated |

---

## 5. Caching Annotations

| Annotation | Location | Purpose |
|-----------|----------|---------|
| `@Cacheable` | `ProductService.java`, `AnalyticsService.java` | Caches method results; returns cached value on subsequent calls with same parameters |
| `value` (attribute) | Caching methods | Specifies the cache name |
| `key` (attribute) | Caching methods | Specifies the cache key (SpEL expression) |
| `unless` (attribute) | Caching methods | Specifies condition to exclude caching (e.g., `#result == null`) |

### 5.1 Caches Used in Project

| Cache Name | TTL | Usage |
|-----------|-----|-------|
| `productById` | 2 hours | Caches individual product lookups |
| `products` | 1 hour | Caches all products list |
| `categoryProducts` | 3 hours | Caches products by category |
| `popularProducts` | 15 minutes | Caches popular products |
| `eventSummary` | 5 minutes | Caches event summary statistics |
| `topViewedProducts` | 5 minutes | Caches top viewed products |
| `topPurchasedProducts` | 5 minutes | Caches top purchased products |
| `topCategories` | 5 minutes | Caches top categories |
| `trendingProducts` | 5 minutes | Caches trending products |
| `trendingCategories` | 5 minutes | Caches trending categories |
| `productConversionRates` | 5 minutes | Caches product conversion rates |

---

## 6. Elasticsearch Annotations

### 6.1 Document Mapping

| Annotation | Location | Purpose |
|-----------|----------|---------|
| `@org.springframework.data.elasticsearch.annotations.Document` | `UserEvent.java` | Marks a class as an Elasticsearch document |
| `indexName` (attribute) | `UserEvent.java` | Specifies the Elasticsearch index name |

### 6.2 Field Mapping

| Annotation | Location | Purpose |
|-----------|----------|---------|
| `@Field` | `UserEvent.java` | Maps a field to an Elasticsearch field with specific type |
| `@MultiField` | `UserEvent.java` | Defines multiple field mappings for the same property |
| `@InnerField` | `UserEvent.java` | Defines additional field mappings within `@MultiField` |

### 6.3 Field Types Used

| Field Type | Usage | Purpose |
|-----------|-------|---------|
| `FieldType.Text` | `userId`, `sessionId`, `eventType`, `productId`, `category`, `userAgent` | Full-text searchable fields |
| `FieldType.Keyword` | Suffix `.keyword` on text fields | Exact match and aggregation fields |
| `FieldType.Date` | `timestamp` | Date/time fields |
| `FieldType.Object` | `metadata` | Complex object fields |
| `FieldType.Ip` | `ipAddress` | IP address fields |

---

## 7. MongoDB Annotations

| Annotation | Location | Purpose |
|-----------|----------|---------|
| `@org.springframework.data.mongodb.core.mapping.Document` | `UserEvent.java` | Marks a class as a MongoDB document |
| `collection` (attribute) | `UserEvent.java` | Specifies the MongoDB collection name |

### 7.1 MongoDB ID Mapping

| Annotation | Location | Purpose |
|-----------|----------|---------|
| `@Id` (Spring Data) | `UserEvent.java` | Maps to MongoDB's `_id` field |

---

## 8. Spring Data Repository Annotations

### 8.1 Repository Configuration

| Annotation | Location | Purpose |
|-----------|----------|---------|
| `@EnableMongoRepositories` | `EcomBackendApplication.java` | Enables MongoDB repository scanning |
| `@EnableElasticsearchRepositories` | `EcomBackendApplication.java` | Enables Elasticsearch repository scanning |

---

## 9. Kafka Annotations

| Annotation | Location | Purpose |
|-----------|----------|---------|
| `@KafkaListener` | `AnalyticsConsumerService.java` (implied) | Marks a method as a Kafka message listener |

---

## 10. Transactional Annotations

| Annotation | Location | Purpose |
|-----------|----------|---------|
| `@Transactional` | `ProductService.java`, `OrderService.java` | Marks a method as transactional; manages database transactions |

---

## Annotation Usage Summary by File

### Controllers
- **AnalyticsController.java**: `@RestController`, `@RequestMapping`, `@Autowired`, `@GetMapping`, `@RequestParam`
- **OrderController.java**: `@RestController`, `@RequestMapping`, `@Autowired`, `@PostMapping`, `@GetMapping`, `@PutMapping`, `@PathVariable`, `@RequestBody`, `@Valid`
- **ProductController.java**: `@RestController`, `@RequestMapping`, `@Autowired`, `@GetMapping`, `@PostMapping`, `@PutMapping`, `@DeleteMapping`, `@PathVariable`, `@RequestParam`, `@RequestBody`, `@Valid`
- **InventoryController.java**: `@RestController`, `@RequestMapping`, `@Autowired`, `@PostMapping`, `@GetMapping`, `@PathVariable`, `@RequestBody`
- **UserEventController.java**: `@RestController`, `@RequestMapping`, `@Autowired`, `@PostMapping`, `@GetMapping`
- **ProductSearchController.java**: `@RestController`, `@RequestMapping`, `@Autowired`, `@GetMapping`, `@RequestParam`
- **ProductAnalyticsController.java**: `@RestController`, `@RequestMapping`, `@Autowired`, `@GetMapping`

### Services
- **ProductService.java**: `@Service`, `@Autowired`, `@Cacheable`, `@Transactional`
- **AnalyticsService.java**: `@Service`, `@Autowired`, `@Cacheable`
- **OrderService.java**: `@Service`, `@Autowired`, `@Transactional`
- **InventoryService.java**: `@Service`, `@Autowired`, `@Transactional`
- **KafkaProducerService.java**: `@Service`, `@Autowired`
- **AnalyticsConsumerService.java**: `@Service`, `@Autowired`, `@KafkaListener`
- **UserBehaviorService.java**: `@Service`, `@Autowired`
- **ProductSearchService.java**: `@Service`, `@Autowired`

### Models
- **Product.java**: `@Entity`, `@Table`, `@Id`, `@GeneratedValue`, `@NotBlank`, `@NotNull`, `@PositiveOrZero`, `@Column`, `@PrePersist`, `@PreUpdate`
- **Orders.java**: `@Entity`, `@Id`, `@GeneratedValue`, `@NotBlank`, `@NotNull`, `@Enumerated`, `@OneToMany`, `@PrePersist`, `@PreUpdate`
- **OrderItem.java**: `@Entity`, `@Table`, `@Id`, `@GeneratedValue`, `@ManyToOne`, `@JoinColumn`, `@NotNull`, `@Min`
- **UserEvent.java**: `@Document` (MongoDB & Elasticsearch), `@Id`, `@Field`, `@MultiField`, `@InnerField`

### Configuration
- **CachingConfig.java**: `@Configuration`, `@EnableCaching`, `@Bean`, `@Value`
- **KafkaConsumerConfig.java**: `@Configuration`, `@EnableKafka`, `@Bean`, `@Value`
- **JacksonConfig.java**: `@Configuration`, `@Bean`
- **ElasticsearchIndexInitializer.java**: `@Configuration`, `@PostConstruct`

### Application
- **EcomBackendApplication.java**: `@SpringBootApplication`, `@EnableMongoRepositories`, `@EnableElasticsearchRepositories`

---

## Annotation Categories Summary

| Category | Count | Key Annotations |
|----------|-------|-----------------|
| Spring Core | 7 | `@SpringBootApplication`, `@Configuration`, `@EnableCaching`, `@EnableKafka`, `@Autowired`, `@Value`, `@Bean` |
| Web/REST | 8 | `@RestController`, `@RequestMapping`, `@GetMapping`, `@PostMapping`, `@PutMapping`, `@DeleteMapping`, `@PathVariable`, `@RequestParam`, `@RequestBody` |
| Validation | 5 | `@NotBlank`, `@NotNull`, `@PositiveOrZero`, `@Min`, `@Valid` |
| JPA/Persistence | 11 | `@Entity`, `@Table`, `@Id`, `@GeneratedValue`, `@Column`, `@OneToMany`, `@ManyToOne`, `@JoinColumn`, `@PrePersist`, `@PreUpdate`, `@Enumerated` |
| Caching | 3 | `@Cacheable`, `value`, `key` |
| Elasticsearch | 4 | `@Document`, `@Field`, `@MultiField`, `@InnerField` |
| MongoDB | 2 | `@Document`, `collection` |
| Spring Data | 2 | `@EnableMongoRepositories`, `@EnableElasticsearchRepositories` |
| Kafka | 1 | `@KafkaListener` |
| Transactional | 1 | `@Transactional` |

---

## Key Annotation Patterns

### 1. REST Endpoint Pattern
```java
@RestController
@RequestMapping("/api/resource")
public class ResourceController {
    @Autowired
    private ResourceService service;
    
    @GetMapping
    public ResponseEntity<List<Resource>> getAll() { }
    
    @PostMapping
    public ResponseEntity<Resource> create(@Valid @RequestBody Resource resource) { }
}
```

### 2. Service Layer Pattern
```java
@Service
public class ResourceService {
    @Autowired
    private ResourceRepository repository;
    
    @Cacheable(value = "resources")
    public List<Resource> getAll() { }
    
    @Transactional
    public Resource save(Resource resource) { }
}
```

### 3. Entity Pattern
```java
@Entity
@Table(name = "resources")
public class Resource {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    @NotBlank
    private String name;
    
    @PrePersist
    protected void onCreate() { }
}
```

### 4. Configuration Pattern
```java
@Configuration
@EnableCaching
public class CacheConfig {
    @Bean
    public CacheManager cacheManager() { }
}
```

---

## Notes

- **Jakarta Persistence**: The project uses Jakarta Persistence API (formerly javax.persistence) for JPA annotations
- **Spring Data**: Supports multiple data stores (JPA, MongoDB, Elasticsearch) through Spring Data abstractions
- **Caching Strategy**: Redis-based caching with configurable TTLs per cache name
- **Validation**: Uses Jakarta Validation for input validation on DTOs and entities
- **Transactional Management**: Spring manages transactions declaratively via `@Transactional`
- **Kafka Integration**: Configured for asynchronous event processing with error handling and retry logic
