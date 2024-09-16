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


## State Management in Microfrontends

State management in microfrontends is challenging due to the distributed nature of the architecture and can undermine the benefits of independent development. By coupling each fronten to each other, it becomes harder to develop each frontend independently, and it might defeat the purpose of microfrontends. Shared state becomes a coupling point that can lead to conflicts and inconsistencies where changes to the shared state in one microfrontend can affect others.

However, in some scenarios, shared state management is necessary for cross-cutting concerns like authentication, user preferences, or global settings. In such cases, the following strategies can be used:

1. **Local Storage**: Storing state in the browser's local storage can provide a simple way to share data across microfrontends without the need for external libraries or complex setups. However, it has some limitations like data size restrictions and security concerns.
2. **Shared API Utility**: A shared API utility microfrontend can be implemented to cache fetch/XHR requests and their corresponding responses. This reduces network requests and improves responsiveness by allowing individual microfrontends to query the shared API, which determines whether data needs to be refetched based on its freshness. This approach minimizes network overhead, increases efficiency, and reduces errors across the microfrontends.
3. **Create exported functions**: In the Single SPA framework, you can share state between different frontend applications by exporting functions from one application and importing them into another. This is done by creating a shared module in one application, which exports functions to get or set the shared state. Other applications can then import these functions to access or update the shared state. This shared state could be a simple object or a more complex data structure managed by libraries like Redux or MobX or React's Context. It's important to ensure the shared module is loaded before any dependent applications.
   - Example with simple functions:
   ```javascript
     // Shared module in Application A
     let sharedState = { someValue: 100};
   
     export function getSharedState() {
         return sharedState;
     }
   
     export function setSharedState(value) {
         sharedState = value;
     }
    ```
    ```javascript
      // Application B
      import { getSharedState, setSharedState } from 'application-a/shared';
    
      const sharedState = getSharedState();
      console.log(sharedState.someValue); // 100
    
      setSharedState({ someValue: 200 });
      console.log(getSharedState().someValue); // 200
     ```

4. **Event Bus**: An event bus can be used to broadcast state changes across microfrontends. Each microfrontend can subscribe to specific events and update its state accordingly. This approach decouples the microfrontends and allows them to communicate without direct dependencies. However, it will probalby lead to event spaghetti, where it's hard to follow the flow of the data.
5. **State Management Libraries**: In micro frontends, several libraries are available for state management, including Zustand and redux-micro-frontend. Here's an example of using Zustand to manage shared state between two apps:

**App 1:**

```javascript
// Define your Zustand store in App 1
import create from 'zustand';

const useStore = create((set) => ({
  count: 0,
  increment: () => set(state => ({ count: state.count + 1 })),
}));

// Export your store and selectors
export { useStore };
```

**App 2:**

```javascript
// Define your Zustand store in App 2
import create from 'zustand';

const useStore = create((set) => ({
  message: 'Hello',
  setMessage: (newMessage) => set({ message: newMessage }),
}));

// Export your store and selectors
export { useStore };
```

**Shared State Management Solution:**

```javascript
// In a shared file, import the stores from both apps and create a combined store
import { combine } from 'zustand';
import { useStore as useStoreApp1 } from 'App1/store';
import { useStore as useStoreApp2 } from 'App2/store';

const useCombinedStore = combine(
  useStoreApp1,
  useStoreApp2,
  (app1Store, app2Store) => ({
    count: app1Store.count,
    increment: app1Store.increment,
    message: app2Store.message,
    setMessage: app2Store.setMessage,
  })
);

// Export the combined store
export { useCombinedStore };
```

In this setup, both App 1 and App 2 have independent Zustand stores, but the state is synchronized between them through a shared file using the `combine` function. The combined store allows access to and modification of the shared state from both applications.

## Assigment

Divide [this example app](https://users.metropolia.fi/~ilkkamtk/juutube/) into microfrontends and implement a shared state management solution using one of the strategies discussed above.

- [Repository](https://github.com/ilkkamtk/juutube-react/) of the example app.
- [Live demo](https://users.metropolia.fi/~ilkkamtk/juutube/)

### Starter repositories
- https://github.com/ilkkamtk/host-starter
- https://github.com/ilkkamtk/juutube-front-and-sidebar-starter
- https://github.com/ilkkamtk/store-starter
- https://github.com/ilkkamtk/palvelinohjelmointi-types

Clone the starter repositories to the same parent directory and follow the instructions in the TODOs to complete the assignment.

1. Separeate state management from the host to the store.
   - Create a Context store to `store-starter` microfrontend.
2. Move frontpage and sidebar to a separate microfrontend.
   - Create a new microfrontend to `juutube-front-and-sidebar-starter`.
3. Import the store and the frontpage and sidebar microfrontends to the host in `host-starter`.

## Group Assignment

Continue to separate the different parts of the app into microfrontends like the top bar, video player, profile, upload etc. Each group member should work on a separate part of the app and integrate it into the host. 
