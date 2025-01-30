# GPA-Analyzer-Fraz-Ahmed-1#include <iostream>
#include <fstream>
#include <cstdlib>
#include <ctime>
using namespace std;


class Date { // Date class (A1)
     private:
     int day;
     int month;
     int year;

public: 
     Date();
     void SetDate(int,int,int);
     void DisplayDate();
     void CompTest();
};

Date::Date() {
       time_t currentTime = time(nullptr);
       tm* localTime = localtime(&currentTime);
       day = localTime->tm_mday;
       month = localTime->tm_mon + 1; 
       year = localTime->tm_year + 1900; 

}

void Date::SetDate(int m, int d, int y) { // External date Initialization (A2)
    month = m;
    day = d;
    year = y;
}

void Date::DisplayDate() {
    cout << "Today's date: " << month << "/" << day << "/" << year << endl;
    cout << endl;
}
 void Date::CompTest() { // Component Test method (A3)
   SetDate(2,17,2024);
   cout << "Comp Test Run:" << endl;
   cout << endl;
   cout << "Date output should be: 02/17/2024 (as set)" << endl;
   cout << "Date output is: ";
   DisplayDate();
 }

class Grades {
private:
     double *grades; 
     int capacity; 
     int count; 
     char option;

public:
    Grades(); 
    ~Grades(); 
    // at least 3 functions (C1)
    void ProgramGreeting();
    void Getgrades();
    char Grade2Lttr(double);
    void PrintScores();
    double GpaCalc();
    void Logger(const string& funct); 
};

Grades::Grades() { 
  capacity=1; 
  grades=new double[capacity]; 
  count=0;
}

Grades::~Grades() { 
  delete[] grades;
}

void Grades::ProgramGreeting() {
    Logger("greeting function"); // function activity to disk (C4)
    cout << "gpa.cpp" << endl;
}
char Grades::Grade2Lttr(double Grade1) {
    Logger("grade2lttr converting function");
if (Grade1>=90 && Grade1<=100) {
  return 'A';
 }
else if (Grade1<=90 && Grade1>=80) {
  return 'B';
 }
else if (Grade1<=80 && Grade1>=70) {
  return 'C';
 }
  else if (Grade1<0 || Grade1>100) {
    return 'X';
  }
else {
  return 'F';
 }
}
void Grades::Getgrades() {
    Logger("get grades and implement menu options function");
   do {
     cout << "1. Add grades (enter 'a')" << endl;
     cout << "2. Display All Grades (enter 'd')" << endl; 
     cout << "3. Process All Grades ( enter 'p')" << endl;
     cout << "4. Quit Program (enter 'q')" << endl;
     cin >> option;

     switch (option) {
       case 'a':
         if (count==capacity) {
           double *pTmp=new double[capacity*2]; // Dynamic array (B1)
         for (int i=0; i<count; i++){ 
           pTmp[i]=grades[i]; 
         }
           delete[] grades; 
           grades=pTmp;
           capacity*=2;
         }
           cout << "Enter a numeric grade:" << endl;
           cin >> grades[count]; // Add elements (B2)
           cout << "Letter grade: " << Grade2Lttr(grades[count]) << endl;
           count++;
       break;

       case 'd':
        PrintScores();
       break;

       case 'p':
        cout << "Grade average:" << GpaCalc() << endl;
       break;

       case 'q':
       break;

       default:
       cout << "Invalid" << endl; // Menu Input Validation (B3)
       break;
     }
     } while (option != 'q');
   }
     // Print Scores (C2)
void Grades::PrintScores() {
     Logger( "print scores function");
  cout << "Grades:" << endl;
      for (int i = 0; i < count; i++) {
          if (grades[i]<=100 && grades[i]>=90) {
          cout << "Grade " << i+1 << ": " << "\033[32m" << grades[i] <<  " - Letter grade: " << Grade2Lttr(grades[i]) << "\033[0m" << endl;
      } 
        else if (grades[i]<=89 && grades[i]>=70) {
              cout << "Grade " << i+1 << ": " << "\033[33m" << grades[i] << " - Letter grade: " << Grade2Lttr(grades[i]) << "\033[0m" << endl;
            }
        else {
          cout << "Grade " << i+1 << ": " << "\033[31m" << grades[i] << " - Letter grade: " << Grade2Lttr(grades[i]) << "\033[0m" << endl;
        }
        
        }
  }
      // Compute GPA (C3)
double Grades::GpaCalc() {
     Logger("gpa calculation function");
   if (count == 0) {
          cout << "No grades provided to compute GPA." << endl;
          return 0;
      }
      double totalcount = 0;
      for (int i = 0; i < count; i++) {
          totalcount += grades[i];
      }
      return totalcount / count;
  }

void Grades::Logger(const string& funct) {
    ofstream logFile("log.txt", ios_base::app); 
    if (logFile.is_open()) {
        time_t now = time(0);
        char* timeC = ctime(&now);
        logFile << timeC << funct << " function" << endl;
        logFile.close();
    }
}


int main() {
  Date currentDay;
  currentDay.DisplayDate();
  currentDay.CompTest();

  Grades Student1;
  Student1.ProgramGreeting();
  Student1.Getgrades();
  return 0;
}
