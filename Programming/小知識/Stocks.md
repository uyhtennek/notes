firstly if you don't know how stocks work
* that is what it means by buying and selling stocks
* see this first
[How to Invest in Stocks: A Beginner's Guide (investopedia.com)](https://www.investopedia.com/articles/basics/06/invest1000.asp)

here is one of the brokers
[Market Data & Connectivity | IEX Exchange | IEX Group](https://exchange.iex.io/products/market-data-connectivity/)

* it provides stock quotes for us to download via their API, *application programming interface*
* using URLs like https://cloud.iexapis.com/stable/stock/nflx/quote?token=API_KEY
	* notice how Netflix's symbol, `nflx`, is embedded in this URL
		* that's how IEX knows whose data to return
	* also actually this link doesn't work because IEX requires us to use API key
* its response is in JSON format
	* JavaScript Object Notation

```json
{
	"avgTotalVolume": 4329597,
	"calculationPrice": "tops",
	"change": 1.21,
	...
}
```

so this is an object
* between curly braces `{}`, are comma-separated list of key-value pairs
* with a colon `:` separating each key from its value
___

To register for an API key for querying IEX's data
1. register an [IEX account](https://iexcloud.io/cloud-login#/register/), and confirm the account via a confirmation email
2. visit https://iexcloud.io/console/tokens
3. copy the key that appears under the *Token* column
	* maybe also paste the key somewhere in case we need it again later
4. execute `export API_KEY=value` from terminal
	* where `value` is where we paste the copied key
