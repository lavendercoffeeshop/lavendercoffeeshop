# Requirement
- VirtualBox 5.1.x
- Vagrant >= 1.9.0
- PHP 5
- Composer

# Setup
1. Pull source
   - `git clone git@github.com:VLMH/coffee-shop.git`
2. Go to app folder
   - `cd coffee-shop`
3. Install dependencies
   - `composer install`
4. Generate `Vagrantfile` and `Homestead.yaml`
   - `php vendor/bin/homestead make`
5. Copy `.env` file
   - `cp .env.example .env`
6. Generate key
   - `php artisan key:generate`
6. Start VM
   - `vagrant up`
7. SSH to VM
   - `vagrant ssh`
8. Create DB
   - `createdb -U homestead -h 127.0.0.1 -O homestead coffeeshop`
   - password: `secret`
9. Run migration
   - `cd ./Code/coffee-shop && php artisan migrate`
10. Set `/etc/hosts`
   - `coffee-shop.app 192.168.10.10`
11. Go to app http://coffee-shop.app

# Public link
- Create payment http://coffee-shop.app
- Search payment http://coffee-shop.app/payments

# PayPal test credit card
- AMEX `347149799668709` 04/2022
- VISA `4032035073037590` 04/2022
- MASTERCARD `5110921578761093` 04/2022


# Braintree test credit card numbers
https://developers.braintreepayments.com/reference/general/testing/php#credit-card-numbers

# Testing
Run test with `cd path/to/app && vendor/bin/phpunit tests`

# Data structure
- Order
  - Saving order section info
  - Saving payment reference code after payment succeeded
  - polymophic associate with Paypal payment and Braintree payment
- Paypal Payment
  - Saving Paypal specific payment info
  - p.s. currently no info is saved
- Braintree Payment
  - Saving Braintree specific payment info such as nonce

# Cache
- Order with payment reference code will be stored in Redis after payment succeeded
- User can retrieve payment info with customer name and payment reference code
- Cache will last for 5mins
- When user retrieve payment info after cache expired, system will rebuild cache for the payment info
