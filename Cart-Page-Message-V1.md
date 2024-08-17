### Chapter: Displaying a Custom Message in the Cart When the Total Exceeds $200

#### Step 1: Determine the Controller and Template Needed

The first step is to identify the controller and template that handle the cart functionality. In SFCC, the shopping cart page is typically managed by the `Cart-Show` controller.

1. **Locate the Cart-Show Controller:**
   - Navigate to the `app_storefront_base/cartridge/controllers` directory.
   - Open the `Cart.js` file. The `Cart-Show` action in this file is responsible for rendering the cart page.

2. **Identify the Associated Template:**
   - The `Cart-Show` controller typically renders the `cart/cart` template, which is located in the `app_storefront_base/cartridge/templates/default` directory.
   - This template is where the cart items, total, and any messages will be displayed.

#### Step 2: Extend the Cart-Show Controller

To add custom logic for checking the cart total and displaying a message, you need to extend the `Cart-Show` controller in your custom cartridge.

1. **Create a Custom Cartridge:**
   - If not already created, set up a custom cartridge (e.g., `app_custom_cart`). Ensure it is included in the cartridge path in your project configuration.

2. **Extend the Cart-Show Controller:**
   - In your custom cartridge, create a `Cart.js` file within the `cartridge/controllers` directory.
   - Extend the `Cart-Show` controller by using the `module.superModule` pattern.

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

#### Step 3: Determine the Model Responsible for Cart Total

To access and manipulate the cart total, you need to work with the appropriate model in SFCC.

1. **Use the BasketMgr Model:**
   - The `BasketMgr` class in SFCC is responsible for managing the current shopping basket. 
   - You can retrieve the cart total using the `totalGrossPrice` property of the `currentBasket` object.

   ```javascript
   var BasketMgr = require('dw/order/BasketMgr');
   var currentBasket = BasketMgr.getCurrentBasket();
   var cartTotal = currentBasket.totalGrossPrice.value;
   ```

#### Step 4: Create a Site Preference for the Cart Total Threshold

To make the threshold value configurable, you should create a custom site preference. This allows you to easily update the threshold without changing the code.

1. **Create a Custom Site Preference:**
   - In the Business Manager, navigate to **Administration** > **Sites** > **Manage Sites**.
   - Select your site and go to **Custom Preferences**.
   - Create a new custom preference with the following settings:
     - **ID:** `cartTotalThreshold`
     - **Type:** `Number`
     - **Default Value:** `200`

#### Step 5: Implement the Logic in the Extended Controller

With the site preference in place, you can now add the logic to your extended controller to check if the cart total exceeds the threshold and display a message.

1. **Check the Cart Total Against the Custom Preference:**
   - In the `Show` action of the extended `Cart-Show` controller, compare the `cartTotal` with the `customThreshold` retrieved from the site preferences.

   ```javascript
   var customThreshold = Site.current.getCustomPreferenceValue('cartTotalThreshold');
   if (cartTotal >= customThreshold) {
       res.setViewData({
           cartMessage: 'Your cart total exceeds $' + customThreshold + '.'
       });
   }
   ```

2. **Send the Message to the Template:**
   - If the cart total meets or exceeds the threshold, send a warning message to the template using `res.setViewData()`.

   ```javascript
   res.setViewData({
       cartMessage: 'Your cart total exceeds $' + customThreshold + '.'
   });
   ```

#### Step 6: Modify the Template to Display the Message

Finally, update the `cart/cart.isml` template to display the message when it is passed from the controller.

1. **Update the `cart/cart.isml` Template:**
   - Open the `cart/cart.isml` template in your custom cartridge.
   - Add logic to check if the `cartMessage` variable exists and display it on the page.

   ```html
   <isif condition="${pdict.cartMessage}">
       <div class="cart-warning">
           ${pdict.cartMessage}
       </div>
   </isif>
   ```

### Conclusion

By following these steps, you have successfully added a custom warning message to the cart page that alerts users when their cart total exceeds a specified threshold. You extended the `Cart-Show` controller, utilized the `BasketMgr` model to access the cart total, created a site preference for the threshold value, and updated the template to display the message. This approach allows for flexible and maintainable enhancements to your Salesforce Commerce Cloud storefront.
