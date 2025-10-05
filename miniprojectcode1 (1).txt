package miniproject;

import java.io.*;
import java.util.Scanner;

class Attendance {
    String[] students;
    boolean[] attendance;
    int totalStudents;

    Attendance(int n) {
        totalStudents = n;
        students = new String[n];
        attendance = new boolean[n];
    }
    
    public void enterStudentNames() {
        Scanner sc = new Scanner(System.in);
        for (int i = 0; i < totalStudents; i++) {
            System.out.print("Enter name of student " + (i + 1) + ": ");
            students[i] = sc.nextLine();
        }
    }

    public void markAttendance() {
        Scanner sc = new Scanner(System.in);
        for (int i = 0; i < totalStudents; i++) {
            System.out.print("Is " + students[i] + " present? (yes/no): ");
            String input = sc.nextLine().trim().toLowerCase();
            attendance[i] = input.equals("yes");
        }
    }

    public void displayAttendance() {
        System.out.println("\nAttendance Records:");
        for (int i = 0; i < totalStudents; i++) {
            System.out.println(students[i] + " - " + (attendance[i] ? "Present" : "Absent"));
        }
    }

    public void saveToFile(String Attendence) {
        try (BufferedWriter writer = new BufferedWriter(new FileWriter(Attendence))) {
            for (int i = 0; i < totalStudents; i++) {
                writer.write(students[i] + "," + (attendance[i] ? "Present" : "Absent") + "\n");
            }
            System.out.println("Attendance saved successfully to " + Attendence);
        } catch (IOException e) {
            System.out.println("Error saving file: " + e.getMessage());
        }
    }

    public void loadFromFile(String Attendence) {
        try (BufferedReader reader = new BufferedReader(new FileReader(Attendence))) {
            String line;
            int i = 0;
            while ((line = reader.readLine()) != null && i < totalStudents) {
                String[] data = line.split(",");
                students[i] = data[0];
                attendance[i] = data[1].equals("Present");
                i++;
            }
            System.out.println("Attendance loaded successfully from " + Attendence);
        } catch (IOException e) {
            System.out.println("Error loading file: " + e.getMessage());
        }
    }
}

public class mini1 {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int num = 0;

        while (true) {
            System.out.print("Enter number of students: ");
            if (sc.hasNextInt()) {
                num = sc.nextInt();
                sc.nextLine(); 
                break;
            } else {
                System.out.println("Invalid input! Please enter a valid number.");
                sc.next(); 
            }
        }

        Attendance att = new Attendance(num);
        att.enterStudentNames();

        while (true) {
            System.out.println("\nMenu:");
            System.out.println("1. Mark Attendance");
            System.out.println("2. Display Attendance");
            System.out.println("3. Save Attendance to File");
            System.out.println("4. Load Attendance from File");
            System.out.println("5. Exit");
            System.out.print("Choose an option: ");

            int choice;
            if (sc.hasNextInt()) {
                choice = sc.nextInt();
                sc.nextLine(); 
            } else {
                System.out.println("Invalid choice! Please enter a number between 1 and 5.");
                sc.next(); 
                continue;
            }
            switch (choice) {
                case 1:
                    att.markAttendance();
                    break;
                case 2:
                    att.displayAttendance();
                    break;
                case 3:
                    System.out.print("Enter filename to save: ");
                    String saveFile = sc.nextLine();
                    att.saveToFile(saveFile);
                    break;
                case 4:
                    System.out.print("Enter filename to load: ");
                    String loadFile = sc.nextLine();
                    att.loadFromFile(loadFile);
                    break;
                case 5:
                    System.out.println("Exiting...");
                    return;
                default:
                    System.out.println("Invalid choice, try again.");
            }
        }
    }
}
