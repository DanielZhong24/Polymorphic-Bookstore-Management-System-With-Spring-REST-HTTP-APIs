1. How many concrete classes extend the abstract Material class? Identify each one and explain their specific purposes.

In the Lab 4 bookstore system, five concrete subclasses extend the abstract Material class: PrintedBook, EBook, AudioBook, VideoMaterial, and Magazine. Each represents a specific real world item in the bookstore. PrintedBook models traditional physical books with attributes such as pageCount, publisher, and a flag for hardcover status. EBook represents digital publications and introduces fields like fileFormat, fileSize, and drmProtected. AudioBook captures spoken word media, adding details such as duration, narrator, and MediaQuality. VideoMaterial models multimedia products with attributes for resolution, encoding, and duration, while Magazine represents recurring periodicals characterized by issueNumber, publicationDate, and frequency. These subclasses extend the common structure of Material, inheriting attributes like id, title, price, and year, while overriding or extending methods to demonstrate specialization and polymorphism within the domain layer.


2. What are the main challenges when making a system thread safe? How do you balance performance with correctness? Explain the trade offs between different synchronization mechanisms (ReentrantReadWriteLock vs ConcurrentHashMap vs synchronized blocks)

The main challenge when making a system thread safe is trying to share or manage objects across threads without running into issues such as race conditions or deadlock. Multiple threads within a single process will share the same space, and with that being said, multiple threads can read and write to the same object or data, which will cause unexpected outputs such as old or lost values, which are race conditions. Deadlocks can happen when multiple threads try to retrieve locks on shared resources in an unexpected order, possibly leading to each thread waiting for one another due to them holding a resource that the other needs. To balance performance with correctness, when tasked with different problems, choosing the correct data structure can be important to achieving good performance as the wrong data structure can make the implementation both more complex and perform worse. For correctness, use OOP and SOLID principles to create well designed, readable, and reusable code, and as for performance, an example is using caching such as LRU to store the results of operations that are costly so they are not repeated.

As for ReentrantReadWriteLock, it allows for better read performance since multiple reads can happen at the same time. The disadvantage of it is that it is more complex than the typical ReentrantLock.

For ConcurrentHashmap, the advantages of it is that it allows multiple threads to read and write data simultaneously without having to lock the entire map, also supports atomic operations such as replace and remove. Some downsides is that there cannot be null objects inserted, it uses more memory because of its locking mechanism, and its complexity to implement in code

For synchronized blocks, the advantages of it is that only one thread can execute a specific block of code at a time so race conditions can be prevented and consistency across multiple threads can be maintained. A tradeoff is also because of its ability to only allow one thread at a time, because if multiple threads want to read a shared resource but not update it, it is safe to do so but the synchronized block is limited because of it.

3. Which design patterns from this lab would for a real world ecommerce system? Justify your choices with specific use cases. How would you adapt these patterns for a microservices architecture?


The factory method pattern allows for the creation of different product types such as physical products, digital products based on the input. Another pattern would be the Hexagonal architecture to allow the core to communicate with external services like an API. For example, payment processing using the Stripe API. In addition, the observer pattern could be used in the case of notification updates, where users could subscribe to a product for things like stock availability, or shipping updates, where it would notify everyone subscribed to the product if it has come back in stock, or if their order status has changed (ie processed to shipped). The final pattern would be the Decorator Pattern where you could add extra features or characteristics to a product before buying. For example, on checkout, you could add features such as gift wrapping, faster shipping, or extended warranty to the product.
As for adapting to microservice architectures, the factory method pattern can decide which service to call based on the parameter it is given on runtime. In the context of an ecommerce system, this can be done in the payment processing section where the user can select their payment method. If they select Credit/Debit card, the Stripe microservice can be called to handle that method, or if they choose Paypal, the Paypal microservice will be called instead.


For hexagonal architecture, each microservice would have its own port that it needs to implement the functionality for. For example, the payment processing will have a port/interface that will define the contract for how they will interact, and then the adapter connecting to it will implement the actual logic to do so.

As for the observer pattern, when the payment goes through and the order is processed, there can be other microservices that now subscribe to that event of the order going through, such as notifications to email, and the microservice that is responsible for the stock availability subscribes to that and is notified that there is one less stock in the warehouse.

Finally, the decorator pattern can be used to add additional internal features to the application without changing the main logic, like a logger to record events and information when the code is run, and also rate limiting, keeping the original logic in place, but adding a feature on that of that logic to shield against a large number of requests within a certain amount of time.

4. When would you choose a simple ArrayList over a more complex Trie for search operations? What factors influence this decision? How do you measure and validate performance improvements?


   A simple ArrayList would be better than a Trie for search operations when it comes to dealing with a smaller dataset, and also dealing with whole strings by using the .contains() method on the ArrayList. Also, if searches aren’t frequently done, the implementation of an Arraylist would be much better to use because of its straightforward maintainability and code readability.

