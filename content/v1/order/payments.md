---
title: Payments | Spree API
---

# Payments API

* TOC
{:toc}

## Listing payments 

To see details about an order's payments, make this request:

    GET /api/orders/R1234567/payments

### Response

<%= headers 200 %>
<%= json(:payment) { |h| { :payments => [h] } } %>

## A new payment

In order to create a new payment, you will need to know about the available payment methods and attributes. To find these out, make this request:

    GET /api/orders/R1234567/payments/new

### Response

<%= headers 200 %>
<%= json \
  :attributes =>
  ["id", "source_type", "source_id", "amount",
   "payment_method_id", "response_code", "state",
   "avs_response", "created_at", "updated_at"],
  :payment_methods => [Spree::Resources::PAYMENT_METHOD] %>

## Creating a payment

To create a new payment, make a request like this:

   POST /api/orders/R1234567/payments?payment[payment_method_id]=1&payment[amount]=10

### Response

<%= headers 201 %>
<%= json(:payment) %>

## A single payment

To get information for a particular payment, make a request like this:

   GET /api/orders/R1234567/payments/1

### Response

<%= headers 200 %>
<%= json(:payment) %>

## Authorizing a payment

To authorize a payment, make a request like this:

   PUT /api/orders/R1234567/payments/1/authorize

### Response

<%= headers 200 %>
<%= json :payment %>

### Failed Response

<%= headers 422 %>
<%= json :error => "There was a problem with the payment gateway: [text]" %>

## Capturing a payment

<%= warning "Capturing a payment is typically done shortly after authorizing the payment. If you are auto-capturing payments, you may be able to use the purchase endpoint instead." %>

To capture a payment, make a request like this:

   PUT /api/orders/R1234567/payments/1/capture

### Response

<%= headers 200 %>
<%= json :payment %>

### Failed Response

<%= headers 422 %>
<%= json :error => "There was a problem with the payment gateway: [text]" %>

## Purchasing with a payment

<%= warning "Purchasing a payment is typically done only if you are not authorizing payments before-hand. If you are authorizing payments, then use the authorize and capture endpoints instead." %>

To make a purchase with a payment, make a request like this:

   PUT /api/orders/R1234567/payments/1/purchase

### Response

<%= headers 200 %>
<%= json :payment %>

### Failed Response

<%= headers 422 %>
<%= json :error => "There was a problem with the payment gateway: [text]" %>

## Voiding a payment

To void a payment, make a request like this:

   PUT /api/orders/R1234567/payments/1/void

### Response

<%= headers 200 %>
<%= json :payment %>

### Failed Response

<%= headers 422 %>
<%= json :error => "There was a problem with the payment gateway: [text]" %>

## Crediting a payment 

To credit a payment, make a request like this:

   PUT /api/orders/R1234567/payments/1/credit?amount=10

If the payment is over the payment's credit allowed limit, a "Credit Over Limit" response will be returned.

### Response

<%= headers 200 %>
<%= json :payment %>

### Failed Response

<%= headers 422 %>
<%= json :error => "There was a problem with the payment gateway: [text]" %>

### Credit Over Limit Response

<%= headers 422 %>
<%= json :error => "This payment can only be credited up to [amount]. Please specify an amount less than or equal to this number." %>
