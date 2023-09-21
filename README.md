# Polkadot-Rust-Substrate-Bootcamp-Final-Case
Overview
Github repository for the Polkadot Rust & Substrate Bootcamp on the following four hands on projects; 
1. Building a blockchain.
2. Simulating a substrate network.
3. Adding trusted nodes to a network.
8. Smart contracts.


1.BUILDING BLOCKCHAIN

  Node started with the following command:
  ./target/release/node-template --dev
  
  ![Step 1 Node Started](https://github.com/abarslan/Polkadot-Rust-Substrate-Bootcamp-Final-Case/assets/93275022/80b58f08-3661-4637-b166-60074b15390f)
  
  Front end started:
  
  ![Step 1 Frontend Working](https://github.com/abarslan/Polkadot-Rust-Substrate-Bootcamp-Final-Case/assets/93275022/ac0cc2de-d649-46c2-bd23-3ca43b3739f4)

2.SIMULATING A SUBSTRATE NETWORK

  Start the local blockchain node using the “alice” account by running the following command - 

  ./target/release/node-template 
  --base-path /tmp/alice 
  --chain local 
  --alice 
  --port 30333 
  --ws-port 9945 
  --rpc-port 9933 
  --node-key 0000000000000000000000000000000000000000000000000000000000000001 
  --telemetry-url "wss://telemetry.polkadot.io/submit/ 0" 
  --validator
  
  ![Step 2 Alice Node](https://github.com/abarslan/Polkadot-Rust-Substrate-Bootcamp-Final-Case/assets/93275022/423fec98-f9a0-4cfa-9604-d70dd1c4a136)

  Now we will start the new node but using “bob” account (just like we used “alice” account last time).

  Run this command - 

  ./target/release/node-template 
  --base-path /tmp/bob 
  --chain local 
  --bob 
  --port 30334 
  --ws-port 9946 
  --rpc-port 9934 
  --telemetry-url "wss://telemetry.polkadot.io/submit/ 0" 
  --validator 
  --bootnodes /ip4/127.0.0.1/tcp/30333/p2p/12D3KooWEyoppNCUx8Yx66oV9fJnriXwCcXwDDUA2kj6vnc6iDEp
   
  ![Step 2 bob node](https://github.com/abarslan/Polkadot-Rust-Substrate-Bootcamp-Final-Case/assets/93275022/71e682be-2bf4-4d69-ba68-2da56e8eef6e)

  You will see that node identity was discovered and the node has 1 peer where peer is the other node that we have started.
  The nodes will also be producing blocks and finalizing blocks and you will see messages to indicate the same.

  ![Step 2 Nods Met](https://github.com/abarslan/Polkadot-Rust-Substrate-Bootcamp-Final-Case/assets/93275022/9277fac4-236a-4126-956c-97d11d06b419)

3.ADDING TRUSTED NODES TO A NETWORK

  Now we will generate a random secret phase and keys by running the following command - 

  ./target/release/node-template key generate --scheme Sr25519 --password-interactive

  ![Step 3 Random secret phases created](https://github.com/abarslan/Polkadot-Rust-Substrate-Bootcamp-Final-Case/assets/93275022/7729656b-ef68-49bf-bfbe-bdf38dd6ed48)

  We will make some changes to the customSpec.json file,
  What we have done is, we’ve added address keys in the aura field for the validator nodes that can create blocks and we’ve added address keys in the grandpa field for the validator nodes that have the authority to 
  finalize blocks.

  In this way we can specifically define which nodes can do what in the network.

  Prepare for launch

  After you distribute the custom chain specification to all network participants, you're ready to launch your own private blockchain.

  Let’s start the first node - 

  ./target/release/node-template 
  --base-path /tmp/node01 
  --chain ./customSpecRaw.json 
  --port 30333 
  --ws-port 9945 
  --rpc-port 9933 
  --telemetry-url "wss://telemetry.polkadot.io/submit/ 0" 
  --validator 
  --rpc-methods Unsafe 
  --name MyNode01 
  --password-interactive

  ![Step 3 node1 started](https://github.com/abarslan/Polkadot-Rust-Substrate-Bootcamp-Final-Case/assets/93275022/45f59ddd-0d70-4557-98fd-5749ccebd2a0)

  Now all we need to do is, verify whether our keys are there in the keystore for node01 by running the following command - 

  ls /tmp/node01/chains/local_testnet/keystore

  ![Step 3 keys stored verify](https://github.com/abarslan/Polkadot-Rust-Substrate-Bootcamp-Final-Case/assets/93275022/f8de53b0-7c4b-42f2-8914-0a3ac06169a2)

  Enable other participants to join

  Now we have to allow other validators to join the network.

  In the previous section, we have one of our authorized nodes with aura and grandpa keys, so now we can start another node.

  Run the following command to start the second node - 

  ./target/release/node-template 
  --base-path /tmp/node02 
  --chain ./customSpecRaw.json 
  --port 30334 
  --ws-port 9946 
  --rpc-port 9934 
  --telemetry-url "wss://telemetry.polkadot.io/submit/ 0" 
  --validator 
  --rpc-methods Unsafe 
  --name MyNode02 
  --bootnodes /ip4/127.0.0.1/tcp/30333/p2p/12D3KooWLmrYDLoNTyTYtRdDyZLWDe1paxzxTw5RgjmHLfzW96SX 
  --password-interactive
  
  ![Step 3 2nd random secret phases created](https://github.com/abarslan/Polkadot-Rust-Substrate-Bootcamp-Final-Case/assets/93275022/4502e4d1-4253-4421-b44b-fdfdc280862d)

  -Now add the grandpa secret key - 

  ./target/release/node-template key insert --base-path /tmp/node02 
  --chain customSpecRaw.json 
  --scheme Ed25519 --suri <second-participant-secret-seed> 
  --password-interactive 
  --key-type gran

  After both nodes have added their keys to their respective keystores—located under /tmp/node01 and /tmp/node02—and been restarted, you should see the same genesis block and state root hashes.

8.SMART CONTRACTS
  -Deploy contract-

  Now that we have compiled the contract, we just need to start the node and deploy it

  Start your node using 

  substrate-contracts-node --log info,runtime::contracts=debug 2>&1

  ![Step 4 node started](https://github.com/abarslan/Polkadot-Rust-Substrate-Bootcamp-Final-Case/assets/93275022/b0da1970-fdfe-474b-b71a-9fdeb9f2d520)

  Build the contract using cargo contract build.

  cargo contract instantiate --constructor new --args "false" --suri //Alice --salt $(date +%s)

  ![Step 4 contract build](https://github.com/abarslan/Polkadot-Rust-Substrate-Bootcamp-Final-Case/assets/93275022/46d3ec07-7613-4f3b-8c40-223cc42ae8c0)

  -Interact with contract-
  Since the smart contract has been deployed, let’s interact with it 
  We will first use get()
  
  cargo contract call --contract $INSTANTIATED_CONTRACT_ADDRESS --message get --suri //Alice --dry-run

  ![Step 4 interacted contract with get() function](https://github.com/abarslan/Polkadot-Rust-Substrate-Bootcamp-Final-Case/assets/93275022/cffe814d-52ce-4830-b2cd-7a03abc448ce)

  Then we will use flip()

  cargo contract call --contract $INSTANTIATED_CONTRACT_ADDRESS --message flip --suri //Alice

  ![Step 4 interacted contract with flip() function](https://github.com/abarslan/Polkadot-Rust-Substrate-Bootcamp-Final-Case/assets/93275022/fccfa9f8-b266-402c-bdf4-ae743eea5dfd)
  


  


  




  


  
  


  

  
