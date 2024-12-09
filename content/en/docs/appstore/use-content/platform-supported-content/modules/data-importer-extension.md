---
title: "Data Importer"
url: /appstore/modules/data-importer/
description: "Overview of the Data Importer in Studio Pro"
aliases:
    -  /appstore/modules/data-importer-extension/
---

## Introduction

The [Data Importer](https://marketplace.mendix.com/link/component/219833) allows you to import data from an Excel or comma-separated value (CSV) file. You can choose which sheet and columns to import, preview the data, and create a non-persistable entity (NPE) in your domain model that corresponds to your input. Then, you can import data into your app using the [Import Data from File](/refguide/import-data-from-file/) activity.

{{% alert color="info" %}}
The Data Importer is available in [Studio Pro 10.6](/releasenotes/studio-pro/10.6/) and above.
{{% /alert %}}

The Data Importer Document can also be used as a source for creating *Import Mapping* [(https://docs.mendix.com/refguide/import-mappings/)]. This Import Mapping can import data from Excel/CSV file using the *Import With Mapping* [Import with Mapping](https://docs.mendix.com/refguide/import-mapping-action/) activity.

{{% alert color="info" %}}
This Data Importer Document as a source for Import Mapping is available in [Studio Pro 10.18](/releasenotes/studio-pro/10.18/) and above.
{{% /alert %}}

### Typical Use Cases

The Data Importer extension allows you to import data from Excel and CSV files directly into your app. You can create a Data Importer document to define which columns to import and an NPE to hold the imported data, along with source-to-target mapping. 

### Features {#features}

This extension supports following source files:

* Microsoft Excel (*.xls, .xlsx, .csv*)

### Limitations

This extension currently has the following limitations:

* The Excel column cell type is taken from the source file to determine the target attribute type; this cannot be changed during the data preview stage
* Source data can be mapped to one entity only; associations are not currently supported 
* You cannot map data to an existing NPE; you have to create a new entity as part of mapping
* Enumerations are not supported
* **String** is the default attribute type (*.csv* only)

### Prerequisites

* Studio Pro 10.6 or above

### Installation

Download the [Data Importer](https://marketplace.mendix.com/link/component/219833) from the Marketplace and [add it into your app](/appstore/use-content/).

## Data Importer Document (with Implicit Mapping)

### Creating a Data Importer Document {#create-document}

To import data, right-click on the module and click **Add other** > **Data Importer**. Name the document, click **OK**, and the new Data Importer document opens. 

### Previewing Data {#preview-data}

Once you have [created the Data Importer document](#create-document), click **Select a local file** to import an Excel file (*.xls* or *.xslx*) or CSV file (*.csv*).

{{< figure src="/attachments/appstore/platform-supported-content/modules/data-importer-extension/select-file-for-preview.png" class="no-border" >}}

#### Previewing Excel Data {#preview-data-excel}

Select or drop the file in the **Select Source File** window. You can choose which sheet to import data from and specify the header row and starting data row.

* **Sheet Name** – name of the worksheet from where data needs to be imported; if the Excel has multiple worksheets, the sheet name appears in the drop-down
* **Header Row No.** – row number of the file header; the default is 1
* **Read Data From Row No.** – starting line for reading data; the default is 2

{{< figure src="/attachments/appstore/platform-supported-content/modules/data-importer-extension/select-sheet-and-header-data-row.png" class="no-border" >}}

Click **Preview Source Data & Entity** to view the data from the file. The first 10 rows from the source file are shown in the data preview section. The Sheet Name is used to create a NPE, but this can be edited. The column names correspond to the attribute names within the entity.

All the columns are selected (checked) by default. You can uncheck the columns you do not want to import. At the bottom of the table, you can see the target data type of the attribute, which is based on the cell type defined in the file's first data row. If any data types are incorrect, check the cell type of the first data row in Excel and adjust the cell type definition accordingly.

{{% alert color="warning" %}} Column names that do not adhere to Mendix naming conventions will be autocorrected. {{% /alert %}}

{{< figure src="/attachments/appstore/platform-supported-content/modules/data-importer-extension/preview-data-and-entity.png" class="no-border" >}}

##### Header and data row numbering

The empty rows before the start of actual header and data row(s) are trimmed in the preview. This means the preview will be skewed if the provided header row value is >1. To avoid this, you can remove the empty rows yourself before uploading the file and assign the header row as 1, or make sure the rows before the header row contain some data and keep the header row value as its actual value. 

For example, the below file will result in a confusing preview if **Header Row No.** is 2 and **Read Data From** is 3. In this scenario, the first row (which is empty) should be removed from the input Excel file. Then, **Header Row No.** should be set as 1 and **Read Data From** as 2. Otherwise, a static test should be given in any column of first row to continue with **Header Row No.** as 2.

{{< figure src="/attachments/appstore/platform-supported-content/modules/data-importer-extension/empty-row-before-header.png" class="no-border" >}}

#### Previewing CSV Data {#preview-data-csv}

Select or drop the CSV file in the **Select Source File** window. CSV import supports multiple combinations of separator/delimiter, quote, and escape characters. It also supports importing files where the header row is absent.

Specify the values for all four configurations (Delimiter, Quote Character, Escape Character, and Add Header Row):

* **Delimiter (Separator)** – supported delimiters are comma, semicolon, pipe, and tab; the default is comma
* **Quote Characters** – supported quote characters are single and double quotes; the default is double quotes
* **Escape Characters** – supported escape characters are backslash, single, and double quotes; the default is double quotes
* **Add Header Row** – specify if you want to add a header row or if the header row is already part of the CSV file; the default is the header row already included in file

Click **Preview Source Data & Entity** to view the data from the file. The first ten rows from the source file are shown in the data preview section. The file name is used to create a NPE, but this can be edited. The column names correspond to the attribute names within the entity.

All the columns are selected (checked) by default. You can uncheck the columns you do not want to import. At the bottom of the table, you can see the target data type of the attribute, which defaults to **String**.

{{% alert color="warning" %}} Column names that do not adhere to Mendix naming conventions will be autocorrected. {{% /alert %}}

For example, for the following source data (CSV), the separator is specified as Comma. The Quote and Escape Characters are set to Double Quote, and Header is included in the input file.

{{< figure src="/attachments/appstore/platform-supported-content/modules/data-importer-extension/source-csv-data.png" class="no-border" >}}

The data preview and resulting entity would be as seen below:

{{< figure src="/attachments/appstore/platform-supported-content/modules/data-importer-extension/preview-csv-data-and-entity.png" class="no-border" >}}

### Editing an Entity {#edit-entity}

You can edit the entity in the **Entity Preview** section. The Data Importer supports various ways to:

* Edit the name of resultant entity
* Edit the name of the attribute (or attributes) of the entity
* Edit the data type of a given attribute

Click **Edit** at top-right corner of **Entity Preview**. This will render a pop-up window where you can change the name of the entity. You can also change the name of the attribute; *Original Name* is the name of the column from the input file and *Attribute Name* is the new name you can assign to this column. You can also change the data type of this attribute by selecting a relevant value from the drop-down as shown below.

{{< figure src="/attachments/appstore/platform-supported-content/modules/data-importer-extension/edit-csv-entity.png" class="no-border" >}}

Once you are satisfied with the changes, click **OK** to save or **Cancel** to discard your changes.

{{% alert color="info" %}}
The **Edit Entity** feature is useful for CSV import, as all the columns of a CSV file are marked as String by default, so you can change the data type if necessary. The following table shows the source-to-target data conversion matrix:

Input CSV File
  
| Source Type | Target- String | Target- Int | Target- Long | Target- Decimal | Target- Boolean | Target- DateTime |
| :-------- | :------- | :-------- | :------- | :-------- | :------- | :-------- |
| String  | Yes    | Partial    | Partial    | Partial    | Partial    | No    |

Input Excel File
  
| Source Type | Target- String | Target- Int | Target- Long | Target- Decimal | Target- Boolean | Target- DateTime |
| :-------- | :------- | :-------- | :------- | :-------- | :------- | :-------- |
| String  | Yes    | Partial    | Partial    | Partial    | Partial    | No    |
| Boolean  | Yes    | No    | No    | No    | Yes    | No    |
| Decimal  | Yes    | Partial    | Partial    | Yes    | No    | No    |
| DateTime  | Yes    | No    | No    | No    | No    | Yes    |

**Partial** - If the source data is valid and within range, it will be converted into the target data type.

{{% /alert %}}

{{% alert color="warning" %}}

* **Enum** is not supported as a target data type
* Runtime exceptions can occur if the input data cannot be converted into desired the target data type for various reasons (for example, invalid data, data truncation, casting, etc.)
{{% /alert %}}

### Creating an Entity {#create-entity}

When you are done editing the entity, click **Create Entity** > **OK**. This will create the entity in your domain model.

When the entity is created, you can view the mapping of the source columns to the target entity attributes. 

{{< figure src="/attachments/appstore/platform-supported-content/modules/data-importer-extension/source-to-target-mapping.png" class="no-border" >}}

The Data Importer document creation is complete and can be used to [import data in a microflow](#import-microflow).

## Importing Data in a Microflow {#import-microflow}

Use the previously created Data Importer document to import data from your input file (or files) in a microflow. The example below shows how to import data from an Excel file. The same steps are applicable to import data from CSV files.

1. Create a new microflow and drag the **Import data from file** activity into it.

   {{< figure src="/attachments/appstore/platform-supported-content/modules/data-importer-extension/custom-activity.png" class="no-border" >}}

2. Double-click the activity and in the **File** field, select an input file (Excel or CSV).
3. In the **Data importer document** field, click **Select** and choose the Data Importer document you want to use. Choose an appropriate Data Importer document based on the input file.

   {{< figure src="/attachments/appstore/platform-supported-content/modules/data-importer-extension/choose-data-importer-template.png" class="no-border" >}}

4. After selecting the Data Importer document, the **Return type** and **Variable name** will auto-populate. You can also change the name of the output variable.
5. Click **OK**.

The custom activity is configured and you can import data from input files.

{{< figure src="/attachments/appstore/platform-supported-content/modules/data-importer-extension/example-microflow.png" class="no-border" >}}

## Running Your App

To perform testing, you can do the following actions:

* Provide a placeholder to upload a file (System.FileDocument) on a page and a button to call the configured microflow
* Deploy your app locally and browse and upload an input file that resembles the file used to create Data Importer document
* View the message about x number of rows being imported into a list of entities

{{< figure src="/attachments/appstore/platform-supported-content/modules/data-importer-extension/local-app-run.png" class="no-border" >}}

## Data Importer Document (as a source for Import Mapping)

The [Import Mapping](https://docs.mendix.com/refguide/import-mappings/) document and the [Import with Mapping](https://docs.mendix.com/refguide/import-mapping-action/) activity porvide lot of inherent advantages like controlling the commit of objects, flexibility to find or create an object and so on. This new feature of Data Importer document leverages these capabilities by creating a source strucutre which can be used to create Import Mapping. Makers who are comfortable working with Mapping Documents[(https://docs.mendix.com/refguide/mapping-documents/)] can use this feature to address advance use-cases of importing data into Mendix. The section below describes how one can create a structure, create an Import mapping using this structure and finally leveraging the *Import with Mapping* activity in a Microflow to import data into Mendix.

### Creating a Data Importer Document {#create-document-with-imm}

To import data, right-click on the module and click **Add other** > **Data Importer**. Name the document, enable the *Use with Import mapping* checkbox, click **OK**, and the new Data Importer document opens. 

{{< figure src="/attachments/appstore/platform-supported-content/modules/data-importer-extension/create-DI-doc-with-import-mapping.png" class="no-border" >}}

### Previewing Structure {#preview-structure}

Once you have [created the Data Importer document](#create-document-with-imm), click **Select a local file** to upload a sample Excel file (*.xls* or *.xslx*) or CSV file (*.csv*).

{{< figure src="/attachments/appstore/platform-supported-content/modules/data-importer-extension/select-file-for-structure-preview.png" class="no-border" >}}

You can choose which sheet to import data from and specify the header row and starting data row.

* **Sheet Name** – name of the worksheet from where data needs to be imported; if the Excel has multiple worksheets, the sheet name appears in the drop-down
* **Header Row No.** – row number of the file header; the default is 1
* **Read Data From Row No.** – starting line for reading data; the default is 2

Click **Preview Structure Elements** to view the structure of data from the file. The data from first row of the source file is shown in the *schema elements* section. Click **Create Structure** and you will be notified that a new structure is generated successfully.

{{% alert color="warning" %}} Column names that do not adhere to Mendix naming conventions will be autocorrected. {{% /alert %}}

{{< figure src="/attachments/appstore/platform-supported-content/modules/data-importer-extension/preview-data-structure.png" class="no-border" >}}

### Create Import Mapping {#DI-import-mapping}

Using the *Data Importer Document* created in the above step, you can proceed to create *Import Mapping*. Right click on your module or folder > **Add other** > **Import mapping**. Give a proper name to the Import mapping document and Click **OK**, you will be now routed to *Select a schema element for Import mapping*. From the **Schema source**, choose the last radio button *Excel/CSV structure* and select the sheet from the Excel file which was uploaded as sample as shown in the figure below.

{{< figure src="/attachments/appstore/platform-supported-content/modules/data-importer-extension/select-schema-elements-for-imm.png" class="no-border" >}}
 
You can select ALL the columns by pressing **Check all** or selectively choose the columns that you want to import from the input file. After the selction of columns is completed, you can click **OK** to create *Import Mapping*. 

{{< figure src="/attachments/appstore/platform-supported-content/modules/data-importer-extension/create-import-mapping.png" class="no-border" >}}

From here, you can either choose to map an existing Entity by dragging and dropping an existing Entity from your doamin model via **Connector** tab or you can choose to press **Map automatically** to create a new NPE in your domain model.

{{< figure src="/attachments/appstore/platform-supported-content/modules/data-importer-extension/map-automatically.png" class="no-border" >}}

If you choose **Map automatically*, then you can go to your *Domain Model* and change the Entity's name and persistence as per your requirement.

{{< figure src="/attachments/appstore/platform-supported-content/modules/data-importer-extension/entity-name-persist-change.png" class="no-border" >}}

### Import with Mapping Activity in a Microflow {#import-with-mapping-MF}

As this *Data Importer Document* contains a strucutre that is used as a source for *Import mapping*, we can leverage the **Import with mapping** activity to import data from input file(s). The example below shows how to import data from an Excel file. The same steps are applicable to import data from CSV files.

1. Create a new microflow with a parameter (FileDocument) and drag the **Import with mapping** activity into it.
2. Double-click the activity and in the *Input* section > **Variable** field, select an input file (Excel or CSV).
3. Select the *Mapping* in the *Import Mapping* section. Select *Range* and *Commit* Radio buttons as per your requirement.
4. In the *Output* section, you can choose *Store in variable* and click **OK**.
   
{{< figure src="/attachments/appstore/platform-supported-content/modules/data-importer-extension/import-with-mapping-params.png" class="no-border" >}}

### Run your App {#data-import-with-mapping-app}

1. Complete the Microflow to show the a page containing Entities commited after the Import activity.
2. Call this Microflow from a button on another page where a FileDocument object is created and has a provision to upload an input file.
3. Run your App locally and provide a file which is exactly like the sample file you had uploaded while creating this Data Importer document and trigger the Microflow.
4. You should see the data from the file being imported and shown on the page's Data Grid.
   
{{< figure src="/attachments/appstore/platform-supported-content/modules/data-importer-extension/data-imported-from-input-file.png" class="no-border" >}}


## Edit Data Importer Document

You can *Edit* the Data Importer Document, by uploading a new Sample file. This action will erase the existing Mapping or structure elements created for this document and will replace it with new mapping / structure elements.

### Upload a new file {#edit-DI-document-using-new-file}

To Edit the Data Importer Document,
1. Double-click the DI document that you want to *Edit*, it will open it in *read-only* mode.
2. Click on the **Update File** button in the top right corner and it will inform you that when a new file is uploaded and changes are saved, existing mapping/structure elements will be erased and will be replaced by new mapping/structure.

{{< figure src="/attachments/appstore/platform-supported-content/modules/data-importer-extension/update-data-importer-doc-confirmation.png" class="no-border" >}}

4. Click **Update** on the pop-up and then upload the new file. Change the configuration like *Sheet Name*, *Header Row* etc.
5. Clck on **Create Structure** if you find everything in order and the DI Document is updated.

{{< figure src="/attachments/appstore/platform-supported-content/modules/data-importer-extension/data-importer-doc-updated.png" class="no-border" >}}

Similar steps can be followed to update the DI document which was created with *Implicit Mapping* in first section.

You can now update the Domain Model entities, MFs, pages and any other downstream documents used or referenced by this Data Importer document to reflect these changes in your App. 

## Known Issues

### Unchecked Columns

It is not possible to rename an attribute or change a data type if there are unchecked columns. To avoid this issue, format your Excel or CSV file in a way that does not require you to uncheck any columns after inputting to Studio Pro. 
