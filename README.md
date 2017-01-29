<h1 align="center">
  <a href="https://github.com/posquit0/node-iamporter" title="Iamporter.js">
    <img alt="Iamporter.js" src="https://avatars3.githubusercontent.com/u/11437969" width="200px" height="200px" />
  </a>
  <br />
  Iamporter
</h1>

<p align="center">
  A REST API client for I'mport
</p>

<div align="center">
  <a href="https://circleci.com/gh/posquit0/node-iamporter">
    <img alt="CircleCI" src="https://circleci.com/gh/posquit0/node-iamporter.svg?style=shield" />
  </a>
  <a href="https://badge.fury.io/js/iamporter">
    <img alt="npm version" src="https://badge.fury.io/js/iamporter.svg" />
  </a>
  <a href="https://www.npmjs.com/package/iamporter">
    <img alt="npm" src="https://img.shields.io/npm/dt/iamporter.svg" />
  </a>
  <a href="https://david-dm.org/posquit0/node-iamporter">
    <img alt="npm" src="https://img.shields.io/david/posquit0/node-iamporter.svg?style=flat-square" />
  </a>
  <a href="https://opensource.org/licenses/mit-license.php">
    <img alt="MIT Licence" src="https://badges.frapsoft.com/os/mit/mit.svg?v=103" />
  </a>
  <a href="https://github.com/ellerbrock/open-source-badge/">
    <img alt="Open Source Love" src="https://badges.frapsoft.com/os/v1/open-source.svg?v=103" />
  </a>
</div>

<br />

[**Iamporter**](https://github.com/posquit0/node-iamporter)는 [아임포트](http://iamport.kr/)에서 제공하는 REST API를 Node.js에서 쉽게 활용하기 위하여 작성된 클라이언트 모듈입니다.

- 이용 중 발생한 문제에 대하여 책임지지 않습니다.
- 최초 작성은 자동차 렌트 플랫폼 [CARPLAT](https://www.carplat.co.kr)에서 사용하기 위하여 작성되었습니다.

## <a name="installation">Installation

```bash
$ npm install iamporter
```


## <a name="usage">Usage

- 모든 메소드는 [Promise](http://www.html5rocks.com/ko/tutorials/es6/promises/)를 반환

```node
var iamporter = require('iamporter').createClient({
  apiKey: 'YOUR_API_KEY',
  secret: 'YOUR_SECRET'
});

// 비인증 결제를 위한 빌링키 생성
iamporter.createSubscriber({
  'customer_uid': 'test_uid',
  'card_number': '1234-1234-1234-1234',
  'expiry': '2021-11',
  'birth': '620201',
  'pwd_2digit': '99'
}).then(function (result) {
  console.log(result);
}).catch(function (error) {
  console.log(error);
});

// 비인증 결제를 위한 빌링키 조회
iamporter.getSubscriber('test_uid')
.then(function (result) {
  console.log(result);
}).catch(function (error) {
  console.log(error);
});

// 비인증 결제를 위한 빌링키 삭제
iamporter.deleteSubscriber('test_uid')
.then(function (result) {
  console.log(result);
}).catch(function (error) {
  console.log(error);
});

// Onetime 비인증 결제
iamporter.payOnetime({
  'merchant_uid': 'test_merchant',
  'amount': 5000,
  'card_number': '1234-1234-1234-1234',
  'expiry': '2021-12',
  'birth': '590912',
  'pwd_2digit': '11'
}).then(function (result) {
  console.log(result);
}).catch(function (error) {
  console.log(error);
});

// 비인증 결제 (빌링키 이용)
iamporter.paySubscriber({
  'customer_uid': 'test_uid',
  'merchant_uid': 'test_billing_key',
  'amount': 50000
}).then(function (result) {
  console.log(result);
}).catch(function (error) {
  console.log(error);
});

// 결제 취소 (MerchantUid 이용)
iamporter.cancelByMerchantUid(
  'test_billing_key'
).then(function (result) {
  console.log(result);
}).catch(function (error) {
  console.log(error);
});

// 결제 취소 (ImpUid 이용)
iamporter.cancelByImpUid(
  'test_imp_uid'
).then(function (result) {
  console.log(result);
}).catch(function (error) {
  console.log(error);
});

// 결제 취소
iamporter.cancel({
  'imp_uid': 'test_imp_uid',
  'amount': 2500,
  'reason': 'bad product',
  'refund_holder': '박병진',
  'refund_bank': '03',
  'refund_account': '056-076923-01-017'
}).then(function (result) {
  console.log(result);
}).catch(function (error) {
  console.log(error);
});

// 결제정보 조회 (MerchantUid 이용)
iamporter.findByMerchantUid(
  'test_billing_key'
).then(function (result) {
  console.log(result);
}).catch(function (error) {
  console.log(error);
});

// 결제정보 조회 (ImpUid 이용)
iamporter.findByImpUid(
  'test_imp_uid'
).then(function (result) {
  console.log(result);
}).catch(function (error) {
  console.log(error);
});
```


## To-do

- [x] [POST  /users/getToken](https://api.iamport.kr/#!/authenticate/getToken)
- [x] [GET   /payments/:imp_uid](https://api.iamport.kr/#!/payments/getPaymentByImpUid)
- [x] [GET   /payments/find/:merchant_uid](https://api.iamport.kr/#!/payments/getPaymentByMerchantUid)
- [ ] [GET   /payments/status/:payment_status](https://api.iamport.kr/#!/payments/getPaymentsByStatus)
- [x] [POST  /payments/cancel](https://api.iamport.kr/#!/payments/cancelPayment)
- [x] [POST  /payments/prepare](https://api.iamport.kr/#!/payments.validation/preparePayment)
- [x] [GET   /payments/prepare/:merchant_uid](https://api.iamport.kr/#!/payments.validation/getPaymentPrepareByMerchantUid)
- [x] [POST  /subscribe/payments/onetime](https://api.iamport.kr/#!/subscribe/onetime)
- [x] [POST  /subscribe/payments/foreign](https://api.iamport.kr/#!/)
- [x] [POST  /subscribe/payments/again](https://api.iamport.kr/#!/subscribe/again)
- [ ] [POST   /subscribe/payments/schedule](https://api.iamport.kr/#!/subscribe/schedule)
- [ ] [POST   /subscribe/payments/unschedule](https://api.iamport.kr/#!/subscribe/unschedule)
- [x] [DELETE /subscribe/customers/:customer_uid](https://api.iamport.kr/#!/subscribe.customer/customer_delete)
- [x] [GET    /subscribe/customers/:customer_uid](https://api.iamport.kr/#!/subscribe.customer/customer_view)
- [x] [POST   /subscribe/customers/:customer_uid](https://api.iamport.kr/#!/subscribe.customer/customer_save)


## <a name="links">Links

- [**아임포트**](http://www.iamport.kr/)
- [**아임포트 API**](https://api.iamport.kr/)
- [**아임포트 관리자**](https://admin.iamport.kr/)
- [**아임포트 매뉴얼**](http://www.iamport.kr/manual/)


## <a name="contact">Contact

If you have any questions, feel free to join me at [`#posquit0` on Freenode](irc://irc.freenode.net/posquit0) and ask away. Click [here](https://kiwiirc.com/client/irc.freenode.net/posquit0) to connect.


## <a name="license">License

- Claud D. Park <posquit0.bj@gmail.com>
- [MIT License](https://github.com/posquit0/node-iamporter/blob/master/LICENSE)
