---
layout: tutorial
group: rest
title: Step 11. Prepare for checkout
menu_title: Step 11. Prepare for checkout
menu_order: 110
level3_subgroup: msi-tutorial
return_to:
  title: REST Tutorials
  url: rest/tutorials/index.html
version: 2.3
github_link: rest/tutorials/msi-order-processing/11-prepare-for-checkout
  - Integration
---

Now that all the items have been added to the cart, we can prepare the order for {% glossarytooltip 278c3ce0-cd4c-4ffc-a098-695d94d73bde %}checkout{% endglossarytooltip %}. This process includes the following steps:

* Estimate shipping costs
* Set shipping and billing information

### Estimate shipping costs {#estimate-shipping}

Magento calculates shipping costs for each shipping method that can be applied to the order. In this tutorial, the `flatrate` ($5 per item) shipping method is active.

**Endpoint**

`POST http://<host>/rest/uk/V1/carts/mine/estimate-shipping-methods`

**Scope**

`uk` store view

**Headers**

`Content-Type` `application/json`

`Authorization` `Bearer <customer token>`

**Payload**

The payload contains the shipping address.

{% highlight json %}
{  "address": {
      "region": "New York",
      "region_id": 43,
      "region_code": "NY",
      "country_id": "US",
      "street": [
        "123 Oak Ave"
        ],
      "postcode": "10577",
      "city": "Purchase",
      "firstname": "Jane",
      "lastname": "Doe",
      "customer_id": 4,
      "email": "jdoe@example.com",
      "telephone": "(512) 555-1111",
      "same_as_billing": 1
  }
}
{% endhighlight %}

**Response**

The cost for the `flatrate` shipping method is $5. The virtual item does not have a shipping charge because it is not a physical product.

``` json
[
    {
        "carrier_code": "flatrate",
        "method_code": "flatrate",
        "carrier_title": "Flat Rate",
        "method_title": "Fixed",
        "amount": 5,
        "base_amount": 5,
        "available": true,
        "error_message": "",
        "price_excl_tax": 5,
        "price_incl_tax": 5
    }
]
```

### Set shipping and billing information {#set-addresses}

In this call, you specify the shipping and billing addresses, as well as the selected `carrier_code` and `method_code`. The customer has selected the Flat Rate shipping method.

Magento returns a list of payment options and calculates the order totals.

**Endpoint**

`POST http://<host>/rest/uk/V1/carts/mine/shipping-information`

**Scope**

`uk` store view

**Headers**

`Content-Type` `application/json`

`Authorization` `Bearer <customer token>`

**Payload**

``` json
{  "addressInformation": {
	  "shipping_address": {
       "region": "New York",
       "region_id": 43,
       "region_code": "NY",
       "country_id": "US",
       "street": [
         "123 Oak Ave"
       ],
       "postcode": "10577",
       "city": "Purchase",
       "firstname": "Jane",
       "lastname": "Doe",
       "email": "jdoe@example.com",
       "telephone": "512-555-1111"
  },
  "billing_address": {
  	"region": "New York",
    "region_id": 43,
    "region_code": "NY",
    "country_id": "US",
    "street": [
      "123 Oak Ave"
    ],
    "postcode": "10577",
    "city": "Purchase",
    "firstname": "Jane",
    "lastname": "Doe",
    "email": "jdoe@example.com",
    "telephone": "512-555-1111"
  },
  "shipping_carrier_code": "flatrate",
  "shipping_method_code": "flatrate"
  }
}
```

**Response**

The subtotal of the order is $170, and shipping charges are $5. The grand total is $175.

The available payment methods are `banktransfer` and `checkmo`. The customer will specify a {% glossarytooltip 422b0fa8-b181-4c7c-93a2-c553abb34efd %}payment method{% endglossarytooltip %} in the next step.


``` json
{
    "payment_methods": [
        {
            "code": "banktransfer",
            "title": "Bank Transfer Payment"
        },
        {
            "code": "checkmo",
            "title": "Check / Money order"
        }
    ],
    "totals": {
        "grand_total": 175,
        "base_grand_total": 175,
        "subtotal": 170,
        "base_subtotal": 170,
        "discount_amount": 0,
        "base_discount_amount": 0,
        "subtotal_with_discount": 170,
        "base_subtotal_with_discount": 170,
        "shipping_amount": 5,
        "base_shipping_amount": 5,
        "shipping_discount_amount": 0,
        "base_shipping_discount_amount": 0,
        "tax_amount": 0,
        "base_tax_amount": 0,
        "weee_tax_applied_amount": null,
        "shipping_tax_amount": 0,
        "base_shipping_tax_amount": 0,
        "subtotal_incl_tax": 170,
        "shipping_incl_tax": 5,
        "base_shipping_incl_tax": 5,
        "base_currency_code": "USD",
        "quote_currency_code": "USD",
        "items_qty": 2,
        "items": [
            {
                "item_id": 1,
                "price": 70,
                "base_price": 70,
                "qty": 1,
                "row_total": 70,
                "base_row_total": 70,
                "row_total_with_discount": 0,
                "tax_amount": 0,
                "base_tax_amount": 0,
                "tax_percent": 0,
                "discount_amount": 0,
                "base_discount_amount": 0,
                "discount_percent": 0,
                "price_incl_tax": 70,
                "base_price_incl_tax": 70,
                "row_total_incl_tax": 70,
                "base_row_total_incl_tax": 70,
                "options": "[]",
                "weee_tax_applied_amount": null,
                "weee_tax_applied": null,
                "name": "Simple Running Backpack"
            },
            {
                "item_id": 2,
                "price": 100,
                "base_price": 100,
                "qty": 1,
                "row_total": 100,
                "base_row_total": 100,
                "row_total_with_discount": 0,
                "tax_amount": 0,
                "base_tax_amount": 0,
                "tax_percent": 0,
                "discount_amount": 0,
                "base_discount_amount": 0,
                "discount_percent": 0,
                "price_incl_tax": 100,
                "base_price_incl_tax": 100,
                "row_total_incl_tax": 100,
                "base_row_total_incl_tax": 100,
                "options": "[]",
                "weee_tax_applied_amount": null,
                "weee_tax_applied": null,
                "name": "20% Discount"
            }
        ],
        "total_segments": [
            {
                "code": "subtotal",
                "title": "Subtotal",
                "value": 170
            },
            {
                "code": "shipping",
                "title": "Shipping & Handling (Flat Rate - Fixed)",
                "value": 5
            },
            {
                "code": "tax",
                "title": "Tax",
                "value": 0,
                "extension_attributes": {
                    "tax_grandtotal_details": []
                }
            },
            {
                "code": "grand_total",
                "title": "Grand Total",
                "value": 175,
                "area": "footer"
            }
        ]
    }
}

```

### Verify this step {#verify-step}

{% glossarytooltip c3ce6f9b-a66a-4d81-ad4c-700f9dfaa9e2 %}Sign in{% endglossarytooltip %} to the Test store as the customer and go to the checkout page.

The payment method is Bank Transfer, the billing and shipping addresses are displayed, and the shipping charges have been calculated.