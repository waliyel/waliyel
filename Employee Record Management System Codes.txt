///Declared necessary header files.

#include <iostream>
#include <cstdio>
#include <cstring>
#include <cstdlib>
#include <conio.h>
#include <iomanip>
#include<time.h>

/**
*Project Title: Employee Record Management System
*Name: Waliyel Hasnat Zaman
*ID: 212002022
*Sec: DB
*Course Code: CSE 106
*Course Title: Data Structures Lab
*/

using namespace std;


///Declared File Pointers

FILE *fp, *ft; //'ft' represents temporary file pointer.
char another, choice; //Declared variables to store user decisions.


///Declared structure to represent an employee.

struct employee{

    char ID[10];
    char Name[30];
    int Age;
    long Salary;
};

struct employee e; //Declared structure variable.

char xID[10]; //Variable declared to store the ID of employees for comparison.

long int recsize; //Variable declared to store the size of the structure variable 'e'.


///Delay Function created for mimicking the loading screen.

void delay(unsigned int mseconds){

    clock_t goal = mseconds + clock();
    while (goal > clock());
}


///User defined function to add new records of the employees.

void addrecord(){

    fseek(fp,0,SEEK_END); //Function used to set the cursor position at the end.
    another ='Y';
    while(another == 'Y' || another == 'y'){

        system("cls");
        cout<<"Enter New Employee's ID: ";
        fflush(stdin);
        gets(e.ID);
        cout << "Enter New Employee's Name: ";
        fflush(stdin);
        gets(e.Name);
        cout << "Enter New Employee's Age: ";
        fflush(stdin);
        cin >> e.Age;
        cout << "Enter New Employee's Basic Salary: ";
        cin >> e.Salary;
        fwrite(&e,recsize,1,fp); //Writes the records in the file 'emp.txt'.
        cout << "\n Add Another Record (Y/N) ";
        fflush(stdin);
        another = getchar();
    }
}


///User defined function to view records of the employees.

void viewrecord(){

    system("cls");
    rewind(fp);
    cout << "=== View The Records In The Database ===";
    cout << "\n";

    cout<<"\n\n   ID               NAME                AGE     Salary\n";
    while (fread(&e,recsize,1,fp) == 1) //Function used to read from the file.
        printf("\n%-10s\t%-24s %-2d\t%-10d\n",e.ID, e.Name, e.Age, e.Salary);

    cout << "\n\n";
    system("pause");
}


///User defined function to modify old records of the employees.

void modifyrecord(){

    //Records are modified by comparing the ID given by the user with the IDs saved in the file 'emp.txt'.

    system("cls");
    another = 'Y';
    int flag;
    while (another == 'Y'|| another == 'y'){

        cout << "\nEnter ID Of The Employee To Modify His/Her Records: ";
        fflush(stdin);
        gets(xID);
        flag=0;
        rewind(fp);
        while (fread(&e,recsize,1,fp) == 1){

            fflush(stdin);
            if (strcmp(e.ID, xID)==0){

                cout<<"\n\nModify Employee's Name: ";
                fflush(stdin);
                gets(e.Name);
                cout<<"Modify Employee's Age: ";
                cin>>e.Age;
                cout<<"Modify Employee's Salary: ";
                cin>> e.Salary;
                fseek(fp, - recsize, SEEK_CUR);
                fwrite(&e,recsize,1,fp);
                flag++;
                break;
            }
        }
        if(flag==0)
            cout<<"Record Not Found\n";

        cout << "\nModify Another Record (Y/N): ";
        fflush(stdin);
        another = getchar();
    }
}


///User defined function to search records of the employees.

void searchrecord(){

    //Records are searched by comparing the ID given by the user with the IDs saved in the file 'emp.txt'.

    system("cls");
    char sID[10],CH;
    another='Y';
    while(another=='Y'||another=='y'){
        system("cls");
        cout<<"Enter ID To Search: ";
        fflush(stdin);
        gets(sID);
        rewind(fp);

        cout<<"\n\n   ID               NAME                AGE     Salary\n";
        CH='Y';

        while (fread(&e,recsize,1,fp) == 1){

            if(strcmp(e.ID,sID) == 0)
                    printf("\n%-10s\t%-24s %-1d\t%-10d\n\n\n\n",e.ID, e.Name, e.Age, e.Salary);
            }
        cout<<"\n";
        cout<<"Want To Search Again (Y/N): ";
        fflush(stdin);
        another=getchar();
    }
}


