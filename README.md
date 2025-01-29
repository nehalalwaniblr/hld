# HLD
High level design

### [1. System Design: The Typeahead Suggestion System](#The-Typeahead-Suggestion-System)



### The-Typeahead-Suggestion-System
Typeahead suggestion, also referred to as the autocomplete system, is a front-end functionality that provides real-time search query suggestions as users type. It enables users to search for a known and frequently searched query. This feature comes into play when a user types a query in the search box. The typeahead system provides a list of top suggestions to complete a query , and . Frequently searched queries always appear at the top of the suggestion list. The typeahead system doesn’t search faster. However, it helps the user form a sentence more quickly. The underlying  determines the relevance and ranking of suggested terms.

**Requirements**: In this lesson, we focus on the functional and non-functional requirements for designing a typeahead suggestion system. We also discuss resource estimations for the smooth operation of the system.

**High-level design**: In this lesson, we discuss the high-level design of our version of the typeahead suggestion system. We also discuss some essential APIs used in the design.

**Data structure for storing prefixes:** In this lesson, we cover the trie data structure (an efficient tree data structure) that’s used to store search prefixes. We also discuss how it can be further optimized to reduce the tree traversal time.

**Detailed design:** In this lesson, we explain the two main components, the suggestions service and the assembler, which make up the detailed design of the typeahead suggestion system.

**Evaluation**: This lesson evaluates the proposed design of the typeahead suggestion system based on the non-functional requirements of the system. It also presents some client-side optimization and personalization that could significantly affect the system’s design.

### Functional requirements
The system should suggest top N(let’s say top ten) frequent and relevant terms to the user based on the text a user types in the search box.

### Non-functional requirements
Low latency: The system should show all the suggested queries in real time after a user types. The latency shouldn’t exceed 200 ms. A study suggests that the average time between two keystrokes is 160 milliseconds. So, our time-budget of suggestions should be greater than 160 ms to give a real-time response. This is because if a user is typing fast, they already know what to search and might not need suggestions. At the same time, our system response should be greater than 160 ms. However, it should not be too high because in that case, a suggestion might be stale and less useful.

Fault tolerance: The system should be reliable enough to provide suggestions despite the failure of one or more of its components.

Scalability: The system should support the ever-increasing number of users over time.

### Resource estimation#
 Google receives more than 3.5 billion searches every day.
 
**Storage estimation**
Assuming that out of the 3.5 billion queries per day, two billion queries are unique and need to be stored. Let’s also assume that each query consists of 15 characters on average, and each character takes 2 Bytes of storage. According to this formulation, we would require the following:

<img width="746" alt="Screenshot 2025-01-29 at 10 26 08 AM" src="https://github.com/user-attachments/assets/cee1b0c1-e202-40a5-8102-e0c9a4a719f8" />

**Bandwidth estimation#**
<img width="886" alt="Screenshot 2025-01-29 at 10 52 49 AM" src="https://github.com/user-attachments/assets/ab91b2be-b881-4e9c-9b90-dd612c4eb2ba" />

**Number of servers estimation#**
Our system will receive 52.5 billion requests per day. Considering our assumption of using daily active users as a proxy for the number of requests per second for peak load, we get 52.5 billion requests per second (considering each character as a request).

Recall that a typical server can serve 64,000 requests per second (RPS). So, the required servers can be estimated using the following formula:
<img width="1013" alt="Screenshot 2025-01-29 at 10 58 04 AM" src="https://github.com/user-attachments/assets/b196b0c1-ef71-4f37-ab10-ca9befc3864e" />


<img width="1084" alt="Screenshot 2025-01-29 at 10 57 46 AM" src="https://github.com/user-attachments/assets/8fa00d0d-4f0e-4b15-8cb0-bf40a15e6d62" />

### Building blocks 

Databases are required to keep the data related to the queries’ prefixes.
Load balancers are required to disseminate incoming queries among a number of active servers.
Caches are used to keep the top N suggestions for fast retrieval.

