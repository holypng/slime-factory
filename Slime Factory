import random
import time
import colorama
from colorama import Fore, Style
colorama.init()

class Slime:
    def __init__(self, name, traits, base_value):
        self.name = name
        self.traits = traits
        self.base_value = base_value
        self.size = random.uniform(0.5, 1.5)
        self.hp = 10 + len(self.traits)
        if "legendary" in self.traits:
            self.hp += 2
        elif "mythic" in self.traits:
            self.hp += 4
        elif "unique" in self.traits:
            self.hp += 6
        elif "unbound" in self.traits:
            self.hp += 8
    def get_value(self):
        rarity_factor = len(self.traits)
        return self.base_value * rarity_factor * self.size
    def __str__(self):
        trait_str = ""
        for trait in self.traits:
            if trait in ["bounded"]:
                trait_str += Fore.WHITE + trait + " "
            elif trait in ["unique"]:
                trait_str += Fore.BLUE + trait + " "

            elif trait in ["produced"]:
                trait_str += Fore.CYAN + trait + " "
            elif trait in ["crafted"]:
                trait_str += Fore.LIGHTCYAN_EX + trait + " "
            elif trait in ["legendary"]:
                trait_str += Fore.YELLOW + trait + " "
            elif trait in ["mythic"]:
                trait_str += Fore.LIGHTMAGENTA_EX + trait + " "
            elif trait in ["unbound"]:
                trait_str += Fore.BLACK + trait + " "
            elif trait in ["corrupt"]:
                trait_str += Fore.RED + trait + " "
            else:
                trait_str += trait + " "
        return f"{trait_str}{Style.RESET_ALL}{self.name}"

class Inventory:
    def __init__(self):
        self.slimes = []
        self.money = 0
        self.combat_slime = None
    def add_slime(self, slime):
        self.slimes.append(slime)
    def remove_slime(self, slime):
        self.slimes.remove(slime)
    def sell_slime(self, slime):
        value = slime.get_value()
        self.money += value
        self.remove_slime(slime)
        print(f"{Fore.BLACK}Sold {slime} for ${value:.2f}{Style.RESET_ALL}")
    def list_slimes(self):
        print(f"{Fore.BLACK}\nInventory:{Style.RESET_ALL}")
        for i, slime in enumerate(self.slimes):
            print(f"{Fore.BLACK}- {i}. {slime}{Style.RESET_ALL}")
    def __str__(self):
        return f"{Fore.BLACK}Money: ${self.money:.2f}, XP: {xp}, Slimes: {len(self.slimes)}{Style.RESET_ALL}"

inventory = Inventory()

adjectives = [
    "earthy", "firey", "shiny", "watery", "stony", "bubbly", "spongy", "dusty", "crackling", "slippery", "charged", "stained", "glossy", "spiky", "sticky", "sparkling", "wet", "shimmering", "tough", "sparkling", "hard", "soft", "rough", "melting", "metallic", "legendary", "mythic", "unique", "unbound", "magnetic", "fluffy", "slimy", "gooey", "bouncy", "smooth", "rough", "prickly", "pulsating", "stinky", "reflective", "vibrant", "dark", "elastic"
]
slime_names = {
    "base": ["slime"],
    "grassland": ["leafy slime", "mossy slime", "grassy slime", "flowery slime"],
    "forest": ["woody slime", "mushroom slime", "fern slime", "frog slime"],
    "cave": ["rocky slime", "crystal slime", "obsidian slime", "stalagmite slime"],
    "snowplains": ["frosty slime", "snowy slime", "ice slime"],
    "mountains": ["granite slime", "volcanic slime", "basalt slime"],
    "swamp": ["muddy slime", "sludge slime", "vine slime"],
    "desert": ["sand slime", "cactus slime", "tumbleweed slime"]
}

