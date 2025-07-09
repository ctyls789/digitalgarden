---
{"dg-publish":true,"permalink":"/20250702/Spring boot using by the hibernate/"}
---


# interface

---

# Spring Data JPA 核心介面：Repository 家族

在 Spring Data JPA 中，數據訪問層的設計遵循約定優於配置的原則，並且高度抽象化。這主要透過一系列繼承關係的介面來實現，讓開發者無需編寫大量的樣板代碼。

---

## 1. `Repository` (根介面)

`org.springframework.data.repository.Repository`

- **定義：** 這是 **Spring Data 家族中最頂層的介面**，是一個**標記介面 (Marker Interface)**。
    
- **功能：** 它本身不包含任何方法。它的作用就是作為所有 Spring Data 庫 (如 Spring Data JPA, Spring Data MongoDB, Spring Data Redis 等) 中 **Repository 介面的基礎**。只要一個介面繼承了 `Repository`，Spring Data 就會知道需要為它創建一個實現類，並將其註冊為一個 Spring Bean。
    
- **用途：** 你通常不會直接使用或繼承這個介面，而是繼承它的子介面，這些子介面定義了實際的操作方法(最上層)。
    

---

## 2. `CrudRepository`

`org.springframework.data.repository.CrudRepository`

- **定義：** 繼承自 `Repository`。
    
- **功能：** 提供了最基本的 **CRUD (Create, Read, Update, Delete)** 操作方法，覆蓋了大部分單一實體的數據庫交互需求。
    
    - `save(S entity)`: 保存或更新實體。
        
    - `findById(ID id)`: 根據 ID 查找單個實體。
        
    - `findAll()`: 查找所有實體。
        
    - `deleteById(ID id)`: 根據 ID 刪除實體。
        
    - `count()`: 統計實體總數。
        
    - `existsById(ID id)`: 檢查實體是否存在。
        
- **用途：** 當你只需要簡單的 CRUD 功能時，它是最方便的選擇。
    

---

## 3. `PagingAndSortingRepository`

`org.springframework.data.repository.PagingAndSortingRepository`

- **定義：** 繼承自 `CrudRepository`。
    
- **功能：** 在 `CrudRepository` 的基礎上，增加了**分頁 (Paging)** 和**排序 (Sorting)** 的功能。
    
    - `findAll(Sort sort)`: 查找所有實體並排序。
        
    - `findAll(Pageable pageable)`: 查找所有實體並分頁。
        
- **用途：** 當你需要從數據庫獲取大量數據，並且希望對結果進行分頁顯示或按特定字段排序時，這個介面非常有用。它幫助你避免一次性加載所有數據到內存中，提高應用性能。
    

---

## 4. `JpaRepository`

`org.springframework.data.jpa.repository.JpaRepository`

- **定義：** 繼承自 `PagingAndSortingRepository`。它是一個**特定於 JPA (Java Persistence API)** 的介面。
    
- **功能：** 在 `PagingAndSortingRepository` 的基礎上，增加了更多 **JPA 特有的功能**，例如：
    
    - `flush()`: 將所有掛起的更改刷新到數據庫。
        
    - `saveAndFlush(S entity)`: 保存實體並立即刷新到數據庫。
        
    - `deleteInBatch(Iterable<T> entities)`: 批量刪除實體。
        
    - `getOne(ID id)` (在 Spring Data JPA 2.5 後已被 `getReferenceById(ID id)` 取代): 獲取實體的代理實例。
        
- **用途：** 這是你在使用 Spring Data JPA 時 **最常使用** 的介面。它提供了絕大部分你可能需要的數據訪問功能。
    

---

## 5. `QueryByExampleExecutor`

`org.springframework.data.repository.query.QueryByExampleExecutor`

- **定義：** 這是 Spring Data JPA 獨立的一個介面，通常被 `JpaRepository` **自動繼承** (或者你可以獨立繼承它)。
    
- **功能：** 提供了基於 **"QBE (Query By Example)"** 的查詢功能。這是一種非常方便的動態查詢方式，你不需要寫 SQL 或 JPQL 語句，只需提供一個包含查詢條件的實體對象作為「範例」，Spring Data 就會根據這個範例自動構建查詢。
    
    - `findAll(Example<S> example)`
        
    - `findOne(Example<S> example)`
        
    - `count(Example<S> example)`
        
    - `exists(Example<S> example)`
        
- **用途：** 適用於構建動態的、基於屬性匹配的查詢，特別是在用戶界面有許多可選查詢條件時非常有用。它允許你非常簡潔地表達等值匹配、包含匹配等查詢，而無需條件組合。
    

---

### 關係圖示 (簡化)

