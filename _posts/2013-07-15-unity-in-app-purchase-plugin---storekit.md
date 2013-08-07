---
layout: post
title: "Unity In app Purchase Plugin   StoreKit"
description: ""
category: 
tags: []
---
{% include JB/setup %}

在场景内新建一个 empty GameObject 绑定 StoreKitEventListener



	// 支付成功
	void purchaseSuccessful( StoreKitTransaction transaction )
	{
		int addMoney = 0;
		switch(transaction.productIdentifier)
		{
			case "com.company.appname.product1":
				addMoney = PlayerPrefs.GetInt("Money");
				PlayerPrefs.SetInt("Money",addMoney + 200);
				Debug.Log("pay0 ok!");
			break;
			case "com.company.appname.product2":
				addMoney = PlayerPrefs.GetInt("Money");
				PlayerPrefs.SetInt("Money",addMoney + 500);
			break;
			case "com.company.appname.product3":
				addMoney = PlayerPrefs.GetInt("Money");
				PlayerPrefs.SetInt("Money",addMoney + 800);
			break;
			case "com.company.appname.product4":
				addMoney = PlayerPrefs.GetInt("Money");
				PlayerPrefs.SetInt("Money",addMoney + 1500);
			break;
			case "com.company.appname.product5":
				addMoney = PlayerPrefs.GetInt("Money");
				PlayerPrefs.SetInt("Money",addMoney + 3500);
			break;
			case "com.company.appname.product6":
				addMoney = PlayerPrefs.GetInt("Money");
				PlayerPrefs.SetInt("Money",addMoney + 5500);
				ManChonCode.SendMessage("buy_Hero", 4);
			break;
			case "com.company.appname.product7":
				PlayerPrefs.SetInt("Double",1);
			break;
		}
	}