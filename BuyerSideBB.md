# Buyer Side Building Blocks

## Objective
Build domain-agnostic, reusable components for beckn applications, that  can be configured and used across various domains. This requires a flexible and modular architecture, that can adapt to various domains with minimal code changes.

Each building block can be dynamically enabled or disabled using feature toggles.

The frontend (UI layer), and the backend-for-frontend (client layer) should be assembled using building blocks. For example: a building block in the BFF is an API to process a search request, and a building block in the Frontend is a react component with search criteria, and display responses coming back from the API request.

## User-Interface Layer

### Technical Building Blocks

#### User Interface Components
Design reusable React components that cover common UI elements (e.g., search criteria forms, result displays, configurable forms, paginated grids) and can be easily customized for different domains.

#### State Management
Utilize state management libraries like Redux or React Context to manage the state of the application. Store domain-specific configuration and user data.

#### Client API Helper
Develop a flexible API client that can be configured dynamically based on the selected domain. It should handle making requests to the BFF and processing responses.

#### Routing
Implement a routing mechanism to navigate between different views and sections of the application. This should allow for easy integration of new domain-specific pages.

#### Error Handling and Feedback
Create user-friendly error messages and feedback mechanisms to inform users about issues with their requests.

#### Internationalization and Localization
Add support for multiple languages and regions, since the platform will be used globally.

#### Styling and Theming
Allow for easy theming and customization of the user interface to match the branding of different domains.

#### User Analytics and Tracking
Integrate analytics tools to gather insights into user behavior and improve the user experience.

### Functional Building Blocks

#### Search Component:
Provides a user interface for entering search criteria. Displays search results and allows users to refine their searches.

#### Ordering Component
Creates a shopping cart interface. Allows users to add, remove, and update items in the cart. Displays the order summary and facilitates the checkout process.

#### Cancellation Component
Allows users to initiate order cancellations. Provides confirmation and status updates for canceled orders.

#### Payment Component:
Collects payment information and processes transactions. Displays payment status and handles payment-related errors.

#### User Profile Component
Offers user registration and login forms. Provides profile management features for users to update their information.

#### Review and Rating Component
Allows users to leave reviews and ratings for products or services. Displays average ratings and individual reviews on product pages.

### Sample Code for React Search Component
Here's a simplified React search component, that takes a onSearch callback prop that will be called when the user clicks the "Search" button. The component captures the user's input and sends it to the parent component when the "Search" button is clicked.
```
import React, { useState } from 'react';

function SearchComponent({ onSearch }) {
  const [searchTerm, setSearchTerm] = useState('');

  const handleSearch = () => {
    onSearch(searchTerm); // Pass the search term to the parent component
  };

  return (
    <div>
      <input
        type="text"
        placeholder="Enter your search term"
        value={searchTerm}
        onChange={(e) => setSearchTerm(e.target.value)}
      />
      <button onClick={handleSearch}>Search</button>
    </div>
  );
}

export default SearchComponent;
```


## Client Layer

### Technical Building Blocks

#### Authentication & Authorization
This block handles user authentication and authorization to ensure that requests have the necessary entitlements to access the features.

#### Network Handler
This block is responsible for routing API requests to the appropriate API, and signing the requests. This block may also include caching, rate limiting etc to improve performance and prevent abuse of the platform.

#### Domain Configuration
This block allows you to configure the specific domain (e.g., retail, mobility, health) that the buyer app will interact with. It should include settings for API endpoints, data models, and business logic specific to each domain.

#### Request Processing
This block processes incoming requests from the frontend, validates them, and prepares them for sending to the network. It may include request transformation and validation.

#### Response Handling
On receiving callbacks from the network or BPPs, this block processes and transforms the data into a common format that the frontend can easily consume.

#### Error Handling and Logging
Implement a robust error handling mechanism that logs errors and sends appropriate error responses to the frontend.

### Functional Blocks

