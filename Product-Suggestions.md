### Chapter: Implementing a Suggested Products Section on the PDP Based on Brand

In this chapter, you will learn how to enhance the Product Detail Page (PDP) of your Salesforce Commerce Cloud (SFCC) storefront by adding a section that displays four suggested products based on the brand of the currently viewed product. We'll cover identifying the correct controller and template, extending the controller with custom logic, fetching relevant products using the ProductSearchModel, and rendering the results in a custom template.

#### Step 1: Identify the Controller and Template for Rendering Products

The first step is to identify which controller and template are responsible for rendering the PDP. In Salesforce Commerce Cloud, the PDP is typically managed by the `Product-Show` controller and its associated templates.

1. **Locate the Product-Show Controller:**
   - Navigate to the `app_storefront_base/cartridge/controllers` directory in your SFCC project.
   - Open the `Product.js` controller file and locate the `show` action. This action is responsible for rendering the PDP.

2. **Identify the Associated Template:**
   - The `Product-Show` controller typically renders the `product/product` template, located in the `app_storefront_base/cartridge/templates/default` directory.
   - This template is the base for the PDP and will be where you integrate the suggested products section.

#### Step 2: Extend the Product-Show Controller Using Custom Cartridge

Now that you've identified the correct controller, the next step is to extend it using your custom cartridge. In SFCC, extending a controller allows you to add custom functionality without altering the base cartridge, making it easier to manage updates.

1. **Create a Custom Cartridge:**
   - If you haven't already, create a custom cartridge (e.g., `app_custom_pdp`). Ensure that this cartridge is added to the cartridge path in your project configuration.
   
2. **Extend the Product-Show Controller:**
   - In your custom cartridge, create a `Product.js` file within the `cartridge/controllers` directory.
   - Use the `module.superModule` pattern to extend the existing `Product-Show` controller.

   ```javascript
   var server = require('server');
   var ProductShow = module.superModule;

   server.extend(ProductShow);

   server.append('Show', function (req, res, next) {
       var ProductSearchModel = require('dw/catalog/ProductSearchModel');
       var productHelper = require('*/cartridge/scripts/helpers/productHelpers');

       var product = res.getViewData().product;
       var brandID = product.brand; // Assuming `brand` is a custom attribute on the product

       var productSearchModel = new ProductSearchModel();
       productSearchModel.setSearchPhrase(brandID);
       productSearchModel.setCategoryID(null);
       productSearchModel.search();

       var suggestedProducts = productSearchModel.getProductSearchHits().asList().toArray().slice(0, 4);

       res.setViewData({
           suggestedProducts: suggestedProducts
       });

       next();
   });

   module.exports = server.exports();
   ```

3. **Add Logic to Fetch Suggested Products:**
   - In the `Show` action, after the original controller logic runs, fetch products from the same brand using the `ProductSearchModel` class. Limit the results to 4 products and sort them by price.

#### Step 3: Fetch the Brand of the Current Product

To display products related to the same brand, you need to retrieve the brand information of the current product.

1. **Get the Current Product's Brand:**
   - In the `Show` action of your extended controller, extract the brand from the product object. This can typically be done through custom attributes or existing properties on the product.

   ```javascript
   var brandID = product.brand; // Replace 'brand' with the actual custom attribute ID if necessary
   ```

2. **Use the ProductSearchModel to Retrieve Related Products:**
   - Use the `ProductSearchModel` to search for products within the same brand. This class allows you to perform complex searches and sort results, which is ideal for this use case.

   ```javascript
   var productSearchModel = new ProductSearchModel();
   productSearchModel.setSearchPhrase(brandID);
   productSearchModel.setSortingRule('price-asc'); // Sort products by ascending price
   productSearchModel.search();
   ```

#### Step 4: Send an Array of 4 Products to the Template

With the products retrieved, the next step is to pass this data to the PDP template so that it can be rendered.

1. **Limit and Prepare the Products Array:**
   - Use JavaScript’s `slice()` method to limit the product list to four items.

   ```javascript
   var suggestedProducts = productSearchModel.getProductSearchHits().asList().toArray().slice(0, 4);
   ```

2. **Send the Data to the Template:**
   - Pass the array of products to the template using `res.setViewData()`.

   ```javascript
   res.setViewData({
       suggestedProducts: suggestedProducts
   });
   ```

#### Step 5: Create the Suggested Products Template

Now that the data is prepared, you'll need to create a template that will render these suggested products on the PDP.

1. **Create the `suggestedProducts` Template:**
   - In your custom cartridge, create a new template named `suggestedProducts.isml` in the `cartridge/templates/default/product` directory.
   - This template should iterate over the `suggestedProducts` array and render each product's basic information, such as the name, image, and price.

   ```html
   <div class="suggested-products">
       <h2>Suggested Products</h2>
       <ul>
           <isloop items="${suggestedProducts}" var="product">
               <li>
                   <a href="${product.productID}">
                       <img src="${product.imageURL}" alt="${product.productName}" />
                       <p>${product.productName}</p>
                       <p>${product.price}</p>
                   </a>
               </li>
           </isloop>
       </ul>
   </div>
   ```

2. **Include the Template in the PDP:**
   - Finally, include this `suggestedProducts` template in the main PDP template (`product/product.isml`), positioning it where you want the suggested products to appear.

   ```html
   <!-- Other product details -->
   <isinclude template="product/suggestedProducts"/>
   ```

### Conclusion

By following these steps, you have successfully added a "Suggested Products" section to your PDP based on the brand of the current product. You extended the `Product-Show` controller in your custom cartridge, utilized the `ProductSearchModel` to fetch relevant products, and created a custom template to display them. This approach not only enhances the shopping experience but also leverages Salesforce Commerce Cloud's modular architecture, ensuring that your customizations are maintainable and upgradable.
