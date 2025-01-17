# Apple pay payment token signature validation and decryption

A node app to handle apple pay payment token signature validation and decryption

The code follows the standards [Payment Token Format Reference](https://developer.apple.com/documentation/passkit_apple_pay_and_wallet/apple_pay/payment_token_format_reference)

This package is a fork of apple-pay-token-decrypt, but instead of using the deprecated node-webcrypt-ossl, we use the inbuilt node:crypto package

## Getting started  
for npm users
```sh
npm i --save apple-pay-token-decrypt
```
for yarn users
```sh
yarn add  apple-pay-token-decrypt
```

You will need to create a merchant id and payment processing certificate on apple developer site. Details described [here](https://help.apple.com/developer-account/#/devb2e62b839?sub=dev103e030bb). After that you will get the merchant certificate and private key.


## Usage

```js
const requestToken = {
    "data": "<encryptedData>",
    "version": "EC_v1",
    "signature": "<signature>",
    "header": {
        "ephemeralPublicKey": "<ephemeralPublicKey>",
        "publicKeyHash": "<publicKeyHash>",
        "transactionId": "<transactionId>"
    }
}

const publicCert = fs.readFileSync(path.join(__dirname, './publicCert.pem'), 'utf8') // import your certificate file
const privateKey = fs.readFileSync(path.join(__dirname, './privateKey.pem'), 'utf8') // import your private key file

const token = new applePayPaymentToken(requestToken)
const decryptedToken = token.decrypt(publicCert, privateKey)
decryptedToken.then( ret => {
    console.log(ret)
}).catch( err => {
    console.error(err)
})
```

#### Sample output
```js
{
    "applicationExpirationDate": "231231",
    "applicationPrimaryAccountNumber": "4802********4384",
    "currencyCode": "901",
    "deviceManufacturerIdentifier": "0400*****273",
    "paymentDataType": "3DSecure",
    "transactionAmount": 89900,
    "paymentData": {
        "eciIndicator": "7",
        "onlinePaymentCryptogram": "Aj+W784****************MAABAAA="
}
```