```
+----------------+
|    Repository  |<-- 基礎介面 (標記)
+--------^-------+
         |
+--------+-------+
|  CrudRepository  |<-- 基本 CRUD
+--------^-------+
         |
+--------+-------+
|PagingAndSorting |<-- 分頁與排序
|   Repository    |
+--------^-------+
         |
+--------+-------+
|   JpaRepository  |<-- JPA 特有功能 (通常包含 QBE)
+----------------+

同時，QueryByExampleExecutor 是獨立提供的介面，JpaRepository 自動繼承了它的功能。
```


----
## 1. 專案結構與依賴 (Maven/Gradle)

這是最根本的改變。你會需要：

- **新增 Spring Boot Starter 依賴：** 移除大部分手動的 Hibernate、Spring Core、Servlet API 依賴，改為引入 Spring Boot 提供的 Starter 依賴，例如：
    
    - `spring-boot-starter-data-jpa`：包含了 Spring Data JPA、Hibernate Core 和數據源配置。
        
    - `spring-boot-starter-web`：如果你需要建構 RESTful API 或 Web 應用。
        
    - **數據庫驅動**（例如 `mysql-connector-java`）。
        
    - `spring-boot-starter-test`：用於測試。
        
	- **移除傳統 Hibernate 相關依賴：** 例如 `hibernate-core` (如果已經被 `spring-boot-starter-data-jpa` 覆蓋)。
    
- **專案結構調整：** Spring Boot 建議的專案結構通常是單一 JAR 包部署，主應用類放在根包下。
    

---

## 2. 配置 (Configuration)

這是改動最大的地方之一。

- **移除 `hibernate.cfg.xml`：**
    
    - **Spring Boot 會自動配置 Hibernate。** 你將不再需要這個文件。
        
    - 所有的數據庫連接信息 (URL, 用戶名, 密碼, 驅動) 和 Hibernate 屬性 (方言, `show_sql`, `ddl-auto` 等) 都將會移到 **`src/main/resources/application.properties` 或 `application.yml`** 中。
        
- **移除你的 `HibernateUtil.java`：**
    
    - 由於 Spring Boot 會自動配置和管理 `SessionFactory` (實際上是 JPA 的 `EntityManagerFactory`) 和 `Session` (實際上是 `EntityManager`)，你的 `HibernateUtil` 工具類就不再需要了。
        
    - Spring 會透過 **依賴注入 (Dependency Injection)** 提供 `EntityManager` 和 `SessionFactory` (如果你需要底層的 Hibernate API)。
        
- **數據源配置：** 在 `application.properties` 中配置 `spring.datasource.*` 屬性。
    
- **JPA/Hibernate 屬性：** 在 `application.properties` 中配置 `spring.jpa.*` 屬性來控制 Hibernate 的行為。
    

---

## 3. 模型層 (Model / Entity)

這一層的改動相對較小，但有最佳實踐。

- **JPA 註解：** 如果你傳統的 Hibernate 應用已經使用了 JPA 註解 (`@Entity`, `@Table`, `@@Id`, `@Column` 等)，那麼你的實體類幾乎可以保持不變。
    
- **移除 `.hbm.xml` 映射文件：** 如果你之前是用 `XXX.hbm.xml` 來做 ORM 映射，那麼你需要將這些映射信息轉化為 JPA 註解直接寫在 Java 實體類上。這是現代 JPA/Hibernate 的主流做法。
    
- **套件掃描：** Spring Boot 通常會自動掃描主應用類所在套件及其子套件下的實體類。如果你的實體類在其他位置，你可能需要在 `application.properties` 中配置 `spring.jpa.properties.hibernate.packagesToScan` 或在 Spring 配置類中明確指定。
    

---

## 4. 數據訪問層 (DAO / Repository)

這是改動最顯著且獲益最大的地方。

- **棄用手動 DAO 實現：** 你將不再需要手動編寫類似 `ProductDaoImpl` 這樣的類，裡面包含 `session.beginTransaction()`, `session.save()`, `session.close()` 等冗長的代碼。
    
- **引入 Spring Data JPA `Repository` 介面：**
    
    - 對於每個實體，你只需創建一個繼承自 `JpaRepository` (或 `CrudRepository`, `PagingAndSortingRepository`) 的介面。
        
    - Spring Data JPA 會在運行時自動為這些介面生成實現，並將它們作為 Spring Bean 註冊。
        
    - 你只需定義方法簽名（例如 `findByName(String name)`），Spring Data JPA 就會自動解析並生成相應的查詢。
        
- **依賴注入 `Repository`：** 在你的 Service 層中，直接透過 `@Autowired` 或建構子注入你的 `ProductRepository` 介面。
    

