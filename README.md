#Chew Hong Cherng 
#TP064802

import datetime

def add_patient():
    with open("patient.txt","a") as fh:
        rec = []
        while True:
            print("\nPlease choose your vaccination center (1 or 2)")
            vc = input("Select your Vaccination Center : ")
            if vc == "1":
                print("\nYou chose Vaccination Center 1")
                break
            elif vc == "2":
                print("\nYou chose Vaccination Center 2")
                break
            else:
                print("Invalid option")
                continue
        while True:
            name = input("Please enter your name : ")
            if(name.isalpha() and name.isspace() for name in name):
                break
            else:
                print("Invalid name")
                continue
        while True:
            cont = input("Please enter your contact number : ")
            if len(str(cont)) <= 11 and len(str(cont)) >=10:
                break
            else:
                print("Invalid option")
                continue
        while True:
            age = int(input("Please enter your age : "))
            if age >= 12 and age < 140:
                print("\nYou are allow to take vaccination!")
                print("\nNow we neeed to choose the vaccine")
                if age <= 17:
                    print("\nThe vaccine available is (AF , CZ , DM)")
                    choice = input("Please select your choice of vaccine : ")
                    if choice == "cz":
                        print("\nThe vaccine you will going to take is CZ")
                        break
                    elif choice == "af":
                        print("\nThe vaccine you will going to take is AF")
                        break
                    elif choice == "dm":
                        print("\nThe vaccine you will going to take is DM")
                        break
                    else:
                        print("Invalid option")
                        continue
                elif age >=18 and age <= 45:
                    print("\nThe vaccine available are (AF , BV , CZ , DM , EC)")
                    choice = input("Please select your choice of vaccine : ")
                    if choice == "cz":
                        print("\nThe vaccine you will going to take is CZ")
                        break
                    elif choice == "af":
                        print("\nThe vaccine you will going to take is AF")
                        break
                    elif choice == "dm":
                        print("\nThe vaccine you will going to take is DM")
                        break
                    elif choice == "bv":
                        print("\nThe vaccine you will going to take is BV")
                        break
                    elif choice == "ec":
                        print("\nThe vaccine you will going to take is EC")
                        break
                    else:
                        print("Invalid option")
                        continue
                elif age >= 46:
                    print("\nThe vaccine available are (BV , EC)")
                    choice = input("Please select your choice of vaccine : ")
                    if choice == "bv":
                        print("\nThe vaccine you will going to take is BV")
                        break
                    elif choice == "ec":
                        print("\nThe vaccine you will going to take is EC")
                        break
                    else:
                        print("Invalid option")
                        continue
            else:
                print("You are not allow to take the vaccination yet")
                continue
        check = False
        while check == False:
            email = input("Please enter your Email Address : ")
            if email[-4:] == ".com":
                for e in email:
                    if e == "@":
                        check = True
                        break
                    else:
                        check = False
            else:
                pass
            if check == True: 
                break
            else:
                print("Invalid Email Address")
                continue
        ptid = gen_new_id("PID")
        print("\nNew patient's ID : ",ptid)
        rec.append("VC"+vc)
        rec.append(name)
        rec.append(str(cont))
        rec.append(str(age))
        rec.append(choice)
        rec.append(email)
        rec.append(ptid)
        fh.write(":".join(rec) + "\n")
        return main_menu()
 
    
def admin(allrec, allrec2):
    rec2 = []
    print("\nIf your a new patient please press 1")
    print("If you had already taken dose one press 2")
    choice = input("Please select 1 or 2 : ")
    if choice == "1":
        search = input("Enter patient ID : ")
        while True:
            date = input("Enter your vaccination date(day-month-year) : ")
            date = datetime.datetime.strptime(date, "%d-%m-%Y").date()
            ptvc = input("Enter selected vaccine code : ")
            if ptvc == "af":
                af = datetime.timedelta(14)
                date2 = date + af
                break
            elif ptvc == "bv":
                bv = datetime.timedelta(21)
                date2 = date + bv
                break
            elif ptvc == "cz":
                cz = datetime.timedelta(21)
                date2 = date + cz
                break
            elif ptvc == "dm":
                dm = datetime.timedelta(28)
                date2 = date + dm
                break
            elif ptvc == "ec":
                print("You are fully vaccinated")
                date2 = date
                break
            else:
                print("Invalid option")
                continue
    elif choice == "2":
        print("Dose one taken")
    else:
        print("Invalid option")
    today = datetime.datetime.now().date()
    print(today, date, date2)
    if today >= date and today < date2:
        status = "First dose complete"
    elif today >= date2:
        status = "Fully vaccinated"
    else:
        status = "Not yet vaccinated"
    with open("vaccination.txt","a") as fh:
        rec2.append(search)
        rec2.append(ptvc)
        rec2.append(str(date))
        rec2.append(str(date2))
        rec2.append(status)
        fh.write(":".join(rec2) + "\n")
    print("patient's dose record")
    print("patient's ID : ", search)
    print("patient's selected vaccine : ", ptvc)
    print("patient's vaccne date : ", date)
    print("patient's dose two date : ", date2)
    return


def gen_new_id(ptid):
    with open("id.txt","r") as fh:
        rec = fh.readline()
        if ptid == "PID":
            ind = 0
        elif ptid == "VID":
            ind == 1
        elif ptid == "STF":
            ind = 2
        mylist = rec.split(":")
        nextid = mylist[ind]
        newid = str(int(nextid[3:])+1)
        if len(newid) == 1:
            nextid = nextid[:3]+"0000"+newid
        if len(newid) == 2:
            nextid = nextid[:3]+"000"+newid
        if len(newid) == 3:
            nextid = nextid[:3]+"00"+newid
        if len(newid) == 4:
            nextid = nextid[:3]+"0"+newid
        if len(newid) == 5:
            nextid = nextid[:3]+newid
        mylist[ind] = nextid
        rec = ":".join(mylist)
        with open("id.txt","w") as fh:
            fh.write(rec)
        return (nextid)


def display_all_rec(allrec):
    no_of_rec = len(allrec)
    for i in range(no_of_rec):
        print(allrec[i])
    return
        
def display_all_rec2(allrec2):
    no_of_rec2 = len(allrec2)
    for i in range(no_of_rec2):
        print(allrec2[i])
    return


def main_menu():
    allrec = []
    with open("patient.txt","r") as fh:
        for line in fh:
            mylist = line.split(":")
            allrec.append(mylist)
    allrec2 = []
    with open("vaccination.txt","r") as fh:
        for line in fh:
            list2 = line.split(":")
            allrec2.append(list2)
        
        print("\n\nToday date is : ",datetime.datetime.now().date())
        print("\n1 - Add patient")
        print("2 - Vaccine Administrations")
        print("3 - Search patient's details")
        print("4 - Display patient's vaccination record")
        print("5 - Exit")
        choice = int(input("Enter your choice : "))
        if choice == 1:
            mainrec = add_patient()
            allrec.append(mainrec)
        elif choice == 2:
            allrec = admin(allrec,allrec2)
        elif choice == 3:
            display_all_rec(allrec)
        elif choice == 4:
            display_all_rec2(allrec2)
        elif choice == 5:
            print("Thank you so much!")
        else:
            print("\nInvalid option")
        return


usern = input("Please Enter Username : ")
passw = input("please Enter Password : ")
if usern == "staff123" and passw == "staffabc":
    while True:
        main_menu()
else:
    print("No allowed")
