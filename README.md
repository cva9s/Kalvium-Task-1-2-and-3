# Kalvium-Task-1-2-and-3
In this repository the answer or code of the Kalvium all tasks are saved



Question 1) The three gestures used in base Rock Paper Scissors are rock, paper,
and scissors. The way these are scored is as such: Rock beats Scissors,
Scissors beats Paper, Paper beats Rock. It gets a lot more complicated
when you introduce new gestures, but let's keep it simple for now.
We're definitely going to need a way to decide who has won and who has lost, or
whether the round has ended in a draw. Using the rules provided, give us an
engine for deciding this based on the player's moves.
Rock Beats Scissors
As a player, I want rock to beat scissors. So that I can play with rules
I'm familiar with.
 Given I have chosen rock
When the opponent chooses scissors
Then I should win.
 Given I have chosen scissors
When the opponent chooses rock
Then they should win
Scissors Beats Paper
As a player, I want scissors to beat paper. So that I can play with rules
I'm familiar with.
 Given I have chosen scissors
When the opponent chooses paper
Then I should win.
 Given I have chosen paper
When the opponent chooses scissors
Then they should win.
Paper Beats Rock
As a player, I want paper to beat rock. So that I can play with rules I'm
familiar with.
 Given I have chosen paper
When the opponent chooses rock
Then I should win.
 Given I have chosen rock
When the opponent chooses paper
Then they should win.

Same move results in Draw
As a player, I want the same moves to draw. So that I can play with
rules I'm familiar with.
 Given I have chosen rock
When the opponent chooses rock
Then it should be a draw.
 Given I have chosen paper
When the opponent chooses paper
Then it should be a draw.
 Given I have chosen scissors
When the opponent chooses scissors
Then it should be a draw.
Some rules to keep in mind
 In a Single file the developer needs to store the player’s name and the
highest score
 Developer need to display the highest score when any person starts a new
game
 If any player beats the highest score then his/her score should be updated
as the highest score for the game
 The score cannot be in negative value and the file in which developer is
storing user’s data should provide the information when it is required to be
fetched and if not able to find then proper handling of this scenario should
be there
 Until and unless user wants to quit the game the playing option should be avaialble


Solution-->

```Pyhton

class RockPaperScissorsEngine:
    def __init__(self):
        self.players = {}
        self.highest_score = 0

    def load_players(self):
        try:
            with open("players.txt", "r") as file:
                for line in file:
                    name, score = line.strip().split(":")
                    self.players[name] = int(score)
                    self.highest_score = max(self.highest_score, int(score))
        except FileNotFoundError:
            print("players.txt not found. Starting a new game.")

    def save_players(self):
        with open("players.txt", "w") as file:
            for name, score in self.players.items():
                file.write(f"{name}:{score}\n")

    def update_score(self, name, score):
        self.players[name] = max(0, score)
        self.highest_score = max(self.highest_score, score)

    def play(self, player1, player2):
        if player1 == player2:
            return "Draw"
        elif player1 == "rock" and player2 == "scissors":
            return "Player 1 wins"
        elif player1 == "scissors" and player2 == "paper":
            return "Player 1 wins"
        elif player1 == "paper" and player2 == "rock":
            return "Player 1 wins"
        else:
            return "Player 2 wins"

engine = RockPaperScissorsEngine()
engine.load_players()
print("The current highest score is", engine.highest_score)

while True:
    player1 = input("Player 1, choose rock, paper, or scissors: ").lower()
    if player1 == "quit":
        break
    player2 = input("Player 2, choose rock, paper, or scissors: ").lower()
    result = engine.play(player1, player2)
    print(result)
    if result.startswith("Player 1"):
        engine.update_score(player1, engine.players.get(player1, 0) + 1)
    elif result.startswith("Player 2"):
        engine.update_score(player2, engine.players.get(player2, 0) + 1)

engine.save_players()
```


Quesrion 2)You need to write the software to calculate the minimum number of
coins required to return an amount of change to a user of Acme
Vending machines. For example, the vending machine has coins 1,2,5
and 10 what is the minimum number of coins required to make up the
change of 43 cents?
The coin denominations will be supplied as a parameter. This is so the
algorithm is not specific to one country. You may not hardcode these
into the algorithm, they must be passed as a parameter.
The country’s denominations to use for the Question are:
● British Pound ○ 1,2,5,10,20,50

