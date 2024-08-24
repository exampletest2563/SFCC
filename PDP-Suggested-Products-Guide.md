To create a chapter in your Salesforce Commerce Cloud course that guides students through the task of displaying a section on the Product Detail Page (PDP) that lists four suggested products based on the current product's brand, follow this structured approach:

### Chapter: Displaying Suggested Products on the PDP

#### 1. **Introduction**
   - **Objective:** By the end of this chapter, students will be able to implement a feature on the PDP that displays four suggested products based on the brand of the current product.
   - **Prerequisites:** Basic knowledge of Salesforce Commerce Cloud (SFCC), familiarity with the Product Detail Page structure, and understanding of ISML templates and controllers in SFCC.

#### 2. **Understanding the Problem**
   - **Scenario:** The task is to enhance the PDP by adding a section that dynamically lists four suggested products. These suggestions should be filtered based on the brand of the currently viewed product.
   - **Expected Outcome:** A new section on the PDP will display up to four products from the same brand as the current product.

#### 3. **Step-by-Step Implementation Guide**

   ##### Step 1: **Retrieve the Brand of the Current Product**
   - **Task:** Fetch the brand attribute from the current product displayed on the PDP.
   - **Guide:**
     - Access the current product using the `ProductMgr.getProduct()` method.
     - Extract the brand attribute using the `product.custom.brand` (or the appropriate attribute based on the site configuration).
   - **Hint:** Check if the brand attribute is available in the custom attributes or core attributes of the product.

   ##### Step 2: **Fetch Suggested Products**
   - **Task:** Use the brand value to retrieve a list of products that match the brand.
   - **Guide:**
     - Use `ProductSearchModel` to search for products that match the brand.
     - Limit the search results to four products.
     - Ensure the products retrieved are active and available in the catalog.
   - **Sample Code:**
     ```javascript
     var ProductSearchModel = require('dw/catalog/ProductSearchModel');
     var searchModel = new ProductSearchModel();
     searchModel.setSearchPhrase(currentProduct.brand);
     searchModel.search();
     var suggestedProducts = searchModel.getProductSearchHits().asList().slice(0, 4);
     ```
   - **Hint:** Be mindful of performance. Avoid large searches and ensure that only necessary fields are fetched.

   ##### Step 3: **Display the Suggested Products in the ISML Template**
   - **Task:** Update the PDP ISML template to include a section that displays the suggested products.
   - **Guide:**
     - Create a new ISML template (e.g., `suggestedProducts.isml`) to handle the display of the products.
     - Loop through the list of suggested products and render each product’s name, image, and price.
     - Include this template in the main PDP ISML template using the `isinclude` directive.
   - **Sample ISML:**
     ```isml
     <isloop items="${suggestedProducts}" var="product">
         <div class="suggested-product">
             <img src="${product.getImage('small').URL}" alt="${product.name}" />
             <a href="${product.getURL()}">${product.name}</a>
             <span>${product.price}</span>
         </div>
     </isloop>
     ```
   - **Hint:** Style the suggested products section to match the overall design of the PDP.

   ##### Step 4: **Handle Edge Cases**
   - **Task:** Ensure the feature works correctly even when there are fewer than four suggested products or none at all.
   - **Guide:**
     - Add a condition to check if there are fewer than four suggested products and handle the display accordingly.
     - Consider adding a fallback message or alternative suggestions if no matching products are found.
   - **Sample ISML Condition:**
     ```isml
     <isif condition="${suggestedProducts.length > 0}">
         <div class="suggested-products-section">
             <h3>Suggested Products</h3>
             <isinclude template="suggestedProducts.isml" />
         </div>
     <iselse>
         <div>No suggested products available.</div>
     </isif>
     ```

#### 4. **Testing the Implementation**
   - **Task:** Ensure the suggested products section works across different products and brands.
   - **Guide:**
     - Test with products that have well-defined brands and those that do not.
     - Verify that the correct products are suggested based on the current product's brand.
     - Check the display on different devices to ensure responsiveness.

#### 5. **Best Practices and Optimization**
   - **Performance Considerations:**
     - Limit the number of product attributes fetched to improve performance.
     - Use caching mechanisms where appropriate to reduce database load.
   - **User Experience:**
     - Ensure the suggested products are relevant and enhance the shopping experience.
     - Consider adding analytics to track the effectiveness of the suggestions.

#### 6. **Summary**
   - **Recap:** This chapter guided you through the process of adding a brand-based suggested products section to the PDP. You learned how to retrieve product data, filter by brand, and render this dynamically on the page.

