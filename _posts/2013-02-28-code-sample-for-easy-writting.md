---
layout: post
title: "Code Sample for easy writting"
description: ""
category: 
tags: []
---
{% include JB/setup %}
记录几种常用语言的语法,方便写作
#C
	#include <stdio.h>
	int add (int, int);
	int subtract (int, int);
	int do_math (int (*math_fn_ptr) (int, int), int, int);
	int main();

	int main()
	{
	  int result;

	  result = do_math (add, 10, 5);
	  printf ("Addition = %d.\n", result);

	  result = do_math (subtract, 40, 5);
	  printf ("Subtraction = %d.\n\n", result);

	  return 0;
	}

	int add (int num1, int num2)
	{
	  return (num1 + num2);
	}


	int subtract (int num1, int num2)
	{
	  return (num1 - num2);
	}


	int do_math (int (*math_fn_ptr) (int, int), int num1, int num2)
	{
	  int result;

	  printf ("\ndo_math here.\n");

	  /* Call one of the math functions passed to us:
	     either add or subtract. */

	  result = (*math_fn_ptr) (num1, num2);
	  return result;
	}
#C++
	#include <iostream>
	using namespace std;

	class CRectangle {
	    int width, height;
	  public:
	    void set_values (int, int);
	    int area (void) {return (width * height);}
	};

	void CRectangle::set_values (int a, int b) {
	  width = a;
	  height = b;
	}

	int main () {
	  CRectangle a, *b, *c;
	  CRectangle * d = new CRectangle[2];
	  b= new CRectangle;
	  c= &a;
	  a.set_values (1,2);
	  b->set_values (3,4);
	  d->set_values (5,6);
	  d[1].set_values (7,8);
	  cout << "a area: " << a.area() << endl;
	  cout << "*b area: " << b->area() << endl;
	  cout << "*c area: " << c->area() << endl;
	  cout << "d[0] area: " << d[0].area() << endl;
	  cout << "d[1] area: " << d[1].area() << endl;
	  delete[] d;
	  delete b;
	  return 0;
	}

#Objective-c