---

## 5. 業務邏輯層 (Service)

這一層的代碼會變得更簡潔、更聚焦於業務邏輯。

- **移除手動的 Session 管理：** 你將不再需要在 Service 層手動獲取 `Session`、啟動事務、提交或回滾事務、關閉 `Session`。
    
- **使用 Spring 的 `@Transactional` 註解：**
    
    - Spring Boot 會自動配置事務管理器。
        
    - 你只需在 Service 方法上簡單地添加 `@Transactional` 註解，Spring 就會自動為你處理事務的開始、提交和回滾。
        
- **注入 `Repository`：** 直接將新的 Spring Data JPA `Repository` 介面注入到 Service 類中，並調用其方法。
    

---

## 6. 控制層 (Servlet / Controller)

如果你的傳統應用是基於 Servlet 或 JSP 的，這部分也會有重大改變。

- **棄用傳統 Servlet：** 如果你直接使用 `HttpServlet`，你會將其替換為 Spring MVC 的 **`@RestController`** 或 **`@Controller`**。
    
- **`@RestController` / `@Controller`：**
    
    - 用於定義處理 HTTP 請求的類。
        
    - 你可以使用 `@RequestMapping`, `@GetMapping`, `@PostMapping` 等註解來定義請求映射。
        
    - 利用 `@RequestBody` 和 `@ResponseBody` 輕鬆處理 JSON/XML 請求體和響應體。
        
- **注入 Service：** 在 Controller 中注入你的 Service 層 Bean，並調用其方法處理業務邏輯。
    

---

### 總結一下，從傳統 Hibernate 遷移到 Spring Boot 的核心變化是：

- **從手動配置轉向自動配置：** 幾乎所有數據庫和 ORM 的基礎配置都由 Spring Boot 自動處理或透過 `application.properties` 完成。
    
- **從手動資源管理轉向 Spring 管理：** `Session`, `SessionFactory`, 事務等都由 Spring 容器管理，你只需聲明式地使用註解。
    
- **從樣板代碼轉向聲明式介面：** 透過 Spring Data JPA 的 `Repository` 介面，大量數據庫 CRUD 相關的樣板代碼被消除。
    
- **從傳統 Web 層轉向 Spring MVC/WebFlux：** 構建 RESTful API 或 Web 應用更加便捷。

![Pasted image 20250703145049.png](/img/user/Pasted%20image%2020250703145049.png)
https://ithelp.ithome.com.tw/m/articles/10323805
https://openhome.cc/Gossip/SpringGossip/SessionFactoryInjection.html


![Pasted image 20250703151621.png](/img/user/Pasted%20image%2020250703151621.png)

https://matthung0807.blogspot.com/2019/12/spring-boot-controlleradvice.html
```java
import com.example.exception.InvalidProductDataException; import com.example.exception.ProductNotFoundException; import com.example.exception.ProductReferencedException; import com.example.api.error.ErrorResponse; // 你的錯誤響應 DTO import org.springframework.http.HttpStatus; import org.springframework.http.ResponseEntity; import org.springframework.web.bind.annotation.ControllerAdvice; import org.springframework.web.bind.annotation.ExceptionHandler; import org.springframework.web.context.request.WebRequest; @ControllerAdvice public class GlobalExceptionHandler { @ExceptionHandler(InvalidProductDataException.class) public ResponseEntity<ErrorResponse> handleInvalidProductDataException(InvalidProductDataException ex, WebRequest request) { ErrorResponse errorResponse = new ErrorResponse( HttpStatus.BAD_REQUEST, // 400 ex.getMessage(), request.getDescription(false).replace("uri=", "") ); if (ex.getFieldErrors() != null && !ex.getFieldErrors().isEmpty()) { // 如果你的 ErrorResponse DTO 有一個 Map<String, String> errors 字段，可以在這裡設置 // errorResponse.setErrors(ex.getFieldErrors()); } return new ResponseEntity<>(errorResponse, HttpStatus.BAD_REQUEST); } @ExceptionHandler(ProductNotFoundException.class) public ResponseEntity<ErrorResponse> handleProductNotFoundException(ProductNotFoundException ex, WebRequest request) { ErrorResponse errorResponse = new ErrorResponse( HttpStatus.NOT_FOUND, // 404 ex.getMessage(), request.getDescription(false).replace("uri=", "") ); return new ResponseEntity<>(errorResponse, HttpStatus.NOT_FOUND); } @ExceptionHandler(ProductReferencedException.class) public ResponseEntity<ErrorResponse> handleProductReferencedException(ProductReferencedException ex, WebRequest request) { ErrorResponse errorResponse = new ErrorResponse( HttpStatus.CONFLICT, // 409 ex.getMessage(), request.getDescription(false).replace("uri=", "") ); return new ResponseEntity<>(errorResponse, HttpStatus.CONFLICT); } // 通用的異常處理器 @ExceptionHandler(Exception.class) public ResponseEntity<ErrorResponse> handleGlobalException(Exception ex, WebRequest request) { LOGGER.error("An unhandled exception occurred: {}", ex.getMessage(), ex); ErrorResponse errorResponse = new ErrorResponse( HttpStatus.INTERNAL_SERVER_ERROR, // 500 "An unexpected error occurred: " + ex.getMessage(), request.getDescription(false).replace("uri=", "") ); return new ResponseEntity<>(errorResponse, HttpStatus.INTERNAL_SERVER_ERROR); } }

```

