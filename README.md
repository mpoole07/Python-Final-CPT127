# Python-Final-CPT127
Party planner program


#!/usr/bin/env python3
#
# Matthew Poole, April 26, 2026, CPT127-424, Final Project
#
# Program: Party Planner Program
# Purpose: Save a list of attendees going to a party including details such as their menu choices, attendee types,
#          and cost to the partyplanner.csv file. Allow attendees to be added, removed, or edited from the list and
#          save when changes are made.
#
# Associated file(s): partyplanner.csv

# Insert in the output student's name and exact date/time lab was run.
import datetime
print(f"Student Name: Matthew Poole")
labtime = datetime.datetime.now()
print(f"Lab Time: {labtime}")
print()

import csv

# declare constants such as the partyplanner file, valid menu options, valid types, and the regular price.
PARTYPLANNER = "partyplanner.csv"
VALID_MENUS = ["chicken", "beef", "vegetarian"]
VALID_TYPES = ["member", "guest"]
DEFAULT_PRICE = 25.00

# read what is in the partyplanner.csv file and append it to party_list in the correct format
def read_file():
    party_list = [] # initialize the party_list list
    try:
        with open(PARTYPLANNER, "r", newline="") as file:
            # run through each row and append its details to the list
            reader = csv.reader(file)
            for row in reader:
                if len(row) != 4: # if the row doesn't have the correct number of items (4), don't add it
                    continue
                name = row[0]
                menu_choice = row[1]
                attendee_type = row[2]
                try:
                    price = float(row[3]) # the price value should be convertable to a float, if not, assign default price ($25.00) to it
                except:
                    price = DEFAULT_PRICE
                party_list.append([name, menu_choice, attendee_type, price]) # append attendee to list
    except:
        save_list(party_list)
    return party_list

# show the commands for listing
def display_list_menu():
    print("\nLIST COMMAND MENU")
    print("0 - Go back")
    print("1 - List all attendees")
    print("2 - List Members Only")
    print("3 - List Guests Only")
    print("4 - List Total Fees Paid")
    print("5 - List Total of Menu Choices")

# take user's command input and call the proper function
def list_choice(party_list):
    while True:
        display_list_menu() # print the list command menu
        command = input("Command: ")
        print()
        
        if command == "0":
            break
        elif command == "1":
            list_all(party_list)
        elif command == "2":
            list_members(party_list)
        elif command == "3":
            list_guests(party_list)
        elif command == "4":
            list_total_fees(party_list)
        elif command == "5":
            list_menu_choices(party_list)
        else:
            print("Unknown command. Please try again.")

# list all of the attendees including details such as menu choice, type, and price
def list_all(party_list):
    if len(party_list) > 0:
        print("\n=" * 85) #decorative
        print(f"{'NAME':<20}\t{'MENU CHOICE':<20}{'TYPE':<20}{'PRICE':>15}") # print column headers
        print("-" * 85) # seperates column headers from list items

        # format and display all attendees and their details
        for attendee in party_list:
            print(f"{attendee[0]:<20}\t{attendee[1].capitalize():<20}{attendee[2].capitalize():<20}{f'${attendee[3]:.2f}':>15}")
        print("=" * 85)
        print(f"Total: {len(party_list)}") # list total number of all attendees
    else:
        print("There are no attendees in the list") # display if no attendees in list

# list all members attending
def list_members(party_list):
    matches = [] # initialize list to store matches

    # if the attendee's type is member, append them to the matches list
    for attendee in party_list:
        if attendee[2] == "member":
            matches.append(attendee)

    if len(matches) == 0: # if there are no matches, there are no members in the list
        print("\nThere are no members in the party list.\n")
    else:
        # format and display the matching attendees and their details
        print("\n=" * 85) #decorative
        print(f"{'NAME':<20}\t{'MENU CHOICE':<20}{'TYPE':<20}{'PRICE':>15}")
        print("-" * 85)
        for match in matches:
            print(f"{match[0]:<20}\t{match[1].capitalize():<20}{match[2].capitalize():<20}{f'${match[3]:.2f}':>15}")
        print("=" * 85)
        print(f"Total: {len(matches)}") # list total number of members

