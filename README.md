# Belly Button Biodiversity Dashboard

This module uses Javascript to create an interactive dashboard to explore the Belly Button Biodiversity dataset, which catalogs the microbes that colonize human navels. The dataset reveals that a small handful of microbial species (also called operational taxonomic units, or OTUs, in the study) were present in more than 70% of people, while the rest were relatively rare.

## Table of Contents

1. [Overview](#overview)
2. [Files](#files)
3. [Data Visualization](#data-visualization)
   - [Bar Chart](#bar-chart)
   - [Bubble Chart](#bubble-chart)
4. [Metadata Display](#metadata-display)
   - [Building Metadata Panel](#building-metadata-panel)
   - [Updating Metadata](#updating-metadata)
5. [Deployment](#deployment)


## Overview

The Belly Button Biodiversity Dashboard visualizes microbial data from human navels. The dashboard features:

- A dropdown menu for selecting a sample.
- A horizontal bar chart showing the top 10 OTUs found in the selected sample.
- A bubble chart displaying each sample.
- A panel showing the demographic metadata for the selected sample.

## Files

- **`index.html`**: The main HTML file that sets up the structure and layout of the dashboard.
- **`samples.json`**: Contains the dataset used for visualizations (available online at [this URL](https://static.bc-edx.com/data/dl-1-2/m14/lms/starter/samples.json)).
- **`static/js/app.js`**: Contains the JavaScript code that handles data visualization and interactivity.

## Data Visualization

### Bar Chart

1. Use the D3 library to read in `samples.json` from the URL: [https://static.bc-edx.com/data/dl-1-2/m14/lms/starter/samples.json](https://static.bc-edx.com/data/dl-1-2/m14/lms/starter/samples.json).

2. Create a horizontal bar chart to display the top 10 OTUs found in each individual sample:
   - Use `sample_values` as the values for the bar chart.
   - Use `otu_ids` as the labels for the bar chart.
   - Use `otu_labels` as the hover text for the chart.

   ```javascript
   let bar_chart = {
     x: getSampleValues.slice(0, 10).reverse(),
     y: getOtuIds.slice(0, 10).map(id => `OTU ${id}`).reverse(),
     text: getOtuLabels.slice(0, 10).reverse(),
     type: "bar",
     orientation: "h"
   };
    ```
3. Render the Bar Chart using Plotly
    ```javascript
    Plotly.newPlot("bar", [bar_chart], barLayout);
    ```
    ![precipitation](https://github.com/omidk414/sqlalchemy-challenge/blob/main/images/precipitation.png)


### Bubble Chart

1. **Create a Bubble Chart** to display each sample:

   - **x values:** Use `otu_ids`.
   - **y values:** Use `sample_values`.
   - **Marker Size:** Use `sample_values` for the size of the markers.
   - **Marker Color:** Use `otu_ids` for the color of the markers.
   - **Text Values:** Use `otu_labels` for the text values displayed on the chart.

   ```javascript
   let bubble_chart = {
     x: getOtuIds,
     y: getSampleValues,
     text: getOtuLabels,
     mode: "markers",
     marker: {
       color: getOtuIds,
       size: getSampleValues,
       colorscale: "Earth"
     }
   };

2. **Define the Layout for the Bubble Chart**
   - **Title:** Set the chart title.
   - **x-axis:** Label the x-axis as "OTU ID".
   - **y-axis:** Label the y-axis as "Number of Bacteria".
   - **Height and Width:** Adjust the chart's dimensions.

   ```javascript
   let bubble_layout = {
     title: "Bacteria Cultures Per Sample",
     xaxis: { title: "OTU ID" },
     yaxis: { title: "Number of Bacteria" },
     height: 600,
     width: 1000
   };
   ```


3. **Render the Bubble Chart using Plotly**
    ```javascript
    Plotly.newPlot("bubble", [bubble_chart], bubble_layout);

    ```
    ![precipitation](https://github.com/omidk414/sqlalchemy-challenge/blob/main/images/precipitation.png)

### Metadata Display

1. **Display the Sample's Metadata:** Show the demographic information of the selected sample by creating a metadata panel.

   - **Build Metadata Panel:**
     - Fetch metadata for the selected sample.
     - Loop through each key-value pair from the metadata JSON object and create a text string.
     - Append an HTML tag with that text to the `#sample-metadata` panel.

   ```javascript
   function buildMetadata(sample) {
     d3.json("https://static.bc-edx.com/data/dl-1-2/m14/lms/starter/samples.json").then((data) => {
       let metadata = data.metadata;
       let resultArray = metadata.filter(sampleObj => sampleObj.id == sample);
       let result = resultArray[0];
       let panel = d3.select("#sample-metadata");
       panel.html("");
       Object.entries(result).forEach(([key, value]) => {
         panel.append("h6").text(`${key.toUpperCase()}: ${value}`);
       });
     });
   }
   ```
    ![precipitation](https://github.com/omidk414/sqlalchemy-challenge/blob/main/images/precipitation.png)

### Updating Metadata

1. **Update Metadata and Charts:** Ensure that the metadata panel and charts are updated whenever a new sample is selected.

   - **Function for Event Listener:** This function will be triggered when a new sample is selected from the dropdown menu. It calls the functions to build both the charts and metadata panel with the selected sample.

   ```javascript
   function optionChanged(newSample) {
     buildCharts(newSample);
     buildMetadata(newSample);
   }
   ```
   ![precipitation](https://github.com/omidk414/sqlalchemy-challenge/blob/main/images/precipitation.png)

### Deployment

1. **HTML File:** This project uses an HTML file (`index.html`) to structure and display the interactive dashboard.

2. **Local Development:** To develop and test the site locally, use the Visual Studio Code extension **"Live Server"**. This extension allows you to view changes in real-time as you edit the HTML, CSS, and JavaScript files.

   - **Install Live Server:**
     - Go to the Extensions view in VSCode (`Ctrl+Shift+X`).
     - Search for "Live Server" and install it.

   - **Run Live Server:**
     - Open the `index.html` file in VSCode.
     - Right-click on the file and select "Open with Live Server" or click on the "Go Live" button in the bottom-right corner of the VSCode window.

   This will open your HTML file in a web browser and automatically refresh the page as you make changes to the files, allowing for an efficient development workflow.