#### 7. **Exercise for Students**
   - **Task:** Extend the suggested products feature to include products from related categories if fewer than four products are found for the brand. Provide the steps they should follow and encourage them to consider performance and user experience.

By following this structure, you provide a clear and comprehensive guide for your students, helping them to understand the process and apply their knowledge effectively.


In Salesforce Commerce Cloud (SFCC), the typical eCommerce page types include the following:

### 1. **Home Page**
   - The main landing page of the eCommerce site, which usually features promotions, categories, and highlighted products.

### 2. **Category Page**
   - Displays a list of products within a specific category. It usually includes filtering and sorting options.

### 3. **Product Listing Page (PLP)**
   - Similar to a category page, this page lists products based on search results, specific criteria, or promotions.

### 4. **Product Detail Page (PDP)**
   - Displays detailed information about a single product, including descriptions, images, pricing, and options for adding to the cart.

### 5. **Search Results Page**
   - Shows products that match the user's search query. It may include filters, sorting, and pagination.

### 6. **Cart Page**
   - Displays items that the customer has added to their shopping cart. It allows for quantity adjustments, removal of items, and proceeding to checkout.

### 7. **Checkout Pages**
   - **Shipping Information Page:** Where customers provide shipping details.
   - **Billing Information Page:** Where customers enter payment information.
   - **Order Review Page:** The final step before placing an order, where customers review all order details.

### 8. **Order Confirmation Page**
   - Displays order details and confirms that the order has been successfully placed.

### 9. **Account Pages**
   - **Login/Register Page:** Where customers can log in or create an account.
   - **Account Dashboard:** A summary page showing account information, order history, and preferences.
   - **Order History Page:** Displays a list of past orders with details.
   - **Address Book Page:** Where customers can manage saved shipping addresses.
   - **Wishlist Page:** Displays products that the customer has saved for later.

### 10. **Customer Service Pages**
   - **Contact Us Page:** Provides a form or information for customer support inquiries.
   - **FAQ Page:** Answers common questions about the store, products, shipping, etc.
   - **Returns/Exchanges Page:** Information on how to return or exchange products.

### 11. **Content Pages**
   - **About Us Page:** Provides information about the company.
   - **Terms and Conditions Page:** Details the legal terms of using the site.
   - **Privacy Policy Page:** Explains how customer data is handled.

### 12. **Promotional Pages**
   - **Landing Pages:** Specific pages created for marketing campaigns, often featuring targeted promotions.
   - **Sale Page:** Showcases products that are currently on sale or part of a promotion.

### 13. **Store Locator Page**
   - Allows customers to find physical store locations, often integrated with maps.

### 14. **Blog/News Page**
   - Contains articles, news updates, and other content related to the brand or industry.

### 15. **Error Pages**
   - **404 Page:** Displayed when a page is not found.
   - **500 Page:** Displayed when there is a server error.

These are the common page types in Salesforce Commerce Cloud, and each serves a specific purpose in the customer journey.


### Chapter: Product Searches

#### 1. **Introduction**
   - **Objective:** By the end of this chapter, students will have a solid understanding of how to implement and utilize product searches within Salesforce Commerce Cloud (SFCC). They will learn how to retrieve products based on various criteria, such as brand, category, or specific attributes, and how to integrate these searches into different pages of an eCommerce site.
   - **Prerequisites:** Basic understanding of SFCC controllers, models, and ISML templates. Familiarity with the structure of eCommerce sites and product data.

#### 2. **Understanding Product Searches in SFCC**
   - **Overview:** Product searches are a core functionality in any eCommerce platform, allowing customers to find products that meet their needs. In SFCC, product searches can be customized to filter products based on various attributes like brand, price, category, and more.
   - **Key Components:**
     - **ProductSearchModel:** The primary class used to perform searches in SFCC.
     - **Search Refinements:** Filters applied to narrow down search results based on specific criteria (e.g., color, size, price range).
     - **Sorting Options:** Order search results by relevance, price, name, or other attributes.

