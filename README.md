# MintedWithLovelace-scripts
### Action Scripts for extending MintedWithLovelace DApp and applying advanced asset/policy/holdings rule-based filtering while minting.

For example, apply a minting discount/tax, change default NFT-qty-per-mintTX, or change NFT-qty-total-per-campaign based on their current holdings, with refined filtering of who to apply the rule to, filter against multiple assets/policies/quantities and utilize evaluators such as equals, greater than, less than, greater or equaal, less or equal, and any qty...and check against 1 or many with OR or AND evaluations. 

An Action Script for Minted is a CSV file which follows the syntax and structure outlined in this document for providing custom filtering actions over payments received to an associated Mint campaign.

The CSV file acts as a simple formatted line-by-line "script" which is acted upon in descending order of the document lines.  Filters are applied for filter values entered only, e.g. if you don't want to change the total NFTs allowed from a number you already set as the default for the campaign, you would just not enter a value after the last comma. Note: if a value is left blank, you must still include the comma, if any, which would follow that value...in other words, each line should contain the same number of commas so the DApp can properly understand where the columns and rows are.

## Script Structure and Formatting
An Action Script for Minted has 5 columns and optionally the first row can act as headings for these columns,
```
apply_to,trigger,filter_price,filter_nft_per,filter_nft_total
```
with each line acting as a "Rule" which will alter one or more of 3 things for an individual payment incoming to a conditionally matched payer, either any payer, a specific wallet or several wallets, or a specific addres or several addresses: Price, NFT-per-Mint/Payment, NFTs-Total-Allowed

Following are details on the structure, syntax, and formatting of an Action Script for use with the Minted DApp. Each column is listed with the acceptible or expected data structure. 

#### "apply_to"

**Purpose:** This indicates what sender address or stake key will invoke a particular script filter/action line entry.
**Data Types/Structure:** 
  - "any"
    ...> Apply to any payer
    
  - "addr..."
    ...> Apply to all addresses associated with this payer's stake-key
    
  - "1_addr..."
    ...> Apply to just *this* payer's inbound address, not other stake-key associated wallet addresses.
**Syntax Single Entry:** Example apply to any: "any"; Example apply to just address ABC: "1_addrABC"
**Syntax Multi-Entry:** Apply to address ABC and wallet associated with DEF and GHI: "1_addrABC|addrDEF|addrGHI"

#### "trigger"

**Purpose:** This is the rule or trigger which will activate the filter(s) for this line entry.
**Data Types/Structure:**
  - Each entry for comparison has two required elements and a third optional
    1. Item Location - "own"

    ...NOTE ~ Presently this is the only supported option, "tx" will be available soon which indicates to look at any asset included in the incoming payment.
    2. Item ID - PolicyID/PolicyIDAssetHexName
    
    ...NOTE ~ Can be either a Policy ID or a concatenated Policy ID + Hex Name, e.g. policy 7c76fc6c7496acb811ca913b3f9f1a3e4a875d7d1f1b11b9676840ce + hex name 4a554445 concatenate as 7c76fc6c7496acb811ca913b3f9f1a3e4a875d7d1f1b11b9676840ce4a554445
    3. Item Comparison + Qty *Optional* - A comparison symbol concatenated with the quantity to evaluate for
    
    ...NOTE ~ Supported comparisons: >, <, >=, <=, ==; Quantity to evaluate for should be concatenated with the compairson symbols (no spaces), e.g. <=100
    ...* Optional - if ommitted: do not include a trailing pipe and the evaluation is "any qty"
**Syntax Single Entry Examples:** If entering a single comparison rule, it's entered as a single with the appropriate pipes, see examples below.
  - Example: "payer holds exactly 3 of any asset with policy ID ABC" = "own|policyID_ABC|==3"
  - Example: "payer holds any qty of asset with policy ID ABC" = "own|policyID_ABC"
  - Example: "payer holds at least 5 of asset with policy ID ABC and hex name 123" = "own|policyID_ABChexName123|>=5"
**Syntax Multi-Entry Examples:** If applying more than 1 possible conditional rule, use the same formatting for each rule as above and separate these rules based on either applying "or" or "and" comparison logic as follows:
  - Example: To evaluate for OR use "||", e.g. itemAlocation|itemApolicyID/asset|itemAcompareQty*||itemBlocation|itemBpolicyID/asset|itemBcompareQty*...etc
  - Example: To evaluate for AND use "+", e.g. itemAlocation|itemApolicyID/asset|itemAcompareQty*+itemBlocation|itemBpolicyID/asset|itemBcompareQty*...etc

#### filter_price

**Purpose:** Apply a variant per-NFT price if the rules apply for this line entry.
**Data Types/Structure:** Price in lovelace to apply for this rule (or blank to leave the default in tact)
**Syntax Entry Examples:**
  - Example: "apply a 5 ADA discount when default per-NFT price is 50 ADA" = "45000000"
  - Example: "leave price per-NFT unchanged" = ""

#### filter_nft_per

**Purpose:** Apply a variant NFT-per-payment allowance if the rules apply for this line entry.
**Data Types/Structure:** Number representing quantity of NFTs to allow in a single mint, to apply for this rule (or blank to leave the default in tact)
**Syntax Entry Examples:**
  - Example: "allow qualified payer to buy max of 8 NFTs per mint when default nft-per-tx is 5" = "8"
  - Example: "leave nft-per-tx unchanged" = ""

#### filter_nft_total

**Purpose:** Apply a variant NFTs-Total allowance if the rules apply for this line entry.
**Data Types/Structure:** Number representing quantity of NFTs to allow in total for the campaign, to apply for this rule (or blank to leave the default in tact)
**Syntax Entry Examples:**
  - Example: "allow qualified payer to buy a total of 20 NFTs" = "20"
  - Example: "leave total-nfts-allowed unchanged" = ""

## Final Notes

- After your CSV file is prepared, you may import it during the configuration or reconfig of a compaign.
- Your imported Action Script is processed directly after processing any whitelist or blacklist which may be configured for your campaign. 
- If you need any support in properly formatting your Action Script file, please connect with us on the [official MintedWithLovelace Discord server](https://discord.gg/HzKvRWPqy5).


