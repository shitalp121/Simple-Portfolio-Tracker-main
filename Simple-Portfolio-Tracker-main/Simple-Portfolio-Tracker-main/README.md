# **Simple Portfolio Tracker Application**

## **Project Overview:**
The Simple Portfolio Tracker Application allows users to manage their stock portfolio by performing CRUD operations (Create, Read, Update, Delete) on stock holdings. It tracks the total value of the portfolio based on real-time stock prices fetched from an external stock price API. Users can view portfolio metrics, such as total value, top-performing stocks, and distribution of their holdings.

---

### 1. **Frontend (React)**

**Tools and Libraries:**
- React (for building the user interface)
- React Router (for managing navigation)
- Axios (for making HTTP requests to the backend)
- TailwindCSS (for styling)
- Chart.js (for portfolio distribution and performance visualization)

**Components:**
1. **Dashboard**:
   - **Portfolio Metrics**: Displays total portfolio value, top-performing stock, and portfolio distribution (e.g., pie chart showing percentage distribution of each stock in the portfolio).
   - **Stocks Table**: A table that lists all stock holdings, showing stock name, ticker, quantity, buy price, current price, and portfolio value.
   - **Stock Form**: A form to add or edit stock details (e.g., stock name, ticker, quantity, buy price).

2. **Actions**:
   - **Add Stock**: A form that submits new stock details to the backend.
   - **Edit Stock**: Update stock details, including quantity or buy price.
   - **Delete Stock**: Option to delete a stock from the portfolio.

**UI Flow:**
- **Home Page**: Displays the portfolio dashboard and a list of stocks.
- **Add/Edit Stock Page**: Allows users to add or edit stock holdings.
- **Stock Details**: Optionally, clicking on a stock in the list can provide detailed information about that stock.

---

### 2. **Backend (Java with Spring Boot)**

**Tools and Libraries:**
- Spring Boot (for creating the REST API)
- Spring Data JPA (for database interaction)
- Hibernate (for ORM functionality)
- MySQL (database for storing stock data)

**API Endpoints:**
- **POST `/stocks`**: Adds a new stock to the portfolio.
  - Request Body: `{ "name": "Apple", "ticker": "AAPL", "quantity": 1, "buyPrice": 150.00 }`
  - Response: `{ "id": 1, "name": "Apple", "ticker": "AAPL", "quantity": 1, "buyPrice": 150.00 }`
  
- **GET `/stocks`**: Retrieves all stocks in the portfolio.
  - Response: `[ { "id": 1, "name": "Apple", "ticker": "AAPL", "quantity": 1, "buyPrice": 150.00 } ]`
  
- **PUT `/stocks/{id}`**: Updates an existing stock's details.
  - Request Body: `{ "quantity": 2, "buyPrice": 145.00 }`
  - Response: `{ "id": 1, "name": "Apple", "ticker": "AAPL", "quantity": 2, "buyPrice": 145.00 }`
  
- **DELETE `/stocks/{id}`**: Deletes a stock from the portfolio.
  - Response: `{ "message": "Stock deleted successfully" }`

- **GET `/portfolio/value`**: Fetches the total value of the portfolio based on real-time stock prices.
  - Response: `{ "totalValue": 2500.00 }`

**Database Schema:**
```sql
CREATE TABLE stocks (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(255) NOT NULL,
    ticker VARCHAR(10) NOT NULL,
    quantity INT NOT NULL,
    buy_price DECIMAL(10, 2) NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

**Stock Entity Class:**
```java
@Entity
public class Stock {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String name;
    private String ticker;
    private int quantity;
    private BigDecimal buyPrice;

    // Getters and Setters
}
```

---

### 3. **Real-Time Data Integration (Stock API)**

**API Integration:**
- Use a free API like **Alpha Vantage**, **Yahoo Finance**, or **Finnhub** to fetch real-time stock prices.
- Example of integration using **Alpha Vantage** API:
  
```java
@Service
public class StockPriceService {
    private static final String API_KEY = "your_alpha_vantage_api_key";
    private static final String API_URL = "https://www.alphavantage.co/query?function=TIME_SERIES_INTRADAY&symbol={ticker}&interval=5min&apikey=" + API_KEY;

    public BigDecimal getStockPrice(String ticker) {
        RestTemplate restTemplate = new RestTemplate();
        ResponseEntity<String> response = restTemplate.exchange(API_URL, HttpMethod.GET, null, String.class, ticker);
        // Parse the JSON response and return the stock price.
        return parsePrice(response.getBody());
    }
}
```

This service fetches the stock price for a given ticker and can be used in the backend to calculate the total portfolio value dynamically.

---

### 4. **Database (MySQL)**

Create a table for storing stock data as described above.

---

### 5. **Deployment**

- **Frontend Deployment**: Use platforms like **Vercel** or **Netlify** for deploying the React frontend.
  - Push the frontend code to GitHub, then link it to Vercel or Netlify for automatic deployment.
  
- **Backend Deployment**: Use platforms like **Heroku** or **AWS** for deploying the Spring Boot backend.
  - Push the backend code to GitHub and deploy using Heroku or AWS services.

---


### **Contributing**

We welcome contributions to enhance the platform! Please follow these steps to contribute:
1. Fork the repository.
2. Create a new branch:
   ```bash
   git checkout -b feature/your-feature
   ```
3. Make your changes and commit them:
   ```bash
   git commit -m "Add your message"
   ```
4. Push to the branch:
   ```bash
   git push origin feature/your-feature
   ```
5. Create a pull request.




