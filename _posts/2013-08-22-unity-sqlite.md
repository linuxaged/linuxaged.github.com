---
layout: post
title: "Unity Sqlite"
description: ""
category: 
tags: []
---
{% include JB/setup %}

#准备
需要添加 **Mono.Data.Sqlite.dll**

下面两个子目录下都含有这个库, 区别在于 c# 标准不一样, 2.0 和 unity 分别对应于 PlayerSettings 下面 api compatibility level: **.NET 2.0** 和 **.NET 2.0 subset**
	
	Unity.app/Contents/Frameworks/Mono/lib/mono
								/2.0
								/unity
											    
											    


#使用

引用 sqlite 库

	using Mono.Data.Sqlite;
	
##无返回结果查询
	// 建立连接
    connectionString = "URI=file:" +Application.dataPath + "/player.db";
    SqliteConnection conn = new SqliteConnection (connectionString);
    
    try
    {
        conn.Open ();
        // 无返回结果的查询-新建表
        SqliteCommand dbcmd_create = conn.CreateCommand();
        dbcmd_create.CommandText = "CREATE TABLE player (id INTEGER, name TEXT, score INTEGER);";
        dbcmd_create.ExecuteNonQuery();

        dbcmd_create.Dispose();
        dbcmd_create = null;
        // 无返回结果的查询-插入数据
        SqliteCommand dbcmd_insert = conn.CreateCommand();
        dbcmd_insert.CommandText = "INSERT INTO player (id , name, score) VALUES ('0', 'NAME', '0');";
        dbcmd_insert.ExecuteNonQuery();

        // 扫尾工作
        dbcmd_insert.Dispose();
        dbcmd_insert = null;
        conn.Close();
        conn = null;

        Debug.Log ("Create db success");
    }
    catch(Exception e)
    {
        string error = e.ToString();
        Debug.Log(error);
    }
        
        
##需要返回结果的查询
    SqliteCommand dbcmd_select = conn.CreateCommand();
    dbcmd_select.CommandText = "SELECT * FROM player ORDER by score DESC LIMIT 50";
    SqliteDataReader reader = dbcmd_select.ExecuteReader();

    if (reader.HasRows) {
        while(reader.Read()) {
        int id = (int)reader.GetInt32(0);
        string name = reader.GetString(1);
        int score = (int)reader.GetInt32(2);
        Debug.Log ( id + name + score);
        }
    }