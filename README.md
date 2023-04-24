# trying_version
// classWork 10.04.23.cpp : Этот файл содержит функцию "main". Здесь начинается и заканчивается выполнение программы.
//

#define _CRT_SECURE_NO_WARNINGS
#include "iostream"
#include "cstdio"
#include "direct.h"
#include "cstring"
#include <fstream>
#include <stdio.h>
#include <stdlib.h>
using namespace std;
struct infomusic {
    char mname[60];
    char avtor[60];
    unsigned short int year;
    char text[256];
};
void InputMusic(infomusic* m, int size)
{
    char buf[255];
    for (int i = 0; i < size; ++i)
    {
        cout << "Input title: "; cin >> buf;
        strcpy(m[i].mname, buf);
        cout << "Input avtor: "; cin >> buf;
        strcpy(m[i].avtor, buf);
        cout << "Input text: "; cin >> buf;
        strcpy(m[i].text, buf);
        cout << "Input year: "; cin >> m[i].year;
        cout << "-------------------\n";
    }
    system("CLS");
}

void SerchAvtorMusic(infomusic* m, int size, char* initAvtor)
{
    for (int i = 0; i < size; ++i)
    {
        if (strcmp(m[i].avtor, initAvtor) == 0)
        {
            cout << "\n--------------------";
            cout << "\nTitle: " << m[i].mname;
            cout << "\nAvtor: " << m[i].avtor;
            cout << "\nYear: " << m[i].year;
            cout << "\nText: " << m[i].text;
            cout << "\n--------------------";
        }
    }
}
int save(infomusic* m, char* list)
{
    FILE* fp;
    char* c;
    int size = sizeof(struct infomusic);

    if ((fp = fopen(list, "ab")) == NULL)
    {
        perror("Error occured while opening file");
        return 1;
    }
    c = (char*)m;
    for (int i = 0; i < size; i++)
    {
        putc(*c++, fp);
    }
    fclose(fp);
    return 0;
}

int load(infomusic* m, char* list)
{
    FILE* fp;
    char* c;
    int i;
    int size = sizeof(struct infomusic);
    struct infomusic* ptr = (struct infomusic*)malloc(size);

    if ((fp = fopen(list, "rb")) == NULL)
    {
        perror("Error occured while opening file");
        return 1;
    }
    c = (char*)ptr;
    while ((i = getc(fp)) != EOF)
    {
        *c = i;
        c++;
    }

    fclose(fp);
    cout << "Title: " << ptr->mname << "\nYear: " << ptr->year << "\nAvtor: " << ptr->avtor << "\nText: \n" << ptr->text;
    free(ptr);
    return 0;
}
int firstload(infomusic* m, char* list)
{
    FILE* fp;
    char* c;
    int i;
    int size = sizeof(struct infomusic);
    struct infomusic* ptr = (struct infomusic*)malloc(size);

    if ((fp = fopen(list, "rb")) == NULL)
    {
        perror("Error occured while opening file");
        return 1;
    }
    c = (char*)ptr;
    while ((i = getc(fp)) != EOF)
    {
        *c = i;
        c++;
    }
    cout << "Title: " << ptr->mname;
    free(ptr);
    fclose(fp);
}

int main()
{
    char list[] = "List.dat";
    int size = 1;
    infomusic* music = new infomusic[size];
    char yn;
    do
    {
        cout << "Music catalod by comand #4\n\n";
        cout << "Output music list [Y-yes/N-no]"; cin >> yn;
        if (tolower(yn) == 'y') {
            firstload(music, list);
            int a;
            cout << "\nInfo track[1] ";
            cout << " Add track[2] ";
            cout << " Delete track[3]";
            cout << " Change track[4] ";
            cout << "Find avtor's music[5] "; cin >> a;
            switch (a) {
            case 1:
            {
                a = 0;
                char inname[60];
                cout << "Enter title music: "; gets_s(inname, 60); break;
            }
            case 2:
            {
                a = 0;
                InputMusic(music, size); save(music, list); load(music, list); break; }
            case 3:
            {
                a = 0;
                load(music, list); cout << "Enter number of music: "; cin >> a; }
            case 4:
            {
                a = 0;
                load(music, list); cout << "Enter number of music: "; cin >> a; }
            case 5: {
                a = 0;
                cout << "Input avtor: ";
                char initAvtor[60];
                gets_s(initAvtor, 60);
                SerchAvtorMusic(music, size, initAvtor);
            }
            default: cout << "Enter corect number";
            }
        }
    } while (tolower(yn) != 'y');
}
