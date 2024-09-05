# E-commerce-application
E-commerce Career Simulation - Fullstack Academy

"""
I developed a shopping/e-commerce application with login and public login features. I implemented the abililty to add and update categories in the application. Additionally, the application allows users to add or remove items from their cart. Finally, the program supports various payment options, including UPI and debit cards.
"""



####CLASS DECLARATIONS
class Admin:
    def __init__(self, admin_name="", admin_password="", admin_loggedIn = False):
        self.admin_name = admin_name
        self.admin_password = admin_password
        self.admin_loggedIn = admin_loggedIn
        
    #ADMIN FUNCTIONS --- i probably should have implemented in the class(Product) ---- XD
    def add_product(self):
        print("\nPRODUCT - ADD\n")
        
        newProductCategory = input("Category: ")
        newProductName = input("Name: ")
        newProductBrand = input("Brand: ")
        inputPrice = input("Price in USD($) with the format XXXX.XX for one item: ")
        newProductPrice = round(float(0 if inputPrice == '' else inputPrice), 2)
        inputQuantity = input("Quantity: ")
        newProductQuantity = int(0 if inputQuantity == '' else inputQuantity)
        
        newProduct = Product(category=newProductCategory, name=newProductName, brand=newProductBrand, price=newProductPrice, quantity=newProductQuantity)
        newProduct.print_info()
        verifiedProduct = input("'Y or 'N'")
        if verifiedProduct ==  'Y':
            catalog.append(newProduct)
        elif verifiedProduct ==  'N':
            self.add_product()
        else:
            pass
    
    def update_product(self):
        print("\nPRODUCT - UPDATE.\n")       
        
        #Get and Verify the name of the product to update
        nameOfProductToUpdate = input(f"\n{self.admin_name}, Please enter the current Name of the Product you wish to Update: ")
        verifiedProductName = input(f"{nameOfProductToUpdate} is this correct? 'Y' or 'N'")
        productObjectInCatalog = next((product for product in catalog if product.name == nameOfProductToUpdate), None)
        
        
        if verifiedProductName == 'Y':
            
            print("Please press 'Enter' if the current attribute of the product is good. \nOtherwise type in a new argument when the attribute that needs to be updated appears.\n")
            
            updateToCategory = input("Category: ")
            updateToName = input("Name: ")
            updateToBrand = input("Brand: ")
            inputPrice = input("Price in USD($) with the format XXXX.XX for one item: ")
            updateToPrice = round(float(productObjectInCatalog.price if inputPrice == '' else inputPrice), 2)
            inputQuantity = input("Quantity: ")
            updateToQuantity = int(0 if inputQuantity == '' else inputQuantity)
        
            
            if updateToCategory:
                finalCategory = updateToCategory
            else: 
                finalCategory = productObjectInCatalog.category
                
                
            if updateToName:
                finalName = updateToName
            else: 
                finalName = productObjectInCatalog.name
                
                
            if updateToBrand:
                finalBrand = updateToBrand
            else: 
                finalBrand = productObjectInCatalog.brand
                
                
            if updateToPrice:
                finalPrice = updateToPrice
            else: 
                finalPrice = productObjectInCatalog.price
                
                
            if updateToQuantity:
                finalQuantity = updateToQuantity
            else: 
                finalQuantity = productObjectInCatalog.quantity

            
            

            updatedProduct = Product(category=finalCategory, name= finalName, brand= finalBrand, price= finalPrice, quantity= finalQuantity)
            updatedProduct.print_info()
            verifiedProduct = input("'Y' or 'N'")
            if verifiedProduct ==  'Y':
                
                catalog.remove(productObjectInCatalog)
                catalog.append(updatedProduct)

            elif(verifiedProduct == 'N'):
                self.update_product()
        elif (verifiedProductName == 'N'):
            self.update_product()
        else:
            pass

    def delete_product(self):
        print("\nPRODUCT - DELETE.\n")       
         
        nameOfProductToDelete = input(f"\n{self.admin_name}, Please enter the current name of the Product you wish to Delete: ")
        verifiedProductName = input(f"!!!\t{nameOfProductToDelete}, is this correct?\t 'Y' or 'N'!!!")
        
        if verifiedProductName == 'Y':
            for p in catalog:
                if p.name == nameOfProductToDelete:
                    catalog.remove(p)
        elif verifiedProductName == 'N':
            self.delete_product()
        else:
            pass

    def add_category(self):
        print("\nCATEGORY - ADD.\n")  
        
        nameOfCategoryToAdd = input(f"\n{self.admin_name}, Please enter the current name of the Category you wish to Add: ")
        verifiedProductName = input(f"{nameOfCategoryToAdd}, is this correct?\t 'Y' or 'N'")
        
        if verifiedProductName == 'Y':
            newProductFormCategory = Product(category=nameOfCategoryToAdd)
            catalog.append(newProductFormCategory)
        elif verifiedProductName == 'N':
            self.add_category()
        else:
            pass


    def delete_category(self):
        print("\nCATEGORY - DELETE.\n")  
        
        nameOfCategoryToDelete = input(f"\n{self.admin_name}, Please enter the current name of the Category you wish to Delete: ")
        verifiedProductName = input(f"!!!\t{nameOfCategoryToDelete}, is this correct?\t 'Y' or 'N'!!!")
        
        delList = []
        if verifiedProductName == 'Y':
            for p in catalog:
                if p.category == nameOfCategoryToDelete:
                    delList.append(p)
            
            for p in delList:
                catalog.remove(p)
        elif verifiedProductName == 'N':
            self.delete_category()
        else:
            pass
        
    
