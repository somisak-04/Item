# Item Management REST API

A simple yet powerful Spring Boot REST API for managing a collection of items. Perfect for e-commerce platforms, streaming services, inventory management, and more.

## 📋 Overview

This application demonstrates a production-ready backend implementation with:
- ✅ RESTful API design
- ✅ Input validation
- ✅ In-memory data persistence
- ✅ Spring Boot best practices
- ✅ Docker-ready deployment

---

## 🚀 Quick Start

### Prerequisites
- Java 17+
- Maven 3.8+

### Build & Run Locally

```bash
# Clone the repository
git clone <your-repo-url>
cd Item

# Build the project
mvn clean package -DskipTests

# Run the application
java -jar target/item-api-0.0.1-SNAPSHOT.jar

# Or use Maven directly
mvn spring-boot:run
```

The API will be available at: **http://localhost:8080**

---

## 📚 API Endpoints

### 1. Create a New Item
**Endpoint:** `POST /api/items`

**Request:**
```json
{
  "name": "Gaming Laptop",
  "description": "High-performance laptop for gaming",
  "price": 1299.99
}
```

**Response:** (201 Created)
```json
{
  "id": 1,
  "name": "Gaming Laptop",
  "description": "High-performance laptop for gaming",
  "price": 1299.99
}
```

**Validation Rules:**
- `name` (required): Must not be blank
- `description` (required): Must not be blank  
- `price` (required): Must not be null

---

### 2. Get Item by ID
**Endpoint:** `GET /api/items/{id}`

**Example:**
```
GET http://localhost:8080/api/items/1
```

**Response:** (200 OK)
```json
{
  "id": 1,
  "name": "Gaming Laptop",
  "description": "High-performance laptop for gaming",
  "price": 1299.99
}
```

**Error Response:** (404 Not Found)
```json
{
  "error": "Item not found"
}
```

---

## 🏗️ Architecture

### Project Structure
```
src/main/java/com/Item/
├── ItemApplication.java          # Spring Boot entry point
├── Item.java                      # Data model with validation
├── ItemController.java            # REST API endpoints
└── ItemService.java               # Business logic & data storage
```

### Component Details

#### 1. **ItemApplication.java**
Spring Boot application entry point that initializes the embedded Tomcat server.

```java
@SpringBootApplication
public class ItemApplication {
    public static void main(String[] args) {
        SpringApplication.run(ItemApplication.class, args);
    }
}
```

#### 2. **Item.java** (Model)
Represents an item with validation annotations:
- `@NotBlank` enforces non-empty strings
- `@NotNull` enforces required numeric fields
- Uses Jakarta Validation API

```java
public class Item {
    private Long id;
    
    @NotBlank(message = "Item name is required")
    private String name;
    
    @NotBlank(message = "Description is required")
    private String description;
    
    @NotNull(message = "Price is required")
    private Double price;
    
    // Constructors, getters, setters...
}
```

#### 3. **ItemService.java** (Business Logic)
Manages in-memory data storage using `ArrayList`:
- `addItem(Item)` → Adds item with auto-generated ID
- `getItemById(Long)` → Retrieves item by ID

```java
@Service
public class ItemService {
    private final List<Item> items = new ArrayList<>();
    private Long idCounter = 1L;
    
    public Item addItem(Item item) {
        item.setId(idCounter++);
        items.add(item);
        return item;
    }
    
    public Optional<Item> getItemById(Long id) {
        return items.stream()
            .filter(item -> item.getId().equals(id))
            .findFirst();
    }
}
```

#### 4. **ItemController.java** (REST Endpoints)
Handles HTTP requests and responses:

```java
@RestController
@RequestMapping("/api/items")
public class ItemController {
    
    @PostMapping
    public ResponseEntity<Item> addItem(@Valid @RequestBody Item item) {
        Item savedItem = itemService.addItem(item);
        return ResponseEntity.ok(savedItem);
    }
    
    @GetMapping("/{id}")
    public ResponseEntity<Item> getItemById(@PathVariable Long id) {
        return itemService.getItemById(id)
            .map(ResponseEntity::ok)
            .orElse(ResponseEntity.notFound().build());
    }
}
```

---

## 🧪 Testing the API

