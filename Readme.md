# Go Vend Me!
## Project-0 in Go!
Go Vend Me! is a project created by Tony Moon in Go. This project is meant to be a learning tool for exploring a variety of features offered by Go.
In this project I explored implementation of defualt and custom packages, command line tools, user input, proper documentation, unit testing, and database commands.

This project is a short adventure exploring a vending machine. Every vending machine is unique in it's own beautiful way. Have fun!

## Instructions
This project uses a Docker image of Postgres. Ensure that you have it intalled. 
If you have Docker Postgres already installed, skip the **Docker and Postgres Setup** section.
If you have already ran the adventure once, skip both the **Docker and Postgres Setup** and the **Starting the Adventure** sections.

### Cloning from Git
Create a file structure resembling the structure below.
>~/go/src/github.com/Tony-Moon

Then call the git clone inside the directory you just created using:
>git clone https://github.com/Tony-Moon/project-0.git

### Docker and Postgres Setup
Ensure you have docker installed. If you do not, you can use the following commands.
>sudo apt search docker
>sudo apt install docker
>sudo apt install docker.io

Ensure you have Postgres installed with the Docker drivers. If you do not, you can use the following commands:
>cd db
>docker build -t project-0 .
>docker run -p 5432:5432 -d --name project-0 project-0

### Starting the Adventure
Once you have run the previous commands, all you will need to do is run the following command.
>go run main.go
To play through again, simply re-run that command.

## Adventuring
In this section I will talk about breifly describe how the application works and point out a few "hidden features."

### Breif "Under the Hood"
This application was built in Go, using VS Code as a light wieght IDE. It is using a Docker image of a Postgres database. The database consists of three tables; **drinklist**, **machine**, and **servicers**

When the program first starts, it checks for any flags. How to utilize these is discussed in the Hidden Features section below. Then, Go Vend Me! will generate a vending machine.

To emmulate real life, there are three drink companies and only one can own a single vending machine. This means, when a vending machine is generated, only drinks from the company randomly selected will appear in the machine. Drinks are randomly selected from the **drinklist** table, ordered, assigned a random amount of stock and placed into the **machine** table. Different drinks have different levels of popularity, so some drinks will show up in less quanities and less often than others.

You can then select a drink and the machine dispense one to you. If you are feeling particularly dehydrated, you can buy every drink in the machine. Still thirsty? Well, you'll need someone to restock the machine. Can't wait? How about trying to apply for a position?

You can navigate to the application page (the how is discussed int the next section) and you will have a chance to get your name on the certified vending machine servicers list, or just the **servicers** table. Now when you see one of your company's vending machine running low on supplies, simply login and restock the machine!

### Hidden Features
If you think its udderly bonkers to have a vending machine with three rows, five columns and a max capacity of ten in each slot, you can change that! Instead of running the normal way, run:
>go run main.go -row <number_of_rows> -col <number_of_columns> -cap <max_capacity_per_slot>

As mentioned above, you can attempt to add yourself to the list of people registered to restock the vending machine. There are a couple of ways to do this.
>go run main.go -apply <company_letter> 
This will take you to an application page, where the CL will prompt you with an application. 

If you are wondering what the company letter is, use **d** to apply for a Duda-Cola position, **s** for Salt-PhD and **t** for TipsyCo.

There is also a quick apply feature. To utilize this, run 
>go run main.go -apply <company_letter> <firstname> <lastname> <desired_username> <desired_password>