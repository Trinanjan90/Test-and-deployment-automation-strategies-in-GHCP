# User Story 03: View Shopping Cart

## Story ID
US-003

## Title
View Shopping Cart Contents

## As a
Customer

## I want to
View all items in my shopping cart with pricing details

## So that
I can review my selections before proceeding to checkout

## Acceptance Criteria

### AC1: Navigate to Cart
- **Given** I am on any page of the website
- **When** I click on the "Cart (X)" link in the navigation
- **Then** I should be directed to the cart page at /cart

### AC2: Empty Cart Display
- **Given** I have no items in my cart
- **When** I view the cart page
- **Then** I should see a message "Your shopping cart is empty"
- **And** I should see a "Continue Shopping" button

### AC3: Cart with Items Display
- **Given** I have items in my cart
- **When** I view the cart page
- **Then** I should see a table/list with the following columns:
  - Product Name (clickable link)
  - Quantity (with controls)
  - Price (unit price)
  - Total (quantity × price)

### AC4: Cart Summary Display
- **Given** I have items in my cart
- **When** I view the cart page
- **Then** I should see a summary section displaying:
  - Subtotal (sum of all items)
  - Shipping cost (shows "Free")
  - Total (final amount)

### AC5: Cart Actions
- **Given** I am viewing my cart
- **When** I look at the action buttons
- **Then** I should see:
  - "Continue Shopping" button
  - "Proceed to Checkout" button

## Priority
High

## Estimated Story Points
5

## Dependencies
- US-001 (Browse Products)
- US-002 (Add Product to Cart)
- Cart data persistence system

## Technical Notes
- Cart page URL: /cart
- Product names in cart are clickable links to product detail pages
- Cart displays real-time pricing calculations
- Shipping is currently free for all orders

## UI Components
- Cart table header row
- Product row for each item
- Summary section with pricing breakdown
- Action buttons for navigation and checkout
