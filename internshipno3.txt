import csv
import os

def input_expense(expenses):
    date = input("Enter the date of the expense (YYYY-MM-DD): ")
    description = input("Enter the description of the expense: ")

    categories = {expense[2] for expense in expenses}
    categories = list(categories)

    print("Select a category by number:")
    for idx, category in enumerate(categories):
        print(f"{idx + 1}. {category}")
    print(f"{len(categories) + 1}. Create a new category")

    category_choice = int(input())
    if category_choice == len(categories) + 1:
        category = input("Enter the new category name: ")
    else:
        category = categories[category_choice - 1]

    price = float(input("Enter the price of the expense: "))
    expenses.append([date, description, category, price])

    with open("expenses.csv", mode='w', newline='') as file:
        writer = csv.writer(file)
        writer.writerows(expenses)

def view_expenses(expenses):
    print("Select an option:")
    print("1. View all expenses")
    print("2. View monthly expenses by category")

    view_choice = int(input())
    if view_choice == 1:
        for expense in expenses:
            print(expense)
    elif view_choice == 2:
        month = input("Enter the month (MM): ")
        year = input("Enter the year (YYYY): ")
        filtered_expenses = [expense for expense in expenses if expense[0][:4] == year and expense[0][5:7] == month]
        category_totals = {}
        for expense in filtered_expenses:
            category = expense[2]
            price = float(expense[3])
            if category in category_totals:
                category_totals[category] += price
            else:
                category_totals[category] = price
        for category, total in category_totals.items():
            print(f"Category: {category}, Total: {total}")

def load_expenses():
    if not os.path.exists("expenses.csv"):
        return []
    with open("expenses.csv", mode='r', newline='') as file:
        reader = csv.reader(file)
        return list(reader)

def main():
    expenses = load_expenses()

    while True:
        print("Select any option below:")
        print("1. Input a new expense")
        print("2. View expenses summary")

        choice = int(input())

        if choice == 1:
            input_expense(expenses)
        elif choice == 2:
            view_expenses(expenses)
        else:
            break

        repeat = input("Would you like to do something else (y/n)?\n")
        if repeat.lower() != "y":
            break

main()