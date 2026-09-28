/*----------------Custom String Class Implementation in Modern C++--------------------*/

#include <iostream>
#include <string.h>
#include <fstream>

using namespace std;

class String
{
    char *p;

public:

    // Default constructor
    String()
    {
        p = new char[1];
        p[0] = '\0';
    }


    // Parameterized constructor
    String(const char *s)
    {
        p = new char[strlen(s) + 1];
        strcpy(p, s);
    }


    // Input constructor
    String(int)
    {
        char ch;
        int count = 0;

        ofstream fout("string.dat");

        if(!fout)
        {
            cout << "File opening failed" << endl;

            p = new char[1];
            p[0] = '\0';

            return;
        }

        cout << "Enter a string: ";

        while(1)
        {
            ch = cin.get();

            if(ch == '\n')
                break;

            fout.put(ch);
            count++;
        }

        fout.close();

        // Allocate exact memory
        p = new char[count + 1];

        // Read characters from file
        ifstream fin("string.dat");

        int i = 0;

        while(fin.get(ch))
        {
            p[i] = ch;
            i++;
        }

        p[i] = '\0';

        fin.close();
    }


    // Copy constructor
    String(const String &s)
    {
        p = new char[strlen(s.p) + 1];
        strcpy(p, s.p);
    }


    void setdata()
    {
        char ch;
        int count = 0;

        ofstream fout("string.dat");

        if(!fout)
        {
            cout << "File opening failed" << endl;
            return;
        }

        cout << "Enter a string: ";

        while(1)
        {
            ch = cin.get();

            if(ch == '\n')
                break;

            fout.put(ch);
            count++;
        }

        fout.close();

        delete [] p;

        p = new char[count + 1];

        ifstream fin("string.dat");

        int i = 0;

        while(fin.get(ch))
        {
            p[i] = ch;
            i++;
        }

        p[i] = '\0';

        fin.close();
    }


    void getdata()
    {
        cout << "Data: " << p << endl;
    }


    ~String()
    {
        delete [] p;
    }


    // Assignment operator
    String &operator=(const String &t)
    {
        if(this != &t)
        {
            delete [] p;

            p = new char[strlen(t.p) + 1];
            strcpy(p, t.p);
        }

        return *this;
    }


    // Assignment from C-string
    String &operator=(const char *s)
    {
        delete [] p;

        p = new char[strlen(s) + 1];
        strcpy(p, s);

        return *this;
    }


    // + operator
    String operator+(const String &t)
    {
        String a;

        delete [] a.p;

        a.p = new char[strlen(p) + strlen(t.p) + 1];

        strcpy(a.p, p);
        strcat(a.p, t.p);

        return a;
    }


    // [] operator
    char &operator[](int i)
    {
        return p[i];
    }


    // Comparison operators
    bool operator<(const String &s)
    {
        return strcmp(p, s.p) < 0;
    }


    bool operator>(const String &s)
    {
        return strcmp(p, s.p) > 0;
    }


    bool operator==(const String &s)
    {
        return strcmp(p, s.p) == 0;
    }


    bool operator!=(const String &s)
    {
        return strcmp(p, s.p) != 0;
    }


    bool operator>=(const String &s)
    {
        return strcmp(p, s.p) >= 0;
    }


    bool operator<=(const String &s)
    {
        return strcmp(p, s.p) <= 0;
    }


    friend ostream &operator<<(ostream &, const String &);
    friend istream &operator>>(istream &, String &);

    friend void Strcpy(String &, String &);
    friend String &Strncpy(String &, String &, int);
    friend bool Strcmp(String &, String &);
    friend int Strncmp(String &, String &, int);
    friend String Strcat(String &, const String &);
    friend String &Strncat(String &, const String &, int);
    friend void Strrev(String &);

    friend int Strlen(const String &);
    friend const char *Strchr(const String &, char ch);
    friend const char *Strrchr(const String &, char ch);
    friend void Strupper(String &);
    friend void Strlower(String &);
    friend const char *Strstr(String &, const char *);
};


