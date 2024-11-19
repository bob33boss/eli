import random

class Biome:
    def __init__(self, name, flora, fauna, challenge, resource):
        self.name = name
        self.flora = flora
        self.fauna = fauna
        self.challenge = challenge
        self.resource = resource
        self.discovered = False

    def discover(self):
        self.discovered = True
        print(f"\nYou've discovered the {self.name} biome!")
        print(f"Flora: {', '.join(self.flora)}")
        print(f"Fauna: {', '.join(self.fauna)}")
        print(f"Challenge: {self.challenge}")
        print(f"Resource: {self.resource}")

    def complete_challenge(self):
        if random.choice([True, False]):
            print(f"Challenge in {self.name} completed successfully!")
            return True
        else:
            print(f"Failed to overcome the challenge in {self.name}. Try again later.")
            return False

    def gather_resources(self):
        print(f"You gather {self.resource} from the biome.")
        return self.resource


class Game:
    def __init__(self):
        self.biomes = [
            Biome("Tropical Rainforest", ["trees", "vines", "orchids"], ["monkey", "toucan", "jaguar"], "deforestation", "water"),
            Biome("Desert", ["cacti", "succulents"], ["camel", "snake", "scorpion"], "drought", "sand"),
            Biome("Savanna", ["acacia", "grass"], ["lion", "elephant", "zebra"], "wildfires", "wood"),
            Biome("Ocean", ["seaweed", "plankton"], ["fish", "whale", "turtle"], "pollution", "saltwater"),
            Biome("Tundra", ["moss", "lichens"], ["polar bear", "arctic fox", "penguin"], "climate change", "ice")
        ]
        self.resources = 0
        self.explored_biomes = []
        self.game_over = False

    def show_status(self):
        print(f"\nResources: {self.resources}")
        print(f"Explored Biomes: {', '.join([biome.name for biome in self.explored_biomes])}")
        print("What would you like to do?")
        print("1. Explore a new biome")
        print("2. Check resources")
        print("3. Exit game")

    def explore(self):
        unvisited_biomes = [biome for biome in self.biomes if not biome.discovered]
        
        if not unvisited_biomes:
            print("\nYou've discovered all the biomes!")
            self.game_over = True
            return
        
        print("\nChoose a biome to explore:")
        for idx, biome in enumerate(unvisited_biomes):
            print(f"{idx + 1}. {biome.name}")

        choice = int(input("\nEnter the number of your choice: ")) - 1
        selected_biome = unvisited_biomes[choice]
        
        selected_biome.discover()
        self.explored_biomes.append(selected_biome)

        action = input(f"\nWould you like to:\n1. Gather resources\n2. Complete challenge\nEnter your choice: ")

        if action == "1":
            self.resources += selected_biome.gather_resources()
        elif action == "2":
            if selected_biome.complete_challenge():
                print(f"Challenge in {selected_biome.name} successfully completed!")
                self.resources += 10
            else:
                print(f"Challenge failed. Try again next time!")

    def run(self):
        print("Welcome to the Biome Exploration Game!")
        
        while not self.game_over:
            self.show_status()
            choice = input("\nEnter your choice: ")

            if choice == "1":
                self.explore()
            elif choice == "2":
                print(f"\nYou have {self.resources} resources.")
            elif choice == "3":
                print("\nThanks for playing! Goodbye!")
                self.game_over = True
            else:
                print("Invalid choice. Please try again.")

# Run the game
game = Game()
game.run()
