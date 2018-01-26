
-l参数是用来指定程序要链接的库，-l参数紧接的就是库名。-L用来指定程序要链接库的位置，-L参数紧接的就是库名的路径。
库名和库文件名有一定的区别：
如：数学库的库名是m，库文件名是libm.so。显而易见，库名就是库文件名的掐头（lib）去尾（.so）。

-l参数的使用：
如要使用第三方提供的库文件名为libtest.so的库文件，则需将libtest.so拷贝到/usr/lib里，编译时加上参数-ltest参数即可链接到libtest.so库如：g++ helloworld.cpp -ltest。而要使用libtest.so里的函数，则需要与libtest.so相匹配的头文件。
放在/lib、/usr/lib和/usr/local/lib里的库文件直接用-l参数链接，但如果库文件不存在于上述三个文件夹的任何一个中，而是在其他文件夹里，仅使用-l参数则会导致链接出错，出错信息类似于：“/usr/lib ld：cannot find -lxxx”,即链接程序ld在上述三个文件夹里没有找到libxxx.so库文件，此时需使用-L参数。

-L参数的使用：
如要使用xyz这一库文件，首先需要知道它所存放的位置，如：/usr/rsu/sur/lib文件夹下，编译时需使用-L参数：-L/usr/rsu/sur/lib -lxyz。

除此之外，大部分libxxxxxx.so只是一个链接，如libm.so链接到/lib/libm.so.x，/lib/libm.so.6链接到/lib/libm-2.3.2.so，如果没有这样的链接，编译时依旧会出错，原因在于命令ld只会寻找libxxxxxx.so文件。所以在只有libxxxxxx.so.x或者libxxxxxx-x.x.x.so的情况下，使用xxxxxx库做一个链接即可：ln -s libxxxxxx-x.x.x.so libxxxxxx.so。

xxxxxx-config的用法：
为开发方便，很多库开发包提供了生成链接参数的程序，类似于xxxxxx-config，一般存放于/usr/bin文件夹下。如xxxxxx1.5.1的链接生成程序是xxxxxx-config，执行xxxxxx-config --libs就能得到编译xxxxxx1.5.1程序所需要的xxxxxx链接参数。xxxxxx-config除了--libs参数外，参数--cflags用来生成头文件包含目录，即-I参数。比如编译一个xxxxxx程序：gcc xxxxxxtest.c ~xxxxxx-config --libs --cflags~。

pkg-config的用法：
除了xxxxxx-config以外，开发包多使用pkg-config来生成链接参数，使用方法和xxxxxx-config类似，但xxxxxx-config是针对特定的开发包，而pkg-config则是针对多个开发包的链接参数的生成，用pkg-config --list-all命令可以列出所有支持的开发包，用法是pkg-config packagename --libs --cflags，packagename是包名，是上述所列出的支持的开发包的其中一个。例如xxxxxx1.5.2的包名是xxxxxx+，pkg-config xxxxxx+ --libs --cflags的作用和pkg-config --libs --cflags相同，编译执行gcc xxxxxxtest.c ~pkg-config xxxxxx+ --libs --cflags~即可。

-I参数的使用：
-I参数用来指定头文件所在文件夹，一般情况下，gcc会自动访问/usr/include文件夹下的头文件，但头文件不在上述文件夹下则需要用-I参数指定头文件的路径。如头文件放在/usr/sur/myproject/myinclude/include文件夹下，则编译命令行需添加-I/usr/sur/myproject/myinclude/include参数，否则会出现类似于“xxxxxx.h：No such file or directory”的错误。路径的指定同样可使用相对路径，如待编译的文件xxxxxx.c在上述文件夹的myproject文件夹下，头文件在include文件夹下，则编译命令为：gcc -I ./myinclude/include xxxxxxtest.c xxxxxxtest.o。