def craft_slimes(slime1, slime2):
    global xp
    if xp >= 15:
        xp -= 15
        new_name = " ".join(set(slime1.traits + slime2.traits))
        if ["firey", "watery"] == sorted([slime1.traits[0], slime2.traits[0]]):
            new_name = "smoky slime"
        elif sorted([slime1.traits[0], slime2.traits[0]]) == ["shimmering", "sparkling"]:
            new_name = "glowing slime"
        elif ["bubbly", "spongy"] == sorted([slime1.traits[0], slime2.traits[0]]):
            new_name = "foamy slime"
        elif ["sparkling", "charged"] == sorted([slime1.traits[0], slime2.traits[0]]):
            new_name = "electric slime"
        elif ["metallic", "magnetic"] == sorted([slime1.traits[0], slime2.traits[0]]):
            new_name = "steel slime"
        new_traits = list(set(slime1.traits + slime2.traits))
        base_value = (slime1.get_value() + slime2.get_value()) / 2
        new_slime = Slime(new_name, new_traits, base_value)
        if random.random() < 0.5:
            new_slime.traits = slime1.traits
        else:
            new_slime.traits = slime2.traits
        return new_slime
    else:
        print(f"{Fore.BLACK}You need 15 XP to craft.{Style.RESET_ALL}")
        return None

def produce_slime(slime):
    production_cost = slime.get_value() * 0.5
    if inventory.money >= production_cost:
        inventory.money -= production_cost
        new_slime = Slime(slime.name, slime.traits, slime.base_value)
        if random.random() < 0.05:
            new_slime.traits.append("produced")
        inventory.add_slime(new_slime)
        print(f"{Fore.BLACK}Produced {new_slime} for ${production_cost:.2f}{Style.RESET_ALL}")
    else:
        print(f"{Fore.BLACK}Not enough money to produce this slime.{Style.RESET_ALL}")

def capture_slime(area):
    slime_name = random.choice(slime_names[area])
    slime_traits = random.sample(adjectives, random.randint(1, 3))
    if random.random() < 1/20:
        slime_traits.append("corrupt")
    base_value = 10 + random.randint(1, 5)
    slime = Slime(slime_name, slime_traits, base_value)
    print(f"{Fore.BLACK}You encountered a {slime}!{Style.RESET_ALL}")
    time.sleep(1)
    if "corrupt" in slime.traits:
        combat(slime)
    else:
        if capture_minigame(slime.size, slime.traits):
            inventory.add_slime(slime)
            print(f"{Fore.BLACK}You captured the {slime}!{Style.RESET_ALL}")
        else:
            print(f"{Fore.BLACK}The slime rolled away..{Style.RESET_ALL}")

def combat(slime):
    global inventory
    if inventory.combat_slime is None:
        print(f"{Fore.BLACK}Choose a slime from your inventory to fight with:{Style.RESET_ALL}")
        inventory.list_slimes()
        slime_index = int(input(f"{Fore.BLACK}Enter the index of the slime you want to use: {Style.RESET_ALL}"))
        inventory.combat_slime = inventory.slimes[slime_index]
        print(f"{Fore.BLACK}You selected {inventory.combat_slime}{Style.RESET_ALL}")
        time.sleep(1)
        combat_round(slime)
    else:
        combat_round(slime)

def combat_round(slime):
    global inventory, xp
    enemy_hp = slime.hp
    player_hp = inventory.combat_slime.hp
    print(f"{Fore.BLACK}\n{inventory.combat_slime} (HP: {player_hp}) vs {slime} (HP: {enemy_hp}){Style.RESET_ALL}")
    while player_hp > 0 and enemy_hp > 0:
        action = input(f"{Fore.BLACK}Choose your action (attack/guard): {Style.RESET_ALL}").lower()
        if action == "attack":
            enemy_hp -= 5
            print(f"{Fore.BLACK}{inventory.combat_slime} attacked! {slime} now has {enemy_hp} HP.{Style.RESET_ALL}")
        elif action == "guard":
            print(f"{Fore.BLACK}{inventory.combat_slime} guarded.{Style.RESET_ALL}")
        else:
            print(f"{Fore.BLACK}Invalid action.{Style.RESET_ALL}")
        time.sleep(1)
        enemy_action = random.choice(["attack", "guard"])
        if enemy_action == "attack":
            player_hp -= 5
            print(f"{Fore.BLACK}{slime} attacked! {inventory.combat_slime} now has {player_hp} HP.{Style.RESET_ALL}")
        elif enemy_action == "guard":
            print(f"{Fore.BLACK}{slime} guarded.{Style.RESET_ALL}")
        time.sleep(1)
        print(f"{Fore.BLACK}\n{inventory.combat_slime} (HP: {player_hp}) vs {slime} (HP: {enemy_hp}){Style.RESET_ALL}")
    if player_hp <= 0:
        print(f"{Fore.BLACK}You lost! {slime} defeated {inventory.combat_slime}.{Style.RESET_ALL}")
        inventory.combat_slime = None
    else:
        print(f"{Fore.BLACK}You won! {inventory.combat_slime} defeated {slime}!{Style.RESET_ALL}")
        inventory.combat_slime = None
    print(f"{Fore.BLACK}\nYou defeated {slime}!{Style.RESET_ALL}")
    xp_gain = 10 + len(slime.traits) * 4
    if "legendary" in slime.traits:
        xp_gain += 5
    elif "mythic" in slime.traits:
        xp_gain += 7
    elif "unique" in slime.traits:
        xp_gain += 10
    elif "unbound" in slime.traits:
        xp_gain += 12
    print(f"{Fore.BLACK}You gained {xp_gain} XP.{Style.RESET_ALL}")
    xp += xp_gain

