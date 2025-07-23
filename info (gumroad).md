The Gumroad OAuth API is based around REST. We return JSON for every request, including errors.

To start using the API, you'll need to register your OAuth application. Note: The Verify License API endpoint does not require an OAuth application.

After creating an application, you'll be given a unique application id and application secret.

Authentication
On the application page, click Generate access token to get the token you will use with the API.

Scopes
We've provided five scopes that you may request when the user authorizes your application.

view_profile: read-only access to the user's public information and products.
edit_products: read/write access to the user's products and their variants, offer codes, and custom fields.
view_sales: read access to the user's products' sales information, including sales counts. This scope is also required in order to subscribe to the user's sales.
mark_sales_as_shipped: write access to mark the user's products' sales as shipped.
refund_sales: write access to refund the user's products' sales.
Resources
Create an OAuth application - A getting started guide for creating an application with our API.

omniauth-gumroad - (Ruby) an OmniAuth strategy for Gumroad OAuth.

More reading
If you're interested in learning more about OAuth, here are some links that might be useful:

OAuth 2 spec
Ruby OAuth2 library
Python OAuth2 library
PHP OAuth2 library
API Errors
Gumroad uses HTTP status codes to indicate the status of a request. Here's a run down on likely response codes.

200 OK everything worked as expected.
400 Bad Request you probably missed a required parameter.
401 Unauthorized you did not provide a valid access token.
402 Request Failed the parameters were valid but request failed.
404 Not Found the requested item doesn't exist.
500, 502, 503, 504 Server Error something else went wrong on our end.

To help you further, we provide a JSON object that goes more in-depth about the problem that led to the failed request. Errors responses from the api will follow the following format.

{
  "success": false,
  "message": "The product could not be found."
}
When present, the message will describe the particular problem and suggestions on what went wrong.

API Methods
Gumroad's OAuth 2.0 API lets you see information about your products, as well as you can add, edit, and delete offer codes, variants, and custom fields. Finally, you can see a user's public information and subscribe to be notified of their sales.

Products
GET /products

Retrieve all of the existing products for the authenticated user.

https://api.gumroad.com/v2/products
cURL example
curl https://api.gumroad.com/v2/products \
  -d "access_token=ACCESS_TOKEN" \
  -X GET
Example response:
{
  "success": true,
  "products": [{
    "custom_permalink": null,
    "custom_receipt": null,
    "custom_summary": "You'll get one PSD file.",
    "custom_fields": [],
    "customizable_price": null,
    "description": "I made this for fun.",
    "deleted": false,
    "max_purchase_count": null,
    "name": "Pencil Icon PSD",
    "preview_url": null,
    "require_shipping": false,
    "subscription_duration": null,
    "published": true,
    "url": "http://sahillavingia.com/pencil.psd",
    "id": "A-m3CDDC5dlrSdKZp0RFhA==",
    "price": 100,
    "purchasing_power_parity_prices": {
      "US": 100,
      "IN": 50,
      "EC": 25
    },
    "currency": "usd",
    "short_url": "https://sahil.gumroad.com/l/pencil",
    "thumbnail_url": "https://public-files.gumroad.com/variants/72iaezqqthnj1350mdc618namqki/f2f9c6fc18a80b8bafa38f3562360c0e42507f1c0052dcb708593f7efa3bdab8",
    "tags": ["pencil", "icon"],
    "formatted_price": "$1",
    "file_info": {},
    "sales_count": "0", # available with the 'view_sales' scope
    "sales_usd_cents": "0", # available with the 'view_sales' scope
    "is_tiered_membership": true,
    "recurrences": ["monthly"], # if is_tiered_membership is true, renders list of available subscription durations; otherwise null
    "variants": [
      {
        "title": "Tier",
        "options": [
          {
            "name": "First Tier",
            "price_difference": 0, # set for non-membership product options
            "purchasing_power_parity_prices": { # set for non-membership product options
              "US": 200,
              "IN": 100,
              "EC": 50
            },
            "is_pay_what_you_want": false,
            "recurrence_prices": { # present for membership products; otherwise null
              "monthly": {
                "price_cents": 300,
                "suggested_price_cents": null, # may return number if is_pay_what_you_want is true
                "purchasing_power_parity_prices": {
                  "US": 400,
                  "IN": 200,
                  "EC": 100
                }
              }
            }
          }
        ]
      }
    ]
  }, {...}, {...}]
}
GET /products/:id

Retrieve the details of a product.

https://api.gumroad.com/v2/products/:id
cURL example
curl https://api.gumroad.com/v2/products/A-m3CDDC5dlrSdKZp0RFhA== \
  -d "access_token=ACCESS_TOKEN" \
  -X GET
Example response:
{
  "success": true,
  "product": {
    "custom_permalink": null,
    "custom_receipt": null,
    "custom_summary": "You'll get one PSD file.",
    "custom_fields": [],
    "customizable_price": null,
    "description": "I made this for fun.",
    "deleted": false,
    "max_purchase_count": null,
    "name": "Pencil Icon PSD",
    "preview_url": null,
    "require_shipping": false,
    "subscription_duration": null,
    "published": true,
    "url": "http://sahillavingia.com/pencil.psd",
    "id": "A-m3CDDC5dlrSdKZp0RFhA==",
    "price": 100,
    "purchasing_power_parity_prices": {
      "US": 100,
      "IN": 50,
      "EC": 25
    },
    "currency": "usd",
    "short_url": "https://sahil.gumroad.com/l/pencil",
    "thumbnail_url": "https://public-files.gumroad.com/variants/72iaezqqthnj1350mdc618namqki/f2f9c6fc18a80b8bafa38f3562360c0e42507f1c0052dcb708593f7efa3bdab8",
    "tags": ["pencil", "icon"],
    "formatted_price": "$1",
    "file_info": {},
    "sales_count": "0", # available with the 'view_sales' scope
    "sales_usd_cents": "0", # available with the 'view_sales' scope
    "is_tiered_membership": true,
    "recurrences": ["monthly"], # if is_tiered_membership is true, renders list of available subscription durations; otherwise null
    "variants": [
      {
        "title": "Tier",
        "options": [
          {
            "name": "First Tier",
            "price_difference": 0, # set for non-membership product options
            "purchasing_power_parity_prices": { # set for non-membership product options
              "US": 200,
              "IN": 100,
              "EC": 50
            },
            "is_pay_what_you_want": false,
            "recurrence_prices": { # present for membership products; otherwise null
              "monthly": {
                "price_cents": 300,
                "suggested_price_cents": null, # may return number if is_pay_what_you_want is true
                "purchasing_power_parity_prices": {
                  "US": 400,
                  "IN": 200,
                  "EC": 100
                }
              }
            }
          }
        ]
      }
    ]
  }
}
DELETE /products/:id

Permanently delete a product.

