# User Story 04: Manage Cart Quantities

## Story ID
US-004

## Title
Update Product Quantities in Cart

## As a
Customer

## I want to
Modify the quantity of products in my shopping cart

## So that
I can adjust my order without having to remove and re-add items

## Acceptance Criteria

### AC1: Increase Quantity
- **Given** I have a product in my cart
- **When** I click the "+" button next to the quantity
- **Then** the quantity should increase by 1
- **And** the item total and cart total should update accordingly

### AC2: Decrease Quantity
- **Given** I have a product in my cart with quantity > 1
- **When** I click the "X" button next to the quantity
- **Then** the quantity should decrease by 1
- **And** the item total and cart total should update accordingly

### AC3: Manual Quantity Entry
- **Given** I have a product in my cart
- **When** I manually enter a quantity in the spinbutton
- **Then** the quantity should update to the entered value
- **And** the pricing should recalculate automatically

### AC4: Price Recalculation
- **Given** I change the quantity of any item
- **When** the quantity is updated
- **Then** the following should recalculate:
  - Item total (unit price × quantity)
  - Subtotal (sum of all item totals)
  - Final total

### AC5: Real-time Updates
- **Given** I modify quantities
- **When** I make any change
- **Then** all prices should update immediately without page reload

## Priority
High

## Estimated Story Points
3

## Dependencies
- US-003 (View Shopping Cart)
- Cart state management system

## Technical Notes
- Quantity controls are implemented as spinbuttons
- "+" button increases quantity
- "X" button decreases quantity (also serves as decrement)
- All calculations must maintain 2 decimal places for currency
- Updates should be immediate (no "Update Cart" button needed)

## Edge Cases to Consider
- Minimum quantity should be 1 (or remove item if decremented below 1)
- Maximum quantity limits (if any stock constraints exist)
- Invalid manual entry (non-numeric values)
- Zero or negative quantities
