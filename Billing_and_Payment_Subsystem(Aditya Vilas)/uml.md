classDiagram
    %% === Services ===
    class BillingService {
        +generate_invoice(order_id: string): Invoice
        +process_payment(invoice_id: string, method: PaymentMethod, details: PaymentDetails, gateway: PaymentGateway): Payment
        +authorize_payment(invoice_id: string, method: PaymentMethod, details: PaymentDetails, gateway: PaymentGateway): Payment
        +capture_payment(payment_id: string, amount: Money): Payment
        +issue_refund(payment_id: string, amount: Money, reason: string): Refund
        +get_invoice(invoice_id: string): Invoice
    }

    %% === Core billing ===
    class Invoice {
        -invoice_id: string
        -order_id: string
        -customer_id: string
        -status: InvoiceStatus
        -currency: Currency
        -sub_total: Money
        -tax_total: Money
        -discount_total: Money
        -grand_total: Money
        +add_line(line: InvoiceLine): void
        +remove_line(line_id: string): void
        +recalculate_totals(): Money
        +send_to_customer(): void
        +mark_as_paid(): void
        +cancel(reason: string): void
        +balance_due(): Money
    }
    class InvoiceLine {
        -line_id: string
        -sku: string
        -description: string
        -qty: int
        -unit_price: Money
        -tax_amount: Money
        -discount_amount: Money
        -line_total: Money
    }
    Invoice "1" o-- "*" InvoiceLine : contains

    class Payment {
        -payment_id: string
        -invoice_id: string
        -amount: Money
        -status: PaymentStatus
        -method: PaymentMethod
        -transaction_ref: string
        +authorize(): void
        +capture(amount: Money): void
        +fail(reason: string): void
    }

    class Refund {
        -refund_id: string
        -payment_id: string
        -amount: Money
        -status: RefundStatus
        -reason: string
        -tx_ref: string
    }

    %% === Pricing & tax ===
    class PricingEngine {
        +calculate_totals(invoice: Invoice): Money
    }
    class TaxCalculator {
        +calculate(inv_or_line): Money
    }
    class DiscountPolicy {
        <<interface>>
        +apply(inv_or_line): Money
    }
    class PercentageDiscount {
        +percent: float
        +apply(inv_or_line): Money
    }
    class FixedAmountDiscount {
        +amount: Money
        +apply(inv_or_line): Money
    }
    class PriceBook {
        +get_unit_price(sku: string, customer_tier: string): Money
    }
    PricingEngine ..> TaxCalculator
    PricingEngine ..> DiscountPolicy
    PricingEngine ..> PriceBook
    DiscountPolicy <|.. PercentageDiscount
    DiscountPolicy <|.. FixedAmountDiscount

    %% === Gateways and responses ===
    class PaymentGateway {
        <<abstract>>
        +authorize(amount: Money, details: PaymentDetails): GatewayResponse
        +capture(charge_id: string, amount: Money): GatewayResponse
        +refund(charge_id: string, amount: Money): GatewayResponse
    }
    class StripeGateway {
        +authorize(amount: Money, details: PaymentDetails): GatewayResponse
        +capture(charge_id: string, amount: Money): GatewayResponse
        +refund(charge_id: string, amount: Money): GatewayResponse
    }
    class PayPalGateway {
        +authorize(amount: Money, details: PaymentDetails): GatewayResponse
        +capture(charge_id: string, amount: Money): GatewayResponse
        +refund(charge_id: string, amount: Money): GatewayResponse
    }
    class CodGateway {
        +authorize(amount: Money, details: PaymentDetails): GatewayResponse
        +capture(charge_id: string, amount: Money): GatewayResponse
        +refund(charge_id: string, amount: Money): GatewayResponse
    }
    class CryptoGateway {
        +authorize(amount: Money, details: PaymentDetails): GatewayResponse
        +capture(charge_id: string, amount: Money): GatewayResponse
        +refund(charge_id: string, amount: Money): GatewayResponse
    }
    class PaymentDetails {
        +token_or_vpa: string
        +meta: map
    }
    class GatewayResponse {
        -success: bool
        -code: string
        -message: string
        -transaction_ref: string
    }
    BillingService ..> PaymentGateway
    BillingService ..> GatewayResponse
    PaymentGateway <|-- StripeGateway
    PaymentGateway <|-- PayPalGateway
    PaymentGateway <|-- CodGateway
    PaymentGateway <|-- CryptoGateway

    %% === Ledger (double-entry) ===
    class Ledger {
        +post(entry: LedgerEntry): void
        +entries_for(ref_type: string, ref_id: string): list~LedgerEntry~
        +reconcile(payout_file): list~ReconciliationIssue~
    }
    class LedgerEntry {
        -entry_id: string
        -timestamp: datetime
        -debit_account: string
        -credit_account: string
        -amount: Money
        -reference_type: string
        -reference_id: string
        -memo: string
    }
    class ReconciliationIssue {
        -issue_id: string
        -reference_id: string
        -reason: string
        -delta: Money
    }
    Ledger "1" -- "*" LedgerEntry
    Ledger ..> ReconciliationIssue

    %% === Notifications ===
    class NotificationManager {
        +invoice_issued(inv_id: string): void
        +payment_confirmed(payment_id: string): void
        +refund_processed(refund_id: string): void
    }
    BillingService ..> NotificationManager

    %% === Value objects and enums ===
    class Money {
        -amount: decimal
        -currency: Currency
        +add(m: Money): Money
        +subtract(m: Money): Money
    }
    class Currency {
        <<enumeration>>
        INR
        USD
        EUR
        USDC
    }
    class PaymentMethod {
        <<enumeration>>
        UPI
        CARD
        NET_BANKING
        WALLET
        COD
        CRYPTO
    }
    class InvoiceStatus {
        <<enumeration>>
        DRAFT
        ISSUED
        PAID
        PARTIALLY_PAID
        VOID
    }
    class PaymentStatus {
        <<enumeration>>
        INITIATED
        AUTHORIZED
        CAPTURED
        FAILED
        REFUNDED
    }
    class RefundStatus {
        <<enumeration>>
        REQUESTED
        APPROVED
        PROCESSED
        FAILED
    }

    %% === Dependencies from services ===
    BillingService ..> Invoice
    BillingService ..> Payment
    BillingService ..> Refund
    BillingService ..> PricingEngine
    BillingService ..> Ledger