https://api.gumroad.com/v2/products/:id
cURL example
curl https://api.gumroad.com/v2/products/A-m3CDDC5dlrSdKZp0RFhA== \
  -d "access_token=ACCESS_TOKEN" \
  -X DELETE
Example response:
{
  "success": true,
  "message": "The product has been deleted successfully."
}
PUT /products/:id/enable

Enable an existing product.

https://api.gumroad.com/v2/products/:id/enable
cURL example
curl https://api.gumroad.com/v2/products/A-m3CDDC5dlrSdKZp0RFhA==/enable \
  -d "access_token=ACCESS_TOKEN" \
  -X PUT
Example response:
{
  "success": true,
  "product": {
    "custom_permalink": null,
    "custom_receipt": null,
    "custom_summary": "You'll get one PSD file.",
    "custom_fields": [],
    "customizable_price": null,
    "description": "I made this for fun.",
    "deleted": false,
    "max_purchase_count": null,
    "name": "Pencil Icon PSD",
    "preview_url": null,
    "require_shipping": false,
    "subscription_duration": null,
    "published": true,
    "url": "http://sahillavingia.com/pencil.psd",
    "id": "A-m3CDDC5dlrSdKZp0RFhA==",
    "price": 100,
    "purchasing_power_parity_prices": {
      "US": 100,
      "IN": 50,
      "EC": 25
    },
    "currency": "usd",
    "short_url": "https://sahil.gumroad.com/l/pencil",
    "thumbnail_url": "https://public-files.gumroad.com/variants/72iaezqqthnj1350mdc618namqki/f2f9c6fc18a80b8bafa38f3562360c0e42507f1c0052dcb708593f7efa3bdab8",
    "tags": ["pencil", "icon"],
    "formatted_price": "$1",
    "file_info": {},
    "sales_count": "0", # available with the 'view_sales' scope
    "sales_usd_cents": "0", # available with the 'view_sales' scope
    "is_tiered_membership": true,
    "recurrences": ["monthly"], # if is_tiered_membership is true, renders list of available subscription durations; otherwise null
    "variants": [
      {
        "title": "Tier",
        "options": [
          {
            "name": "First Tier",
            "price_difference": 0, # set for non-membership product options
            "purchasing_power_parity_prices": { # set for non-membership product options
              "US": 200,
              "IN": 100,
              "EC": 50
            },
            "is_pay_what_you_want": false,
            "recurrence_prices": { # present for membership products; otherwise null
              "monthly": {
                "price_cents": 300,
                "suggested_price_cents": null, # may return number if is_pay_what_you_want is true
                "purchasing_power_parity_prices": {
                  "US": 400,
                  "IN": 200,
                  "EC": 100
                }
              }
            }
          }
        ]
      }
    ]
  }
}
PUT /products/:id/disable

Disable an existing product.

https://api.gumroad.com/v2/products/:id/disable
cURL example
curl https://api.gumroad.com/v2/products/A-m3CDDC5dlrSdKZp0RFhA==/disable \
  -d "access_token=ACCESS_TOKEN" \
  -X PUT
Example response:
{
  "success": true,
  "product": {
    "custom_permalink": null,
    "custom_receipt": null,
    "custom_summary": "You'll get one PSD file.",
    "custom_fields": [],
    "customizable_price": null,
    "description": "I made this for fun.",
    "deleted": false,
    "max_purchase_count": null,
    "name": "Pencil Icon PSD",
    "preview_url": null,
    "require_shipping": false,
    "subscription_duration": null,
    "published": false,
    "url": "http://sahillavingia.com/pencil.psd",
    "id": "A-m3CDDC5dlrSdKZp0RFhA==",
    "price": 100,
    "currency": "usd",
    "short_url": "https://sahil.gumroad.com/l/pencil",
    "thumbnail_url": "https://public-files.gumroad.com/variants/72iaezqqthnj1350mdc618namqki/f2f9c6fc18a80b8bafa38f3562360c0e42507f1c0052dcb708593f7efa3bdab8",
    "tags": ["pencil", "icon"],
    "formatted_price": "$1",
    "file_info": {},
    "view_count": "0", # available with the 'view_sales' scope
    "sales_count": "0", # available with the 'view_sales' scope
    "sales_usd_cents": "0", # available with the 'view_sales' scope
    "is_tiered_membership": true,
    "recurrences": ["monthly"], # if is_tiered_membership is true, renders list of available subscription durations; otherwise null
    "variants": [
      {
        "title": "Tier",
        "options": [
          {
            "name": "First Tier",
            "price_difference": 0, # set for non-membership product options
            "is_pay_what_you_want": false,
            "recurrence_prices": { # present for membership products; otherwise null
              "monthly": {
                "price_cents": 300,
                "suggested_price_cents": null # may return number if is_pay_what_you_want is true
              }
            }
          }
        ]
      }
    ]
  }
}
Variant categories
POST /products/:product_id/variant_categories

Create a new variant category on a product.

https://api.gumroad.com/v2/products/:product_id/variant_categories
Parameters:
variant_category
title
cURL example
curl https://api.gumroad.com/v2/products/A-m3CDDC5dlrSdKZp0RFhA==/variant_categories \
  -d "access_token=ACCESS_TOKEN" \
  -d "title=colors" \
  -X POST
Example response:
{
  "success": true,
  "variant_category": {
    "id": "mN7CdHiwHaR9FlxKvF-n-g==",
    "title": "colors"
  }
}
GET /products/:product_id/variant_categories/:id

Retrieve the details of a variant category of a product.

https://api.gumroad.com/v2/products/:product_id/variant_categories/:id
cURL example
curl https://api.gumroad.com/v2/products/A-m3CDDC5dlrSdKZp0RFhA==/variant_categories/mN7CdHiwHaR9FlxKvF-n-g== \
  -d "access_token=ACCESS_TOKEN" \
  -X GET
Example response:
{
  "success": true,
  "variant_category": {
    "id": "mN7CdHiwHaR9FlxKvF-n-g==",
    "title": "colors"
  }
}
PUT /products/:product_id/variant_categories/:id

Edit a variant category of an existing product.

https://api.gumroad.com/v2/products/:product_id/variant_categories/:id
Parameters:
variant_category
title
cURL example
curl https://api.gumroad.com/v2/products/A-m3CDDC5dlrSdKZp0RFhA==/variant_categories/mN7CdHiwHaR9FlxKvF-n-g== \
  -d "access_token=ACCESS_TOKEN" \
  -d "title=sizes" \
  -X PUT
Example response:
{
  "success": true,
  "variant_category": {
    "id": "mN7CdHiwHaR9FlxKvF-n-g==",
    "title": "colors"
  }
}
DELETE /products/:product_id/variant_categories/:id

Permanently delete a variant category of a product.