# list all guests attending
def list_guests(party_list):
    matches = [] # initialize list to store matches

    # if the attendee's type is guest, append them to the matches list
    for attendee in party_list:
        if attendee[2] == "guest":
            matches.append(attendee)

    if len(matches) == 0: # if there are no matches, there are no guests in the list
        print("\nThere are no guests on the party list.\n")
    else:
        # format and display the matching attendees and their details
        print("\n=" * 85) #decorative
        print(f"{'NAME':<20}\t{'MENU CHOICE':<20}{'TYPE':<20}{'PRICE':>15}")
        print("-" * 85)
        for match in matches:
            print(f"{match[0]:<20}\t{match[1].capitalize():<20}{match[2].capitalize():<20}{f'${match[3]:.2f}':>15}")
        print("=" * 85)
        print(f"Total: {len(matches)}") # list total number of guests

# add up all the fees and display the total
def list_total_fees(party_list):
    total = 0.00
    
    if len(party_list) > 0:
        # get the total by adding up for fees for each attendee
        for attendee in party_list:
            total += float(attendee[3])
        print(f"Total: ${total:.2f}")
    else:
        print("There are no attendees in the party list")

# calculate how many of each menu choice is being ordered and display the results
def list_menu_choices(party_list):
    chicken = 0
    beef = 0
    vegetarian = 0

    # count each occurance of a menu item in the list and display the totals
    for attendee in party_list:
        if attendee[1] == "chicken":
            chicken += 1
        elif attendee[1] == "beef":
            beef += 1
        elif attendee[1] == "vegetarian":
            vegetarian += 1
            
    print(f"\nChicken: {chicken}\nBeef: {beef}\nVegetarian: {vegetarian}\n")

# let the user search for an attendee based on a partial or full name
def search(party_list):
    name = input("Enter attendees name: ").strip()
    
    matches = [] # initialize list to store matches

    # if the name of the attendee at least partially matches the search name, append them to the matches list
    for attendee in party_list:
        if name.lower() in attendee[0].lower():
            matches.append(attendee)

    
    if len(matches) == 0: # run if there are no matches
        print(f"\nThere are no attendees with the name {name}.")
    else:
        # format and display the matching attendees and their details
        print("\n=" * 85) #decorative
        print(f"{'NAME':<20}\t{'MENU CHOICE':<20}{'TYPE':<20}{'PRICE':>15}")
        print("-" * 85)
        for m in matches:
            print(f"{m[0]:<20}\t{m[1].capitalize():<20}{m[2].capitalize():<20}{f'${m[3]:.2f}':>15}")
        print("=" * 85)

# allow the user to add a member or a guest to the list and decide their menu choice and save at the end			
def add(party_list):
    # get users input for name, menu choice, and attendee type
    name = input("Name: ").strip()

    if not name: # if the user inputs nothing this will run instead of adding an attendee with no name
        print("No name entered. No attendee was added.")
        return party_list
    
    while True:
        menu_choice = input("Menu choice (chicken, beef, or vegetarian): ").lower()
        if menu_choice in VALID_MENUS:
            break
        else:
            print(f"{menu_choice} is not a valid menu choice. Please try again")
    while True:
        attendee_type = input("Type (member/guest): ").lower()
        if attendee_type in VALID_TYPES:
            break
        else:
            print(f"{attendee_type} is not a valid type")

    # append new attendee to the party_list and save changes
    attendee = [name, menu_choice, attendee_type, DEFAULT_PRICE]
    party_list.append(attendee)
    print(f"{name} added as {attendee_type}.")
    save_list(party_list)

