# Dice and Dragons
For this project I wanted to make a tool for an incredibly niche market (which might happen to also be just myself). I wanted a tool for some very specific dice-roll based interactions for a Magic: the Gathering deck that become increasingly less possible to perform with real dice as they occur.

## The Context
### Delina, Wild Mage (Dicey Mage)
![afr-138-delina-wild-mage](https://user-images.githubusercontent.com/61814399/179874894-c4236708-6198-4a91-bd9f-9a2b9d5526c5.jpg = 250x350)
Delina, Wild Mage is a creature in the game which when attacking may create a copy of a creature the player owns which is tapped and attacking then rolls a dice to determine whether to repeat this action. Simple enough right? Just repeat the dice roll after each success and note down the total number of successes. It does however become a bit more complicated with a select few cards.

### Pixie Guide (Grant Advantage)
<img src= src="[https://user-images.githubusercontent.com/61814399/179876713-03418de3-eda5-4f54-a0ec-a8bd01a5de93.jpg]" width="250" height="350" />
In Dungeons and Dragons you can be granted Advantage, a condition which lets you roll two dice and take the best result. Pixie Guide's rule text reads: "Grant an Advantage — If you would roll one or more dice, instead roll that many dice plus one and ignore the lowest roll". This helps tremendously with all of the dice-roll-centric cards in this deck but more importantly, for each copy made of this creature, you may roll an additional dice for determining whether another copy is made.
We'll want to roll a number of times equal to the number of dice granted, display a list of results, whether the highest result succeeds and

### Wyll, Blade of Frontiers (Advantage Hurts)
![clb-208-wyll-blade-of-frontiers](https://user-images.githubusercontent.com/61814399/179874911-aecbf0e6-4b93-4539-9c1b-12831801ae3a.jpg | width=50)
Wyll is considered a strict upgrade to Pixie Guide, with the same ability and an additional one, each time you roll one or more dice at the same time, he get's a +1/+1 counter, increasing his potential damage. As each copy is created the previously existing ones will 'tick up' and the new copy will be able to see future dice rolls. As an added feature I want to know the total damage that can dispersed with all these copies of Wyll, we'll want to get Wyll's starting power if he has gained counters prior to being targeted then for the number of copies Add the power of the Original copy of Wyll, then one less damage for each copy.

### Farideh, Devil's Chosen (Card Advantage)
Farideh is another unique interaction which allows you to draw an absurd amount of cards. Farideh's rule tex reads: "Whenever you roll one or more dice, [Farideh] gains flying and menace until the end of turn. If any of those results is a 10 or higher, draw a card". While we're much less likely to create an absurd amount of copies with this as the target, if it is present on the battlefield we will also need to take note of how many of our rolls make us draw a card (Most Likely 1-2 than the total number of copy effects.

### Barbarian Class (Polyhedric Rage)
![afr-131-barbarian-class](https://user-images.githubusercontent.com/61814399/179874824-e61fcd38-f9fd-4d11-b9b4-6001fe412806.jpg | width=50)
Barbarian Class is another flat source of advantage, it's an enchantment so we can't make copies of it but whenever we roll one or more dice we give a creature +2/+0 and menace (this creature can only be blocked by atleast two creatures). Delina makes one copy for free before rolling dice so this will most often just be one less than the number of successes.

## Starting Variables
The things we'll need from the player:
- Sources of advantage before you start rolling
- The creature which delina is targeting
- is there a Farideh or a Level 2 Barbarian Class on the battlefield for additional effects.