def capture_minigame(slime_size, slime_traits):
    time_limit = 2
    if "legendary" in slime_traits:
        time_limit = 1.5
    elif "mythic" in slime_traits:
        time_limit = 1
    elif "unique" in slime_traits:
        time_limit = 0.5
    elif "unbound" in slime_traits:
        time_limit = 0.25
    print(f"{Fore.BLACK}You have {time_limit:.1f} seconds to capture!{Style.RESET_ALL}")
    start_time = time.time()
    input()
    end_time = time.time()
    time_taken = end_time - start_time
    print(f"{Fore.BLACK}Time taken: {time_taken:.2f} seconds{Style.RESET_ALL}")
    if time_taken < time_limit:
        return True
    else:
        return False

def explore(area):
    print(f"{Fore.BLACK}exploring the {area}...{Style.RESET_ALL}")
    time.sleep(1)
    if random.random() < 0.7:
        capture_slime(area)
    else:
        print(f"{Fore.BLACK}nothing found here.{Style.RESET_ALL}")

def save_game():
    with open("save_data.txt", "w") as f:
        f.write(f"{inventory.money}\n")
        for slime in inventory.slimes:
            f.write(f"{slime.name},{','.join(slime.traits)},{slime.base_value},{slime.size}\n")
    print(f"{Fore.BLACK}game saved!{Style.RESET_ALL}")

def load_game():
    try:
        with open("save_data.txt", "r") as f:
            inventory.money = float(f.readline().strip())
            for line in f:
                name, traits, base_value, size = line.strip().split(",")
                inventory.slimes.append(Slime(name, traits.split(","), float(base_value), float(size)))
        print(f"{Fore.BLACK}Game loaded{Style.RESET_ALL}")
    except FileNotFoundError:
        print(f"{Fore.BLACK}No save data found.{Style.RESET_ALL}")

