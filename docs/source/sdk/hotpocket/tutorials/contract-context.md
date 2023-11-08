# Using the contract context

When HotPocket invokes your smart contract application, it passes some contextual data about the current execution of the smart contract. It passes this information as a JSON payload via the standard input (STDIN) of your application. The HotPocket nodejs contract library automatically handles this and exposes this to your application via the `ctx` parameter.

```javascript
const HotPocket = require("hotpocket-nodejs-contract");

const mycontract = async (ctx) => {
  // ctx parameter provides the contextual information.
  console.log(ctx.contractId);
};

const hpc = new HotPocket.Contract();
hpc.init({
    "consensus": async (ctx) => { await mycontract(ctx); },
    "consensus_fallback": async (ctx) => { },
    "read_req": async (ctx) => { }
});
```

## contractId

This is the GUID string of the contract which is specified at the time of deploying the contract. This is used as a validation mechanism by all the nodes of a cluster and the users to ensure they are talking with instances of the correct contract.

Example with javascript client using contract id validation:

```javascript
const client = await HotPocket.createClient(
  ["wss://localhost:8081"],
  userKeyPair,
  {
    contractId: "3c349abe-4d70-4f50-9fa6-018f1f2530ab",
  }
);
```

This client will drop the connection if the contract id presented by the HotPocket instance its connecting to does not match.

## publicKey and privateKey

ed22519 key pair of the node in hexadecimal format prefixed with 'ed'. HotPocket uses this key pair to sign its messages to other nodes and users. Any party can get the help of this to verify that it is indeed communicating with a trusted node.

Example with javascript client using public key validation:

```javascript
const client = await HotPocket.createClient(
  ["wss://localhost:8081"],
  userKeyPair,
  {
    trustedServerKeys: [
      "ed5597c207bbd251997b7133d5d83a2c6ab9600810edf0bdb43f4004852b8c9e17",
    ],
  }
);
```

## mode

A string **(consensus|consensus_fallback|read_req)** indicating the mode which the contract is being invoked. Mode will be **consensus** if invoked due to a consensus execution. Mode will be **consensus_fallback** if invoked due to inability of reaching the consensus. Mode will be **read_req** if invoked due to a [read request](readreq). In **read_req** mode, the filesystem is readonly. In both **consensus_fallback** and **read_req** modes filesystem represents the last consensus state. At any given time, multiple **read_req** mode executions of the contract can happen. However only one **consensus** or **consensus_fallback** mode execution will happen at any given time.

## timestamp

Consensus timestamp of the last closed ledger in UNIX epoch milliseconds. If you need to use a deterministic timestamp in your contract that is compatible with consensus, you should use this. Using the system time for consensus data (eg. user outputs or state) will cause consensus to fail because the system time will differ between different nodes.

## users

Contains the list of [users](../../../platform/hotpocket/users) who were connected to the cluster during the last consensus round. Via this, you can retrieve any inputs sent by users and also generate user outputs.

## unl

Contains information about the nodes that participate in consensus. Via this you can broadcast and receive [NPL messages](npl) with contract instances of other UNL nodes in a real-time manner.

## lclSeqNo and lclHash

This is the last closed ledger sequence number and the hexadecimal hash. During consensus, the ledger data is salted with an agreed-upon random value. Therefore, you could use the lcl hash as a deterministic random seed for your contract.

# non_consensus_rounds

This is the number of consecutive rounds HotPocket missed the consensus (Number of times contract executed on **consensus_fallback** mode), This will be only available in contract **consensus_fallback** mode.