// strstr
const char *Strstr(String &t, const char *sub)
{
    char *s = t.p;
    int i = 0, j = 0, k;

    if(sub[0] == '\0')
        return s;

    while(s[i])
    {
        if(s[i] == sub[0])
        {
            k = i + 1;
            j = 1;

            while(s[k] && sub[j])
            {
                if(s[k] != sub[j])
                    break;

                k++;
                j++;
            }

            if(sub[j] == '\0')
                return &s[i];
        }

        i++;
    }

    return 0;
}


// Convert to uppercase
void Strupper(String &s)
{
    int i = 0;

    while(s.p[i])
    {
        if(s.p[i] >= 'a' && s.p[i] <= 'z')
            s.p[i] -= 32;

        i++;
    }
}


// Convert to lowercase
void Strlower(String &s)
{
    int i = 0;

    while(s.p[i])
    {
        if(s.p[i] >= 'A' && s.p[i] <= 'Z')
            s.p[i] += 32;

        i++;
    }
}


// Find first occurrence
const char *Strchr(const String &t, char ch)
{
    int i = 0;

    while(t.p[i])
    {
        if(t.p[i] == ch)
            return &t.p[i];

        i++;
    }

    return 0;
}


// Find last occurrence
const char *Strrchr(const String &t, char ch)
{
    int i = Strlen(t) - 1;

    while(i >= 0)
    {
        if(t.p[i] == ch)
            return &t.p[i];

        i--;
    }

    return 0;
}


// Find length
int Strlen(const String &t)
{
    int i = 0;

    while(t.p[i])
        i++;

    return i;
}


// Reverse string
void Strrev(String &s)
{
    int i = 0;
    int len = strlen(s.p) - 1;
    char t;

    while(i < len)
    {
        t = s.p[i];
        s.p[i] = s.p[len];
        s.p[len] = t;

        i++;
        len--;
    }
}


// strncat
String &Strncat(String &s, const String &d, int n)
{
    int len1 = strlen(s.p);
    int len2 = strlen(d.p);

    if(n > len2)
        n = len2;

    char *temp = new char[len1 + n + 1];

    strcpy(temp, s.p);

    int i = 0;

    while(i < n)
    {
        temp[len1 + i] = d.p[i];
        i++;
    }

    temp[len1 + i] = '\0';

    delete [] s.p;

    s.p = temp;

    return s;
}


// strcat
String Strcat(String &s, const String &d)
{
    return s + d;
}


// strcmp
// Returns true if both strings are same
// Returns false if strings are different
bool Strcmp(String &s, String &d)
{
    int i = 0;

    while(s.p[i] != '\0' && d.p[i] != '\0')
    {
        if(s.p[i] != d.p[i])
            return false;

        i++;
    }

    if(s.p[i] == '\0' && d.p[i] == '\0')
        return true;

    return false;
}


// strncmp
int Strncmp(String &s, String &d, int n)
{
    int i = 0;

    while(i < n && s.p[i] != '\0' && d.p[i] != '\0')
    {
        if(s.p[i] != d.p[i])
            return s.p[i] - d.p[i];

        i++;
    }

    if(i == n)
        return 0;

    return s.p[i] - d.p[i];
}


// strncpy
String &Strncpy(String &s, String &d, int n)
{
    delete [] s.p;

    s.p = new char[n + 1];

    int i;

    for(i = 0; i < n && d.p[i]; i++)
    {
        s.p[i] = d.p[i];
    }

    s.p[i] = '\0';

    return s;
}


// strcpy
void Strcpy(String &t1, String &t2)
{
    t1 = t2;
}


// << operator
ostream &operator<<(ostream &out, const String &t)
{
    out << t.p;

    return out;
}


// >> operator
istream &operator>>(istream &in, String &t)
{
    char ch;
    int count = 0;

    ofstream fout("string.dat");

    if(!fout)
        return in;

    while(1)
    {
        ch = in.get();

        if(ch == '\n')
            break;

        fout.put(ch);
        count++;
    }

    fout.close();

    delete [] t.p;

    t.p = new char[count + 1];

    ifstream fin("string.dat");

    int i = 0;

    while(fin.get(ch))
    {
        t.p[i] = ch;
        i++;
    }

    t.p[i] = '\0';

    fin.close();

    return in;
}


// ================= MAIN =================

