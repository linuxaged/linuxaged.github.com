---
layout: post
title: "functional programming in c plus plus"
description: ""
category: 
tags: []
---
{% include JB/setup %}

	#include <iostream>

	// abstract base class for a continuation functor
	struct continuation {
	    virtual void operator() (unsigned) const = 0;
	};

	// accumulating continuation functor
	struct accum_cont: public continuation {
	    private:
	        unsigned accumulator_;
	        const continuation &enclosing_;
	    public:
	        accum_cont(unsigned accumulator, const continuation &enclosing)
	            : accumulator_(accumulator), enclosing_(enclosing) {}; 
	        virtual void operator() (unsigned n) const {
	            enclosing_(accumulator_ * n);
	        };
	};

	void fact_cps (unsigned n, const continuation &c)
	{
	    if (n == 0)
	        c(1);
	    else
	        fact_cps(n - 1, accum_cont(n, c));
	}

	int main ()
	{
	    // continuation which displays its' argument when called
	    struct disp_cont: public continuation {
	        virtual void operator() (unsigned n) const {
	            std::cout << n << std::endl;
	        };
	    } dc;

	    // continuation which multiplies its' argument by 2
	    // and displays it when called
	    struct mult_cont: public continuation {
	        virtual void operator() (unsigned n) const {
	            std::cout << n * 2 << std::endl;
	        };
	    } mc;

	    fact_cps(4, dc); // prints 24
	    fact_cps(5, mc); // prints 240

	    return 0;
	}