#### 3. **ProductSearchModel: The Backbone of Product Searches**
   - **What is ProductSearchModel?**
     - The `ProductSearchModel` class in SFCC is the main tool for performing product searches. It allows developers to define search criteria, execute the search, and retrieve the results.
   - **Key Methods:**
     - **`setSearchPhrase(phrase)`**: Sets the search phrase to find products matching specific keywords.
     - **`setCategoryID(categoryID)`**: Restricts the search to products within a specific category.
     - **`addRefinementValue(attribute, value)`**: Adds a refinement based on product attributes (e.g., brand, color).
     - **`setSortingOption(option)`**: Specifies how the search results should be sorted.
     - **`search()`**: Executes the search based on the defined criteria.
     - **`getProductSearchHits()`**: Retrieves the list of products that match the search criteria.

   - **Example:**
     ```javascript
     var ProductSearchModel = require('dw/catalog/ProductSearchModel');
     var searchModel = new ProductSearchModel();
     searchModel.setSearchPhrase('laptop');
     searchModel.addRefinementValue('brand', 'Apple');
     searchModel.setSortingOption('price_asc');
     searchModel.search();
     var products = searchModel.getProductSearchHits().asList();
     ```

   - **Explanation:**
     - This example searches for "laptop" products, refines the results to only include the "Apple" brand, and sorts them by price in ascending order.

#### 4. **Using Product Searches on Different Pages**
   - **Category Pages:**
     - **Scenario:** On category pages, you often want to display products within a specific category, with the ability to refine the results using filters like brand, price, and more.
     - **Implementation:** Use `ProductSearchModel` with the `setCategoryID` method to focus the search on a specific category. Apply refinements based on user-selected filters.

   - **Search Results Pages:**
     - **Scenario:** When a user performs a search using the site’s search bar, you need to display relevant products on the search results page.
     - **Implementation:** Set the search phrase using `setSearchPhrase` and apply necessary refinements or sorting to display the most relevant results.

   - **Product Detail Pages (PDP):**
     - **Scenario:** On PDPs, you might want to display related or suggested products based on the brand or category of the current product.
     - **Implementation:** Use `ProductSearchModel` to find products with similar attributes (e.g., same brand or category) and limit the results to a specific number (e.g., 4 products).

#### 5. **Advanced Search Techniques**
   - **Combining Multiple Criteria:**
     - **Scenario:** Sometimes, you need to refine searches using multiple criteria, such as combining brand, price range, and product availability.
     - **Implementation:** Chain multiple `addRefinementValue` calls to refine searches based on various attributes.

     ```javascript
     searchModel.addRefinementValue('brand', 'Nike');
     searchModel.addRefinementValue('price', { min: 50, max: 150 });
     searchModel.addRefinementValue('availability', 'inStock');
     searchModel.search();
     ```

   - **Pagination:**
     - **Scenario:** For search results or category pages with a large number of products, pagination is essential.
     - **Implementation:** Use methods like `searchModel.getPageSize()` and `searchModel.getPageNumber()` to handle pagination effectively.

   - **Handling No Results:**
     - **Scenario:** There are cases where a search might return no results, which requires a fallback mechanism or a message to the user.
     - **Implementation:** Check if `getProductSearchHits()` returns an empty list and display an appropriate message or suggest alternative products.

#### 6. **Best Practices for Product Searches**
   - **Performance Optimization:**
     - **Caching:** Cache search results where appropriate to reduce load times and improve performance.
     - **Minimal Data Retrieval:** Retrieve only the necessary product attributes to minimize the processing time and improve efficiency.
     - **Lazy Loading:** For large sets of results, consider implementing lazy loading or infinite scroll to enhance user experience.

   - **User Experience:**
     - **Relevance:** Ensure that search results are relevant to the user's query and consider implementing advanced algorithms like faceted search or AI-driven recommendations.
     - **Refinement Options:** Offer clear and intuitive filtering options to help users refine their searches easily.
     - **Sorting Flexibility:** Provide multiple sorting options to cater to different user preferences (e.g., by price, popularity, or newest).

#### 7. **Summary**
   - **Recap:** In this chapter, we explored the fundamentals of product searches in SFCC, focusing on how to use `ProductSearchModel` to perform and refine searches. We covered different scenarios where product searches are used, from category pages to search results pages, and highlighted best practices for optimizing search performance and user experience.

#### 8. **Preparing for the Next Chapter**
   - **Transition:** Now that you have a good understanding of product searches, you’re ready to apply this knowledge in a practical scenario. In the next chapter, you will be tasked with implementing a feature that displays suggested products on the PDP based on the brand of the current product. Be sure to refer back to this chapter as needed while working through the exercise.

This chapter sets the foundation for understanding product searches in Salesforce Commerce Cloud, preparing students to apply these concepts in more complex scenarios, such as the suggested products feature discussed in the following chapter.

### Chapter: Implementing Interactive Category Pills on the Category Landing Page (CLP)

