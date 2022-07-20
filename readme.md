# Dice and Dragons
For this project I wanted to make a tool for an incredibly niche market (which might happen to also be just myself). I wanted a tool for some very specific dice-roll based interactions for a Magic: the Gathering deck that become increasingly less possible to perform with real dice as they occur.

## The Context
### Delina, Wild Mage (Dicey Mage)
![afr-138-delina-wild-mage](https://user-images.githubusercontent.com/61814399/179874894-c4236708-6198-4a91-bd9f-9a2b9d5526c5.jpg | width=50)
Delina, Wild Mage is a creature in the game which when attacking may create a copy of a creature the player owns which is tapped and attacking then rolls a dice to determine whether to repeat this action. Simple enough right? Just repeat the dice roll after each success and note down the total number of successes. It does however become a bit more complicated with a select few cards.

### Pixie Guide (Grant Advantage)
![afr-66-pixie-guide](https://user-images.githubusercontent.com/61814399/179874775-746a86af-5f5a-4ab6-947f-9f3bc935fb90.jpg | width=50)
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


using System.Collections.Generic;
using System;
using System.Threading;

public class DelinaDice
{
    static void Main(string[] args)
    {
        //Target Bools
        bool Wyll = false;
        bool Pixie = false;
        bool Farideh = false;
        bool rolling = true;
        int numberOfDice = 1;
        int currentRoll = 1;
        List<int> rollResults = new List<int>();
        
        //Counts the number of times you have rolled one or more dice
        int rollCount = 0;
        //Counts the number of times you have successfully rolled one or more dice
        int successCount = 1;
        //Counts the total number of dice you have rolled
        int diceCount = 0;
        //Counts how many cards to draw if you have a Farideh
        int drawCount = 0;
        bool Barbarian = false;
        Random rnd = new Random();
      //Input Vars
      Console.WriteLine("Delina Target:\n 1 - Wyll\n 2 - Pixie Guide\n 3 - Farideh, Devil\'s Chosen'");
      string input = Console.ReadLine();
      //Convert input to Boolean for Target Creature
      if (input == "1") {Wyll = true; Console.WriteLine("Targeting Wyll.\n");}
      if (input == "2") {Pixie = true; Console.WriteLine("Targeting Pixie.\n");}
      if (input == "3") {Farideh = true; Console.WriteLine("Targeting Farideh.\n");}
      
      Console.WriteLine("Total sources of advantage?: \n(Wyll, Pixie Guide, Barbarian Class, Krark's Thumb etc.)");
      string advantageInput = Console.ReadLine();
      int advantageSources = Convert.ToInt32(advantageInput);
      Console.WriteLine($"Rolling with " + (advantageSources + 1) + " dice.");
      
        while (rolling){
            //resets values from the previous roll
            numberOfDice = 1 + advantageSources;
            bool success = false;
            currentRoll = 1;
            rollResults.Clear();
            //Adds to the counter of rolls of one or more dice
            rollCount++;
            while (currentRoll < numberOfDice + 1){
                rollResults.Add(rnd.Next(1, 20));
                currentRoll++;
                diceCount++;
            }
            foreach (int result in rollResults)
            {
                if (result >= 15)
                {
                    success = true;
                    if (Farideh && result >= 10) drawCount++;
                    break;
                }
                else success = false;
            }
            if (!success)
            {
                Console.WriteLine(String.Join(",", rollResults));
                Console.WriteLine("The forces of chaos do not favour you this time. Create " + successCount + " copies\n");
                Console.WriteLine($"You rolled {rollCount} times with a total of {diceCount} dice");
                if (Barbarian){
                  Console.WriteLine($"Your rage flows across the battlefield. {rollCount} of your creatures gain +2/+0 and menace until end of turn!");
                }
                rolling = false;
                break; //Stop the Rolling!
            }
            else if (success)
            {
                successCount++;
                Console.WriteLine(String.Join(",", rollResults));
                Console.WriteLine("You get to roll again!\n");
                if (Wyll || Pixie) {
                  advantageSources++;
                  if (advantageSources >= 10){
                    Console.WriteLine("Whoa there, are you sure you want to keep rolling?");
                    Console.ReadLine();
                    rolling = false;
                  }
                }
                Console.WriteLine("Now rolling with " + (advantageSources + 1) +" dice!");
                Thread.Sleep(1000);
            }
        }
    }
}
