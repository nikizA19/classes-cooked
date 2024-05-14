    #include <iostream>
    #include <fstream>
    #include <string>
    
    namespace std;
    
    class Shape {
    protected:
        double height;
        double width;
        int figureNumber;
        string title;
    
    public:
        Shape(double h, double w, int number, const string& t) 
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

    class Triangle : public Shape {
    public:
        Triangle(double h, double w, int number, const string& t) 
            : Shape(h, w, number, t) {}

    ~Triangle() {}

    double calculateSurface() const override {
        return (height * width) / 2;
    }

    void read(istream& is) override {
        Shape::read(is);
    }

    void print(ostream& os) const override {
        Shape::print(os);
    }
};

     class Rectangle : public Shape {
    public:
       Rectangle(double h, double w, int number, const string& t) 
        : Shape(h, w, number, t) {}

    ~Rectangle() {}

    double calculateSurface() const override {
        return height * width;
    }

    void read(istream& is) override {
        Shape::read(is);
    }

    void print(ostream& os) const override {
        Shape::print(os);
    }
    };

    class Circle : public Shape {
    public:
    Circle(double r, int number, const string& t) 
        : Shape(r, r, number, t) {} // Height and width are both radius

    ~Circle() {}

    double calculateSurface() const override {
        return 3.14159 * height * height;
    }

    void read(istream& is) override {
        Shape::read(is);
    }

    void print(ostream& os) const override {
        Shape::print(os);
    }
    };
 
    int main() {
    Shape* shapes[3];
    shapes[0] = new Triangle(5, 10, 1, "Triangle");
    shapes[1] = new Rectangle(6, 8, 2, "Rectangle");
    shapes[2] = new Circle(4, 3, "Circle");

    ofstream outFile("figures.txt");
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