#### 1. **Introduction**
   - **Objective:** By the end of this chapter, students will learn how to implement interactive category pills on the Category Landing Page (CLP) in Salesforce Commerce Cloud (SFCC). These category pills will allow users to filter products dynamically by selecting and deselecting categories.
   - **Prerequisites:** Familiarity with SFCC controllers, ISML templates, and basic JavaScript. Understanding of the Category Landing Page structure and product filtering mechanisms.

#### 2. **Understanding the Use Case**
   - **Scenario:** Category pills are a user-friendly way to filter products on the CLP. Each pill represents a specific product category, and users can select or deselect these pills to dynamically filter the products displayed on the page.
   - **Expected Outcome:** The CLP will display a series of category pills at the top of the page. When a pill is selected, the products on the page will be filtered to show only those belonging to the selected category. Multiple pills can be selected simultaneously, allowing for compound filtering.

#### 3. **Step-by-Step Implementation Guide**

   ##### Step 1: **Fetch Categories to Display as Pills**
   - **Task:** Retrieve the list of categories to be displayed as interactive pills on the CLP.
   - **Guide:**
     - Use the `CategoryMgr` class to fetch subcategories of the current category.
     - Ensure that the categories are active and have products available.
   - **Sample Code:**
     ```javascript
     var CategoryMgr = require('dw/catalog/CategoryMgr');
     var currentCategory = CategoryMgr.getCategory('your-category-id'); // Replace with your category ID
     var subcategories = currentCategory.hasSubCategories() ? currentCategory.getSubCategories() : [];
     ```
   - **Hint:** You can customize the category selection logic based on your site’s taxonomy. Consider excluding categories without products.

   ##### Step 2: **Render Category Pills in the ISML Template**
   - **Task:** Display the fetched categories as interactive pills on the CLP.
   - **Guide:**
     - Create a new ISML template (e.g., `categoryPills.isml`) to render the pills.
     - Loop through the subcategories and display each as a selectable pill.
     - Use data attributes to store category IDs for easy access during interactions.
   - **Sample ISML:**
     ```isml
     <isloop items="${subcategories}" var="subcategory">
         <div class="category-pill" data-category-id="${subcategory.ID}">
             ${subcategory.displayName}
         </div>
     </isloop>
     ```
   - **Hint:** Style the pills using CSS to ensure they are visually distinct and easy to interact with

### Chapter: Implementing Dynamic Category Pills for Selected Refinements

#### 1. **Introduction**
   - **Objective:** In this chapter, students will learn how to dynamically display and manage category pills on the Category Landing Page (CLP) in Salesforce Commerce Cloud (SFCC). These pills will represent the selected categories or search refinements. When a refinement is selected, it will appear as a pill; clicking on the pill will remove it and deselect the associated refinement.
   - **Prerequisites:** Knowledge of SFCC controllers, ISML templates, JavaScript, and AJAX. Familiarity with the search refinement system and product filtering in SFCC.

#### 2. **Understanding the Use Case**
   - **Scenario:** The goal is to enhance the user experience by visually representing selected categories or refinements as pills. This allows users to easily see and manage their active filters. When a user selects a category or search refinement, it should be displayed as a pill. If the user clicks on the pill, the refinement should be deselected and the pill removed.
   - **Expected Outcome:** The CLP will display pills for active refinements. Users can click on a pill to remove the refinement and update the product list dynamically.