https://api.gumroad.com/v2/products/:product_id/variant_categories/:id
cURL example
curl https://api.gumroad.com/v2/products/A-m3CDDC5dlrSdKZp0RFhA==/variant_categories/mN7CdHiwHaR9FlxKvF-n-g== \
  -d "access_token=ACCESS_TOKEN" \
  -X DELETE
Example response:
{
  "success": true,
  "message": "The variant_category has been deleted successfully."
}
GET /products/:product_id/variant_categories

Retrieve all of the existing variant categories of a product.

https://api.gumroad.com/v2/products/:product_id/variant_categories
cURL example
curl https://api.gumroad.com/v2/products/A-m3CDDC5dlrSdKZp0RFhA==/variant_categories \
  -d "access_token=ACCESS_TOKEN" \
  -X GET
Example response:
{
  "success": true,
  "variant_categories": [{
    "id": "mN7CdHiwHaR9FlxKvF-n-g==",
    "title": "colors"
  }, {...}, {...}]
}
POST /products/:product_id/variant_categories/:variant_category_id/variants

Create a new variant of a product.

https://api.gumroad.com/v2/products/:product_id/variant_categories/:variant_category_id/variants
Parameters:
variant
name
price_difference_cents
max_purchase_count (optional)
cURL example
curl https://api.gumroad.com/v2/products/A-m3CDDC5dlrSdKZp0RFhA==/variant_categories/mN7CdHiwHaR9FlxKvF-n-g==/variants \
  -d "access_token=ACCESS_TOKEN" \
  -d "name=red" \
  -d "price_difference_cents=250"
Example response:
{
  "success": true,
  "variant": {
    "id": "l5C1XQfr2TG3WXcGY7YrUg==",
    "max_purchase_count": null,
    "name": "red",
    "price_difference_cents": 100
  }
}
GET /products/:product_id/variant_categories/:variant_category_id/variants/:id

Retrieve the details of a variant of a product.

https://api.gumroad.com/v2/products/:product_id/variant_categories/:variant_category_id/variants/:id
cURL example
curl https://api.gumroad.com/v2/products/A-m3CDDC5dlrSdKZp0RFhA==/variant_categories/mN7CdHiwHaR9FlxKvF-n-g==/variants/kuaXCPHTmRuoK13rNGVbxg== \
  -d "access_token=ACCESS_TOKEN" \
  -X GET
Example response:
{
  "success": true,
  "variant": {
    "id": "l5C1XQfr2TG3WXcGY7YrUg==",
    "max_purchase_count": null,
    "name": "red",
    "price_difference_cents": 100
  }
}
PUT /products/:product_id/variant_categories/:variant_category_id/variants/:id

Edit a variant of an existing product.

https://api.gumroad.com/v2/products/:product_id/variant_categories/:variant_category_id/variants/:id
Parameters:
variant
name
price_difference_cents
max_purchase_count (optional)
cURL example
curl https://api.gumroad.com/v2/products/A-m3CDDC5dlrSdKZp0RFhA==/variant_categories/mN7CdHiwHaR9FlxKvF-n-g==/variants/kuaXCPHTmRuoK13rNGVbxg== \
  -d "access_token=ACCESS_TOKEN" \
  -d "price_difference_cents=150" \
  -X PUT
Example response:
{
  "success": true,
  "variant": {
    "id": "l5C1XQfr2TG3WXcGY7YrUg==",
    "max_purchase_count": null,
    "name": "red",
    "price_difference_cents": 100
  }
}
DELETE /products/:product_id/variant_categories/:variant_category_id/variants/:id

Permanently delete a variant of a product.

https://api.gumroad.com/v2/products/:product_id/variant_categories/:variant_category_id/variants/:id
cURL example
curl https://api.gumroad.com/v2/products/A-m3CDDC5dlrSdKZp0RFhA==/variant_categories/mN7CdHiwHaR9FlxKvF-n-g==/variants/kuaXCPHTmRuoK13rNGVbxg== \
  -d "access_token=ACCESS_TOKEN" \
  -X DELETE
Example response:
{
  "success": true,
  "message": "The variant has been deleted successfully."
}
GET /products/:product_id/variant_categories/:variant_category_id/variants

Retrieve all of the existing variants in a variant category.

https://api.gumroad.com/v2/products/:product_id/variant_categories/:variant_category_id/variants
cURL example
curl https://api.gumroad.com/v2/products/A-m3CDDC5dlrSdKZp0RFhA==/variant_categories/mN7CdHiwHaR9FlxKvF-n-g==/variants \
  -d "access_token=ACCESS_TOKEN" \
  -X GET
Example response:
{
  "success": true,
  "variants": [{
    "id": "l5C1XQfr2TG3WXcGY7YrUg==",
    "max_purchase_count": null,
    "name": "red",
    "price_difference_cents": 100
  }, {...}, {...}]
}
Offer codes
GET /products/:product_id/offer_codes

Retrieve all of the existing offer codes for a product. Either amount_cents or percent_off will be returned depending if the offer code is a fixed amount off or a percentage off. A universal offer code is one that applies to all products.

https://api.gumroad.com/v2/products/:product_id/offer_codes
cURL example
curl https://api.gumroad.com/v2/products/A-m3CDDC5dlrSdKZp0RFhA==/offer_codes \
  -d "access_token=ACCESS_TOKEN" \
  -X GET
Example response:
{
  "success": true,
  "offer_codes": [{
    "id": "mN7CdHiwHaR9FlxKvF-n-g==",
    "name": "1OFF",
    "amount_cents": 100,
    "max_purchase_count": null,
    "universal": false,
    "times_used": 1
  }, {
    "id": "l5C1XQfr2TG3WXcGY7-r-g==",
    "name": "HALFOFF",
    "percent_off": 50,
    "max_purchase_count": null,
    "universal": false,
    "times_used": 1
  }, {...}, {...}]
}
GET /products/:product_id/offer_codes/:id

Retrieve the details of a specific offer code of a product

https://api.gumroad.com/v2/products/:product_id/offer_codes/:id
cURL example
curl https://api.gumroad.com/v2/products/A-m3CDDC5dlrSdKZp0RFhA==/offer_codes/bfi_30HLgGWL8H2wo_Gzlg== \
  -d "access_token=ACCESS_TOKEN" \
  -d "name=1OFF" \
  -d "amount_cents=100" \
  -X GET
Example response:
{
  "success": true,
  "offer_code": {
    "id": "mN7CdHiwHaR9FlxKvF-n-g==",
    "name": "1OFF",
    "amount_cents": 100,
    "max_purchase_count": null,
    "times_used": 1
  }
}
POST /products/:product_id/offer_codes

Create a new offer code for a product. Default offer code is in cents. A universal offer code is one that applies to all products.

https://api.gumroad.com/v2/products/:product_id/offer_codes
Parameters:
name (the coupon code used at checkout)
amount_off
offer_type (optional, "cents" or "percent") Default: "cents"
max_purchase_count (optional)
universal (optional, true or false) Default: false
cURL example
curl https://api.gumroad.com/v2/products/A-m3CDDC5dlrSdKZp0RFhA==/offer_codes \
  -d "access_token=ACCESS_TOKEN" \
  -d "name=1OFF" \
  -d "amount_off=100" \
  -d "offer_type=cents" \
  -X POST
