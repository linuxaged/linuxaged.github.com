---
layout: post
title: "Sorting Algorithm Comparison"
description: ""
category: 
tags: []
---
{% include JB/setup %}

#Reference

[排序算法比较](http://www.cprogramming.com/tutorial/computersciencetheory/sortcomp.html)

[快速排序](http://www.algolist.net/Algorithms/Sorting/Quicksort)

[快速排序C#](http://snipd.net/quicksort-in-c)

效率最高的排序中, QuickSort 最常用


    public static void Quicksort(IComparable[] elements, int left, int right)
            {
                int i = left, j = right;
                IComparable pivot = elements[(left + right) / 2];
     
                while (i <= j)
                {
                    while (elements[i].CompareTo(pivot) < 0)
                    {
                        i++;
                    }
     
                    while (elements[j].CompareTo(pivot) > 0)
                    {
                        j--;
                    }
     
                    if (i <= j)
                    {
                        // Swap
                        IComparable tmp = elements[i];
                        elements[i] = elements[j];
                        elements[j] = tmp;
     
                        i++;
                        j--;
                    }
                }
     
                // Recursive calls
                if (left < j)
                {
                    Quicksort(elements, left, j);
                }
     
                if (i < right)
                {
                    Quicksort(elements, i, right);
                }
            }
