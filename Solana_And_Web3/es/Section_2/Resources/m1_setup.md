## 🍎 Setting up Solana on a M1 macOS Machine.
**First off - I want to give a HUGE shoutout to our TA, Nick! Without Nick, this guide wouldn't have been doable. Once you finish this section make sure to give some love to Nick in Discord (Nick_G#4818)**

We are going to go **from this doesn't work on M1 masOS??** to 

![ankin it's working Gif](https://media.giphy.com/media/CuMiNoTRz2bYc/giphy.gif) 

**real quick.**

This guide will get you up and running with the Solana enviroment on your local machine(shoutout to fellow builder, **@billyjacoby#7369** he mocked up the first guide about getting set up without Rosetta!) We made modifications that will get you to becoming a Solana Master faster with less headaches 🙂.

Let's kick it off!

### ⚙️ Install Rust.

In Solana, programs are written in Rust! If you don't know Rust don't worry. As long as you know some other language — you'll pick it up over the course of this project.

To install Rust we will open up our terminal and run this command:

```bash
curl --proto '=https' --tlsv1.2 https://sh.rustup.rs -sSf | sh
```

You will be prompted with multiple options to install. We are going to go with default entering 1 and then enter!

Once you're done, go ahead and restart your terminal and then verfiy it was installed by entering:

```bash
rustup --version
```

Then, make sure the rust compiler is installed:

```bash
rustc --version
```

Last, let's make sure Cargo is working as well. Cargo is the rust package manager.

```bash
cargo --version
```
As long as all those commands output a version and didn't error, you're good to go!


### 🔥Install Solana - THIS IS WHAT WE CAME FOR!

We are going to build it from it's source. What does this mean? In short, it allows us to build Solana on our computer instead of downloading a pre-built version.

We are to download with this command:

```bash
git clone https://github.com/solana-labs/solana.git/
```

Once it has finished cloning, we are going to enter the Solana directory and checkout the version branch `v1.8.2`:

```bash
cd solana
git checkout v1.8.2
```

`git checkout` is just switching to a stable version, so we can send ourselves some `$SOL` later on without receieving this error `Error: RPC response error -32601: Method not found`.

Next, we are going to run this command:

```bash
./scripts/cargo-install-all.sh .
```

<details>
<summary>Having Problems?</summary>

If you receieve an error that looks like this - `greadlink: command not found` you will need to do two things:
- Install Brew using `/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"` (this may take a while)
  
- Add brew to your path using `export PATH="/opt/homebrew/bin:$PATH"`

- Install coreutils using `brew install coreutils`

Then run the script above once more an see if it works!
If that outputs a version number, you're good to go!
</details>

This might take some time, so don't be alarmed! Once you're done installing, run this to make sure everything is in working order:

```bash
solana --version
```

Next thing you'll want to do is run these two commands separately:

```bash
solana config set --url localhost
solana config get
```

This will output something like this:

```bash
Config File: /Users/nicholas-g/.config/solana/cli/config.yml
RPC URL: http://localhost:8899
WebSocket URL: ws://localhost:8900/ (computed)
Keypair Path: /Users/nicholas-g/.config/solana/id.json 
Commitment: confirmed 
```

This means that Solana is set up to talk to our local network! When developing programs, we're going to be working w/ our local Solana network so we can quickly test stuff on our computer.
The last thing to test is we want to make sure we can get a local Solana node running. Basically, remember how we said that the Solana chain is run by "validators"? Well — we can actually set up a validator on our computer to test our programs with.

First, shoutout to **@dimfled#9450**, and send some love his way! He has 'seen things' building with Solana recently and gave us this step to get things working on M1s!

We are going to run our Solana validator manually. Will exaplin why we need this shortly:

```bash
solana-test-validator --no-bpf-jit
```


This may take a bit to get started but once it's going you should see something like this:

![Untitled](https://i.imgur.com/FUjRage.jpg)


Boom!! You're now running a local validator. Pretty cool :).



### ☕️ Install Node, NPM, and Mocha

Pretty solid chance you already have Node and NPM. When I do node --version I get v16.0.0. The minimum version is v11.0.0. If you don't have node and NPM, get it [here](https://nodejs.org/en/download/).

After that, be sure to install this thing called Mocha. It's a nice little testing framework to help us test our Solana programs.

```bash
npm install -g mocha
```

### ⚓️ The magic of Anchor


We're going to be using this tool called "Anchor" a lot. If you know about Hardhat from the world of Ethereum, it's sorta like that! Except — it's built for Solana. **Basically, it makes it really easy for us to run Solana programs locally and deploy them to the actual Solana chain when we're ready!**

Anchor is a *really early projec*t run by a few core devs. You're bound to run into a few issues. Be sure to join the [Anchor Discord](https://discord.gg/8HwmBtt2ss) and feel free to ask questions or [create an issue](https://github.com/project-serum/anchor/issues) on their Github as you run into issues. The devs are awesome. Maybe even say you're from buildspace in #general on their Discord :).

**BTW — don't just join their Discord and ask random questions expecting people to help. Try hard yourself to search the their Discord to see if anyone else has had the same question you have. Give as much info about your questions as possible. Make people want to help you lol.**

*Seriously — join that Discord, the devs are really helpful.*

To install Anchor, go ahead an run:

```bash
cargo install --git https://github.com/project-serum/anchor anchor-cli --locked
```

The above command may take a while. Once it's done, it may ask you to update you path, remember to do that.

From here run:

```bash
anchor --version
```

If you got that working, nice, you have Anchor!!

We'll also use Anchor's npm module and Solana Web3 JS — these both will help us connect our web app to our Solana program!

```bash
npm install @project-serum/anchor @solana/web3.js
```

### 🏃‍♂️ Create a test project and run it.

Okay, we're *nearly done* haha. The last thing we need to do to finalize installation is to actually run a Solana program 
locally and make sure it actually works.

Before we begin, make sure you have `yarn` installed on your machine:

```bash
brew install yarn
```

We can make the boilerplate Solana project named `myepicproject` with one easy command:

```bash
anchor init myepicproject --javascript
cd myepicproject
```

`anchor init` will create a bunch of files/folders for us. It's sort of like `create-react-app` in a way. Go ahead and open up your project directory in VSCode and take a look around!

Before we dive in, remember when we set our local validator as `solana-test-validator --no-bpf-jit`? Well, we did that because things right now are still really new with M1 Mac's and Anchor. 
Anchor actually runs it's own validator, and on the M1 it will fail to do that and throw an error like - `FetchError: request to http://localhost:8899/ failed` when you go to run `anchor test`.

The solution right now is to have Anchor run with Solana's validator instead. Pretty dope!

Okay, back to it! Let's open up a new terminal window and run:

```bash
solana-test-validator --no-bpf-jit
```

### 🔑 Create a local keypair.

In order for us to talk to our Solana programs we need to generate a keypair. Really all you need to know about this is it allows us to digitally sign for transactions in Solana! Still curious? [Take a look at this page](https://solana-labs.github.io/solana-web3.js/classes/Keypair.html) for more information!

```bash
solana-keygen new -o target/deploy/myepicproject-keypair.json
```
(Don't worry about creating a passphrase for now, just press "Enter" when asked!)

You will see this keypair in a generated `JSON` file located at `target/deploy/myepicproject-keypair.json`.

Then run this command: 

```bash
solana address -k target/deploy/myepicproject-keypair.json
```

This will return the keypair in the terminal. We are going to copy that address and open up our project in our code editor and go to `Anchor.toml` in the root of our project and paste this on line two replacing the address that is already there.
Now, we will go back over to our terminal where we got set up in our project folder and run:

```bash
anchor test --skip-local-validator
```

This may take a while the first time you run it! As long as you get the green words the bottom that say "1 passing" you're good to go!! Keep us posted in the Discord if you run into issues here.


![Untitled](https://i.imgur.com/V35KchA.png)