Example response:
{
  "success": true,
  "offer_code": {
    "id": "mN7CdHiwHaR9FlxKvF-n-g==",
    "name": "1OFF",
    "amount_cents": 100,
    "max_purchase_count": null,
    "times_used": 1
  }
}
PUT /products/:product_id/offer_codes/:id

Edit an existing product's offer code.

https://api.gumroad.com/v2/products/:product_id/offer_codes/:id
Parameters:
offer_code
max_purchase_count
cURL example
curl https://api.gumroad.com/v2/products/A-m3CDDC5dlrSdKZp0RFhA==/offer_codes/bfi_30HLgGWL8H2wo_Gzlg== \
  -d "access_token=ACCESS_TOKEN" \
  -d "max_purchase_count=10" \
  -X PUT
Example response:
{
  "success": true,
  "offer_code": {
    "id": "mN7CdHiwHaR9FlxKvF-n-g==",
    "name": "1OFF",
    "amount_cents": 100,
    "max_purchase_count": 10,
    "universal": false
  }
}
DELETE /products/:product_id/offer_codes/:id

Permanently delete a product's offer code.

https://api.gumroad.com/v2/products/:product_id/offer_codes/:id
cURL example
curl https://api.gumroad.com/v2/products/A-m3CDDC5dlrSdKZp0RFhA==/offer_codes/bfi_30HLgGWL8H2wo_Gzlg== \
  -d "access_token=ACCESS_TOKEN" \
  -X DELETE
Example response:
{
  "success": true,
  "message": "The offer_code has been deleted successfully."
}
Custom fields
GET /products/:product_id/custom_fields

Retrieve all of the existing custom fields for a product.

https://api.gumroad.com/v2/products/:product_id/custom_fields
cURL example
curl https://api.gumroad.com/v2/products/A-m3CDDC5dlrSdKZp0RFhA==/custom_fields \
  -d "access_token=ACCESS_TOKEN" \
  -X GET
Example response:
{
  "success": true,
  "custom_fields": [{
    "name": "phone number",
    "required": "false"
  }, {...}, {...}]
}
POST /products/:product_id/custom_fields

Create a new custom field for a product.

https://api.gumroad.com/v2/products/:product_id/custom_fields
Parameters:
variant
name
required (true or false)
cURL example
curl https://api.gumroad.com/v2/products/A-m3CDDC5dlrSdKZp0RFhA==/custom_fields \
  -d "access_token=ACCESS_TOKEN" \
  -d "name=phone number" \
  -d "required=true" \
  -X POST
Example response:
  {
    "success": true,
    "custom_field": {
      "name": "phone number",
      "required": "false"
    }
  }
PUT /products/:product_id/custom_fields/:name

Edit an existing product's custom field.

https://api.gumroad.com/v2/products/:product_id/custom_fields/:name
Parameters:
variant
required (true or false)
cURL example
curl https://api.gumroad.com/v2/products/A-m3CDDC5dlrSdKZp0RFhA==/custom_fields/phone%20number \
  -d "access_token=ACCESS_TOKEN" \
  -d "required=false" \
  -d "name=phone number" \
  -X PUT
Example response:
  {
    "success": true,
    "custom_field": {
      "name": "phone number",
      "required": "false"
    }
  }
DELETE /products/:product_id/custom_fields/:name

Permanently delete a product's custom field.

https://api.gumroad.com/v2/products/:product_id/custom_fields/:name
cURL example
curl https://api.gumroad.com/v2/products/A-m3CDDC5dlrSdKZp0RFhA==/custom_fields/phone%20number \
  -d "access_token=ACCESS_TOKEN" \
  -X DELETE
Example response:
{
  "success": true,
  "message": "The custom_field has been deleted successfully."
}
User
GET /user

Retrieve the user's data.

https://api.gumroad.com/v2/user
cURL example
curl https://api.gumroad.com/v2/user \
  -d "access_token=ACCESS_TOKEN" \
  -X GET
Example response:
{
  "success": true,
  "user": {
    "bio": "a sailor, a tailor",
    "name": "John Smith",
    "twitter_handle": null,
    "user_id": "G_-mnBf9b1j9A7a4ub4nFQ==",
    "email": "johnsmith@gumroad.com", # available with the 'view_sales' scope
    "url": "https://gumroad.com/sailorjohn" # only if username is set
  }
}
Resource subscriptions
PUT /resource_subscriptions

Subscribe to a resource. Currently there are 8 supported resource names - "sale", "refund", "dispute", "dispute_won", "cancellation", "subscription_updated", "subscription_ended", and "subscription_restarted".

sale - When subscribed to this resource, you will be notified of the user's sales with an HTTP POST to your post_url. The format of the POST is described on the Gumroad Ping page.

refund - When subscribed to this resource, you will be notified of refunds to the user's sales with an HTTP POST to your post_url. The format of the POST is same as described on the Gumroad Ping page.

dispute - When subscribed to this resource, you will be notified of the disputes raised against user's sales with an HTTP POST to your post_url. The format of the POST is described on the Gumroad Ping page.

dispute_won - When subscribed to this resource, you will be notified of the sale disputes won by the user with an HTTP POST to your post_url. The format of the POST is described on the Gumroad Ping page.

cancellation - When subscribed to this resource, you will be notified of cancellations of the user's subscribers with an HTTP POST to your post_url.

subscription_updated - When subscribed to this resource, you will be notified when subscriptions to the user's products have been upgraded or downgraded with an HTTP POST to your post_url. A subscription is "upgraded" when the subscriber switches to an equally or more expensive tier and/or subscription duration. It is "downgraded" when the subscriber switches to a less expensive tier and/or subscription duration. In the case of a downgrade, this change will take effect at the end of the current billing period. (Note: This currently applies only to tiered membership products, not to all subscription products.)

subscription_ended - When subscribed to this resource, you will be notified when subscriptions to the user's products have ended with an HTTP POST to your post_url. These events include termination of a subscription due to: failed payment(s); cancellation; or a subscription of fixed duration ending. Notifications are sent at the time the subscription has officially ended, not, for example, at the time cancellation is requested.

subscription_restarted - When subscribed to this resource, you will be notified when subscriptions to the user's products have been restarted with an HTTP POST to your post_url. A subscription is "restarted" when the subscriber restarts their subscription after previously terminating it.

In each POST request, Gumroad sends these parameters:
subscription_id: id of the subscription
product_id: id of the product
product_name: name of the product
user_id: user id of the subscriber
user_email: email address of the subscriber
purchase_ids: array of charge ids belonging to this subscription
created_at: timestamp when subscription was created
charge_occurrence_count: number of charges made for this subscription
recurrence: subscription duration - monthly/quarterly/biannually/yearly/every_two_years
free_trial_ends_at: timestamp when free trial ends, if free trial is enabled for the membership
custom_fields: custom fields from the original purchase
license_key: license key from the original purchase