As for a Trie, it would be better when you are dealing with larger data sets, but also handling scenarios like autocomplete and finding words with a common prefix as they are connected by nodes.

When it comes to measuring and validating performances for the ArrayList and Trie for search operations, the time complexity of an ArrayList if it is unsorted is O(N) as you may have to search the entire list. For a Trie, the time complexity of searching is O(L) where L is the length of the word we are string we are searching for. The space complexity of a Trie is O(N*M) where N is the number of nodes it has, and M is the number of references a single node may have. As for an ArrayList, it is O(N) where N is the number of elements in the list.

To measure and validate this, we can use the Java microbenchmark harness (JMH) to test these data structures on larger datasets, so thousands of words as the complexities of the data structure really matter in its worst case scenario, when inputs become larger. We can create several test benchmarks for Trie and ArrayLists, and take the average of their results. We will know that it is valid if we see that an Arraylist will have better results than a Trie on smaller datasets, and then a Trie performing better for prefix searching or larger data sets.

5. How do mutation testing and static analysis complement traditional unit testing? What additional confidence do they provide? Explain the difference between code coverage and mutation score, and why both metrics are important


   Static analysis means analyzing code without executing it to find any potential bugs or issues, which are typically done by external automated tools rather than a human. Mutation testing is where the program is modified in small ways like changing the operator or using the wrong variable name and then rerunning the tests. If the tests fail, the mutant was killed meaning that your test caught the bug, but if they pass, your test cases missed an issue.

Therefore, static analysis will prevent bad code and any potential issues or bugs before they are even executed. Unit tests will ensure that your code functions properly as intended, and mutation testing will help against any errors that may happen in your code, such as missing variables.

Static analysis will give confidence for catching security issues before releasing to production, and enhance proper coding styles and guidelines. Mutation testing will give confidence in catching real errors if another person has introduced them.

Code coverage only shows how many lines of your code are actually executed by your unit tests, whereas mutation score shows the percentage of errors found in your code making small changes, meaning how well your test cases properly detect the mutants. This means that just having 80% code coverage or higher only means that amount is executed by tests, but doesn’t explicitly show whether or not your tests are good at detecting different bugs in your code.

Code coverage is important because you want to ensure that your code can run properly with as little as possible to untested code, and a high mutation score is important since it will contain assertions so it will check for errors if there are any bugs found in the code.

6. How does the hexagonal architecture separate domain logic from infrastructure? Provide specific examples of ports and adapters.

The hexagonal architecture, also known as the Ports and Adapters pattern, separates the core business logic (domain layer) from the external infrastructure such as databases, web controllers, and user interfaces. In the  bookstore system, the domain layer contains the core entities and rules that define how materials behave — for example, the abstract Material class and its subclasses form the central domain model. These classes operate independently of any external frameworks or data sources, ensuring that the business rules remain pure and reusable.

The ports are defined as interfaces that specify how external systems can interact with the domain. Examples include MaterialRepository, MaterialStore, and MaterialEventPublisher. These interfaces describe operations such as saving materials, retrieving them, or publishing events, but they do not dictate how those actions are implemented. This abstraction allows the system to remain adaptable to different environments.

The adapters are the concrete implementations that connect these interfaces to real technologies. For instance, JsonMaterialRepository acts as an outbound adapter that implements MaterialRepository and handles data persistence using JSON files, while ConcurrentMaterialStore serves as an inbound adapter, providing thread safe access to the material data for clients or APIs. Similarly, MaterialController is a web adapter that exposes REST endpoints for interacting with the system, translating HTTP requests into service calls that operate through defined ports.

By isolating the domain from the infrastructure, the system achieves high testability, scalability, and maintainability. The core business logic can be tested independently of databases or frameworks, and infrastructure components (like repositories or controllers) can be swapped or extended without changing the domain code. This clear separation exemplifies clean architecture principles and aligns with enterprise level software design best practices.

7. Error Handling: How do the different patterns(Observer, chain of Responsibility) handle error conditions? What are the trade offs between fail fast and fail safe error handling strategies?

The Observer Pattern handles error conditions by catching exceptions and logging them so that other observers can be executed. The Chain of Responsibility pattern passes the error condition through a chain of handlers where each handler either processes the request if it meets the right conditions or passes onto the next handler in the chain. For fail fast error handling, it makes debugging much easier since the error is shown when the code fails; however, it can lead to the system stopping frequently while fail safe can still continue running the code while handling the errors, making errors difficult to locate.

8. Testing Strategy: How does property based testing differ from traditional unit testing? When is each approach most valuable? How do you design effective property based tests for complex business logic?

Property based testing specifies requirements that the code must meet, regardless of the user's input and then creates varying inputs to test the requirements to find examples where the requirements are not met. Is best used for validating lone functions and is effective for ensuring correctness of core logic. While traditional unit testing focuses on the code receiving the right input and displaying the right output. Best for complex data structures and algorithms as well as business logic where there is a vast amount of possible inputs. Property based tests are designed effectively when based around system invariants where there are small units instead of large complex functions, and when the outcome for property based tests are specified.







