---
title: "File Uploader"
url: /appstore/modules/file-uploader/
description: "Describes the configuration and usage of the File Uploader module, which is available in the Mendix Marketplace."
#If moving or renaming this doc file, implement a temporary redirect and let the respective team know they should update the URL in the product. See Mapping to Products for more details. 
---

## Introduction

The [File Uploader](https://marketplace.mendix.com/link/component/235351) enables you to upload files or images by dragging and dropping them onto the widget or by using the file selection dialog. It supports multi-file uploading and includes an image-only mode that previews thumbnails of the uploaded images. The module comes with a domain model containing entities and nanoflows required for it to function properly.

{{< figure src="/attachments/appstore/platform-supported-content/modules/file-uploader/fileuploader.png" class="no-border" >}}

## Setup

{{% alert color="info" %}}
In the following sections, the "context entity" refers to the entity of the context object in which the File Uploader widget is placed. This can be either a data view or a page parameter.
{{% /alert %}}

### Simple Setup

For a simple setup, the context entity needs to be a specialization of the **FileUploader.FileUploadContext** entity. This allows you to use the nanoflows and entities provided with the module:
* **In your domain model**: 
    * Set the generalization of the context entity to **FileUploader.FileUploadContext**.
    
* **On your page**: 
    * Place the **FileUploader** widget.
    * Set property Associated files to **FileUploader.UploadedFile** or **FileUploader.UploadedImage**, depending on the upload mode.
    * Set property **Action to create new files** to one of the nanoflows shipped with the module, depending on the upload mode.

{{% alert color="info" %}}
Note: Starting with File Uploader v2.0, the widget properties will be set automatically.
{{% /alert %}}

### Advanced Setup

For an advanced setup, where modifying the context entity is not possible, follow these steps:
* **In your domain model**: 
    * Create a new specialized image/file entity by setting its generalization to **System.FileDocument** or **System.Image**, depending on the upload mode.
    * Create a **\*-1** association from this image/file entity to your context entity.

* **In the App explorer / Logic editor**:
    * Copy one of the two Nanoflows (depending on the upload mode) from the module and modify it to use your context entity and the newly created file/image entity.

* **On your page**: 
    * Place the **FileUploader** widget.
    * Set the widget property **Associated files** to the newly created file/image entity.
    * Set the widget property **Action to create new files** to the modified nanoflow.


### Widget Configuration

#### General Tab {#general}

##### Upload mode

* **Files** – Manually specify allowed file types.
* **Images** - Limits uploads to images and shows a preview thumbnail.

##### Associated Files / Associated Images

* Defines the entity which is used to store the files or images. Needs to be a specialization of **System.FileDocument** or **System.Image**.
* By default, use **FileUploader.UploadedFile** for upload mode Files and **FileUploader.UploadedImage** for upload mode Images.

{{% alert color="info" %}}
Note: Starting with File Uploader v2.0, the widget properties will be set automatically.
{{% /alert %}}

##### Action to Create New Files

* Configuration to setup the nanoflow which creates an associated file object and commits it.
* By default, call **Nanoflow ACT_CreateUploadedFileDocument** for upload mode Files and **ACT_CreateUploadedImageDocument** for upload mode Images.

{{% alert color="info" %}}
Note: Starting with File Uploader v2.0, the widget properties will be set automatically.
{{% /alert %}}

##### Action to Create New Files

This configuration only shown for upload mode **Files**. User can manually set restriction on the allowed file types.

##### Maximum Number of Files

This configuration is to sets a limit on how many files can be uploaded at once.

##### Maximum File Size (MB)

This configuration is to sets a maximum limit on the file size of uploaded files.

#### Text Tab {#general}

Configure the displayed text used in **File Uploader**.


### Nanoflows

This module includes two predefined nanoflows:

#### ACT_CreateUploadedFileDocument

This nanoflow will attaches an uploaded file to the context object.

#### ACT_CreateUploadedImageDocument

This nanoflow will attaches an uploaded image to the context object.





