# Deploying the program on to Solana

This is part 2 of learning Solana. If you have landed directly here, make sure you have a good understanding of Rust by going through these quests on basics of Rust ( [https://questb.uk/category/solana](https://questb.uk/category/solana) ). This part assumes you have completed the Quest 1 ( [https://questb.uk/category/solana](https://questb.uk/category/solana) ). We will now be deploying the program that we had written in the previous quest. By the end of this Quest you should be able to deploy programs yourself, test them out and start learning how to build a UI for your application.

Now, on to building …

## Setting up Solana

First up we need to install Solana’s commandline tool on our local machine.

We recommend using a unix based commandline for this. You can install the commandline using your terminal on Mac or Linux machine. However if you’re on windows, we recommend using WSL2.

This script will automatically download and install solana for you.

`sh -c "$(curl -sSfL [https://release.solana.com/v1.7.10/install](https://release.solana.com/v1.7.10/install))"`

Once you’ve downloaded it, let’s see if it has been installed properly

`solana -V`

This should show you the same version that we had in the previous step. (v1.7.10)

Solana is a world computer. This world computer runs by many computers around the world. You can plug in your computer to this world computer over the internet. When you plug it in, you are adding one more CPU to this already powerful computer. This world computer in Solana’s jargon is called a cluster.

Instead of connecting to the world computer directly, we will run a local instance of this world computer. That way, it’s easier to test.

To do so,

`solana-test-validator`

Now let this terminal keep running. Open a new terminal for the rest of the instructions. Solana is running on one terminal, we’ll interact with it from another terminal. This is exactly similar to what we’d do with the actual Solana as well. Solana is running across machines on the internet, and we can connect to it from our terminals, browsers and apps.

## Getting some money

To do anything on Solana you need to pay money. If you want to deploy a program, you need to pay money. If you want to execute some program on this world computer, you need to pay money. The money is to be paid in the local currency of Solana, that is SOLs or Lamports. Lamport is nothing but 10^-10 SOLs. Lamport is the smallest denomination of payments on Solana.

In order to hold some SOLs, we first need to have an account. An account, as we had seen earlier, can hold

- Lamports (aka money)
- Code (aka program)
- Data

An account has a public and private key. The public key is also called the address of your account. If someone wants to send you money, this address is what they’d use to send you the money. The private key is more like a password that only you will have. You use the private key to transact _from_ your account.

So, first up let’s get an account for ourselves.

`solana-keygen new`

This creates a new account. The account itself has nothing but the public and the private key this far. It also sets this new account as the default account for all the transactions that’ll follow.

The output will show a pubkey. Keep a note of this. This is your account address.

It will also display a series of words titled seed phrase. The seed phrase is the equivalent of a private key. Make sure you store it safe somewhere. We might need it if you have to reconfigure or reinstall solana later. Remember that if you lose your private key (or seed phrase), you lose all the money in that account. There is no way to recover it. But, if you save your seedphrase carefully, you should be good. But don’t worry too much about it for now. We’ll only be playing with toy money.

Now let’s see how much money we have

`solana balance`

This shows the balance for our default account that was created in the previous step. It might have shown you the harsh reality - 0 SOL. We are SOL poor at this point, lets get some SOLs!

To get SOLs, we’ll have to first tell solana which cluster to connect to, to request money.

To do so,

`solana config set --url [http://127.0.0.1:8899](http://127.0.0.1:8899) `

By doing so, we’ve told solana to connect to the Solana instance running on our other terminal.

And since we’re on a toy cluster, we can request for toy money. We can do so by

` solana airdrop 10`

This will get you 10 more SOL into your account

You can check your latest balance by

`solana balance`

Great, now that you have non zero balance in your account, we can go ahead and do interesting things like deploying the code that we had written in the previous quest.

## Getting ready to deploy the program

First we need to compile our program. To do so, first copy the file you’ve written to `src/lib.rs`

Then copy the following file into a file called Cargo.toml ; this is where we tell rust what all libraries we’re going to be using. For now you can copy paste as is.

[https://gist.github.com/madhavanmalolan/1a88f711fb80632f018dd20ce8338bc8](https://gist.github.com/madhavanmalolan/1a88f711fb80632f018dd20ce8338bc8)

This is what your directory should look like :

<p id="gdcalert1" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image1.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert2">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>

![alt_text](learn_src/learn_assets/1.png 'image_tooltip')

Now let’s compile the program

`cargo build-bpf --manifest-path=./Cargo.toml `

This will create a deployable solana file at `targets/deploy/crowdsource.so`

Now you can deploy this compiled program using

`solana program deploy targets/deploy/crowdsource.so`

In a couple of seconds, it will get deployed and display the program id on the console.

## Up next …

Great, now you have an account, you have a program that is deployed to Solana - albeit a local version. This local deployment is as good as the steps that you need to follow to deploy on the world computer. We will do that again in just a bit.

But for now, we’ll first write some code to interact with the program we’ve just written!
