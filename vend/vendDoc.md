# vend
Welcome to the technical documentation of the vend package.
The purpose of the vend package is to navigate the user to interacting with the vending machine.
The information pertaining to the vending machine is stored in the database table **machine**.
Additionally, users can restock the vending machine. 
The list of users that can is stored in the **servicers** table.

>package vend // import "github.com/Tony-Moon/project-0/vend"

## Functions in vend

>func Navigate(db *sql.DB, capacity int)
    Navigate is the main function of the vend package. This should only be
    called once in main.go of the main package. This function should be the only
    thing in this package called outside of the vend package. It is important to
    note that the gen package should have a chance to run, via calling the
    Generate function in the gen package, before this function is called.

    Navigate first runs the Encounter function,then navigates to one of three
    options; the Vending function, the Restock function or exiting Naviagte back
    to the main function in the main package. Additionally, if the user tries to
    pick an option that is not one of those three options, it will continue ask
    the user to retry until the user picks a valid option.I wrote in some hidden
    messages if you keep trying to pick non-options, just for fun.

    Naviate should be passed the database information and the max capactiy of
    each row in the vending machine. Navigate passes the database information to
    the Vending function, and passes the database and capacity information to
    the Restock function.

>func Encounter() int
    Encounter simply propmts the user the first time they find the vending
    machine. Then, it records and returns the users choice.

>func Vending(db *sql.DB) int
    Vending is the function called when the user chooses to buy a drink, either
    through the Encounter function, or through the Restock function. Both
    options will go back to the Navigate function, then to the Vending function.
    The Vending function should be passed the database information. The Vending
    function first lists the contents of the vending machine using the
    ListDrinks function inside the vend package. Then, Vending calls the
    BuyDrink function, where the user will select the drink of their choice, and
    the **machine** table will be updated to dispense one of that beverage.
    Finally, Vending reprompts the user to see if they want to buy another
    drink, restock the machine or leave. If they wish to buy another drink,
    Vending loops over and recalls ListDrinks and BuyDrink. Otherwise, Vending
    returns to Navigate, then returns to main.go in the main package.

    Vending also has a counter which counts the number of drinks a user buys.
    This is passed to both ListDrinks and BuyDrink. ListDrink will print a
    unique message if the user has not bought any drinks yet. BuyDrink will
    increment the counter.

>func ListDrinks(db *sql.DB, counter int)
    ListDrinks prints the contents of the vending machine to the console. If the
    user has not bought a drink yet, ListDrinks will print a welcome message.
    ListDrinks will get the index, name, stock and brand of the vending machine
    in slices from the GetDrinks function. Then ListDrinks will print the index
    and name of the drink to the console. If the stock is zero, ListDrinks will
    print the index and "empty" to tell the user that row is out of stock.

    List drinks recives the database information and the counter mentioned in
    the Vending documentation. ListDrinks passes the database information to the
    GetDrinks function.

>func GetDrinks(db *sql.DB) ([]string, []string, []int, string)
    GetDrinks reads all data from the **machine** table and saves the data into
    slices. Then, GetDrinks returns taht data.

>func BuyDrink(db *sql.DB, counter int) int
    BuyDrink is called by Vending to get the users drink selection then dispense
    the drink. To do this, BuyDrink calls GetDrinks to get the index, name and
    stock from the **machine** table in slices. BuyDrink then gets the index of
    the drink the user wanted and increments a counter passed back to Vending at
    the end of the function. After a drink has been selected, BuyDrink calls the
    Dispense function to reduce the stock of the machine table by one.

    BuyDrink recieves the database information and the counter. It passes the
    database information to GetDrinks and Dispense. Along with the database
    information, BuyDrink also sends Dispense the index and stock fo the users
    choice.

>func Dispense(db *sql.DB, index string, stock int)
    Dispense reduces the stock of the users selection by one. It recieves the
    database information, the index of the users selection and the stock amount
    of that row. Then it updates the **machine** table using index and then new
    stock amount.

>func Restock(db *sql.DB, capacity int) int
    Restock is called when the users requests to restock the vending machine.
    Restock retrieves the company associated with the vending machine through
    the GetDrinks function. Then Restock calls Login to verify that the user can
    restock the vending machine. Login will return a one when the user quits or
    restocks the machine. After this, Restock will prompt the user if they want
    to buy a drink, returning them to the Vending function then to BuyDrink, or
    leave, sending them back to main.go in the main package. If Login returns a
    zero, Restock will restart Login.

    Restock recieves the database information and the maxium capacity of the
    rows in the vending machine. Restock returns an integer telling Naviage
    which function to navigate to. Restock sends the database information to
    GetDrinks and Login. In addition, Login also recieves from Restock the
    maxium capacity and company associated with the vending machine.

>func Login(db *sql.DB, brand string, capacity int) int
    Login allows registered technicians to restock the vending machine by first
    checking their creditials with a username and password. If the user
    successfully logs in, Login will check if that user has privaleges to
    restock the vending machine. If they do, Login will call the Refill function
    to set all rows to max capcity.

    To do this, first Login saves all the data from the **servicers** table.
    Then, Login prompts the user to enter their username and login and checks
    with the saved data from the servicers table. Login will match a username
    from the table with the username provided by the user. If the username does
    not match with any usernames in thedatabase, Login will proceed with the
    first name in the database. Then, Login will check the password associated
    with the username. If the passwords do not match, Login will notify the user
    and ask if they want to try again or go back to Navigate. If the user
    chooses to retry Login returns a zero, and Restock will recall Login. If the
    user chooses to quit, Login will return a one and Restock will prompt the
    user if they want to buy a drink or leave.

    If the user successfully logs in, Login will check the company associated
    with the user and compare it to the company associated with the vending
    machine. If the users company and the vending machine company do not match,
    the usere is sent back to asked if they want to buy a drink or leave. If the
    companies do match, the Refill is called to refill the vending machine.

    Login recieves the database information, the company of the vending machine
    and the maxium capacity of each row. It returns an integer that determines
    if Login should be run again. Login sends the databse information and the
    maximum capacity to Refill.

>func Refill(db *sql.DB, max int)
    Refill recieves the database information and maximum capacity of each row.
    Then, Refill updates the **machine** table so the entire stock column is set
    to the maximum capcity.
