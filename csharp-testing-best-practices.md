# C# Testing Best Practices: A Comprehensive Guide

# Table of Contents

## Strategy and Workflow
- [✅ Do: Start with integration/component tests](#-do-start-with-integration-component-tests)
- [❌ Don't: Rely solely on unit tests](#-dont-rely-solely-on-unit-tests)
- [✅ Do: Run a very few E2E tests, consider unit tests selectively](#-do-run-a-very-few-e2e-tests-consider-unit-tests-selectively)
- [❌ Don't: Write tests after completing development](#-dont-write-tests-after-completing-development)
- [✅ Do: Cover features, not functions](#-do-cover-features-not-functions)
- [✅ Do: Write tests during coding, never after](#-do-write-tests-during-coding-never-after)
- [✅ Do: Test the five known backend outcomes](#-do-test-the-five-known-backend-outcomes)

## Infrastructure and Database Setup
- [✅ Do: Use TestContainers to host databases and infrastructure](#-do-use-testcontainers-to-host-databases-and-infrastructure)
- [❌ Don't: Use SQLite or in-memory DB engines to replace real databases](#-dont-use-sqlite-or-in-memory-db-engines-to-replace-real-databases)
- [✅ Do: Start containers using assembly initialization](#-do-start-containers-using-assembly-initialization)
- [✅ Do: Optimize your real DB for testing, don't fake it](#-do-optimize-your-real-db-for-testing-dont-fake-it)
- [✅ Do: Use in-memory storage where possible](#-do-use-in-memory-storage-where-possible)
- [✅ Do: Build the DB schema using migrations](#-do-build-the-db-schema-using-migrations)
- [❌ Don't: Rely on snapshots for assertions](#-dont-rely-on-snapshots-for-assertions)
- [❌ Don't: Use implementation mocks](#-dont-use-implementation-mocks)
- [❌ Don't: Share test data between tests](#-dont-share-test-data-between-tests)
- [❌ Don't: Check data directly in the database](#-dont-check-data-directly-in-the-database)
- [❌ Don't: Rely on manual DB setup](#-dont-rely-on-manual-db-setup)
- [❌ Don't: Test against remote environments](#-dont-test-against-remote-environments)
- [❌ Don't: Hardcode ports in tests](#-dont-hardcode-ports-in-tests)
- [✅ Do: Reuse containers in development but restart in CI](#-do-reuse-containers-in-development-but-restart-in-ci)

## Web Server Setup
- [✅ Do: Keep tests and app under test in the same process](#-do-keep-tests-and-app-under-test-in-the-same-process)
- [✅ Do: Let tests control when servers start and stop](#-do-let-tests-control-when-servers-start-and-stop)
- [✅ Do: Use dynamic ports in testing](#-do-use-dynamic-ports-in-testing)

## The Test Anatomy
- [✅ Do: Follow unit testing best practices](#-do-follow-unit-testing-best-practices)
- [✅ Do: Provide real credentials or tokens, if possible](#-do-provide-real-credentials-or-tokens-if-possible)
- [✅ Do: Assert on the entire HTTP response](#-do-assert-on-the-entire-http-response)
- [✅ Do: Structure tests by routes and stories](#-do-structure-tests-by-routes-and-stories)
- [✅ Do: Test the five potential outcomes](#-do-test-the-five-potential-outcomes)

## Integrations
- [✅ Do: Isolate the component using HTTP interceptors](#-do-isolate-the-component-using-http-interceptors)
- [✅ Do: Define default responses before each test](#-do-define-default-responses-before-each-test)
- [✅ Do: Override happy paths with corner cases](#-do-override-happy-paths-with-corner-cases)
- [✅ Do: Deny unexpected outgoing requests](#-do-deny-unexpected-outgoing-requests)
- [✅ Do: Simulate network chaos](#-do-simulate-network-chaos)
- [✅ Do: Verify outgoing request validity](#-do-verify-outgoing-request-validity-1)
- [✅ Do: Use API contract testing](#-do-use-api-contract-testing)
- [❌ Don't: Allow unexpected outgoing calls](#-dont-allow-unexpected-outgoing-calls)
- [✅ Do: Record real outgoing requests for awareness](#-do-record-real-outgoing-requests-for-awareness)

## Dealing with Data
- [✅ Do: Make each test work with its own data](#-do-make-each-test-work-with-its-own-data)
- [✅ Do: Seed metadata and context data appropriately](#-do-seed-metadata-and-context-data-appropriately)
- [✅ Do: Assert data state using public APIs](#-do-assert-data-state-using-public-apis)
- [✅ Do: Use a clear data cleanup strategy](#-do-use-a-clear-data-cleanup-strategy)
- [✅ Do: Add randomness to unique fields](#-do-add-randomness-to-unique-fields)
- [✅ Do: Test response schema for dynamic data](#-do-test-response-schema-for-dynamic-data)
- [✅ Do: Test for unintended side effects](#-do-test-for-unintended-side-effects)

## Message Queues
- [✅ Do: Use in-memory fakes for most message queue tests](#-do-use-in-memory-fakes-for-most-message-queue-tests)
- [✅ Do: Use async/await patterns for message queue tests](#-do-use-asyncawait-patterns-for-message-queue-tests)
- [✅ Do: Test message acknowledgment](#-do-test-message-acknowledgment)
- [✅ Do: Test processing of message batches](#-do-test-processing-of-message-batches)
- [✅ Do: Test for poisoned messages](#-do-test-for-poisoned-messages)
- [✅ Do: Test for idempotency](#-do-test-for-idempotency)
- [✅ Do: Test connection failures](#-do-test-connection-failures)
- [✅ Do: Include some E2E tests with real message queues](#-do-include-some-e2e-tests-with-real-message-queues)
- [❌ Don't: Use real message queues for all tests](#-dont-use-real-message-queues-for-all-tests)
- [❌ Don't: Use polling in tests](#-dont-use-polling-in-tests)

## Mocking
- [✅ Do: Understand the different types of mocks](#-do-understand-the-different-types-of-mocks)
- [✅ Do: Keep mocks visible in test files](#-do-keep-mocks-visible-in-test-files)
- [❌ Don't: Hide mocks in external files](#-dont-hide-mocks-in-external-files)
- [✅ Do: Be careful with partial mocks](#-do-be-careful-with-partial-mocks)
- [✅ Do: Clean up all mocks between tests](#-do-clean-up-all-mocks-between-tests)
- [❌ Don't: Leave mocks uncleaned](#-dont-leave-mocks-uncleaned)
- [✅ Do: Choose the right mocking mechanism](#-do-choose-the-right-mocking-mechanism)
- [✅ Do: Ensure mocks are type-safe](#-do-ensure-mocks-are-type-safe)


## Strategy and Workflow

### ✅ Do: Start with integration/component tests

Begin with component tests that test an entire component through its API with all layers included (database, services, etc.). These tests deliver high confidence with good developer experience.

When you start developing, you first know the expected behavior of your component. It's natural to begin testing this public interface. Component tests provide enormous value: they're realistic, require less effort than unit testing every function, minimize mocking, trigger high coverage, and keep you focused on what matters.

**Conceptual Example:**
```csharp
using Microsoft.AspNetCore.Mvc.Testing;
using Microsoft.VisualStudio.TestTools.UnitTesting;
using System.Net.Http.Json;
using System.Threading.Tasks;

[TestClass]
public class OrderApiTests
{
    private WebApplicationFactory<Program> _factory;
    private HttpClient _client;
    
    [TestInitialize]
    public void Initialize()
    {
        // Set up the test server with the full application
        _factory = new WebApplicationFactory<Program>();
        _client = _factory.CreateClient();
    }
    
    [TestMethod]
    public async Task WhenRequestingExistingOrder_ThenRetrieveSuccessfully()
    {
        // Arrange
        var orderToAdd = new OrderDto
        {
            UserId = 1,
            ProductId = 2,
            Mode = "approved"
        };
        
        var postResponse = await _client.PostAsJsonAsync("/api/orders", orderToAdd);
        var createdOrder = await postResponse.Content.ReadFromJsonAsync<OrderResponseDto>();
        
        // Act
        var getResponse = await _client.GetAsync($"/api/orders/{createdOrder.Id}");
        var retrievedOrder = await getResponse.Content.ReadFromJsonAsync<OrderResponseDto>();
        
        // Assert
        Assert.IsTrue(getResponse.IsSuccessStatusCode);
        Assert.AreEqual(orderToAdd.UserId, retrievedOrder.UserId);
        Assert.AreEqual(orderToAdd.ProductId, retrievedOrder.ProductId);
        Assert.AreEqual(orderToAdd.Mode, retrievedOrder.Mode);
    }
    
    [TestCleanup]
    public void Cleanup()
    {
        _client.Dispose();
        _factory.Dispose();
    }
}
```

**Best Practice Example:**
```csharp
using DotNet.Testcontainers.Containers;
using Microsoft.AspNetCore.Mvc.Testing;
using Microsoft.EntityFrameworkCore;
using Microsoft.Extensions.DependencyInjection;
using Microsoft.VisualStudio.TestTools.UnitTesting;
using System.Net;
using System.Net.Http.Json;
using System.Threading.Tasks;

[TestClass]
public class OrderApiTests : IAsyncDisposable // IAsyncDisposable allows us to clean up resources asynchronously
{
    private readonly TestcontainersContainer _dbContainer; // Creates an isolated database just for our tests
    private readonly WebApplicationFactory<Program> _factory;
    private readonly HttpClient _client;
    
    public OrderApiTests()
    {
        // Constructor runs once when test class is created - we configure but don't start containers here
        // We use TestContainers to create an isolated database that won't affect our real database
        _dbContainer = new TestcontainersBuilder<PostgreSqlTestcontainer>()
            .WithDatabase(new PostgreSqlTestcontainerConfiguration
            {
                Database = "testdb",
                Username = "test",
                Password = "test"
            })
            .Build();
    }
    
    [TestInitialize]
    public async Task Initialize()
    {
        // TestInitialize runs before EACH test method - we start fresh services here for each test
        // Start the database container
        await _dbContainer.StartAsync();
        
        // Configure the application to use the test container instead of the real database
        _factory = new WebApplicationFactory<Program>()
            .WithWebHostBuilder(builder =>
            {
                builder.ConfigureServices(services =>
                {
                    // This replaces the real database connection with our test container
                    var descriptor = services.SingleOrDefault(
                        d => d.ServiceType == typeof(DbContextOptions<ApplicationDbContext>));
                    
                    if (descriptor != null)
                    {
                        services.Remove(descriptor);
                    }
                    
                    services.AddDbContext<ApplicationDbContext>(options =>
                    {
                        options.UseNpgsql(_dbContainer.ConnectionString);
                    });
                    
                    // Ensure database is created with the right schema before tests run
                    using var scope = services.BuildServiceProvider().CreateScope();
                    var dbContext = scope.ServiceProvider.GetRequiredService<ApplicationDbContext>();
                    dbContext.Database.EnsureCreated();
                });
            });
            
        _client = _factory.CreateClient(); // Creates an HTTP client that can make requests to our API
    }
    
    [TestMethod]
    public async Task GetOrder_WhenOrderExists_ReturnsOrderWithCorrectData()
    {
        // Arrange: Set up the test by creating an order in the database through the API
        var orderToCreate = new OrderDto
        {
            UserId = 42,
            ProductId = 101,
            Quantity = 3,
            Status = "Pending"
        };
        
        var createResponse = await _client.PostAsJsonAsync("/api/orders", orderToCreate);
        createResponse.EnsureSuccessStatusCode(); // This will throw if the API returns an error
        var createdOrder = await createResponse.Content.ReadFromJsonAsync<OrderResponseDto>();
        Assert.IsNotNull(createdOrder);
        
        // Act: Perform the action we want to test - getting the order by ID
        var getResponse = await _client.GetAsync($"/api/orders/{createdOrder.Id}");
        
        // Assert: Verify the results match what we expect
        getResponse.EnsureSuccessStatusCode();
        Assert.AreEqual(HttpStatusCode.OK, getResponse.StatusCode);
        
        var retrievedOrder = await getResponse.Content.ReadFromJsonAsync<OrderResponseDto>();
        Assert.IsNotNull(retrievedOrder);
        Assert.AreEqual(createdOrder.Id, retrievedOrder.Id);
        Assert.AreEqual(orderToCreate.UserId, retrievedOrder.UserId);
        Assert.AreEqual(orderToCreate.ProductId, retrievedOrder.ProductId);
        Assert.AreEqual(orderToCreate.Quantity, retrievedOrder.Quantity);
        Assert.AreEqual(orderToCreate.Status, retrievedOrder.Status);
    }
    
    [TestMethod]
    public async Task GetOrder_WhenOrderDoesNotExist_ReturnsNotFound()
    {
        // This test checks the error case - what happens when we request an order that doesn't exist
        // Arrange: Create a non-existent order ID
        var nonExistentOrderId = 9999;
        
        // Act: Try to get an order that doesn't exist
        var response = await _client.GetAsync($"/api/orders/{nonExistentOrderId}");
        
        // Assert: Verify we get a 404 Not Found response
        Assert.AreEqual(HttpStatusCode.NotFound, response.StatusCode);
    }
    
    public async ValueTask DisposeAsync()
    {
        // DisposeAsync runs at the end to clean up all resources - important to prevent memory leaks
        _client?.Dispose();
        _factory?.Dispose();
        
        if (_dbContainer != null)
        {
            await _dbContainer.StopAsync(); // Stop the container when we're done
            await _dbContainer.DisposeAsync(); // Release all resources
        }
    }
}
```

**Best Practice Explained**

1. What is TestContainers and why are we using it?
2. Why are we creating a database in the test?
3. What's this IAsyncDisposable interface?
4. What's the difference between TestInitialize and the constructor?

This test is showing how to test our Order API from end-to-end. Instead of testing small pieces of code, we're testing the whole system at once - from the API endpoint all the way to the database.
The cool thing is we're using a real database (through TestContainers), but it's isolated just for our tests. This means:

* Our tests won't mess up the real database
* Each test run starts with a clean database
* The tests are more reliable and realistic

I like how the test follows a clear pattern:

Arrange: We set up the test by creating an order

Act: We perform the action we want to test (getting the order)

Assert: We verify the results are what we expect

The second test is great because it shows what happens when things go wrong (order doesn't exist). This helps me understand both the happy path and error cases.
I see that at the end we clean up everything with DisposeAsync - this is important so we don't leave test containers running after tests finish.

The constructor and TestInitialize method serve different purposes in the test lifecycle:

Constructor: This runs only once when the test class is created. In our example, we use it to configure our database container but we don't actually start it yet. Think of this as "defining what we need" before any tests run.
TestInitialize: This method runs before each individual test method. In our code, this is where we actually start the database container and set up the web application with the right configuration. This ensures each test starts with a fresh, properly configured environment.

This separation is important because:

If we put everything in the constructor, we'd have no way to use async/await (constructors can't be async)
Having the setup in TestInitialize ensures each test starts with the same clean state
It follows the MSTest lifecycle: create test class once, but run initialization before each test method

This is a common pattern in testing frameworks that helps maintain test isolation, which is crucial for reliable tests.

### ❌ Don't: Rely solely on unit tests

While unit tests are fast and focused, they're not enough to build confidence in your system. They typically mock dependencies, making them less realistic and potentially missing integration problems. Use them selectively for complex algorithms and business logic, not as your primary testing strategy.

### ✅ Do: Run a very few E2E tests, consider unit tests selectively

Write just a small number of E2E tests (3-10) on top of your component tests. E2E tests run against real infrastructure with multiple components integrated. Since component tests catch most bugs, use E2E tests to focus on different risks: configuration issues, integration problems with third-party services, and infrastructure surprises.

Write unit tests only when dealing with non-trivial logic or algorithms. If a module has complex business logic, isolating it can make testing easier.

**Example:**
```csharp
[TestClass]
public class OrderSystemE2ETests
{
    private static TestcontainersContainer _dbContainer;
    private static WebApplicationFactory<Program> _factory;
    private static HttpClient _client;
    
    [ClassInitialize]
    public static async Task InitializeClass(TestContext context)
    {
        // Start real infrastructure
        _dbContainer = new ContainerBuilder()
            .WithImage("postgres:13")
            .WithEnvironment("POSTGRES_PASSWORD", "postgres")
            .WithPortBinding(5432, true)
            .Build();
            
        await _dbContainer.StartAsync();
        
        // Configure the application to use the real database
        Environment.SetEnvironmentVariable("ConnectionStrings__DefaultConnection", 
            $"Host=localhost;Port={_dbContainer.GetMappedPublicPort(5432)};Database=postgres;Username=postgres;Password=postgres");
            
        _factory = new WebApplicationFactory<Program>();
        _client = _factory.CreateClient();
    }
    
    [TestMethod]
    public async Task EndToEndOrderCreationAndFulfillment()
    {
        // A complex test that exercises multiple components
        // Create user -> Login -> Create order -> Process payment -> Check inventory -> Ship order
        
        // This test ensures the entire business process works end-to-end
        // Only a few tests like this are needed
    }
    
    [ClassCleanup]
    public static async Task CleanupClass()
    {
        _client.Dispose();
        _factory.Dispose();
        await _dbContainer.StopAsync();
    }
}

// Example of when unit testing is appropriate - complex algorithm
[TestClass]
public class PricingEngineTests
{
    [TestMethod]
    public void CalculateDiscount_WithComplexRules_AppliesCorrectly()
    {
        // Unit test for complex pricing logic
        var pricingEngine = new PricingEngine();
        
        var result = pricingEngine.CalculateDiscount(
            basePrice: 100.0m,
            quantity: 5,
            isVipCustomer: true,
            hasSeasonalPromotion: true,
            daysUntilExpiration: 10
        );
        
        Assert.AreEqual(70.0m, result); // Expected result after complex discount calculation
    }
}
```

**Best Practice Example**

```csharp
using DotNet.Testcontainers.Containers;
using Microsoft.AspNetCore.Mvc.Testing;
using Microsoft.VisualStudio.TestTools.UnitTesting;
using System.Net.Http.Json;
using System.Threading.Tasks;

[TestClass]
public class OrderSystemE2ETests : IAsyncLifetime // IAsyncLifetime provides async setup/teardown methods
{
    private TestcontainersContainer _dbContainer;
    private PaymentGatewayContainer _paymentContainer; // Simulates a payment gateway
    private WebApplicationFactory<Program> _factory;
    private HttpClient _client;
    
    // Setup runs once before all tests in this class
    public async Task InitializeAsync()
    {
        // Start real infrastructure - using a container network allows services to communicate
        var network = new NetworkBuilder().Build();
        
        // Setup database with retry logic for reliability
        _dbContainer = new PostgreSqlBuilder()
            .WithDatabase("orders")
            .WithUsername("testuser")
            .WithPassword("testpass")
            .WithNetwork(network)
            .WithNetworkAliases("database") // Services can reach this using this hostname
            .WithWaitStrategy(Wait.ForUnixContainer().UntilPortIsAvailable(5432))
            .Build();
        
        // Setup mock payment gateway for true E2E testing
        _paymentContainer = new PaymentGatewayBuilder()
            .WithNetwork(network)
            .WithNetworkAliases("payment-service")
            .Build();
            
        try
        {
            // Start all infrastructure in parallel for efficiency
            await Task.WhenAll(
                _dbContainer.StartAsync(),
                _paymentContainer.StartAsync()
            );
            
            // Configure the application using environment variables in isolated test host
            _factory = new WebApplicationFactory<Program>()
                .WithWebHostBuilder(builder =>
                {
                    builder.ConfigureAppConfiguration((context, config) =>
                    {
                        // Override configuration instead of environment variables
                        config.AddInMemoryCollection(new Dictionary<string, string>
                        {
                            ["ConnectionStrings:DefaultConnection"] = _dbContainer.GetConnectionString(),
                            ["Services:PaymentGateway:Url"] = _paymentContainer.GetApiUrl()
                        });
                    });
                });
                
            _client = _factory.CreateClient(new WebApplicationFactoryClientOptions
            {
                AllowAutoRedirect = false // Important for testing authentication flows
            });
        }
        catch (Exception ex)
        {
            // Proper cleanup if setup fails
            await CleanupAsync();
            throw new Exception("E2E test setup failed", ex);
        }
    }
    
    [TestMethod]
    public async Task CompleteOrderFlow_HappyPath_OrderSuccessfullyFulfilled()
    {
        // Arrange: Create a test user and authenticate
        var user = await CreateTestUserAsync(); // Create a test user in the system
        var authResponse = await AuthenticateUserAsync(user.Email, "TestPassword123!"); // Get auth token
        _client.DefaultRequestHeaders.Authorization = 
            new AuthenticationHeaderValue("Bearer", authResponse.Token);
        
        // Create a product that's in stock
        var product = await CreateTestProductAsync(stockQuantity: 10, price: 29.99m);
        
        // Act: Place an order through the API
        var orderRequest = new CreateOrderRequest
        {
            ProductId = product.Id,
            Quantity = 2,
            ShippingAddress = new AddressDto
            {
                Street = "123 Test St",
                City = "Testville",
                ZipCode = "12345"
            },
            PaymentDetails = new PaymentDetailsDto
            {
                CardNumber = "4111111111111111", // Test card number
                ExpiryMonth = 12,
                ExpiryYear = 2030,
                Cvv = "123"
            }
        };
        
        var orderResponse = await _client.PostAsJsonAsync("/api/orders", orderRequest);
        
        // Assert: Verify the entire flow succeeded
        orderResponse.EnsureSuccessStatusCode();
        var order = await orderResponse.Content.ReadFromJsonAsync<OrderDto>();
        Assert.IsNotNull(order);
        
        // Check order status after processing (may need to wait or poll)
        await WaitForOrderStatus(order.Id, "Processed", timeoutSeconds: 30);
        
        // Verify inventory was updated
        var updatedProduct = await _client.GetFromJsonAsync<ProductDto>($"/api/products/{product.Id}");
        Assert.AreEqual(8, updatedProduct.StockQuantity); // 10 initial - 2 ordered
        
        // Verify payment was processed (check the payment service logs/status)
        var paymentStatus = await _client.GetFromJsonAsync<PaymentStatusDto>($"/api/payments/{order.PaymentId}");
        Assert.AreEqual("Completed", paymentStatus.Status);
    }
    
    [TestMethod]
    public async Task OrderWithInsufficientInventory_ReturnsAppropriateError()
    {
        // This E2E test focuses on a different risk: the inventory constraint
        // Arrange: Setup product with limited stock
        var user = await CreateAndAuthenticateUserAsync();
        var product = await CreateTestProductAsync(stockQuantity: 1, price: 29.99m);
        
        // Act: Try to order more than available
        var orderRequest = new CreateOrderRequest
        {
            ProductId = product.Id,
            Quantity = 5, // More than available
            ShippingAddress = GetTestAddress(),
            PaymentDetails = GetTestPaymentDetails()
        };
        
        var orderResponse = await _client.PostAsJsonAsync("/api/orders", orderRequest);
        
        // Assert: Verify we get the correct error
        Assert.AreEqual(HttpStatusCode.BadRequest, orderResponse.StatusCode);
        var error = await orderResponse.Content.ReadFromJsonAsync<ErrorResponse>();
        Assert.IsTrue(error.Message.Contains("insufficient inventory"));
        
        // Verify no payment was processed
        var paymentsResponse = await _client.GetAsync("/api/payments");
        paymentsResponse.EnsureSuccessStatusCode();
        var payments = await paymentsResponse.Content.ReadFromJsonAsync<List<PaymentDto>>();
        Assert.IsFalse(payments.Any(p => p.ProductId == product.Id));
    }
    
    // Helper methods would be implemented here
    private async Task<UserDto> CreateTestUserAsync() => /* implementation */;
    private async Task<AuthResponseDto> AuthenticateUserAsync(string email, string password) => /* implementation */;
    private async Task<ProductDto> CreateTestProductAsync(int stockQuantity, decimal price) => /* implementation */;
    private async Task WaitForOrderStatus(int orderId, string expectedStatus, int timeoutSeconds) => /* implementation */;
    private AddressDto GetTestAddress() => /* implementation */;
    private PaymentDetailsDto GetTestPaymentDetails() => /* implementation */;
    
    // Cleanup runs once after all tests in this class
    public async Task CleanupAsync()
    {
        _client?.Dispose();
        _factory?.Dispose();
        
        // Clean up containers
        var cleanupTasks = new List<Task>();
        if (_dbContainer != null)
            cleanupTasks.Add(_dbContainer.StopAsync());
        if (_paymentContainer != null)
            cleanupTasks.Add(_paymentContainer.StopAsync());
            
        await Task.WhenAll(cleanupTasks);
    }
}

// Unit test for complex logic that's easier to test in isolation
[TestClass]
public class PricingEngineTests
{
    [TestMethod]
    [DataRow(100.0, 5, true, true, 10, 70.0, DisplayName = "VIP customer with seasonal promo and bulk discount")]
    [DataRow(100.0, 1, true, false, 10, 90.0, DisplayName = "VIP customer only")]
    [DataRow(100.0, 10, false, true, 5, 75.0, DisplayName = "Non-VIP with bulk discount and nearing expiration")]
    [DataRow(100.0, 1, false, false, 30, 100.0, DisplayName = "No discounts apply")]
    public void CalculateDiscount_WithVariousScenarios_ReturnsExpectedPrice(
        decimal basePrice, int quantity, bool isVipCustomer, 
        bool hasSeasonalPromotion, int daysUntilExpiration, decimal expectedResult)
    {
        // Arrange: Create the pricing engine and prepare inputs
        var pricingEngine = new PricingEngine();
        
        // Act: Calculate the discounted price
        var actualResult = pricingEngine.CalculateDiscount(
            basePrice,
            quantity,
            isVipCustomer,
            hasSeasonalPromotion,
            daysUntilExpiration
        );
        
        // Assert: Verify the calculation is correct
        Assert.AreEqual(expectedResult, actualResult, 0.001m, 
            $"Failed for scenario: {TestContext.CurrentContext.Test.Properties["DisplayName"]}");
    }
    
    [TestMethod]
    public void CalculateDiscount_WithNegativeBasePrice_ThrowsArgumentException()
    {
        // Arrange
        var pricingEngine = new PricingEngine();
        
        // Act & Assert: Verify invalid inputs are handled appropriately
        Assert.ThrowsException<ArgumentException>(() =>
            pricingEngine.CalculateDiscount(
                basePrice: -50.0m, 
                quantity: 1,
                isVipCustomer: false,
                hasSeasonalPromotion: false,
                daysUntilExpiration: 30
            )
        );
    }
}
```

**Best Practice Explained**


E2E Tests vs. Component Tests:

* E2E tests are more comprehensive but fewer in number (3-10 total)
* They focus on risks like configuration issues and third-party integrations
* They run against real infrastructure (that's why we're using containers)

The IAsyncLifetime Interface:

* This looks neat! It gives us async setup/teardown methods instead of the regular synchronous ones
* We can start containers and wait for them to be ready before running tests

Testing Multiple Components Together:

* We're setting up both a database AND a payment gateway!
* The containers are placed on the same network so they can talk to each other
* This simulates a real production environment

When To Write Unit Tests:

* Only when there's complex business logic (like the pricing engine)
* The unit test uses DataRow attributes to test multiple scenarios easily
* We test both normal cases AND error cases (like negative prices)

Error Paths in E2E Tests:

* We're not just testing when everything works - we also test failure scenarios
* The "insufficient inventory" test shows what happens when something goes wrong
* This helps ensure our system handles errors gracefully

### ❌ Don't: Write tests after completing development

Writing tests after development means losing the safety net they provide. By the time all development is done, bugs may have crept in that could have been caught earlier.

### ✅ Do: Cover features, not functions

Focus your tests on features (business capabilities) rather than individual functions. Features represent the core behavior of your application—often reflected in API routes—and testing them ensures you're focusing on what matters to users.

This approach helps set the right priorities and often requires fewer tests than function-level testing.

**Example:**
```csharp
// Testing a feature - order placement
[TestClass]
public class OrderPlacementTests
{
    private WebApplicationFactory<Program> _factory;
    private HttpClient _client;
    
    [TestInitialize]
    public void Initialize()
    {
        _factory = new WebApplicationFactory<Program>();
        _client = _factory.CreateClient();
    }
    
    [TestMethod]
    public async Task PlaceOrder_WithValidData_OrderIsCreated()
    {
        // Test that covers the full order placement feature
        // A single feature test exercises multiple functions in the system
        var orderData = new OrderRequest
        {
            ProductId = 123,
            Quantity = 2,
            ShippingAddress = "123 Test St"
        };
        
        var response = await _client.PostAsJsonAsync("/api/orders", orderData);
        response.EnsureSuccessStatusCode();
        
        var order = await response.Content.ReadFromJsonAsync<OrderResponseDto>();
        Assert.IsNotNull(order);
        Assert.AreEqual(OrderStatus.Pending, order.Status);
    }
    
    [TestCleanup]
    public void Cleanup()
    {
        _client.Dispose();
        _factory.Dispose();
    }
}
```

### ✅ Do: Write tests during coding, never after

Write tests when it's most convenient—before or during coding—but never after everything is built. The earlier you write tests, the more they protect against regressions.

When you write tests as you go, they act like a safety net, catching issues immediately. Tests should run frequently, validating that recent changes haven't broken anything.

The sweet spot is when both requirements and implementation are clear enough. For some modules, clarity comes before writing code; for others, it follows experimentation.

**Example:**
```csharp
// First, define your test which acts as a specification
[TestMethod]
public async Task CreateOrder_WithValidData_ReturnsCreatedOrder()
{
    // Arrange
    var orderData = new OrderRequest { ProductId = 1, Quantity = 2 };
    
    // Act - This will fail initially because the endpoint doesn't exist yet
    var response = await _client.PostAsJsonAsync("/api/orders", orderData);
    
    // Assert - Define what we expect
    response.EnsureSuccessStatusCode();
    var order = await response.Content.ReadFromJsonAsync<OrderResponseDto>();
    Assert.IsNotNull(order.Id);
    Assert.AreEqual(orderData.ProductId, order.ProductId);
    Assert.AreEqual(orderData.Quantity, order.Quantity);
}

// Then implement the controller to make the test pass
public class OrdersController : ControllerBase
{
    private readonly IOrderService _orderService;
    
    public OrdersController(IOrderService orderService)
    {
        _orderService = orderService;
    }
    
    [HttpPost]
    public async Task<ActionResult<OrderResponseDto>> CreateOrder(OrderRequest request)
    {
        var order = await _orderService.CreateOrderAsync(request);
        return CreatedAtAction(nameof(GetOrder), new { id = order.Id }, order);
    }
    
    // Implement other actions...
}
```

### ✅ Do: Test the five known backend outcomes

When planning your tests, cover the five typical backend outcomes:

1. **HTTP Response** - Check that API calls return the correct status codes and data
2. **State Changes** - Verify that data is properly stored or modified
3. **External Calls** - Test interactions with external services (email, payment providers, etc.)
4. **Message Queue Events** - Ensure messages are properly published to queues
5. **Observability** - Verify that errors and events are properly logged/monitored

**Example:**
```csharp
[TestClass]
public class OrderProcessingTests
{
    private WebApplicationFactory<Program> _factory;
    private HttpClient _client;
    private WireMockServer _mockEmailServer;
    private FakeMessageQueue _messageQueue;
    private List<LogEntry> _capturedLogs;
    
    [TestInitialize]
    public void Initialize()
    {
        _capturedLogs = new List<LogEntry>();
        
        // Set up mock logger
        var loggerFactory = A.Fake<ILoggerFactory>();
        var logger = A.Fake<ILogger<OrderProcessor>>();
        A.CallTo(() => loggerFactory.CreateLogger(A<string>._))
            .Returns(logger);
        A.CallTo(() => logger.Log(
            A<LogLevel>._, 
            A<EventId>._, 
            A<It.IsAnyType>._, 
            A<Exception>._, 
            A<Func<It.IsAnyType, Exception, string>>._))
            .Invokes((LogLevel level, EventId eventId, object state, Exception ex, Func<object, Exception, string> formatter) => 
            {
                _capturedLogs.Add(new LogEntry { Level = level, Message = formatter(state, ex) });
            });
            
        // Set up fake message queue
        _messageQueue = new FakeMessageQueue();
        
        // Set up mock external service
        _mockEmailServer = WireMockServer.Start();
        
        // Configure the application with test dependencies
        _factory = new WebApplicationFactory<Program>()
            .WithWebHostBuilder(builder =>
            {
                builder.ConfigureServices(services =>
                {
                    services.AddSingleton<ILoggerFactory>(loggerFactory);
                    services.AddSingleton<IMessageQueue>(_messageQueue);
                    services.Configure<ExternalServicesOptions>(opts => 
                    {
                        opts.EmailApiUrl = _mockEmailServer.Urls[0];
                    });
                });
            });
            
        _client = _factory.CreateClient();
        
        // Set up email API mock response
        _mockEmailServer
            .Given(Request.Create().WithPath("/api/email").UsingPost())
            .RespondWith(Response.Create().WithStatusCode(200));
    }
    
    [TestMethod]
    public async Task ProcessOrder_VerifyAllFiveOutcomes()
    {
        // Arrange
        var order = new OrderRequest { ProductId = 1, Quantity = 1 };
        
        // Act
        var response = await _client.PostAsJsonAsync("/api/orders", order);
        
        // Assert - 1. Response
        response.EnsureSuccessStatusCode();
        var createdOrder = await response.Content.ReadFromJsonAsync<OrderResponseDto>();
        Assert.IsNotNull(createdOrder.Id);
        
        // Assert - 2. State Change
        var getResponse = await _client.GetAsync($"/api/orders/{createdOrder.Id}");
        getResponse.EnsureSuccessStatusCode();
        var retrievedOrder = await getResponse.Content.ReadFromJsonAsync<OrderResponseDto>();
        Assert.AreEqual(createdOrder.Id, retrievedOrder.Id);
        
        // Assert - 3. External Calls
        _mockEmailServer.VerifyThat(Request.Create().WithPath("/api/email").UsingPost());
        
        // Assert - 4. Message Queue Events
        Assert.IsTrue(_messageQueue.Messages.Any(m => 
            m.Type == "OrderCreated" && 
            m.Content.Contains(createdOrder.Id.ToString())));
            
        // Assert - 5. Observability
        Assert.IsTrue(_capturedLogs.Any(l => 
            l.Level == LogLevel.Information && 
            l.Message.Contains($"Order {createdOrder.Id} created successfully")));
    }
    
    [TestCleanup]
    public void Cleanup()
    {
        _client.Dispose();
        _factory.Dispose();
        _mockEmailServer.Dispose();
    }
    
    private class LogEntry
    {
        public LogLevel Level { get; set; }
        public string Message { get; set; }
    }
}
```

## Infrastructure and Database Setup

### ✅ Do: Use TestContainers to host databases and infrastructure

For integration tests, use TestContainers to run real databases and infrastructure services in containers. This gives you realistic testing with the same database engine you use in production, while keeping tests isolated and repeatable.

**Example:**
```csharp
using Microsoft.VisualStudio.TestTools.UnitTesting;
using DotNet.Testcontainers.Containers;
using DotNet.Testcontainers.Builders;
using System.Threading.Tasks;

[TestClass]
public class DatabaseTests
{
    private static TestcontainersContainer _dbContainer;
    private static string _connectionString;
    
    [ClassInitialize]
    public static async Task Initialize(TestContext context)
    {
        // Define and start a PostgreSQL container
        _dbContainer = new ContainerBuilder()
            .WithImage("postgres:13")
            .WithEnvironment("POSTGRES_PASSWORD", "postgres")
            .WithPortBinding(5432, true) // Random host port
            .WithWaitStrategy(Wait.ForUnixContainer().UntilPortIsAvailable(5432))
            .Build();
            
        await _dbContainer.StartAsync();
        
        // Get the mapped port and build connection string
        int mappedPort = _dbContainer.GetMappedPublicPort(5432);
        _connectionString = $"Host=localhost;Port={mappedPort};Database=postgres;Username=postgres;Password=postgres";
    }
    
    [TestMethod]
    public void TestWithRealDatabase()
    {
        // Use _connectionString to connect to the database
        using var connection = new Npgsql.NpgsqlConnection(_connectionString);
        connection.Open();
        
        // Execute database operations
        using var cmd = connection.CreateCommand();
        cmd.CommandText = "SELECT 1";
        var result = cmd.ExecuteScalar();
        
        Assert.AreEqual(1, result);
    }
    
    [ClassCleanup]
    public static async Task Cleanup()
    {
        // Stop and remove the container
        if (_dbContainer != null)
        {
            await _dbContainer.StopAsync();
        }
    }
}
```

### ❌ Don't: Use SQLite or in-memory DB engines to replace real databases

While in-memory or SQLite databases are tempting for their speed, they don't behave identically to your production database. Features, SQL dialects, and edge cases differ, potentially hiding bugs that would appear in production.

### ✅ Do: Start containers using assembly initialization

Initialize your test infrastructure once at the assembly level to save time. This approach makes the database an explicit dependency of the test, preventing failures due to missing infrastructure and making onboarding easier.

**Example:**
```csharp
using Microsoft.VisualStudio.TestTools.UnitTesting;
using DotNet.Testcontainers.Containers;
using DotNet.Testcontainers.Builders;
using System.Threading.Tasks;

[TestClass]
public class TestInitialization
{
    public static TestcontainersContainer DbContainer { get; private set; }
    public static string ConnectionString { get; private set; }
    
    [AssemblyInitialize]
    public static async Task AssemblyInitialize(TestContext context)
    {
        // Start database container once for all tests in assembly
        DbContainer = new ContainerBuilder()
            .WithImage("postgres:13")
            .WithEnvironment("POSTGRES_PASSWORD", "postgres")
            .WithPortBinding(5432, true)
            .WithWaitStrategy(Wait.ForUnixContainer().UntilPortIsAvailable(5432))
            .Build();
            
        await DbContainer.StartAsync();
        
        // Get the mapped port and build connection string
        int mappedPort = DbContainer.GetMappedPublicPort(5432);
        ConnectionString = $"Host=localhost;Port={mappedPort};Database=postgres;Username=postgres;Password=postgres";
        
        // Run migrations to create schema
        await RunDatabaseMigrations(ConnectionString);
    }
    
    private static async Task RunDatabaseMigrations(string connectionString)
    {
        // Use EF Core migrations or other migration tool
        var migrationsRunner = new MigrationsRunner(connectionString);
        await migrationsRunner.MigrateAsync();
    }
    
    [AssemblyCleanup]
    public static async Task AssemblyCleanup()
    {
        // Only stop container in CI environment
        if (IsRunningInCI())
        {
            await DbContainer.StopAsync();
        }
    }
    
    private static bool IsRunningInCI()
    {
        return Environment.GetEnvironmentVariable("CI") == "true";
    }
}
```

### ✅ Do: Optimize your real DB for testing, don't fake it

Use your production database engine in tests, but configure it for performance rather than durability. This gives you realistic testing while keeping execution fast.

**Example:**
```csharp
using DotNet.Testcontainers.Builders;

// Configure PostgreSQL for speed instead of durability
var dbContainer = new ContainerBuilder()
    .WithImage("postgres:13")
    .WithEnvironment("POSTGRES_PASSWORD", "postgres")
    // Turn off durability features for performance
    .WithCommand(
        "postgres", 
        "-c", "fsync=off", 
        "-c", "synchronous_commit=off", 
        "-c", "full_page_writes=off", 
        "-c", "random_page_cost=1.0")
    .WithPortBinding(5432, true)
    .Build();
```

### ✅ Do: Use in-memory storage where possible

Store test data in memory to reduce I/O and gain performance. For databases in containers, you can use tmpfs mounts on Linux.

**Example:**
```csharp
// For Linux environments, use tmpfs for database storage
var dbContainer = new ContainerBuilder()
    .WithImage("postgres:13")
    .WithEnvironment("POSTGRES_PASSWORD", "postgres")
    // Store database files in RAM
    .WithMount("/var/lib/postgresql/data", "tmpfs")
    .WithPortBinding(5432, true)
    .Build();
```

### ✅ Do: Build the DB schema using migrations

Use the same database migration approach in tests that you use in production. This validates your migration code and ensures your tests run against the same schema as production.

**Example:**
```csharp
[AssemblyInitialize]
public static async Task AssemblyInitialize(TestContext context)
{
    // Set up database container
    _dbContainer = new ContainerBuilder()
        .WithImage("postgres:13")
        .WithEnvironment("POSTGRES_PASSWORD", "postgres")
        .WithPortBinding(5432, true)
        .Build();
        
    await _dbContainer.StartAsync();
    
    // Build connection string
    int mappedPort = _dbContainer.GetMappedPublicPort(5432);
    string connectionString = $"Host=localhost;Port={mappedPort};Database=postgres;Username=postgres;Password=postgres";
    
    // Apply migrations just like in production
    var services = new ServiceCollection()
        .AddDbContext<AppDbContext>(options => 
            options.UseNpgsql(connectionString))
        .BuildServiceProvider();
        
    using var scope = services.CreateScope();
    var dbContext = scope.ServiceProvider.GetRequiredService<AppDbContext>();
    await dbContext.Database.MigrateAsync();
}
```




### ❌ Don't: Rely on snapshots for assertions

Snapshot testing can be inflexible and make test failures hard to understand. Explicit assertions are usually clearer.

**Example:**
```csharp
// Anti-pattern - Don't do this!
[TestMethod]
public async Task SnapshotTesting_AntiPattern()
{
    // Arrange
    var order = new OrderDto
    {
        UserId = 1,
        ProductId = 2,
        Quantity = 3
    };
    
    // Act
    var response = await _client.PostAsJsonAsync("/api/orders", order);
    var responseBody = await response.Content.ReadAsStringAsync();
    
    // Assert - Using snapshots
    // This approach can be brittle and makes failures hard to understand
    Assert.AreEqual(File.ReadAllText("snapshots/order_response.json"), responseBody);
    
    // Problems with this approach:
    // 1. Even minor formatting changes will break the test
    // 2. When the test fails, it's hard to see what's different
    // 3. Maintaining snapshot files adds complexity
}
```


### ❌ Don't: Use implementation mocks

Avoid mocking implementation details and verifying internal calls. This creates brittle tests that break when implementation changes even if behavior stays the same.

**Example:**
```csharp
// Anti-pattern - Don't do this!
[TestMethod]
public void ImplementationMock_AntiPattern()
{
    // Arrange
    var orderService = new OrderService();
    
    // Create spy to check internal implementation
    var orderValidatorSpy = A.Fake<OrderValidator>();
    typeof(OrderService)
        .GetField("_validator", BindingFlags.NonPublic | BindingFlags.Instance)
        .SetValue(orderService, orderValidatorSpy);
    
    // Act
    orderService.ProcessOrder(new Order { Id = 1 });
    
    // Assert - BAD PRACTICE!
    // Testing implementation details rather than behavior
    A.CallTo(() => orderValidatorSpy.Validate(A<Order>._)).MustHaveHappened();
    
    // Problems with this approach:
    // 1. Tests break when implementation changes, even if behavior doesn't
    // 2. Tests are tightly coupled to code structure
    // 3. Refactoring becomes harder because tests need to change too
}
```

### ❌ Don't: Share test data between tests

Creating shared test data leads to brittle tests with hidden dependencies. When one test modifies shared data, it can cause other tests to fail in confusing ways.

**Example:**
```csharp
// Anti-pattern - Don't do this!
[TestClass]
public class SharedDataAntiPatternTests
{
    private static int _sharedOrderId;
    private WebApplicationFactory<Program> _factory;
    private HttpClient _client;
    
    [ClassInitialize]
    public static async Task ClassInitialize(TestContext context)
    {
        // Creating shared data for all tests - BAD PRACTICE
        var factory = new WebApplicationFactory<Program>();
        var client = factory.CreateClient();
        
        var orderToCreate = new OrderDto
        {
            UserId = 1,
            ProductId = 2,
            Quantity = 3
        };
        
        var response = await client.PostAsJsonAsync("/api/orders", orderToCreate);
        var order = await response.Content.ReadFromJsonAsync<OrderResponseDto>();
        _sharedOrderId = order.Id;
        
        client.Dispose();
        factory.Dispose();
    }
    
    [TestInitialize]
    public void TestInitialize()
    {
        _factory = new WebApplicationFactory<Program>();
        _client = _factory.CreateClient();
    }
    
    [TestMethod]
    public async Task GetOrder_ReturnsCorrectOrder()
    {
        // Using shared order ID - creates hidden dependencies
        var response = await _client.GetAsync($"/api/orders/{_sharedOrderId}");
        response.EnsureSuccessStatusCode();
        
        var order = await response.Content.ReadFromJsonAsync<OrderResponseDto>();
        Assert.AreEqual(1, order.UserId);
        Assert.AreEqual(2, order.ProductId);
        Assert.AreEqual(3, order.Quantity);
    }
    
    [TestMethod]
    public async Task UpdateOrder_UpdatesCorrectFields()
    {
        // Using shared order ID again
        var updateData = new OrderUpdateDto
        {
            Quantity = 5
        };
        
        var updateResponse = await _client.PutAsJsonAsync($"/api/orders/{_sharedOrderId}", updateData);
        updateResponse.EnsureSuccessStatusCode();
        
        // This test now modifies the shared data, which will break the first test
        // if tests run in a different order
    }
    
    [TestCleanup]
    public void TestCleanup()
    {
        _client.Dispose();
        _factory.Dispose();
    }
}
```

### ❌ Don't: Check data directly in the database

Checking database state directly misses potential issues in your API or service layer. It also increases coupling between tests and database schema.

**Example:**
```csharp
// Anti-pattern - Don't do this!
[TestMethod]
public async Task DirectDatabaseCheck_AntiPattern()
{
    // Arrange
    var dbContext = _factory.Services.GetRequiredService<AppDbContext>();
    
    var orderToAdd = new Order
    {
        UserId = 1,
        ProductId = 2,
        Quantity = 3
    };
    
    // Act - Using API to create the order
    var createDto = new OrderDto
    {
        UserId = orderToAdd.UserId,
        ProductId = orderToAdd.ProductId,
        Quantity = orderToAdd.Quantity
    };
    
    var response = await _client.PostAsJsonAsync("/api/orders", createDto);
    response.EnsureSuccessStatusCode();
    
    // BAD PRACTICE: Directly checking database state
    var savedOrder = await dbContext.Orders
        .Where(o => o.UserId == 1 && o.ProductId == 2)
        .FirstOrDefaultAsync();
        
    Assert.IsNotNull(savedOrder);
    Assert.AreEqual(3, savedOrder.Quantity);
    
    // Problem: This bypasses the API's GET endpoint
    // If there are bugs in the retrieval logic, this test won't catch them
    // It also couples the test to the database schema
}
```

### ❌ Don't: Rely on manual DB setup

Requiring manual setup steps creates friction, inconsistencies between developers, and potential test failures. Automate infrastructure setup to ensure everyone runs tests against the same environment.

**Example:**
```csharp
// Anti-pattern - Don't do this!
public static class TestHelper
{
    public static void SetupTestDatabase()
    {
        // Instructions that require manual steps
        Console.WriteLine("Please ensure PostgreSQL is running on port 5432");
        Console.WriteLine("Please ensure the 'test_db' database exists");
        Console.WriteLine("Please ensure the 'test_user' role exists with password 'test'");
        
        // This approach relies on developers setting up their environment correctly
        // which leads to inconsistencies and "works on my machine" problems
    }
}

[TestClass]
public class ManualSetupAntiPatternTests
{
    [ClassInitialize]
    public static void ClassInitialize(TestContext context)
    {
        // Calling manual setup helper
        TestHelper.SetupTestDatabase();
    }
    
    [TestMethod]
    public void TestRequiringManualSetup()
    {
        // Test will fail if developer didn't set up environment correctly
    }
}
```

### ❌ Don't: Test against remote environments

Running tests against remote servers or containers loses control over the environment, makes it harder to set up test data, and limits your ability to monitor or inject test doubles.

**Example:**
```csharp
// Anti-pattern - Don't do this!
[TestClass]
public class RemoteEnvironmentAntiPatternTests
{
    private HttpClient _client;
    
    [TestInitialize]
    public void Initialize()
    {
        // Connecting to a shared development environment
        _client = new HttpClient
        {
            BaseAddress = new Uri("https://dev-api.example.com")
        };
        
        // Problems with this approach:
        // 1. Tests may interfere with each other in the shared environment
        // 2. Cannot reliably set up test data
        // 3. Cannot mock external services
        // 4. Tests fail if the remote environment is down
        // 5. Tests might affect other developers using the environment
    }
    
    [TestMethod]
    public async Task TestAgainstRemoteEnvironment()
    {
        // This test depends on the state of a remote environment we don't control
        var response = await _client.GetAsync("/api/status");
        response.EnsureSuccessStatusCode();
    }
}
```

### ❌ Don't: Hardcode ports in tests

Using fixed ports creates conflicts between parallel test runs and can lead to flaky tests if those ports are in use.

**Example:**
```csharp
// Anti-pattern - Don't do this!
[TestClass]
public class HardcodedPortAntiPatternTests
{
    private WebApplicationFactory<Program> _factory;
    private HttpClient _client;
    
    [TestInitialize]
    public void Initialize()
    {
        // Using WebApplicationFactory but with a hardcoded port
        _factory = new WebApplicationFactory<Program>()
            .WithWebHostBuilder(builder =>
            {
                builder.UseUrls("http://localhost:5001"); // Hardcoded port - BAD
            });
            
        _client = new HttpClient
        {
            BaseAddress = new Uri("http://localhost:5001")
        };
        
        // Problems with this approach:
        // 1. Tests will fail if port 5001 is already in use
        // 2. Cannot run tests in parallel
        // 3. Tests might interfere with other processes
    }
    
    [TestMethod]
    public async Task TestWithHardcodedPort()
    {
        var response = await _client.GetAsync("/api/status");
        response.EnsureSuccessStatusCode();
    }
    
    [TestCleanup]
    public void Cleanup()
    {
        _client.Dispose();
        _factory.Dispose();
    }
}
```

### ✅ Do: Reuse containers in development but restart in CI

In local development, keep containers running between test runs to save time. In CI environments, recreate containers for each build to ensure clean state.

**Example:**
```csharp
[AssemblyCleanup]
public static async Task AssemblyCleanup()
{
    // Only stop and remove containers in CI environments
    if (Environment.GetEnvironmentVariable("CI") == "true")
    {
        await _dbContainer.StopAsync();
        await _dbContainer.DisposeAsync();
    }
    // In local development, keep containers running for faster subsequent test runs
}
```

## Web Server Setup

### ✅ Do: Keep tests and app under test in the same process

Run your tests and the application being tested in the same process to enable full control over the application's lifecycle and configuration.

**Example:**
```csharp
using Microsoft.AspNetCore.Mvc.Testing;
using Microsoft.Extensions.DependencyInjection;
using Microsoft.VisualStudio.TestTools.UnitTesting;
using System.Net.Http;
using System.Threading.Tasks;

[TestClass]
public class WebApiTests
{
    private WebApplicationFactory<Program> _factory;
    private HttpClient _client;
    
    [TestInitialize]
    public void Initialize()
    {
        // Create an application in the same process
        _factory = new WebApplicationFactory<Program>()
            .WithWebHostBuilder(builder =>
            {
                // Configure services for testing
                builder.ConfigureServices(services =>
                {
                    // Replace services with test doubles as needed
                    services.AddScoped<IPaymentGateway, FakePaymentGateway>();
                });
                
                // Override configuration
                builder.ConfigureAppConfiguration((context, config) =>
                {
                    // Add test-specific configuration
                });
            });
            
        _client = _factory.CreateClient();
    }
    
    [TestMethod]
    public async Task GetProducts_ReturnsSuccessStatusCode()
    {
        // Act
        var response = await _client.GetAsync("/api/products");
        
        // Assert
        response.EnsureSuccessStatusCode();
    }
    
    [TestCleanup]
    public void Cleanup()
    {
        _client.Dispose();
        _factory.Dispose();
    }
}
```

### ✅ Do: Let tests control when servers start and stop

Structure your code to allow tests to control the application's lifecycle, including startup and shutdown. This allows tests to configure the application before it starts.

**Example:**
```csharp
// In your application code
public class Program
{
    public static void Main(string[] args)
    {
        var app = CreateWebApplication(args);
        app.Run();
    }
    
    // Expose this method for testing
    public static WebApplication CreateWebApplication(string[] args)
    {
        var builder = WebApplication.CreateBuilder(args);
        
        // Add services
        builder.Services.AddControllers();
        builder.Services.AddSwaggerGen();
        builder.Services.AddScoped<IOrderService, OrderService>();
        // ...
        
        var app = builder.Build();
        
        // Configure middleware
        app.UseHttpsRedirection();
        app.UseAuthorization();
        app.MapControllers();
        
        return app;
    }
}

// In your test code
[TestMethod]
public async Task ControlAppLifecycle()
{
    // Create application without starting it
    var app = Program.CreateWebApplication(Array.Empty<string>());
    
    // Configure for testing
    app.Services.GetRequiredService<IDbConnection>().ConnectionString = _testConnectionString;
    
    // Start the app
    await app.StartAsync();
    
    try
    {
        // Run tests...
        var client = new HttpClient
        {
            BaseAddress = new Uri("http://localhost:5000")
        };
        
        var response = await client.GetAsync("/api/products");
        response.EnsureSuccessStatusCode();
    }
    finally
    {
        // Stop the app
        await app.StopAsync();
        await app.DisposeAsync();
    }
}
```

### ✅ Do: Use dynamic ports in testing

Let the server select a free port automatically to prevent port conflicts when running multiple tests in parallel.

**Example:**
```csharp
[TestMethod]
public async Task UseRandomPort()
{
    // Create application with a random port
    var builder = WebApplication.CreateBuilder(new[] { "--urls", "http://localhost:0" });
    
    // Configure services...
    builder.Services.AddControllers();
    
    var app = builder.Build();
    app.MapControllers();
    
    // Start the app
    await app.StartAsync();
    
    try
    {
        // Get the actual port that was assigned
        var server = app.Services.GetRequiredService<IServer>();
        var addressFeature = server.Features.Get<IServerAddressesFeature>();
        var address = addressFeature.Addresses.First();
        
        // Create client using the dynamic address
        var client = new HttpClient
        {
            BaseAddress = new Uri(address)
        };
        
        var response = await client.GetAsync("/api/health");
        response.EnsureSuccessStatusCode();
    }
    finally
    {
        await app.StopAsync();
        await app.DisposeAsync();
    }
}
```

## The Test Anatomy

### ✅ Do: Follow unit testing best practices

Write integration tests with the same structure and practices as unit tests. Keep tests small (around 7 statements), use consistent naming (When/Then), and follow the AAA pattern (Arrange-Act-Assert).

**Example:**
```csharp
[TestClass]
public class OrderControllerTests
{
    private WebApplicationFactory<Program> _factory;
    private HttpClient _client;
    
    [TestInitialize]
    public void Initialize()
    {
        _factory = new WebApplicationFactory<Program>();
        _client = _factory.CreateClient();
    }
    
    [TestMethod]
    public async Task WhenRequestingExistingOrder_ThenRetrieveSuccessfully()
    {
        // Arrange
        var orderToAdd = new OrderDto
        {
            UserId = 1,
            ProductId = 2,
            Mode = "approved"
        };
        
        var createResponse = await _client.PostAsJsonAsync("/api/orders", orderToAdd);
        var createdOrder = await createResponse.Content.ReadFromJsonAsync<OrderResponseDto>();
        
        // Act
        var getResponse = await _client.GetAsync($"/api/orders/{createdOrder.Id}");
        var retrievedOrder = await getResponse.Content.ReadFromJsonAsync<OrderResponseDto>();
        
        // Assert
        Assert.AreEqual(200, (int)getResponse.StatusCode);
        Assert.AreEqual(orderToAdd.UserId, retrievedOrder.UserId);
        Assert.AreEqual(orderToAdd.ProductId, retrievedOrder.ProductId);
        Assert.AreEqual(orderToAdd.Mode, retrievedOrder.Mode);
    }
    
    [TestCleanup]
    public void Cleanup()
    {
        _client.Dispose();
        _factory.Dispose();
    }
}
```

### ✅ Do: Provide real credentials or tokens, if possible

If your API requires authentication, use real tokens or credentials in tests rather than disabling authentication. This ensures you're testing the full authorization flow.

**Example:**
```csharp
[TestClass]
public class AuthenticatedApiTests
{
    private WebApplicationFactory<Program> _factory;
    private HttpClient _client;
    private string _authToken;
    
    [TestInitialize]
    public async Task Initialize()
    {
        _factory = new WebApplicationFactory<Program>();
        _client = _factory.CreateClient();
        
        // Get a real auth token for testing
        _authToken = await GetAuthToken();
        _client.DefaultRequestHeaders.Authorization = 
            new System.Net.Http.Headers.AuthenticationHeaderValue("Bearer", _authToken);
    }
    
    private async Task<string> GetAuthToken()
    {
        // Option 1: Call your authentication endpoint
        var loginResponse = await _client.PostAsJsonAsync("/api/auth/login", 
            new { Username = "testuser", Password = "testpassword" });
        var tokenResponse = await loginResponse.Content.ReadFromJsonAsync<TokenResponse>();
        return tokenResponse.Token;
        
        // Option 2: Generate a token with the same code your API uses to validate tokens
        /* 
        var securityKey = new SymmetricSecurityKey(
            Encoding.UTF8.GetBytes(Configuration["Jwt:Key"]));
        var credentials = new SigningCredentials(securityKey, SecurityAlgorithms.HmacSha256);
        
        var claims = new[]
        {
            new Claim(ClaimTypes.NameIdentifier, "1"),
            new Claim(ClaimTypes.Name, "testuser"),
            new Claim(ClaimTypes.Role, "User")
        };
        
        var token = new JwtSecurityToken(
            issuer: "test-issuer",
            audience: "test-audience",
            claims: claims,
            expires: DateTime.Now.AddHours(1),
            signingCredentials: credentials);
            
        return new JwtSecurityTokenHandler().WriteToken(token);
        */
    }
    
    [TestMethod]
    public async Task GetUserProfile_WithValidToken_ReturnsUserData()
    {
        // Act
        var response = await _client.GetAsync("/api/user/profile");
        
        // Assert
        response.EnsureSuccessStatusCode();
        var profile = await response.Content.ReadFromJsonAsync<UserProfileDto>();
        Assert.AreEqual("testuser", profile.Username);
    }
    
    [TestCleanup]
    public void Cleanup()
    {
        _client.Dispose();
        _factory.Dispose();
    }
}
```

### ✅ Do: Assert on the entire HTTP response

When testing APIs, assert on the entire response object including status code, headers, and body. This provides a complete validation of the response and makes test failures more descriptive.

**Example:**
```csharp
[TestMethod]
public async Task GetOrder_WithExistingId_ReturnsCompleteOrderData()
{
    // Arrange
    var createdOrder = await CreateTestOrder();
    
    // Act
    var response = await _client.GetAsync($"/api/orders/{createdOrder.Id}");
    var content = await response.Content.ReadAsStringAsync();
    var order = JsonSerializer.Deserialize<OrderDto>(content, 
        new JsonSerializerOptions { PropertyNameCaseInsensitive = true });
    
    // Assert - Check full response
    Assert.AreEqual(System.Net.HttpStatusCode.OK, response.StatusCode);
    Assert.AreEqual("application/json; charset=utf-8", 
        response.Content.Headers.ContentType.ToString());
    Assert.IsNotNull(order);
    Assert.AreEqual(createdOrder.Id, order.Id);
    Assert.AreEqual(createdOrder.UserId, order.UserId);
    Assert.AreEqual(createdOrder.ProductId, order.ProductId);
    Assert.AreEqual(createdOrder.Status, order.Status);
}
```

### ✅ Do: Structure tests by routes and stories

Organize your tests using MSTest's `TestClass` attributes to represent API routes or feature areas. This creates a clear structure that mirrors your API and makes it easy to find tests related to specific endpoints.

**Example:**
```csharp
[TestClass]
public class OrdersApiTests
{
    // Common setup code

    [TestClass]
    public class GetOrders : OrdersApiTests
    {
        [TestMethod]
        public async Task WhenRequestingExistingOrder_ThenReturnSuccessfully()
        {
            // Test GET /api/orders/{id}
        }
        
        [TestMethod]
        public async Task WhenRequestingNonExistentOrder_ThenReturn404()
        {
            // Test GET /api/orders/{id} with invalid ID
        }
    }
    
    [TestClass]
    public class CreateOrders : OrdersApiTests
    {
        [TestMethod]
        public async Task WhenCreatingOrderWithValidData_ThenReturnCreated()
        {
            // Test POST /api/orders
        }
        
        [TestMethod]
        public async Task WhenCreatingOrderWithInvalidData_ThenReturnBadRequest()
        {
            // Test POST /api/orders with invalid data
        }
    }
    
    [TestClass]
    public class UpdateOrders : OrdersApiTests
    {
        // Update order tests
    }
    
    [TestClass]
    public class DeleteOrders : OrdersApiTests
    {
        // Delete order tests
    }
}
```

### ✅ Do: Test the five potential outcomes

For each feature, test all relevant outcomes: HTTP response, state changes, external calls, message queue events, and observability. This ensures comprehensive coverage of all side effects.

**Example:**
```csharp
[TestMethod]
public async Task ProcessOrder_VerifiesAllFiveOutcomes()
{
    // Setup mocks and test data
    var mockEmailService = A.Fake<IEmailService>();
    var mockLogger = A.Fake<ILogger<OrdersController>>();
    var messageQueue = new FakeMessageQueue();
    
    var factory = new WebApplicationFactory<Program>()
        .WithWebHostBuilder(builder =>
        {
            builder.ConfigureServices(services =>
            {
                services.AddSingleton<IEmailService>(mockEmailService);
                services.AddSingleton<ILogger<OrdersController>>(mockLogger);
                services.AddSingleton<IMessageQueue>(messageQueue);
            });
        });
    
    var client = factory.CreateClient();
    
    // Act - Place an order
    var orderDto = new OrderDto { ProductId = 1, Quantity = 2 };
    var response = await client.PostAsJsonAsync("/api/orders", orderDto);
    
    // Assert 1: HTTP Response
    response.EnsureSuccessStatusCode();
    var createdOrder = await response.Content.ReadFromJsonAsync<OrderResponseDto>();
    Assert.IsNotNull(createdOrder);
    
    // Assert 2: State Change - Check data was saved
    var getResponse = await client.GetAsync($"/api/orders/{createdOrder.Id}");
    var retrievedOrder = await getResponse.Content.ReadFromJsonAsync<OrderResponseDto>();
    Assert.AreEqual(createdOrder.Id, retrievedOrder.Id);
    
    // Assert 3: External Calls - Check email was sent
    A.CallTo(() => mockEmailService.SendOrderConfirmation(
        A<string>.That.Contains(createdOrder.Id.ToString()), 
        A<string>._))
        .MustHaveHappened();
    
    // Assert 4: Message Queue - Check event was published
    Assert.IsTrue(messageQueue.PublishedMessages.Any(m => 
        m.Topic == "order-created" && 
        m.Payload.Contains(createdOrder.Id.ToString())));
    
    // Assert 5: Observability - Check logging
    A.CallTo(() => mockLogger.Log(
        LogLevel.Information,
        A<EventId>._,
        A<It.IsAnyType>.That.Matches(o => o.ToString().Contains("Order created")),
        A<Exception>._,
        A<Func<It.IsAnyType, Exception, string>>._))
        .MustHaveHappened();
}
```



## Integrations

### ✅ Do: Isolate the component using HTTP interceptors

Use tools like WireMock.NET to intercept HTTP calls to external services and provide predetermined responses. This isolates your component under test and allows simulation of various scenarios.

**Example:**
```csharp
using Microsoft.VisualStudio.TestTools.UnitTesting;
using WireMock.RequestBuilders;
using WireMock.ResponseBuilders;
using WireMock.Server;
using System.Net.Http;
using System.Threading.Tasks;

[TestClass]
public class ExternalServiceTests
{
    private WireMockServer _userServiceMock;
    private HttpClient _client;
    private WebApplicationFactory<Program> _factory;
    
    [TestInitialize]
    public void Initialize()
    {
        // Start a mock server for the external user service
        _userServiceMock = WireMockServer.Start();
        
        // Configure mock responses
        _userServiceMock
            .Given(Request.Create().WithPath("/api/users/1").UsingGet())
            .RespondWith(Response.Create()
                .WithStatusCode(200)
                .WithHeader("Content-Type", "application/json")
                .WithBody(@"{""id"": 1, ""name"": ""John Doe""}"));
                
        // Configure our app to use the mock service
        _factory = new WebApplicationFactory<Program>()
            .WithWebHostBuilder(builder =>
            {
                builder.ConfigureServices(services =>
                {
                    services.Configure<ExternalServicesOptions>(options =>
                    {
                        options.UserServiceBaseUrl = _userServiceMock.Urls[0];
                    });
                });
            });
            
        _client = _factory.CreateClient();
    }
    
    [TestMethod]
    public async Task CreateOrder_CallsUserService_ReturnsSuccess()
    {
        // Arrange
        var orderDto = new OrderDto
        {
            UserId = 1,
            ProductId = 2,
            Quantity = 1
        };
        
        // Act
        var response = await _client.PostAsJsonAsync("/api/orders", orderDto);
        
        // Assert
        response.EnsureSuccessStatusCode();
        
        // Verify the request was made to the mock user service
        _userServiceMock.LogEntries.Should().Contain(entry =>
            entry.RequestMessage.Path == "/api/users/1");
    }
    
    [TestCleanup]
    public void Cleanup()
    {
        _client.Dispose();
        _factory.Dispose();
        _userServiceMock.Dispose();
    }
}
```

### ✅ Do: Define default responses before each test

Set up default mock responses before each test to ensure a clean slate. This prevents tests from interfering with each other if one test changes the mock behavior.

**Example:**
```csharp
[TestClass]
public class ExternalServiceIntegrationTests
{
    private WireMockServer _mockServer;
    private HttpClient _client;
    
    [TestInitialize]
    public void Initialize()
    {
        // Start mock server
        _mockServer = WireMockServer.Start();
        
        // Configure default responses before each test
        SetupDefaultMocks();
        
        // Setup your application with the mock service URL
        var factory = new WebApplicationFactory<Program>()
            .WithWebHostBuilder(builder =>
            {
                builder.ConfigureServices(services =>
                {
                    services.Configure<ApiSettings>(settings =>
                    {
                        settings.ExternalServiceUrl = _mockServer.Urls[0];
                    });
                });
            });
            
        _client = factory.CreateClient();
    }
    
    private void SetupDefaultMocks()
    {
        // Reset all mappings to ensure clean state
        _mockServer.Reset();
        
        // Setup default mock responses
        _mockServer
            .Given(Request.Create().WithPath("/api/users/1").UsingGet())
            .RespondWith(Response.Create()
                .WithStatusCode(200)
                .WithHeader("Content-Type", "application/json")
                .WithBody(@"{""id"": 1, ""name"": ""John Doe"", ""status"": ""active""}"));
                
        _mockServer
            .Given(Request.Create().WithPath("/api/products").UsingGet())
            .RespondWith(Response.Create()
                .WithStatusCode(200)
                .WithHeader("Content-Type", "application/json")
                .WithBody(@"[{""id"": 1, ""name"": ""Product 1"", ""price"": 19.99}]"));
    }
    
    [TestMethod]
    public async Task GetUserInfo_ReturnsUserData()
    {
        // Act
        var response = await _client.GetAsync("/api/user-info/1");
        
        // Assert
        response.EnsureSuccessStatusCode();
        var userData = await response.Content.ReadFromJsonAsync<UserInfoDto>();
        Assert.AreEqual(1, userData.Id);
        Assert.AreEqual("John Doe", userData.Name);
    }
    
    [TestCleanup]
    public void Cleanup()
    {
        _client.Dispose();
        _mockServer.Dispose();
    }
}
```

### ✅ Do: Override happy paths with corner cases

For each test that needs to simulate a specific scenario (like errors or timeouts), override the default mock behavior with the desired response.

**Example:**
```csharp
[TestMethod]
public async Task GetUserInfo_WhenUserDoesNotExist_ReturnsNotFound()
{
    // Arrange - Override the default mock for user 7 (non-existent user)
    _mockServer
        .Given(Request.Create().WithPath("/api/users/7").UsingGet())
        .RespondWith(Response.Create()
            .WithStatusCode(404)
            .WithHeader("Content-Type", "application/json")
            .WithBody(@"{""message"": ""User does not exist"", ""code"": ""nonExisting""}"));
            
    // Act
    var response = await _client.GetAsync("/api/user-info/7");
    
    // Assert
    Assert.AreEqual(System.Net.HttpStatusCode.NotFound, response.StatusCode);
    var error = await response.Content.ReadFromJsonAsync<ErrorResponse>();
    Assert.AreEqual("User not found", error.Message);
}
```

### ✅ Do: Deny unexpected outgoing requests

Configure your mocking framework to block and fail on any outgoing HTTP requests that haven't been explicitly mocked. This ensures your tests remain isolated and prevents accidental calls to real services.

**Example:**
```csharp
[TestInitialize]
public void Initialize()
{
    // Start mock server
    _mockServer = WireMockServer.Start(new WireMockServerSettings
    {
        StartAdminInterface = true,
        AllowPartialMapping = false, // Forces exact matching of requests
    });
    
    // Set up default mocks
    SetupDefaultMocks();
    
    // Configure global fallback to block unexpected requests
    _mockServer
        .Given(Request.Create().WithPath("/*").UsingAnyMethod())
        .AtPriority(999) // Lower priority than all other mappings
        .RespondWith(Response.Create()
            .WithStatusCode(500)
            .WithBody("Unexpected request - no matching mock configured"));
            
    // Configure the application to use the mock server for all external requests
    _factory = new WebApplicationFactory<Program>()
        .WithWebHostBuilder(builder =>
        {
            builder.ConfigureServices(services =>
            {
                services.Configure<HttpClientOptions>(options =>
                {
                    // For TestHttpMessageHandler on internal HttpClient
                    options.BaseAddress = new Uri(_mockServer.Urls[0]);
                });
            });
        });
        
    _client = _factory.CreateClient();
}

[TestMethod]
public async Task UnexpectedRequest_ShouldFailTest()
{
    // Act - No mock defined for this path
    var exception = await Assert.ThrowsExceptionAsync<HttpRequestException>(async () =>
    {
        await _client.GetAsync("/api/unmocked-endpoint");
    });
    
    // Assert
    Assert.IsTrue(exception.Message.Contains("500") ||
                 exception.Message.Contains("Unexpected request"));
}
```

### ✅ Do: Simulate network chaos

Test your application's resilience by simulating network problems like timeouts, slow responses, and failed requests. This ensures your code can handle real-world networking issues.

**Example:**
```csharp
[TestMethod]
public async Task OrderService_WithSlowUserService_HandlesTimeoutGracefully()
{
    // Arrange - Configure a slow response that takes 5 seconds
    _mockServer
        .Given(Request.Create().WithPath("/api/users/1").UsingGet())
        .RespondWith(Response.Create()
            .WithStatusCode(200)
            .WithBody(@"{""id"": 1, ""name"": ""John""}")
            .WithDelay(TimeSpan.FromSeconds(5))); // Add a 5-second delay
            
    // Act
    var response = await _client.PostAsJsonAsync("/api/orders", new OrderDto
    {
        UserId = 1,
        ProductId = 2,
        Quantity = 1
    });
    
    // Assert - Our service should return a 503 Service Unavailable
    Assert.AreEqual(System.Net.HttpStatusCode.ServiceUnavailable, response.StatusCode);
    var error = await response.Content.ReadFromJsonAsync<ErrorResponse>();
    Assert.IsTrue(error.Message.Contains("timeout") || error.Message.Contains("unavailable"));
}

[TestMethod]
public async Task OrderService_WithRetryableError_EventuallySucceeds()
{
    // Arrange - Configure a service that fails on first call but succeeds on second
    var callCount = 0;
    
    _mockServer
        .Given(Request.Create().WithPath("/api/users/1").UsingGet())
        .RespondWith(Response.Create()
            .WithCallback(req => 
            {
                callCount++;
                if (callCount == 1)
                {
                    return new ResponseMessage
                    {
                        StatusCode = 503,
                        Headers = new Dictionary<string, string> 
                        {
                            { "Retry-After", "1" }
                        }
                    };
                }
                return new ResponseMessage
                {
                    StatusCode = 200,
                    Headers = new Dictionary<string, string>
                    {
                        { "Content-Type", "application/json" }
                    },
                    Body = @"{""id"": 1, ""name"": ""John""}"
                };
            }));
            
    // Act
    var response = await _client.PostAsJsonAsync("/api/orders", new OrderDto
    {
        UserId = 1,
        ProductId = 2,
        Quantity = 1
    });
    
    // Assert - Service should retry and eventually succeed
    response.EnsureSuccessStatusCode();
    Assert.AreEqual(2, callCount);
}
```

### ✅ Do: Verify outgoing request validity

When testing integrations, also verify that your code sends the correct request data to external services. This catches issues in how your code constructs API calls.

**Example:**
```csharp
[TestMethod]
public async Task SendingEmail_UsesCorrectTemplateAndData()
{
    // Arrange
    var capturedRequest = new RequestMessage();
    
    _mockServer
        .Given(Request.Create().WithPath("/api/email").UsingPost())
        .RespondWith(Response.Create()
            .WithStatusCode(202)
            .WithCallback(req => 
            {
                capturedRequest.Body = req.Body;
                capturedRequest.Headers = req.Headers;
                return new ResponseMessage { StatusCode = 202 };
            }));
            
    // Act - Trigger email sending
    var response = await _client.PostAsJsonAsync("/api/orders/1/send-confirmation", new {});
    
    // Assert - Verify the email API was called with correct data
    response.EnsureSuccessStatusCode();
    
    // Check that the request was properly formatted
    var emailRequest = JsonSerializer.Deserialize<EmailRequest>(
        capturedRequest.Body, new JsonSerializerOptions { PropertyNameCaseInsensitive = true });
    
    Assert.IsNotNull(emailRequest);
    Assert.AreEqual("order_confirmation", emailRequest.TemplateId);
    Assert.IsNotNull(emailRequest.To);
    Assert.IsTrue(emailRequest.To.Matches(@"^[\w-\.]+@([\w-]+\.)+[\w-]{2,4}$"));
    Assert.IsNotNull(emailRequest.Subject);
    Assert.IsTrue(emailRequest.Subject.Contains("Order Confirmation"));
}

public class RequestMessage
{
    public string Body { get; set; }
    public Dictionary<string, string> Headers { get; set; }
}

public class EmailRequest
{
    public string TemplateId { get; set; }
    public string To { get; set; }
    public string Subject { get; set; }
    public Dictionary<string, object> TemplateData { get; set; }
}
```

### ✅ Do: Use API contract testing

Ensure that your code correctly follows the contract of external APIs by validating requests against API schemas and producing valid responses according to your own API contract.

**Example:**
```csharp
using Swashbuckle.AspNetCore.SwaggerGen;
using Microsoft.OpenApi.Readers;
using Microsoft.OpenApi.Models;
using NJsonSchema;

[TestClass]
public class ApiContractTests
{
    private WireMockServer _mockServer;
    private HttpClient _client;
    private OpenApiDocument _apiSpec;
    
    [TestInitialize]
    public async Task Initialize()
    {
        // Load OpenAPI specification
        var openApiStream = File.OpenRead("api-specification.json");
        var openApiDocument = new OpenApiStreamReader().Read(openApiStream, out var diagnostic);
        _apiSpec = openApiDocument;
        
        // Start mock server
        _mockServer = WireMockServer.Start();
        
        // Configure application with mock server
        _client = new WebApplicationFactory<Program>()
            .WithWebHostBuilder(builder =>
            {
                builder.ConfigureServices(services =>
                {
                    services.Configure<ExternalServicesOptions>(options =>
                    {
                        options.UserServiceBaseUrl = _mockServer.Urls[0];
                    });
                });
            })
            .CreateClient();
    }
    
    [TestMethod]
    public async Task ApiEndpoints_ConformToOpenApiSpec()
    {
        // Get all paths from the API spec
        foreach (var path in _apiSpec.Paths)
        {
            var pathUrl = path.Key;
            
            // Check GET operations
            if (path.Value.Operations.ContainsKey(OperationType.Get))
            {
                // Replace path parameters with actual values
                var url = ReplacePathParameters(pathUrl);
                
                // Call the API
                var response = await _client.GetAsync(url);
                
                // Check response status code matches one in the spec
                var operation = path.Value.Operations[OperationType.Get];
                Assert.IsTrue(operation.Responses.ContainsKey(((int)response.StatusCode).ToString()),
                    $"Status code {response.StatusCode} not defined in spec for GET {pathUrl}");
                
                // If success, validate response against schema
                if (response.IsSuccessStatusCode)
                {
                    var content = await response.Content.ReadAsStringAsync();
                    var responseSchema = GetResponseSchema(operation, response.StatusCode);
                    
                    if (responseSchema != null)
                    {
                        var jsonSchema = NJsonSchema.JsonSchema.FromJsonAsync(
                            responseSchema.ToString()).Result;
                        var errors = jsonSchema.Validate(content);
                        
                        Assert.AreEqual(0, errors.Count, 
                            $"Response doesn't match schema for GET {pathUrl}: {string.Join(", ", errors)}");
                    }
                }
            }
            
            // Similar checks for other HTTP methods...
        }
    }
    
    private string ReplacePathParameters(string pathTemplate)
    {
        // Replace {id} with 1, {userId} with 1, etc.
        var result = System.Text.RegularExpressions.Regex.Replace(
            pathTemplate, @"\{([^}]+)\}", match => "1");
        return result;
    }
    
    private object GetResponseSchema(OpenApiOperation operation, System.Net.HttpStatusCode statusCode)
    {
        var statusCodeString = ((int)statusCode).ToString();
        
        if (operation.Responses.TryGetValue(statusCodeString, out var response))
        {
            if (response.Content.TryGetValue("application/json", out var mediaType))
            {
                return mediaType.Schema;
            }
        }
        
        return null;
    }
    
    [TestCleanup]
    public void Cleanup()
    {
        _client.Dispose();
        _mockServer.Dispose();
    }
}
```

### ❌ Don't: Allow unexpected outgoing calls

Allowing unexpected external calls can lead to flaky tests, slow performance, and potential side effects. It also hides the fact that your component is making calls you might not be aware of.

**Example:**
```csharp
// Anti-pattern - Don't do this!
[TestMethod]
public async Task TestWithoutControllingExternalCalls_AntiPattern()
{
    // Arrange
    var client = new HttpClient();
    var service = new OrderService(client);
    
    // Act - This might call real external services!
    await service.ProcessOrderAsync(new Order { Id = 1 });
    
    // Assert
    // ...
    
    // Problems with this approach:
    // 1. Tests might fail if external services are down
    // 2. Tests might be slow due to real network calls
    // 3. External services might be affected by test data
    // 4. Tests might not cover error scenarios from external services
}
```

### ✅ Do: Record real outgoing requests for awareness

When testing your component, it's valuable to know exactly what HTTP requests it makes to external services. Various network interceptors like WireMock.NET can record these outgoing requests, helping you discover all endpoints being called and understand their various response patterns.

Run these tools in recording mode during development to capture the full spectrum of network traffic, which will be saved to local files for analysis. This helps you ensure all integration points are properly covered in your tests.

Why is this important? You might miss some endpoints or, more likely, certain corner scenarios. For example, you may believe a `/users/:id` endpoint only returns HTTP 200 with a body, but it might sometimes return HTTP 204 with an empty body. If these scenarios aren't tested, they'll be discovered in production.

For complex and critical integrations, consider also monitoring production network logs to understand all possible response patterns.

**Example:**
```csharp
using Microsoft.VisualStudio.TestTools.UnitTesting;
using WireMock.RequestBuilders;
using WireMock.ResponseBuilders;
using WireMock.Server;
using System.Net.Http;
using System.Threading.Tasks;
using System.IO;

[TestClass]
public class RecordingOutgoingRequestsTests
{
    private WireMockServer _mockServer;
    private WebApplicationFactory<Program> _factory;
    private HttpClient _client;
    
    [TestInitialize]
    public void Initialize()
    {
        // Start WireMock server in recording mode
        _mockServer = WireMockServer.Start(new WireMockServerSettings
        {
            StartAdminInterface = true,
            ReadStaticMappings = true,
            
            // Set up recording mode - this will capture all requests
            ProxyAndRecordSettings = new ProxyAndRecordSettings
            {
                Url = "https://real-api.example.com", // The real service URL
                SaveMapping = true,
                SaveMappingToFile = true,
                MappingFolder = "RecordedMappings" // Folder where recordings will be saved
            }
        });
        
        // Configure your application to use the mock server as a proxy
        _factory = new WebApplicationFactory<Program>()
            .WithWebHostBuilder(builder =>
            {
                builder.ConfigureServices(services =>
                {
                    services.Configure<HttpClientOptions>(options =>
                    {
                        options.BaseAddress = new Uri(_mockServer.Urls[0]);
                    });
                });
            });
            
        _client = _factory.CreateClient();
    }
    
    [TestMethod]
    public async Task RecordActualServiceResponses()
    {
        // Arrange
        // In recording mode, WireMock will proxy requests to the real service
        // and record both the request and response
        
        // Act - This will make actual HTTP calls to the real service via WireMock
        var response = await _client.GetAsync("/api/users/1");
        
        // This call might reveal different response patterns, like:
        var emptyResponse = await _client.GetAsync("/api/users/2"); // Might return 204 No Content
        
        // Assert
        response.EnsureSuccessStatusCode();
        
        // Verify that WireMock recorded the mappings
        Assert.IsTrue(Directory.Exists("RecordedMappings"));
        Assert.IsTrue(Directory.GetFiles("RecordedMappings").Length > 0);
        
        // These recorded mappings can now be used as a basis for your test mocks
        // and help identify all possible response patterns
    }
    
    [TestMethod]
    public async Task AnalyzeRecordedMappings()
    {
        // Once recordings exist, you can analyze them programmatically
        if (!Directory.Exists("RecordedMappings"))
        {
            return; // Skip if no recordings exist
        }
        
        var mappingFiles = Directory.GetFiles("RecordedMappings", "*.json");
        foreach (var file in mappingFiles)
        {
            var content = await File.ReadAllTextAsync(file);
            Console.WriteLine($"Found recorded mapping: {Path.GetFileName(file)}");
            // Could parse and analyze the JSON to understand patterns
        }
    }
    
    [TestCleanup]
    public void Cleanup()
    {
        _client.Dispose();
        _factory.Dispose();
        _mockServer.Stop();
    }
}
```

For an even more comprehensive approach, you can create a dedicated utility for recording and analyzing API interactions:

```csharp
public class ApiInteractionRecorder
{
    private readonly string _recordingsFolder;
    private readonly WireMockServer _server;
    
    public ApiInteractionRecorder(string targetBaseUrl, string recordingsFolder = "ApiRecordings")
    {
        _recordingsFolder = recordingsFolder;
        
        // Create recordings directory if it doesn't exist
        Directory.CreateDirectory(recordingsFolder);
        
        // Initialize WireMock server
        _server = WireMockServer.Start(new WireMockServerSettings
        {
            ProxyAndRecordSettings = new ProxyAndRecordSettings
            {
                Url = targetBaseUrl,
                SaveMapping = true,
                SaveMappingToFile = true,
                MappingFolder = recordingsFolder
            }
        });
    }
    
    public HttpClient CreateClientForRecording()
    {
        return new HttpClient
        {
            BaseAddress = new Uri(_server.Urls[0])
        };
    }
    
    public List<ApiEndpointStats> AnalyzeRecordings()
    {
        var mappingFiles = Directory.GetFiles(_recordingsFolder, "*.json");
        var endpointStats = new Dictionary<string, ApiEndpointStats>();
        
        foreach (var file in mappingFiles)
        {
            var mapping = JsonSerializer.Deserialize<WireMockMapping>(
                File.ReadAllText(file));
                
            var path = mapping.Request.Path;
            var method = mapping.Request.Method;
            var statusCode = mapping.Response.StatusCode;
            
            var key = $"{method} {path}";
            if (!endpointStats.ContainsKey(key))
            {
                endpointStats[key] = new ApiEndpointStats
                {
                    Method = method,
                    Path = path,
                    ResponseCodes = new HashSet<int>()
                };
            }
            
            endpointStats[key].ResponseCodes.Add(statusCode);
            endpointStats[key].CallCount++;
        }
        
        return endpointStats.Values.ToList();
    }
    
    public void Dispose()
    {
        _server?.Stop();
    }
}

public class ApiEndpointStats
{
    public string Method { get; set; }
    public string Path { get; set; }
    public HashSet<int> ResponseCodes { get; set; } = new HashSet<int>();
    public int CallCount { get; set; }
    
    public bool HasMultipleResponsePatterns => ResponseCodes.Count > 1;
    
    public override string ToString()
    {
        return $"{Method} {Path}: {CallCount} calls, Response codes: {string.Join(", ", ResponseCodes)}";
    }
}
```

This approach allows you to:

1. Record real API interactions during exploratory testing
2. Discover all endpoints your application interacts with
3. Identify response variations that need to be handled in your code
4. Generate realistic mock responses for your tests

With this information, you can ensure your tests cover all possible integration scenarios and response patterns, preventing surprises in production.

## Dealing with Data

### ✅ Do: Make each test work with its own data

Each test should create the data it needs to run, rather than depending on data created by other tests. This makes tests self-contained and easier to understand.

**Example:**
```csharp
[TestClass]
public class OrderTests
{
    private WebApplicationFactory<Program> _factory;
    private HttpClient _client;
    
    [TestInitialize]
    public void Initialize()
    {
        _factory = new WebApplicationFactory<Program>();
        _client = _factory.CreateClient();
    }
    
    [TestMethod]
    public async Task GetOrder_ReturnsCorrectOrder()
    {
        // Arrange - Create test data specifically for this test
        var orderToCreate = new OrderDto
        {
            UserId = 1,
            ProductId = 2,
            Quantity = 3
        };
        
        var createResponse = await _client.PostAsJsonAsync("/api/orders", orderToCreate);
        var createdOrder = await createResponse.Content.ReadFromJsonAsync<OrderResponseDto>();
        
        // Act
        var getResponse = await _client.GetAsync($"/api/orders/{createdOrder.Id}");
        
        // Assert
        getResponse.EnsureSuccessStatusCode();
        var retrievedOrder = await getResponse.Content.ReadFromJsonAsync<OrderResponseDto>();
        Assert.AreEqual(createdOrder.Id, retrievedOrder.Id);
        Assert.AreEqual(orderToCreate.UserId, retrievedOrder.UserId);
        Assert.AreEqual(orderToCreate.ProductId, retrievedOrder.ProductId);
        Assert.AreEqual(orderToCreate.Quantity, retrievedOrder.Quantity);
    }
    
    [TestMethod]
    public async Task UpdateOrder_UpdatesCorrectFields()
    {
        // Arrange - Create test data specifically for this test
        var orderToCreate = new OrderDto
        {
            UserId = 1,
            ProductId = 2,
            Quantity = 3
        };
        
        var createResponse = await _client.PostAsJsonAsync("/api/orders", orderToCreate);
        var createdOrder = await createResponse.Content.ReadFromJsonAsync<OrderResponseDto>();
        
        var updateData = new OrderUpdateDto
        {
            Quantity = 5
        };
        
        // Act
        var updateResponse = await _client.PutAsJsonAsync($"/api/orders/{createdOrder.Id}", updateData);
        
        // Assert
        updateResponse.EnsureSuccessStatusCode();
        
        var getResponse = await _client.GetAsync($"/api/orders/{createdOrder.Id}");
        var updatedOrder = await getResponse.Content.ReadFromJsonAsync<OrderResponseDto>();
        Assert.AreEqual(createdOrder.Id, updatedOrder.Id);
        Assert.AreEqual(orderToCreate.UserId, updatedOrder.UserId); 
        Assert.AreEqual(orderToCreate.ProductId, updatedOrder.ProductId);
        Assert.AreEqual(updateData.Quantity, updatedOrder.Quantity);
    }
    
    [TestCleanup]
    public void Cleanup()
    {
        _client.Dispose();
        _factory.Dispose();
    }
}
```

### ✅ Do: Seed metadata and context data appropriately

Different types of data require different seeding approaches:

1. **Metadata** - Reference data like countries, currencies, etc., can be seeded once globally
2. **Context data** - Parent entities that test data depends on, seed per test class
3. **Test records** - The actual data being tested, should be created within each test

**Example:**
```csharp
[TestClass]
public class OrderTestsWithDataSeeding
{
    private static TestcontainersContainer _dbContainer;
    private static string _connectionString;
    private WebApplicationFactory<Program> _factory;
    private HttpClient _client;
    private int _userId;
    
    [ClassInitialize]
    public static async Task ClassInitialize(TestContext context)
    {
        // Start database container
        _dbContainer = new ContainerBuilder()
            .WithImage("postgres:13")
            .WithEnvironment("POSTGRES_PASSWORD", "postgres")
            .WithPortBinding(5432, true)
            .Build();
            
        await _dbContainer.StartAsync();
        
        var mappedPort = _dbContainer.GetMappedPublicPort(5432);
        _connectionString = $"Host=localhost;Port={mappedPort};Database=postgres;Username=postgres;Password=postgres";
        
        // Seed metadata (once for all tests)
        await SeedMetadata(_connectionString);
    }
    
    private static async Task SeedMetadata(string connectionString)
    {
        // Seed global reference data like countries, currencies, etc.
        using var connection = new Npgsql.NpgsqlConnection(connectionString);
        await connection.OpenAsync();
        
        using var cmd = connection.CreateCommand();
        cmd.CommandText = @"
            INSERT INTO countries (code, name) VALUES 
                ('US', 'United States'), 
                ('CA', 'Canada'),
                ('MX', 'Mexico')
            ON CONFLICT DO NOTHING;
            
            INSERT INTO currencies (code, name, symbol) VALUES
                ('USD', 'US Dollar', '$'),
                ('CAD', 'Canadian Dollar', 'C$'),
                ('MXN', 'Mexican Peso', '₱')
            ON CONFLICT DO NOTHING;";
            
        await cmd.ExecuteNonQueryAsync();
    }
    
    [TestInitialize]
    public async Task TestInitialize()
    {
        // Set up test context data per test class
        _factory = new WebApplicationFactory<Program>()
            .WithWebHostBuilder(builder =>
            {
                builder.ConfigureServices(services =>
                {
                    // Replace the DB context with one pointing to the test container
                    services.Remove(services.Single(d => 
                        d.ServiceType == typeof(DbContextOptions<AppDbContext>)));
                        
                    services.AddDbContext<AppDbContext>(options =>
                        options.UseNpgsql(_connectionString));
                });
            });
            
        _client = _factory.CreateClient();
        
        // Create context data (parent entities) for this test class
        _userId = await CreateTestUser();
    }
    
    private async Task<int> CreateTestUser()
    {
        var newUser = new UserDto
        {
            Name = "Test User",
            Email = "test@example.com"
        };
        
        var response = await _client.PostAsJsonAsync("/api/users", newUser);
        response.EnsureSuccessStatusCode();
        
        var user = await response.Content.ReadFromJsonAsync<UserResponseDto>();
        return user.Id;
    }
    
    [TestMethod]
    public async Task CreateOrder_WithValidData_SavesCorrectly()
    {
        // Create test record specific to this test
        var orderToCreate = new OrderDto
        {
            UserId = _userId, // Use context data
            ProductId = 123,
            Quantity = 2,
            Currency = "USD" // Use metadata
        };
        
        var response = await _client.PostAsJsonAsync("/api/orders", orderToCreate);
        response.EnsureSuccessStatusCode();
        
        var order = await response.Content.ReadFromJsonAsync<OrderResponseDto>();
        Assert.IsNotNull(order.Id);
        Assert.AreEqual(_userId, order.UserId);
        Assert.AreEqual(123, order.ProductId);
    }
    
    [ClassCleanup]
    public static async Task ClassCleanup()
    {
        await _dbContainer.StopAsync();
    }
    
    [TestCleanup]
    public void TestCleanup()
    {
        _client.Dispose();
        _factory.Dispose();
    }
}
```

### ✅ Do: Assert data state using public APIs

After actions that change data, verify the changes by calling the appropriate API endpoint rather than checking the database directly. This tests the full stack and catches issues in the retrieval code.

**Example:**
```csharp
[TestMethod]
public async Task UpdateOrder_SavesChangesCorrectly()
{
    // Arrange - Create an order
    var createDto = new OrderDto
    {
        UserId = 1,
        ProductId = 2,
        Quantity = 3
    };
    
    var createResponse = await _client.PostAsJsonAsync("/api/orders", createDto);
    var createdOrder = await createResponse.Content.ReadFromJsonAsync<OrderResponseDto>();
    
    // Update the order
    var updateDto = new OrderUpdateDto
    {
        Quantity = 5
    };
    
    // Act
    var updateResponse = await _client.PutAsJsonAsync($"/api/orders/{createdOrder.Id}", updateDto);
    updateResponse.EnsureSuccessStatusCode();
    
    // Assert - Check via API that changes were saved
    var getResponse = await _client.GetAsync($"/api/orders/{createdOrder.Id}");
    getResponse.EnsureSuccessStatusCode();
    
    var updatedOrder = await getResponse.Content.ReadFromJsonAsync<OrderResponseDto>();
    Assert.AreEqual(5, updatedOrder.Quantity); // Verify data was updated
}
```

### ✅ Do: Use a clear data cleanup strategy

Choose a consistent approach to cleaning test data. The two main options are:

1. **After-each cleanup** - Clean data after each test for isolation
2. **After-all cleanup** - Clean data after all tests, more performant but requires careful test design

For most cases, after-all cleanup with careful test design is recommended.

**Example:**
```csharp
[TestClass]
public class OrderTests
{
    private static TestcontainersContainer _dbContainer;
    private static string _connectionString;
    private WebApplicationFactory<Program> _factory;
    private HttpClient _client;
    
    [ClassInitialize]
    public static async Task ClassInitialize(TestContext context)
    {
        // Start database container
        _dbContainer = new ContainerBuilder()
            .WithImage("postgres:13")
            .WithEnvironment("POSTGRES_PASSWORD", "postgres")
            .WithPortBinding(5432, true)
            .Build();
            
        await _dbContainer.StartAsync();
        
        var mappedPort = _dbContainer.GetMappedPublicPort(5432);
        _connectionString = $"Host=localhost;Port={mappedPort};Database=postgres;Username=postgres;Password=postgres";
    }
    
    [TestInitialize]
    public void TestInitialize()
    {
        _factory = new WebApplicationFactory<Program>()
            .WithWebHostBuilder(builder =>
            {
                builder.ConfigureServices(services =>
                {
                    services.Remove(services.Single(d => 
                        d.ServiceType == typeof(DbContextOptions<AppDbContext>)));
                        
                    services.AddDbContext<AppDbContext>(options =>
                        options.UseNpgsql(_connectionString));
                });
            });
            
        _client = _factory.CreateClient();
    }
    
    [TestMethod]
    public async Task GetOrder_AfterCreation_ReturnsOrder()
    {
        // Use unique identifiers to avoid collisions with other tests
        var uniqueId = Guid.NewGuid().ToString("N").Substring(0, 8);
        var orderToCreate = new OrderDto
        {
            UserId = 1,
            ProductId = 2,
            Quantity = 3,
            Reference = $"ORDER-{uniqueId}" // Unique reference for this test
        };
        
        var createResponse = await _client.PostAsJsonAsync("/api/orders", orderToCreate);
        var createdOrder = await createResponse.Content.ReadFromJsonAsync<OrderResponseDto>();
        
        var getResponse = await _client.GetAsync($"/api/orders/{createdOrder.Id}");
        getResponse.EnsureSuccessStatusCode();
        
        var retrievedOrder = await getResponse.Content.ReadFromJsonAsync<OrderResponseDto>();
        Assert.AreEqual(orderToCreate.Reference, retrievedOrder.Reference);
    }
    
    [ClassCleanup]
    public static async Task ClassCleanup()
    {
        // Clean up data after all tests have run
        using var connection = new Npgsql.NpgsqlConnection(_connectionString);
        await connection.OpenAsync();
        
        using var cmd = connection.CreateCommand();
        cmd.CommandText = "TRUNCATE orders CASCADE";
        await cmd.ExecuteNonQueryAsync();
        
        await _dbContainer.StopAsync();
    }
    
    [TestCleanup]
    public void TestCleanup()
    {
        _client.Dispose();
        _factory.Dispose();
    }
}
```

### ✅ Do: Add randomness to unique fields

Use random values or GUIDs for fields that must be unique to prevent collisions between tests, especially when not cleaning up data after each test.

**Example:**
```csharp
[TestMethod]
public async Task CreateOrder_WithUniqueFields_Succeeds()
{
    // Generate unique values for fields that must be unique
    var uniqueRef = $"REF-{Guid.NewGuid().ToString("N").Substring(0, 8)}";
    var uniqueTrackingId = $"TRK-{DateTime.UtcNow.Ticks}";
    
    var orderToCreate = new OrderDto
    {
        UserId = 1,
        ProductId = 2,
        Quantity = 3,
        Reference = uniqueRef, // Unique reference
        TrackingId = uniqueTrackingId // Unique tracking ID
    };
    
    var response = await _client.PostAsJsonAsync("/api/orders", orderToCreate);
    response.EnsureSuccessStatusCode();
    
    var createdOrder = await response.Content.ReadFromJsonAsync<OrderResponseDto>();
    Assert.AreEqual(uniqueRef, createdOrder.Reference);
    Assert.AreEqual(uniqueTrackingId, createdOrder.TrackingId);
}
```

### ✅ Do: Test response schema for dynamic data

When testing responses with dynamic fields (like timestamps or generated IDs), assert on the schema and types rather than exact values.

**Example:**
```csharp
[TestMethod]
public async Task CreateOrder_ResponseHasExpectedSchema()
{
    // Arrange
    var orderToCreate = new OrderDto
    {
        UserId = 1,
        ProductId = 2,
        Quantity = 3
    };
    
    // Act
    var response = await _client.PostAsJsonAsync("/api/orders", orderToCreate);
    response.EnsureSuccessStatusCode();
    
    // Read response as JSON to check schema
    var responseJson = await response.Content.ReadFromJsonAsync<JsonElement>();
    
    // Assert schema is correct
    Assert.IsTrue(responseJson.TryGetProperty("id", out var idProp));
    Assert.AreEqual(JsonValueKind.Number, idProp.ValueKind);
    
    Assert.IsTrue(responseJson.TryGetProperty("createdAt", out var createdAtProp));
    Assert.AreEqual(JsonValueKind.String, createdAtProp.ValueKind);
    // Validate it's a valid datetime
    Assert.IsTrue(DateTime.TryParse(createdAtProp.GetString(), out _));
    
    Assert.IsTrue(responseJson.TryGetProperty("userId", out var userIdProp));
    Assert.AreEqual(1, userIdProp.GetInt32());
    
    Assert.IsTrue(responseJson.TryGetProperty("productId", out var productIdProp));
    Assert.AreEqual(2, productIdProp.GetInt32());
}
```

### ✅ Do: Test for unintended side effects

Check that operations affect only the data they're supposed to, not other records. This catches issues like accidentally deleting or modifying unrelated data.

**Example:**
```csharp
[TestMethod]
public async Task DeleteOrder_OnlyDeletesSpecifiedOrder()
{
    // Arrange - Create two orders
    var order1 = new OrderDto { UserId = 1, ProductId = 2, Quantity = 1 };
    var order2 = new OrderDto { UserId = 1, ProductId = 3, Quantity = 2 };
    
    var response1 = await _client.PostAsJsonAsync("/api/orders", order1);
    var response2 = await _client.PostAsJsonAsync("/api/orders", order2);
    
    var createdOrder1 = await response1.Content.ReadFromJsonAsync<OrderResponseDto>();
    var createdOrder2 = await response2.Content.ReadFromJsonAsync<OrderResponseDto>();
    
    // Act - Delete only the first order
    var deleteResponse = await _client.DeleteAsync($"/api/orders/{createdOrder1.Id}");
    deleteResponse.EnsureSuccessStatusCode();
    
    // Assert - Verify first order is gone but second still exists
    var getResponse1 = await _client.GetAsync($"/api/orders/{createdOrder1.Id}");
    Assert.AreEqual(System.Net.HttpStatusCode.NotFound, getResponse1.StatusCode);
    
    var getResponse2 = await _client.GetAsync($"/api/orders/{createdOrder2.Id}");
    getResponse2.EnsureSuccessStatusCode();
    
    var retrievedOrder2 = await getResponse2.Content.ReadFromJsonAsync<OrderResponseDto>();
    Assert.AreEqual(createdOrder2.Id, retrievedOrder2.Id);
}
```

## Message Queues

### ✅ Do: Use in-memory fakes for most message queue tests

Create simple in-memory implementations of message queue interfaces for testing. This provides better control, faster execution, and more predictable behavior than real message queues.

**Example:**
```csharp
// Define the interface for your message queue
public interface IMessageQueue
{
    Task PublishAsync<T>(string topic, T message);
    Task SubscribeAsync<T>(string topic, Func<T, Task> handler);
    Task AcknowledgeAsync(string messageId);
}

// Create an in-memory implementation for testing
public class InMemoryMessageQueue : IMessageQueue
{
    private readonly Dictionary<string, List<Func<object, Task>>> _subscribers = new();
    private readonly List<PublishedMessage> _publishedMessages = new();
    private readonly List<string> _acknowledgedMessages = new();
    private readonly TaskCompletionSource<bool> _messageProcessedTcs = new();
    
    public IReadOnlyList<PublishedMessage> PublishedMessages => _publishedMessages;
    public IReadOnlyList<string> AcknowledgedMessages => _acknowledgedMessages;
    public Task WaitForMessageProcessedAsync() => _messageProcessedTcs.Task;
    
    public Task PublishAsync<T>(string topic, T message)
    {
        var messageId = Guid.NewGuid().ToString();
        _publishedMessages.Add(new PublishedMessage
        {
            Id = messageId,
            Topic = topic,
            Payload = message
        });
        
        if (_subscribers.TryGetValue(topic, out var handlers))
        {
            foreach (var handler in handlers)
            {
                _ = handler(message).ContinueWith(_ => _messageProcessedTcs.TrySetResult(true));
            }
        }
        
        return Task.CompletedTask;
    }
    
    public Task SubscribeAsync<T>(string topic, Func<T, Task> handler)
    {
        if (!_subscribers.ContainsKey(topic))
        {
            _subscribers[topic] = new List<Func<object, Task>>();
        }
        
        _subscribers[topic].Add(message => handler((T)message));
        return Task.CompletedTask;
    }
    
    public Task AcknowledgeAsync(string messageId)
    {
        _acknowledgedMessages.Add(messageId);
        return Task.CompletedTask;
    }
    
    public class PublishedMessage
    {
        public string Id { get; set; }
        public string Topic { get; set; }
        public object Payload { get; set; }
    }
}

// Test using the in-memory queue
[TestClass]
public class OrderProcessingMessageTests
{
    private WebApplicationFactory<Program> _factory;
    private HttpClient _client;
    private InMemoryMessageQueue _messageQueue;
    
    [TestInitialize]
    public void Initialize()
    {
        _messageQueue = new InMemoryMessageQueue();
        
        _factory = new WebApplicationFactory<Program>()
            .WithWebHostBuilder(builder =>
            {
                builder.ConfigureServices(services =>
                {
                    services.AddSingleton<IMessageQueue>(_messageQueue);
                });
            });
            
        _client = _factory.CreateClient();
    }
    
    [TestMethod]
    public async Task CreateOrder_PublishesOrderCreatedEvent()
    {
        // Arrange
        var order = new OrderDto
        {
            UserId = 1,
            ProductId = 2,
            Quantity = 3
        };
        
        // Act
        var response = await _client.PostAsJsonAsync("/api/orders", order);
        response.EnsureSuccessStatusCode();
        
        var createdOrder = await response.Content.ReadFromJsonAsync<OrderResponseDto>();
        
        // Wait for message processing to complete
        await _messageQueue.WaitForMessageProcessedAsync();
        
        // Assert
        Assert.IsTrue(_messageQueue.PublishedMessages.Any(m => 
            m.Topic == "order-created" && 
            ((OrderCreatedEvent)m.Payload).OrderId == createdOrder.Id));
    }
    
    [TestCleanup]
    public void Cleanup()
    {
        _client.Dispose();
        _factory.Dispose();
    }
}
```

### ✅ Do: Use async/await patterns for message queue tests

Make message queue tests easy to follow by using async/await and avoiding callback patterns. Use TaskCompletionSource to convert event-based callbacks to awaitable tasks.

**Example:**
```csharp
[TestMethod]
public async Task ProcessOrder_WhenMessageReceived_UpdatesOrderStatus()
{
    // Arrange
    var order = new OrderDto { UserId = 1, ProductId = 2, Quantity = 3 };
    var createResponse = await _client.PostAsJsonAsync("/api/orders", order);
    var createdOrder = await createResponse.Content.ReadFromJsonAsync<OrderResponseDto>();
    
    // Set up message handler
    var processingComplete = new TaskCompletionSource<bool>();
    await _messageQueue.SubscribeAsync<OrderStatusUpdatedEvent>("order-status-updated", async evt => 
    {
        if (evt.OrderId == createdOrder.Id && evt.NewStatus == "Processed")
        {
            processingComplete.SetResult(true);
        }
        await Task.CompletedTask;
    });
    
    // Act - Publish message to process the order
    await _messageQueue.PublishAsync("process-order", new ProcessOrderCommand
    {
        OrderId = createdOrder.Id
    });
    
    // Wait for processing to complete with timeout
    await Task.WhenAny(processingComplete.Task, Task.Delay(TimeSpan.FromSeconds(5)));
    
    // Assert
    Assert.IsTrue(processingComplete.Task.IsCompletedSuccessfully);
    
    // Verify order status was updated
    var getResponse = await _client.GetAsync($"/api/orders/{createdOrder.Id}");
    var updatedOrder = await getResponse.Content.ReadFromJsonAsync<OrderResponseDto>();
    Assert.AreEqual("Processed", updatedOrder.Status);
}
```

### ✅ Do: Test message acknowledgment

Verify that your code properly acknowledges messages after successful processing and does not acknowledge them when processing fails.

**Example:**
```csharp
[TestClass]
public class MessageAcknowledgmentTests
{
    private WebApplicationFactory<Program> _factory;
    private InMemoryMessageQueue _messageQueue;
    
    [TestInitialize]
    public void Initialize()
    {
        _messageQueue = new InMemoryMessageQueue();
        
        _factory = new WebApplicationFactory<Program>()
            .WithWebHostBuilder(builder =>
            {
                builder.ConfigureServices(services =>
                {
                    services.AddSingleton<IMessageQueue>(_messageQueue);
                    services.AddHostedService<OrderProcessingService>();
                });
            });
            
        var sp = _factory.Services;
        var orderProcessor = sp.GetRequiredService<OrderProcessingService>();
        orderProcessor.StartAsync(CancellationToken.None).Wait();
    }
    
    [TestMethod]
    public async Task ProcessOrderMessage_WhenSuccessful_AcknowledgesMessage()
    {
        // Arrange - Create an order to process
        var dbContext = _factory.Services.GetRequiredService<AppDbContext>();
        var order = new Order { UserId = 1, ProductId = 2, Quantity = 3, Status = "Pending" };
        dbContext.Orders.Add(order);
        await dbContext.SaveChangesAsync();
        
        // Create a process order message
        var messageId = Guid.NewGuid().ToString();
        var message = new ProcessOrderMessage
        {
            Id = messageId,
            OrderId = order.Id
        };
        
        // Act - Publish the message
        await _messageQueue.PublishAsync("process-order", message);
        
        // Wait for processing to complete
        await Task.Delay(100);
        
        // Assert - Message should be acknowledged
        Assert.IsTrue(_messageQueue.AcknowledgedMessages.Contains(messageId));
        
        // Check order status was updated
        await dbContext.Entry(order).ReloadAsync();
        Assert.AreEqual("Processed", order.Status);
    }
    
    [TestMethod]
    public async Task ProcessOrderMessage_WhenFails_DoesNotAcknowledgeMessage()
    {
        // Arrange - Create a process order message with non-existent order ID
        var messageId = Guid.NewGuid().ToString();
        var message = new ProcessOrderMessage
        {
            Id = messageId,
            OrderId = 999999 // Non-existent order ID
        };
        
        // Act - Publish the message
        await _messageQueue.PublishAsync("process-order", message);
        
        // Wait for processing attempt to complete
        await Task.Delay(100);
        
        // Assert - Message should NOT be acknowledged
        Assert.IsFalse(_messageQueue.AcknowledgedMessages.Contains(messageId));
    }
    
    [TestCleanup]
    public void Cleanup()
    {
        _factory.Dispose();
    }
}
```

### ✅ Do: Test processing of message batches

Test how your code handles batches of messages, including scenarios where some messages in a batch fail. This verifies that your batch processing logic is robust.

**Example:**
```csharp
[TestMethod]
public async Task ProcessBatchOfMessages_HandlesPartialFailures()
{
    // Arrange - Create several orders, some valid and some invalid
    var dbContext = _factory.Services.GetRequiredService<AppDbContext>();
    
    var validOrder1 = new Order { Id = 1, UserId = 1, ProductId = 2, Status = "Pending" };
    var validOrder2 = new Order { Id = 2, UserId = 1, ProductId = 3, Status = "Pending" };
    dbContext.Orders.AddRange(validOrder1, validOrder2);
    await dbContext.SaveChangesAsync();
    
    // Prepare batch of messages
    var messages = new List<ProcessOrderMessage>
    {
        new ProcessOrderMessage { Id = "msg1", OrderId = 1 }, // Valid
        new ProcessOrderMessage { Id = "msg2", OrderId = 999 }, // Invalid - non-existent order
        new ProcessOrderMessage { Id = "msg3", OrderId = 2 }, // Valid
    };
    
    // Set up message processor to handle batches
    var orderProcessor = _factory.Services.GetRequiredService<OrderBatchProcessor>();
    
    // Act - Process batch of messages
    await orderProcessor.ProcessBatchAsync(messages);
    
    // Assert - Check acknowledgments
    Assert.IsTrue(_messageQueue.AcknowledgedMessages.Contains("msg1"));
    Assert.IsFalse(_messageQueue.AcknowledgedMessages.Contains("msg2"));
    Assert.IsTrue(_messageQueue.AcknowledgedMessages.Contains("msg3"));
    
    // Check valid orders were processed
    await dbContext.Entry(validOrder1).ReloadAsync();
    await dbContext.Entry(validOrder2).ReloadAsync();
    Assert.AreEqual("Processed", validOrder1.Status);
    Assert.AreEqual("Processed", validOrder2.Status);
}
```

### ✅ Do: Test for poisoned messages

Verify that your code properly handles invalid messages without crashing or blocking other messages. This tests your application's resilience against malformed data.

**Example:**
```csharp
[TestMethod]
public async Task ProcessMessage_WithInvalidFormat_LogsErrorAndContinues()
{
    // Arrange
    var messageQueue = new InMemoryMessageQueue();
    var loggerMock = A.Fake<ILogger<OrderProcessor>>();
    var orderProcessor = new OrderProcessor(messageQueue, loggerMock);
    
    // Start the processor
    await orderProcessor.StartAsync(CancellationToken.None);
    
    // Act - Send malformed message (missing required fields)
    var invalidMessage = new { SomeField = "value" }; // Not a valid order message
    await messageQueue.PublishAsync("orders", invalidMessage);
    
    // Send a valid message right after
    var validMessage = new ProcessOrderMessage { OrderId = 1 };
    await messageQueue.PublishAsync("orders", validMessage);
    
    // Wait for processing to complete
    await Task.Delay(100);
    
    // Assert
    // Verify error was logged
    A.CallTo(() => loggerMock.Log(
        LogLevel.Error,
        A<EventId>._,
        A<It.IsAnyType>.That.Matches(o => o.ToString().Contains("Invalid message format")),
        A<Exception>._,
        A<Func<It.IsAnyType, Exception, string>>._))
        .MustHaveHappened();
    
    // Verify only the invalid message was rejected, and the valid one was processed
    Assert.IsFalse(messageQueue.AcknowledgedMessages.Contains(invalidMessage.ToString()));
    Assert.IsTrue(messageQueue.PublishedMessages.Any(m => 
        m.Topic == "order-processed" && 
        ((OrderProcessedEvent)m.Payload).OrderId == 1));
}
```

### ✅ Do: Test for idempotency

Verify that receiving the same message multiple times doesn't cause duplicate operations or side effects. This is critical for message-based systems where "at least once" delivery is common.

**Example:**
```csharp
[TestMethod]
public async Task ProcessOrderMessage_WhenMessageReceivesTwice_ProcessesOnlyOnce()
{
    // Arrange - Create an order to process
    var dbContext = _factory.Services.GetRequiredService<AppDbContext>();
    var order = new Order 
    { 
        Id = 1, 
        UserId = 1, 
        ProductId = 100, 
        Quantity = 1, 
        Status = "Pending",
        PaymentAmount = 0 // Initially not charged
    };
    dbContext.Orders.Add(order);
    await dbContext.SaveChangesAsync();
    
    // Create a payment processor service
    var paymentProcessor = _factory.Services.GetRequiredService<PaymentProcessorService>();
    
    // Create a payment message with unique idempotency key
    var paymentMessage = new ProcessPaymentMessage
    {
        OrderId = order.Id,
        Amount = 99.99m,
        IdempotencyKey = "payment-123" // Same key for duplicate messages
    };
    
    // Act - Process the same message twice
    await paymentProcessor.ProcessAsync(paymentMessage);
    await paymentProcessor.ProcessAsync(paymentMessage); // Duplicate message
    
    // Assert - Payment should only be processed once
    await dbContext.Entry(order).ReloadAsync();
    Assert.AreEqual(99.99m, order.PaymentAmount);
    
    // Check payment records
    var payments = await dbContext.Payments
        .Where(p => p.OrderId == order.Id)
        .ToListAsync();
    
    // Ensure only one payment was created
    Assert.AreEqual(1, payments.Count);
    Assert.AreEqual(99.99m, payments[0].Amount);
}
```

### ✅ Do: Test connection failures

Test how your application handles connection issues with message queues, including initial connection failures and disconnections during operation.

**Example:**
```csharp
[TestMethod]
public async Task QueueClient_WhenConnectionFails_RetrySuccessfully()
{
    // Arrange
    var loggerMock = A.Fake<ILogger<MessageQueueClient>>();
    var connectionFailureCount = 0;
    
    // Create a mock connection factory that fails the first 2 attempts
    var connectionFactoryMock = A.Fake<IConnectionFactory>();
    A.CallTo(() => connectionFactoryMock.CreateConnectionAsync(A<CancellationToken>._))
        .ReturnsLazily((CancellationToken token) => 
        {
            connectionFailureCount++;
            if (connectionFailureCount <= 2)
            {
                throw new TimeoutException("Connection timeout");
            }
            
            // After 2 failures, return a successful connection
            var connectionMock = A.Fake<IConnection>();
            return Task.FromResult(connectionMock);
        });
    
    var client = new MessageQueueClient(connectionFactoryMock, loggerMock);
    
    // Act
    await client.StartAsync(CancellationToken.None);
    
    // Assert
    A.CallTo(() => connectionFactoryMock.CreateConnectionAsync(A<CancellationToken>._))
        .MustHaveHappened(3, Times.Exactly); // 2 failures + 1 success
    
    // Verify the connection failures were logged
    A.CallTo(() => loggerMock.Log(
        LogLevel.Error,
        A<EventId>._,
        A<It.IsAnyType>.That.Matches(o => o.ToString().Contains("Connection timeout")),
        A<TimeoutException>._,
        A<Func<It.IsAnyType, Exception, string>>._))
        .MustHaveHappened(2, Times.Exactly);
    
    // Verify success was logged
    A.CallTo(() => loggerMock.Log(
        LogLevel.Information,
        A<EventId>._,
        A<It.IsAnyType>.That.Matches(o => o.ToString().Contains("Connected")),
        A<Exception>._,
        A<Func<It.IsAnyType, Exception, string>>._))
        .MustHaveHappened();
    
    // Client should be in connected state
    Assert.IsTrue(client.IsConnected);
}

[TestMethod]
public async Task QueueClient_WhenConnectionBreaksDuringOperation_ReconnectsAutomatically()
{
    // Arrange
    var loggerMock = A.Fake<ILogger<MessageQueueClient>>();
    var disconnectEvent = new TaskCompletionSource<bool>();
    var reconnectEvent = new TaskCompletionSource<bool>();
    
    // Create mock connection that can be disconnected
    var connectionMock = A.Fake<IConnection>();
    var connectionFactoryMock = A.Fake<IConnectionFactory>();
    
    A.CallTo(() => connectionFactoryMock.CreateConnectionAsync(A<CancellationToken>._))
        .Returns(Task.FromResult(connectionMock));
    
    // Setup disconnect/reconnect behavior
    var client = new MessageQueueClient(connectionFactoryMock, loggerMock);
    client.ConnectionClosed += (s, e) => disconnectEvent.TrySetResult(true);
    client.ConnectionRestored += (s, e) => reconnectEvent.TrySetResult(true);
    
    // Act - Start client
    await client.StartAsync(CancellationToken.None);
    
    // Simulate connection break
    var connectionClosedEventHandler = A.Fake<EventHandler<ConnectionClosedEventArgs>>();
    A.CallTo(() => connectionMock.ConnectionClosed += connectionClosedEventHandler)
        .Invokes(() => 
        {
            // Trigger connection closed event
            connectionClosedEventHandler(connectionMock, new ConnectionClosedEventArgs());
        });
    
    // Wait for disconnect and reconnect events
    await Task.WhenAny(disconnectEvent.Task, Task.Delay(1000));
    await Task.WhenAny(reconnectEvent.Task, Task.Delay(1000));
    
    // Assert
    Assert.IsTrue(disconnectEvent.Task.IsCompletedSuccessfully);
    Assert.IsTrue(reconnectEvent.Task.IsCompletedSuccessfully);
    
    // Verify reconnection attempt was made
    A.CallTo(() => connectionFactoryMock.CreateConnectionAsync(A<CancellationToken>._))
        .MustHaveHappened(2, Times.AtLeast); // Initial + reconnect
    
    // Client should be in connected state
    Assert.IsTrue(client.IsConnected);
}
```

### ✅ Do: Include some E2E tests with real message queues

Include a small number of end-to-end tests with real message queues to verify configuration and integration issues that mocks might miss.

**Example:**
```csharp
[TestClass]
public class RealMessageQueueE2ETests
{
    private TestcontainersContainer _rabbitMqContainer;
    private WebApplicationFactory<Program> _factory;
    private HttpClient _client;
    private string _rabbitMqConnectionString;
    
    [TestInitialize]
    public async Task Initialize()
    {
        // Start RabbitMQ container
        _rabbitMqContainer = new ContainerBuilder()
            .WithImage("rabbitmq:3-management")
            .WithPortBinding(5672, true) // AMQP port
            .WithPortBinding(15672, true) // Management UI
            .WithWaitStrategy(Wait.ForUnixContainer().UntilPortIsAvailable(5672))
            .Build();
            
        await _rabbitMqContainer.StartAsync();
        
        var mappedPort = _rabbitMqContainer.GetMappedPublicPort(5672);
        _rabbitMqConnectionString = $"amqp://guest:guest@localhost:{mappedPort}";
        
        // Configure the application with the real RabbitMQ
        _factory = new WebApplicationFactory<Program>()
            .WithWebHostBuilder(builder =>
            {
                builder.ConfigureServices(services =>
                {
                    services.Configure<RabbitMQOptions>(options =>
                    {
                        options.ConnectionString = _rabbitMqConnectionString;
                    });
                });
            });
            
        _client = _factory.CreateClient();
    }
    
    [TestMethod]
    public async Task EndToEndOrderProcessing_WithRealMessageQueue()
    {
        // Arrange - Create a handler for the processed order event
        var orderProcessedEvent = new TaskCompletionSource<int>();
        
        // Connect to RabbitMQ and subscribe to order-processed events
        var factory = new ConnectionFactory { Uri = new Uri(_rabbitMqConnectionString) };
        using var connection = factory.CreateConnection();
        using var channel = connection.CreateModel();
        
        // Declare the queue
        channel.QueueDeclare("order-processed", durable: true, exclusive: false, autoDelete: false);
        
        // Subscribe to processed order events
        var consumer = new EventingBasicConsumer(channel);
        consumer.Received += (sender, args) =>
        {
            var body = args.Body.ToArray();
            var message = System.Text.Encoding.UTF8.GetString(body);
            var orderId = int.Parse(JsonSerializer.Deserialize<JsonElement>(message)
                .GetProperty("orderId").GetString());
                
            orderProcessedEvent.TrySetResult(orderId);
            channel.BasicAck(args.DeliveryTag, false);
        };
        
        channel.BasicConsume("order-processed", false, consumer);
        
        // Act - Create an order
        var order = new OrderDto
        {
            UserId = 1,
            ProductId = 100,
            Quantity = 1
        };
        
        var response = await _client.PostAsJsonAsync("/api/orders", order);
        response.EnsureSuccessStatusCode();
        
        var createdOrder = await response.Content.ReadFromJsonAsync<OrderResponseDto>();
        
        // Wait for the order processed event
        await Task.WhenAny(orderProcessedEvent.Task, Task.Delay(TimeSpan.FromSeconds(10)));
        
        // Assert
        Assert.IsTrue(orderProcessedEvent.Task.IsCompletedSuccessfully);
        Assert.AreEqual(createdOrder.Id, orderProcessedEvent.Task.Result);
        
        // Check final order status
        var getResponse = await _client.GetAsync($"/api/orders/{createdOrder.Id}");
        var processedOrder = await getResponse.Content.ReadFromJsonAsync<OrderResponseDto>();
        Assert.AreEqual("Processed", processedOrder.Status);
    }
    
    [TestCleanup]
    public async Task Cleanup()
    {
        _client.Dispose();
        _factory.Dispose();
        await _rabbitMqContainer.StopAsync();
    }
}
```

### ❌ Don't: Use real message queues for all tests

Real message queue tests are valuable but should be limited due to their complexity, maintenance overhead, and performance impact. Keep them focused on configuration and end-to-end scenarios.

**Example:**
```csharp
// Anti-pattern - Don't do this!
[TestClass]
public class RealMessageQueueAntiPatternTests
{
    private static RabbitMQContainer _rabbitMqContainer;
    
    [ClassInitialize]
    public static async Task ClassInitialize(TestContext context)
    {
        // Starting a real RabbitMQ for ALL tests - expensive and slow
        _rabbitMqContainer = new RabbitMQContainer()
            .WithUsername("guest")
            .WithPassword("guest");
            
        await _rabbitMqContainer.StartAsync();
    }
    
    [TestMethod]
    public async Task Test1_UsingRealMessageQueue() 
    {
        // Test using real queue
    }
    
    [TestMethod]
    public async Task Test2_UsingRealMessageQueue() 
    {
        // Another test using real queue
    }
    
    // Dozens more tests all using real message queue
    // This approach is slow and can lead to flaky tests
    
    [ClassCleanup]
    public static async Task ClassCleanup()
    {
        await _rabbitMqContainer.StopAsync();
    }
}
```

### ❌ Don't: Use polling in tests

Polling for changes is inefficient and makes tests flaky. Use event-driven patterns with TaskCompletionSource to create clean, reliable tests.

**Example:**
```csharp
// Anti-pattern - Don't do this!
[TestMethod]
public async Task PollingAntiPattern()
{
    // Arrange
    var messageQueue = new FakeMessageQueue();
    var orderProcessor = new OrderProcessor(messageQueue);
    await orderProcessor.StartAsync(CancellationToken.None);
    
    // Act
    await messageQueue.PublishAsync("orders", new OrderMessage { Id = 1 });
    
    // Polling for results - BAD PRACTICE
    var orderProcessed = false;
    for (int i = 0; i < 10; i++)
    {
        // Check if order was processed
        var orderStatus = await _client.GetAsync($"/api/orders/1");
        var order = await orderStatus.Content.ReadFromJsonAsync<OrderDto>();
        
        if (order.Status == "Processed")
        {
            orderProcessed = true;
            break;
        }
        
        // Waiting with Thread.Sleep is even worse!
        await Task.Delay(500);
    }
    
    // Assert
    Assert.IsTrue(orderProcessed, "Order processing timed out");
    
    // Problems with this approach:
    // 1. Tests might be flaky if processing takes longer than expected
    // 2. Tests are slower because they always wait for the maximum timeout
    // 3. The test is more complex and harder to understand
}
```

## Mocking

### ✅ Do: Understand the different types of mocks

Mocking is a powerful technique, but the term "mock" is often used generically for very different testing approaches. Understanding these distinctions helps you choose the right mocking strategy for each situation. There are three main categories of mocks, each serving a different purpose:

#### 1. Isolation Mocks - Controlling External Boundaries

Isolation mocks prevent your test from accessing external systems and verify that your code interacts with those systems correctly. These mocks typically stub boundary functions that make external calls or intercept network requests.

**Purpose:**
- Prevent real calls to external services (databases, APIs, email services)
- Verify that your code is making correct external calls with proper parameters
- Simulate different external responses

This type of mocking is almost always beneficial since it isolates your component and focuses the test on your code, not external dependencies.

**Example:**
```csharp
[TestMethod]
public async Task ProcessOrder_SendsEmailWithOrderDetails()
{
    // Arrange
    var emailServiceMock = A.Fake<IEmailService>();
    A.CallTo(() => emailServiceMock.SendEmailAsync(A<string>._, A<string>._, A<string>._))
        .Returns(Task.FromResult(true));
    
    var orderService = new OrderService(emailServiceMock);
    var order = new Order { Id = 123, CustomerId = 456, Amount = 99.99m };
    
    // Act
    await orderService.ProcessOrderAsync(order);
    
    // Assert - Verify the isolation mock was called correctly
    A.CallTo(() => emailServiceMock.SendEmailAsync(
        A<string>.That.Contains("@"),
        A<string>.That.Contains("Order Confirmation"),
        A<string>.That.Contains("$99.99")
    )).MustHaveHappened();
}
```

#### 2. Simulation Mocks - Creating Specific Scenarios

Simulation mocks force specific scenarios that would be difficult or impossible to trigger through external inputs. These mocks modify the behavior of internal components to simulate error conditions, timing issues, or other edge cases.

**Purpose:**
- Simulate hard-to-trigger conditions like network timeouts or database failures
- Test error handling and edge cases
- Simulate time-related behavior

While these mocks couple your tests to implementation details, they're often necessary to thoroughly test error handling and corner cases.

**Example:**
```csharp
[TestMethod]
public async Task ProcessOrder_WhenDatabaseFails_ReturnsErrorAndLogsException()
{
    // Arrange
    var loggerMock = A.Fake<ILogger<OrderProcessor>>();
    var orderRepositoryMock = A.Fake<IOrderRepository>();
    
    // Simulation mock - forcing a database failure scenario
    A.CallTo(() => orderRepositoryMock.GetByIdAsync(A<int>._))
        .Throws(new DbException("Connection timeout"));
        
    var orderProcessor = new OrderProcessor(orderRepositoryMock, loggerMock);
    
    // Act
    var result = await orderProcessor.ProcessOrderAsync(123);
    
    // Assert
    Assert.IsFalse(result.Success);
    Assert.AreEqual("Database error", result.ErrorMessage);
    
    // Verify error was logged
    A.CallTo(() => loggerMock.Log(
        LogLevel.Error,
        A<EventId>._,
        A<It.IsAnyType>.That.Matches(o => o.ToString().Contains("Connection timeout")),
        A<Exception>._,
        A<Func<It.IsAnyType, Exception, string>>._
    )).MustHaveHappened();
}
```

#### 3. Implementation Mocks - Verifying Internal Behavior

Implementation mocks verify that specific internal methods were called or that a particular algorithm was followed. These mocks check *how* something works rather than its observable outputs.

**Purpose:**
- Verify internal method calls
- Assert that a particular implementation approach was followed

This type of mocking is generally **discouraged** because it tightly couples tests to implementation details, making them brittle during refactoring and potentially missing actual output issues.

**Example (ANTI-PATTERN):**
```csharp
[TestMethod]
public void ProcessOrder_FollowsExpectedWorkflow()
{
    // Arrange
    var orderService = new OrderService();
    var orderValidator = A.Fake<IOrderValidator>();
    var paymentProcessor = A.Fake<IPaymentProcessor>();
    
    // Replace internal dependencies using reflection (ANTI-PATTERN)
    typeof(OrderService)
        .GetField("_validator", BindingFlags.NonPublic | BindingFlags.Instance)
        .SetValue(orderService, orderValidator);
        
    typeof(OrderService)
        .GetField("_paymentProcessor", BindingFlags.NonPublic | BindingFlags.Instance)
        .SetValue(orderService, paymentProcessor);
    
    var order = new Order { Id = 123 };
    
    // Act
    orderService.ProcessOrder(order);
    
    // Assert - Testing IMPLEMENTATION details (ANTI-PATTERN)
    A.CallTo(() => orderValidator.Validate(order)).MustHaveHappened();
    A.CallTo(() => paymentProcessor.ProcessPayment(order)).MustHaveHappened();
    
    // This test is brittle - if the internal workflow changes but produces the same
    // correct result, this test will fail unnecessarily
}
```

### A Formula for Identifying Problematic Mocks

A mock is likely problematic if it meets both these criteria:
1. It affects code that is internal to your component (not a boundary)
2. It's used in the assert phase of your test to verify internal calls

These mocks make tests fragile during refactoring and can miss actual issues with the component's observable behavior.

### Choosing the Right Mock Type

- **Isolation mocks** (external boundaries) - Almost always beneficial
- **Simulation mocks** (specific scenarios) - Use judiciously where needed for edge cases
- **Implementation mocks** (internal behavior) - Avoid when possible; focus on outputs instead

By understanding these distinctions, you can write tests that provide maximum value with minimum maintenance burden, focusing on your component's behavior rather than its implementation details.

### ✅ Do: Keep mocks visible in test files

Define mocks within test files, not in external locations. Mocks that directly affect test outcomes should be in the test itself, while shared mocks should be in setup methods.

**Example:**
```csharp
[TestClass]
public class OrderServiceTests
{
    private IEmailService _emailServiceMock;
    private IPaymentService _paymentServiceMock;
    private IOrderRepository _orderRepositoryMock;
    private OrderService _orderService;
    
    [TestInitialize]
    public void Initialize()
    {
        // Common mocks defined in setup
        _emailServiceMock = A.Fake<IEmailService>();
        _paymentServiceMock = A.Fake<IPaymentService>();
        _orderRepositoryMock = A.Fake<IOrderRepository>();
        
        // Default behavior
        A.CallTo(() => _emailServiceMock.SendEmailAsync(A<string>._, A<string>._, A<string>._))
            .Returns(Task.FromResult(true));
            
        _orderService = new OrderService(
            _orderRepositoryMock, 
            _paymentServiceMock, 
            _emailServiceMock);
    }
    
    [TestMethod]
    public async Task CompleteOrder_WithValidOrder_SendsConfirmationEmail()
    {
        // Arrange
        var order = new Order { Id = 1, UserId = 2, Total = 99.99m };
        
        // Mock specific to this test defined in the test itself
        A.CallTo(() => _orderRepositoryMock.GetByIdAsync(1))
            .Returns(Task.FromResult(order));
            
        // Act
        await _orderService.CompleteOrderAsync(1);
        
        // Assert
        A.CallTo(() => _emailServiceMock.SendEmailAsync(
            A<string>.That.Contains("@"),
            A<string>.That.Contains("Confirmation"),
            A<string>.That.Contains("99.99")
        )).MustHaveHappened();
    }
    
    [TestMethod]
    public async Task CompleteOrder_WithNonExistentOrder_ThrowsException()
    {
        // Arrange
        // Mock specific to this test defined in the test itself
        A.CallTo(() => _orderRepositoryMock.GetByIdAsync(A<int>._))
            .Returns(Task.FromResult<Order>(null));
            
        // Act & Assert
        await Assert.ThrowsExceptionAsync<OrderNotFoundException>(
            async () => await _orderService.CompleteOrderAsync(999));
    }
}
```

### ❌ Don't: Hide mocks in external files

Mocks hidden in external files make tests harder to understand and debug. Keep mocks visible within the test class or test method.

**Example:**
```csharp
// Anti-pattern - Don't do this!
// In a separate file: TestMocks.cs
public static class TestMocks
{
    public static IEmailService CreateEmailServiceMock()
    {
        var mock = A.Fake<IEmailService>();
        A.CallTo(() => mock.SendEmailAsync(A<string>._, A<string>._, A<string>._))
            .Returns(Task.FromResult(true));
        return mock;
    }
    
    public static IPaymentService CreatePaymentServiceMock()
    {
        var mock = A.Fake<IPaymentService>();
        A.CallTo(() => mock.ProcessPaymentAsync(A<decimal>._, A<string>._))
            .Returns(Task.FromResult(new PaymentResult { Success = true }));
        return mock;
    }
    
    // Many more hidden mocks...
}

// In test file
[TestMethod]
public async Task TestWithHiddenMocks_AntiPattern()
{
    // Arrange
    var emailService = TestMocks.CreateEmailServiceMock();
    var paymentService = TestMocks.CreatePaymentServiceMock();
    
    var orderService = new OrderService(emailService, paymentService);
    
    // Act
    await orderService.ProcessOrderAsync(new Order { Id = 1 });
    
    // Assert
    A.CallTo(() => emailService.SendEmailAsync(A<string>._, A<string>._, A<string>._))
        .MustHaveHappened();
        
    // Problem: Hidden mock behavior makes it hard to understand this test
    // When the test fails, you need to look in another file to understand why
}
```

### ✅ Do: Be careful with partial mocks

When mocking a class or interface, be thoughtful about whether to mock all methods or just some. Partial mocks can be confusing and unpredictable.

**Example:**
```csharp
[TestClass]
public class PartialMockingTests
{
    [TestMethod]
    public void CompletelyMockExternalService_GoodPractice()
    {
        // Arrange
        // Complete mock of external dependency - clear and safe
        var paymentGatewayMock = A.Fake<IPaymentGateway>();
        
        // Configure only the methods we need
        A.CallTo(() => paymentGatewayMock.ProcessPayment(A<decimal>._, A<string>._))
            .Returns(new PaymentResult { Success = true, TransactionId = "tx-123" });
            
        // Block unexpected calls to ensure isolated test
        A.CallTo(paymentGatewayMock).Throws(new Exception("Unexpected call to payment gateway"));
        
        // Unblock the method we need
        A.CallTo(() => paymentGatewayMock.ProcessPayment(A<decimal>._, A<string>._))
            .DoesNothing();
            
        var orderService = new OrderService(paymentGatewayMock);
        
        // Act
        var result = orderService.ProcessOrder(new Order { Total = 99.99m });
        
        // Assert
        Assert.IsTrue(result.Success);
        Assert.AreEqual("tx-123", result.TransactionId);
    }
    
    [TestMethod]
    public void PartialMock_SimulateSpecificFailure_AcceptableUseCase()
    {
        // Arrange
        // Create a real class (not an interface)
        var databaseAccess = new DatabaseAccess("connection-string");
        
        // Create a partial mock to simulate a specific failure
        var databaseAccessMock = A.Fake<DatabaseAccess>(options => 
            options.CallsBaseMethods());
            
        // Mock only the specific method that should fail
        A.CallTo(() => databaseAccessMock.ExecuteQuery(A<string>._))
            .Throws(new DbException("Simulated database failure"));
            
        var orderRepository = new OrderRepository(databaseAccessMock);
        
        // Act & Assert
        var exception = Assert.ThrowsException<OrderDataException>(() => 
            orderRepository.GetAllOrders());
            
        Assert.IsTrue(exception.Message.Contains("database failure"));
    }
}
```

### ✅ Do: Clean up all mocks between tests

Reset mocks between tests to prevent test interference. This ensures each test starts with a clean state.

**Example:**
```csharp
[TestClass]
public class MockCleanupTests
{
    private IEmailService _emailServiceMock;
    
    [TestInitialize]
    public void Initialize()
    {
        // Create fresh mocks for each test
        Fake.ClearRecordedCalls();
        _emailServiceMock = A.Fake<IEmailService>();
    }
    
    [TestMethod]
    public async Task SendOrderConfirmation_CallsEmailService()
    {
        // Arrange
        var notificationService = new NotificationService(_emailServiceMock);
        
        // Act
        await notificationService.SendOrderConfirmationAsync("user@example.com", 1);
        
        // Assert
        A.CallTo(() => _emailServiceMock.SendEmailAsync(
            "user@example.com",
            A<string>.That.Contains("Confirmation"),
            A<string>._
        )).MustHaveHappened();
    }
    
    [TestMethod]
    public async Task SendPaymentReceipt_CallsEmailService()
    {
        // Arrange
        var notificationService = new NotificationService(_emailServiceMock);
        
        // Act
        await notificationService.SendPaymentReceiptAsync("user@example.com", 99.99m);
        
        // Assert
        A.CallTo(() => _emailServiceMock.SendEmailAsync(
            "user@example.com",
            A<string>.That.Contains("Receipt"),
            A<string>.That.Contains("99.99")
        )).MustHaveHappened();
    }
}
```

### ❌ Don't: Leave mocks uncleaned

Failing to clean up mocks can lead to test interference, where behavior in one test affects others.

**Example:**
```csharp
// Anti-pattern - Don't do this!
[TestClass]
public class MockCleanupAntiPatternTests
{
    // Static mocks shared between tests - BAD PRACTICE
    private static IEmailService _emailServiceMock = A.Fake<IEmailService>();
    private static IPaymentService _paymentServiceMock = A.Fake<IPaymentService>();
    
    [TestMethod]
    public async Task Test1_ConfiguresMock()
    {
        // Arrange
        A.CallTo(() => _emailServiceMock.SendEmailAsync(A<string>._, A<string>._, A<string>._))
            .Returns(Task.FromResult(true));
            
        var orderService = new OrderService(_emailServiceMock, _paymentServiceMock);
        
        // Act & Assert
        // ...
    }
    
    [TestMethod]
    public async Task Test2_DependsOnPreviousMockConfiguration()
    {
        // Arrange
        // No mock setup here - relying on previous test's setup
        
        var orderService = new OrderService(_emailServiceMock, _paymentServiceMock);
        
        // Act
        await orderService.ProcessOrderAsync(new Order { Id = 1 });
        
        // Assert
        // This might pass or fail depending on whether Test1 ran first
        A.CallTo(() => _emailServiceMock.SendEmailAsync(A<string>._, A<string>._, A<string>._))
            .MustHaveHappened();
    }
}
```

### ✅ Do: Choose the right mocking mechanism

C# offers various mocking approaches, each with different tradeoffs:

1. **Interface-based mocking** - The standard approach, simple and predictable
2. **Type-based mocking** - Uses dynamic proxies to mock non-virtual methods, can be brittle
3. **Accessor-based mocking** - Uses special accessors or shims, more invasive but powerful

**Example:**
```csharp
[TestClass]
public class MockingMechanismsExamples
{
    [TestMethod]
    public void InterfaceBasedMocking_PreferredApproach()
    {
        // Interface-based mocking - Simple, reliable, recommended
        var loggerMock = A.Fake<ILogger>();
        
        // Configure the mock
        A.CallTo(() => loggerMock.Log(A<string>._)).DoesNothing();
        
        // Use the mock
        var service = new OrderService(loggerMock);
        service.ProcessOrder(new Order());
        
        // Verify
        A.CallTo(() => loggerMock.Log(A<string>.That.Contains("Order processed"))).MustHaveHappened();
    }
    
    [TestMethod]
    public void TypeBasedMocking_UseWithCaution()
    {
        // Type-based mocking - More complex, can be brittle
        
        // Create a fake of a concrete class
        var repository = A.Fake<OrderRepository>(options => 
            options.CallsBaseMethods()); // Calls real methods by default
        
        // Override only specific methods
        A.CallTo(() => repository.GetById(42)).Returns(new Order { Id = 42 });
        
        // Use the mock
        var order = repository.GetById(42);
        
        // Verify
        Assert.AreEqual(42, order.Id);
    }
    
    [TestMethod]
    public void AccessorBasedMocking_AdvancedScenarios()
    {
        // This is an advanced technique using Microsoft Fakes or similar
        // It allows mocking of static methods, sealed classes, etc.
        
        // This is a simplified example of how it might look
        // with Microsoft Fakes (not FakeItEasy)
        
        /*
        using (ShimsContext.Create())
        {
            // Replace static DateTime.Now with a fixed time
            System.Fakes.ShimDateTime.NowGet = 
                () => new DateTime(2023, 1, 1, 12, 0, 0);
            
            var service = new OrderService();
            var result = service.CreateOrder(new Order());
            
            Assert.AreEqual(new DateTime(2023, 1, 1, 12, 0, 0), result.CreatedAt);
        }
        */
    }
}
```

### ✅ Do: Ensure mocks are type-safe

Use type-safe mocking to catch signature mismatches at compile time. This prevents tests from passing when the code they're testing has changed incompatibly.

**Example:**
```csharp
[TestMethod]
public void TypeSafeMocking_WithFakeItEasy()
{
    // FakeItEasy provides compile-time type checking
    var calculatorMock = A.Fake<ICalculator>();
    
    // This will cause a compiler error if the signature changes
    A.CallTo(() => calculatorMock.Add(A<int>._, A<int>._))
        .Returns(10);
        
    // If someone changes the method to return double instead of int,
    // the test would fail at compile time
    
    var result = calculatorMock.Add(3, 7);
    Assert.AreEqual(10, result);
}

[TestMethod]
public void EnsureTypeMatching_WhenSettingUpMocks()
{
    var userServiceMock = A.Fake<IUserService>();
    
    // Type-safe setup
    A.CallTo(() => userServiceMock.GetUserByIdAsync(A<int>._))
        .Returns(Task.FromResult(new User { Id = 1, Name = "Test User" }));
        
    // This would cause a compiler error if GetUserByIdAsync were changed
    // to return something other than Task<User>
    
    var user = userServiceMock.GetUserByIdAsync(1).Result;
    Assert.AreEqual("Test User", user.Name);
}
```