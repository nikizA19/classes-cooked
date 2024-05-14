    #include <iostream>
    #include <fstream>
    #include <string>
    
    using namespace std;
    
    template<typename T>
    class Shape {
    protected:
        T height;
        T width;
        int figureNumber;
        string title;
    
    public:
        Shape(T h, T w, int number, const string& t) 
            : height(h), width(w), figureNumber(number), title(t) {}

    virtual ~Shape() {}

    virtual double calculateSurface() const = 0;

    virtual void print(ostream& os) const {
        os << "Figure Number: " << figureNumber << endl;
        os << "Title: " << title << endl;
        os << "Height: " << height << endl;
        os << "Width: " << width << endl;
        os << "Surface: " << calculateSurface() << endl;
    }

    virtual void read(istream& is) {
        is >> height >> width >> figureNumber >> title;
    }
    };
    
    template<typename T>
    class Triangle : public Shape<T> {
    public:
        Triangle(T h, T w, int number, const string& t) 
            : Shape<T>(h, w, number, t) {}

    ~Triangle() {}

    double calculateSurface() const override {
        return (this->height * this->width) / 2;
    }

    void read(istream& is) override {
        Shape<T>::read(is);
    }

    void print(ostream& os) const override {
        Shape<T>::print(os);
    }
    };
    
    template<typename T>
    class Rectangle : public Shape<T> {
    public:
        Rectangle(T h, T w, int number, const string& t) 
            : Shape<T>(h, w, number, t) {}

    ~Rectangle() {}

    double calculateSurface() const override {
        return this->height * this->width;
    }

    void read(istream& is) override {
        Shape<T>::read(is);
    }

    void print(ostream& os) const override {
        Shape<T>::print(os);
    }
    };
    
    template<typename T>
    class Circle : public Shape<T> {
    public:
        Circle(T r, int number, const string& t) 
            : Shape<T>(r, r, number, t) {} // Height and width are both radius

    ~Circle() {}

    double calculateSurface() const override {
        return 3.14159 * this->height * this->height;
    }

    void read(istream& is) override {
        Shape<T>::read(is);
    }

    void print(ostream& os) const override {
        Shape<T>::print(os);
    }
    };

    int main() {
    Shape<double>* shapes[3];
    shapes[0] = new Triangle<double>(2, 5, 10, "Triangle");
    shapes[1] = new Rectangle<double>(5, 5, 5, "Rectangle");
    shapes[2] = new Circle<double>(5, 2, "Circle");

    ofstream outFile("figure.txt");
    if (outFile.is_open()) {
        for (int i = 0; i < 3; ++i) {
            shapes[i]->print(outFile);
            outFile << endl;
        }
        outFile.close();
    } else {
        cerr << "Unable to open file." << endl;
    }

    for (int i = 0; i < 3; ++i) {
        delete shapes[i];
    }

    return 0;
    }