# remove an attendee based on name and save the list
def remove(party_list):
    name = input("Enter the name of the attendee you want to remove: ").strip()

    if not name: # if the user inputs nothing this will run instead of trying to remove every attendee
        print("No name entered. No attendees were removed.")
        return party_list

    new_list = [a for a in party_list if name.lower() not in a[0].lower()] # create a new list including all who are not being removed
    count = len(party_list) - len(new_list) # calculate how many have been removed

    if count == 0:
        print(f"There are no attendees with the name {name}")
    else:
        while True:
            # ask user if they are sure and either cancel action or remove attendee(s) from list
            choice = input((f"{count} attendee(s) will be removed from the list, are you sure you want to continue? (y/n): ").lower()).strip()
            if choice == "y":
                print(f"{count} attendee(s) were removed")
                save_list(new_list)
                return new_list
            elif choice == "n":
                print("Action canceled.")
                return party_list
            else:
                print("Invalid input, please enter 'y' or 'n'")

# allow the user to edit an attendee by entering new details and replacing the old version with the new
def edit(party_list):
    name = input("Enter of the name of the attendee you want to edit: ").strip()

    # if the user inputs nothing, cancel the edit
    if not name:
        print("No name entered. Edit cancelled.")
        return party_list

    # make a list to store matching attendees and their index # in party_list
    matches = [(i, a) for i, a in enumerate(party_list) if name.lower() in a[0].lower()]
    
    if len(matches) == 0:
        print(f"There are no attendees with the name {name}") # runs if no attendees matched the search
        return party_list
    elif len(matches) > 1: # runs if more than one attendees match the search
        print(f"There are {len(matches)} matches:")
        for i, (_, attendee) in enumerate(matches, start=1):
            print(f"{i}. {attendee[0]}") # list the matches

        # let the user choose which attendee to edit
        while True:
            try:
                choice = int(input("Select a number to edit (0 to cancel): "))
                if choice == 0:
                    return party_list
                elif 1 <= choice <= len(matches):
                    index = matches[choice - 1][0] # save the index value of the attendee from party_list so it can be replaced later
                    break
                else:
                    print("Invalid selection.")
            except:
                print("Enter a valid number.")
    else:
        index = matches[0][0] # save the index value of the attendee from party_list so it can be replaced later

    # have the user enter the attendee's new details
    print("Enter new details:")

    new_name = input("Name: ").strip()

    while True:
        menu_choice = input("Menu choice (chicken, beef, vegetarian): ").lower()
        if menu_choice in VALID_MENUS:
            break
        print(f"{menu_choice} is not a valid menu choice. Please try again.")

    while True:
        attendee_type = input("Type (member/guest): ").lower()
        if attendee_type in VALID_TYPES:
            break
        print(f"{attendee_type} is not a valid type. Please try again.")

    # replace the old attendee's details with the new one and save the changes
    party_list[index] = [new_name, menu_choice, attendee_type, DEFAULT_PRICE]

    save_list(party_list)
    print("Attendee updated.")

    return party_list

# save the party_list list to the partyplanner.csv file
def save_list(party_list):
    with open(PARTYPLANNER, "w", newline="") as file:
        writer = csv.writer(file)
        for attendee in party_list:
            writer.writerow([attendee[0], attendee[1], attendee[2], f"{attendee[3]}"]) # write each row of the list to the file in the correct order and format
    print("List saved successfully.")
    print()

# display the main command menu
def display_menu():
    print("\nCOMMAND MENU")
    print("0 – Exit program")
    print("1 – List")
    print("2 – Search")
    print("3 – Add attendee")
    print("4 – Remove attendee")
    print("5 - Edit attendee")

# call all functions need to run the program 
def main():
    print("The Party Planner Program") # display title
    
    party_list = read_file() # read from the csv file to the party list
    while True:
        display_menu() # display the command menu
        
        # allow user to choose what they want to do by entering an integer command
        command = input("Command: ")
        if command == "0":
            break
        elif command == "1":
            list_choice(party_list)
        elif command == "2":
            search(party_list)
        elif command == "3":
            add(party_list)
        elif command == "4":
            party_list = remove(party_list)
        elif command == "5":
            edit(party_list)
        else:
            print("Unknown command entered. Please try again.")
            
    # save the list after the user is done
    save_list(party_list)
    
    print("Bye!") # say bye

# check if this is the main module and if so run main()
if __name__ == "__main__":
    main()
