# Commerce Platform

The language of a single-vendor Iranian online store that sells digital products and electronics to customers who sign in with a phone number.

## Language

### Catalog and delivery

**Product**:
Something the store sells, with its own page, one main category and one or more variants. It is either physical or digital, never both.
_Avoid_: item, listing

**Variant**:
One purchasable version of a product, such as a color and storage combination, with its own SKU, price, stock (or code stock) and images.
_Avoid_: option, model, child product

**SKU**:
The identifier staff give to one variant.
_Avoid_: product code, article number

**Category**:
A node in the category tree (at most three levels) that groups products; each product has one main category.
_Avoid_: collection, department

**Brand**:
The maker of a product, used as a filter and with its own page.
_Avoid_: manufacturer

**Specification**:
A named fact about a product, such as RAM. Staff define specification fields once per category; a product can also carry free-form extra rows.
_Avoid_: attribute, property, feature

**Product state**:
Where a product stands in its life: Draft (staff only), Published (visible and sellable), Discontinued (the page stays visible marked "no longer available" and is not sellable), Hidden (shown nowhere, not sellable). Out of stock is not a state; it comes from stock.
_Avoid_: status

**Before price**:
The optional earlier price of a variant, shown struck through; the discount percentage is worked out from it.
_Avoid_: old price, original price, list price

**Warranty**:
The guarantee that comes with a variant: a provider name and a duration in months, or none. Shown on the product page and passed to price-comparison feeds.
_Avoid_: guarantee

**Feed opt-out**:
A per-product switch that keeps a product out of price-comparison feeds while it stays on the store.
_Avoid_: feed blacklist

**Digital product**:
A product the buyer receives electronically: it delivers a digital code, a downloadable file, or both.
_Avoid_: virtual product, soft product

**Physical product**:
A product that is shipped to the buyer or collected at the store. A product is never both physical and digital.
_Avoid_: hardware

**Digital code**:
A single-use secret value (a license key, gift-card number or voucher) issued to one buyer.
_Avoid_: key, serial, coupon

**Code stock**:
The loaded, not-yet-sold digital codes of one digital product.
_Avoid_: key pool

**Downloadable file**:
A file the buyer gets through an expiring download link after paying.
_Avoid_: attachment, asset

**Digital delivery**:
Making a digital code or downloadable file available to the buyer once payment succeeds.

**Shipment**:
A physical parcel sent to a buyer.
_Avoid_: delivery (reserved for digital products)

**Shipping fee table**:
The staff-edited table that sets the shipping fee by province and weight.
_Avoid_: rate card, tariff

**Store pickup**:
Collecting a physical order at the store instead of having it shipped.
_Avoid_: in-store delivery

### Pricing

**Discount code**:
A code a customer enters at checkout to reduce the price of an order.
_Avoid_: coupon, promo code, voucher

### People, roles and payment

**Customer**:
A person who buys from the store, identified by a verified mobile phone number.
_Avoid_: user, buyer, client, account

**Staff member**:
A person who runs the store through the admin panel.
_Avoid_: admin user, employee, operator

**Payment gateway**:
An Iranian online payment service through which a customer pays by card.
_Avoid_: bank portal, PSP, payment provider

**Role**:
A named set of permissions given to staff members.
_Avoid_: permission group, access level

**Audit log**:
The record of who changed what, and when, in the admin panel.
_Avoid_: history, activity feed
