# Case Dataset - Anonymized Order Data

This folder contains the anonymized dataset for the Software Engineer take-home case.

## Files (6 JSON tables)

### `companies.json`
Companies that have placed orders. Each company can have multiple contacts.

**Fields:**
- `id` - Internal company ID
- `company_id` - API company ID
- `company_name` - Anonymized company name (Company A, Company B, etc.)
- `default_contact_id` - Default contact for this company

### `contacts.json`
Individual contacts (people) who send order emails. One contact can send multiple emails.

**Fields:**
- `id` - Contact ID
- `company_id` - Which company this contact belongs to
- `email_address` - Anonymized email (contact1@example.com, etc.)
- `api_token` - API authentication token
- `api_contact_id` - External API contact reference

### `emails.json`
The 32 email messages (26 orders, 6 non-orders). Contains both body_text and body_html.

**Fields:**
- `id` - Email ID
- `email_id` - UUID identifier
- `subject` - Email subject line
- `sender` - Who sent it (references contacts.json)
- `recipients` / `cc` / `bcc` - Email recipients
- `received_date` - When email was received
- `body_text` - Plain text email body (anonymized)
- `body_html` - HTML email body (anonymized)
- `has_attachments` - Whether email has attachments
- `is_processed` / `processing_status` - Processing state
- `is_order_email` - True if this is an order
- `created_at` / `updated_at` - Timestamps
- `receiving_email` - Which inbox received this
- `agent_modified` - Whether agent modified this email

### `orders.json`
Processed orders extracted from the order emails. All have status = "Sent".

**Fields:**
- `id` - Order ID
- `order_number` - Unique order number
- `reference` - Customer reference
- `comment` - Order comments/notes
- `total_amount` - Total order amount
- `status` - Order status (all are "Sent")
- `external_order_id` - External system order ID
- `contact_id` - Which contact placed this order
- `delivery_address_id` - Delivery address reference
- `delivery_date` - Requested delivery date
- `parsed_delivery_date` / `predicted_delivery_date` - AI-extracted dates
- `parsed_comment` / `predicted_comment` - AI-extracted comments
- `created_at` / `updated_at` - Timestamps

### `order_lines.json`
Individual product line items for each order.

**Fields:**
- `id` - Line item ID
- `order_id` - Which order this belongs to
- `sku` - Product SKU code
- `product_name` - Product description
- `qty` - Quantity ordered
- `unit` - Unit of measurement (stuks, meter, rol, plaat, etc.)
- `reference` - Line item reference
- `is_correct` - Whether this line was verified
- `parsed_product_name` / `predicted_product_name` - AI extractions
- `parsed_product_qty` / `predicted_product_qty` - AI extractions
- `parsed_product_unit` / `predicted_product_unit` - AI extractions
- `parsed_product_reference` / `predicted_product_reference` - AI extractions
- `base_unit` / `logistical_units` - Unit conversions
- `is_cuttable` / `sawing_type` / `cutting_type` / `grain_direction` / `packaging` - Product specs
- `ai_generated` - Whether this line was AI-generated

### `order_emails.json`
Junction table linking emails to orders (one email can result in multiple orders).

**Fields:**
- `id` - Junction ID
- `email_id` - Email UUID (references emails.json)
- `order_id` - Order ID (references orders.json)
- `created_at` - When link was created

## Dataset Characteristics

- **32 total emails** (26 orders, 6 non-orders)
- **26 orders** (all with status = "Sent")
- **~60 order lines** (product items)
- **16 contacts**
- **14 companies**

### Special Features

✅ **Contact with multiple emails:** 4 contacts each have 2-3 emails in dataset  
✅ **Company with multiple contacts:** Company B has 2 different contacts  
✅ **Similar products:** Several products with near-identical names (tests matching logic)  
✅ **Incomplete data:** Some orders missing fields (customer name, delivery date, etc.)  
✅ **No attachment dependencies:** All order information is in email body  

## Anonymization

All sensitive data has been replaced:
- Company names → Company A, B, C...
- Email addresses → contact1@example.com
- Person names → "Person"
- Phone numbers → 06-12345678 or 088-1234567
- URLs → https://example.com
- Addresses → 1234 AB, Examplestraat 1
- KVK/BTW numbers → Generic placeholders
- API tokens → Placeholder test tokens

**Note:** "IGEPA" is kept as-is since it's the vendor company the candidate would be working for.
