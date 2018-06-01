# Assem - hodler function preview

* [Assem.app](http://assem.app)
* [USA - MEGA MIliON](http://assem.app/mm)
* [KOREA - 나눔로또 6/45](http://assem.app/krl)
* [JAPAN - ロトセブン](http://assem.app/jpl)
* [Omikuji「おみくじ」](http://assem.app/omk)
* [Hodler page](http://assem.app/hodler)

We will open full sourcecode when 30% of ICO is filled

```sh
uint hodl_Profit = thisMonthProfit*9/10;
ownerBalance += (thisMonthProfit - hodl_Profit);
```
## Hodler function

```sh
    function submit_hodler() public{
        bool already = false;
        for(uint i=0;i<hodlerLength;i++){
            if(hodler[i] == msg.sender){
                already = true;
                break;
            }
        }
        if(!already){
            uint tmp_token_balance = tokenContract.balanceOf(msg.sender);
            if(tmp_token_balance >= 100000000000000000000){
                hodler.push(msg.sender);
                hodlerLength++;
            }
        }
    }
    
    function snapShot() public onlyBy (owner){
        uint thisMonthProfit = mm_profit+krl_profit+jpl_profit+omk_profit;
        uint now_full_token = 0;
        uint hodl_Profit = thisMonthProfit*9/10;
        for(uint i=0;i<hodlerLength;i++){
            uint tmp_token_balance = tokenContract.balanceOf(hodler[i]);
            if(tmp_token_balance >= 100000000000000000000){
                now_full_token += tokenContract.balanceOf(hodler[i]);
            }
        }
        uint spreaded_Profit = hodl_Profit;
        uint this_hodler_Profit = 0;
        for(uint j=0;j<hodlerLength;j++){
            tmp_token_balance = tokenContract.balanceOf(hodler[j]);
            if(tmp_token_balance >= 100000000000000000000){
                this_hodler_Profit = hodl_Profit*percent(tmp_token_balance,now_full_token,3)/1000;
                if((spreaded_Profit + this_hodler_Profit) <= hodl_Profit){
                    lastSanpEther[hodler[j]] += this_hodler_Profit;
                    spreaded_Profit += this_hodler_Profit;
                }
                else{
                    lastSanpEther[hodler[j]] += (hodl_Profit - spreaded_Profit);
                    spreaded_Profit += this_hodler_Profit;
                }
            }
        }
        ownerBalance += (thisMonthProfit - hodl_Profit);
        mm_profit = 0;
        krl_profit = 0;
        jpl_profit = 0;
        omk_profit = 0;
    }
    
    function hodler_withdraw() public returns(bool){
        if(lastSanpEther[msg.sender] > 0){
            msg.sender.transfer(lastSanpEther[msg.sender]);
            lastSanpEther[msg.sender] = 0;
            return true;
        }
        else{
            return false;
        }
    }
    
    function owner_withdraw() public onlyBy (owner){
        owner.transfer(ownerBalance);
        ownerBalance = 0;
    }
```



