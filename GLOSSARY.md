# Commerce Platform

The language of a single-vendor Iranian online store that sells digital products and electronics to customers who sign in with a phone number.

## Language

### Catalog and delivery

**Digital product**:
A product the buyer receives electronically, as a digital code or a downloadable file.
_Avoid_: virtual product, soft product

**Physical product**:
A product that is shipped to the buyer or collected at the store.
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
