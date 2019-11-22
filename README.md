# React Native Detox Cucumber Integration

This integration is mainly tested against android. But there shouldn't be any problem with ios also.

## How this works



## Requirements

* Make sure you have Xcode or Android Studio installed based on your requirements
* make sure you have node installed (`brew install node`, node 8.3.0 and up is required for native async-await support, otherwise you'll have to babel the tests).
* Make sure you have react-native dependencies installed:
   * react-native-cli is installed (`npm install -g react-native-cli`)
   * watchman is installed (`brew install watchman`)

### Step 0: 
Before starting out with Cucumber, please be sure to go over the [Getting Started](Introduction.GettingStarted.md) guide from Detox.

### Step 1: Adding dependencies

* Make sure you're inside your git cloned folder `react-native-detox-cucumber`.
* Run `yarn`. This will take care of installing detox and cucumber.

## To test Release build of your app
### Step 2: Build 
* Build the project
 
 ```sh
 detox build --configuration android.emu.release
 ```
 
### Step 3: Test 
* Run tests on the project via cucumber command.
 
 ```sh
 node_modules/.bin/cucumber-js ./e2e/features --require-module @babel/register --configuration android.emu.release
 ```

 This action will open a new simulator and run the tests on it.

 In cucumber, since we write the test steps in plain [gherkin](https://cucumber.io/docs/gherkin/reference/) syntax, we need to start the test via cucumber command which will understand the gherkin syntax and execute them.

 Any arguments which you would have wanted to pass it to detox directly, you can continue to pass it as an argument to `cucumber-js` like how we have passed `--configuration` and detox will read it internally.

 #### How this actually works under the hood is,

 If you had seen the default `mocha` setup that comes via `detox init -r mocha`, you would have noticed a `init.js` file where the actual Detox initialisation and cleanup happens via the hooks that mocha provides. Same needs to be done for Cucumber also via the hooks provided by Cucumber.

```js
const detox = require('detox');
const { BeforeAll, AfterAll } = require('cucumber');
const config = require('path-to-pacjage.json').detox;

BeforeAll(async () => {
  await detox.init(config);
})

AfterAll(async () => {
  await detox.cleanup();
});
```

You can notice that same being done in `e2e/features/support/init.js` in this repository. So when you execute the `cucumber` command, cucumber hooks gets called and detox initialisation happens and then cucumber starts executing every scenarios written in its feature file.


## To test Debug build of your app
### Step 2: Build 
* Build the demo project
 
 ```sh
 detox build --configuration android.emu.debug
 ```
 
### Step 3: Test 

 * start react-native packager
 
  ```sh
 npm run start
 ```
 * Run tests on the demo project
 
 ```sh
 node_modules/.bin/cucumber-js ./e2e/features --require-module @babel/register --configuration android.emu.debug
 ```
 This action will open a new simulator and run the tests on it.