def inspect_slime(slime):
    descriptions = {
        "leafy slime": ["A vibrant green slime covered in leaves.", "A mischievous slime that loves to play hide and seek in the tall grass.", "A gentle slime that enjoys basking in the sun."],
        "mossy slime": ["A soft slime with a velvety green moss covering.", "A slow-moving slime that blends seamlessly with its mossy surroundings.", "A slime that brings a touch of nature wherever it goes."],
        "grassy slime": ["A slime that looks like a patch of grass.", "A slime that enjoys rolling around in the meadows.", "A slime that brings a touch of spring to any room."],
        "woody slime": ["A tough slime with a rough bark-like texture.", "A strong slime that can withstand even the harshest conditions.", "A slime that enjoys climbing trees."],
        "mushroom slime": ["A colorful slime with a mushroom-shaped cap.", "A slime that glows with a faint bioluminescence.", "A slime that enjoys spending time in damp, shady areas."],
        "fern slime": ["A slime with fern-like fronds.", "A slime that enjoys the shade of the forest canopy.", "A slime that brings a touch of tranquility to any space."],
        "frog slime": ["A slime that resembles a frog.", "A slime that enjoys hopping around the forest floor.", "A slime that brings a touch of froginess to any adventure."],
        "rocky slime": ["A sturdy slime with a hard, rocky exterior.", "A slime that enjoys rolling around in the caves.", "A slime that brings a touch of earthiness to any collection."],
        "crystal slime": ["A translucent slime with sparkling crystals embedded in its body.", "A slime that refracts light in mesmerizing ways.", "A slime that brings a touch of shinyness to any adventure."],
        "obsidian slime": ["A dark and mysterious slime with a smooth, obsidian-like surface.", "A slime that absorbs light, creating a sense of shadow.", "A slime that brings a touch of elegance to any collection."],
        "frosty slime": ["A cold slime with a icy blue glow.", "A slime that enjoys sliding on the snow.", "A slime that brings a touch of winter to any space."],
        "snowy slime": ["A fluffy slime with a soft, white texture.", "A slime that enjoys building snow forts.", "A slime that brings a touch of cold to your collection."],
        "granite slime": ["A strong slime with a gray, granite-like exterior.", "A slime that enjoys rolling down mountains.", "A slime that brings a touch of ruggedness to any collection."],
        "volcanic slime": ["A fiery slime with a molten orange glow.", "A slime that enjoys basking in the heat of a volcano.", "A slime that brings a touch of danger to any adventure."],
        "basalt slime": ["A dark and mysterious slime with a basalt-like surface.", "A slime that enjoys exploring the rugged terrain of mountains.", "A slime that brings a touch of mystery to any collection."],
        "muddy slime": ["A slimy slime with a dark brown, muddy texture.", "A slime that enjoys wallowing in the mud.", "A slime that brings a touch of earthiness to any space."],
        "sludge slime": ["A disgusting slime with a foul odor and a slimy texture.", "A slime that enjoys living in dark, damp places.", "A slime that brings a touch of horror to any adventure."],
        "vine slime": ["A slime with vines growing out of its body.", "A slime that enjoys climbing trees and other plants.", "A slime that brings a touch of nature to any collection."],
        "sand slime": ["A dry and gritty slime with a golden brown color.", "A slime that enjoys rolling around in the desert.", "A slime that brings a touch of desert to any space."],
        "cactus slime": ["A prickly slime with spines growing out of its body.", "A slime that enjoys living in hot, dry climates.", "A slime that brings a touch of danger to any adventure."],
        "tumbleweed slime": ["A rolling slime that resembles a tumbleweed.", "A slime that enjoys being blown around by the wind.", "A slime that brings a touch of weediness to any adventure."],
        "smoky slime": ["A mysterious slime with a wispy, smoky appearance.", "A slime that enjoys floating around in the air.", "A slime that adds a foggy feeling to any adventure."],
        "glowing slime": ["A luminous slime that glows with a soft light.", "A slime that enjoys illuminating dark places.", "A slime that brings a touch of warmth to any place."],
        "foamy slime": ["A bubbly slime that resembles a foamy wave.", "A slime that enjoys bouncing around.", "A slime that brings a touch of foaminess to any adventure."],
        "electric slime": ["A charged slime that crackles with electricity.", "A slime that enjoys shocking unsuspecting objects.", "A slime that brings a touch of excitement to any adventure."],
        "metallic slime": ["A somewhat rusty slime that often gets stuck to magnets.", "A slime that enjoys banging things and making a lot of noise.", "A slime that brings a touch of metal to any adventure."],
        "magnetic slime": ["A very attracting slime that often gets stuck to metal things.", "A slime that enjoys climbing up metal structures.", "A slime that brings a touch of magnetism to any place."],
        "steel slime": ["A very strong slime that often gets stuck to magnets.", "A slime that enjoys banging metal things together to make a lot of noise.", "A slime that brings a touch of steel to any collection."]
    }
    description = random.choice(descriptions.get(slime.name, ["A fascinating slime with unique traits.", "A slime that is sure to bring joy to any collector.", "A slime that is full of surprises."]))
    print(f"{Fore.BLACK}You are inspecting a {slime}. {description} It can be sold for ${slime.get_value():.2f}{Style.RESET_ALL}")

