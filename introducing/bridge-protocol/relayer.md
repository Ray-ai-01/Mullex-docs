# Relayer

Typical Workflow：\
\
1Administrator initialization process

2Enable supported chains and tokens

3User calls cash\_out to transfer assets

4Repeater listens for events and performs operations on the target chain

5Repeater calls payout to complete asset transfer on target chain



The coexistence of multiple chains is the current and future landscape of the blockchain world. Asset transfers between different chains have made cross-chain protocols a fundamental necessity for blockchain users. Currently, cross-chain transactions are primarily facilitated by cross-chain bridges, which establish a pathway between two blockchains. Based on message communication, these bridges enable the minting of a 1:1 pegged representation of the asset on the destination chain.



The cross chain of  Relay Chain  is realized by the cross-chain bridge listening to the origin chain and the target chain. Assets will not be locked in the cross-chain bridge during the cross-chain process. And thanks to the state synchronization mechanism used in the cross-chain process, the historical data of assets can be completely preserved. The detailed cross chain process is as follows:

Suppose the origin chain is chain A, the target chain is chain B, and the cross-chain Token is X.

* The dapp developer deploys the X contract on chain A and chain B respectively;
* The dapp developer needs to submit the names of the two chains, the contract addresses, and the  project name to  Relay Chain .  Relay Chain  sets the above information into the cross-chain system of chain A and chain B and provides the dapp developer with the cross-chain contract address;X
* The dapp user initiates a cross-chain request of X to the cross-chain contract of chain A, and authorizes Relay Chain to lock X in the cross-chain contract of chain A;
* The  Relay Chain  nodes detect the lock event;
* The node that detects the lock event initiates a cross-chain consensus and starts the consensus through TSS-LIB. The node conducts verification and TSS signature first, and then broadcasts to other  Relay Chain  nodes;
* Other nodes decide whether to confirm the cross-chain request based on the information they have detected, and if so, conduct TSS signature;
* If the number of TSS signatures exceeds the threshold, a consensus is reached;
* &#x20;Relay Chain  adds the consensus data into the cross-chain information chart;
* &#x20;Relay Chain  sends a consensus-successful cross-chain request to chain B;
* The NFT contract on chain B verifies the validity and legitimacy of the TSS group signature;
* Upon a successful verification, chain B transfers X from the cross-chain contract to the dapp user, and the cross-chain process is completed.
