# How to Open the Power BI Dashboard

The Power BI dashboard uses the Excel dataset included in this repository. Since the `.pbix` file stores the original file path of the data source, Power BI may show a **"File Source Not Available"** or similar data-source error when the project is opened on another computer.

Please follow the steps below to connect the dashboard to the included dataset.

## Step 1: Download the Repository

Download or clone this GitHub repository to your computer.

## Step 2: Locate the Project Files

After downloading, you will find the following folders:

```text
Raw Data/
    └── ad_tech_campaign_raw_data.xlsx

Power BI Dashboard/
    └── ad_tech_campaign_performance_analysis.pbix
```

Keep both folders and files together in the downloaded project folder.

## Step 3: Open the Power BI File

Open:

`Power BI Dashboard → ad_tech_campaign_performance_analysis.pbix`

The dashboard may initially display a **data source/file path error** because the original Excel file path is different on your computer.

**This is expected.**

## Step 4: Update the Data Source

In Power BI Desktop:

1. Go to **Home → Transform data → Data source settings**.

2. Select the Excel data source that is showing the error.

3. Click **Change Source...**

4. Browse to the downloaded repository folder.

5. Open:

   `Raw Data → ad_tech_campaign_raw_data.xlsx`

6. Click **OK**.

7. Close the **Data Source Settings** window.

## Step 5: Refresh the Data

In Power BI Desktop, select:

**Home → Refresh**

Allow Power BI to refresh the dataset.

## Troubleshooting Refresh Errors

If the dashboard does not refresh successfully, try the following:

### 1. Check the Excel File Path

Make sure you selected the correct file:

`Raw Data → ad_tech_campaign_raw_data.xlsx`

Do not select a different copy of the dataset.

### 2. Check That the Excel File Has Not Been Moved

After updating the data source, avoid moving or renaming the Excel file or its parent folder until the dashboard has been successfully refreshed.

### 3. Check for Power Query Errors

If Power BI displays a **Power Query** or **Applied Steps** error:

1. Go to **Home → Transform data**.
2. Check the queries in the **Queries** pane.
3. Select the query showing the error.
4. Review the **Applied Steps** pane.
5. Make sure the source table/columns required by the transformation steps are available in the Excel dataset.

### 4. Refresh Again

After correcting the data source or query issue, select:

**Home → Refresh**

If the refresh completes without errors, the dashboard is ready to use.

> **Note:** The project was created in Power BI Desktop. For the best experience, open the `.pbix` file using **Power BI Desktop** rather than trying to open it directly in a web browser.

## Step 6: Check the Dashboard

Once the data has refreshed successfully, the dashboard should be fully functional.

You can interact with:

* KPI cards
* Charts and visualizations
* Campaign filters
* Date filters
* Country filters
* Device filters
* Ad-format filters
* Cross-filtering between visuals
* Other slicers and report interactions

The Power BI file contains the data model, calculations, visuals, and dashboard layout used for the project.

# Important Note

The Excel file included in the `Raw Data` folder is provided so that the dashboard can be reconnected to the data source when opened on another computer.

The dataset itself has not been altered from the source dataset used to build the dashboard.
