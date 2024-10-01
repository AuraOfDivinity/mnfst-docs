---
id: upload
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

# File and Image upload

Manifest comes with a built-in local storage for uploaded assets. [File upload](#upload-a-file) lets you update pretty much anything whereas [image upload](#upload-an-image) comes with a simple resize system.

A `public/storage` folder is automatically created when needed. Uploaded files and images will be renamed with a unique name and stored in a specific folder based on entity and property name, ending by a folder with the current month name to prevent having to many files in a single folder.

Example: _public/storage/project/contract/Nov24/8dab3936m1p54a66-contract.pdf_

## Upload a file

A file should be related to an property with the [file property type](./properties.md#file).

<Tabs>
  <TabItem value="sdk" label="JS SDK" default>
    ```js

    // Create a Blob, adapt this step to your use case.
    const file = new Blob(['Hello, this is a test file!'], {
      type: 'text/plain',
    })

    // Upload a new invoice.contract file.
    const filePath = await manifest.from('invoices').upload('contract', file)

    console.log(filePath)
    // Output: {"path":"invoices/contract/Oct2024/8dabo9qm1q3swvu-my-contract.pdf"}

    ```

  </TabItem>
  <TabItem value="rest" label="REST API" default>
    ```http
    // Upload file.
    POST /api/upload/file
    Content-Type: multipart/form-data
    {
        file: (binary)
        entity: invoices
        property: contract
    }

    // Response.
    {
        "path":"invoices/contract/Oct2024/8dabo9qm1q3swvu-my-contract.pdf"
    }
    ```

  </TabItem>
</Tabs>

## Upload an image

An image should be related to a property with the [image property type](./properties.md#image). Manifest accepts **PNG** and **JPG** images only.

Each image uploaded will be optimized and resized into several sizes based on [your parameters](properties#parameters-1). By default it generates a -thumbnail- of 80x80 and a _medium_ of 160x160

<Tabs>
  <TabItem value="sdk" label="JS SDK" default>
    ```js

    // Create a Blob from an image, adapt this step to your use case.
    const base64Image =
      'iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAQAAAC1HAwCAAAAC0lEQVR42mP8/wcAAwAB/eb7jLwAAAAASUVORK5CYII='
    const image: Blob = base64ToBlob(base64Image, 'image/png')

    // Upload a new invoice.contract file.
    const image = await manifest.from('cats').uploadImage('avatar', image)

    console.log(image)
    // Output: {
    // "medium": "cats/avatar/Oct2024/8dabo9qm1q4n1nk-medium.jpg",
    // "thumbnail": "cats/avatar/Oct2024/8dabo9qm1q4n1nk-thumbnail.jpg"
    // }
    ```

  </TabItem>
  <TabItem value="rest" label="REST API" default>
    ```http
    // Upload image.
    POST /api/upload/image
    Content-Type: multipart/form-data
    {
        image: (binary)
        entity: invoices
        property: contract
    }

    // Response.
    {
     "medium": "cats/avatar/Oct2024/8dabo9qm1q4n1nk-medium.jpg",
     "thumbnail": "cats/avatar/Oct2024/8dabo9qm1q4n1nk-thumbnail.jpg"
    }
    ```

  </TabItem>
</Tabs>
