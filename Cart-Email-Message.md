### Task Overview: Implementing Cart Email Notifications in Salesforce Commerce Cloud

In this task, you will develop a feature in Salesforce Commerce Cloud (SFCC) that automatically sends an email notification to customers whenever they add a product to their shopping cart. This email will include details about the newly added product and will be sent only if the customer is logged in. This functionality aims to enhance customer engagement by keeping them informed about their cart activities.

**Objective:**  
By completing this task, you will set up an automated email notification system that triggers when a customer adds a product to their cart. You will create an email template, integrate the email-sending functionality into the appropriate controller, and ensure that emails are dispatched only to logged-in customers.

### Steps to Complete the Task

#### Step 1: Create an Email Template

Begin by designing an email template that will be used to notify customers. This template should include placeholders for dynamic information about the product and the customer.

1. **Design the Email Template:**
   - Create a new ISML template file, for instance, `cartEmailNotification.isml`.
   - Include placeholders in the template for dynamic content such as the customer’s name and the product details.

   ```html
   <html>
   <body>
       <p>Dear ${customer.firstName},</p>
       <p>You've added the following product to your cart:</p>
       <p><strong>Product Name:</strong> ${product.name}</p>
       <p><strong>Price:</strong> ${product.price}</p>
       <p>Thank you for shopping with us!</p>
   </body>
   </html>
   ```

2. **Save the Template:**
   - Place this template in your custom cartridge, such as `app_custom_cart/cartridge/templates/default/email`.

#### Step 2: Learn How to Send Emails Programmatically

Understand how to send emails programmatically within SFCC using built-in modules.

1. **Explore Email Sending in SFCC:**
   - Refer to SFCC documentation or code examples to learn about sending emails. SFCC provides the `dw/net/Email` module for this purpose.
   - Example code for sending an email:

   ```javascript
   var Email = require('dw/net/Email');
   var email = new Email({
       from: 'no-reply@yourdomain.com',
       to: 'customer@example.com',
       subject: 'Product Added to Your Cart',
       htmlBody: 'Email content here' // This can be dynamically generated
   });
   email.send();
   ```

#### Step 3: Integrate Email Sending into the Controller

Determine the appropriate controller that handles adding products to the cart and extend it to include email-sending functionality.

1. **Identify the Relevant Controller:**
   - The `Cart-Add` controller manages the actions related to adding items to the cart. You will need to extend this controller.

2. **Extend the Controller:**
   - Create or modify a controller in your custom cartridge, extending the existing `Cart-Add` controller.
   - Append logic to the `Add` action to include email-sending functionality.

   ```javascript
   var server = require('server');
   var CartAdd = module.superModule;
   var Email = require('dw/net/Email');

   server.extend(CartAdd);

   server.append('Add', function (req, res, next) {
       var BasketMgr = require('dw/order/BasketMgr');
       var currentBasket = BasketMgr.getCurrentBasket();
       var customer = req.currentCustomer.raw;
       
       if (customer && customer.authenticated) {
           var product = req.form.productID; // Fetch product details as needed

           // Compose the email content
           var emailContent = {
               from: 'no-reply@yourdomain.com',
               to: customer.email,
               subject: 'Product Added to Your Cart',
               htmlBody: renderTemplate('email/cartEmailNotification', {
                   customer: customer,
                   product: product
               })
           };

           // Send the email
           var email = new Email(emailContent);
           email.send();
       }

       next();
   });

   module.exports = server.exports();
   ```

   In this code:
   - Verify the customer’s authentication status using `customer.authenticated`.
   - Use `renderTemplate` to dynamically generate the email body with the provided template.

#### Step 4: Ensure Emails Are Sent Only to Logged-In Customers

Implement logic to ensure that emails are only sent to customers who are logged in.

1. **Check Customer Authentication:**
   - Confirm that the customer is logged in before sending the email. Use the `customer.authenticated` property to verify this.

   ```javascript
   if (customer && customer.authenticated) {
       // Proceed with sending the email
   }
   ```

### What You'll Learn

By completing this task, you will:

- **Design Email Templates:** Create and format email templates with dynamic content placeholders.
- **Send Emails Programmatically:** Understand how to use SFCC’s `dw/net/Email` module to send emails.
- **Extend Controllers:** Gain experience in extending controllers to add custom functionality.
- **Handle Customer Authentication:** Implement logic to ensure that emails are only sent to authenticated customers.

This task will enhance your skills in integrating email notifications within Salesforce Commerce Cloud and help you create a more engaging shopping experience for customers.
