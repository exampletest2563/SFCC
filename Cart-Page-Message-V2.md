### Task Overview: Adding a Cart Total Warning Message in Salesforce Commerce Cloud

In this task, you will enhance the shopping cart functionality in Salesforce Commerce Cloud (SFCC) by displaying a custom warning message when the total value of the cart exceeds a specified amount, which will default to $200. This feature is designed to inform customers when their purchase surpasses a certain threshold, such as for eligibility for shipping discounts or special promotions.

**Objective:**  
By the end of this task, you will have modified the cart page to display a message like "Your cart total exceeds $200" whenever the cart total reaches or exceeds $200. You will achieve this by extending existing SFCC functionality, implementing a custom site preference for the threshold, and updating the cart template to display the message.

### Steps to Complete the Task

#### Step 1: Identify the Relevant Controller and Template

First, identify the controller and template responsible for rendering the shopping cart page. In SFCC, the cart page is managed by:

- **Controller:** `Cart-Show` controller located in the `Cart.js` file. This controller is responsible for displaying the shopping cart.
- **Template:** `cart/cart.isml` template, which is used to render the cart's content, including items, totals, and messages.

You will extend the `Cart-Show` controller to include custom logic that checks the cart total and conditionally displays the warning message.

#### Step 2: Extend the `Cart-Show` Controller

To add the custom functionality, you need to extend the `Cart-Show` controller within your custom cartridge.

1. **Create a Custom Cartridge:**
   - If you haven't already, set up a custom cartridge (e.g., `app_custom_cart`). Ensure that this cartridge is included in the cartridge path in your project configuration.

2. **Extend the Controller:**
   - In your custom cartridge, create a `Cart.js` file under the `cartridge/controllers` directory.
   - Extend the existing `Cart-Show` controller using the `module.superModule` pattern.

   ```javascript
   var server = require('server');
   var CartShow = module.superModule;

   server.extend(CartShow);

   server.append('Show', function (req, res, next) {
       var BasketMgr = require('dw/order/BasketMgr');
       var Site = require('dw/system/Site');
       var currentBasket = BasketMgr.getCurrentBasket();
       var cartTotal = currentBasket.totalGrossPrice.value;

       var customThreshold = Site.current.getCustomPreferenceValue('cartTotalThreshold');
       if (cartTotal >= customThreshold) {
           res.setViewData({
               cartMessage: 'Your cart total exceeds $' + customThreshold + '.'
           });
       }

       next();
   });

   module.exports = server.exports();
   ```

#### Step 3: Access the Cart Total Using the BasketMgr Model

To retrieve and compare the cart total, you’ll work with the `BasketMgr` model in SFCC, which manages the current shopping basket.

- Use `BasketMgr.getCurrentBasket()` to access the current basket.
- Retrieve the total value of the basket using the `totalGrossPrice` property.

   ```javascript
   var BasketMgr = require('dw/order/BasketMgr');
   var currentBasket = BasketMgr.getCurrentBasket();
   var cartTotal = currentBasket.totalGrossPrice.value;
   ```

This value will be compared against the custom threshold to determine if the warning message should be displayed.

#### Step 4: Create and Use a Custom Site Preference

To make the threshold value configurable, create a custom site preference. This allows you to easily adjust the threshold (defaulting to $200) from the Business Manager without modifying your code.

1. **Create the Site Preference:**
   - In Business Manager, navigate to **Administration** > **Sites** > **Manage Sites**.
   - Select your site and go to the **Custom Preferences** tab.
   - Create a new custom preference:
     - **ID:** `cartTotalThreshold`
     - **Type:** `Number`
     - **Default Value:** `200`

2. **Use the Custom Preference in Your Controller:**
   - In the extended `Cart-Show` controller, retrieve the `cartTotalThreshold` value using `Site.current.getCustomPreferenceValue('cartTotalThreshold')`.
   - Compare the cart total to this threshold in your logic:

   ```javascript
   var customThreshold = Site.current.getCustomPreferenceValue('cartTotalThreshold');
   if (cartTotal >= customThreshold) {
       res.setViewData({
           cartMessage: 'Your cart total exceeds $' + customThreshold + '.'
       });
   }
   ```

This approach ensures that the threshold value can be adjusted without any code changes, making your implementation flexible and easily maintainable.

#### Step 5: Update the Cart Template to Display the Message

Finally, modify the `cart/cart.isml` template to display the warning message if it is set.

1. **Update the Template:**
   - Open the `cart/cart.isml` template.
   - Add a conditional check to display the message if the `cartMessage` variable exists.

   ```html
   <isif condition="${pdict.cartMessage}">
       <div class="cart-warning">
           ${pdict.cartMessage}
       </div>
   </isif>
   ```

This ensures that the message only appears when the cart total exceeds the specified threshold.

### What You'll Learn

By completing this task, you will:

- **Understand Controller Extensions:** Learn how to extend base controllers to add custom functionality in a maintainable way.
- **Work with SFCC Models:** Gain experience using the `BasketMgr` model to access and manipulate cart data.
- **Implement Site Preferences:** Learn how to create and use custom site preferences to make your logic configurable and adaptable.
- **Customize Templates:** Modify ISML templates to dynamically display content based on business logic.

This task provides practical experience with extending and customizing Salesforce Commerce Cloud, preparing you for more complex development challenges.