![Pasted image 20250704092632.png](/img/user/Pasted%20image%2020250704092632.png)


```java
Spring MVC 常用註解功能整理
Spring MVC 透過豐富的註解（Annotations）來簡化開發，讓開發者能夠以更宣告式（Declarative）的方式定義控制器、處理請求、綁定參數等等。以下將常用註解的功能進行整理：

控制器與請求映射相關
@Controller: 標註在類別上，表示該類別是一個 Spring MVC 的控制器。Spring 會自動掃描並註冊此類別，使其能夠處理網頁請求。

@RestController: 結合了 @Controller 和 @ResponseBody 的功能。通常用於開發 RESTful API，表示控制器中的所有方法的返回值都會直接寫入 HTTP 響應體中，而不是解析為視圖名稱。

@RequestMapping:

在類別上使用: 定義控制器處理請求的基礎路徑。例如，@RequestMapping("/users") 表示該控制器下的所有方法都以 /users 開頭。

在方法上使用: 定義方法處理特定請求的路徑、HTTP 方法（GET, POST, PUT, DELETE 等）、請求參數、請求頭等。

常用屬性:

value (或不寫，直接寫路徑): 定義請求的路徑。

method: 指定處理的 HTTP 方法，例如 RequestMethod.GET。

consumes: 指定請求的 Content-Type，例如 consumes = "application/json"。

produces: 指定響應的 Content-Type，例如 produces = "application/xml"。

params: 指定請求必須包含的參數。

headers: 指定請求必須包含的請求頭。

@GetMapping: 專門用於處理 HTTP GET 請求的快捷註解，等同於 @RequestMapping(method = RequestMethod.GET)。

@PostMapping: 專門用於處理 HTTP POST 請求的快捷註解，等同於 @RequestMapping(method = RequestMethod.POST)。

@PutMapping: 專門用於處理 HTTP PUT 請求的快捷註解，等同於 @RequestMapping(method = RequestMethod.PUT)。

@DeleteMapping: 專門用於處理 HTTP DELETE 請求的快捷註解，等同於 @RequestMapping(method = RequestMethod.DELETE)。

@PatchMapping: 專門用於處理 HTTP PATCH 請求的快捷註解，等同於 @RequestMapping(method = RequestMethod.PATCH)。

請求參數與資料綁定相關
@RequestParam: 用於綁定請求參數（URL 查詢參數或表單參數）到方法的參數上。

常用屬性:

value (或不寫，直接寫參數名): 指定請求參數的名稱。

required: 指定該參數是否為必需，預設為 true。

defaultValue: 當請求中沒有該參數時的預設值。

@PathVariable: 用於綁定 URI 模板變數到方法的參數上。

範例: @GetMapping("/users/{id}") 中的 id 可以通過 @PathVariable("id") Long id 來獲取。

@RequestBody: 用於將 HTTP 請求體中的內容綁定到方法的參數上。通常用於接收 JSON 或 XML 格式的請求數據。Spring 會自動將請求體轉換為指定的 Java 物件。

@ResponseBody: 標註在方法上，表示該方法的返回值會直接寫入 HTTP 響應體中，而不是作為視圖名稱。常用於 RESTful API 開發。

@ModelAttribute:

在方法參數上: 將請求參數綁定到一個物件上（通常是 JavaBean），Spring 會自動創建或從模型中獲取該物件，並填充其屬性。

在方法上: 標註在方法上，表示該方法的返回值會被添加到模型（Model）中，供視圖使用。這個方法會在控制器中的其他 @RequestMapping 方法執行之前被呼叫。

@RequestHeader: 用於綁定請求頭（Request Header）的值到方法的參數上。

@CookieValue: 用於綁定 HTTP Cookie 的值到方法的參數上。

模型與視圖相關
@SessionAttribute: (已棄用，推薦使用 @SessionAttributes) 用於指定模型中的屬性應該被儲存到 HttpSession 中。

@SessionAttributes: 標註在類別上，用於指定哪些模型屬性應該被儲存在會話（Session）中。

@RequestAttribute: 用於訪問通過 request.setAttribute() 設置的請求屬性。

資料驗證相關
Spring MVC 通常與 JSR 303 (Bean Validation) 規範結合使用，提供數據驗證功能。

@Valid: 標註在方法參數上，表示對該參數進行驗證。通常與 @RequestBody 或 @ModelAttribute 結合使用。

@Validated: 與 @Valid 類似，但功能更強大，支援分組驗證。

其他常用註解
@Component: Spring 泛型元件的基礎註解，可以用在任何 Spring 管理的元件上。

@Service: 標註在服務層（Service Layer）的類別上，通常用於定義業務邏輯。

@Repository: 標註在資料存取層（DAO Layer）的類別上，通常用於資料庫操作。

@Autowired: 用於自動裝配（Dependency Injection），Spring 會自動尋找並注入相依的 Bean。

@Qualifier: 與 @Autowired 結合使用，當有多個相同類型的 Bean 時，可以用來指定要注入哪一個 Bean。

@Value: 用於注入配置屬性值，例如從 application.properties 或 application.yml 中讀取值。

@Configuration: 標註在類別上，表示該類別是一個配置類，可以用來定義 Bean。

@Bean: 標註在方法上，表示該方法的返回值是一個 Spring Bean。通常在 @Configuration 類別中使用。

@ControllerAdvice: 標註在類別上，用於定義全局的異常處理、數據綁定初始化或模型數據處理等。可以集中處理多個控制器的共同邏輯。

@ExceptionHandler: 標註在方法上，用於處理特定類型的異常。通常在 @ControllerAdvice 類別中使用。
```