#### 3. **Step-by-Step Implementation Guide**

   ##### Step 1: **Capture Selected Refinements**
   - **Task:** Identify and capture the selected refinements (e.g., categories, brands, price ranges) that will be displayed as pills.
   - **Guide:**
     - Use the `ProductSearchModel` to retrieve the selected refinements.
     - After the search or filter operation, inspect the selected refinements using the `getRefinements()` method.
   - **Sample Code:**
     ```javascript
     var ProductSearchModel = require('dw/catalog/ProductSearchModel');
     var searchModel = new ProductSearchModel();
     searchModel.setCategoryID('your-category-id'); // Replace with your category ID
     searchModel.search();
     var selectedRefinements = searchModel.getRefinements().iterator();
     ```
   - **Hint:** Refinements can include various attributes like brand, price, and categories. Make sure to handle each type appropriately.

   ##### Step 2: **Render Selected Refinements as Pills**
   - **Task:** Display the selected refinements as pills on the CLP.
   - **Guide:**
     - Create an ISML template (e.g., `selectedRefinementsPills.isml`) to render the pills.
     - Loop through the selected refinements and create a pill for each one.
     - Include the refinement's identifier (e.g., ID, value) in a data attribute for easy access when interacting with the pill.
   - **Sample ISML:**
     ```isml
     <isloop items="${selectedRefinements}" var="refinement">
         <div class="refinement-pill" data-refinement-id="${refinement.value}">
             ${refinement.displayValue} <span class="remove-pill">x</span>
         </div>
     </isloop>
     ```
   - **Hint:** The `remove-pill` span acts as a button to remove the pill. Style it appropriately to make it user-friendly.

   ##### Step 3: **Handle Pill Interactions with JavaScript**
   - **Task:** Implement the functionality to remove a pill and update the product search when a pill is clicked.
   - **Guide:**
     - Write JavaScript to listen for clicks on the pill’s remove button.
     - When clicked, retrieve the refinement ID from the data attribute, remove the refinement from the search model, and re-execute the search.
     - Update the CLP dynamically using AJAX to avoid full page reloads.
   - **Sample JavaScript:**
     ```javascript
     $(document).on('click', '.remove-pill', function() {
         var refinementId = $(this).closest('.refinement-pill').data('refinement-id');

         // Update the search model and deselect the refinement
         var searchModel = new dw.catalog.ProductSearchModel();
         searchModel.setCategoryID('your-category-id'); // Replace with your category ID
         searchModel.search();

         // Example of deselecting the refinement (specific implementation may vary)
         searchModel.addRefinementValue('category', refinementId);
         searchModel.search();

         // Update the page using AJAX
         $.ajax({
             url: '/path-to-your-controller',
             method: 'GET',
             data: { selectedRefinements: searchModel.getRefinements() },
             success: function(response) {
                 // Update the CLP with the new filtered product list and pills
                 $('#product-list-container').html(response.productListHtml);
                 $('#selected-refinements-container').html(response.selectedRefinementsHtml);
             }
         });
     });
     ```
   - **Hint:** Adjust the AJAX request and response handling based on your site’s structure and controller logic. Make sure the search results and pills are updated without refreshing the entire page.

   ##### Step 4: **Update the CLP with AJAX**
   - **Task:** Ensure the CLP updates smoothly when pills are added or removed.
   - **Guide:**
     - Use AJAX to refresh only the parts of the page that need updating: the product list and the selected refinement pills.
     - Ensure the server-side controller returns the necessary HTML snippets for both the product list and the pills.
   - **Sample Controller Response:**
     ```javascript
     function updateCLP() {
         var productSearchModel = new dw.catalog.ProductSearchModel();
         productSearchModel.search();

         var productListHtml = renderTemplate('productList', { products: productSearchModel.getProductSearchHits() });
         var selectedRefinementsHtml = renderTemplate('selectedRefinementsPills', { selectedRefinements: productSearchModel.getRefinements() });

         return { productListHtml: productListHtml, selectedRefinementsHtml: selectedRefinementsHtml };
     }
     ```
   - **Hint:** Optimize the AJAX calls to ensure quick response times, especially when dealing with large product catalogs or complex refinements.

#### 4. **Handling Edge Cases**
   - **Task:** Ensure the solution is robust and handles scenarios where:
     - No refinements are selected.
     - Multiple refinements are selected.
     - The user deselects all refinements.
   - **Guide:**
     - Implement conditional checks in both the ISML template and JavaScript to manage cases where no pills should be displayed.
     - Ensure that removing all pills resets the product list to display all available products.

   - **Sample ISML Condition:**
     ```isml
     <isif condition="${!empty(selectedRefinements)}">
         <isinclude template="selectedRefinementsPills.isml" />
     <iselse>
         <div>No refinements selected.</div>
     </isif>
     ```

#### 5. **Best Practices and Optimization**
   - **Performance Considerations:**
     - Minimize the number of AJAX calls by batching requests where possible.
     - Cache refinement data when appropriate to reduce server load.
   - **User Experience:**
     - Ensure pills are easy to interact with on both desktop and mobile devices.
     - Provide visual feedback (e.g., loading spinners) during AJAX operations.

#### 6. **Summary**
   - **Recap:** This chapter guided you through the process of creating dynamic, interactive category pills on the CLP. You learned how to capture selected refinements, display them as pills, and handle user interactions to update the product list dynamically.
   - **Transition:** Now that you have implemented this feature, you are well-prepared to tackle more advanced filtering and search functionalities, enhancing the overall user experience on your site.

This chapter provides a comprehensive guide to implementing interactive category pills, ensuring that students can effectively manage and display selected refinements in a dynamic and user-friendly manner.







