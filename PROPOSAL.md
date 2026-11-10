# PayeeCheck

**Repository:** https://github.com/payeecheck/payeecheck

## 1. Project name

PayeeCheck: payee verification for stablecoin payments on Stellar.

## 2. One-line description

Before you send USDC, PayeeCheck tells you whether the destination address belongs to the person or business you mean to pay. It does for stablecoin payments what the "show recipient name" step does in a banking app.

## 3. Problem

A bank transfer shows the recipient's name so the sender can check it. A stablecoin transfer shows only a 56-character address. A payment sent to the wrong address cannot be reversed.

Senders usually lose funds in one of three ways:

- **Payment-detail fraud.** An attacker takes over a supplier's email or Telegram account and announces a "new wallet". The accountant then pays the attacker.
- **Address poisoning.** An attacker sends a tiny payment from an address whose first and last characters match a familiar one, then waits for the victim to copy the wrong address from their transaction history.
- **A valid address for the wrong party.** The sender copies an old address, picks the wrong saved contact, or pastes from the wrong message. A Stellar address has a checksum, so wallets already reject a mistyped character. They cannot tell, however, whether a valid address belongs to the intended payee.

Some Stellar wallets flag addresses already known to be malicious. None confirms that an address belongs to the payee the sender expects.

## 4. Who has this problem

- **Businesses paying suppliers in stablecoins**, for example importers paying overseas partners. These payments are often large, so one mistake is costly.
- **Payout companies and anchors** that send funds to thousands of recipients a day.
- **Finance teams at crypto companies** that pay salaries and vendors in USDC.
- **Wallets**, because a customer who loses funds to the wrong address may blame the wallet and stop using it.

## 5. Solution

- **Verified directory.** Each entry binds a Stellar address to an organisation's web domain. An organisation proves the binding in one of two ways. It can list the address in its `stellar.toml` and set its domain as the account's on-chain `home_domain`. Or it can sign a SEP-10 challenge with the account and prove control of the domain.
- **Pre-send check.** The wallet sends PayeeCheck the address and, when known, the payee the sender expects. PayeeCheck answers using the match / close match / no match model that banks use for Confirmation of Payee. Examples:
  - ✅ "Example Ltd (example.com), verified"
  - ⚠️ "This address belongs to examp1e.com, not example.com"
  - ⚠️ "This organisation changed its payment address recently"
  - ⚠️ "This address looks like one you know, but it is DIFFERENT"
- **API and widget.** Wallets, anchors and payout companies can add the check to their send flow. A public web page offers quick lookups.

PayeeCheck never holds funds, never needs a wallet connection to look up an address, and never blocks a payment. It advises, and the sender decides.

## 6. Why Stellar

- **The building blocks already exist.** Organisations publish the accounts they control on their own domain through `stellar.toml` (SEP-1), and SEP-10 is the standard way to prove control of an account. Anchors and issuers already publish their addresses this way, so the directory has real data from day one.
- **Stellar is a payments network for institutions.** Circle issues USDC natively on Stellar, and MoneyGram and anchors in many countries operate on the network. As B2B stablecoin payments grow, they will need this safety check.
- **Regulation is heading here.** The EU now requires payment providers to verify the payee's name on euro transfers (Verification of Payee, under the Instant Payments Regulation). We expect regulators to ask digital-asset payment providers for an equivalent check.
- **Nobody has built payee confirmation on Stellar yet.** We searched Stellar Scout and found no existing project. On other chains, RecipientGuard and Bound are working on the same problem.

## 7. Go-to-market

- **Month 1: free tool.** Launch the public address-check page, with a directory pre-populated from anchors and issuers. Reach users through the Stellar Vietnam community, the SCF and Stellar Discord servers, and the crypto projects our team has worked with.
- **Months 2–3: verified businesses.** Invite SCF-funded teams and projects that pay in USDC to verify their addresses. Publish write-ups of real losses from payment-detail fraud and address poisoning, so the problem is easy to understand and share.