For "cancellation" resource:
cancelled: true if subscription has been cancelled, otherwise false
cancelled_at: timestamp at which subscription will be cancelled
cancelled_by_admin: true if subscription was been cancelled by admin, otherwise not present
cancelled_by_buyer: true if subscription was been cancelled by buyer, otherwise not present
cancelled_by_seller: true if subscription was been cancelled by seller, otherwise not present
cancelled_due_to_payment_failures: true if subscription was been cancelled automatically because of payment failure, otherwise not present

For "subscription_updated" resource:
type: "upgrade" or "downgrade"
effective_as_of: timestamp at which the change went or will go into effect
old_plan: tier, subscription duration, price, and quantity of the subscription before the change
new_plan: tier, subscription duration, price, and quantity of the subscription after the change

Example
{
  ...
  type: "upgrade",
  effective_as_of: "2021-02-23T16:31:44Z",
  old_plan: {
    tier: { id: "G_-mnBf9b1j9A7a4ub4nFQ==", name: "Basic tier" },
    recurrence: "monthly",
    price_cents: "1000",
    quantity: 1
  },
  new_plan: {
    tier: { id: "G_-mnBf9b1j9A7a4ub4nFQ==", name: "Basic tier" },
    recurrence: "yearly",
    price_cents: "12000",
    quantity: 2
  }
}
For "subscription_ended" resource:
ended_at: timestamp at which the subscription ended
ended_reason: the reason for the subscription ending ("cancelled", "failed_payment", or "fixed_subscription_period_ended")

For "subscription_restarted" resource:
restarted_at: timestamp at which the subscription was restarted

https://api.gumroad.com/v2/resource_subscriptions
cURL example
curl https://api.gumroad.com/v2/resource_subscriptions \
  -d "access_token=ACCESS_TOKEN" \
  -d "resource_name=sale" \
  -d "post_url=https://postatmebro.com" \
  -X PUT
Example response:
{
  "success": true,
  "resource_subscription": {
    "id": "G_-mnBf9b1j9A7a4ub4nFQ==",
    "resource_name": "sale",
    "post_url": "https://postatmebro.com"
  }
}
GET /resource_subscriptions

Show all active subscriptions of user for the input resource.

https://api.gumroad.com/v2/resource_subscriptions
Parameters:
resource_name (string) - Currently there are 8 supported values - "sale", "refund", "dispute", "dispute_won", "cancellation", "subscription_updated", "subscription_ended", and "subscription_restarted".
cURL example
curl https://api.gumroad.com/v2/resource_subscriptions \
  -d "access_token=ACCESS_TOKEN" \
  -d "resource_name=sale" \
  -X GET
Example response:
{
  "success": true,
  "resource_subscriptions": [{
    "id": "G_-mnBf9b1j9A7a4ub4nFQ==",
    "resource_name": "sale",
    "post_url": "https://postatmebro.com"
  }, {...}, {...}]
}
DELETE /resource_subscriptions/:resource_subscription_id

Unsubscribe from a resource.

https://api.gumroad.com/v2/resource_subscriptions/:resource_subscription_id
cURL example
curl https://api.gumroad.com/v2/resource_subscriptions/G_-mnBf9b1j9A7a4ub4nFQ== \
  -d "access_token=ACCESS_TOKEN" \
  -X DELETE
Example response:
{
  "success": true,
  "message": "The resource_subscription was deleted successfully."
}
Sales
GET /sales

Retrieves all of the successful sales by the authenticated user. Available with the 'view_sales' scope.

https://api.gumroad.com/v2/sales
Parameters:
after (optional, date in form YYYY-MM-DD) - Only return sales after this date
before (optional, date in form YYYY-MM-DD) - Only return sales before this date
product_id (optional) - Filter sales by this product
email (optional) - Filter sales by this email
order_id (optional) - Filter sales by this Order ID
page_key (optional) - A key representing a page of results. It is given in the response as `next_page_key`.
cURL example
curl https://api.gumroad.com/v2/sales \
  -d "access_token=ACCESS_TOKEN" \
  -d "before=2021-09-03" \
  -d "after=2020-09-03" \
  -d "product_id=bfi_30HLgGWL8H2wo_Gzlg==" \
  -d "email=calvin@gumroad.com" \
  -X GET
Example response:
{
  "success": true,
  "next_page_url": "/v2/sales?page_key=20230119081040000000-123456&before=2021-09-03&after=2020-09-03&email=calvin%40gumroad.com",
  "next_page_key": "20230119081040000000-123456",
  "sales": [
    {
      "id": "B28UKN-dvxYabdavG97Y-Q==",
      "email": "calvin@gumroad.com",
      "seller_id": "kL0paVL2SdmJSYsNs-OCMg==",
      "timestamp": "about 2 months ago",
      "daystamp": " 5 Jan 2021 11:38 AM",
      "created_at": "2021-01-05T19:38:56Z",
      "product_name": "Pencil Icon PSD",
      "product_has_variants": true,
      "price": 1000,
      "gumroad_fee": 60,
      "subscription_duration": "monthly",
      "formatted_display_price": "$10 a month",
      "formatted_total_price": "$10 a month",
      "currency_symbol": "$",
      "amount_refundable_in_currency": "0",
      "product_id": "32-nPainqpLj1B_WIwVlMw==",
      "product_permalink": "XCBbJ",
      "partially_refunded": false,
      "chargedback": false,
      "purchase_email": "calvin@gumroad.com",
      "zip_code": "625003",
      "paid": false,
      "has_variants": true,
      "variants": {
        "Tier": "Premium"
      },
      "variants_and_quantity": "(Premium)",
      "has_custom_fields": true,
      "custom_fields": {"Twitter handle": "@gumroad"},
      "order_id": 524459995,
      "is_product_physical": false,
      "purchaser_id": "5530311507811",
      "is_recurring_billing": true,
      "can_contact": true,
      "is_following": false,
      "disputed": false,
      "dispute_won": false,
      "is_additional_contribution": false,
      "discover_fee_charged": false,
      "is_gift_sender_purchase": false,
      "is_gift_receiver_purchase": false,
      "referrer": "https://www.facebook.com",
      "card": {
        "visual": null,
        "type": null
      },
      "product_rating": null,
      "reviews_count": 0,
      "average_rating": 0,
      "subscription_id": "GazW4_NBcQy-o7Gjjng7lw==",
      "cancelled": false,
      "ended": false,
      "recurring_charge": false,
      "license_key": "83DB262A-C19D3B06-A5235A6B-8C079166",
      "license_id": "bEtKQ3Zu9SgNopem0-ZywA==",
      "license_disabled": false,
      "affiliate": {
        "email": "affiliate@example.com",
        "amount": "$2.50"
      },
      "quantity": 1
    }, {...}, {...}
  ]
}
GET /sales/:id

