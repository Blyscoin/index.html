<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Blyscoin Faucet</title>
    <script src="https://cdn.jsdelivr.net/npm/web3/dist/web3.min.js"></script>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 0;
            padding: 0;
            text-align: center;
            background: linear-gradient(120deg, #2a2a72, #009ffd);
            color: white;
        }
        .container {
            padding: 20px;
            max-width: 800px;
            margin: 0 auto;
        }
        .form-group {
            margin: 15px 0;
        }
        input[type="text"] {
            width: 100%;
            max-width: 300px;
            padding: 10px;
            font-size: 16px;
            border: none;
            border-radius: 5px;
            margin: 10px auto;
        }
        button {
            padding: 10px 20px;
            font-size: 16px;
            cursor: pointer;
            background-color: #ff5722;
            color: white;
            border: none;
            border-radius: 5px;
            margin: 10px auto;
        }
        button:disabled {
            background-color: #cccccc;
            cursor: not-allowed;
        }
        img {
            max-width: 100%;
            height: auto;
        }
        h1, h2, h3, p {
            margin: 10px 0;
        }
        ul {
            list-style-type: none;
            padding: 0;
        }
        ul li {
            margin: 5px 0;
        }
        ul li a {
            color: #ffcc00;
            text-decoration: none;
        }
        ul li a:hover {
            text-decoration: underline;
        }
    </style>
</head>
<body>
    <div class="container">
        <img src="https://i.postimg.cc/Hk9zXwFc/BLYS.png" alt="BlysCoin Logo" style="width: 200px;">
        <h1>Blyscoin Faucet</h1>
        <p>Receba BLYS diretamente na sua carteira! (Máximo de 10 solicitações por dia)</p>

        <div class="form-group">
            <input id="walletAddress" type="text" placeholder="Endereço da carteira">
        </div>
        <div class="form-group">
            <button id="claimTokens">Solicitar Tokens</button>
        </div>

        <h2>Informações do Token</h2>
        <div id="tokenInfo">
            <p>Nome: <span id="tokenName">Blyscoin</span></p>
            <p>Símbolo: <span id="tokenSymbol">BLYS</span></p>
            <p>Suprimento Total: <span id="totalSupply">1B</span></p>
        </div>

        <div class="form-group">
            <img src="https://i.postimg.cc/d1HSd2km/Leonardo-Phoenix-09-Create-an-animated-cute-and-joyful-mascot-1.jpg" alt="Mascot" style="width: 150px;">
        </div>

        <h3>Links Oficiais</h3>
        <ul>
            <li><a href="https://t.me/blyscoinn" target="_blank">Telegram</a></li>
            <li><a href="https://x.com/BlysCoin" target="_blank">X (Twitter)</a></li>
            <li><a href="https://bitcointalk.org" target="_blank">BitcoinTalk</a></li>
            <li><a href="https://opensea.io/Blyscoin" target="_blank">OpenSea NFTs</a></li>
            <li><a href="https://discord.gg/qYa5NTkk" target="_blank">Discord</a></li>
            <li><a href="https://blyscoin-rewards-nt43rb6.gamma.site" target="_blank">Rewards</a></li>
            <li><a href="https://blyscoin.printify.me/products" target="_blank">Official Store</a></li>
            <li><a href="https://github.com/Blyscoin/smartcontrat" target="_blank">GitHub</a></li>
        </ul>
    </div>

    <script>
        const contractABI = [
            {"inputs":[],"stateMutability":"nonpayable","type":"constructor"},
            {"anonymous":false,"inputs":[{"indexed":true,"internalType":"address","name":"owner","type":"address"},{"indexed":true,"internalType":"address","name":"spender","type":"address"},{"indexed":false,"internalType":"uint256","name":"value","type":"uint256"}],"name":"Approval","type":"event"},
            {"anonymous":false,"inputs":[{"indexed":false,"internalType":"uint256","name":"amount","type":"uint256"}],"name":"TokensBurned","type":"event"},
            {"anonymous":false,"inputs":[{"indexed":true,"internalType":"address","name":"from","type":"address"},{"indexed":true,"internalType":"address","name":"to","type":"address"},{"indexed":false,"internalType":"uint256","name":"value","type":"uint256"}],"name":"Transfer","type":"event"},
            {"inputs":[],"name":"MAX_SUPPLY","outputs":[{"internalType":"uint256","name":"","type":"uint256"}],"stateMutability":"view","type":"function"},
            {"inputs":[],"name":"name","outputs":[{"internalType":"string","name":"","type":"string"}],"stateMutability":"view","type":"function"},
            {"inputs":[],"name":"symbol","outputs":[{"internalType":"string","name":"","type":"string"}],"stateMutability":"view","type":"function"},
            {"inputs":[],"name":"totalSupply","outputs":[{"internalType":"uint256","name":"","type":"uint256"}],"stateMutability":"view","type":"function"},
            {"inputs":[{"internalType":"address","name":"recipient","type":"address"},{"internalType":"uint256","name":"amount","type":"uint256"}],"name":"transfer","outputs":[{"internalType":"bool","name":"","type":"bool"}],"stateMutability":"nonpayable","type":"function"}
        ];

        const contractAddress = "0x9f72d5eabffc8c3a75dc4750d10c6cfb74fb20bf";
        const dailyLimit = 10;
        const rewardAmount = 10000000; // Valor em wei
        let web3;
        let contract;

        async function init() {
            if (window.ethereum) {
                web3 = new Web3(window.ethereum);
                await window.ethereum.enable();
            } else {
                alert("Por favor, instale uma carteira Ethereum como MetaMask!");
                return;
            }

            contract = new web3.eth.Contract(contractABI, contractAddress);
        }

        document.getElementById("claimTokens").addEventListener("click", async () => {
            const walletAddress = document.getElementById("walletAddress").value;
            if (!web3.utils.isAddress(walletAddress)) {
                alert("Endereço de carteira inválido!");
                return;
            }

            try {
                const accounts = await web3.eth.getAccounts();
                await contract.methods.transfer(walletAddress, rewardAmount).send({ from: accounts[0] });
                alert("Tokens enviados com sucesso para: " + walletAddress);
            } catch (error) {
                console.error("Erro ao solicitar tokens", error);
                alert("Erro ao solicitar tokens. Tente novamente mais tarde.");
            }
        });

        init();
    </script>
</body>
</html>
