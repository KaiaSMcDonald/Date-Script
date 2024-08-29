# Date-Script
# Requesting user to input the name of who is on the date with them
name = input("Enter the name of the person you are on date with:")

# Print the user's input 
print("I am on a date with " + name + ".")

# Requesting user to input their budget which is a integer
budget = input ("Enter your budget:")
budget_1 = float(budget)

#Print the use's budget 
print("This is my budget for today's date: " + budget + ".")

#Breakdown of the different categories of the restaurants menu 
#Appetizers = Chicken Wings, Cheese Quesadillas, Crab cakes 
#Entree = Shrimp Alfredo, Cobb Salad, Barbecue Ribs and mash potatoes
#Dessert = Strawberry Cheesecake, Ice cream, Brownie cake
#Beverages = Sprite, Lemonade, Water

menu ={
    "Appetizers":{
        "Chicken Wings":{"Price":8.00, "Ingredients": ["Chicken Wings", "Butter", "Hot Sauce", "Black pepper"], "Calories":720},
        "Cheese Quesadillas":{"Price":6.00, "Ingredients": ["Flour tortilla", "Cheese"], "Calories":528},
        "Crab cakes":{"Price":13.00, "Ingredients": ["Crab meat", "Bread crumbs", "Mayonnaise", "Eggs", "Mustard"], "Calories":160}
    },
    "Entree":{
        "Shrimp Alfredo":{"Price":25.00, "Ingredients": ["Shrimp", "Pasta", "butter", "cream sauce"], "Calories":330},
        "Cobb Salad":{"Price":20.00, "Ingredients": ["Lettuce", "bacon", "eggs", "cheese", "avocado", "chicken breast"], "Calories":623},
        "Barbecue Ribs and mash potatoes":{"Price":35.00, "Ingredients": ["Ribs", "Barbecue sauce", "potatoes", "butter", "milk", "black pepper"], "Calories": 550},
    },
    "Dessert":{
        "Strawberry Cheesecake":{"Price":15.00, "Ingredients": ["Strawberry", "Sugar", "Graham cracker crumbs", "butter"], "Calories":374},
        "Ice cream":{"Price":10.00, "Ingredients": ["Vanilla Extract", "Sugar", "Milk", "Water"], "Calories":137},
        "Brownie cake":{"Price":15.00, "Ingredients": ["Brownie mix", "Sugar", "Flour", "Cocoa powder"], "Calories":369},
    },
    "Beverages":{
        "Sprite":{"Price":7.00, "Ingredients": ["Sprite"], "Calories":192},
        "Lemonade":{"Price":10.00, "Ingredients": ["Lemons", "Sugar", "Honey", "Water"], "Calories":53},
        "Water":{"Price":5.00, "Ingredients": ["Water"], "Calories":0},
    }
}

# Define the function which is necessary to print out the menu in a formatted/organized way
def print_menu(menu):

# Loop through each category that is in the menu 
    for category, items in menu.items():
        print(f"{category}:") # Print the name of the category

#Loop through each item in the category 
        for dish, details in items.items():
            ingredients = ", ".join(details["Ingredients"]) # Puts commas in the string to seperate the ingredients
# Print details that belong to each dish and beverage on the menu 
            print(f"  {dish}:") # Prints out the name of the dish 
            print(f"  Price: ${details['Price']}") #Prints out the price of the dish 
            print(f"  Ingredients: {ingredients}") #Prints out the list of ingredients 
            print(f"  Calories: {details['Calories']}") #Prints the calories for the dish or beverage

print_menu(menu) # Call the function to print out the menu

print() #Blank line

def get_user_choices(menu,initial_balance):

    menu_choices = [] #List to store all of the user's choices from the menu
    total_cost = 0 #Variable to keep track of the total cost of the user's order

# Repeatedly prompts the user for input until they write done
    while True:
        choice_input = input("Enter your choice(s) seperated by commas (or type 'done' to finish ordering): ")

        if choice_input.lower() == 'done':  # Allows the user to write done in any format so their input session won't be terminated
            break 

        if not choice_input:
            print("No input provided. Please enter a choice")
            continue

        choices = [choice.strip() for choice in choice_input.split(',')] #Splits the user's input into a list of choices by commas and get rid of extra whitespace
        
        for choice in choices: #Iterate over each choice 
            valid_choice = False # Tracks whether a choice is valid
            for category, items in menu.items(): #iterates over the items in the menu dictionary
                if choice in items: # Making sure the users input is valid
                    valid_choice = True
                    item_price = items[choice]['Price'] #Get the price of the item selected
                    if total_cost + item_price <= initial_balance: #checking if the user has enough money to purchase item
                        menu_choices.append(choice) # a new item is added to the order
                        total_cost += item_price #add the item's price to the total cost 
                        print(f"{choice} has been added to your order.")
                        print(f"Total cost so far: ${total_cost:}") #Display the total cost thus far
                        print(f"Remaining balance: ${initial_balance - total_cost:}") #Display the remaining balance 
                    else:
                        print("Insufficient funds for this item")
                    break  

            if not valid_choice:
                print("Invalid choice. Please choose something from the menu.")
    remaining_balance = initial_balance - total_cost #How the remaning balance is calaculated
    return menu_choices, remaining_balance
    
initial_balance = float(input("Enter your budget: $")) #The user will input their budget because that is their initial balance
user_choices, remaining_balance = get_user_choices(menu,initial_balance) #Calling the function and retrieving the results

#Prints out the user's choices
print ("Your order summary:")
for choice in user_choices: #Loop through the choices the user made
    print(choice)

print(f"Remaining balance: ${remaining_balance:}") #Display the final remaining balance

#Ask the user to confirm payment 
confirmation = input(f"Your total cost is ${total_cost:}. Do you want to pay this amount? (yes/no):")
if confirmation == 'yes':
    print(f"Payment successful. Your final remaining balance is ${remaining_balance}.")
        
elif confirmation == 'no':
    print("Order canceled. No payment has been made.")