Retrieves the details of a sale by this user. Available with the 'view_sales' scope.

https://api.gumroad.com/v2/sales/:id
cURL example
curl https://api.gumroad.com/v2/sales/FO8TXN-dvxYabdavG97Y-Q== \
  -d "access_token=ACCESS_TOKEN" \
  -X GET
Example response:
{
  "success": true,
  "sale": {
    "id": "FO8TXN-dvxYabdavG97Y-Q==",
    "email": "calvin@gumroad.com",
    "seller_id": "kL0paVL2SdmJSYsNs-OCMg==",
    "timestamp": "about 2 months ago",
    "daystamp": " 5 Jan 2021 11:38 AM",
    "created_at": "2021-01-05T19:38:56Z",
    "product_name": "Pencil Icon PSD",
    "product_has_variants": true,
    "price": 1000,
    "gumroad_fee": 60,
    "subscription_duration": "monthly",
    "formatted_display_price": "$10 a month",
    "formatted_total_price": "$10 a month",
    "currency_symbol": "$",
    "amount_refundable_in_currency": "0",
    "product_id": "32-nPainqpLj1B_WIwVlMw==",
    "product_permalink": "XCBbJ",
    "partially_refunded": false,
    "chargedback": false,
    "purchase_email": "calvin@gumroad.com",
    "zip_code": "625003",
    "paid": false,
    "has_variants": true,
    "variants": {
      "Tier": "Premium"
    },
    "variants_and_quantity": "(Premium)",
    "has_custom_fields": false,
    "custom_fields": {},
    "order_id": 524459995,
    "is_product_physical": false,
    "purchaser_id": "5530311507811",
    "is_recurring_billing": true,
    "can_contact": true,
    "is_following": false,
    "disputed": false,
    "dispute_won": false,
    "is_additional_contribution": false,
    "discover_fee_charged": false,
    "is_gift_sender_purchase": false,
    "is_gift_receiver_purchase": false,
    "referrer": "direct",
    "card": {
      "visual": null,
      "type": null
    },
    "product_rating": null,
    "reviews_count": 0,
    "average_rating": 0,
    "subscription_id": "GazW4_NBcQy-o7Gjjng7lw==",
    "cancelled": false,
    "ended": false,
    "recurring_charge": false,
    "license_key": "83DB262A-C19D3B06-A5235A6B-8C079166",
    "license_id": "bEtKQ3Zu9SgNopem0-ZywA==",
    "license_disabled": false,
    "affiliate": {
      "email": "affiliate@example.com",
      "amount": "$2.50"
    },
    "offer_code": {
      "name": "FLAT50",
      "displayed_amount_off": "50%"
    }
    "quantity": 1
  }
}
PUT /sales/:id/mark_as_shipped

Marks a sale as shipped. Available with the 'mark_sales_as_shipped' scope.

https://api.gumroad.com/v2/sales/:id/mark_as_shipped
Parameters:
tracking_url (optional)
cURL example
curl https://api.gumroad.com/v2/sales/A-m3CDDC5dlrSdKZp0RFhA==/mark_as_shipped \
  -d "access_token=ACCESS_TOKEN" \
  -d "tracking_url=https://www.shippingcompany.com/track/t123" \
  -X PUT
Example response:
{
  "success": true,
  "sale": {
    "id": "A-m3CDDC5dlrSdKZp0RFhA==",
    "email": "calvin@gumroad.com",
    "seller_id": "RkCCaDkPPciPd9155vcaJg==",
    "timestamp": "about 1 month ago",
    "daystamp": "23 Jan 2021 12:23 PM",
    "created_at": "2021-01-23T20:23:21Z",
    "product_name": "classic physical product",
    "product_has_variants": true,
    "price": 2200,
    "gumroad_fee": 217,
    "formatted_display_price": "$22",
    "formatted_total_price": "$22",
    "currency_symbol": "$",
    "amount_refundable_in_currency": "22",
    "product_id": "CCQadnagaqfmKxdHaG5AKQ==",
    "product_permalink": "KHc",
    "refunded": false,
    "partially_refunded": false,
    "chargedback": false,
    "purchase_email": "calvin@gumroad.com",
    "full_name": "Sample Name",
    "street_address": "Sample street",
    "city": "New York",
    "state": "NY",
    "zip_code": "10001",
    "country": "United States",
    "country_iso2": "US",
    "paid": true,
    "has_variants": true,
    "variants": {
      "Format": "Premium"
    },
    "variants_and_quantity": "(Premium)",
    "has_custom_fields": false,
    "custom_fields": {},
    "order_id": 292372715,
    "is_product_physical": true,
    "purchaser_id": "6225273416381",
    "is_recurring_billing": false,
    "can_contact": true,
    "is_following": false,
    "disputed": false,
    "dispute_won": false,
    "is_additional_contribution": false,
    "discover_fee_charged": false,
    "is_gift_sender_purchase": false,
    "is_gift_receiver_purchase": false,
    "referrer": "direct",
    "card": {
      "visual": "**** **** **** 4242",
      "type": "visa"
    },
    "product_rating": null,
    "reviews_count": 0,
    "average_rating": 0,
    "shipped": true,
    "tracking_url": "https://www.shippingcompany.com/track/t123",
    "license_key": "740A36FE-80134D88-9998290C-1B30910C",
    "license_id": "mN7CdHiwHaR9FlxKvF-n-g==",
    "license_disabled": false,
    "sku_id": "6Oo2MGSSagZU5naeWaDaNQ==",
    "sku_external_id": "6Oo2MGSS1gaU5a5eWaDaNQ==",
    "affiliate": {
      "email": "affiliate@example.com",
      "amount": "$2.50"
    },
    "quantity": 1
  }
}
PUT /sales/:id/refund

Refunds a sale. Available with the 'refund_sales' scope.

https://api.gumroad.com/v2/sales/:id/refund
Parameters:
amount_cents (optional) - Amount in cents (in currency of the sale) to be refunded. If set, issue partial refund by this amount. If not set, issue full refund. You can issue multiple partial refunds per sale until it is fully refunded.
cURL example
curl https://api.gumroad.com/v2/sales/A-m3CDDC5dlrSdKZp0RFhA==/refund \
  -d "access_token=ACCESS_TOKEN" \
  -d "amount_cents=200" \
  -X PUT
