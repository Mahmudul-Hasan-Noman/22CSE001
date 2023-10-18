# 22CSE001
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

struct Patient {
    char name[50];
    int age;
    char gender[10];
    char homeAddress[100];
    char bloodGroup[5];
    float prevBloodPressure;
    float prevBloodSugarLevel;
    float prevHDLLevel;
    float prevLDLLevel;
};

struct Doctor {
    char name[50];
    char medicalCollege[100];
    char qualification[100];
    int experienceYears;
};

void admitPatient() {
    struct Patient patient;
    FILE *admitFile = fopen("admit_patient.txt", "a");

    if (admitFile == NULL) {
        printf("Error opening file for admission.\n");
        return;
    }

    printf("\nEnter patient details:\n");
    printf("\nName: ");
    scanf("%s", patient.name);
    printf("\nAge: ");
    scanf("%d", &patient.age);
    printf("\nGender: ");
    scanf("%s", patient.gender);
    printf("\nHome Address: ");
    scanf("%s", patient.homeAddress);
    printf("\nBlood Group: ");
    scanf("%s", patient.bloodGroup);
    printf("\nPrevious Blood Pressure: ");
    scanf("%f", &patient.prevBloodPressure);
    printf("\nPrevious Blood Sugar Level: ");
    scanf("%f", &patient.prevBloodSugarLevel);
    printf("\nPrevious HDL Level: ");
    scanf("%f", &patient.prevHDLLevel);
    printf("\nPrevious LDL Level: ");
    scanf("%f", &patient.prevLDLLevel);

    fprintf(admitFile, "Name: %s\nAge: %d\nGender: %s\nHome Address: %s\nBlood Group: %s\nPrevious Blood Pressure: %.2f\nPrevious Blood Sugar Level: %.2f\nPrevious HDL Level: %.2f\nPrevious LDL Level: %.2f\n\n",
            patient.name, patient.age, patient.gender, patient.homeAddress, patient.bloodGroup,
            patient.prevBloodPressure, patient.prevBloodSugarLevel, patient.prevHDLLevel, patient.prevLDLLevel);

    fclose(admitFile);
    printf("\nPatient admitted successfully.\n");
}

void showPatientList() {
    char line[100];
    FILE *admitFile = fopen("admit_patient.txt", "r");

    if (admitFile == NULL) {
        printf("No patients admitted yet.\n");
        return;
    }

    printf("Admitted Patients List:\n");
    while (fgets(line, sizeof(line), admitFile) != NULL) {
        printf("%s", line);
    }

    fclose(admitFile);
}

void dischargePatient() {
    char patientName[50];
    FILE *admitFile = fopen("admit_patient.txt", "r");
    FILE *dischargeFile = fopen("discharge_patient.txt", "a");

    if (admitFile == NULL || dischargeFile == NULL) {
        printf("Error opening files for discharge.\n");
        return;
    }

    printf("Enter the name of the patient to discharge: ");
    scanf("%s", patientName);

    char line[100];
    while (fgets(line, sizeof(line), admitFile) != NULL) {
        if (strstr(line, patientName) != NULL) {
            fprintf(dischargeFile, "%s", line);
        }
    }

    fclose(admitFile);
    fclose(dischargeFile);

    admitFile = fopen("admit_patient.txt", "w");
    fclose(admitFile);

    printf("Patient discharged successfully.\n");
}

void addDoctor() {
    struct Doctor doctor;
    FILE *doctorFile = fopen("doctor_list.txt", "a");

    if (doctorFile == NULL) {
        printf("Error opening file for adding a doctor.\n");
        return;
    }

    printf("\nEnter doctor details:\n");
    printf("Name: ");
    scanf("%s", doctor.name);
    printf("\nQualification: ");
    scanf("%s", doctor.qualification);
    printf("\nMedical College: ");
    scanf("%s", doctor.medicalCollege);
    printf("\nExperience Years: ");
    scanf("%d", &doctor.experienceYears);

    fprintf(doctorFile, "Name: %s\nQualification: %s\nMedical College: %s\nExperience Years: %d\n\n",
            doctor.name, doctor.qualification,doctor.medicalCollege, doctor.experienceYears);

    fclose(doctorFile);
    printf("\nDoctor added successfully.\n");
}

void showDoctorList() {
    char line[100];
    FILE *doctorFile = fopen("doctor_list.txt", "r");

    if (doctorFile == NULL) {
        printf("\n No doctors added yet.\n");
        return;
    }

    printf("Doctor List:\n");
    while (fgets(line, sizeof(line), doctorFile) != NULL) {
        printf("%s", line);
    }

    fclose(doctorFile);
}

int main() {
    int choice;
    printf("Hospital Management System\n");

    do {
        printf("\nMenu:\n");
        printf("1. Admit a Patient\n");
        printf("2. Show Admitted Patients List\n");
        printf("3. Discharge a Patient\n");
        printf("4. Add a Doctor\n");
        printf("5. Show Doctor List\n");
        printf("6. Exit\n");
        printf("Enter your choice: ");
        scanf("%d", &choice);

        switch (choice) {
            case 1:
                admitPatient();
                break;
            case 2:
                showPatientList();
                break;
            case 3:
                dischargePatient();
                break;
            case 4:
                addDoctor();
                break;
            case 5:
                showDoctorList();
                break;
            case 6:
                printf("Exiting the program.\n");
                break;
            default:
                printf("Invalid choice. Please try again.\n");
        }
    } while (choice != 6);

    return 0;
}

