<Section name="1. Cartesi’s Mission" description="Understand the goals of Cartesi">

## Cartesi’s mission

Today, a significant number of developers aspire to create applications in the Web 3.0 world but are met with a steep learning curve and a confusing landscape. [Cartesi](https://cartesi.io/) aims to solve this problem by:

* Enabling more innovation by reusing existing tooling, such as programming languages and libraries, to build more sophisticated applications at a higher pace. 
*  Growing the pull of use cases that can be put on chain.
* Making Web 3.0 accessible to developers with traditional programming backgrounds who can build DApps using a programming environment they are already familiar with. For example, you can create a DApp using Python without the need to write the application code in Solidity. When the app is deployed, a standard Cartesi smart contract will put your DApp on-chain.

In terms of the blockchain trilemma, Cartesi solves the scalability problem by using [Cartesi Optimistic Rollups](https://docs.cartesi.io/cartesi-rollups/overview/) along with the [Cartesi Machine](https://docs.cartesi.io/cartesi-machine/) to support and scale complex computations. By doing this, Cartesi allows developers to use rich code for decentralized applications and bring Web 2.0’s power and expressiveness to Web 3.0.



## Question

<Quiz id={"????"}

What part of the blockchain trilemma does Cartesi address?

A. Scalability (Correct Answer)
B. Security
C. Decentralization

## Question

<Quiz id={"????"}

What is Cartesi’s mission?

A. Enable more innovation by reusing existing tooling.
B. Grow the pull of use cases that can be put on chain.
C. Make Web 3.0 accessible to developers with traditional programming backgrounds. 
D. All of the above. (Correct Answer)

/>

<Section name="2. How Cartesi works" description="How Cartesi works">

## What is Cartesi

Cartesi Rollups is a modular execution layer that elevates simple smart contracts to decentralized Linux runtimes. By using Cartesi Rollups, each DApp will have its own execution environment,  thus avoiding the need to consume resources from other DApps. This technology paves the way for a new cohort of DApps that were previously unable to run on EVM chains. 

Cartesi ecosystem is based on a special RISC-V-based VM - the [Cartesi Machine](https://docs.cartesi.io/cartesi-machine/) - which provides a Linux environment for the development of scalable decentralized applications in traditional programming languages. Cartesi Machine is deterministic and transparent, which allows the verification of complex computations on Ethereum, therefore enabling Cartesi DApps to inherit the base blockchain security.  

In a nutshell, you can think of Cartesi as a special operating system that makes your life easier, allowing you to create computationally heavy DApps while providing you with the freedom to choose the programming languages, libraries and tools of your preference.

## Dispute resolution

Cartesi’s fraud proof mechanism is the Verification Game, which is a dispute resolution protocol that allows an arbiter with limited computational resources to referee a game between two computationally unlimited players who need to reach the same result. Optimistic Rollups are at the core of the Verification Game.

The Verification Game consists of two parts.

In the first part, we take the Cartesi machine of both parties off chain and post a signature of the state of each machine to the blockchain. In doing so, we consider the entire state of the Cartesi machine (RAM, Disc, processor etc.), map it to the host memory and divide it into 4K pages, which are then hashed to basically represent the leaves of the Merkle tree. The root hash of this Merkle tree represents the whole state, which is used to check against the state of the disputed Cartesi machine. The entire computation sequence of this Cartesi machine is used to conduct an inner search to find the first instruction where players disagree, which is detected by checking the root hash against each cycle of the sequence. The divergence is found by continuously narrowing down the search span on the computation sequence. 

Once the instruction has been found, the second part of the verification game involves sending this instruction to the on chain implementation of the Cartesi machine, which is a set of on chain contracts that can emulate each instruction of the Cartesi Machine. The on chain implementation of the Cartesi machine then executes the instruction to check its validity. In this way, we guarantee the security standards of the Ethereum blockchain.

In summary, as long as there is one honest validator, Ethereum can give the correct settlement.


## Question

<Quiz id={"????"}

What types of Rollups does Cartesi use?

A. Optimistic Rollups (Correct Answer)
B. Zero-knowledge
C. None, Cartesi is a side-chain

## Question

<Quiz id={"????"}

How does validation happen in Cartesi?

A. Local consensus by people validating the application
B. Verification game (Correct Answer)
C. By publishing validity proof per transaction

## Question

<Quiz id={"????"}

Where do the complex computations of your DApp happen?

A. Inside a deterministic transparent VM called the Cartesi Machine (Correct Answer)
B. All computations happen On-chain
C. Cartesi cannot handle heavy computations
/>

</Section>








<Section name="3. Build DApps Now!" description="Steps to run a DApp">

### DApp architecture


![img](./hla.png)


Each Cartesi DApp has two main parts:

- **[Front-end](https://docs.cartesi.io/cartesi-rollups/dapp-architecture/#front-end)**: the user facing interface, which will often provide a UI (e.g., a web application) but may also be a command line interface (e.g., a Hardhat task using Ethers, or a command line using Python).
- **[Back-end](https://docs.cartesi.io/cartesi-rollups/dapp-architecture/#back-end)**: the verifiable logic that will run inside the Cartesi Rollups infrastructure; this will store and update the application state given user input and will produce outputs in the form of [vouchers](https://docs.cartesi.io/cartesi-rollups/components/#vouchers) (transactions that can be carried out on layer-1) and [notices](https://docs.cartesi.io/cartesi-rollups/components/#notices) (information that can be validated on layer-1).

Aside from the back-end running inside the Cartesi Rollups infrastructure, the DApp front-end can, of course, also make use of external resources such as 3rd-party services. Indeed, for more complex DApps it is expected that there will be other back-ends besides the one running verifiable logic. These would be used whenever the application doesn’t really need a service to be decentralized and trustless, such as providing fast and accessible data caches, helping users communicate with each other, or interfacing with other non-blockchain services.

Conversely, it is also possible for complex DApps to provide more than one front-end application, with the goal of supporting different kinds of users and use cases.

### Hands-on: Try it out!

Let’s try locally running a small existing DApp written in Python, called Echo-Python. The Echo-Python DApp simply copies (or "echoes") each input received as a corresponding output notice. 

##### Start the application

*Before you begin, make sure that you have [Docker](https://www.docker.com/) installed on your local machine.*

1. Clone the `cartesi/rollups-examples` Github repository by running the following command:

```
git clone https://github.com/cartesi/rollups-examples.git
```

2. Navigate to the DApp example directory by running the following command:

```
cd rollups-examples/echo-python
```

3. Build the Echo DApp by running the following command:

```
docker buildx bake --load
```

4. Start the application by running the following command:

```
docker compose -f ../docker-compose.yml -f ./docker-compose.override.yml up
```

##### Interact with the application

Now that the application is up and running, let’s send a request.

1. Open a separate terminal window.

2. From the `rollups-examples` base directory, navigate to the `frontend-console` one:

```
cd frontend-console
```

3. Build the front-end console application:

```
yarn
yarn build
```

3. Send input to the current locally deployed DApp:

```
yarn start input send --payload "Hello, Cartesi."
```

4. Verify the outputs (notices) generated by your input. To display your DApp notices, run the following command:

```
yarn start notice list
```

5. After completing all the steps above, you should get a response similar to the following:

```
[ { epoch: '0', input: '1', notice: '0', payload: 'Hello, Cartesi.' } ]
```

Congratulations, you’ve successfully run a Cartesi DApp!

## Question

<Quiz id={"????"}

What does a simple Cartesi DApp consist of?

A. Solidity Smart contracts.
B. Back-end and front-end as any other traditional applications. (Correct Answer)
C. Linux OS.

/>

</Section>



<Section name="4. Developer Resources" description="Resources">

## Developer Resources

### [Documentation](https://docs.cartesi.io/)

The home for Cartesi documentation and learning for all kinds of developers, blockchain technology professionals, and researchers.

### [Run back-end in Host Mode](https://docs.cartesi.io/build-dapps/dapp-host-mode/)

When developing an application, it is often important to easily test and debug it. For that matter, it is possible to run the Cartesi Rollups environment in host mode.

### [DApp examples](https://github.com/cartesi/rollups-examples)

Explore our DApps either in our [Github](https://github.com/cartesi/rollups-examples#examples) or [Docs](https://docs.cartesi.io/build-dapps/run-dapp/#explore-our-dapps).

### [Rollups HTTP APIs](https://docs.cartesi.io/cartesi-rollups/http-api/)

In a Cartesi DApp, the front-end and back-end parts of the application communicate with each other through the Rollups framework. This is accomplished in practice by a set of HTTP APIs.

### [Rollups HTTP APIs reference](https://docs.cartesi.io/cartesi-rollups/api/)

APIs available for DApp developers to interact with the Cartesi Rollups framework.

### [StackOverflow](https://stackoverflow.com/questions/tagged/cartesi)

Feel free to ask your questions on StackOverflow or in our [Discord](https://discord.gg/Pt2NrnS) channel.

</Section>
