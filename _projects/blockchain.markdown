---
layout: page
title: Block Chain
permalink: /blockchain/
---

The goal of this program was to create a simplified block chain network using linked lists to record transactions on a distributed ledger. The block chain would record transactions between three of the schools at SMU, the Lyle School of Engineering, Cox School of Business, and Meadows School of the Arts, shown below is a visual of the network. 

<div style="text-align: center"><img src="/images/blockchain.jpg" width="350" height="300" /></div>

As shown above the block chain network allows for each school to have their own ledger consisting of solely their transactions rather than having one large ledger of all the transactions between the schools. The transactions could be conducted through a cash, credit, or check transaction. Every form of transaction will have a randomly generated I.D., a sender, a receiver, and a dollar amount. If it is a cash transaction then the ledger will also contain a total amount paid and total change due, if it was a credit transaction then the ledger would contain a card number, expiration date, and cvv, a check transaction will include an account and routing number. To accomplish this I created a inheritance objects, cash, credit, check, all of which inherit from a transaction object holding the I.D., sender, receiver, a dollar amount. I then created a Peer object that hold's the peer's I.D., name, and a linked list of transaction objects that represents that peer's ledger. Finally I created a network object that contained a linked list of peers and peer objects. I used this network object to parse and process the transactions into the correct peer ledgers. An example of the output to the console and one of the peer's ledger saved as a csv file is shown below. 

The complete GitHub repository for this program can be found [here][block-chain-link].

{% highlight text %}
-- Peer 1 Ledger --
Transaction #16807 Lyle School of Engineering sent $150.00 to Cox School of Business
	Payment Method: Credit Card
	Card Number: 12345678
	Exp. Date: 10-10-2020
	CVV: 123
Transaction #1622650073 Lyle School of Engineering sent $100.00 to Dedman School of Law
	Payment Method: Check
	Routing number: 111000614
	Account number: 124356499
Transaction #984943658 Cox School of Business sent $50.00 to Lyle School of Engineering
	Payment Method: Credit Card
	Card Number: 987654321
	Exp. Date: 10-10-2021
	CVV: 321

-- Peer 2 Ledger --
Transaction #16807 Lyle School of Engineering sent $150.00 to Cox School of Business
	Payment Method: Credit Card
	Card Number: 12345678
	Exp. Date: 10-10-2020
	CVV: 123
Transaction #282475249 Cox School of Business sent $199.00 to Dedman School of Law
	Payment Method: Cash
	Amount Paid: $200.00
	Change due: $1.00
Transaction #984943658 Cox School of Business sent $50.00 to Lyle School of Engineering
	Payment Method: Credit Card
	Card Number: 987654321
	Exp. Date: 10-10-2021
	CVV: 321

-- Peer 3 Ledger --
Transaction #282475249 Cox School of Business sent $199.00 to Dedman School of Law
	Payment Method: Cash
	Amount Paid: $200.00
	Change due: $1.00
Transaction #1622650073 Lyle School of Engineering sent $100.00 to Dedman School of Law
	Payment Method: Check
	Routing number: 111000614
	Account number: 124356499
{% endhighlight %}

I.D.|Sender|Receiver|Amount
:--:|:----:|:------:|:-----:
16807|Lyle School of Engineering|Cox School of Business|150.00
282475249|Cox School of Business|Dedman School of Law|199.00
984943658|Cox School of Business|Lyle School of Engineering|50.00

[block-chain-link]: https://github.com/sltimmins/Block-Chain