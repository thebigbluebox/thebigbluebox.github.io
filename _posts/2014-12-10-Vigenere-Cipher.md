---
title:  "Vignere Cipher"
date:   2014-12-10 9:00:00
description: Vignere Cipher
---
<script src="//ajax.googleapis.com/ajax/libs/jquery/2.1.1/jquery.min.js"></script>
During one of my recent study sessions with my friends for an exam on computer security, one of the notes on ancient ciphers caught my eye. With procrastination looming over my head while I study, I decided to take a poke at creating a JavaScript based Vigenère cipher.

Let’s talk history, and what a cipher is. As Google puts it, a cipher is “A secret or disguised way of writing; a code” which is exactly what it is. Ciphers are everywhere, your slang can be a cipher as people that aren’t familiar with slang won’t understand hence disguising your message, but the cipher that I was studying was closer to what Caesar used. The Caesar cipher is a simple substitution cipher (meaning it replaces alphabets) that encrypted the alphabets by shifting them by N+X, N being the location of the alphabet and X being how many times was it moved forwards or backwards. The Caesar cipher banked on the fact that majority of Rome couldn’t read, hence encrypting the message using this cipher was enough to deter others from trying to decrypt the message. However, if you looked at the message and understood how the cipher worked, and using a brute force method of shifting it from 0 to the length of alphabets available, it essentially broke down immediately and was not a very good form of encryption.

Now we move on to what is the stronger Caesar cipher, the Vigenère cipher, another substitution cipher with added features that make it much stronger than the simpleton’s Caesar cipher. No longer you need to bank of the fact that your population is illiterate to protect your messages, the Vigenère cipher makes it hard for everyone to decrypt without the correct passage. A Vigenere cipher works by transcribing a key repeatedly under the plaintext, and according to the letter of the corresponding key (the secret message) the shift is determined. And the matrix below can accompany new characters by generating a new matrix, the matrix is read according to the first column A and given the corresponding key character on the top row, will give the according letter in return. Try it your self!


<div id="charset" style="width: 50%; margin: 0 auto;"><input id="charsetinput" style="width: 80%;" type="text" value="charset"> <button id="charsetupdate">Update</button></div>
<div id="venmatrix" style="overflow-x:scroll"></div>
<div id="textencipher" style="width: 80%; margin: 0 auto;"><input id="key" type="text" value="key"> <button id="encipherbutton">Encipher</button> <input id="plaintext" type="text" value="plaintext"> <button id="decipherbutton">Decipher</button> <input id="cipheredtext" type="text" value="cipheredtext"></div>

