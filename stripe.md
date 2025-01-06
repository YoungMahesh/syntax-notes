# [webhook](https://docs.stripe.com/webhooks)

- receive events as they occur in your Stripe accounts
  - e.g. if user completes payment on stripe, receive this event in webhook and register related details in database

## withdraw

- https://docs.stripe.com/connect/account-capabilities
- withdraw is only available through stripe-connect
- stripe connect does not support companies from multiple countries like South africa, India, etc
- People who does not have bank-account from supported countries cannot create stripe-connect account

## accept donation with bank account

- stripeInstance.checkout.sessions.create({ payment_method_types: ['card', 'us_bank_account'] })

  - error: The payment method type provided: us_bank_account is invalid. Please ensure the provided type is activated in your dashboard (https://dashboard.stripe.com/account/payments/settings)
  - No option for for us_bank_account in settings (probably because my stripe account is from India)

- stripe.accounts.create()
  - Error: The card_payments capability is not supported for this account type and this account's country (IN).
  - stripe.transfers.create() without accounts.create()
  - No such destination: '<account-id>'

## [events](https://docs.stripe.com/api/events)

- stripe uses https to send webhook events to your app as a JSON payload that includes an Event Object

## [stripe-cli](https://docs.stripe.com/stripe-cli)

- a developer tool to help you build, test and manage your integration with Stripe directly from the command line
- [install using docker](https://docs.stripe.com/stripe-cli?install-method=docker)

  - docker package to cli-command `alias stripe='docker run --rm -it stripe/stripe-cli:latest'`
  - by default, docker does not store api-key after login, fix this by passing `--api-key` with every command

- [stripe-cli-keys](https://docs.stripe.com/stripe-cli/keys)
- [stripe-api-keys](https://dashboard.stripe.com/test/apikeys)

## [triggers](https://docs.stripe.com/stripe-cli/triggers)

- triggers webhook with two ways

  1. do the actions on stripe that lead to the event you want to trigger
  2. run a command with the stripe CLI to automatically generate a event

- [stripe-listen](https://github.com/stripe/stripe-cli/wiki/listen-command)

  - connect with stripe to receive webhook events in real-time
  - forward events to your local development server to see how it reacts to events

- `stripe listen` vs `stripe trigger`
  - `trigger` creates event while `listen` listens event
