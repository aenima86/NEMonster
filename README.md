![alt text](https://i.imgur.com/PXtjnAK.png "NEMonster")
Welcome to NEMonsters, a witty little collection game based on the NEM blockchain. You can catch and collect over 110,000,000 different combinations of generated monsters; spanning from different colors, names and other spicy characteristics.  You catch a new monster by sending a fixed transaction; the monster is summoned when the transaction is confirmed. The characteristics of your new (evil) monster is based on the transaction hash – how cool is that. See if you can catch some of the monsters from the NEM foundation.

The **purpose** of NEMonster is 
* to have fun
* to demonstration how you can own non-tokenized assets on the blockchain utilizing off-chain and on-chain logic 
* to show how you can use the unique hash of an transaction as a means of procedural generating an gaming asset.  

NEMonster is a browser-based game, implemented as a NEM NanoWallet module, it can be found under the tab services.

![alt text](https://i.imgur.com/DqjWed2.png "NEMonster")

![alt text](https://i.imgur.com/IHfbHZ5.png "NEMonster")

#### The making
We are looking at outgoing transaction, finding relevant transactions to the monster recipient address. The hash data for relevant transactions are saved in a array.
```js
$.each(data, function( index, value ) {
    //change if multisig
    if (value['transaction']['type']==4100){
        var value2 = value['transaction']['otherTrans'];
    }else{
        var value2 = value['transaction'];    
    }
        // loop transactions to find catch trans                
        if ( value2['recipient'] == recipient){
        //minimum 0.5 xem 
        if((value2['amount']/1000000) <= 0.5) {
            potentialMonsterHash.push(value['meta']['hash']['data']);   
        }                       
    }                                                   
});
```
 The hasharray is then analyzed and each hash is rehashed a couple of times with the purpose of making the hash a seed for procedural generating different attributes for the monsters:
 
 ```js
 var colors = ['#F45B69', '#4286f4', '#42f4b6', '#8941f4', '#f49d41', '#ce3333', '#a9ce33', '#33ce35', '#338ace', '#ce3385','#f4ce42','#cee5ce'];
 
 var hash = this.hashStr(potentialMonsterHash[index],5381);
var ind = Math.abs(hash % colors.length);
...
Monsters[index] = {
color : colors[ind],
monster : monster[ind2],
hair : hair[ind3],
eyes : eyes[ind4],
iris : iris[ind4],
mouth : mouth[ind5],
teeth : teeth[ind6],
nam1 : nam1[ind7],
nam2 : nam2[ind8],
grad : grad[ind9],
grad2 : eyecolor
};
 ```
