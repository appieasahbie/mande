# Mande Chain testnet setup


![download (1)](https://user-images.githubusercontent.com/108979536/198679629-31134951-5695-424a-890d-6936310bd6d6.jpg)




I am, because you are

### Human Capital Network on Blockchain
Inspired by ubuntu philosophy we shaped our network consensus to center it around realization of under-utilized potential of people  - we are a network that invests in “Human capital”

Our mission is to invest in people and tie the success of our tokens and network to the realization of their potential. 




# Official links :

Discord : [link](https://discord.gg/WmgT2xkr)

Github : [link](https://github.com/mande-labs)

Whitepaper : [link](https://drive.google.com/file/d/17EScDNUlaYT1Xiera20x8rYsmI3ejggj/view)



# Hardware Requirements

 ### Minimum Hardware Requirements
 
  + 4x CPUs; the faster clock speed the better

  + 8GB RAM

  + 100GB of storage (SSD or NVME)

 ### Recommended Hardware Requirements
 
  + 8x CPUs; the faster clock speed the better

  + 64GB RAM

  + 1TB of storage (SSD or NVME)

# 1-ɪɴᴛᴀʟʟᴀᴛɪᴏɴ ᴏɴᴇ ᴛɪᴍᴇ (ꜱᴄʀɪᴘᴛ ᴀᴜᴛᴏᴍᴀᴛɪᴄ ɪɴꜱᴛᴀʟʟᴀᴛɪᴏɴ)

      wget -O mande.sh https://raw.githubusercontent.com/appieasahbie/mande/main/mande.sh && chmod +x mande.sh && ./mande.sh
      
      
# Post installation

     source $HOME/.bash_profile
     
     
(Check the status of your node)

      mande-chaind status 2>&1 | jq .SyncInfo
      
     
# open ports and active the firewall

      sudo ufw default allow outgoing
      sudo ufw default deny incoming
      sudo ufw allow ssh/tcp
      sudo ufw limit ssh/tcp
      sudo ufw allow ${MANDE_PORT}656,${MANDE_PORT}660/tcp
      sudo ufw enable
     
# Create wallet

 + (Please save all keys on your notepad)     
 
      mande-chaind keys add $WALLET
      
 + To recover your old wallet use this command

      mande-chaind keys add $WALLET --recover
      
 # Add wallet and valoper address and load variables into the system
 
 
      MANDE_WALLET_ADDRESS=$(mande-chaind keys show $WALLET -a)
      MANDE_VALOPER_ADDRESS=$(mande-chaind keys show $WALLET --bech val -a)
      echo 'export MANDE_WALLET_ADDRESS='${MANDE_WALLET_ADDRESS} >> $HOME/.bash_profile
      echo 'export MANDE_VALOPER_ADDRESS='${MANDE_VALOPER_ADDRESS} >> $HOME/.bash_profile
      source $HOME/.bash_profile
     
     
# Fund your wallet on discord faucet chain

# Create validator (after recieving of tokens 1 know  )

     
      mande-chaind tx staking create-validator \
      --amount 1000000mand \
      --from $WALLET \
      --commission-max-change-rate "0.01" \
      --commission-max-rate "0.2" \
      --commission-rate "0.07" \
      --min-self-delegation "1" \
      --pubkey  $(mande-chaind tendermint show-validator) \
      --moniker $NODENAME \
      --chain-id $MANDE_CHAIN_ID
      
### Delegate stake

     mande-chaind tx staking delegate $MANDE_VALOPER_ADDRESS 10000000mand --from=$WALLET --chain-id=$MANDE_CHAIN_ID --gas=auto
     
### Voting

     mande-chaind tx gov vote 1 yes --from $WALLET --chain-id=$MANDE_CHAIN_ID
     
# Node info

 ### Synchronization info
 
     mande-chaind status 2>&1 | jq .SyncInfo
     
 ### Validator info

     mande-chaind status 2>&1 | jq .ValidatorInfo
     
 ### Node info

     mande-chaind status 2>&1 | jq .NodeInfo
     
 ### Show node id

     mande-chaind tendermint show-node-id     

# Service management

 ### Check logs

     journalctl -fu mande-chaind -o cat
     
 ### Start service

     sudo systemctl start mande-chaind
     
 ### Stop service

     sudo systemctl stop mande-chaind
     
 ### Restart service

     sudo systemctl restart mande-chaind
     
# Delete node


      sudo systemctl stop mande-chaind
      sudo systemctl disable mande-chaind
      sudo rm /etc/systemd/system/mande* -rf
      sudo rm $(which mande-chaind) -rf
      sudo rm $HOME/.mande-chain* -rf
      sudo rm $HOME/mande -rf
      sed -i '/MANDE_/d' ~/.bash_profile
      
      
### That`s All 

{Buy me a cup of coffe ](https://www.paypal.com/paypalme/AbdelAkridi?country.x=NL&locale.x=en_US)
     