● US Dollar ○ 1,5,10,25
● Norwegian Krone ○ 1,5,10,20
The Question assumes an infinite number of coins of each
denomination. You are to return an array with each coin to be given as
change.
Increment- Remove the assumption that there are infinite coins of
each denomination. Modify the code to accept a fixed number of each
denomination. It will affect the change calculation in that you now
need to consider the availability of coins when calculating change.


Solution--> **a)Unlimited Supply Recursive Solution** 

```Python
def minCoinsR(denomination, change):
    if(change==0):
        return 0
    res = []
    for d in denomination:
        if d <= change:
            res.append(minCoinsR(denomination, change-d)+1)
    return min(res)
```
    
 **b)Unlimited Supply Non-recursive Solution**
 
 ```Python
 def min_coins(coin_denominations, change): 
    num_coins = [0] * (change + 1) 
    print(num_coins)
    num_coins[0] = 0 

    for i in range(1, change + 1): 
        num_coins[i] = float("inf")
        for denomination in coin_denominations:
            if denomination <= i:
                num_coins[i] = min(num_coins[i], num_coins[i - denomination] + 1)

        print(num_coins[:i+1])
    
    return num_coins[change]

# Example usage
coin_denominations = [1, 2, 5, 10]
change = 7
print("Minimum number of coins required:", minCoinsR(coin_denominations, change))
```

**c)Limited Supply Recursive Solution**

```Python
def minCoinsR(denomination, change, supply):
    if(change==0):
        return 0
    res = []
    for d in denomination:
        if d <= change and supply[d] > 0:
            supply[d] -= 1
            res.append(minCoinsR(denomination, change-d, supply)+1)
    return min(res) if len(res) > 0 else 0
  ```


Question 3)There are varying opinions amongst writers as to when one should
spell out a number vs. using figures. However there are a few
generally agreed upon rules for doing so. 1 There are 2 of them:
● Numbers beginning a sentence should be spelt out.

● Hyphenate all compound numbers from twenty-one through ninety-
nine

Write an implementation of these rules for numbers starting a
sentence. It should take an input of figures and return the number
spelt out. Assume all the numbers given start sentences and your
code will be used like a spell checker for grammar issues related to
numbers. Only worry about up numbers up to 4 digits long. Assume all
numbers are positive.
Examples
1 digit
0 = zero
5 = five
8 = eight
2 digits
10 = ten
21 = twenty-one
77 = seventy-seven
3 digits

100 = one hundred
303 = three hundred three
555 = five hundred fifty-five
4 digits
2000 = two thousand
3466 = three thousand four hundred sixty-six
2400 = two thousand four hundred
additional rule: The simplest way to express large numbers is
usually best. Instead of 5300 being equal to five thousand three
hundred it should return fifty-three hundred.
Please upload the solutions to your github repository and provide us
the link of your github repo

Solution-->
```Pyhton
def spell_out_number(n):
    if n < 10:
        return ["zero", "one", "two", "three", "four", "five", "six", "seven", "eight", "nine"][n]
    elif n < 20:
        return ["ten", "eleven", "twelve", "thirteen", "fourteen", "fifteen", "sixteen", "seventeen", "eighteen", "nineteen"][n-10]
    elif n < 100:
        tens = ["twenty", "thirty", "forty", "fifty", "sixty", "seventy", "eighty", "ninety"][(n//10)-2]
        if n % 10 == 0:
            return tens
        else:
            return tens + "-" + spell_out_number(n % 10)
    elif n < 1000:
        hundreds = spell_out_number(n // 100) + " hundred"
        if n % 100 == 0:
            return hundreds
        else:
            return hundreds + " " + spell_out_number(n % 100)
    elif n < 10000:
        thousands = spell_out_number(n // 1000) + " thousand"
        if n % 1000 == 0:
            return thousands
        else:
            return thousands + " " + spell_out_number(n % 1000)

print(spell_out_number(91))
```
