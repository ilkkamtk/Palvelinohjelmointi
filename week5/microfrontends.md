# Microfrontends

[Jaska-sedän video](https://www.youtube.com/watch?v=t-nchkL9yIg&t=176s)

**Microfrontend Architecture** breaks a frontend into independent, manageable pieces that are developed and maintained by separate teams. This approach enhances scalability and reduces complexity but requires effective integration strategies.

## Integration patterns for Microfrontends

1. **Micro Apps: Segmentation by Routes**
    - **Description**: Each route loads a fully independent micro app.
    - **Benefits**: Teams have full autonomy; fault isolation improves reliability.
    - **Challenges**: Longer load times; requires efficient caching and clear route boundaries.

2. **Microfrontends with iFrames**
    - **Description**: Each microfrontend is embedded in its own iframe.
    - **Benefits**: Strong fault isolation; independence in development and deployment.
    - **Challenges**: Complex inter-iframe communication; performance overhead; responsiveness issues.

3. **Microfrontends with App Shell**
    - **Description**: A central shell application integrates various microfrontends.
    - **Benefits**: Facilitates code reuse and handles cross-cutting concerns like authentication.
    - **Challenges**: Requires compatibility with the shell; potential bottlenecks; complex loading strategies.

4. **Microfrontends with Bit Components**
    - **Description**: Uses Bit for composable, reusable components.
    - **Benefits**: Supports independent development and high code reuse; flexible integration.
    - **Challenges**: Initial setup and learning curve; managing peer dependencies effectively.

5. **Microfrontends with NPM**
    - **Description**: Microfrontends are packaged as NPM modules.
    - **Benefits**: Modular development; easy rollback with versioning.
    - **Challenges**: Requires compatible frameworks; complex dependency management; performance impact.

6. **Module Federation**
    - **Description**: Dynamically loads microfrontends at runtime using Webpack 5.
    - **Benefits**: Flexibility in module updates; supports fine-grained modularity.
    - **Challenges**: Complex setup; dependency management; performance considerations.

7. **Backend for Microfrontend (BFMF)**
    - **Description**: Each microfrontend has its own backend service.
    - **Benefits**: Clear ownership; optimized performance.
    - **Challenges**: Managing consistency and resources; integration overhead.

8. **Event Sourcing-Based Microfrontends**
    - **Description**: Uses event sourcing for state changes and synchronization.
    - **Benefits**: Real-time updates; consistent state; improved resilience.
    - **Challenges**: Complex infrastructure; performance overhead; consistency management.

9. **Hypermedia Pattern**
    - **Description**: Uses hypermedia controls for navigation and interaction.
    - **Benefits**: High flexibility; reduced coupling; dynamic structure.
    - **Challenges**: Complex backend requirements; performance impacts; standardization issues.
