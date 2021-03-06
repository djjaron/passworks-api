Assets
===============

Assets are the visual elements of the pass.

Passworks supports 6 types of assets (`icon`, `logo`, `strip`, `thumbnail`, `background` and `footer`) each type of pass uses one or more types of assets.

You can reuse assets, meaning that you can assign the same asset to multiple passes, reducing the number of asset operations and also reducing the amount of bandwidth required to create a pass.

Types of assets
----------------

Each pass type (`store card`, `event ticket`, `boarding pass`, `generic pass` and `coupons`) use specific set of assets/images.

| Asset Name |  Used in       | Size
|------------|----------------------------------|----------------------------------|
| icon       | All type of passes               | 118x118 |
| logo		   | All type of passes 		        | 320x100 
| strip	   | Coupon, Event Ticket, Store Card | 640x84 for Event Tickets or 640x246 for other types of passes 
| thumbnail  | Event ticket, Generic | 180x180 The aspect ratio should be in the range of 2:3 to 3:2, otherwise the image is cropped.
| background | Event Ticket | 360x440	        |
| footer	   | Boarding Pass | 286x30           |    

Preferred image format is .png since it supports transparencies.

Maximum upload size is 1 Megabyte (1024 Kb)

Listing the assets
----------------

```shell
GET /v1/assets
```

Response:

```json
[
  {
    "id": "aab705bb-5bf0-4c14-93e0-f5fe01a4d3f5",
    "asset_type": "strip",
    "created_at": "2014-08-25T14:52:49Z"
  },
  {
    "id": "cecd7470-2ba8-4737-afe0-30d0cd4fd00c",
    "asset_type": "icon",
    "created_at": "2014-09-24T09:38:48Z"
  },
  {
    "id": "eb66127b-cbb7-41f4-b7ea-a6b8c106fead",
    "asset_type": "logo",
    "created_at": "2014-09-24T09:39:40Z"
  }
]
```

Creating a asset
----------------

Passworks API allows you to upload images in the `.jpg`, `.png` and `.gif` format, note that all the uploads images will be reprocessed and converted to into `png`.

Images must be uploaded using `multipart` upload.


```shell
POST /v1/assets
```

Body payload:

```json
	asset[file]       = <multipart upload>
	asset[asset_type] = 'background'
```


| Field Name           | Type      | Description    |
|----------------------|-----------|----------------|
| file   			   | string    |  Required. The uploaded filename (eg: company-logo.png)
| asset_type    		   | string    | Required. The type of asset being uploaded. Valid asset types are `icon`, `logo`, `strip `, `thumbnail`, `background`, `footer`

Response:


```json
{
  "id": "32f85671-2e12-4a46-9fe2-05435cd5fc14",
  "asset_type": "background",
  "created_at": "2014-09-24T17:41:27Z"
}
```

Example of a multipart upload using [curl](http://en.wikipedia.org/wiki/CURL#cURL):

```shell
curl -u <api username>:<api key> -v -H 'Content-Type: multipart/form-data' -H 'Accept: application/json' -F "asset[asset_type]=background" -F "asset[file]=@/images-folder/my-background.png" http://api.passworks.io/v1/assets
```

Deleting a asset
----------------

```shell
DELETE /v1/assets/{asset-id}
```

If the asset is deleted successfully the endpoint returns a HTTP 200.

In case an asset can't be deleted because it's being used by an active "campaign" an HTTP 412 error code is returned. 
