---
title: Spring Boot Postgresql Crud App
date: 2024-12-30 13:14 +0300
categories: [Spring Boot Postgresql Crud App]
tags: [spring-boot, react, java, postgresql, material-ui, web-development, api, rest-api, deployment, frontend, backend, scalable, user-interface, full-stack-development, java-backend, crud, basic-app, todo, to-do app]
author: selin-topcu
lang: en
---

# Spring Boot PostgreSQL CRUD Application

## Let's Create the Backend with Spring Initializr

- Let's create the application with [Spring Initializr](https://start.spring.io).
  - Project: Maven
  - Language: Java
  - Spring Boot: 3.4.1
  - Java: 21
  - Dependencies
    - Spring Web
    - PostgreSQL Driver
    - Spring Data JPA

Note: Why we didn't use Lombok is explained after the 20th minute in this [podcast](https://open.spotify.com/episode/2SKXhtsUsIqON4AafgxavK?si=qvd4wA1sTHywm6RQYjPYyQ&nd=1&dlsi=321b8f07b0324cfd), you can check it in detail.

Let's download the zip file. Extract the zip and open the project in IntelliJ IDEA.

### IntelliJ IDEA

#### IntelliJ IDEA Installation

- You can download either IntelliJ IDEA Ultimate or the free Community edition [here](https://www.jetbrains.com/idea/download/other.html).

#### Project Configuration

* In **IntelliJ IDEA**, let's perform the following checks.
  * File > Project Structure SDK: 21 should be selected.
    * If version 21 is not installed, download it from [here](https://www.oracle.com/java/technologies/downloads/). Download and install from the Installer section.
    * Check the Java version from CMD.
      * java -version
  * If annotations are not found in the project, and it gives the error "Cannot resolve symbol," let's do the following:
    * File > Invalidate Caches, select all the options and click the Invalidate and Restart button.

### Let's Create the Package Structure

Add the following files in the same directory as `CrudAppApplication.java`:


```
crud_app/
├── controller/
│   └── ProductController.java
├── entity/
│   └── Product.java
├── repository/
│   └── ProductRepository.java
├── service/
│   └── ProductService.java
└── CrudAppApplicationMyProjectApplication.java
```

### Layered Architecture

#### Controller Layer

Manages communication between the user and the application. API endpoints are defined here.

- `ProductController.java` handles requests related to products and triggers the necessary actions.

#### DTO Layer

Contains classes used for data transfer. Request and response objects are defined here.

- `ProductRequestDto.java` carries the data for creating or updating a product.
- `ProductResponseDto.java` holds the converted product data to be returned.

#### Entity Layer

Defines the core data structure of the application. It consists of entity classes representing database tables.

- `Product.java` contains the structure of product data and necessary annotations for database interaction.

#### Repository Layer

Interacts with the database. CRUD operations and custom queries are executed here.

- `ProductRepository.java` provides access to product data using Spring Data JPA.

#### Service Layer

Applies business logic and performs operations. It acts as an intermediary between the Controller and Repository layers.

- `ProductService.java` manages product-related operations such as adding, deleting, or updating products.

#### CrudAppApplicationMyProjectApplication.java

This is the entry point of the application. It starts the Spring Boot application.

### Product.java

```java
@Entity
@Table(name = "product")
public class Product {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    private String name;
    private String description;
    private String brand;
    private Double price;
    private LocalDateTime createdAt;
    private LocalDateTime updatedAt;

    public Product() {
    }

    public Product(Long id, String name, String description, String brand, Double price, LocalDateTime createdAt, LocalDateTime updatedAt) {
        this.id = id;
        this.name = name;
        this.description = description;
        this.brand = brand;
        this.price = price;
        this.createdAt = createdAt;
        this.updatedAt = updatedAt;
    }

    public Long getId() {
        return id;
    }

    public void setId(Long id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getDescription() {
        return description;
    }

    public void setDescription(String description) {
        this.description = description;
    }

    public String getBrand() {
        return brand;
    }

    public void setBrand(String brand) {
        this.brand = brand;
    }

    public Double getPrice() {
        return price;
    }

    public void setPrice(Double price) {
        this.price = price;
    }

    public LocalDateTime getCreatedAt() {
        return createdAt;
    }

    public void setCreatedAt(LocalDateTime createdAt) {
        this.createdAt = createdAt;
    }

    public LocalDateTime getUpdatedAt() {
        return updatedAt;
    }

    public void setUpdatedAt(LocalDateTime updatedAt) {
        this.updatedAt = updatedAt;
    }
}
```

### ProductRepository.java

```java
@Repository
public interface ProductRepository extends JpaRepository<Product, Long> {

}
```

### ProductService.java

```java
@Service
public class ProductService {
    private final ProductRepository productRepository;

    public ProductService(ProductRepository productRepository) {
        this.productRepository = productRepository;
    }

    public List<Product> getProducts() {
        return productRepository.findAll();
    }

    public Product getProduct(Long id) {
        return productRepository.findById(id).orElse(null);
    }

    public Product addProduct(Product product) {
        return productRepository.save(product);
    }

    public Product updateProduct(Product product) {
        return productRepository.save(product);
    }

    public void deleteProduct(Long id) {
        productRepository.deleteById(id);
    }
}
```

### ProductController.java

```java
@RestController
@RequestMapping("/api")
public class ProductController {

    private final ProductService productService;

    public ProductController(ProductService productService) {
        this.productService = productService;
    }

    @GetMapping("/get-products")
    public List<Product> getProducts() {
        return productService.getProducts();
    }

    @GetMapping("/get-product")
    public Product getProduct(@RequestParam("id") Long id) {
        return productService.getProduct(id);
    }

    @PutMapping("/update-product")
    public Product updateProduct(@RequestBody Product product, @RequestParam("id") Long id) {
        return productService.updateProduct(product);
    }

    @PostMapping("/save-product")
    public ResponseEntity<Product> saveProduct(@RequestBody Product product) {
        Product newProduct = productService.saveProduct(product);
        return ResponseEntity.status(HttpStatus.CREATED).body(newProduct);
    }

    @DeleteMapping("/delete-product")
    public void deleteProduct(@RequestParam("id") Long id) {
        productService.deleteProduct(id);
    }
}
```

### Pom.xml

For Swagger UI, let's add the following [OpenAPI](https://www.baeldung.com/spring-rest-openapi-documentation) dependecy.

```xml
<dependency>
	<groupId>org.springdoc</groupId>
	<artifactId>springdoc-openapi-starter-webmvc-ui</artifactId>
	<version>2.8.0</version>
</dependency>
<dependency>
	<groupId>org.springdoc</groupId>
	<artifactId>springdoc-openapi-starter-webmvc-api</artifactId>
	<version>2.8.0</version>
</dependency>
```

### Postgresql

#### Postgresql Installation

- [Here](https://www.postgresql.org/download) Let's go to the Postgresql download site. Let's select the operating system.
- For Windows;
- Click on "Windows".
- Click on "Download the installer". Let's download the latest version. Windows 64 Bit users can download it from the "Windows x86-64" section.

#### Postgresql Database

* Right click on the Databases section and add a database named productDb.
* Right click on the PostgreSQL 17 section and look at the username information in the "properties" section.
* Add the database to the Intellij Idea application.
* Click on the database icon on the right side of Intellij Idea.
* Click on the plus sign. Select Postgresql as the database.
* Enter the username and password information as the same names we gave in the Postgresql installation.
* Write the database name productDb in the Database section.
* If it says Missing File below, click it and download the necessary files for the database.
* Test Connection by clicking on it.
* Apply & Ok.

#### Database Configuration

- application.properties

```properties
spring.application.name=crud-app

spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.PostgreSQLDialect
spring.datasource.driver-class-name=org.postgresql.Driver
spring.datasource.password=123456
spring.datasource.username=postgres
spring.datasource.url=jdbc:postgresql://localhost:5432/productDb
spring.jpa.hibernate.ddl-auto=create
spring.jpa.show-sql=true
```

- Let's run the Spring boot project. Create a database table.

#### Adding Data to Postgresql Database

```sql
INSERT INTO product (price, created_at, updated_at, brand, description, name)
VALUES
    (299.99, '2023-01-03 12:00:00', '2023-01-03 12:00:00', 'Apple', 'Latest model of iPhone', 'iPhone 14'),
    (199.99, '2023-01-03 12:00:00', '2023-01-03 12:00:00', 'Samsung', 'Latest model of Galaxy', 'Galaxy S23'),
    (99.99, '2023-01-03 12:00:00', '2023-01-03 12:00:00', 'Google', 'Latest model of Pixel', 'Pixel 7'),
    (399.99, '2023-01-03 12:00:00', '2023-01-03 12:00:00', 'OnePlus', 'Latest model of OnePlus', 'OnePlus 12'),
    (499.99, '2023-01-03 12:00:00', '2023-01-03 12:00:00', 'Xiaomi', 'Latest model of Xiaomi', 'Xiaomi 12'),
    (599.99, '2023-01-03 12:00:00', '2023-01-03 12:00:00', 'Sony', 'Latest model of Xperia', 'Xperia 5'),
    (699.99, '2023-01-03 12:00:00', '2023-01-03 12:00:00', 'LG', 'Latest model of LG', 'LG V90'),
    (799.99, '2023-01-03 12:00:00', '2023-01-03 12:00:00', 'Motorola', 'Latest model of Motorola', 'Motorola Edge'),
    (899.99, '2023-01-03 12:00:00', '2023-01-03 12:00:00', 'Nokia', 'Latest model of Nokia', 'Nokia 9'),
    (999.99, '2023-01-03 12:00:00', '2023-01-03 12:00:00', 'Huawei', 'Latest model of Huawei', 'Huawei P50'),
    (1099.99, '2023-01-03 12:00:00', '2023-01-03 12:00:00', 'HTC', 'Latest model of HTC', 'HTC U20'),
    (1199.99, '2023-01-03 12:00:00', '2023-01-03 12:00:00', 'Asus', 'Latest model of Asus', 'Asus Zenfone 8'),
    (1299.99, '2023-01-03 12:00:00', '2023-01-03 12:00:00', 'Oppo', 'Latest model of Oppo', 'Oppo Find X5'),
    (1399.99, '2023-01-03 12:00:00', '2023-01-03 12:00:00', 'Vivo', 'Latest model of Vivo', 'Vivo X90'),
    (1499.99, '2023-01-03 12:00:00', '2023-01-03 12:00:00', 'Realme', 'Latest model of Realme', 'Realme GT 2'),
    (1599.99, '2023-01-03 12:00:00', '2023-01-03 12:00:00', 'Lenovo', 'Latest model of Lenovo', 'Lenovo Legion 3'),
    (1699.99, '2023-01-03 12:00:00', '2023-01-03 12:00:00', 'ZTE', 'Latest model of ZTE', 'ZTE Axon 40'),
    (1799.99, '2023-01-03 12:00:00', '2023-01-03 12:00:00', 'TCL', 'Latest model of TCL', 'TCL 30'),
    (1899.99, '2023-01-03 12:00:00', '2023-01-03 12:00:00', 'Meizu', 'Latest model of Meizu', 'Meizu 19');
```

### Postman

Let's download the Postman application [here](https://www.postman.com/downloads).

### Let's Make a Postman Request

Let's look at the requests of the APIs we added from swagger-ui as follows, let's create a request in the Postman application and run the products API.

- http://localhost:8080/swagger-ui/index.html
- http://localhost:8080/v3/api-docs

```bash
curl --location 'http://localhost:8080/api/products' \
--header 'accept: */*'
```

#### saveProduct

```java
    @Transactional
    public Product saveProduct(ProductRequestDto productRequestDto) {
        Product product = new Product();
        product.setName(productRequestDto.getName());
        product.setDescription(productRequestDto.getDescription());
        product.setBrand(productRequestDto.getBrand());
        product.setPrice(productRequestDto.getPrice());
        product.setCreatedAt(LocalDateTime.now());
        return productRepository.save(product);
    }
```

- Let's use objectMapper.

```xml
<dependency>
    <groupId>com.fasterxml.jackson.core</groupId>
    <artifactId>jackson-databind</artifactId>
    <version>2.15.2</version>
</dependency>
```

```java

import com.fasterxml.jackson.databind.ObjectMapper;

public class ProductService {

    private final ObjectMapper objectMapper = new ObjectMapper();

    @Transactional
    public Product saveProduct(ProductRequestDto productRequestDto) {
        Product product = objectMapper.convertValue(productRequestDto, Product.class);
        product.setCreatedAt(LocalDateTime.now());
        return productRepository.save(product);
    }
}

```

#### Exception

```
crud_app/
├── exception/
│   └── GlobalExceptionHandler.java
```

```java

@RestControllerAdvice
public class GlobalExceptionHandler {

    @ExceptionHandler(Exception.class)
    public ResponseEntity<?> handleAllExceptions(Exception e) {
        Map<String, Object> errorResponse = new HashMap<>();
        errorResponse.put("timestamp", LocalDateTime.now());
        errorResponse.put("status", determineHttpStatus(e).value());
        errorResponse.put("error", determineHttpStatus(e).getReasonPhrase());
        errorResponse.put("message", e.getMessage());
        errorResponse.put("path", getRequestPath());
        return new ResponseEntity<>(errorResponse, determineHttpStatus(e));
    }

    private HttpStatus determineHttpStatus(Exception e) {
        if (e instanceof EntityNotFoundException) {
            return HttpStatus.NOT_FOUND;
        } else if (e instanceof IllegalArgumentException || e instanceof IllegalStateException) {
            return HttpStatus.BAD_REQUEST;
        } else {
            return HttpStatus.INTERNAL_SERVER_ERROR;
        }
    }

    private String getRequestPath() {
        ServletRequestAttributes attributes = (ServletRequestAttributes) RequestContextHolder.getRequestAttributes();
        if (attributes != null) {
            return attributes.getRequest().getRequestURI();
        }
        return "Unknown Path";
    }
}

```
