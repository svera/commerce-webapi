---
title: WishlistItemInterface attributes and implementations | Commerce Web APIs
---

# WishlistItemInterface attributes and implementations

`WishlistItemInterface` provides details about items in a wish list. It has the following implementations:

*  [`BundleWishlistItem`](#bundlewishlistitem-attributes)
*  [`ConfigurableWishlistItem`](#configurablewishlistitem-attributes)
*  [`DownloadableWishlistItem`](#downloadablewishlistitem-attributes)
*  [`GiftCardWishlistItem`](#giftcardwishlistitem-attributes)
*  [`GroupedProductWishlistItem`](#groupedproductwishlistitem-attributes)
*  [`SimpleWishlistItem`](#simplewishlistitem-attributes)
*  [`VirtualWishlistItem`](#virtualwishlistitem-attributes)

## Attributes

import WishlistItemInterface from '/src/pages/_includes/graphql/wishlist-item-interface.md'

<WishlistItemInterface />

## Implementations

### BundleWishlistItem attributes

The `BundleWishlistItem` object defines the following bundle-product specific attribute:

Attribute | Data type | Description
--- | --- | ---
`bundle_options` | [SelectedBundleOption!]| An array containing information about the selected bundle items

### ConfigurableWishlistItem attributes

The `ConfigurableWishlistItem` object defines the following attributes that are specific to configurable products:

Attribute | Data type | Description
--- | --- | ---
`child_sku` | String! | Deprecated. Use `configured_variant` instead. The SKU of the simple product corresponding to a set of selected configurable options
`configurable_options` | [SelectedConfigurableOption!] | An array of selected configurable options
`configured_variant` | [ProductInterface](../../products/interfaces/index.md) | Returns details about the selected variant. The value is null if some options are not configured

### DownloadableWishlistItem attributes

The `DownloadableWishlistItem` object defines the following attributes that are specific to downloadable products:

Attribute | Data type | Description
--- | --- | ---
`links_v2` | [DownloadableProductLinks] | An array containing information about the selected links
`samples` | [DownloadableProductSamples] | An array containing information about the selected samples

### GiftCardWishlistItem attributes

The `GiftCardWishlistItem` object defines the following gift card-specific attribute:

Attribute | Data type | Description
--- | --- | ---
`gift_card_options` | GiftCardOptions! | Contains details about a gift card product

#### GiftCardOptions attributes

The GiftCardOptions object provides details about a gift card. All attributes are optional for a wish list.

Attribute | Data type | Description
--- | --- | ---
`amount` | Money | The amount and currency of the gift card
`custom_giftcard_amount` | Money | The custom amount and currency of the gift card
`message` | String | A message to the recipient
`recipient_email` | String | The email of the person receiving the gift card
`recipient_name` | String | The name of the person receiving the gift card
`sender_email` | String | The email of the sender
`sender_name` | String | The name of the sender

### GroupedProductWishlistItem attributes

The GroupedProductWishlistItem data type does not provide additional attributes to the `WishlistItemInterface`.

### SimpleWishlistItem attributes

The SimpleWishlistItem data type does not provide additional attributes to the `WishlistItemInterface`.

### VirtualWishlistItem attributes

The VirtualWishlistItem data type does not provide additional attributes to the `WishlistItemInterface`.

## Example usage

The following mutation adds a downloadable product to a wish list and returns detailed information about the product.

**Request:**

```graphql
mutation {
  addProductsToWishlist(
    wishlistId: 3,
    wishlistItems: [
      {
        sku: "240-LV06"
        quantity: 1
      }
    ]
) {
    user_errors {
      code
      message
    }
    wishlist {
      id
      sharing_code
      items_count
      updated_at
      items_v2 {
        uid
        quantity
        added_at
        ... on DownloadableWishlistItem {
          links_v2 {
            uid
            title
            sample_url
          }
          samples {
            title
            sample_url
          }
        }
      }
    }
  }
}
```

**Response:**

```json
{
  "data": {
    "addProductsToWishlist": {
      "user_errors": [],
      "wishlist": {
        "id": "3",
        "sharing_code": "cVeOljNmNKm0e1jRFj7PRMyBXrpu88Sf",
        "items_count": 1,
        "updated_at": "2020-11-12 03:01:46",
        "items_v2": [
          {
            "uid": "MTQ=",
            "quantity": 1,
            "added_at": "2020-11-12 03:01:46",
            "links_v2": [
              {
                "uid": "ZG93bmxvYWRhYmxlLzM=",
                "title": "Yoga Adventure",
                "sample_url": "http://example.com/downloadable/download/linkSample/link_id/3/"
              }
            ],
            "samples": [
              {
                "title": "Trailer #1",
                "sample_url": "http://example.com/downloadable/download/sample/sample_id/7/"
              },
              {
                "title": "Trailer #2",
                "sample_url": "http://example.com/downloadable/download/sample/sample_id/8/"
              },
              {
                "title": "Trailer #3",
                "sample_url": "http://example.com/downloadable/download/sample/sample_id/9/"
              }
            ]
          }
        ]
      }
    }
  }
}
```
