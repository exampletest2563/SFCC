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