def combat_system(slime):
    global xp, combat_slot
    player_slime = inventory.slimes[combat_slot]
    player_hp = 10 + len(player_slime.traits)
    if "legendary" in player_slime.traits:
        player_hp += 2
    elif "mythic" in player_slime.traits:
        player_hp += 3
    elif "unbound" in player_slime.traits:
        player_hp += 5

    enemy_hp = 10 + len(slime.traits)
    if "legendary" in slime.traits:
        enemy_hp += 2
    elif "mythic" in slime.traits:
        enemy_hp += 4
    elif "unique" in slime.traits:
        enemy_hp += 6
    elif "unbound" in slime.traits:
        enemy_hp += 8

    print(f"{Fore.BLACK}Your {player_slime} ({player_hp} HP) vs. {slime} ({enemy_hp} HP){Style.RESET_ALL}")
    while player_hp > 0 and enemy_hp > 0:
        print(f"{Fore.BLACK}\nChoose your action:{Style.RESET_ALL}")
        print(f"{Fore.BLACK}1. Attack{Style.RESET_ALL}")
        print(f"{Fore.BLACK}2. Guard{Style.RESET_ALL}")

        choice = input(f"{Fore.BLACK}Enter your choice: {Style.RESET_ALL}")
        if choice == "1":
            player_damage = 2 + len(player_slime.traits) * 0.5
            print(f"{Fore.BLACK}You attack for {player_damage:.1f} damage!{Style.RESET_ALL}")
            enemy_hp -= player_damage
        elif choice == "2":
            print(f"{Fore.BLACK}You guard.{Style.RESET_ALL}")
        else:
            print(f"{Fore.BLACK}Invalid choice.{Style.RESET_ALL}")

        if enemy_hp > 0:
            enemy_choice = random.choice(["attack", "guard"])
            if enemy_choice == "attack":
                enemy_damage = 2 + len(slime.traits) * 0.5
                print(f"{Fore.BLACK}{slime} attacks for {enemy_damage:.1f} damage!{Style.RESET_ALL}")
                player_hp -= enemy_damage
            elif enemy_choice == "guard":
                print(f"{Fore.BLACK}{slime} guards.{Style.RESET_ALL}")
            time.sleep(1)

    if player_hp > 0:
        print(f"{Fore.BLACK}\nYou defeated {slime}!{Style.RESET_ALL}")
        xp_gain = 15 + len(slime.traits) * 4
        if "legendary" in slime.traits:
            xp_gain += 5
        elif "mythic" in slime.traits:
            xp_gain += 7
        elif "unique" in slime.traits:
            xp_gain += 10
        elif "unbound" in slime.traits:
            xp_gain += 10
        print(f"{Fore.BLACK}You gained {xp_gain} XP.{Style.RESET_ALL}")
        xp += xp_gain
    else:
        print(f"{Fore.BLACK}\nYou were defeated by {slime}!{Style.RESET_ALL}")

