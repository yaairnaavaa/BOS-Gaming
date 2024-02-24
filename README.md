# BOS + Gaming

This repository is an example of how to create a game in BOS using any blockchain.

This game is a tamagotchi with a Burrito theme where you can mint new NFT and interact with them with the possibility to play, feed them and make them sleep.

![Burrito](https://drive.google.com/uc?id=1ix6w76D6P4wmovlnSAaehrK59qqCKgB1)

## How to implement the game in BOS?

A simple way to implement this type of games in BOS is by using animated images, which will be displayed depending on the activity that the Burrito is performing.

All the statistics will be stored in the smart contract from where we will also be retrieving the information from the NFTs and sending to update the information after performing any action.

The main configurations and methods to be called from BOS to successfully run this game will be shown below:

```jsx
// Smart contract address
const virtualPetContract = "0xE3B4cf554EA9113fbbF0715309ce87165024901E";

// Obtaining the ABI with the list of methods available in the contract
const virtualPetAbi = fetch(
  "https://raw.githubusercontent.com/cloudmex/burritobattle-pet/main/ABI.txt"
);
```

Mint:

```jsx
const mint = () => {
  // We initialize the contract with ethers.Contract and put into use the contract, the ABI and the account that will sign the transactions
  const contract = new ethers.Contract(
    virtualPetContract,
    virtualPetAbi.body,
    Ethers.provider().getSigner()
  );

  // We call the mintPet method
  contract.mintPet(state.burritoName).then((res) => {
    const lastId = (state.mintedBurritos += 1);
    State.update({
      init: false,
      burritoName: "",
      mintedBurritos: lastId,
      minting: true,
    });
    setTimeout(() => {
      contract.getMintedTokens().then((res) => {
        State.update({
          minting: false,
        });
      });
    }, "20000");
  });
};
```

Play:

```jsx
const play = () => {
  // We initialize the contract with ethers.Contract and put into use the contract, the ABI and the account that will sign the transactions
  const contract = new ethers.Contract(
    virtualPetContract,
    virtualPetAbi.body,
    Ethers.provider().getSigner()
  );

  // We call the play method
  contract.play(state.tokenId).then((res) => {
    State.update({
      isBusy: true,
      isPlay: true,
    });
  });
};
```

![Play](https://drive.google.com/uc?id=1geax6MzEKyPqrnJO5RQdel4V84xOWJxR)

Eat:

```jsx
const eat = () => {
  // We initialize the contract with ethers.Contract and put into use the contract, the ABI and the account that will sign the transactions
  const contract = new ethers.Contract(
    virtualPetContract,
    virtualPetAbi.body,
    Ethers.provider().getSigner()
  );

  // We call the eat method
  contract.eat(state.tokenId).then((res) => {
    State.update({
      currentImg: _getEatImg(state.pet.image),
      isBusy: true,
    });
    setTimeout(() => {
      getNft();
    }, "20000");
  });
};
```

![Eat](https://drive.google.com/uc?id=13ptJzTIzHr14b3E45w204NathGljjUYC)

Sleep:

```jsx
const sleep = () => {
  // We initialize the contract with ethers.Contract and put into use the contract, the ABI and the account that will sign the transactions
  const contract = new ethers.Contract(
    virtualPetContract,
    virtualPetAbi.body,
    Ethers.provider().getSigner()
  );

  // We call the doze method
  contract.doze(state.tokenId).then((res) => {
    State.update({
      currentImg: _getSleepImg(state.pet.image),
      isBusy: true,
    });
    setTimeout(() => {
      getNft();
    }, "20000");
  });
};
```

![Sleep](https://drive.google.com/uc?id=1HB8gUR5OQl8nA3FYHTQAKcM1rQ6S-gmA)

BOS Widget Interact: https://near.org/owa-is-bos.near/widget/BOS-Gaming-Burrito-Mint <br/>
BOS Widget Interact: https://near.org/owa-is-bos.near/widget/BOS-Gaming-Burrito-Interact
