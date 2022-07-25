# Dice and Dragons
For this project I wanted to make a tool for an incredibly niche market (which might happen to also be just myself). I wanted a tool for some very specific dice-roll based interactions for a Magic: the Gathering deck that become increasingly less possible to perform with real dice as they occur.

## The Context
### Delina, Wild Mage (Dicey Mage)
Delina, Wild Mage is a creature in the game which when attacking may create a copy of a creature the player owns which is tapped and attacking then rolls a dice to determine whether to repeat this action. Simple enough right? Just repeat the dice roll after each success and note down the total number of successes. It does however become a bit more complicated with a select few cards.

### Pixie Guide (Grant Advantage)
In Dungeons and Dragons you can be granted Advantage, a condition which lets you roll two dice and take the best result. Pixie Guide's rule text reads: "Grant an Advantage — If you would roll one or more dice, instead roll that many dice plus one and ignore the lowest roll". This helps tremendously with all of the dice-roll-centric cards in this deck but more importantly, for each copy made of this creature, you may roll an additional dice for determining whether another copy is made.
We'll want to roll a number of times equal to the number of dice granted, display a list of results, whether the highest result succeeds and 

### Wyll, Blade of Frontiers (Advantage Hurts)
Wyll is considered a strict upgrade to Pixie Guide, with the same ability and an additional one, each time you roll one or more dice at the same time, he get's a +1/+1 counter, increasing his potential damage. As each copy is created the previously existing ones will 'tick up' and the new copy will be able to see future dice rolls. As an added feature I want to know the total damage that can dispersed with all these copies of Wyll, we'll want to get Wyll's starting power if he has gained counters prior to being targeted then for the number of copies Add the power of the Original copy of Wyll, then one less damage for each copy.

### Farideh, Devil's Chosen (Card Advantage)
Farideh is another unique interaction which allows you to draw an absurd amount of cards. Farideh's rule tex reads: "Whenever you roll one or more dice, [Farideh] gains flying and menace until the end of turn. If any of those results is a 10 or higher, draw a card". While we're much less likely to create an absurd amount of copies with this as the target, if it is present on the battlefield we will also need to take note of how many of our rolls make us draw a card (Most Likely 1-2 than the total number of copy effects.



## Starting Variables
The things we'll need from the player:
- Sources of advantage before you start rolling
- The creature which delina is targeting
- is there a Farideh or a Level 2 Barbarian Class on the battlefield for additional effects.


## Fun Variables
With all that set up, here's a few nice things we can add for some polish:
- With an absurdly high roll count, what's the total number of dice we rolled?
- What is the total damage Wyll can do with all of his clones?

### Barbarian Class (Polyhedric Rage)
Barbarian Class is another flat source of advantage, it's an enchantment so we can't make copies of it but whenever we roll one or more dice we give a creature +2/+0 and menace (this creature can only be blocked by atleast two creatures). For this we can just tell the player how many of their creatures/copies they can give the bonus to, which will be the same as the 'rollCount'

```cs
Console.WriteLine($"Your rage flows across the battlefield. {rollCount} of your creatures gain +2/+0 and menace until end of turn!");
```

### Hordes of Dice
This part is simple enough, we just have another int which takes the number of dice we're rolling each time and adds it to the count. I decided to call it 'diceCount', in line with the naming conventions. This is a simple addition to our outcome message if we've counted our dice correctly:

```cs
Console.WriteLine($"You rolled {rollCount} times with a total of {diceCount} dice");
```

### Where there's a Wyll, there's probably several more
For this part we need to extend the input we get from the player when they select Wyll as the target. We'll need Wyll's starting power as each copy will have one less power than its predecessor as a result of not having seen the previous rolls. We'll grab the starting power from the player then after all rolls are complete
the strongest Wyll's power will be equal to his starting value +1 for each successful roll. then we'll loop until the power of each one has been added to 'wyllPower'

```cs
int wyllCopies = successCount + 1;
int wyllPower = wyllStart + wyllCopies;
while (wyllCopies > 0)
{
    wyllPower += wyllCopies;
    wyllCopies--;
}
Console.WriteLine($"With that many copies you can dish out {wyllPower} points of damage");
```
