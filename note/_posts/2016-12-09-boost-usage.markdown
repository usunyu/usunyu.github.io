---
layout: post
title:  "Use boost for serialization"
---

### Use boost on MacOS:

##### 1. Install [boost](http://www.boost.org/doc/libs/1_62_0/more/getting_started/unix-variants.html):
{% highlight bash %}
$ ./b2 install
{% endhighlight %}

##### 2. Setup Xcode:
Header Search Paths:
{% highlight bash %}
/usr/local/boost
{% endhighlight %}

Library Search Paths:
{% highlight bash %}
/usr/local/boost/stage/lib
{% endhighlight %}

Linked Frameworks an Libraries:
{% highlight bash %}
Add dylib in /usr/local/boost/stage/lib
{% endhighlight %}

Other Linker Flags:
{% highlight bash %}
-lboost_serialization
{% endhighlight %}

##### 3. Example serialization:
{% highlight c %}
#include <boost/archive/text_oarchive.hpp>
#include <boost/archive/text_iarchive.hpp>
#include <boost/serialization/map.hpp>
#include <map>
#include <iostream>
#include <fstream>

struct A {
    std::string aString;
    bool aBool;
    
    A() {}
    
    A(std::string s, bool b) {
        aString = s;
        aBool = b;
    }
    
private:
    friend class boost::serialization::access;
    template <typename Archive>
    void serialize(Archive &ar, const unsigned int version) {
        ar & aString;
        ar & aBool;
    }
};

struct B {
    std::string bString;
    std::map<std::string, A> aMap;
    
    B(std::string s) {
        bString = s;
        aMap = std::map<std::string, A>();
    }
    
    void AddA(std::string key, A a) {
        aMap[key] = a;
    }
    
private:
    friend class boost::serialization::access;
    template <typename Archive>
    void serialize(Archive &ar, const unsigned int version) {
        ar & bString;
        ar & aMap;
    }
};

void save() {
    std::ofstream file("archive.bin");
    boost::archive::text_oarchive oa(file);
    A a = A("This is A struct", true);
    B b = B("This is B struct");
    b.AddA("A", a);
    oa << b;
}

void load() {
    std::ifstream file("archive.bin");
    boost::archive::text_iarchive ia(file);
    B b = B("This is new B struct");
    ia >> b;
    std::cout << b.bString << std::endl;
    std::cout << b.aMap["A"].aString << std::endl;
}

int main() {
    save();
    load(); 
}
{% endhighlight %}

### Use boost on Windows:

##### 1. Install [boost](http://www.boost.org/doc/libs/1_62_0/more/getting_started/windows.html):
{% highlight bash %}
$ bootstrap
$ .\b2 --build-type=complete  address-model=64
{% endhighlight %}

##### 2. Setup Visual Studio:
VC++ Directories -> Include Directories:
{% highlight bash %}
boost
{% endhighlight %}

VC++ Directories -> Library Directories:
{% highlight bash %}
boost/stage/lib
{% endhighlight %}

C/C++ -> Precompiled Headers:
{% highlight bash %}
Not Using Precompiled Headers
{% endhighlight %}

#### Resource:
* [Boost C++ 库 第 11 章 序列化](http://zh.highscore.de/cpp/boost/serialization.html)
* [How to set up Boost in Visual Studio 2012 on Windows 8](https://www.youtube.com/watch?v=6trC5zVXzG0)
