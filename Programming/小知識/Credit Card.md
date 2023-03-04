# Credit Card Number

**Length of credit card no.**
* American Express
    15-digit numbers
    = $10^{15} = 1,000,000,000,000,000$ (a quadrillion) unique cards
* MasterCard
	16-digit
* Visa
	13- and 16-digit

but actually there are constraints for the numbers
so it provides less than $10^{15}$ numbers


**Structure of credit card no.**
* American Express
	all start with *34* or *37*
* MasterCard
	mostly start with *51, 52, 53, 54 or 55*
* Visa
	all start with *4*


**Checksum**
Then also the number respect some mathematical constraints and computers use it to check if the number valid.
But surely there are some operations require checking database too, which however is slow.

**Luhn's Algorithm**
* the secret(?) formula for the *checksum*
* invented by *Hans Peter Luhn of IBM*
* a valid credit card number fulfill a requirement, and here is how to check if a number fulfilled it:
> exmaple number - 4003600000000014

1. Multiply every other digit by 2, starting with the last digit, then add those products'digits together
> **4** 0 **0** 3 **6** 0 **0** 0 **0** 0 **0** 0 **0** 0 **1** 4
> after multiply, `2 + 0 + 0 + 0 + 0 + 12 + 0 + 8`
> now add those digits, `2 + 0 + 0 + 0 + 0 + 1 + 2 + 0 + 8 = 13`

2. Add the sum to the sum of the digits that weren't multiplied by 2
> 4 **0** 0 **3** 6 **0** 0 **0** 0 **0** 0 **0** 0 **0** 1 **4**
> again, add them starting from the last digit, `4 + 0 + 0 + 0 + 0 + 0 + 3 + 0 = 7`
> sum the results: `13 + 7 = 20`

3. if the result's last digit is 0, the number is valid.
> `20 % 10 = 0`
> so this is a valid credit card number.


**[PayPal's standard test card](https://developer.paypal.com/api/nvp-soap/payflow/integration-guide/test-transactions/#standard-test-cards)**
