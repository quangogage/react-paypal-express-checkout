# react-paypal-express-checkout
React component that renders Paypal's express check out button

# Forked!
This is a fork of the original repo. I forked it so that I could implement the ability to send item information with the payment.

I only edited `/dist/` rather than `/src` because my edits are very minor.

### Added Necessary Props
An `items` array. Refer to [this documentation](https://developer.paypal.com/docs/api/payments/#payment_create_request) for proper syntax.

Items array object example:
```{
  name: "White Shirt",
  quantity: 1,
  price: 200, // Make sure this is correct! Paypal will not accept the payment if the combined total of all the items is not the same as the specified total purchase total
  currency: "USD"
}```

If you wish to add options such as color, size, etc,. append it to the name string.

## Install

```bash
npm install --save https://github.com/quangogage/react-paypal-express-checkout/blob/master/README.md
```

## Usage

## Simplest Example (with minimum set of parameters, this will use the "sandbox" environment)

```javascript
import PaypalExpressBtn from 'react-paypal-express-checkout';

export default class MyApp extends React.Component {
	render() {
		const client = {
			sandbox:    'YOUR-SANDBOX-APP-ID',
			production: 'YOUR-PRODUCTION-APP-ID',
		}	
        return (
            <PaypalExpressBtn client={client} currency={'USD'} total={1.00} />
        );
    }
}    
```

### Full Example

```javascript
import React from 'react';
import PaypalExpressBtn from 'react-paypal-express-checkout';

export default class MyApp extends React.Component {
    render() {		
		const onSuccess = (payment) => {
			// Congratulation, it came here means everything's fine!
            		console.log("The payment was succeeded!", payment);
            		// You can bind the "payment" object's value to your state or props or whatever here, please see below for sample returned data
		}		
		
		const onCancel = (data) => {
			// User pressed "cancel" or close Paypal's popup!
			console.log('The payment was cancelled!', data);
			// You can bind the "data" object's value to your state or props or whatever here, please see below for sample returned data
		}	
		
		const onError = (err) => {
			// The main Paypal's script cannot be loaded or somethings block the loading of that script!
			console.log("Error!", err);
			// Because the Paypal's main script is loaded asynchronously from "https://www.paypalobjects.com/api/checkout.js"
			// => sometimes it may take about 0.5 second for everything to get set, or for the button to appear			
		}			
			
		let env = 'sandbox'; // you can set here to 'production' for production
		let currency = 'USD'; // or you can set this value from your props or state  
		let total = 1; // same as above, this is the total amount (based on currency) to be paid by using Paypal express checkout
		// Document on Paypal's currency code: https://developer.paypal.com/docs/classic/api/currency_codes/
		
		const client = {
			sandbox:    'YOUR-SANDBOX-APP-ID',
			production: 'YOUR-PRODUCTION-APP-ID',
		}
		// In order to get production's app-ID, you will have to send your app to Paypal for approval first
		// For sandbox app-ID (after logging into your developer account, please locate the "REST API apps" section, click "Create App"): 
		//   => https://developer.paypal.com/docs/classic/lifecycle/sb_credentials/
		// For production app-ID:
		//   => https://developer.paypal.com/docs/classic/lifecycle/goingLive/		
		
		// NB. You can also have many Paypal express checkout buttons on page, just pass in the correct amount and they will work!		  
        return (
            <PaypalExpressBtn env={env} client={client} currency={currency} total={total} onError={onError} onSuccess={onSuccess} onCancel={onCancel} />
        );
    }
}
```

### Props

- `env: String (default: "sandbox")` - You can set this to "production" for production
- `client: Object (with "sandbox" and "production" as keys)` - MUST set, please see the above example
- `currency: String ("USD", "JPY" etc.)` - MUST set, please see the above example
- `total: Number (1.00 or 1 - depends on currency)` - MUST set, please see the above example
- `onError: Callback function (happens when Paypal's main script cannot be loaded)` - If not set, this will take the above function (in example) as a default return
- `onSuccess: Callback function (happens after payment has been finished successfully)` - If not set, this will take the above function (in example) as a default return
- `onCancel: Callback function (happens when users press "cancel" or close Paypal's popup)` - If not set, this will take the above function (in example) as a default return

## Sample Returned Data

- `onSuccess` - the returned `payment` object will look like: 
	+ `{paid: true, cancelled: false, payerID: "H8S4CU73PFRAG", paymentID: "PAY-47J75876PA321622TLESPATA", paymentToken: "EC-8FE085188N269774L", returnUrl: "https://www.sandbox.paypal.com/?paymentId=PAY-47J75876PA321622TLESPATA&token=EC-8FE085188N269774L&PayerID=H8S4CU73PFRAG"}`
- `onCancel` - the returned `data` object will look like: 
	+ `{paymentToken: "EC-42A825696K839141X", cancelUrl: "https://www.sandbox.paypal.com?token=EC-42A825696K839141X"}`

### Reference Document on How to Have Paypal's Developer Account and Merchant + Buyer accounts

- In fact, please just go to Paypal's developer page, login with your "Real" Paypal account: 
	+ Click on the "Sandbox accounts" on the left hand side
	+ You will see Merchant + Buyer accounts have been created automatically
	+ You can then change password, profile, can also clone those accounts, or create more accounts (choose country, default currency, set amount for new accounts)
- You can check balance & transaction history of those testing accounts (the same as real accounts)	by login with the accounts' credentials with:
	+ https://www.sandbox.paypal.com/
- Official documents here: (just be patient and go step-by-step, by clicking their next topic's button on page, you can then understand the whole process!)
	+ https://developer.paypal.com/docs/classic/paypal-payments-standard/integration-guide/create_payment_button/

## TODO

- Upgrade this library into more advanced Paypal button library (with features like recurring, add-to-cart, checkout-now etc.)
- Fork & pull-request are very welcomed!

## Thank you

- [React NPM Boilerplate](https://github.com/juliancwirko/react-npm-boilerplate)

## License

MIT
