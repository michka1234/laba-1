#include <iostream>
#include <fstream>
#include <string>
#include <vector>
#include <cstdlib>
#include <algorithm>
#include <locale>
using namespace std;

class CatPublic {
public:
    string name;
    string breed;
    double weight;
    int ageMonths;

    int getFullYears() const {
        return ageMonths / 12;
    }

    void show() const {
        cout << name << "  " << breed << "  " << weight << "кг  "
             << ageMonths << " мес  (" << getFullYears() << " лет)" << endl;
    }
};

class CatPrivate {
private:
    string name;
    string breed;
    double weight;
    int ageMonths;

public:
    void set_name(string n) { name = n; }
    void set_breed(string b) { breed = b; }
    void set_weight(double w) { weight = w; }
    void set_ageMonths(int am) { ageMonths = am; }

    string get_name() const { return name; }
    string get_breed() const { return breed; }
    double get_weight() const { return weight; }
    int get_ageMonths() const { return ageMonths; }

    int getFullYears() const {
        return ageMonths / 12;
    }

    void show() const {
        cout << name << "  " << breed << "  " << weight << "кг  "
             << ageMonths << " мес  (" << getFullYears() << " лет)" << endl;
    }
};

bool compareByNamePublic(const CatPublic& a, const CatPublic& b) {
    return a.name < b.name;
}

bool compareByNamePrivate(const CatPrivate& a, const CatPrivate& b) {
    return a.get_name() < b.get_name();
}

int main() {
    setlocale(LC_ALL, "RUS");
    vector<CatPublic> pub;
    vector<CatPrivate> priv;

    ofstream f1("кошки.txt");
    f1 << "Мурка,Сиамская,3.5,24\n"
        << "Барсик,Дворовая,5.2,60\n"
        << "Рыжик,Персидская,4.8,36\n"
        << "Снежка,Британская,6.1,132\n"
        << "Тимофей,Мейн-кун,7.5,48\n"
        << "Васька,Сфинкс,3.2,18\n"
        << "Люся,Русская_голубая,4.2,144\n"
        << "Персик,Бенгальская,5.5,30\n"
        << "Маркиза,Абиссинская,4.0,96\n"
        << "Кузя,Шотландская,5.0,72\n";
    f1.close();

    ifstream f2("кошки.txt");
    string line;

    while (getline(f2, line)) {
        int p1 = line.find(',');
        int p2 = line.find(',', p1 + 1);
        int p3 = line.find(',', p2 + 1);

        string n = line.substr(0, p1);
        string b = line.substr(p1 + 1, p2 - p1 - 1);
        double w = atof(line.substr(p2 + 1, p3 - p2 - 1).c_str());
        int am = atoi(line.substr(p3 + 1).c_str());

        CatPublic cp;
        cp.name = n;
        cp.breed = b;
        cp.weight = w;
        cp.ageMonths = am;
        pub.push_back(cp);

        CatPrivate cp2;
        cp2.set_name(n);
        cp2.set_breed(b);
        cp2.set_weight(w);
        cp2.set_ageMonths(am);
        priv.push_back(cp2);
    }
    f2.close();

    double min_weight;
    int min_age_years;

    cout << "PUBLIC КЛАСС" << endl;
    cout << "Введите минимальный вес (кг): ";
    cin >> min_weight;
    cout << "Введите минимальный возраст (лет): ";
    cin >> min_age_years;

    vector<CatPublic> pub_sorted = pub;
    sort(pub_sorted.begin(), pub_sorted.end(), compareByNamePublic);

    cout << "\nа) Все кошки (в алфавитном порядке):" << endl;
    for (int i = 0; i < pub_sorted.size(); i++) pub_sorted[i].show();

    cout << "\nб) Кошки с весом > " << min_weight << " кг:" << endl;
    for (int i = 0; i < pub.size(); i++)
        if (pub[i].weight > min_weight) pub[i].show();

    cout << "\nв) Кошки старше " << min_age_years << " лет:" << endl;
    for (int i = 0; i < pub.size(); i++)
        if (pub[i].getFullYears() > min_age_years) pub[i].show();

    cout << "\n\nRIVATE КЛАСС" << endl;

    vector<CatPrivate> priv_sorted = priv;
    sort(priv_sorted.begin(), priv_sorted.end(), compareByNamePrivate);

    cout << "\nа) Все кошки (в алфавитном порядке):" << endl;
    for (int i = 0; i < priv_sorted.size(); i++) priv_sorted[i].show();

    cout << "\nб) Кошки с весом > " << min_weight << " кг:" << endl;
    for (int i = 0; i < priv.size(); i++)
        if (priv[i].get_weight() > min_weight) priv[i].show();

    cout << "\nв) Кошки старше " << min_age_years << " лет:" << endl;
    for (int i = 0; i < priv.size(); i++)
        if (priv[i].getFullYears() > min_age_years) priv[i].show();

    return 0;
}
