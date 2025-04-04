---
title: Direct EA billing invoice documents
description: Learn how to understand the invoice files associated with your direct enterprise agreement.
author: sahilkhurana
ms.reviewer: sahilkhurana
ms.service: cost-management-billing
ms.subservice: enterprise
ms.topic: conceptual
ms.date: 04/02/2025
ms.author: sahilkhurana
---

# Direct EA billing invoice documents

This article helps you understand the invoice files associated with your direct enterprise agreement. For information about downloading the files, see [View your Azure usage summary details and download reports for direct EA enrollments](direct-ea-azure-usage-charges-invoices.md).

All documents are available between the 12th- 15th day of each month. However, when an invoice is unusually large, availability might be delayed.

## Understand the invoice file

Your invoice is a PDF file that contains at least two pages.

The first page is the billing summary. It contains general information about the invoice, amount due, and payment instructions, if applicable. It also contains address information for your organization and high-level details about your order.

The second page lists the individual products in your order.

### Invoice terms

On the top of page one, you'll find the summary with the following fields:

**Invoice** – The number is a unique number generated by Microsoft that identifies your spending for the corresponding billing period.

**Invoice Date** - The date Microsoft created the invoice.

**PO Number** - The purchase order (PO) number that you specify. The PO number can't be updated on an invoice that's already generated.

**PO Date** – It's generally the date when the order was entered into Microsoft systems.

**Billing Period** - The date range covered by the invoice.

**Payment Terms** – They explain the arrangement for when the invoice payment is due.  
  > [!IMPORTANT]
  > Pay using wire transfer or ACH using the instructions provided on the invoice. Don't mail a physical check to a Microsoft address shown on your invoice.

**Due Date** - The date when the invoice payment is due to Microsoft.

#### Addresses

The following addresses differ, depending on the size and configuration of your organization.

- The Sold-To address is the name and address of the organization that bought the subscription.
- The Bill-To address is the address of your billing department.
- The Ship-To address are details of the location to which the products are shipped or used for tax exemption (for India).
- The End Customer Address is the address where the service is used.

#### Billing summary

Billing summary gives the breakdown of the total amount due. Total = Charges – Commitment usage (if applicable) + Sales Tax.

#### Billing details by product

Page two lists billing details by product, including unit price, quantity, commitment usage (if applicable), net charge, tax rate, tax amount, and total corresponding to each usage charge.

Here's an example of the first page of the invoice file.

:::image type="content" source="./media/direct-ea-billing-invoice-documents/invoice-page-01.png" alt-text="Example screenshot of the first page of the invoice file." lightbox="./media/direct-ea-billing-invoice-documents/invoice-page-01.png" :::

Here's an example of the last page of the invoice file.

:::image type="content" source="./media/direct-ea-billing-invoice-documents/invoice-page-02.png" alt-text="Example screenshot of the second page of the invoice file." lightbox="./media/direct-ea-billing-invoice-documents/invoice-page-02.png" :::

_Prices shown on the invoice are for informational purposes only._

## Understand the summary invoice file

The summary invoice file is a concise version of the detailed PDF file with a broad categorization of purchases at product family level. The first page is the same as the invoice PDF. The second page contains a billing summary by product family covering usages charges and corresponding amount incurred.

Here's an example of the first page of the summary invoice file.

:::image type="content" source="./media/direct-ea-billing-invoice-documents/invoice-summary-page-01.png" alt-text="Example screenshot of the first page of the summary invoice file." lightbox="./media/direct-ea-billing-invoice-documents/invoice-summary-page-01.png" :::

Here's an example of the last page of the summary invoice file.

:::image type="content" source="./media/direct-ea-billing-invoice-documents/invoice-summary-page-02.png" alt-text="Example screenshot of the second page of the summary invoice file." lightbox="./media/direct-ea-billing-invoice-documents/invoice-summary-page-02.png" :::


## Understand the transactions file

The transactions file is a CSV file that includes the same information as the invoice in a format that helps you quickly reconcile charges. It contains the following line-item details:

| Invoice item | Description |
| --- | --- |
| Invoice Number | A Microsoft generated unique number that identifies your spending for the corresponding billing period. |
| Invoice Date | The date Microsoft created the invoice. |
| Document Type | Determines whether it's an invoice or credit note. |
| Agreement Number | The Licensing ID # / enrollment # / contract number. |
| Bill To | Customer Number, Bill To Customer Name, and Bill To Customer Country are details of the billing department. |
| Sold To | Customer Number, Sold To Customer Name, and Sold To Customer Country are details of the organization that bought the subscription. |
| Ship To | Customer Number, Ship To Customer Name, and Ship To Customer Country are details of the location where the products are shipped or used for tax exemption (for India). |
| End Customer Name and End Customer Country | Details of the final consumer where the service is used. |
| Purchase Order Number | The purchase order (PO) number that you specify. |
| Billing Currency | Shows the currency that was chosen by the end customer in terms of payment. |
| Transaction Type | Reflects whether it's a debit invoice or a credit memo. |
| Line Item Number | The line ID for internal reference. |
| Usage Country | The location where the product is used. |
| Delivery | Shows how the invoice is being delivered. |
| MS Part Number | A reference number for the product. |
| Item Name | The description of the purchased product. |
| Product Family | The logical categorization of products. |
| License Type | Reflects the terms of buying the product. |
| Price Level | The price categorization of product. |
| Billing Option | The mode of payment. |
| Taxable | Indicates whether the product is taxable. |
| Pool | The classification of the product into a system, server, or application. |
| Service Period Start Date, Service Period End Date | Indicates the eligible service period. |
| Reason Code | Used at the time of return. |
| Description | The explanation of the reason code. |
| Quantity | The number of units bought or used. |
| Unit Price | The price per unit product. |
| Extended Amount | The quantity multiplied by the unit price. |
| Commitment Usage | The amount of monetary commitment that has been used. |
| Net Amount | The extended amount minus the commitment usage. |
| Tax Rate | The tax rate applicable to the product based on the country/region of billing. |
| Tax Amount | The net amount multiplied by tax rate. |
| Total | The sum of the net amount and tax amount. |
| Is Third Party | Indicates whether the product or service is a third-party product. |

## Related content

- Learn how to download your Direct EA billing invoice documents at [View your Azure usage summary details and download reports for direct EA enrollments](direct-ea-azure-usage-charges-invoices.md).