int main()
{
    String s1("Hello");
    String s2("World");
    String s3;

    cout << "===== Initial Strings =====" << endl;
    cout << "s1 = " << s1 << endl;
    cout << "s2 = " << s2 << endl;


    cout << "\n===== Copy Constructor =====" << endl;

    String s4(s1);

    cout << "s4 = " << s4 << endl;


    cout << "\n===== Assignment Operator =====" << endl;

    s3 = s2;

    cout << "s3 = " << s3 << endl;


    cout << "\n===== Assignment from const char* =====" << endl;

    s3 = "C++ Programming";

    cout << "s3 = " << s3 << endl;


    cout << "\n===== + Operator =====" << endl;

    String s5 = s1 + s2;

    cout << "s1 + s2 = " << s5 << endl;


    cout << "\n===== [] Operator =====" << endl;

    cout << "s1[0] = " << s1[0] << endl;

    s1[0] = 'h';

    cout << "After changing s1[0] = "
         << s1 << endl;


    cout << "\n===== Comparison Operators =====" << endl;

    cout << "s1 = " << s1 << endl;
    cout << "s2 = " << s2 << endl;

    cout << "s1 < s2  = " << (s1 < s2) << endl;
    cout << "s1 > s2  = " << (s1 > s2) << endl;
    cout << "s1 == s2 = " << (s1 == s2) << endl;
    cout << "s1 != s2 = " << (s1 != s2) << endl;
    cout << "s1 >= s2 = " << (s1 >= s2) << endl;
    cout << "s1 <= s2 = " << (s1 <= s2) << endl;


    cout << "\n===== Strlen =====" << endl;

    cout << "Length of s1 = "
         << Strlen(s1) << endl;


    cout << "\n===== Strcpy =====" << endl;

    String s6("Temporary");

    Strcpy(s6, s1);

    cout << "s6 = " << s6 << endl;


    cout << "\n===== Strncpy =====" << endl;

    String s7("ABCDEFGHIJ");
    String s8("Hello");

    Strncpy(s7, s8, 3);

    cout << "s7 = " << s7 << endl;


    cout << "\n===== Strcmp =====" << endl;

    if(Strcmp(s1, s2))
        cout << "Same" << endl;
    else
        cout << "Not Same" << endl;


    cout << "\n===== Strncmp =====" << endl;

    cout << "Strncmp(s1,s2,3) = "
         << Strncmp(s1, s2, 3) << endl;


    cout << "\n===== Strcat =====" << endl;

    String s9("Hello ");
    String s10("World");

    String s11 = Strcat(s9, s10);

    cout << "Strcat = " << s11 << endl;


    cout << "\n===== Strncat =====" << endl;

    String s12("Hello ");

    Strncat(s12, s10, 3);

    cout << "Strncat = " << s12 << endl;


    cout << "\n===== Strrev =====" << endl;

    String s13("ABCDE");

    cout << "Before reverse = "
         << s13 << endl;

    Strrev(s13);

    cout << "After reverse = "
         << s13 << endl;


    cout << "\n===== Strupper =====" << endl;

    String s14("hello world");

    Strupper(s14);

    cout << "Uppercase = "
         << s14 << endl;


    cout << "\n===== Strlower =====" << endl;

    String s15("HELLO WORLD");

    Strlower(s15);

    cout << "Lowercase = "
         << s15 << endl;


    cout << "\n===== Strchr =====" << endl;

    String s16("Hello World");

    const char *p1 = Strchr(s16, 'W');

    if(p1)
        cout << "Character found: "
             << p1 << endl;
    else
        cout << "Character not found" << endl;


    cout << "\n===== Strrchr =====" << endl;

    const char *p2 = Strrchr(s16, 'l');

    if(p2)
        cout << "Last occurrence: "
             << p2 << endl;
    else
        cout << "Character not found" << endl;


    cout << "\n===== Strstr =====" << endl;

    const char *p3 = Strstr(s16, "World");

    if(p3)
        cout << "Substring found: "
             << p3 << endl;
    else
        cout << "Substring not found" << endl;


    cout << "\n===== Character-by-Character Input =====" << endl;

    String s17(1);

    cout << "Entered string: ";
    s17.getdata();


    return 0;
}