```
package com.example.service.impl;

import com.example.model.WorkOrder;
import com.example.model.Product; // Import the Product model
import com.example.repository.WorkOrderRepository; // Use Spring Data JPA Repository
import com.example.repository.ProductRepository; // Use Spring Data JPA Repository
import com.example.service.WorkOrderService; // Service Interface

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;
import org.springframework.transaction.annotation.Transactional; // For transaction management

import java.math.BigDecimal;
import java.time.LocalDateTime; // Use LocalDateTime consistently
import java.util.Arrays;
import java.util.List;
import java.util.Optional; // For Optional return types

import org.slf4j.Logger; // Use SLF4J for logging
import org.slf4j.LoggerFactory;

/**
 * WorkOrderService 介面的實現類。
 * 負責生產工單相關的業務邏輯，並調用 Spring Data JPA Repository 進行數據庫操作。
 */
@Service // 標註此類別為 Spring 服務元件
@Transactional // 預設所有公共方法都在事務中運行
public class WorkOrderServiceImpl implements WorkOrderService {

    private static final Logger LOGGER = LoggerFactory.getLogger(WorkOrderServiceImpl.class);

    // 注入 Spring Data JPA Repositories
    private final WorkOrderRepository workOrderRepository;
    private final ProductRepository productRepository;

    // 定義有效的工單狀態
    private static final List<String> VALID_STATUSES = Arrays.asList("Pending", "In Progress", "Completed", "Cancelled");

    /**
     * Spring 推薦的構造函數注入方式。
     *
     * @param workOrderRepository WorkOrderRepository 實例。
     * @param productRepository ProductRepository 實例。
     */
    @Autowired
    public WorkOrderServiceImpl(WorkOrderRepository workOrderRepository, ProductRepository productRepository) {
        this.workOrderRepository = workOrderRepository;
        this.productRepository = productRepository;
        LOGGER.info("WorkOrderServiceImpl 已初始化，並透過構造函數注入 Repository。");
    }

    @Override
    public WorkOrder createWorkOrder(WorkOrder workOrder) throws IllegalArgumentException {
        // 數據驗證
        if (workOrder == null || workOrder.getWorkOrderNumber() == null || workOrder.getWorkOrderNumber().trim().isEmpty() ||
                workOrder.getProductId() <= 0 || workOrder.getQuantity() == null ||
                workOrder.getQuantity().compareTo(BigDecimal.ZERO) <= 0 ||
                workOrder.getScheduledStartDate() == null || workOrder.getScheduledDueDate() == null ||
                workOrder.getStatus() == null || workOrder.getStatus().trim().isEmpty()) {
            LOGGER.warn("新增工單時數據無效: 必填字段缺失或無效。");
            throw new IllegalArgumentException("工單編號、產品ID、數量、預計開始日期、預計完成日期和狀態為必填項，且數量必須大於零。");
        }

        if (!VALID_STATUSES.contains(workOrder.getStatus())) {
            LOGGER.warn("新增工單時狀態無效: {}", workOrder.getStatus());
            throw new IllegalArgumentException("無效的工單狀態: " + workOrder.getStatus());
        }

        // 使用 LocalDateTime 的 isAfter/isBefore 進行日期比較
        if (workOrder.getScheduledStartDate().isAfter(workOrder.getScheduledDueDate())) {
            LOGGER.warn("新增工單時預計日期無效: 預計開始日期晚於預計完成日期。");
            throw new IllegalArgumentException("預計開始日期不能晚於預計完成日期。");
        }

        // 驗證產品是否存在
        Optional<Product> productOptional = productRepository.findById(workOrder.getProductId());
        if (productOptional.isEmpty()) {
            LOGGER.warn("新增工單失敗: 產品ID {} 不存在。", workOrder.getProductId());
            throw new IllegalArgumentException("指定的產品不存在。");
        }
        Product product = productOptional.get();

        // 設置產品相關的非持久化屬性（如果WorkOrder模型中有這些屬性）
        workOrder.setProductName(product.getProductName());
        workOrder.setProductCode(product.getProductCode());
        // 確保工單的單位與產品的單位一致 (如果產品有單位)
        if (product.getUnit() != null && !product.getUnit().trim().isEmpty()) {
             workOrder.setUnit(product.getUnit());
        } else if (workOrder.getUnit() == null || workOrder.getUnit().trim().isEmpty()){
            // 如果產品沒有單位，並且工單單位為空，拋出異常
             throw new IllegalArgumentException("工單單位不能為空，請檢查產品單位或手動指定工單單位。");
        }
        // 如果workOrder.getUnit() 不為空且產品單位為空，則繼續使用workOrder.getUnit()


        LOGGER.info("Service: 嘗試新增工單: {}", workOrder.getWorkOrderNumber());
        // JpaRepository 的 save 方法會返回持久化後的實體，其中會包含自動生成的ID
        return workOrderRepository.save(workOrder);
    }

    @Override
    @Transactional(readOnly = true) // 標註為只讀事務，優化查詢性能
    public Optional<WorkOrder> getWorkOrderById(int workOrderId) throws IllegalArgumentException {
        if (workOrderId <= 0) {
            LOGGER.warn("獲取工單時ID無效: {}", workOrderId);
            throw new IllegalArgumentException("工單ID無效，必須大於0。");
        }
        LOGGER.info("Service: 嘗試獲取工單，ID: {}", workOrderId);
        return workOrderRepository.findById(workOrderId); // 返回 Optional
    }

    @Override
    @Transactional(readOnly = true)
    public List<WorkOrder> getAllWorkOrders() {
        LOGGER.info("Service: 嘗試獲取所有工單。");
        return workOrderRepository.findAll();
    }

    @Override
    @Transactional(readOnly = true)
    public List<WorkOrder> getWorkOrdersByStatus(String status) throws IllegalArgumentException {
        if (status == null || status.trim().isEmpty() || !VALID_STATUSES.contains(status)) {
            LOGGER.warn("獲取工單時狀態無效或為空: {}", status);
            throw new IllegalArgumentException("無效或為空的工單狀態。");
        }
        LOGGER.info("Service: 嘗試獲取狀態為 '{}' 的工單。", status);
        // 假設 WorkOrderRepository 已經定義了 findByStatus 方法
        return workOrderRepository.findByStatus(status);
    }

    @Override
    @Transactional(readOnly = true)
    public List<WorkOrder> getWorkOrdersByProductId(int productId) throws IllegalArgumentException {
        if (productId <= 0) {
            LOGGER.warn("獲取工單時產品ID無效: {}", productId);
            throw new IllegalArgumentException("產品ID無效，必須大於0。");
        }
        // 驗證產品是否存在
        if (!productRepository.existsById(productId)) {
            LOGGER.warn("獲取工單失敗: 產品ID {} 不存在。", productId);
            throw new IllegalArgumentException("指定的產品不存在。");
        }
        LOGGER.info("Service: 嘗試獲取產品ID {} 的所有工單。", productId);
        // 假設 WorkOrderRepository 已經定義了 findByProductId 方法
        return workOrderRepository.findByProductId(productId);
    }

    @Override
    public Optional<WorkOrder> updateWorkOrder(WorkOrder workOrder) throws IllegalArgumentException {
        // 數據驗證
        if (workOrder == null || workOrder.getWorkOrderId() <= 0 ||
                workOrder.getWorkOrderNumber() == null || workOrder.getWorkOrderNumber().trim().isEmpty() ||
                workOrder.getProductId() <= 0 || workOrder.getQuantity() == null ||
                workOrder.getQuantity().compareTo(BigDecimal.ZERO) <= 0 ||
                workOrder.getScheduledStartDate() == null || workOrder.getScheduledDueDate() == null ||
                workOrder.getStatus() == null || workOrder.getStatus().trim().isEmpty()) {
            LOGGER.warn("更新工單時數據無效: 必填字段缺失或無效。工單ID: {}", workOrder.getWorkOrderId());
            throw new IllegalArgumentException("工單ID、工單編號、產品ID、數量、預計開始日期、預計完成日期和狀態為必填項，且數量必須大於零。");
        }
        if (!VALID_STATUSES.contains(workOrder.getStatus())) {
            LOGGER.warn("更新工單時狀態無效: {}", workOrder.getStatus());
            throw new IllegalArgumentException("無效的工單狀態: " + workOrder.getStatus());
        }
        // 日期比較
        if (workOrder.getScheduledStartDate().isAfter(workOrder.getScheduledDueDate())) {
            LOGGER.warn("更新工單時預計日期無效: 預計開始日期晚於預計完成日期。");
            throw new IllegalArgumentException("預計開始日期不能晚於預計完成日期。");
        }

        // 確保要更新的工單存在
        Optional<WorkOrder> existingWorkOrderOptional = workOrderRepository.findById(workOrder.getWorkOrderId());
        if (existingWorkOrderOptional.isEmpty()) {
            LOGGER.warn("更新工單失敗，工單ID不存在: {}", workOrder.getWorkOrderId());
            return Optional.empty(); // 要更新的工單不存在
        }
        WorkOrder workOrderToUpdate = existingWorkOrderOptional.get();

        // 驗證產品是否存在 (如果用戶更改了產品ID)
        Optional<Product> productOptional = productRepository.findById(workOrder.getProductId());
        if (productOptional.isEmpty()) {
            LOGGER.warn("更新工單失敗: 產品ID {} 不存在。", workOrder.getProductId());
            throw new IllegalArgumentException("指定的產品不存在。");
        }
        Product product = productOptional.get();
        // 更新非持久化屬性及確保單位一致性
        workOrder.setProductName(product.getProductName());
        workOrder.setProductCode(product.getProductCode());
        if (product.getUnit() != null && !product.getUnit().trim().isEmpty()) {
            workOrder.setUnit(product.getUnit());
        } else if (workOrder.getUnit() == null || workOrder.getUnit().trim().isEmpty()){
            throw new IllegalArgumentException("工單單位不能為空，請檢查產品單位或手動指定工單單位。");
        }
        // 如果workOrder.getUnit() 不為空且產品單位為空，則繼續使用workOrder.getUnit()


        // 僅更新可修改的業務屬性
        workOrderToUpdate.setWorkOrderNumber(workOrder.getWorkOrderNumber());
        workOrderToUpdate.setProductId(workOrder.getProductId());
        workOrderToUpdate.setQuantity(workOrder.getQuantity());
        workOrderToUpdate.setUnit(workOrder.getUnit());
        workOrderToUpdate.setScheduledStartDate(workOrder.getScheduledStartDate());
        workOrderToUpdate.setScheduledDueDate(workOrder.getScheduledDueDate());
        workOrderToUpdate.setActualStartDate(workOrder.getActualStartDate()); // 允許在更新時設定
        workOrderToUpdate.setActualCompletionDate(workOrder.getActualCompletionDate()); // 允許在更新時設定
        workOrderToUpdate.setStatus(workOrder.getStatus());
        workOrderToUpdate.setNotes(workOrder.getNotes());
        // createDate 不會被更新 (因為 @Column(updatable = false))
        // updateDate 會在 save 時被 @PreUpdate 自動更新

        LOGGER.info("Service: 嘗試更新工單，ID: {}", workOrder.getWorkOrderId());
        return Optional.of(workOrderRepository.save(workOrderToUpdate));
    }

    @Override
    public boolean deleteWorkOrder(int workOrderId) throws IllegalArgumentException {
        if (workOrderId <= 0) {
            LOGGER.warn("刪除工單時ID無效: {}", workOrderId);
            throw new IllegalArgumentException("工單ID無效，必須大於0。");
        }
        // 檢查工單是否存在
        if (!workOrderRepository.existsById(workOrderId)) {
            LOGGER.warn("刪除工單失敗，工單ID不存在: {}", workOrderId);
            return false; // 如果工單不存在，則認為刪除成功 (冪等性)
        }

        LOGGER.info("Service: 嘗試刪除工單，ID: {}", workOrderId);
        workOrderRepository.deleteById(workOrderId);
        return true;
    }

    @Override
    public Optional<WorkOrder> startWorkOrder(int workOrderId, LocalDateTime actualStartDate) throws IllegalArgumentException, IllegalStateException {
        if (workOrderId <= 0) {
            throw new IllegalArgumentException("工單ID無效，必須大於0。");
        }
        if (actualStartDate == null) {
            throw new IllegalArgumentException("實際開始日期不能為空。");
        }

        Optional<WorkOrder> workOrderOptional = workOrderRepository.findById(workOrderId);
        if (workOrderOptional.isEmpty()) {
            throw new IllegalArgumentException("要開始的工單不存在。");
        }
        WorkOrder workOrder = workOrderOptional.get();

        // 業務規則：只有 Pending 狀態的工單才能開始
        String currentStatus = workOrder.getStatus();
        if (!"Pending".equalsIgnoreCase(currentStatus)) {
            throw new IllegalStateException("只有 '待處理' 狀態的工單才能開始。當前狀態: " + currentStatus);
        }

        // 日期比較：確保實際開始日期不早於計劃開始日期
        if (actualStartDate.isBefore(workOrder.getScheduledStartDate())) {
            LOGGER.warn("工單ID {}: 實際開始日期 {} 早於計劃開始日期 {}", workOrderId, actualStartDate, workOrder.getScheduledStartDate());
            // 這裡可以選擇拋出 IllegalArgumentException 或僅記錄警告
            // throw new IllegalArgumentException("實際開始日期不能早於計劃開始日期。");
        }

        workOrder.setActualStartDate(actualStartDate);
        workOrder.setStatus("In Progress");
        // updateDate 會在 save 時被 @PreUpdate 自動更新

        LOGGER.info("Service: 嘗試開始工單，ID: {}", workOrderId);
        return Optional.of(workOrderRepository.save(workOrder));
    }

    @Override
    public Optional<WorkOrder> completeWorkOrder(int workOrderId, LocalDateTime actualCompletionDate) throws IllegalArgumentException, IllegalStateException {
        if (workOrderId <= 0) {
            throw new IllegalArgumentException("工單ID無效，必須大於0。");
        }
        if (actualCompletionDate == null) {
            throw new IllegalArgumentException("實際完成日期不能為空。");
        }

        Optional<WorkOrder> workOrderOptional = workOrderRepository.findById(workOrderId);
        if (workOrderOptional.isEmpty()) {
            throw new IllegalArgumentException("要完成的工單不存在。");
        }
        WorkOrder workOrder = workOrderOptional.get();

        // 業務規則：只有 In Progress 狀態的工單才能完成
        String currentStatus = workOrder.getStatus();
        if (!"In Progress".equalsIgnoreCase(currentStatus)) {
            throw new IllegalStateException("只有 '進行中' 狀態的工單才能完成。當前狀態: " + currentStatus);
        }

        // 確保實際完成日期不早於實際開始日期 (如果實際開始日期存在)
        if (workOrder.getActualStartDate() != null && actualCompletionDate.isBefore(workOrder.getActualStartDate())) {
            LOGGER.warn("工單ID {}: 實際完成日期 {} 早於實際開始日期 {}", workOrderId, actualCompletionDate, workOrder.getActualStartDate());
            throw new IllegalArgumentException("實際完成日期不能早於實際開始日期。");
        }

        workOrder.setActualCompletionDate(actualCompletionDate);
        workOrder.setStatus("Completed");
        // updateDate 會在 save 時被 @PreUpdate 自動更新

        LOGGER.info("Service: 嘗試完成工單，ID: {}", workOrderId);
        return Optional.of(workOrderRepository.save(workOrder));
    }
}

```



```
**`extends JpaRepository<Product, Integer>`:**

- `JpaRepository` 繼承自 `PagingAndSortingRepository` 和 `CrudRepository`。
    
- `CrudRepository` 已經提供了最常見的 CRUD（Create, Read, Update, Delete）操作的方法，例如：
    
    - `save(T entity)`: 用於新增或更新實體。
        
    - `findById(ID id)`: 根據 ID 查詢一個實體，返回 `Optional`。
        
    - `findAll()`: 查詢所有實體。
        
    - `deleteById(ID id)`: 根據 ID 刪除實體。
        
    - `count()`: 查詢實體總數。
        
    - `existsById(ID id)`: 判斷實體是否存在。
        
- `JpaRepository` 在 `CrudRepository` 的基礎上，增加了更多 JPA 特有的功能，例如批次操作、分頁和排序等。
    

**這意味著，對於基本的增刪改查，您不需要寫任何 SQL，甚至不需要寫任何方法實作！Spring Data JPA 會在運行時自動為您生成這些方法的實現，並執行對應的 SQL 語句。**
```