#### The code used to create the cipher
Available on Github here [here](https://github.com/thebigbluebox/My-Project/blob/master/Algorithms/ciphers.js)

```javascript
var alphabets = ["A","B","C","D","E","F","G","H","I","J","K","L","M","N","O","P","Q","R","S","T","U","V","W","X","Y","Z"];
var alpha = {};

for(var i = 0; i < alphabets.length; i++){
	alpha[alphabets[i]] = i; //assigns each alphabet a integer key
}

function newAlpha(inputChar){
	alphabets.push(inputChar);
	mapfunc();
	genVernier("venmatrix");
}

function alphaset(inputString){
	alphabets = inputString.split("");
	mapfunc();
	genVernier("venmatrix");
}

function returnsalphaset(){
	return 	alphabets.join("");
}

function mapfunc(){ //creates a mapping of the alphabets
	for(var i = 0; i < alphabets.length; i++){
		alpha[alphabets[i]] = i; //assigns each alphabet a integer key
	}
}

Number.prototype.mod = function(n) { return ((this%n)+n)%n; }

function verniercipher(inputString, keyString, flag){
	var outputArray = new Array(inputString.length);
	var inputStringArray = inputString.toUpperCase().split('');
	var keyArray = keyString.toUpperCase().split('');
	var counter = 0;

	for(var i = 0; i < outputArray.length ; i++)
	{
		if(flag == true)
		{
			outputArray[i] = alphabets[(alpha[inputStringArray[i]] + alpha[keyArray[counter]]).mod(alphabets.length-1)];		
		}
		else
		{
			outputArray[i] = alphabets[(alpha[inputStringArray[i]] - alpha[keyArray[counter]]).mod(alphabets.length-1)];		
		}
		if((counter + 1) >= keyArray.length)
		{
			counter = 0; //Resets the keyArray Counter so it continues from zero again
		}
		else
		{
			counter ++;
		}
	}
	return outputArray.join("");
}

function ceasercipher(inputString, shift, flag){
	var outputArray = new Array(inputString.length);
	var inputStringArray = inputString.toUpperCase().split('');
	var counter = 0;

	for(var i = 0; i < outputArray.length ; i++)
	{

		if(flag == true)
		{
			outputArray[i] = alphabets[(alpha[inputStringArray[i]] + shift).mod(alphabets.length-1)];		
		}
		else
		{
			outputArray[i] = alphabets[(alpha[inputStringArray[i]] - shift).mod(alphabets.length-1)];		
		}
	}
	return outputArray.join("");
}

function Encipher(){
	document.getElementById("cipheredtext").value = verniercipher(document.getElementById("plaintext").value , document.getElementById("key").value, true);
}

function Decipher(){
	document.getElementById("plaintext").value = verniercipher(document.getElementById("cipheredtext").value , document.getElementById("key").value, false);
}

function genVernier(name) {
    var myTableDiv = document.getElementById(name);
    if(document.getElementById("genVernier")){
    	$('#genVernier').remove();
    }

    var table = document.createElement('TABLE');
    table.setAttribute("id", "genVernier");
    var tableBody = document.createElement('TBODY');

    table.border = '0'
    table.appendChild(tableBody);


    //TABLE COLUMNS
    var tr = document.createElement('TR');

    for (i = 0; i <= alphabets.length; i++) {
        var th = document.createElement('TH')
        th.width = '10';
        if(i == 0){
        	th.appendChild(document.createTextNode(" "));
        	tr.appendChild(th);
        }
        else{
	        th.appendChild(document.createTextNode(alphabets[i-1]));
	        tr.appendChild(th);
        }
    }
    tableBody.appendChild(tr);

    //TABLE ROWS
    for (i = 0; i < alphabets.length; i++) {
        var tr = document.createElement('TR');

        for (j = 0; j <= alphabets.length; j++) {
            var td = document.createElement('TD')
            if(j == 0){
        		td.appendChild(document.createTextNode(alphabets[i]));
        		tr.appendChild(td);
	        }
	        else{
            	td.appendChild(document.createTextNode(alphabets[(alpha[alphabets[i]] + alpha[alphabets[j-1]]).mod(alphabets.length)]));
            	tr.appendChild(td)
            }
        }
        tableBody.appendChild(tr);
    }  
    myTableDiv.appendChild(table)
}

$(document).ready(function() {
    	genVernier("venmatrix");
    	document.getElementById("charsetinput").value = returnsalphaset();
    	$("#charsetupdate").click(function(){
			alphabets = document.getElementById("charsetinput").value.split("");
			mapfunc();
			genVernier("venmatrix");
		});
		$("#encipherbutton").click(function(){
			Encipher();
		});
		$("#decipherbutton").click(function(){
			Decipher();
		});
});
```

<script type="text/javascript">
var alphabets = ["A","B","C","D","E","F","G","H","I","J","K","L","M","N","O","P","Q","R","S","T","U","V","W","X","Y","Z"];
var alpha = {};

for(var i = 0; i < alphabets.length; i++){
	alpha[alphabets[i]] = i; //assigns each alphabet a integer key
}

function newAlpha(inputChar){
	alphabets.push(inputChar);
	mapfunc();
	genVernier("venmatrix");
}

function alphaset(inputString){
	alphabets = inputString.split("");
	mapfunc();
	genVernier("venmatrix");
}

function returnsalphaset(){
	return 	alphabets.join("");
}

function mapfunc(){ //creates a mapping of the alphabets
	for(var i = 0; i < alphabets.length; i++){
		alpha[alphabets[i]] = i; //assigns each alphabet a integer key
	}
}

Number.prototype.mod = function(n) { return ((this%n)+n)%n; }

function verniercipher(inputString, keyString, flag){
	var outputArray = new Array(inputString.length);
	var inputStringArray = inputString.toUpperCase().split('');
	var keyArray = keyString.toUpperCase().split('');
	var counter = 0;

	for(var i = 0; i < outputArray.length ; i++)
	{
		if(flag == true)
		{
			outputArray[i] = alphabets[(alpha[inputStringArray[i]] + alpha[keyArray[counter]]).mod(alphabets.length-1)];		
		}
		else
		{
			outputArray[i] = alphabets[(alpha[inputStringArray[i]] - alpha[keyArray[counter]]).mod(alphabets.length-1)];		
		}
		if((counter + 1) >= keyArray.length)
		{
			counter = 0; //Resets the keyArray Counter so it continues from zero again
		}
		else
		{
			counter ++;
		}
	}
	return outputArray.join("");
}

function ceasercipher(inputString, shift, flag){
	var outputArray = new Array(inputString.length);
	var inputStringArray = inputString.toUpperCase().split('');
	var counter = 0;

	for(var i = 0; i < outputArray.length ; i++)
	{

		if(flag == true)
		{
			outputArray[i] = alphabets[(alpha[inputStringArray[i]] + shift).mod(alphabets.length-1)];		
		}
		else
		{
			outputArray[i] = alphabets[(alpha[inputStringArray[i]] - shift).mod(alphabets.length-1)];		
		}
	}
	return outputArray.join("");
}

function Encipher(){
	document.getElementById("cipheredtext").value = verniercipher(document.getElementById("plaintext").value , document.getElementById("key").value, true);
}

function Decipher(){
	document.getElementById("plaintext").value = verniercipher(document.getElementById("cipheredtext").value , document.getElementById("key").value, false);
}

function genVernier(name) {
    var myTableDiv = document.getElementById(name);
    if(document.getElementById("genVernier")){
    	$('#genVernier').remove();
    }

    var table = document.createElement('TABLE');
    table.setAttribute("id", "genVernier");
    var tableBody = document.createElement('TBODY');

    table.border = '0'
    table.appendChild(tableBody);


    //TABLE COLUMNS
    var tr = document.createElement('TR');

    for (i = 0; i <= alphabets.length; i++) {
        var th = document.createElement('TH')
        th.width = '10';
        if(i == 0){
        	th.appendChild(document.createTextNode(" "));
        	tr.appendChild(th);
        }
        else{
	        th.appendChild(document.createTextNode(alphabets[i-1]));
	        tr.appendChild(th);
        }
    }
    tableBody.appendChild(tr);

    //TABLE ROWS
    for (i = 0; i < alphabets.length; i++) {
        var tr = document.createElement('TR');

        for (j = 0; j <= alphabets.length; j++) {
            var td = document.createElement('TD')
            if(j == 0){
        		td.appendChild(document.createTextNode(alphabets[i]));
        		tr.appendChild(td);
	        }
	        else{
            	td.appendChild(document.createTextNode(alphabets[(alpha[alphabets[i]] + alpha[alphabets[j-1]]).mod(alphabets.length)]));
            	tr.appendChild(td)
            }
        }
        tableBody.appendChild(tr);
    }  
    myTableDiv.appendChild(table)
}

$(document).ready(function() {
    	genVernier("venmatrix");
    	document.getElementById("charsetinput").value = returnsalphaset();
    	$("#charsetupdate").click(function(){
			alphabets = document.getElementById("charsetinput").value.split("");
			mapfunc();
			genVernier("venmatrix");
		});
		$("#encipherbutton").click(function(){
			Encipher();
		});
		$("#decipherbutton").click(function(){
			Decipher();
		});
});
</script>
