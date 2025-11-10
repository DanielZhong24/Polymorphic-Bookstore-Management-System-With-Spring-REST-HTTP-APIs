7. Error Handling: How do the different patterns(Observer, chain of Responsibility) handle error conditions? What are the trade-offs between fail-fast and fail-safe error handling strategies?

The Observer Pattern handles error conditions by catching exceptions and logging them so that other observers can be executed. The Chain of Responsibility pattern passes the error condition through a chain of handlers where each handler either processes the request if it meets the right conditions or passes onto the next handler in the chain. For fail-fast error handling, it makes debugging much easier since the error is shown when the code fails; however, it can lead to the system stopping frequently while fail-safe can still continue running the code while handling the errors, making errors difficult to locate.

8. Testing Strategy: How does property-based testing differ from traditional unit testing? When is each approach most valuable? How do you design effective property-based tests for complex business logic?

Property-based testing specifies requirements that the code must meet, regardless of the user's input and then creates varying inputs to test the requirements to find examples where the requirements are not met. Is best used for validating lone functions and is effective for ensuring correctness of core logic. While traditional unit testing focuses on the code receiving the right input and displaying the right output. Best for complex data structures and algorithms as well as business logic where there is a vast amount of possible inputs. Property-based tests are designed effectively when based around system invariants where there are small units instead of large complex functions, and when the outcome for property-based tests are specified.

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
