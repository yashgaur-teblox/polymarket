import React, { useState, useEffect } from 'react';
import Web3 from 'web3';
import MarketFactory from './contracts/MarketFactory.json';

function App() {
  const [web3, setWeb3] = useState(null);
  const [accounts, setAccounts] = useState([]);
  const [marketFactory, setMarketFactory] = useState(null);

  useEffect(() => {
    async function loadWeb3() {
      const web3 = new Web3(Web3.givenProvider || "http://localhost:8545");
      setWeb3(web3);
      const accounts = await web3.eth.requestAccounts();
      setAccounts(accounts);
      const networkId = await web3.eth.net.getId();
      const deployedNetwork = MarketFactory.networks[networkId];
      const instance = new web3.eth.Contract(
        MarketFactory.abi,
        deployedNetwork && deployedNetwork.address,
      );
      setMarketFactory(instance);
    }
    loadWeb3();
  }, []);

  return (
    <div>
      <h1>Prediction Market</h1>
      {accounts.length > 0 && <p>Connected as: {accounts[0]}</p>}
      {/* UI components for creating and interacting with markets */}
    </div>
  );
}

export default App;
