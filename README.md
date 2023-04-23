# Aquadactal Smart Contract

The Aquadactal Smart Contract is an EOSIO-based smart contract designed for the Aquadactal governance system. It manages the election and ranking process, member agreements, token issuance and distribution, and the EOS reward mechanism.

## Table of Contents

- [Installation](#installation)
- [Usage](#usage)
- [Actions](#actions)
  - [electdeleg](#electdeleg)
  - [setagreement](#setagreement)
  - [sign](#sign)
  - [unsign](#unsign)
  - [retire](#retire)
  - [transfer](#transfer)
  - [open](#open)
  - [close](#close)
  - [eosrewardamt](#eosrewardamt)
  - [startelect](#startelect)
  - [submitcons](#submitcons)
  - [issuerez](#issuerez)
  - [send](#send)

## Installation

1. Set up an EOSIO development environment by following the [official guide](https://developers.eos.io/manuals/eos/latest/install/build-from-source).
2. Clone this repository.
3. Build the smart contract using the EOSIO CDT.
4. Deploy the smart contract to an EOSIO blockchain.

## Usage

After deploying the smart contract to an EOSIO blockchain, you can interact with it using the available actions described in the following section.

# Actions

## Action Overview

electdeleg: This action allows an elector to vote for a delegate in a specific group during an election. The elector and delegate must have valid accounts, and the election must be ongoing.

setagreement: This action sets the community's agreement version and content. It can only be executed by the contract owner.

sign: This action allows a user to sign the current agreement. Users can only sign the agreement once.

unsign: This action allows a user to remove their signature from the agreement.

retire: This action reduces the token supply by the specified quantity. It can only be executed by the contract owner.

transfer: This action transfers tokens from the contract owner to another account. The recipient must have a valid account.

open: This action creates a new account for a specified user, with a specific token symbol. The account must not already exist.

close: This action closes a user's account with a specific token symbol. The account balance must be zero.

eosrewardamt: This action sets the reward amount and Fibonacci sequence offset for the EOS reward distribution. It can only be executed by the contract owner.

startelect: This action starts a new election and calculates the average respect score for all members, then selects the top six members as leaders. This action can only be executed by an admin.

submitcons: This action allows a user to submit their rankings for a specific group. Each ranking must have a valid account.

issuerez: This action issues tokens to a specified account.

send: This action sends tokens from the contract owner to a specified account.

submitranks: This action sets the rankings for all groups in the election. This action can only be executed by the contract owner.

issue: This action issues new tokens to the contract owner.

## Action Details

Here is the entire markdown for the README.MD file with explanations for each action using `code` syntax:

# EDEN Fractal Contract

This is a smart contract for the EDEN Fractal project on the EOS blockchain. It manages the delegation process, token transfers, and other related functionalities.

## Actions

### startelect
Initiates the election process.
`ACTION startelect();`

### submitcons
Submits the rankings for a specific group.
`ACTION submitcons(const uint64_t &groupnr, const std::vector<name> &rankings, const name &submitter);`

### electdeleg
Elects a delegate for a specific group.
`ACTION electdeleg(const name &elector, const name &delegate, const uint64_t &groupnr);`

### transfer
Transfers tokens from one account to another.
`ACTION transfer(const name &from, const name &to, const asset &quantity, const string &memo);`

### issue
Issues new tokens to a specified account.
`ACTION issue(const name &to, const asset &quantity, const string &memo);`

### submitranks
Submits the rankings for all members of one group.
`ACTION submitranks(const AllRankings &ranks);`

### setagreement
Sets the agreement text for the contract.
`ACTION setagreement(const std::string &agreement);`

### sign
Signs the agreement by a specified user.
`ACTION sign(const name &signer);`

### unsign
Removes the agreement signature by a specified user.
`ACTION unsign(const name &signer);`

### retire
Retires tokens from circulation.
`ACTION retire(const asset &quantity, const string &memo);`

### open
Opens a new account for a specified user.
`ACTION open(const name &owner, const symbol &symbol, const name &ram_payer);`

### close
Closes an account for a specified user.
`ACTION close(const name &owner, const symbol &symbol);`

### eosrewardamt
Sets the EOS reward amount and the Fibonacci offset.
`ACTION eosrewardamt(const asset &quantity, const uint8_t &offset);`

## Data Structures

The contract uses several tables and data structures to manage its state. These include:

- delegates
- agreementx
- signaturex
- rewardconfgx
- orderlead
- permission_level_weight
- wait_weight
- key_weight
- authority
- GroupRanking
- AllRankings
- consensus
- electioninfx
- currency_stats
- account
- avgbalance
- memberz

## Data Structures Explained

### Delegates
TABLE delegates {
uint64_t groupNr;
eosio::name elector;
eosio::name delegate;
uint64_t primary_key() const { return elector.value; }
};

### Agreement
TABLE agreementx {
std::string agreement;
uint8_t versionNr;
};

### Signature
TABLE signaturex {
eosio::name signer;
uint64_t primary_key() const { return signer.value; }
};

### Reward Configuration
TABLE rewardconfgx {
int64_t eos_reward_amt;
uint8_t fib_offset;
};

### Order Leader
TABLE orderlead {
name leader;
uint64_t primary_key() const { return leader.value; }
};

### Consensus
TABLE consensus {
std::vectoreosio::name rankings;
uint64_t groupnr;
eosio::name submitter;
uint64_t primary_key() const { return submitter.value; }
uint64_t by_secondary() const { return groupnr; }
};

### Election Information
TABLE electioninfx {
uint64_t electionnr = 0;
eosio::time_point_sec starttime;
};

### Currency Stats
TABLE currency_stats {
asset supply;
asset max_supply;
name issuer;
uint64_t primary_key() const { return supply.symbol.code().raw(); }
};

### Account
TABLE account {
asset balance;
uint64_t primary_key() const { return balance.symbol.code().raw(); }
};

### Average Balance
TABLE avgbalance {
name user;
uint64_t balance;
uint64_t primary_key() const { return user.value; }
uint64_t by_secondary() const { return balance; }
};

### Members
TABLE memberz {
name user;
vector<uint64_t> period_rezpect;
uint8_t meeting_counter;
uint64_t primary_key() const { return user.value; }
};