class User:
    def __init__(self, user_name="", user_password="", user_cart = list(), user_CCNumber=None, user_Address="", user_loggedIn=False): 
        self.user_name = user_name
        self.user_password = user_password
        self.user_cart = user_cart
        self.user_CCNumber = user_CCNumber
        self.user_Address = user_Address
        self.user_loggedIn = user_loggedIn
        
        
    #USER FUNCTIONS  
    def add_to_cart(self, Product):
        num_requsted = int(input(f"How many of the {Product.name} would you like to add to your cart? \n There is currently {Product.quantity} avialable."))
        if num_requsted < 0 or Product.quantity <= 0:
            print(f"Hmmm, there is a problem. Try again.")
            pass
        elif Product.quantity >= num_requsted:
            append_cart = [Product]*num_requsted
            self.user_cart += append_cart
            Product.quantity -=num_requsted
        elif Product.quantity < num_requsted:
            append_cart = [Product]*Product.quantity
            self.user_cart += append_cart
            Product.quantity -=Product.quantity
            print(f"There was only {Product.quantity}, but we went ahead and added those anyway...")
            
    def delete_from_cart(self, Product):
        num_of_requested_items_in_cart = self.user_cart.count(Product)
        num_requsted = int(input(f"How many of the {Product.name} would you like to delete from your cart? \n There is currently {num_of_requested_items_in_cart} in your cart."))
        if num_requsted < 0 or num_of_requested_items_in_cart <= 0:
            print(f"Hmmm, there is a problem. Try again.")
            pass
        elif num_of_requested_items_in_cart >= num_requsted:
            remove_cart = [Product]*num_requsted
            
            for i in remove_cart:
                self.user_cart.remove(i)
            Product.quantity +=num_requsted
        elif num_of_requested_items_in_cart < num_requsted:
            remove_cart = [Product]*num_of_requested_items_in_cart
                       
            for i in remove_cart:
                self.user_cart.remove(i)
                
            Product.quantity += num_of_requested_items_in_cart
            print(f"There was only {num_of_requested_items_in_cart}, but we went ahead and removed those anyway...")
        pass

    def view_cart(self):
        
        disposable_ = {}
        for product in self.user_cart:
            if product.name not in disposable_.keys():
                disposable_[product.name] = 1
            else:
                disposable_[product.name] += 1
        for p in disposable_:
            print(f"{self.user_name}, you currently have {disposable_[p]}: {p}(s) in your cart.")
        
        disposable_.clear()
        
        pass
    
    def check_out(self):
        print("Check out")
        
        total = float()
        for product in self.user_cart:
            total += product.price
            
        total = round(total, 2)
        
        
        if not self.user_name:
            self.user_name = input(f"It seems you are a new customer, please add your name as it appears on your Credit Card.")
        
        print(f"{self.user_name}\tOkay, it seems your total is going to be: \t${total}")
        if self.user_CCNumber:
            print(f"We have your Credit Card number on file as: {self.user_CCNumber}")
        else:
            self.user_CCNumber = input("Please enter the Credit Card Number")
        
        if self.user_Address:
            print(f"We have your Address on file as: {self.user_Address}")
        else:
            self.user_Address = input("Please enter your mailing address")
        
        #clearing the cart
        self.user_cart = list()
        print(f"Success!!! {self.user_name}, your items are on their way!")
            
        pass
    

