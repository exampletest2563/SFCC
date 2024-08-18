### Chapter: Creating a Product Listing Component by Category or ID in Salesforce Commerce Cloud

#### Objective

In this chapter, you'll learn how to create a Product Listing Component using Salesforce Commerce Cloud’s Page Designer. You will configure the component to list products by category or by product IDs, with the category-based listing taking precedence over product IDs.

---

### Step 1: Identify the Controller and Template Needed

**1. Determine the Controller Handling Product Listings**

   The first step is to identify the controller responsible for managing product listings. In Salesforce Commerce Cloud, product listings are often managed by a custom controller in your project.

   - **Locate the Controller File:**
     - Navigate to your custom cartridge's `controllers` directory (e.g., `app_custom_cartridge/cartridge/controllers`).
     - Open or create a file named `ProductListing.js` if it does not already exist. This file will handle the logic for your Product Listing Component.

   - **Identify the Associated Template:**
     - Determine the template that will render the product listings. This template is usually located in the `templates` directory (e.g., `app_custom_cartridge/cartridge/templates/default`).
     - For this exercise, you might have templates such as `productListingByCategory.isml` and `productListingByIDs.isml`.

---

### Step 2: Extend or Create the Controller for Product Listing

**1. Create or Extend the Controller**

   To manage the logic for displaying products based on category or ID, you need to create or extend the Product Listing Controller.

   - **Create a Custom Cartridge:**
     - If not already created, set up a custom cartridge (e.g., `app_custom_product_listing`). Ensure this cartridge is included in your cartridge path configuration.

   - **Extend or Create the Product Listing Controller:**
     - In your custom cartridge, create or modify `ProductListing.js` within the `controllers` directory.

     **Example Code:**

     ```javascript
     var server = require('server');
     var CategoryMgr = require('dw/catalog/CategoryMgr');
     var ProductMgr = require('dw/catalog/ProductMgr');

     server.get('Show', function (req, res, next) {
         var categoryID = req.querystring.categoryID;
         var productIDs = req.querystring.productIDs ? req.querystring.productIDs.split(',') : [];
         var category;
         var products = [];

         if (categoryID) {
             category = CategoryMgr.getCategory(categoryID);
             if (category) {
                 products = category.products.toArray();
             }
         } else if (productIDs.length > 0) {
             products = productIDs.map(function(id) {
                 return ProductMgr.getProduct(id);
             }).filter(function(product) {
                 return product;
             });
         }

         res.render('productListing', { products: products });
         next();
     });

     module.exports = server.exports();
     ```

**2. Create the Associated Templates**

   Create or modify the `productListing.isml` template to handle the display of products.

   - **Template for Category Listing:**

     **File:** `productListingByCategory.isml`

     ```html
     <isif condition="${products.length > 0}">
         <ul>
             <isforeach items="${products}" var="product">
                 <li>
                     <a href="${URLUtils.url('Product-Show', 'pid', product.ID)}">
                         <img src="${product.imageURL}" alt="${product.name}" />
                         <h3>${product.name}</h3>
                         <p>${product.price}</p>
                     </a>
                 </li>
             </isforeach>
         </ul>
     </isif>
     <iselse>
         <p>No products found in this category.</p>
     </iselse>
     ```

   - **Template for ID Listing:**

     **File:** `productListingByIDs.isml`

     ```html
     <isif condition="${products.length > 0}">
         <ul>
             <isforeach items="${products}" var="product">
                 <li>
                     <a href="${URLUtils.url('Product-Show', 'pid', product.ID)}">
                         <img src="${product.imageURL}" alt="${product.name}" />
                         <h3>${product.name}</h3>
                         <p>${product.price}</p>
                     </a>
                 </li>
             </isforeach>
         </ul>
     </isif>
     <iselse>
         <p>No products found with the provided IDs.</p>
     </iselse>
     ```

---

### Step 3: Implement Custom Logic in the Controller

**1. Add Logic for Priority Listing**

   Implement the logic in the controller to prioritize category-based listings over product IDs.

   - **Update `ProductListing.js` with Prioritization Logic:**

     ```javascript
     server.get('Show', function (req, res, next) {
         var categoryID = req.querystring.categoryID;
         var productIDs = req.querystring.productIDs ? req.querystring.productIDs.split(',') : [];
         var category;
         var products = [];

         if (categoryID) {
             category = CategoryMgr.getCategory(categoryID);
             if (category && category.products.length > 0) {
                 products = category.products.toArray();
             }
         }

         if (products.length === 0 && productIDs.length > 0) {
             products = productIDs.map(function(id) {
                 return ProductMgr.getProduct(id);
             }).filter(function(product) {
                 return product;
             });
         }

         res.render('productListing', { products: products });
         next();
     });
     ```

---

### Step 4: Create Site Preferences for Configurable Values

**1. Define Custom Site Preferences (if needed)**

   If you require configurable thresholds or additional settings, create custom site preferences.

   - **Create a Custom Site Preference:**
     - In Business Manager, navigate to **Administration > Sites > Manage Sites**.
     - Select your site and go to **Custom Preferences**.
     - Add preferences with appropriate IDs, types, and default values as needed.

---

### Step 5: Update the Template to Display Products

**1. Modify the Product Listing Template**

   Ensure the `productListing.isml` template handles the display of product data passed from the controller.

   - **Update the Template to Render Product Data:**

     ```html
     <isif condition="${products && products.length > 0}">
         <ul>
             <isforeach items="${products}" var="product">
                 <li>
                     <a href="${URLUtils.url('Product-Show', 'pid', product.ID)}">
                         <img src="${product.imageURL}" alt="${product.name}" />
                         <h3>${product.name}</h3>
                         <p>${product.price}</p>
                     </a>
                 </li>
             </isforeach>
         </ul>
     </isif>
     <iselse>
         <p>No products available.</p>
     </iselse>