def main():
    global xp, combat_slot
    xp = 0
    combat_slot = 0 
    while True:
        print(f"{Fore.BLACK}\nSlime Factory{Style.RESET_ALL}")
        print(f"{Fore.BLACK}1. Explore{Style.RESET_ALL}")
        print(f"{Fore.BLACK}2. Inventory{Style.RESET_ALL}")
        print(f"{Fore.BLACK}3. Craft{Style.RESET_ALL}")
        print(f"{Fore.BLACK}4. Produce{Style.RESET_ALL}")
        print(f"{Fore.BLACK}5. Sell{Style.RESET_ALL}")
        print(f"{Fore.BLACK}6. Save{Style.RESET_ALL}")
        print(f"{Fore.BLACK}7. Load{Style.RESET_ALL}")
        print(f"{Fore.BLACK}8. Inspect{Style.RESET_ALL}")
        print(f"{Fore.BLACK}9. Exit{Style.RESET_ALL}")

        choice = input(f"{Fore.BLACK}Enter your choice: {Style.RESET_ALL}")

        if choice == "1":
            print(f"{Fore.BLACK}\nWhere do you want to explore?{Style.RESET_ALL}")
            print(f"{Fore.BLACK}1. Grassland{Style.RESET_ALL}")
            print(f"{Fore.BLACK}2. Forest{Style.RESET_ALL}")
            print(f"{Fore.BLACK}3. Cave{Style.RESET_ALL}")
            print(f"{Fore.BLACK}4. Snowplains{Style.RESET_ALL}")
            print(f"{Fore.BLACK}5. Mountains{Style.RESET_ALL}")
            print(f"{Fore.BLACK}6. Swamp{Style.RESET_ALL}")
            print(f"{Fore.BLACK}7. Desert{Style.RESET_ALL}")

            explore_choice = input(f"{Fore.BLACK}Enter your choice: {Style.RESET_ALL}")

            if explore_choice == "1":
                explore("grassland")
            elif explore_choice == "2":
                explore("forest")
            elif explore_choice == "3":
                explore("cave")
            elif explore_choice == "4":
                explore("snowplains")
            elif explore_choice == "5":
                explore("mountains")
            elif explore_choice == "6":
                explore("swamp")
            elif explore_choice == "7":
                explore("desert")
            else:
                print(f"{Fore.BLACK}invalid choice.{Style.RESET_ALL}")

        elif choice == "2":
            print(f"{Fore.BLACK}{inventory}{Style.RESET_ALL}")
            inventory.list_slimes()

        elif choice == "3":
            if len(inventory.slimes) < 2:
                print(f"{Fore.BLACK}you need at least two slimes to craft.{Style.RESET_ALL}")
            else:
                inventory.list_slimes()
                slime1_index = int(input(f"{Fore.BLACK}choose the first slime (index): {Style.RESET_ALL}"))
                slime2_index = int(input(f"{Fore.BLACK}choose the second slime (index): {Style.RESET_ALL}"))
                slime1 = inventory.slimes[slime1_index]
                slime2 = inventory.slimes[slime2_index]
                if ("firey" in slime1.traits and "watery" in slime2.traits) or ("watery" in slime1.traits and "firey" in slime2.traits):
                    new_slime = Slime("smoky slime", ["crafted"], (slime1.get_value() + slime2.get_value()) / 2)
                elif ("shimmering" in slime1.traits and "sparkling" in slime2.traits) or ("sparkling" in slime1.traits and "shimmering" in slime2.traits):
                    new_slime = Slime("glowing slime", ["crafted"], (slime1.get_value() + slime2.get_value()) / 2)
                elif ("bubbly" in slime1.traits and "spongy" in slime2.traits) or ("spongy" in slime1.traits and "bubbly" in slime2.traits):
                    new_slime = Slime("foamy slime", ["crafted"], (slime1.get_value() + slime2.get_value()) / 2)
                elif ("metallic" in slime1.traits and "magnetic" in slime2.traits) or ("magnetic" in slime1.traits and "metallic" in slime2.traits):     new_slime = Slime("steel slime", ["crafted"], (slime1.get_value() + slime2.get_value()) / 2)
                elif ("sparkling" in slime1.traits and "charged" in slime2.traits) or ("charged" in slime1.traits and "sparkling" in slime2.traits):     new_slime = Slime("electric slime", ["crafted"], (slime1.get_value() + slime2.get_value()) / 2)
                else:
                    new_slime = craft_slimes(slime1, slime2)
                if new_slime is not None:
                    inventory.add_slime(new_slime)
                    inventory.remove_slime(slime1)
                    inventory.remove_slime(slime2)
                    print(f"{Fore.BLACK}You crafted a {new_slime}!{Style.RESET_ALL}")

        elif choice == "4":
            if len(inventory.slimes) == 0:
                print(f"{Fore.BLACK}you dont have any slimes to produce.{Style.RESET_ALL}")
            else:
                inventory.list_slimes()
                slime_index = int(input(f"{Fore.BLACK}choose a slime to produce (index): {Style.RESET_ALL}"))
                slime = inventory.slimes[slime_index]
                produce_slime(slime)

        elif choice == "5":
            if len(inventory.slimes) == 0:
                print(f"{Fore.BLACK}you dont have any slimes to sell.{Style.RESET_ALL}")
            else:
                inventory.list_slimes()
                slime_index = int(input(f"{Fore.BLACK}choose a slime to sell (index): {Style.RESET_ALL}"))
                slime = inventory.slimes[slime_index]
                inventory.sell_slime(slime)
        elif choice == "6":
            save_game()
        elif choice == "7":
            load_game()
        elif choice == "8":
            if len(inventory.slimes) == 0:
                print(f"{Fore.BLACK}you don't have any slimes to inspect.{Style.RESET_ALL}")
            else:
                inventory.list_slimes()
                slime_index = int(input(f"{Fore.BLACK}choose a slime to inspect (index): {Style.RESET_ALL}"))
                slime = inventory.slimes[slime_index]
                inspect_slime(slime)
        elif choice == "9":
            break

        else:
            print(f"{Fore.BLACK}invalid choice.{Style.RESET_ALL}")

if __name__ == "__main__":
    main()