///User defined function to delete records of the employees.

void deleterecord(){

    //Records are deleted by comparing the ID given by the user with the IDs saved in the file 'emp.txt'.

    system("cls");

    another = 'Y';
    while (another == 'Y'|| another == 'y'){

        cout << "\nEnter The ID Of The Employee To Delete: ";
        fflush(stdin);
        cin >> xID;

        ft = fopen("temp.txt", "wb"); //Open 'temp.txt' in Write Binary mode.

        rewind(fp);
        while (fread (&e, recsize,1,fp) == 1)

        if (strcmp(e.ID,xID) != 0)
            fwrite(&e,recsize,1,ft); //If ID given by the user matches with the ID of the file, except the records of that ID the rest will be moved to 'temp.txt'.

        fclose(ft); //File pointer 'ft' closed.
        fclose(fp); //File pointer 'fp' closed.
        remove("emp.txt"); //Remove 'emp.txt'.
        rename("temp.txt","emp.txt"); //Rename 'temp.txt' to 'emp.txt'.

        fp=fopen("emp.txt","rb+"); //Opens 'emp.txt' in Read Binary Plus mode.

        cout<<"Record Deleted Successfully\n";
        cout << "\n\nDelete Another Record (Y/N): ";
        fflush(stdin);
        another = getchar(); //Get another choice from user.
    }
}


///Main function starts from here.

int main(){

    system("cls");

    for(int i=1;i<=50;i++){ //For loading screen.
        system("cls");
        cout<<"\n\n\n\n\n\n\n\n\t\t\t\t\t====LOADING==== "<<"\n\n"<<"\t"<<"\t";
        cout<<"\t";
        for(int j=1;j<=i;j++)
            cout<<"|";
        cout<<"\n\n\t\t\t "<<2*i<<"%";
        if( i > 1 && i < 20)
            cout<<"\n\n\t\t\tCreating Temporary files";
        else if( i > 20 && i<40)
                cout<<"\n\n\t\t\tAccessing Main Memory";
        else if(i >40 && i<48)
                cout<<"\n\n\t\t\tAccessing Cache";
        else cout<<"\n\n\t\t\tLoading Complete. Press Enter to Continue ";
        delay(150 - i*3);
      }
    getch();


    recsize = sizeof(e);
    while(1){

        system("cls"); //Clears screen.

        cout << "\n\n\n\n\t\t      ====== EMPLOYEE RECORD MANAGEMENT SYSTEM ======";
        cout <<"\n\n";
        cout << "\n\n";
        cout << "\n \t\t\t\t 1. Add    Records";
        cout << "\n \t\t\t\t 2. View   Records";
        cout << "\n \t\t\t\t 3. Modify Records";
        cout << "\n \t\t\t\t 4. Search Records";
        cout << "\n \t\t\t\t 5. Delete Records";
        cout << "\n \t\t\t\t 6. Exit   Program";
        cout << "\n\n";
        cout << "\t\t\t\t Select Your Choice : ";
        fflush(stdin);
        choice = getche();


        ///Switch statements are used in this program to take choice from user to perform any operations.

        switch(choice){

        case '1' : //Adds new records.
            fp=fopen("emp.txt","rb+");
            if (fp == NULL){ //If file 'emp.txt' exists the open in rb+ mode else create new 'emp.txt' fileand open it in wb+ mode.

                fp = fopen("emp.txt","wb+");

                if (fp==NULL){

                    puts("Cannot Open File");
                    return 0;
                }
            }
            addrecord();
            fclose(fp);
            break;
        case '2': //View records.
            fp=fopen("emp.txt","rb+");
            viewrecord(); //Calling user defined function.
            fclose(fp);
            break;

        case '3' : //Modify old records.
            fp=fopen("emp.txt","rb+");
            modifyrecord();
            fclose(fp);
            break;

        case '4' : //Search records.
            fp=fopen("emp.txt","rb+");
            searchrecord();
            fclose(fp);
            break;

        case '5': //Delete records.
            fp=fopen("emp.txt","rb+");
            deleterecord();
            fclose(fp);
            break;

        case '6': //Close/Exits the program.
            fclose(fp);
            cout << "\n\n\n\t\t\t\t     THANK YOU!\n\n";
            exit(0);
        }
    }

    system("pause");
    return 0;
}
