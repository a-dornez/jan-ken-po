# Jan Ken Po ✊✋✌ 

Jan Ken Po game project using Java! A simple project meant to exercise and acess my skills. I started with the following ***requirements**: **1.** the player faces a machine player, thus a Pc plays system should be coded, **2.** player must be able to set another match or quite game, **3.** display a scoreboard.

Iniatially, I thought about a player vs player mode, but that seems to make little sense in the current state. Though, I want to add it later, when I learn about making applications interact with each other, I believe it will be a good practice about establishing communication between different machines.

## ⚙ How it works

It is played on terminal: it prompts player to choose a hand, then Pc player chooses a hand, chosen hands are compared and result is displayed. Finally, asks if player wants to play again or not.

#### 🤖 How Pc chooses it's hands

First of all, there is no connection between what player chooses and what Pc plays. It could be done in reverse order, pc making it's play first, the result would be the same.

That is because **it's plays are randomly generated**. How so? There is a array of "possible hands" *(R, P, S)*: At every match a random number is drawn, indicating a position in possible hands array and so a hand is picked for Pc:

```
String[] possibleHands = new String[] {"R", "S", "P"};
Random getPcHand = new Random();
int chosenHandIndex = getPcHand.nextInt(possibleHands.length);
String pcHand = possibleHands[chosenHandIndex];
```

Of course, depending on your lucky, it may look rigged during some plays (once, I lost seven times on a row and got a bit confused). But there is not a mechanism that considers present or past plays while choosing Pc hands.

#### 🤼 How matches are decided

Good old **conditionals**. Simple and functional.

I was worried about making it a mess while covering all possibilities, but I believe it turned out to be easily understood.

**It works considering in which cases player wins**. There are only three possible scenarios in which a player wins, one for each possible hand (rock facing scissor, scissor facing papper and papper facing rock). So, first the system checks each of those player-victory scenarios, second it checks a draw possibilitie (player hand equal to pc hand), finally, there is only one scenario left: player loses. For each scenario, there is a corresponding result message.

Making it that way, **only explicitly checking victory and draw scenarios**, simplifies if-else structure, as **it avoids also individually checking each loss scenario**. Explicitly checking draw is necessary, as when it was not checked, draws were interpreted was player losing, as they felt in "else means defeat" case.

```
String winLose;
if ( playerHand.equalsIgnoreCase("R") && pcHand == "S" ) {
    winLose = "Win";
    wins++;
} else if ( playerHand.equalsIgnoreCase("S") && pcHand == "P" ) {
    winLose = "Win";
    wins++;
} else if ( playerHand.equalsIgnoreCase("P") && pcHand == "R" ) {
    winLose = "Win";
    wins++;
} else if ( playerHand.equalsIgnoreCase(pcHand) ) {
    winLose = "Draw";
    draws++;
} else {
    winLose = "Lose";
    losses++;
}
```

Separating ifs to avoid else ifs to improve readability proved troublesome, as the last if was always the only one to actually matter to the final result. As it was always checked and always the last one to be checked, the registered result would be always be modified by it.

There were actually other approaches and middle-steps I considered, but this way I knew I could make it functional faster using if-elses. That way, I can test and study about other approaches while already having a functional version. Some of them will probably be over engineered and not make it to main version, but at least will be useful as study cases. 

They involve using OOP features, such as defining hand objects, setting properties and using lambda to decide matches. Not sure how much sense some of these and others ideas actually make, but the whole point of thinking about, studying and testing is to to figure it out.

#### 📊 How scoreboard works

**For each match result conditional there is a variable *winLose* that stores the corresponding win, draw or lose scenario**. Every time a match result is checked, the value of wins, draws or loses is increased.
