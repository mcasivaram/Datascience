# Datascience#Register: Username(@gmail.,not atart with special character or number)
#Password: <5 and >16,one uppercase,one lowercase,one digit,one special character
import re
import os.path
import pandas as pd


def sign_up():
    Username = input("Create Username : ")
    Password = input("Create Password : ")
    counter1= True
    counter2= True
    # Password Validation
    Specialchar = ['$', '@', '#', '%']
    if len(Password) < 5:
        print('Password length should be at least 5')
        counter2 = False

    if len(Password) > 16:
        print('Password length should be not be greater than 16')
        counter2 = False

    if not any(char.isdigit() for char in Password):
        print('Password should have at least one numeral')
        counter2 = False

    if not any(char.isupper() for char in Password):
        print('Password should have at least one uppercase letter')
        counter2 = False

    if not any(char.islower() for char in Password):
        print('Password should have at least one lowercase letter')
        counter2 = False

    if not any(char in Specialchar for char in Password):
        print("Password should have special character")
        counter2 = False

    #Username Validation
    pattern = "^[a-zA-Z][a-zA-Z0-9-_]+@[a-z]+.[a-z]{1,3}$"
    if re.match(pattern, Username):
        counter1  = True
    else:
        counter1 = False
        print("Username pattern not matched")
    #file handling
    if counter1 and counter2:
        if (os.path.exists('Data.csv')):
            write(Username, Password)
        else:
            fileCreation()
            write(Username, Password)
        print("Registration Successfull")
    else:
        print("Invalid Username and Password")
    return

def write(User,Pass):
    data = {
        'Username': [User],
        'Password': [Pass]
    }
    df = pd.DataFrame(data)
    df.to_csv('Data.csv', mode='a', index=False, header=False)
    return

#File Creation
def fileCreation():
    data = {
        'Username': [],
        'Password': []
    }
    df = pd.DataFrame(data)
    df.to_csv('Data.csv', index=False, mode='w')
    return
#Login Validation
def Login():
    User = input("Enter username : ")
    Password = input("Enter password : ")
    if (os.path.exists('Data.csv')):
        read(User, Password)
    else:
        fileCreation()
        read(User, Password)
    return
#File reading
def read(Username,Password):
    df = pd.read_csv('Data.csv')
    Users_list = df['Username'].tolist()
    pass_list = df['Password'].tolist()
    # show the list
    if Username in Users_list and Password in pass_list:
        print("Login Successfull")
    else:
        print("Invalid Credentials")
    return
#Forgot password
def forgotpassword():
    User = input("Enter username : ")
    if (os.path.exists('Data.csv')):
        getpassword(User)

    else:
        fileCreation()
        getpassword(User)
    return
#getting password for file
def getpassword(User):
    df = pd.read_csv('Data.csv', index_col=None)
    Users_list = df['Username'].tolist()
    if User in Users_list:
        Password = df.loc[df['Username'] == User, 'Password']
        Pass = str(Password.tolist())
        print("Password is",Pass)
    else:
        print("Invalid Username please register")
    return


#Mainprogram
print("Choose the option")
print("1. Sign Up")
print("2. Login")
print("3. Forgot Password")
print("Enter the number (1,2 or 3) according to option: ")
number=int(input())
if number == 1:
    sign_up()
elif number == 2:
    Login()
else:
    forgotpassword()