Example response:
{
  "success": true,
  "sale": {
    "id": "A-m3CDDC5dlrSdKZp0RFhA==",
    "email": "calvin@gumroad.com",
    "seller_id": "RkCCODaPPciPd9155vcQJg==",
    "timestamp": "about 1 month ago",
    "daystamp": "23 Jan 2021 10:24 AM",
    "created_at": "2021-01-23T18:24:07Z",
    "product_name": "Pencil Icon PSD",
    "product_has_variants": false,
    "price": 1000,
    "gumroad_fee": 115,
    "formatted_display_price": "$10",
    "formatted_total_price": "$10",
    "currency_symbol": "$",
    "amount_refundable_in_currency": "8",
    "product_id": "e7xqFa2WL0E-qJlQ4WYJxA==",
    "product_permalink": "RSE",
    "refunded": false,
    "partially_refunded": true,
    "chargedback": false,
    "purchase_email": "calvin@gumroad.com",
    "street_address": "",
    "city": "",
    "state": "AA",
    "zip_code": "67600",
    "paid": true,
    "has_variants": false,
    "variants_and_quantity": "",
    "has_custom_fields": false,
    "custom_fields": {},
    "order_id": 343932147,
    "is_product_physical": false,
    "is_recurring_billing": false,
    "can_contact": true,
    "is_following": false,
    "disputed": false,
    "dispute_won": false,
    "is_additional_contribution": false,
    "discover_fee_charged": false,
    "is_gift_sender_purchase": false,
    "is_gift_receiver_purchase": false,
    "referrer": "direct",
    "card": {
      "visual": "**** **** **** 4242",
      "type": "visa"
    },
    "product_rating": null,
    "reviews_count": 0,
    "average_rating": 0,
    "affiliate": {
      "email": "affiliate@example.com",
      "amount": "$2.50"
    },
    "quantity": 1
  }
}
Subscribers
GET /products/:product_id/subscribers

Retrieves all of the active subscribers for one of the authenticated user's products. Available with the 'view_sales' scope

A subscription is terminated if any of failed_at, ended_at, or cancelled_at timestamps are populated and are in the past.

A subscription's status can be one of: alive, pending_cancellation, pending_failure, failed_payment, fixed_subscription_period_ended, cancelled.

https://api.gumroad.com/v2/products/:product_id/subscribers
Parameters:
email (optional) - Filter subscribers by this email
paginated (optional, default: "false") - Set to "true" to limit the number of subscribers returned to 100.
page_key (optional) - A key representing a page of results. It is given in the paginated response of the previous page as `next_page_key`.
cURL example
curl https://api.gumroad.com/v2/products/0ssD7adjRklGBjS5cwlWPq==/subscribers \
  -d "access_token=ACCESS_TOKEN" \
  -d "paginated=true" \
  -d "email=calvin@gumroad.com" \
  -X GET
Example response:
{
  "success":true,
  "next_page_url": "/v2/products/0ssD7adjRklGBjS5cwlWPq==/subscribers?page_key=20241004235318372406-857093235&email=calvin%40gumroad.com",
  "next_page_key": "20241004235318372406-857093235",
  "subscribers": [{
    "id": "P5ppE6H8XIjy2JSCgUhbAw==",
    "product_id": "0ssD7adjRklGBjS5cwlWPq==",
    "product_name":"Pencil Icon PSD",
    "user_id": "3523953790232",
    "user_email":"calvin@gumroad.com",
    "purchase_ids": [O4pjE6H8XNjy2JSCgKhbAw==],
    "created_at": "2015-06-30T17:38:04Z",
    "user_requested_cancellation_at": null,
    "charge_occurrence_count": null,
    "recurrence": "monthly",
    "cancelled_at": null,
    "ended_at": null,
    "failed_at": null,
    "free_trial_ends_at": null,
    "license_key": "85DB562A-C11D4B06-A2335A6B-8C079166",
    "status": "alive"
  }]
}
GET /subscribers/:id

Retrieves the details of a subscriber to this user's product. Available with the 'view_sales' scope.

https://api.gumroad.com/v2/subscribers/:id
cURL example
curl https://api.gumroad.com/v2/subscribers/P5ppE6H8XIjy2JSCgUhbAw== \
  -d "access_token=ACCESS_TOKEN" \
  -X GET
Example response:
{
  "success":true,
  "subscribers": {
    "id": "P5ppE6H8XIjy2JSCgUhbAw==",
    "product_id": "0ssD7adjRklGBjS5cwlWPq==",
    "product_name":"Pencil Icon PSD",
    "user_id": "3523953790232",
    "user_email":"calvin@gumroad.com",
    "purchase_ids": [O4pjE6H8XNjy2JSCgKhbAw==],
    "created_at": "2015-06-30T17:38:04Z",
    "user_requested_cancellation_at": null,
    "charge_occurrence_count": null,
    "recurrence": "monthly",
    "cancelled_at": null,
    "ended_at": null,
    "failed_at": null,
    "free_trial_ends_at": null,
    "license_key": "85DB562A-C11D4B06-A2335A6B-8C079166",
    "status": "alive"
  }
}
Licenses
POST /licenses/verify

Verify a license

