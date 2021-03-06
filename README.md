KurOTR.js: JavaScript Implementation of Off-the-Record Messaging
================================================================================
version 0.5.1
by [Khandar William](khandar.william@sangkuriang.co.id)
@ [Sangkuriang Internasional](http://sangkuriang.co.id)

For more information on Off-the-Record Messaging, see
<http://www.cypherpunks.ca/otr/>.

LIMITATIONS
-----------

-   only supports V2 protocol
-   doesn't support SMP (Socialist Millionaires' Protocol)
-   doesn't support whitespace tag, yet
-   doesn't support fragmentation, only support receiving fragmentation
-   only support TLV type Padding and Disconnect

THIRD PARTY
-----------

This library uses several [CryptoJS](http://code.google.com/p/crypto-js/) components.
CryptoJS is released under [New BSD License](http://code.google.com/p/crypto-js/wiki/License).

Otr.BigInteger uses [Leemon BigInt.js](http://www.leemon.com/crypto/BigInt.html) 
and [seedrandom.js](http://davidbau.com/archives/2010/01/30/random_seeds_coded_hints_and_quintillions.html).
BigInt.js (c) 2000-2009 Leemon Baird is [public domain](http://www.leemon.com/crypto/BigInt.js).
seedrandom.js (c) 2011 David Bau is released under [BSD License](http://davidbau.com/encode/seedrandom.js).

COMPONENTS
----------

	components/core.js
	components/cipher-core.js
	components/enc-base64.js
	components/aes.js
	components/mode-ctr.js
	components/pad-nopadding.js
	components/sha256.js
	components/sha1.js
	components/hmac.js
	otr.js
	otr.util.js
	otr.biginteger.js
	otr.type.js
	otr.bytebuffer.js
	otr.message.js
	otr.dsa.js
	otr.auth.js
	otr.communication.js

USAGE
-----

1.  Include the combined script
	
		<script src="kurotr-js.min.js" type="text/javascript">

	You can use `/tool/minify.bat` to create the combined script

2.  Create a DSA object and generate its parameters
	
		var dsa = new Otr.DSA();
		
		// the normal way (takes ~10-30 seconds, make browser unresponsive and CPU 100%)
		dsa.generateParameters();
		
		// or, use a pre-generated values (make sure the values are valid)
		dsa.q = new Otr.BigInteger('ede638a754419345c28b15c1a550c4a9fc04edd5', 16);
		dsa.p = new Otr.BigInteger('82f6ca41c437402b46179e1a11fd8018ee5b38666fe04859a38a1f6ac9a8da9be7a99cf36a7c610ec0c547d0117dda1c44e66701b4de209030ebd02f1128b2edefe81a3b80299d2462ed657812343617172609f695cd19550b4a17ca2d6dbb407857163b1c8fa00876c843efb70d79b4d843e84c2e39f59b43fc2c7b24deeb57', 16);
		dsa.g = new Otr.BigInteger('9eefdc5317f7488ebc676ef0c3be6cb2d387d41610181a9171de867ff13aec54008d5632301b8a260ae7cc3a261f41f7f1d8eb87d6e5ddd27073834bbabe6fe8109bed8520b78c038230b1ccde1e1db39ea908a9e6ac5bfe7c3017ae5bc20b493a4a912b5e8b90dc583187c95ffc97c7d66ce8d1b2e2aec071f85505c84670b', 16);
		
		// or, the non-blocking method (slower but browser is responsive)
		var inProgress = function () {
			// give visual feedback about progress
		};
		var whenDone = function () {
			// what to do when parameter generation is finished
		};
		dsa.generateParametersWithTimeout(inProgress, whenDone);

3.  Create a Communication object

		var bobReceiveMessage = function (text, other) {
			// what to do when receiving a message
			console.log('Bob receive: '+text);
		};
		var bobSendMessage = function (text, other) {
			// how to send a message
			aliceToBob.receiveMessage(text);
		};
		var bobToAlice = new Otr.Communication(dsa, bobReceiveMessage, bobSendMessage);

		var aliceReceiveMessage = function (text, other) {
			console.log('Alice receive: '+text);
		};
		var aliceSendMessage = function (text, other) {
			bobToAlice.receiveMessage(text);
		};
		var aliceToBob = new Otr.Communication(dsa, aliceReceiveMessage, aliceSendMessage);

4.  Do some sending and receiving message

		bob.sendMessage('Hello darling');
		alice.sendMessage('No dinner tonight');
	
5.  Need documentation? Read the source code.
	I believe you will only use two components, Otr.DSA and Otr.Communication, so start there.
	For now, this README is the only guide I made.

LICENSE
-------

KurOTR.js is covered by the following (MIT) license:
	
	KurOTR.js
	Copyright (C) 2012 Khandar William, Sangkuriang Internasional

	Permission is hereby granted, free of charge, to any person obtaining
	a copy of this software and associated documentation files (the
	"Software"), to deal in the Software without restriction, including
	without limitation the rights to use, copy, modify, merge, publish,
	distribute, sublicense, and/or sell copies of the Software, and to
	permit persons to whom the Software is furnished to do so, subject to
	the following conditions:

	The above copyright notice and this permission notice shall be
	included in all copies or substantial portions of the Software.

	THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND,
	EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF
	MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND
	NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE
	LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION
	OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION
	WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

VERSION HISTORY
---------------
	0.5.1 - 2012-11-*
	new component otr.communication
	can receive fragmentation
	old mac keys revealed
	no longer use JSBN

	0.5.0 - 2012-08-30
	cover all AKE state transitions

	0.1.1 - 2012-08-10
	TLV disconnect is supported

	0.1.0 - 2012-07-30
	DH key rotation added, basic functionality runs

	0.0.9a - 2012-07-26
	able to do AKE and secure chat but without DH key rotation
