---
title:  "Credit Card Checker"
date:   2017-01-24 9:00:00
description: Credit Card Checker for 4TB3 using regular expressions
---

<script src="https://cdnjs.cloudflare.com/ajax/libs/jquery/3.1.1/jquery.min.js"></script>

### Credit Card Number Checker Using Regex

So this is primarily done to understand how Regex actually works. With Credit Card numbers,verification of the credit card number is done by the Luhn Algorithm. [Wikipedia](https://en.wikipedia.org/wiki/Luhn_algorithm) has a better explanation than I can give, but the gist is that the checksum doubles every other number in account number and then adding it together. Once the total is calculated and modulo 10 equals zero, we have our verified account number.

In regards to the regex, it is here:

`^[4][0-9]{15}$|^[4][0-9]{12}$|^[5][1-5][0-9]{14}|^[3][(4|7)][0-9]{13}`

In short essence, this regex will parse the full 16 digit long credit card number, and verify the values that enter the text field.


<div id="creditCardGroup">
<form id="cc" name="ccForm">
<input type="number" name="ccnumber" placeholder="12341234123412345">
</form>

<img src="/assets/images/creditCard.png" id="ccImage" alt="creditCard">
</div>
<script>
    function isValidCreditCard(sText){
        var res = sText.match(/^[4][0-9]{15}$|^[4][0-9]{12}$|^[5][1-5][0-9]{14}|^[3][(4|7)][0-9]{13}/);
        if(Number(res[0]) == Number(sText)){
            //continue
            var count = 0;
            for(var i = 0; i < sText.length; i++ ){
                var currentVal = Number(sText.charAt(i));
                if(i%2 == 0){
                    currentVal = Number(sText.charAt(i))*2;
                    if(currentVal > 9){
                        currentVal -= 9;
                    }
                }
                count+=currentVal;
            }
            if(count%10 == 0){
                return true;
            }
        }
        return false;
    }
    $('#cc').submit(function () {
        event.preventDefault();
        if($("input[name=ccnumber]").val().length==16){
            if(isValidCreditCard($("input[name=ccnumber]").val())){
                alert("valid card");
            } else {
                alert("invalid card");
            }
        }
        return false;
    });
</script>
<style>
#creditCardGroup{
    position:relative;
}
#cc{
    position: absolute;
    right: 50px;
    bottom: 175px;
    height: 40px;
    width: 300px;
}
#cc input[name=ccnumber]{
    height:35px;
    width:340px;
    font-size:30px;
    color:white;
    border: none;
    background-color: rgba(0,0,0,0);
}
</style>