11. Pattern Integration: How do the design patterns in this lab complement each other? What challenges arise when integrating multiple patterns, and how does the hexagonal architecture help manage this complexity? Provide specific examples from the implementation.

In this lab I noticed how the different design patterns really work together to make the system both flexible and organized. The creational patterns like Factory and Builder help create complex objects which then fit into structural patterns such as Composite to manage hierarchical relationships. For example the MaterialDirector uses the Builder to create MaterialBundle objects that follow the Composite pattern.

I also found it interesting how Decorator and Composite complement each other because decorators can add features like gift wrapping to individual materials or bundles showing how flexible the structure can be. Meanwhile behavioral patterns like Observer and Chain of Responsibility connect system behavior to real time updates. When a discount request is processed observers automatically log and analyze the outcome.

The biggest challenge I saw was that with so many patterns working together the design could easily get complicated. That is where the Hexagonal Architecture really helped. By keeping the core business logic separate from external interfaces like the web controller or database adapters it made it easier to connect everything without breaking other parts. In short the architecture acted like a framework that allowed all the patterns to interact smoothly while keeping the system maintainable and less error prone.


12. Data Structure Choices: Why was a Trie chosen for prefix search over other data structures? What are the memory vs. time complexity trade offs? How would you optimize the Trie implementation for a production system?

I learned that the Trie data structure was chosen for prefix searching because it is very fast for autocomplete style queries. Compared to scanning every title which would take O(N × L) the Trie lets us search by prefix in just O(P + R) time where P is the prefix length and R is the number of matches. That is a huge performance improvement.

However this efficiency comes with a trade off in memory usage. Since each node in the Trie stores references to its children and sometimes a list of materials it uses more space than something like a hash map. In our case every node even keeps a list of materials that match the prefix which speeds things up but increases memory use.

If I were optimizing this for a real production system I would probably store only IDs or pointers instead of full objects to reduce duplication. I might also store lists only at the end nodes or use node compression to make the Trie smaller. Overall the Trie is worth it when fast prefix searching is critical but memory optimization becomes important as the dataset grows.


13. Caching Strategy: Explain the LRU cache implementation and its benefits. How would you handle cache invalidation in a distributed system? What are the trade offs between different cache eviction policies?

The system uses an LRU Least Recently Used cache for search results which I thought was a clever way to boost performance. The idea is simple it keeps the most recently used items in memory and removes the ones that have not been used for a while. Using a HashMap and a Deque it can perform both get and put operations in O(1) time which makes repeated searches much faster.

The benefit is that users get faster results for common queries but it also raises the question of cache invalidation what happens when data changes. In our implementation invalidation happens locally when materials are added or removed but in a distributed system keeping multiple caches in sync would be harder. That is a big challenge because outdated data could stay in one cache longer than it should.

Different cache eviction policies also have trade offs. LRU works well when access frequency matters but TTL Time To Live policies are better for ensuring data freshness. LRU can use more memory and processing to track order while TTL might remove items that are still popular. Choosing between them depends on whether performance or freshness is more important for the system.


14. Event Driven Architecture: How does the Observer pattern enable event driven architecture? What are the benefits and challenges of loose coupling through events? How would you implement event sourcing with this architecture?

The Observer pattern in this project really helped me understand how event driven systems work. Basically when something happens like adding a new material or changing a price the event publisher notifies all the observers that care about that event. This lets parts of the system like logging or analytics update automatically without changing the main business logic.

The main advantage I saw was loose coupling because you can add or remove observers easily without rewriting core code. For example adding a new AnalyticsObserver or InventoryObserver does not affect how materials are added. But this also makes the system harder to trace and debug since multiple things can happen at once. Another challenge is ensuring that if one observer fails it does not stop others from running which is why our implementation uses thread safe structures like CopyOnWriteArrayList.

If we wanted to take this further we could add event sourcing where every change in the system is saved as a sequence of immutable events. That would make it easier to rebuild system state or audit changes but it would also increase complexity.


15. Builder Pattern Complexity: The MaterialDirector demonstrates complex object construction. When is the Builder pattern worth the additional complexity? How do you balance flexibility with simplicity in object creation?

At first I thought the Builder pattern made the code more complicated but I started to see why it is worth it for complex object creation. It is really useful when an object has many optional settings like a MaterialBundle or an EBook with different configurations. Instead of using long constructors the Builder lets you build the object step by step and you can even validate properties before the final build call.

The MaterialDirector adds another layer of organization by managing the sequence of building steps for common bundles like buildTextbookBundle. That makes it easier for other developers to create standard configurations without worrying about all the individual details.

To me the Builder pattern strikes a good balance between flexibility and simplicity. The Builder itself gives flexibility to create anything while the Director simplifies the process for common cases. It is extra code but it pays off when object creation is complex or needs strict control and readability.
