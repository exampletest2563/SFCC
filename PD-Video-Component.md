### Chapter: Adding a Video Component in Salesforce Commerce Cloud Using Page Designer

#### Objective

In this chapter, you will learn how to create and configure a Video Component using Salesforce Commerce Cloud’s Page Designer. You will add the component to a page and configure it to display a video from a given URL.

---

### Step 1: Identify the Controller and Template Needed

**1. Determine the Controller Handling Video Components**

The first step is to identify or create the controller responsible for managing video content. In Salesforce Commerce Cloud, video components might not have a dedicated controller but could be managed through custom logic in your project’s controllers.

- **Locate the Controller File:**
  - Navigate to your custom cartridge’s `controllers` directory (e.g., `app_custom_cartridge/cartridge/controllers`).
  - If a video component controller does not exist, you might need to create a new controller or extend an existing one.

- **Identify the Associated Template:**
  - Determine the template that will render the video component. This template will be located in the `templates` directory (e.g., `app_custom_cartridge/cartridge/templates/default`).
  - For this exercise, you might create a new template such as `videoComponent.isml`.

---

### Step 2: Create or Extend the Controller for Video Component

**1. Create or Extend the Controller**

To manage the logic for displaying videos, you need to create or extend a controller that handles video components.

- **Create a Custom Cartridge (if not already created):**
  - Set up a custom cartridge (e.g., `app_custom_video_component`) and ensure it is included in your cartridge path configuration.

- **Create or Extend the Controller:**
  - In your custom cartridge, create a new file named `VideoComponent.js` within the `controllers` directory.

  ```javascript
  var server = require('server');
  var URLUtils = require('dw/web/URLUtils');

  server.get('ShowVideo', function (req, res, next) {
      var videoURL = req.querystring.videoURL;

      // Validate the video URL
      if (!videoURL) {
          videoURL = '';
      }

      res.render('videoComponent', { videoURL: videoURL });
      next();
  });

  module.exports = server.exports();
  ```

**2. Create the Associated Template**

Create the `videoComponent.isml` template to handle the display of the video.

- **Template for Video Component:**

  **File:** `videoComponent.isml`

  ```html
  <isif condition="${videoURL}">
      <div class="video-container">
          <video controls>
              <source src="${videoURL}" type="video/mp4">
              Your browser does not support the video tag.
          </video>
      </div>
  </isif>
  <iselse>
      <p>No video available.</p>
  </iselse>
  ```

---

### Step 3: Add and Configure the Video Component in Page Designer

**1. Add the Video Component to a Page**

Now that the controller and template are set up, you can add the Video Component to a page using Page Designer.

- **Log in to Business Manager:**
  - Open your browser and navigate to your Salesforce Commerce Cloud Business Manager URL.
  - Log in using your credentials.

- **Navigate to Page Designer:**
  - In the Business Manager menu, go to **Content**.
  - Select **Page Designer** to open the Page Designer interface.

- **Create a New Page or Edit an Existing Page:**
  - Click **Create New Page** or select an existing page from the list.
  - Name your page (e.g., “Video Page”) and choose the template you want to use.

- **Add the Video Component:**
  - On the page editor, click **Add Component**.
  - Search for the **Video Component** and drag it to the desired location on your page.

**2. Configure the Video Component**

- **Component Configuration:**
  - Click on the newly added Video Component to open its configuration panel.
  - **Component Name:** Enter a name for the component (e.g., “Video Player”).
  - **Video URL:** Enter the URL of the video you want to display. Ensure the URL points to a video file compatible with HTML5 (e.g., MP4 format).

- **Save and Preview:**
  - Click **Save** to apply the configuration.
  - Use the **Preview** function to view the page and ensure the video displays correctly.

---

### Step 4: Implement Custom Logic (Optional)

**1. Add Logic for Dynamic Video URL**

If you want the video URL to be dynamically set or fetched from a data source, you can extend the controller or add custom logic to manage video URLs.

- **Update `VideoComponent.js` with Custom Logic:**

  ```javascript
  server.get('ShowVideo', function (req, res, next) {
      var videoID = req.querystring.videoID;
      var videoURL = '';

      // Example logic to fetch video URL based on ID
      if (videoID) {
          var videoData = getVideoDataByID(videoID); // Implement this function based on your data source
          videoURL = videoData ? videoData.url : '';
      }

      res.render('videoComponent', { videoURL: videoURL });
      next();
  });

  function getVideoDataByID(id) {
      // Fetch video data from your data source
      // Return an object with the video URL
      return {
          url: 'https://example.com/video.mp4'
      };
  }
  ```
