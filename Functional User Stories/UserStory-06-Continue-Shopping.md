# User Story 06: Continue Shopping

## Story ID
US-006

## Title
Return to Products from Cart

## As a
Customer

## I want to
Return to the product catalog from my shopping cart

## So that
I can add more items to my order without losing my current cart contents

## Acceptance Criteria

### AC1: Continue Shopping from Empty Cart
- **Given** I am on the cart page with an empty cart
- **When** I click the "Continue Shopping" button
- **Then** I should be redirected to the products page (homepage)
- **And** I should see the full product catalog

### AC2: Continue Shopping from Active Cart
- **Given** I am on the cart page with items in my cart
- **When** I click the "Continue Shopping" button
- **Then** I should be redirected to the products page
- **And** my cart contents should be preserved
- **And** the cart counter should still show the correct item count

### AC3: Button Availability
- **Given** I am on the cart page
- **When** I view the page (regardless of cart contents)
- **Then** I should always see a "Continue Shopping" button

### AC4: Cart State Persistence
- **Given** I have items in my cart
- **When** I click "Continue Shopping" and return to products
- **Then** my cart should retain all previously added items
- **And** I should be able to add more items
- **And** the cart count should accumulate correctly

## Priority
Medium

## Estimated Story Points
2

## Dependencies
- US-001 (Browse Products)
- US-003 (View Shopping Cart)
- Cart state management system

## Technical Notes
- Button appears in both empty and filled cart states
- Navigation does not clear cart contents
- Cart state persists across page navigation
- Simple URL navigation to homepage/products page

## User Flow
1. Customer adds items to cart
2. Customer navigates to cart page
3. Customer clicks "Continue Shopping"
4. Customer returns to products page
5. Customer can add more items
6. Cart accumulates all items
