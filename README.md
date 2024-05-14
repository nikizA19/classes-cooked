# classes-cooked
Dai boje da si mina izpita
1. (30 %) Абстрактният базов клас Property трябва да съдържа поле за стойност и
собственик (до 50 символа), голямата петица, публични функции за извеждане на
текстовото представяне на член-данните в изходен поток и за въвеждането им от
входен поток, както и публична член-функция наречена taxCalculation().


#include <iostream>
#include <string>

using namespace std;

class Property
{
protected:
    string ownerName;
    double value;
public:
    //конструктор по подразбиране
    Property(){}
    //конструктор с параметри
        Property(const string& owner, double val) : ownerName(owner), value(val) {}
        // Конструктор по копиране
    Property(const Property& other) : ownerName(other.ownerName), value(other.value) {}
    // Оператор за присвояване
    Property& operator=(const Property& other) {
        if (this != &other) {
            ownerName = other.ownerName;
            value = other.value;
        }
        return *this;
    }
    // Чисто виртуална функция за изчисляване на данъка
    virtual double calculateTax() const = 0;
     //Функции за извеждане и въвеждане на данни от/в поток
    virtual void input(istream& in){
        cout<<"Enter owner name";
        in>>ownerName;
        cout<<"Enter property value:";
        in>>value;
    }
    virtual void output(ostream& out){
        out<<"Onwer Name: "<< ownerName <<endl;
        out <<" Property value: "<< value <<endl;
    }
    // Виртуален деструктор за правилното изтриване на обекти от производните класове
    virtual ~Property() {}
};
void Property::output(ostream& out) const {
    out << "Owner name: " << ownerName << endl;
    out << "Property value: " << value << endl;
}

2. (30 %) Производните класове Appartment и CountryHouse се характеризират с
адрес на жилището (до 50 символа) и размер в кв. м. (дробно число), а за класа
Car – марка (до 30 символа), модел (до 30 символа) и година на производство
(цяло число). За производните класове:
2.1 (15 %)Реализирате голямата петица и препокрийте функциите за четене и
писане в текстов поток.

class Appartment : public Property {
private:
    string address;
    double area;

public:
    Appartment(const string& owner, double val, const string& addr, double ar)
        : Property(owner, val), address(addr), area(ar) {}
    // Голямата четворка
    Appartment(const Appartment& other) : Property(other), address(other.address), area(other.area) {}
    Appartment& operator=(const Appartment& other) {
        if (this != &other) {
            Property::operator=(other);
            address = other.address;
            area = other.area;
        }
        return *this;
    }
    double taxCalculation() const override {
        return value * 0.001;
    }
    // Преепокриване на функциите за извеждане и въвеждане на данни
    void input(istream& in) override {
        Property::input(in);
        cout << "Enter address: ";
        in >> address;
        cout << "Enter area: ";
        in >> area;
    }
    void output(ostream& out) const override {
        Property::output(out);
        out << "Address: " << address << endl;
        out << "Area: " << area << endl;
    }
};

// Клас за къщи в село
class CountryHouse : public Property {
private:
    bool hasGarden;

public:
    CountryHouse(const string& owner, double val, bool garden) : Property(owner, val), hasGarden(garden) {}
    // Голямата четворка
    CountryHouse(const CountryHouse& other) : Property(other), hasGarden(other.hasGarden) {}
    CountryHouse& operator=(const CountryHouse& other) {
        if (this != &other) {
            Property::operator=(other);
            hasGarden = other.hasGarden;
        }
        return *this;
    }
    double taxCalculation() const override {
        return value * 0.002;
    }
    // Преепокриване на функциите за извеждане и въвеждане на данни
    void input(istream& in) override {
        Property::input(in);
        cout << "Does it have a garden? (1 for Yes, 0 for No): ";
        in >> hasGarden;
    }
    void output(ostream& out) const override {
        Property::output(out);
        out << "Has a garden: " << (hasGarden ? "Yes" : "No") << endl;
    }
};

// Клас за коли
class Car : public Property {
private:
    string brand;
    string model;
    int year;

public:
    Car(const string& owner, double val, const string& br, const string& mdl, int yr)
        : Property(owner, val), brand(br), model(mdl), year(yr) {}
    // Голямата четворка
    Car(const Car& other) : Property(other), brand(other.brand), model(other.model), year(other.year) {}
    Car& operator=(const Car& other) {
        if (this != &other) {
            Property::operator=(other);
            brand = other.brand;
            model = other.model;
            year = other.year;
        }
        return *this;
    }
    double taxCalculation() const override {
        return value * 0.005;
    }
    // Преепокриване на функциите за извеждане и въвеждане на данни
    void input(istream& in) override {
        Property::input(in);
        cout << "Enter brand: ";
        in >> brand;
        cout << "Enter model: ";
        in >> model;
        cout << "Enter year: ";
        in >> year;
    }
    void output(ostream& out) const override {
        Property::output(out);
        out << "Brand: " << brand << endl;
        out << "Model: " << model << endl;
        out << "Year: " << year << endl;
    }
};


int main(){
    Appartment apt("John Doe", 100000, "123 Main St", 80);
    Car car("Alice Smith", 20000, "Toyota", "Corolla", 2015);
    CountryHouse house("Bob Johnson", 150000, true);
    // Извеждане на данните за всяко имущество
    cout << "Appartment details:" << endl;
    apt.output(cout);
    //
    cout << "Tax for the appartment: $" << apt.taxCalculation() << endl;
    cout << endl;
    cout << "Car details:" << endl;
    //
    car.output(cout);
    cout << "Tax for the car: $" << car.taxCalculation() << endl;
    cout << endl;
    cout << "Country house details:" << endl;
    //
    house.output(cout);
    cout << "Tax for the country house: $" << house.taxCalculation() << endl;
    cout << endl;
    //
    return 0;
    

}
