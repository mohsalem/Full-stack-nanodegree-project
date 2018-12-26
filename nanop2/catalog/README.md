To run this project, you'll need database software (provided by a Linux virtual machine) and the data to analyze.

The virtual machine
This project makes use of the same Linux-based virtual machine (VM).

install the virtual machine(https://www.virtualbox.org/wiki/Downloads). 
install vagrant(https://www.vagrantup.com/downloads.html).
use VM configuration(https://github.com/udacity/fullstack-nanodegree-vm/blob/master/vagrant/Vagrantfile).


You need to bring the VM up using vagrant up, do so now. Then log into it with vagrant ssh.

A terminal in which we've successfully logged into the virtual machine using "vagrant ssh".
Successfully logged into the virtual machine.

move the data
you need to unzip the project file inside the vagrant folder where you installed the VM



The database includes three tables:

The User table includes user accounts created.
The category table includes the categories' names.
The item table includes the name and description for each item.

after you log into your VM you run commands python database_setup.py and python project2.py

you are now ready to open the website on address: localhost:5000

the website allows you to login with your google account to add items and categories and delete and items you created or you can view available categories and items if you do not want to log in 