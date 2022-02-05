
const Discord = require('discord.js');
const bot = new Discord.Client({intents: ["GUILD_PRESENCES"]});

const { ethers } = require("ethers");

// Provider
const provider = new ethers.providers.WebSocketProvider('INSERT_PROVIDER_HERE');


// Juicebox Deposit Terminal
const terminalAddress = "0x981c8ECD009E3E84eE1fF99266BF1461a12e5c68";
const terminalAbi = ["function reservedTicketBalanceOf(uint256 _projectId, uint256 _reservedRate) external view returns (uint256)"];
const terminalContract = new ethers.Contract(terminalAddress, terminalAbi, provider);

// Project ID and how many tokens are given per ETH
const projectId = 36;
const tokensPerETH = 1000000;

var ethRaised;

async function main(){
  ethRaised = await getEthRaised();
}

bot.on('ready', () => {
    bot.user.setStatus('available')
    bot.user.setActivity(`${ethRaised}Ξ Raised`, { type: 'WATCHING' });
});

// Updates bot status every 60 seconds
setInterval(async function updateStatus(){
  ethRaised = await getEthRaised();
  bot.user.setActivity(`${ethRaised}Ξ Raised`, { type: 'WATCHING' });
}, 60000);

async function getEthRaised(){
  let ticketsReserved = ethers.utils.formatEther(await terminalContract.reservedTicketBalanceOf(projectId, 100));
  let ethRaised = (ticketsReserved / tokensPerETH).toFixed(0);
  return ethRaised;
}

main();

// Bot token
bot.login('DISCORD_BOT_TOKEN');
