---
layout: post
title: 在cocos2d-x中使用LUA
---
####1、注册LUA脚本引擎
    CCLuaEngine* pEngine = CCLuaEngine::defaultEngine();
    CCScriptEngineManager::sharedManager()->setScriptEngine(pEngine);


####2、执行一段LUA字符串
    luaEngine->executeString("print(\"test executeString\\n\")");


####3、执行一个LUA文件
    luaEngine->executeScriptFile(CCFileUtils::sharedFileUtils()->fullPathFromRelativePath("hello.lua"));


####4、执行LUA中的一个全局方法
	luaEngine->executeGlobalFunction("test");	// 执行LUA中的test方法


####5、执行LUA中的某个带参数的方法并获取返回值
	lua_State* state = luaEngine->getLuaState();
	lua_getglobal(state, "myadd");	//查找lua_add函数,并压入栈底    
	luaEngine->pushInt(5);		//函数参数1  
	luaEngine->pushFloat(6.5);	//函数参数2
	lua_pcall(state, 2, 1, 0);	//调用myadd函数，同时会对myadd及两个参加进行出栈操作,并压入返回值
	float result = lua_tonumber(state, -1);	//从栈中取回返回值   
	lua_pop(state, 1);	//清栈，由于当前只有一个返回值 
	printf("result is %f\n", result);


####6、在LUA中执行C++中的一个全局方法
	1）、在C++中注册方法：lua_register(state, "lua_add", add); // 第二个参数为将在LUA中用到的方法名，第三个参数为C++中对应的全局函数
	2）、在LUA中调用方法：lua_add(2, 3);
	
	第一步中的add方法示例：
	int add(lua_State* L)
	{
		int a = lua_tointeger(L, 1);
		int b = lua_tointeger(L, 2);
		
		lua_pushinteger(L, a + b);	//入栈返回值
		return 1;	//1表示压入栈数据个数 
	}
	
	
####7、在LUA中调用C++的对象及对象的方法
1）、在C++中编写一个C++的类。
{% highlight c++ %}
#ifndef _HELLO_CLASS_CTEST_
#define _HELLO_CLASS_CTEST_

#include <iostream>

class CTest
{
public:
	int v;
	
public:
	CTest() { v = 1; }
	CTest(int a) { v = a; }
	
	void test()
	{
		printf("[CTest::test]hello,tolua++|(%d).\n", this->v);
	}

};

#endif
{% endhighlight %}
		
	2）、编写PKG文件。
	{% highlight c++ %}
	// 如果要包含C/C++的头文件，使用 $#include "xxx.h"
	// 如果要包含其它的PKG文件，使用 $pfile "xxx.pkg"
	$#include "CTest.h"

	class CTest
	{
		int v;
		CTest();
		CTest(int a);
		void test();
	};
	{% endhighlight %}
		

		
		
	3）、执行tolua++，生成lua_CTest.cpp文件。
		tolua++ -n CTest -o lua_CTest.cpp CTest.pkg
		
	4）、将生成的文件加入到工程。
	
	5）、在LUA中使用C++的CTest对象。
		tolua_CTest_open(state); // 这个方法的名称中的CTest取决于执行tolua++时-n后的参数
		std::string str = "t = CTest:new(); t:test();";  // 当然也可以把LUA命令写到lua文件中，然后直接执行lua文件。
		luaEngine->executeString(str.c_str());

		// 要顺利的调用到tolua_CTest_open方法，还需要把lua_CTest.cpp文件中的方法声明取出并放到合适的地方。		
		// TOLUA_API int  tolua_CTest_open (lua_State* tolua_S);