#Define the Product class with attributes: category= "", name= "", brand= "", price= float, quantity= int.
class Product:
    def __init__(self, category= "NONE", name= "NONE", brand= "NONE", price= 0.00, quantity= 0):
        self.category = category
        self.name = name
        self.brand = brand
        self.price = round(price, 2)
        self.quantity = quantity
        
    def print_info(self):
        print(f"""{self.category}: category,
             {self.name}: name, 
             {self.brand}: brand,
            ${self.price}: price,
             {self.quantity}: quantity,
              """)


####FUNCTIONS        
#one day would hope to make unique session ids for each login
def generate_session_id(user):
    if user.user_name:
        return (f"{user.user_name}-{12345}-{id(user)}")
    else:
        return (f"New User-{12345}-{id(user)}")


def  generate_session_admin_id(admin):
    return (f"{admin.admin_name}-{54321}-{id(admin)}")


# TODO: Implement the logic to choose between user and admin login.        
def ask_login():
    
    type_user = input("Welcome back! Will you be accessing the site as an ('admin'), ('user'), or ('new')?")
    if type_user == "new":
        newUser = User()
        user_db.append(newUser)
        # Generate session id for newUser
        session_id = generate_session_id(newUser)
        print(f"Your session id is: {session_id} \n")
        return(newUser)
    elif type_user == "admin":
        return try_admin_login()
    elif type_user == "user":
        return try_user_login()
    else:
        print("I'm sorry, that is not a valid option... ")
        return ask_login()
        
        
# Function to handle admin login
def try_admin_login():
    
    admin = input("Enter your admin username: ")
    password = input("Enter your password: ")
    
    for A in admin_db:
        if  A.admin_name == admin and A.admin_password == password:
            A.admin_loggedIn = True
            print(f"Login successful as admin: {A.admin_name}.")
            # Generate session id for admin
            session_id = generate_session_admin_id(A)
            print(f"Your session id is: {session_id}")
            return A
            
    #only runs when the for loop goes through all options in database
    else:
        #admin not found or incorrect password
        print("Login failed. Please try again.\n")
        ask_login()
        
        
# Function to handle user login
def try_user_login():
    
    user = input("Enter your username: ")
    password = input("Enter your password: ")
    
    #look throught the user database
    for U in user_db:
        if U.user_name == user and U.user_password == password:
            U.user_loggedIn = True
            print(f"Login successful as user: {U.user_name}.")
            # Generate session id for user
            session_id = generate_session_id(U)
            print("Your session id is: ", session_id)
            return U
            
    #only runs when the for loop goes through all options in database
    else:
        #user not found or incorrect password
        print("Login failed. Please try again.\n")
        ask_login()        
        
        
# Function to view the catalog
def view_catalog():
    print("\nCatalog\n__________________________________________________")
    for product in range(len(catalog)):
            catalog[product].print_info()
            
            
# Function to log anyone out, should they choose                        
def toggle_login_status(login_status):
    return not login_status



