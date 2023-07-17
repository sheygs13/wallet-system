### Wallet-System

The Wallet System Service is an API that mocks a basic wallet system. The API provides information about various aspects of wallet systems such as User Authentication/Role-Based Authorization, Paystack API Gateway integration for wallet funding, Wallet Creation, Wallet Balance Retrieval, Wallet Funds Transfer and Transaction History Summary.

#### Features

- User Authentication/Role-Based Authorization
- User Wallets Creation
- Wallet Account Crediting/Funding via Paystack Payment Gateway
- Wallet Funds Transfer & Approval
- Wallet Balance Retrival
- Wallet Transaction History

### High Level Implementation Details

The following steps were followed in the implementation of a wallet system with minimalistic features leveraging on the Paystack API Gateway:

1. PayStack Account Setup: Visit the Paystack website (`https://paystack.com`) and sign up for an account. Then obtain the API keys from the `developer section` of the the aforementioned website. Paystack provides both test and production keys for development and testing purposes. However, in most cases, one'd be utilizing the test keys.

2. Paystack API Integration.

3. Create Wallet Functionality: Within the application, the necessary logic and database structure were implemented to manage user wallets. This involved creating a wallet table, associating wallets with the user accounts, defining actions such as deposit, withdrawal amongst others.

4. User Authentication/Role-Based (Admin) Authorization: A user authentication system was implemented to secure wallet functionality. This includes features like user registration, user login, JWT token authentication, API endpoints protection.

5. Funds Deposit to Wallets: The Paystack API was used to create a payment request or payment authorization link for crediting funds into a user's wallet. This involved setting the necessary details to Paystack like including amount, customer details, and callback URL to handle payment response.

6. Payments Verification: After a payment is made, Paystack will send a `callback` to your specified URL i.e. `http://example.com` either set on the paystack developer dashboard or the one specified in your implementation. Once the notification has been processed, verify the transaction status and update the user's wallet accordingly.

7. Wallet Withdrawal Functionality: This was implemented from the scratch - point `3`. For every `deposit` or `transfer` request initiated regardless of the `transfer_status`, a request was logged to the `wallet_transactions` to keep track of the wallet transactions.

8. Balance Retrieval & Transaction History: Endpoints were implemented to retrieve user's wallets balances' & transaction history. This involves querying from the `wallet_transactions` and `wallets` tables to spool the results data sets.

### Project Structure

Overall, the project is designed to be scalable, maintainable and extensible. The use of a monolithic architecture that can easily spin off to a micro-service following modular architecture pattern that promotes code organization and separation of concerns.

#### Development Tools

- NodeJS
- [NestJS](https://docs.nestjs.com/)
- [PostgreSQL](https://www.postgresql.org/download/)
- [Typeorm](https://typeorm.io/)
- [Jest](https://jestjs.io/)
- [Paystack](https://paystack.com/)

#### Requirements

- [NodeJS](https://nodejs.org/en/download/)
- [Postman](https://www.postman.com/downloads/)
- [Git](https://git-scm.com/downloads)
- [Paystack API Secret Key](https://support.paystack.com/hc/en-us/articles/360011508199-How-do-I-generate-new-API-keys-#:~:text=How%20do%20I%20get%20a,'Generate%20new%20secret%20key'.)

#### Rename _.env.test_ to _.env_ and replace the placeholders

```bash
PORT=4000
NODE_ENV=development
PAYSTACK_SECRET_KEY=XXXX
PAYSTACK_API_BASE_URL=https://api.paystack.co
POSTGRES_HOST=localhost
POSTGRES_PORT=5432
POSTGRES_USER=XXXX
POSTGRES_PASSWORD=XXXX
POSTGRES_DB=postgres
DB_TYPE=postgres
MININUM_APPROVAL_AMOUNT=1000000
```

#### Installation 📦

```bash
   $ git clone https://github.com/sheygs13/wallet-system.git
   $ cd cd wallet-system
   $ yarn install
```

#### Development

```bash
   $ yarn run start:dev
```

#### Running the Service

#### Docker

- Install [Docker](https://www.docker.com/)
- Run `docker-compose up -d db`. But If you like to see those clumsy logs 😬, RUN `docker-compose up`
- Open browser and visit `http://localhost:4000` and rock it

#### Without Docker

- Replace database_url in .env with your corresponding valid database url 👌
- Run `yarn install` to install project dependencies
- Run `yarn start:dev` to run the services and you are good
- Open browser and visit `http://localhost:4000` and rock it

### Production Packaging

- RUN `yarn run start:prod` to start the production build

```
docker build -t ${IMAGETAG} -f Dockerfile .
```

#### Test

```bash
   $ yarn run test
```

#### Available Endpoints

`/api/v1/`

| method | route        | description   |
| ------ | ------------ | ------------- |
| POST   | /auth/signup | Register User |
| POST   | /auth/login  | Login User    |

`/api/v1/wallets`

| method | route                 | description        |
| ------ | --------------------- | ------------------ |
| POST   | /                     | Create Wallet      |
| GET    | /:`wallet_id`/balance | Get Wallet Balance |
| POST   | /initialize-payment   | Initialize Payment |
| POST   | /deposit              | Fund Payment       |

`/api/v1/transfers`

| method | route                   | description            |
| ------ | ----------------------- | ---------------------- |
| POST   | /                       | Create Wallet Transfer |
| PATCH  | /:`transfer_id`/approve | Approve Transfer       |

`/api/v1/wallet-transactions`

| method | route    | description             | Query Parameters - Optional                            |
| ------ | -------- | ----------------------- | ------------------------------------------------------ |
| GET    | /history | Get Transaction Summary | `from_date`, `to_date`, `target_month` , `target_year` |

#### Postman Documentation

- Please see `/postman_docs` on the root directory.

### Improvement Points

- Implement a Notification process (email or mobile notification) when an automated deposit fails due to insufficient funds.
- Implement Phone Number verification using third-party providers e.g. Twilio API
