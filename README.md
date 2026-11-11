# Awesome-Sandbox-Staging-Billing-Environment

# Awesome Sandbox & Staging Billing Environment

> A curated list of top Sandbox and Staging Billing Environment platforms — both SaaS/Hosted commercial solutions and open-source alternatives for safely testing payment flows, billing logic, and subscription lifecycle without touching real money or customer data.
> 🌟 Star this repo if you find it useful! 🌟

---

## 📋 Table of Contents

- [SaaS / Hosted Platforms](#-saas--hosted-platforms)
  - [Chargebee Test Site](#chargebee-test-site)
  - [Stripe Test Mode](#stripe-test-mode)
  - [PayPal Sandbox](#paypal-sandbox)
  - [Braintree Sandbox](#braintree-sandbox)
  - [Authorize.Net Sandbox](#authorizenet-sandbox)
  - [Square Sandbox](#square-sandbox)
  - [Adyen Test Platform](#adyen-test-platform)
  - [Checkout.com Sandbox](#checkoutcom-sandbox)
  - [FastSpring Test Mode](#fastspring-test-mode)
  - [Rebilly Sandbox](#rebilly-sandbox)
- [Open-Source Software](#-open-source-software)
  - [seahaven-stripe-world](#seahaven-stripe-world)
  - [paymock](#paymock)
- [Comparison at a Glance](#-comparison-at-a-glance)
- [Contributing](#-contributing)

---

## 🟦 SaaS / Hosted Platforms

Commercial, cloud-hosted Sandbox and Staging Billing Environment platforms. These are not open-source but are included as reference points for feature comparison.

### Chargebee Test Site
**Type:** Full-featured billing sandbox with time simulation
Chargebee provides every account with two sites: a **Test Site** and a **Live Site** . The Test Site is a sandbox environment where you can test billing configurations, create plans, add customers, and manage subscriptions without affecting actual customer data . It includes a **Chargebee Test Payment Gateway** that simulates payment success, failure (invalid card, insufficient funds), and smart routing scenarios . The standout feature is **Time Machine**, which allows you to virtually travel in time to test dunning, consolidated invoicing, subscription states, scheduled actions, and webhooks without waiting for real-world time to pass . Chargebee also offers AI-powered test site population by parsing your website's pricing page, automatically configuring product families, plans, addons, and brand elements [citation:1][citation:13].  
**Best for:** Subscription businesses needing to test complex billing scenarios, dunning flows, and time-dependent logic without real-world waiting periods.

### Stripe Test Mode
**Type:** Comprehensive test environment with extensive test card library
Stripe Test Mode is a fully isolated environment accessible via API keys prefixed with `sk_test_` and a separate dashboard toggle. It provides an extensive library of **test card numbers** to simulate virtually any payment outcome: successful charges, declines (insufficient funds, expired card, incorrect CVC), 3D Secure authentication flows (frictionless, challenge, out-of-band, one-time password), and mobile-specific verification UI flows [citation:2]. Test transactions settle instantly in test mode (unlike live mode), and webhooks can be triggered via the Stripe CLI or Dashboard. The test environment mirrors production APIs exactly, so code written against test mode works in live mode without modification.  
**Best for:** Developers needing fine-grained control over payment simulation with the most comprehensive test card library and 3DS flow testing.

### PayPal Sandbox
**Type:** Virtual PayPal environment with buyer/seller account management
PayPal Sandbox provides a self-contained virtual environment simulating the live PayPal production environment, with the same API feature set as live . It includes **sandbox accounts** for both buyers (personal) and sellers (business), which PayPal generates by default when you first log into the Developer Dashboard [citation:3]. You can create additional accounts, duplicate existing ones, bulk-upload accounts, or link existing sandbox accounts to your developer account. The sandbox test site (`sandbox.paypal.com`) mirrors the production site, allowing you to log in as buyer or seller to verify money movement, invoices, and account actions . API calls use `https://api-m.sandbox.paypal.com/` instead of the production URL.  
**Best for:** Teams integrating PayPal payments needing to test both buyer and seller perspectives with realistic account balances and transaction flows.

### Braintree Sandbox
**Type:** Gateway sandbox with amount-triggered response scenarios
Braintree's sandbox provides test card numbers for authorization and settlement testing [citation:4]. A unique feature is **amount-based response triggers**: transaction amounts in specific ranges produce different outcomes automatically. For example, amounts from $0.01 to $1999.99 are authorized and settled, $2000.00 to $2999.99 trigger a processor decline, $3000.00 to $3000.99 produce a failed 3000 processor response, and $5001.00 triggers a gateway rejection with "Application Incomplete" [citation:4]. This allows testing error handling without specifying special test cards for every scenario.  
**Best for:** Teams wanting to test decline and error scenarios through transaction amount manipulation rather than memorizing test card numbers.

### Authorize.Net Sandbox
**Type:** Full-featured sandbox with auto-generated API credentials
Authorize.Net provides a sandbox environment with most products and services enabled by default, including Automated Recurring Billing (ARB), Customer Information Manager (CIM), Invoicing, Virtual Point of Sale, and Mobile Point of Sale [citation:5]. API ID and Transaction Key are **auto-generated** for sandbox accounts, and SDKs are available for .NET, Java, PHP, Ruby, and Python via GitHub. **Important limitation:** The sandbox is for integration testing only and should **not** be used for load or stress testing—Authorize.Net may suspend accounts sending unusually high transaction volumes [citation:5]. Transactions settle at the account's Transaction Cut-Off Time (default 4 PM Pacific).  
**Best for:** Developers needing a straightforward sandbox with auto-provisioned credentials and SDK support across multiple languages.

### Square Sandbox
**Type:** Isolated sandbox with API Explorer for reliable testing
Square Sandbox is a free, isolated server environment with a separate base URL (`https://connect.squareupsandbox.com`) for testing Square APIs without affecting real data [citation:6]. Each application registered in the Developer Console gets its own Sandbox environment, and each test account gets a Sandbox Square Dashboard. **Critical caveat:** The Sandbox Dashboard has significant limitations—receipts are not generated, refunds can be viewed but not issued, subscriptions cannot be viewed or edited, and Square hardware cannot be used [citation:6]. Square explicitly recommends using the **API Explorer in Sandbox mode** for reliable test coverage, as it matches production API behavior more closely than the limited Dashboard. Sandbox test values (card numbers, payment tokens) must be used; real cards are rejected.  
**Best for:** Developers wanting a clean sandbox with a reliable API Explorer, accepting that the Dashboard is limited for comprehensive testing.

### Adyen Test Platform
**Type:** Test environment with comprehensive card scenario library
Adyen provides a test platform with standard test card numbers to simulate successful payments, declines (triggered by cardholder name or billing address fields), 3D Secure authentication, surcharges, and chargebacks [citation:7]. The API returns structured `declineCode` values (INSUFFICIENT_FUNDS, FRAUD_SUSPECTED, INVALID_CVC, etc.) with customer-friendly `declineReason` strings, allowing developers to verify error handling for each specific decline type. Refunds can be tested via API or the sandbox portal.  
**Best for:** Teams needing to test specific decline codes and error handling paths with structured responses suitable for production UI.

### Checkout.com Sandbox
**Type:** Separate sandbox authorization server and base URLs
Checkout.com provides a distinct sandbox environment with its own authorization server (`https://{prefix}.access.sandbox.checkout.com/connect/token`) and sandbox-specific base URLs for each API (e.g., `https://balances.sandbox.checkout.com/balances/{id}`) [citation:8]. This isolation ensures sandbox tokens and resources cannot be used in production. The sandbox mirrors production API structure, allowing full integration testing before going live.  
**Best for:** Checkout.com merchants needing a cleanly separated sandbox with distinct credentials and URLs for each service.

### FastSpring Test Mode
**Type:** Hosted checkout test mode with `test.onfastspring.com` domain
FastSpring provides a test mode accessible via a distinct domain pattern: `https://{yourstore}.test.onfastspring.com` for test, versus `https://{yourstore}.onfastspring.com` for live [citation:9]. Test mode supports Web Checkouts (shareable links), Popup Checkouts (embedded via Store Builder script), and the Sessions API for programmatic checkout creation. Test orders use test payment details, and the `order.completed` webhook fires in test mode for fulfillment verification.  
**Best for:** Teams using FastSpring's hosted checkout needing a straightforward test mode with the same checkout experience as production.

### Rebilly Sandbox
**Type:** Sandbox with TestProcessor gateway and 90-day retention
Rebilly provides a sandbox environment accessible at `app-sandbox.rebilly.com`, with a yellow warning banner indicating sandbox mode [citation:10]. It is pre-configured with a **TestProcessor** gateway that sends no information to third parties. Sandbox and live environments share login credentials and some organizations/users but are otherwise separate—hosted payment forms and gateways must be recreated in live. **Important:** Recurring orders in sandbox are automatically canceled after 90 days unless kept [citation:10].  
**Best for:** Rebilly users needing a simple sandbox with a built-in test gateway and awareness of the 90-day recurring order retention limit.

---

## 🟩 Open-Source Software

Self-hosted, community-driven Sandbox and Staging Billing Environment tools. The open-source ecosystem for **billing sandboxes** is very limited, but two projects provide stateful mock environments for Stripe and African payment gateways.

### seahaven-stripe-world
**Stack:** Python | **License:** Open Source (per PyPI)
seahaven-stripe-world is a **stateful Stripe sandbox** designed for agent evaluations, reinforcement learning, and tests [citation:11]. Unlike Stripe's native test mode, which is stateless across API calls, seahaven maintains state within a world instance—customers, subscriptions, invoices, and payment methods persist between requests. It reproduces **8 of the 10 tools** on Stripe's MCP server, allowing AI agents (Claude Desktop, etc.) to interact with a realistic Stripe environment that resets between runs. **Failure injection** works with Stripe's test cards, so you can test decline paths (e.g., `insufficient_funds`) exactly as in test mode. Fixtures allow freezing a starting state (e.g., a "Pro plan at $20/month with three customers") for reproducible tests.  
**Best for:** AI agent developers and researchers needing a stateful, reproducible Stripe environment for evaluation and testing, not as a replacement for Stripe's native test mode.

### paymock
**Stack:** Python | **License:** Open Source (per PyPI)
paymock is a **stateful, offline mock server for Paystack and Flutterwave** (African payment gateways) that includes signed webhooks, no API keys, and no live internet requirement [citation:12]. It addresses the pain of brittle sandboxes and webhook delivery that requires public tunnels. Features include: forcing payment outcomes (`POST /_settle/:ref {"status": "failed"}`), exact HMAC signature generation for webhook verification, **failure injection** for testing decline and timeout paths, auto-delivery of webhooks with real retry schedules, and scenarios for duplicate/delayed/out-of-order webhooks. Transactions persist in memory between requests, and time compression (`PAYMOCK_TIME_SCALE`) allows retry schedules to resolve in milliseconds during tests.  
**Best for:** Developers building on Paystack or Flutterwave needing a reliable, offline sandbox with realistic webhook behavior and failure injection that the official gateways do not provide.

---

## 📊 Comparison at a Glance

| Software | License | Stack | Time Simulation | Failure Injection | Self-Hosted | Best For |
|----------|---------|-------|-----------------|-------------------|-------------|----------|
| **Chargebee Test Site** | Proprietary | Cloud | ✅ Time Machine | ✅ Test Gateway | ❌ | Billing lifecycle + dunning |
| **Stripe Test Mode** | Proprietary | Cloud | ❌ | ✅ Test cards + 3DS | ❌ | Payment scenario coverage |
| **PayPal Sandbox** | Proprietary | Cloud | ❌ | ✅ Account states | ❌ | Buyer/seller perspective |
| **Braintree Sandbox** | Proprietary | Cloud | ❌ | ✅ Amount-triggered | ❌ | Amount-based decline testing |
| **Square Sandbox** | Proprietary | Cloud | ❌ | ✅ Test values | ❌ | API Explorer reliability |
| **Adyen Test** | Proprietary | Cloud | ❌ | ✅ Decline codes | ❌ | Structured decline handling |
| **seahaven-stripe-world** | Open Source | Python | ✅ Fixed clock | ✅ Stripe test cards | ✅ | Stateful agent evaluation |
| **paymock** | Open Source | Python | ✅ Time scale | ✅ Forced outcomes | ✅ | Offline Paystack/Flutterwave |

---

## 🤝 Contributing

Contributions are welcome! If you know of an open-source Sandbox or Staging Billing Environment tool that should be listed here, please open a pull request with:

- Project name and URL
- Brief description (1–2 sentences)
- Stack and license
- Why it stands out

Please ensure the project is actively maintained (releases within the last 12 months) and genuinely open-source. Note that the open-source ecosystem for **billing sandboxes** is extremely limited—most payment gateways provide proprietary sandbox environments that cannot be self-hosted or replaced. **seahaven-stripe-world** is notable for adding **statefulness** that Stripe's native test mode lacks, making it suitable for AI agent evaluations where reproducible starting states matter [citation:11]. **paymock** fills a specific gap for African payment gateways (Paystack, Flutterwave) by providing offline, stateful mocking with realistic webhook retry behavior that the official sandboxes do not expose [citation:12]. For production billing logic testing (plans, dunning, proration), commercial platforms like Chargebee's Test Site with Time Machine remain unmatched by open-source alternatives.

---

## 📄 License

This list is released under CC0 1.0 Universal (Public Domain). The software projects listed retain their respective licenses.

---

⭐ **If this list helped you discover a useful tool, please star the repo!** ⭐
