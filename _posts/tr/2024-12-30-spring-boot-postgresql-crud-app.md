---
title: Spring Boot Postgresql Crud Uygulama
date: 2024-12-30 13:14 +0300
categories: [Backend, Spring Boot Postgresql Crud Uygulama]
tags: [spring-boot, java, postgresql, material-ui, web-development, api, rest-api, deployment, backend, scalable, user-interface, java-backend, crud, basic-app, todo, to-do app, uygulama, proje]
author: selin-topcu
lang: tr
---

## Spring Initalizr ile backend kısmını oluşturalım

- [Spring initalizr](https://start.spring.io) uygulamayı oluşturalım.
  - Project: Maven
  - Language: Java
  - Spring Boot: 3.4.1
  - Java: 21
  - Dependencies
    - Spring Web
    - PostgreSQL Driver
    - Spring Data JPA

Not: Neden lombok kullanmadık bu [podcastte](https://open.spotify.com/episode/2SKXhtsUsIqON4AafgxavK?si=qvd4wA1sTHywm6RQYjPYyQ&nd=1&dlsi=321b8f07b0324cfd) 20.dk dan sonra anlatılıyor, detaylı bakabilirsiniz.

Zip dosyasını indirelim. Zipten çıkaralım. Intellij Idea da projeyi açalım.

### İntellij Idea

#### İntellij Idea Kurulum

- İntellij Idea Ultimate veya ücretsiz sürümü olan Community [burdan](https://www.jetbrains.com/idea/download/other.html) indirebilirsiniz.

#### Proje Konfigurasyonları

* **İntellij idea** uygulamasında aşağıdaki kontrolleri yapalım.
  * File>Project Structure SDK: 21 seçili olmalıdır.
    * Eğer 21 sürümü kurulu değilse [burdan](https://www.oracle.com/java/technologies/downloads/) JDK 21 seçelim. Installer yazan kısımdan indirip kuralım.
    * CMD'den java sürümünü kontrol edelim.
      * java -version
  * Proje de anatosyonlar bulunamıyorsa ve "Cannot resolve symbol" bu hatayı veriyorsa aşağıdaki işlemleri yapalım.
    * File>Invalidate Caches çıkan seçeneklerin hepsini seçip Invalidate and Restart butonuna basalım.

### Package Yapısını Oluşturalım

CrudAppApplication.java dizinin olduğu yere aşağıdaki gibi dosyaları ekleyelim

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

### Katmanlı Mimari

#### Controller Katmanı

Kullanıcı ile uygulama arasındaki iletişimi yönetir. API uç noktaları burada tanımlanır.

- `ProductController.java`, ürünle ilgili istekleri alır ve işlemleri başlatır.

#### DTO Katmanı

Veri iletimi için kullanılan sınıfları içerir. İstek ve yanıt objeleri burada tanımlanır.

- `ProductRequestDto.java` Ürün oluşturma veya güncelleme işlemleri için gelen veriyi taşır.
- `ProductResponseDto.java` Ürün verisinin dışarıya dönüştürülmüş halini taşır.

#### Entity Katmanı

Uygulamanın temel veri yapısını tanımlar. Genellikle veritabanı tablolarını temsil eden entity sınıflarından oluşur.

- `Product.java`, ürün verisinin yapısını ve veritabanı ile etkileşim için gerekli anotasyonları içerir.

#### Repository Katmanı

Veritabanı ile etkileşim kurar. CRUD işlemleri ve özel sorgular burada gerçekleştirilir.

- `ProductRepository.java`, Spring Data JPA kullanarak ürün verisine erişim sağlar.

#### Service Katmanı

İş kurallarını uygular ve mantıksal işlemleri yürütür. Controller ve Repository katmanları arasında aracıdır.

- `ProductService.java`, ürünle ilgili işlemleri yönetir, örneğin ürün ekleme, silme, güncelleme gibi işlemleri içerir.

#### CrudAppApplicationMyProjectApplication.java

Uygulamanın başlangıç noktasıdır. Spring Boot uygulamasını başlatır.

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

Swagger ui için aşağıdaki [OpenAPI](https://www.baeldung.com/spring-rest-openapi-documentation) dependecy ekleyelim.

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

#### Postgresql Kurulum

- [Burdan](https://www.postgresql.org/download) Postgresql indirme sitesine gidelim. İşletim sistemini seçelim.
- Windows için;
  - "Windows" yazana tıklayalım.
  - "Download the installer" tıklayalım. Son sürümü indirelim. Windows 64 Bit kullananlar "Windows x86-64" yazan kısımdan indirebilir.

#### Postgresql Veritabanı

* Databases kısmından sağ tık yaparak productDb adında bir veritabanını ekleyelim.
* PostgreSQL 17 yazan kısıma sağ tık "properties" yazan kısımdan username bilgisine bakalım.
* İntellij idea uygulamasına veritabanını ekleyelim.
  * İntellij idea sağ tarafta veritabanı simgesine tıklayalım.
  * Artı işaretine tıklayalım. Veritabanı olarak Postgresql seçelim
  * Username ve password bilgilerini Postgresql kurulumunda verdiğimiz isimlerle aynı olacak şekilde girelim.
  * Database kısmına productDb veritabanı ismini yazalım.
  * Aşağıda Missing File yazıyorsa tıklayalım ve veritabanı için gerekli dosyaları indirsin.
  * Test Connection tıklayarak test edelim.
  * Apply & Ok.

#### Veritabanı Konfigurasyonu

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

- Spring boot projesini çalıştıralım. Veritabanı tablosu oluşturulsun.

#### Postgresql Veritabanına Veri Ekleme

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

Postman uygulamasını [burdan](https://www.postman.com/downloads) indirelim.

### Postman İstek Atalım

Aşağıdaki gibi swagger-ui dan eklediğimiz apilerin isteklerine bakalım, postman uygulamasında bir request oluşturup products apisini çalıştıralım.

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

- objectMapper kullanalım.

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
        // Dto'yu Product nesnesine dönüştür
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
