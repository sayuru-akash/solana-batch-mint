# SOLANA BATCH MINT SCRIPT

## Overview
Script to setup, upload and batch (bulk) mint all the NFTs at once on Solana blockchain using Metaplex's candy machine. The script will build up everything from scratch, except for your images, of course! Any required packages or modules will be automatically installed if not found on the system so you don't have to worry about anything excpet creating awesome images.

## Special Thanks
Special thanks to <a href="https://github.com/jrod091/">jrod091</a> for the <a href="https://github.com/jrod091/candy-machine-setup">base repository</a> which I grabbed the base code and did some changes to develop this new script.

And yes, this script supports the candy machine v2 update!

## Usage
```
Usage: candy_machine_setup <command> <options>

Commands:
   initial_mint   setup candy machine and mint images
   mint_more      run candy machine to mint more images

Options:
   -h, --help              print this help message

You can run 'candy_machine_setup <command> -h/--help to see help for specific command.
```

- `-h` or `--help`: This flag is used to print the usage of the script

### Initial Mint
The `initial_mint` command will ensure that your environment is properly setup with all the modules and libraries required to mint images. It will also mint your initial set of images.

```
Usage: candy_machine_setup initial_mint <options>

Options:
   -d, --devnet            set 'devnet' as network
   -h, --help              print command line options
   -m, --mainnet           set 'mainnet' as network
   -n, --newwallet         setup new wallet locally
   -w {ID}, --wallet={ID}  set public wallet key address from Phantom or other source
```

- `-d` or `--devnet`: This flag is used to choose the `devnet` network while implementing the solution. This allows you to run in a test environment without real money.
- `-h` or `--help`: This flag is used to print the usage of this command
- `-m` or `--mainnet`: This flag is used to choose the `mainnet-beta` network while implementing this solution. This means everything will be live, with real money.
- `-n` or `-newwallet`: If you do _not_ have a SOL wallet already, this will create one locally on the server
- `-w {ID}` or `--wallet={ID}`: IF you _do_ have a SOL wallet, use this flag along with passing the public address (`{ID}`)

#### Notes
- You need to pass either `-d` or `-m` as an option to choose the network you want to run in. The script will error out if you do not and remind you to choose a network.
- You need to pass either `-n` or `-w {ID}` as an option for a wallet. The script will error out if you do not and remind you to choose a wallet option.

### Mint More Images
The `mint_more` commmand will allow you to upload more images and mint those as well, assuming the environment is properly setup already.

```
Usage: candy_machine_setup mint_more <options>

Options:
   -d, --devnet            set 'devnet' as network
   -h, --help              print command line options
   -m, --mainnet           set 'mainnet' as network
```

- `-d` or `--devnet`: This flag is used to choose the `devnet` network while implementing the solution. This allows you to run in a test environment without real money.
- `-h` or `--help`: This flag is used to print the usage of this command
- `-m` or `--mainnet`: This flag is used to choose the `mainnet-beta` network while implementing this solution. This means everything will be live, with real money.

#### Notes
- You need to pass either `-d` or `-m` as an option to choose the network you want to run in. The script will error out if you do not and remind you to choose a network.
- Everytime you add more images, you must start from {0.png - 0.json} combination (this means replacing all png/json combos with new set).

## Image Setup
The script will pause for you to get your images in the proper directory with associated JSON files before continuing. The below are things you should keep in mind when adding your images.
- Make sure to replace the assets folder with your assets. You can find out more about the metadata layout by 0.json (sample) metadata file. Also, you can find out more about assets and metadata in this <a href="https://docs.metaplex.com/candy-machine-v2/preparing-assets">documentation.</a>
- The script creates an `assets` directory in the same directory that the script runs from. This is the directory you should drop all PNG and JSON files for minting.
- Each PNG file requires a corresponding JSON file.
- Filenames are important. The files need to named as simply a number, starting at `0`. For example, if you have 3 images, you need to have the following files in the `assests` directory:
```
0.png
0.json
1.png
1.json
2.png
2.json
```
- The JSON files need to be configured in accordance with the [NFT standard](https://docs.metaplex.com/nft-standard). This repository has an `example.json` file to try to minimize the effort required to meet the standard. You need to replace each of the `__PLACEHOLDER__`s found in the JSON file with the appropriate information, as detailed below:
   -  Line 2: __"name": "\_\_PLACEHOLDER\_\_"__
      - This is the name for the NFT, it can be anything you want
         - For example, `"name": "NFT name"
   - Line 5: __"seller_fee_basis_points": \_\_PLACEHOLDER\_\___
      - This is the royalty rate for each sell of the NFT. This is on a scale from 0-10000
         - For example, replacing with `"seller_free_basis_points: 250"` would be a 2.5% royalty rate
   - Lines 9-11
      - These are the attributes for an NFT. You need to replace the following:
         - __"trait_type"__ with the name of the trait
         - __"value"__ with the value for that trait
      - You may add additional attributes by adding more code blocks in the form of:\
           ```
           {
              "trait_type": "TRAIT NAME",
              "value": "TRAIT VALUE"
           }
           ```
      - each additonal attribute will need to be seperated with a comma, ___EXCEPT THE LAST ONE___, for example, for three traits, it would be:\
              
              {
                 "trait_type": "hair color",
                  "value": "green"
              },
              {
                 "trait_type": "eye color",
                  "value": "brown"
              },
              {
                 "trait_type": "eyewear",
                  "value": "sunglasses"
              }
   - Line 14: __"name": "\_\_PLACEHOLDER\_\_"__
      - This is the name of your collection, it can be anything you want
         - For example, `"name": "Collection Name"`
   - Line 15: __"family": "\_\_PLACEHOLDER\_\_"__
      - The family holds multiple collections, this is simply the name of the family, it can be anything you want
         - For example, `"family": "Family Name"`
   - Lines 27-28
      - __"address": "\_\_PLACEHOLDER\_\_"__
         - This is the wallet address of the creator for payments
      - __"share": \_\_PLACEHOLDER\_\___
         - This is the percent share to be distributed to this wallet, for example `"share": 100` would be 100% distributed to the wallet above it
         - If you want to have multiple creators, you will have multiple code blocks with the same fields. For example, for 2 creators with a 50/50 split:\

               {
                  "address": "WALLET 1",
                  "share": 50
               },
               {
                  "address": "WALLET 2",
                  "share": 50
               }
            - ** Notice the comma between code blocks, similar to the attributes section above **

## Note
If you are sure that the required scripts are already installed and still experience script not found errors you can simply comment line 173 and 185 with "#"s and proceed. Any contributions to the script are also welcome!
You can identify the required scripts in the code itself.

## Examples
#### run initial configuration and mint
- `./sol-batch-mint initial_mint -d -n`
- `./sol-batch-mint initial_mint --mainnet --wallet={ID}`
##### Mac OS
- `bash sol-batch-mint initial_mint -d -n`
- `bash sol-batch-mint initial_mint --mainnet --wallet={ID}`

#### mint more images
- `./sol-batch-mint mint_more -m`
- `./sol-batch-mint mint_more --devnet`
##### Mac OS
- `bash sol-batch-mint mint_more -m`
- `bash sol-batch-mint mint_more --devnet`