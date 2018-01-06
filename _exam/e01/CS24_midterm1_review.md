cs24 review


friend function vs member function
pointers - won't be on the exam but you are expected to know them for the upcoming pas
Design and implementation of classes
Use of references for passing parameters
operator overloading 
the big three
--------------------------------------

friend vs member

class point{
	public:
	    //Parameterized constructor with default parameters. 
		point(int xx=0, int yy=0){ x=xx; y=yy;} 
		point add(point p); //Member function
	private:
		int x;
		int y;
}

int main(){
	//Using the parameterized contructor
	point p; // x and y member variables are set to 0 0
	point p1(10); // x is set to 10, y is 0
	point p2(10, 20); // x is 10 and y is 20
}

--------------------------------------
preconditions and postconditions 
preconditions -- what must be true before function continues
postconditions -- what is expected to have happened after the function returns

classes versus abstract data type
classes -- data and member functions
abstract data type -- class + information hiding

class dogHouse{
	public: 
		dogHouse(int capacity);     //constructor that takes a parameter
		 
		//copy constructor 
		
		// assignment operator 
		// the big three are: constructor, copy constructor, assignment operator

		//destructor

		//member functions

	private:
		Dog* dogs;
		int capacity;
		int size; //number of dogs


};

class Dog{
	string name;
	string breed;
	Dog();
	Dog(string name, string breed);
};



// This example is good for the upcoming pas but will not be on the midterm
class MyClass {
 public:
	int x;
	int *y;
	MyClass(int i, int j) {x=i; y=new int; *y=j} // in line definition
	MyClass(const &MyClass obj); // The const keyword makes sure we can't modify 
                                 // the parameter obj even though it is passed by reference 
};

int main() {
	MyClass *c1 = new MyClass(3, 5);
	MyClass *c2 = new MyClass(*c1);

	c2.x = 10;
	*(c2.y) = 12;

}

MyClass::MyClass(const MyClass& obj) {
	x = obj.x;
	//y = obj.y;  where we copy the address instead of the value
	y = new int(obj->y);
}




--------------------------------------
operator overloading

function overloading ex:
Foo(){
	//do something	
}
Foo(int a){
	//do something with a
}

kind of like constructors, you can have constructors with no paramters, and constructors with parameters in the same class, the compiler will know what to call depending on how you call it. 

so there are many ways to define a certain operator, you overload the operator to define it the way you want/the way it should be. (ex: adding two arrays, what does it mean to you?)

OVERLOADING EXAMPLES:
class Array{
	public:
		int get(int index) const;
		bool append(int num);

		friend Array operator +(const Array& LHS, const Array& RHS);


	private:
		int* arr;
		int size;
		int capacity;
};

// The operator is applied to two Array objects: LHS and RHS
// It returns a new array that contains the sum of the elements of the two array objects. The capacity and size of the new array is set to be the maximum size and capacity of the two Array objects.
Array operator +(const Array& LHS, const Array& RHS){
	Array result;
	if(LHS.capacity >= RHS.capacity){
		result.capacity = LHS.capacity;
	}else{
		result.capacity = RHS.capacity;
	}
	if(LHS.size >= RHS.size){
		result.size = LHS.size;
	}else{
		result.size = RHS.size;
	}

	result.arr = new int[result.capacity];
	for(int i = 0; i<result.size; i++){
		int LHS_num;
		int RHS_num;

		if(i < LHS.size){
			LHS_num = LHS.arr[i];
		}
		else{
			LHS_num = 0;
		}
		if(i < RHS.size){
			RHS_num = RHS.arr[i];
		}
		else{
			RHS_num = 0;
		}

		result.arr[i] = LHS_num + RHS_num;
	}
	return result;
}




int main(){
	Point a; 
	Point b;

	cout<< a == b<< endl; //Need to overload == before you can use it like this

}


//this is for the Array class example
//what does it mean for two arrays to be equal? you overload to define what it means for two arrays to be equal
bool operator ==(const Array& LHS, const Array& RHS){
	if((LHS.size != RHS.size )|| (LHS.capacity != RHS.capacity)){
		return false;
	}

	for(int i = 0; i<RHS.size; i++){
		if(LHS.get(i) != RHS.get(i)){
			return false;
		}	
	}

	return true;

}

//this is for Point class with variables x and y.
//what does it mean for two Points to be equal?
//we want to define the == operator so that we can compare two points by checking their variable values 
bool operator ==(const Point& LHS, const Point& RHS){
	return(LHS.x == RHS.x && LHS.y == RHS.y);
}

------------------------------------------------------------
3 uses of const:
before function return type -- const int foo(); //used to return a const value
before variable -- int foo(const int x); //used so the parameter won't change
after function in classes -- void foo() const; //this won't change any of the member variables, only meaningful within a class definition
 									

------------------------------------------------------------
ASSERT VS IF/ELSE STATEMENT
assert will exit the program when condition is not met
if/else will continue to the else statement when condition is not met


------------------------------------------------------------
























