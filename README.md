import csv
import os

accounts = {}

class Account:
    def __init__(self, acc_no, name, acc_type, balance):
        self.acc_no = acc_no
        self.name = name
        self.acc_type = acc_type
        self.balance = balance

    def deposit(self, amount):
        self.balance += amount

    def withdraw(self, amount):
        if amount <= self.balance:
            self.balance -= amount
            return True
        else:
            return False


def save_accounts():
    with open("accounts.csv", "w", newline='') as f:
        writer = csv.writer(f)
        writer.writerow(["AccNo", "Name", "Type", "Balance"])
        for acc in accounts.values():
            writer.writerow([acc.acc_no, acc.name, acc.acc_type, acc.balance])


def load_accounts():
    if os.path.exists("accounts.csv"):
        with open("accounts.csv", "r") as f:
            reader = csv.reader(f)
            next(reader)
            for row in reader:
                acc_no, name, acc_type, balance = row
                accounts[acc_no] = Account(acc_no, name, acc_type, float(balance))


def create_account():
    acc_no = input("Enter Account Number: ")
    if acc_no in accounts:
        print("Account already exists!")
        return

    name = input("Enter Name: ")
    acc_type = input("Enter Type (Savings/Current): ")
    balance = float(input("Enter Initial Balance: "))

    accounts[acc_no] = Account(acc_no, name, acc_type, balance)
    print("Account Created Successfully")


def deposit():
    acc_no = input("Enter Account Number: ")
    if acc_no in accounts:
        amount = float(input("Enter Amount: "))
        if amount > 0:
            accounts[acc_no].deposit(amount)
            print("Deposit Successful")
        else:
            print("Invalid Amount")
    else:
        print("Account Not Found")


def withdraw():
    acc_no = input("Enter Account Number: ")
    if acc_no in accounts:
        amount = float(input("Enter Amount: "))
        if amount > 0:
            success = accounts[acc_no].withdraw(amount)
            if success:
                print("Withdrawal Successful")
            else:
                print("Insufficient Balance")
        else:
            print("Invalid Amount")
    else:
        print("Account Not Found")


def transfer():
    from_acc = input("From Account: ")
    to_acc = input("To Account: ")

    if from_acc in accounts and to_acc in accounts:
        amount = float(input("Enter Amount: "))
        if amount > 0:
            if accounts[from_acc].withdraw(amount):
                accounts[to_acc].deposit(amount)
                print("Transfer Successful")
            else:
                print("Insufficient Balance")
        else:
            print("Invalid Amount")
    else:
        print("Invalid Account Numbers")


def view_accounts():
    print("\n------ ALL ACCOUNTS ------")
    for acc in accounts.values():
        print(acc.acc_no, acc.name, acc.acc_type, acc.balance)


def menu():
    load_accounts()

    while True:
        print("\n===== SMART BANKING SYSTEM =====")
        print("1. Create Account")
        print("2. Deposit")
        print("3. Withdraw")
        print("4. Transfer")
        print("5. View Accounts")
        print("6. Exit")

        choice = input("Enter choice: ")

        if choice == "1":
            create_account()
        elif choice == "2":
            deposit()
        elif choice == "3":
            withdraw()
        elif choice == "4":
            transfer()
        elif choice == "5":
            view_accounts()
        elif choice == "6":
            save_accounts()
            print("Data Saved. Exiting...")
            break
        else:
            print("Invalid Choice")


menu()