https://api.gumroad.com/v2/licenses/verify
Parameters:
product_id (the unique ID of the product, available on product's edit page)
license_key (the license key provided by your customer)
increment_uses_count ("true"/"false", optional, default: "true")
cURL example
curl https://api.gumroad.com/v2/licenses/verify \
  -d "product_id=32-nPAicqbLj8B_WswVlMw==" \
  -d "license_key=YOUR_CUSTOMERS_LICENSE_KEY" \
  -X POST
Example response:
{
  "success": true,
  "uses": 3,
  "purchase": {
    "seller_id": "kL0psVL2admJSYRNs-OCMg==",
    "product_id": "32-nPAicqbLj8B_WswVlMw==",
    "product_name": "licenses demo product",
    "permalink": "QMGY",
    "product_permalink": "https://sahil.gumroad.com/l/pencil",
    "email": "customer@example.com",
    "price": 0,
    "gumroad_fee": 0,
    "currency": "usd",
    "quantity": 1,
    "discover_fee_charged": false,
    "can_contact": true,
    "referrer": "direct",
    "card": {
      "visual": null,
      "type": null
    },
    "order_number": 524459935,
    "sale_id": "FO8TXN-dbxYaBdahG97Y-Q==",
    "sale_timestamp": "2021-01-05T19:38:56Z",
    "purchaser_id": "5550321502811",
    "subscription_id": "GDzW4_aBdQc-o7Gbjng7lw==",
    "variants": "",
    "license_key": "85DB562A-C11D4B06-A2335A6B-8C079166",
    "is_multiseat_license": false,
    "ip_country": "United States",
    "recurrence": "monthly",
    "is_gift_receiver_purchase": false,
    "refunded": false,
    "disputed": false,
    "dispute_won": false,
    "id": "FO8TXN-dvaYbBbahG97a-Q==",
    "created_at": "2021-01-05T19:38:56Z",
    "custom_fields": [],
    "chargebacked": false, # purchase was refunded, non-subscription product only
    "subscription_ended_at": null, # subscription was ended, subscription product only
    "subscription_cancelled_at": null, # subscription was cancelled, subscription product only
    "subscription_failed_at": null # we were unable to charge the subscriber's card
  }
}
PUT /licenses/enable

Enable a license

https://api.gumroad.com/v2/licenses/enable
Parameters:
product_id (the unique ID of the product, available on product's edit page)
license_key (the license key provided by your customer)
cURL example
curl https://api.gumroad.com/v2/licenses/enable \
  -d "access_token=ACCESS_TOKEN" \
  -d "product_id=32-nPAicqbLj8B_WswVlMw==" \
  -d "license_key=YOUR_CUSTOMERS_LICENSE_KEY" \
  -X PUT
Example response:
{
  "success": true,
  "uses": 3,
  "purchase": {
    "seller_id": "kL0psVL2admJSYRNs-OCMg==",
    "product_id": "32-nPAicqbLj8B_WswVlMw==",
    "product_name": "licenses demo product",
    "permalink": "QMGY",
    "product_permalink": "https://sahil.gumroad.com/l/pencil",
    "email": "customer@example.com",
    "price": 0,
    "gumroad_fee": 0,
    "currency": "usd",
    "quantity": 1,
    "discover_fee_charged": false,
    "can_contact": true,
    "referrer": "direct",
    "card": {
      "visual": null,
      "type": null
    },
    "order_number": 524459935,
    "sale_id": "FO8TXN-dbxYaBdahG97Y-Q==",
    "sale_timestamp": "2021-01-05T19:38:56Z",
    "purchaser_id": "5550321502811",
    "subscription_id": "GDzW4_aBdQc-o7Gbjng7lw==",
    "variants": "",
    "license_key": "85DB562A-C11D4B06-A2335A6B-8C079166",
    "is_multiseat_license": false,
    "ip_country": "United States",
    "recurrence": "monthly",
    "is_gift_receiver_purchase": false,
    "refunded": false,
    "disputed": false,
    "dispute_won": false,
    "id": "FO8TXN-dvaYbBbahG97a-Q==",
    "created_at": "2021-01-05T19:38:56Z",
    "custom_fields": [],
    "chargebacked": false, # purchase was refunded, non-subscription product only
    "subscription_ended_at": null, # subscription was ended, subscription product only
    "subscription_cancelled_at": null, # subscription was cancelled, subscription product only
    "subscription_failed_at": null # we were unable to charge the subscriber's card
  }
}
PUT /licenses/disable

Disable a license

https://api.gumroad.com/v2/licenses/disable
Parameters:
product_id (the unique ID of the product, available on product's edit page)
license_key (the license key provided by your customer)
cURL example
curl https://api.gumroad.com/v2/licenses/disable \
  -d "access_token=ACCESS_TOKEN" \
  -d "product_id=32-nPAicqbLj8B_WswVlMw==" \
  -d "license_key=YOUR_CUSTOMERS_LICENSE_KEY" \
  -X PUT
Example response:
{
  "success": true,
  "uses": 3,
  "purchase": {
    "seller_id": "kL0psVL2admJSYRNs-OCMg==",
    "product_id": "32-nPAicqbLj8B_WswVlMw==",
    "product_name": "licenses demo product",
    "permalink": "QMGY",
    "product_permalink": "https://sahil.gumroad.com/l/pencil",
    "email": "customer@example.com",
    "price": 0,
    "gumroad_fee": 0,
    "currency": "usd",
    "quantity": 1,
    "discover_fee_charged": false,
    "can_contact": true,
    "referrer": "direct",
    "card": {
      "visual": null,
      "type": null
    },
    "order_number": 524459935,
    "sale_id": "FO8TXN-dbxYaBdahG97Y-Q==",
    "sale_timestamp": "2021-01-05T19:38:56Z",
    "purchaser_id": "5550321502811",
    "subscription_id": "GDzW4_aBdQc-o7Gbjng7lw==",
    "variants": "",
    "license_key": "85DB562A-C11D4B06-A2335A6B-8C079166",
    "is_multiseat_license": false,
    "ip_country": "United States",
    "recurrence": "monthly",
    "is_gift_receiver_purchase": false,
    "refunded": false,
    "disputed": false,
    "dispute_won": false,
    "id": "FO8TXN-dvaYbBbahG97a-Q==",
    "created_at": "2021-01-05T19:38:56Z",
    "custom_fields": [],
    "chargebacked": false, # purchase was refunded, non-subscription product only
    "subscription_ended_at": null, # subscription was ended, subscription product only
    "subscription_cancelled_at": null, # subscription was cancelled, subscription product only
    "subscription_failed_at": null # we were unable to charge the subscriber's card
  }
}
PUT /licenses/decrement_uses_count

Decrement the uses count of a license

https://api.gumroad.com/v2/licenses/decrement_uses_count
Parameters:
product_id (the unique ID of the product, available on product's edit page)
license_key (the license key provided by your customer)
cURL example
curl https://api.gumroad.com/v2/licenses/decrement_uses_count \
  -d "access_token=ACCESS_TOKEN" \
  -d "product_id=32-nPAicqbLj8B_WswVlMw==" \
  -d "license_key=YOUR_CUSTOMERS_LICENSE_KEY" \
  -X PUT
Example response:
{
  "success": true,
  "uses": 3,
  "purchase": {
    "seller_id": "kL0psVL2admJSYRNs-OCMg==",
    "product_id": "32-nPAicqbLj8B_WswVlMw==",
    "product_name": "licenses demo product",
    "permalink": "QMGY",
    "product_permalink": "https://sahil.gumroad.com/l/pencil",
    "email": "customer@example.com",
    "price": 0,
    "gumroad_fee": 0,
    "currency": "usd",
    "quantity": 1,
    "discover_fee_charged": false,
    "can_contact": true,
    "referrer": "direct",
    "card": {
      "visual": null,
      "type": null
    },
    "order_number": 524459935,
    "sale_id": "FO8TXN-dbxYaBdahG97Y-Q==",
    "sale_timestamp": "2021-01-05T19:38:56Z",
    "purchaser_id": "5550321502811",
    "subscription_id": "GDzW4_aBdQc-o7Gbjng7lw==",
    "variants": "",
    "license_key": "85DB562A-C11D4B06-A2335A6B-8C079166",
    "is_multiseat_license": false,
    "ip_country": "United States",
    "recurrence": "monthly",
    "is_gift_receiver_purchase": false,
    "refunded": false,
    "disputed": false,
    "dispute_won": false,
    "id": "FO8TXN-dvaYbBbahG97a-Q==",
    "created_at": "2021-01-05T19:38:56Z",
    "custom_fields": [],
    "chargebacked": false, # purchase was refunded, non-subscription product only
    "subscription_ended_at": null, # subscription was ended, subscription product only
    "subscription_cancelled_at": null, # subscription was cancelled, subscription product only
    "subscription_failed_at": null # we were unable to charge the subscriber's card
  }
}