###USERS AND ADMINS        
user_db = []
#user_name, user_password, user_cart = list(), user_CCNumber=int, user_Address=""
user1 = User(user_name="u1", user_password="p1", user_cart=[], user_CCNumber=1234567890, user_Address="42 Wallaby Way, Sydney")
user2 = User(user_name="u2", user_password="p2", user_cart=[])
user3 = User(user_name="u3", user_password="spaghetti", user_cart=[])
user_db.append(user1)
user_db.append(user2)
user_db.append(user3)


admin_db = []
# admin_name, admin_password
admin1 = Admin(admin_name="A1", admin_password="P1")
admin2 = Admin(admin_name="A2", admin_password="P2")
admin3 = Admin(admin_name="A3", admin_password="P3")
admin_db.append(admin1)
admin_db.append(admin2)
admin_db.append(admin3)



###PRODCUT CATALOG
catalog = []

Product1 = Product(category= "Boots", name= "Work Boots", brand= "Ariat", price= 120.008668, quantity= 100)
Product2 = Product(category= "Coats", name= "Work Coat", brand= "Carhartt", price= 150.00, quantity= 90)
Product3 = Product(category= "Jackets", name= "Thermal Jacket", brand= "Carhartt", price= 165.00, quantity= 144)
Product4 = Product(category= "Hats", name= "Golf Hat", brand= "Titleist", price= 40.00, quantity= 3)

catalog.append(Product1)
catalog.append(Product2)
catalog.append(Product3)
catalog.append(Product4)




#FUNCTION CALLS
person = ask_login()
view_catalog()

#Loop
print("Welcome to the Demo Marketplace")
print("Type 'help' for a list of actions, or if you already know what action you would like to take you can input it now!\n")

looping = True
while looping:

    
    action = input("\t")
        
    if type(person) == type(User()):
        match action: 
            
            case 'help':
                print("help")
                print("view_catalog")
                print("add_to_cart")
                print("delete_from_cart")
                print("view_cart")
                print("check_out")
                print("log_out")
                print("end")
            
            case 'view_catalog':
                view_catalog()
            
            case 'add_to_cart':
                
                nameOfProductToAdd = input("Please input the NAME of the Product to add to your cart")
                productObjectInCatalog = next((product for product in catalog if product.name == nameOfProductToAdd), None)
                if productObjectInCatalog:
                    person.add_to_cart(productObjectInCatalog)
                print("add_to_cart")
                
            case 'delete_from_cart':
                
                nameOfProductToDel = input("Please input the NAME of the Product to del from your cart")
                productObjectInCatalog = next((product for product in catalog if product.name == nameOfProductToDel), None)
                if productObjectInCatalog:
                    person.delete_from_cart(productObjectInCatalog)
                
                print("delete_from_cart")
                
            case 'view_cart':
                print("view_cart")
                person.view_cart()


                
            case 'check_out':
                print("check_out")
                person.check_out()
          
                
            case 'log_out':
                print("log_out")
                print(f"Good Bye! {person.user_name}")
                person.logged_in = False
                person = ask_login()
            
            case 'end':
                print(f"Good Bye! {person.user_name}")
                person.logged_in = False
                looping = False
                
    elif type(person) == type(Admin()):
        
         match action: 
            
            case 'help':
                print("help")
                print("view_catalog")
                print("add_product")
                print("update_product")
                print("delete_product")
                print("add_category")
                print("delete_category")
                print("log_out")
                print("end")
                
            case 'view_catalog':
                print("view_catalog")
                view_catalog()
                
            case 'add_product':
                print("add_product")
                person.add_product()
                
            case 'update_product':
                print("update_product")
                person.update_product()
                
            case 'delete_product':
                print("delete_product")
                person.delete_product()
                
            case 'add_category':
                print("add_category")
                person.add_category()
                
            case 'delete_category':
                print("delete_category")
                person.delete_category()
                       
            case 'log_out':
                print("log_out")
                print(f"Good Bye! {person.admin_name}\n")
                person.logged_in = False
                person = ask_login()
            
            case 'end':
                print(f"Good Bye! {person.admin_name}")
                person.logged_in = False
                looping = False

