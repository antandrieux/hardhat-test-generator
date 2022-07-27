# Hardhat Test Generator

A utility to create new unit tests files in a Hardhat project.

This provides a baseline for writing your own unit tests by listing all public (or external) functions with all their arguments. The goal is to speed up the initialization process of unit tests and not to miss any function in smart contracts.

## Install

```
npm i @antandrieux/hardhat-test-generator --save-dev
```

## Usage

First use Hardhart to compile your contracts.

```
hardhat compile
```

Generate tests for all contracts in your contract folder.

```
npx htg
```

Generate tests for a specific contract named MyContract from the file.

```
npx htg -c MyContract
```

_Replace MyContract with the name of the contract for which you want to create tests._

## How It Works

The test generator will look at the json files located in the `./build/contracts` directory for the given contract name. As long as that contract exists and has been compiled, a test file will be created for it.

Each test will include a standard template for the beginning of the file, which includes the test declaration of the contract and the definition of some common account variables.

```javascript
describe('MyContract', (accounts) => {
  const CONTRACT_NAME = 'MyContract';
```

By reading the contract json file, it will also import the contract, generate input parameters for your builder. However, it will be up to you to update the values as your contract requires.

```javascript
beforeEach(async () => {
	[owner, account1, account2, account3] = await ethers.getSigners();

	const MyContract = await ethers.getContractFactory(CONTRACT_NAME);

	// To change
	const _constructorParam_1 = 'example_value_respecting_type';
	const _constructorParam_2 = 'example_value_respecting_type';

	myContract = await MyContract.deploy(
		_constructorParam_1,
		_constructorParam_2
	);
	await myContract.deployed();
});
```

All inherited contracts with external or public functions will have a test feature (as a `describe` block) created for it, with all their arguments.

```javascript
describe('Test setValue', async () => {
	it('Should test setValue', async () => {
		let _firstParam = 'example_value_respecting_type';
		let _secondParam = 'example_value_respecting_type';
		await myContract.connect(owner).setValue(_firstParam, _secondParam);
	});
});
```

This allows you to easily have a structure for your tests and to make sure that every external/public method will be tested, provided you write a test for it.

The test file will be placed in the `test` directory of your Hardhat project and named `MyContract.test.js`, where MyContract is the name of the contract. To prevent a user from accidentally deleting an existing file, you can use the `--new` option to add a new version of the test file. Otherwise the file will be overwritten.

## FYI

1. The library ignores files that start with _I_, we consider them as interfaces and we don't want to test them.
2. The library needs the file names to have the same name as the contract names.

## Contributions & future

Any improvement is welcome. Anyone can create a issue and propose something.

In the future, we would like to improve this library with more intelligent testing by getting information and typical vulnerabilities from another library that does static analysis (e.g. Slither).

If you are interested, please contact us, and we will discuss about it.

Contact to:

- [My LinkedIn](https://www.linkedin.com/in/antoine-andrieux/)
- My Discord: AntoineAdx#1429