#### Discovery Module
Handles search requests from the frontend. Communicates with protocol APIs to perform searches. Aggregates and formats search results for the frontend. Supports filtering, sorting, and pagination.

#### Ordering Module
Manages the ordering process, including adding items to the cart, updating quantities, and submitting orders. Validates order requests and ensures they meet domain-specific requirements. Communicates with sellers' APIs to place orders. Provides order confirmation and tracking information.

### Payment Module
Manages payment processing and integration with various payment gateways. Ensures secure handling of payment information. Provides payment status updates to the frontend.

#### Fulfillment Module
Allows buyers to cancel orders or specific items within an order. Communicates with sellers' APIs to initiate order cancellations. Handles refund and cancellation confirmation.

#### User Profile Module
Handles user registration, login, and profile management. Allows users to update personal information, addresses, and payment methods. Implements authentication and authorization.

#### Post-fulfillment Module
Enables buyers to leave reviews and ratings for products or services. Displays aggregated ratings and reviews on product listings. Manages the moderation and publication of reviews.

### Sample Code for Node.js Search Handler

Here's a simplified Node.js example, where Express.js framework to create an HTTP server. It provides a /api/search POST endpoint that receives a JSON payload containing the search term. The server then searches for products that match the search term and responds with the results in JSON format.

```
const express = require('express');
const app = express();
const bodyParser = require('body-parser');

// Middleware to parse JSON requests
app.use(bodyParser.json());

// Sample data (in-memory)
const products = [
  { id: 1, name: 'Product 1' },
  { id: 2, name: 'Product 2' },
  { id: 3, name: 'Product 3' },
  // ... other products
];

// Search endpoint
app.post('/api/search', (req, res) => {
  const searchTerm = req.body.searchTerm.toLowerCase();
  
  // Simulate searching for products based on the search term
  const searchResults = products.filter((product) =>
    product.name.toLowerCase().includes(searchTerm)
  );

  res.json(searchResults);
});

const port = process.env.PORT || 3000;
app.listen(port, () => {
  console.log(`Server is running on port ${port}`);
});
```


## Feature Flag Service
This application has an UI that allows control of feature flags without changing the code by using a feature flag service. Administrators can toggle feature flags on or off dynamically without modifying the application's code. Toggle these flags on or off, change their targeting rules and validate results on the application.

Feature flags are stored in the configuration management system. The frontend and backend applications fetch these configurations at runtime. The state of feature flags can be changed without re-deploying the application.

## Sample Usage of Feature Flag in Node.js
```
// Load configuration from a configuration file or environment variables
const featureConfig = {
  searchEnabled: process.env.SEARCH_ENABLED === 'true',
  // Add more feature flags as needed
};

// Middleware to parse JSON requests
app.use(bodyParser.json());

// Search endpoint
app.post('/api/search', (req, res) => {
  if (!featureConfig.searchEnabled) {
    return res.status(404).json({ message: 'Search feature is disabled' });
  }

  // Rest of your search logic
});

// Other endpoints and application logic
```

## Sample Usage of Feature Flag in React.js

```
import React from 'react';

// Your feature flag manager or configuration
const featureFlags = {
  searchEnabled: true, // Example: Set to true to enable the search feature
  // Add more feature flags as needed
};

function SearchComponent() {
  // Check the state of the feature flag
  const isSearchEnabled = featureFlags.searchEnabled;

  return (
    <div>
      {isSearchEnabled ? (
        // Render the SearchComponent when the feature flag is enabled
        <SearchComponentEnabled />
      ) : (
        // Render a message or an alternative component when the feature flag is disabled
        <p>Search feature is currently disabled.</p>
      )}
    </div>
  );
}

function SearchComponentEnabled() {
  // The component for the enabled state
  return (
    <div>
      <h1>Search Component</h1>
      {/* Render your search form and logic here */}
    </div>
  );
}

export default SearchComponent;
```


## Build and deployment
