class Bank:
    name = 'International Bank'
    clients = []

    def update_db(self, client):
        self.clients.append(client)

    def authentication(self, name, account_number):
        for client in self.clients:
            if name == client.account['name'] and account_number == client.account['account_number']:
                print("\nAuthentication successful!")
                return client
        return None

    def delete_account(self, account_number):
        for client in self.clients:
            if client.account['account_number'] == account_number:
                self.clients.remove(client)
                print("\nAccount deleted successfully!")
                return True
        print("\nAccount not found.")
        return False


from random import randint


class Client:
    def _init_(self, name, deposit):
        self.account = {
            'account_number': randint(10000, 99999),
            'name': name,
            'holdings': deposit
        }

    def withdraw(self, amount):
        if self.account['holdings'] >= amount:
            self.account['holdings'] -= amount
            print("\nThe sum of {} has been withdrawn from your account balance.".format(amount))
            self.balance()
        else:
            print("\nNot enough funds!")
            self.balance()

    def deposit(self, amount):
        self.account['holdings'] += amount
        print("\nThe sum of {} has been added to your account balance.".format(amount))
        self.balance()

    def balance(self):
        print("\nYour current account balance is: {} ".format(self.account['holdings']))


bank = Bank()
print("\nWelcome to {}!".format(bank.name))
running = True
while running:
    print("\nChoose an option:\n1. Open new bank account\n2. Open existing bank account\n3. Delete account\n4. Exit")
    choice = int(input("1, 2, 3, or 4: "))

    if choice == 1:
        print("\nTo create an account, please fill in the information below.")
        client = Client(input("Name: "), int(input("Deposit amount: ")))
        bank.update_db(client)
        print("\nAccount created successfully! Your account number is: ", client.account['account_number'])
    elif choice == 2:
        print("\nTo access your account, please enter your credentials below.")
        name = input("Name: ")
        account_number = int(input("Account number: "))
        current_client = bank.authentication(name, account_number)
        if current_client:
            print("\nWelcome {}!".format(current_client.account['name']))
            acc_open = True
            while acc_open:
                print("\nChoose an option:\n1. Withdraw\n2. Deposit\n3. Balance\n4. Exit")
                acc_choice = int(input("1, 2, 3, or 4: "))
                if acc_choice == 1:
                    current_client.withdraw(int(input("Withdraw amount: ")))
                elif acc_choice == 2:
                    current_client.deposit(int(input("Deposit amount: ")))
                elif acc_choice == 3:
                    current_client.balance()
                elif acc_choice == 4:
                    print("\nThank you for visiting!")
                    acc_open = False
        else:
            print("\nAuthentication failed! Reason: account not found.")
    elif choice == 3:
        print("\nTo delete your account, please enter your account number.")
        account_number = int(input("Account number: "))
        bank.delete_account(account_number)
    elif choice == 4:
        print("\nGoodbye!")
        running = False