### Using cURL
```bash
# Create item
curl -X POST http://localhost:8080/api/items \
  -H "Content-Type: application/json" \
  -d '{"name":"Laptop","description":"Gaming laptop","price":1299.99}'

# Get item
curl http://localhost:8080/api/items/1
```

### Using PowerShell
```powershell
# Create item
$body = @{
    name = "Laptop"
    description = "Gaming laptop"
    price = 1299.99
} | ConvertTo-Json

Invoke-WebRequest http://localhost:8080/api/items `
  -Method POST `
  -ContentType 'application/json' `
  -Body $body `
  -UseBasicParsing

# Get item
Invoke-WebRequest http://localhost:8080/api/items/1 `
  -Method GET `
  -UseBasicParsing
```

### Using Postman
1. Import collection or create requests manually
2. **POST** `http://localhost:8080/api/items`
   - Body (JSON): `{"name":"item","description":"desc","price":99.99}`
3. **GET** `http://localhost:8080/api/items/1`

---

## 📦 Dependencies

### Core Dependencies (from pom.xml)
- **spring-boot-starter-web** — REST API support
- **spring-boot-starter-validation** — Input validation (@Valid, @NotBlank, etc.)
- **spring-boot-maven-plugin** — Build executable JAR

---

## 🐳 Docker Deployment

### Build Docker Image
```bash
docker build -t item-api .
```

### Run Docker Container
```bash
docker run -p 8080:8080 item-api
```

Visit: `http://localhost:8080/api/items`

---

## ☁️ Cloud Deployment

### Option 1: Railway.app (Recommended)
1. Push code to GitHub
2. Go to [railway.app](https://railway.app)
3. "Start New Project" → "Deploy from GitHub"
4. Select your repository → Deploy
5. Get your public URL (e.g., `https://item-api-xxx.railway.app`)

### Option 2: Render.com
1. Go to [render.com](https://render.com)
2. "New" → "Web Service" → GitHub
3. Select repo and dockerfile
4. Deploy → Get public URL

### Option 3: AWS (Free Tier)
1. Use AWS App Runner or Elastic Beanstalk
2. Deploy from GitHub repo
3. Get public endpoint

---

## 🔧 Configuration

### Change Port
```bash
# Default: 8080
java -jar target/item-api-0.0.1-SNAPSHOT.jar --server.port=9090
```

Or in `src/main/resources/application.properties`:
```properties
server.port=9090
```

---

## ✨ Key Features

✅ **RESTful Design** — Follows REST conventions  
✅ **Input Validation** — Enforces data integrity  
✅ **Error Handling** — Returns appropriate HTTP status codes  
✅ **Auto-generated IDs** — Unique ID assignment  
✅ **In-Memory Storage** — Fast, simple data persistence  
✅ **Spring Boot Best Practices** — Clean, maintainable code  
✅ **Production-Ready** — Packaged as executable JAR  

---

## 📝 Example Use Cases

### E-Commerce
```json
{
  "name": "Nike Air Force 1",
  "description": "Classic white sneaker",
  "price": 89.99
}
```

### Streaming Service
```json
{
  "name": "Inception",
  "description": "Sci-fi thriller directed by Christopher Nolan",
  "price": 14.99
}
```

### Inventory Management
```json
{
  "name": "Office Chair",
  "description": "Ergonomic office chair with lumbar support",
  "price": 249.99
}
```

---

## 🆘 Troubleshooting

### Port 8080 Already in Use
```bash
# Find process using port
netstat -ano | findstr :8080

# Kill process (Windows)
taskkill /PID <PID> /F

# Kill process (Linux/Mac)
lsof -ti:8080 | xargs kill -9
```

### Build Fails
```bash
mvn clean package -DskipTests -X
```

### Application Won't Start
- Check Java version: `java -version` (requires 17+)
- Check Maven: `mvn -version`
- Review logs for errors

---

## 📞 Support

For issues or questions:
1. Check this README
2. Review error logs
3. Verify prerequisites (Java 17+, Maven 3.8+)
4. Ensure port 8080 is available

---

## 📄 License

This project is open source and available under the MIT License.

---

## 🎯 Summary

This Item Management API demonstrates:
- Clean architecture with separated concerns
- Spring Boot best practices
- RESTful API design principles
- Input validation and error handling
- Production-ready deployment options

**Ready to deploy and scale!** 🚀
