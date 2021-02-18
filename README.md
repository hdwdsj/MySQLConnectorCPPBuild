openssl、mysqlclient、mysql connector c++均安装在本地

vs2019 命令行工具 x86 Native Tools Command Prompt for VS 2019

1、先编译openssl静态库，/MT;
2、编译mysqlclient.lib,/MT,
cmake -G"Visual Studio 16" -A Win32 -DCMAKE_BUILD_TYPE=DEBUG -DWITH_DEBUG=1 -DLINK_STATIC_RUNTIME_LIBRARIES=ON -DWITHOUT_SERVER=ON -DCMAKE_INSTALL_PREFIX=C:\mysql\ -DWITH_BOOST=F:\boost_1_73_0 -DWITH_SSL="C:\OpenSSL\x86" ..
cmake --build . --target install --config Debug

3、编译mysql connector c++, /MT,
源码中所有的 STATIC_MSVCRT 修改为ON
cmake -G"Visual Studio 16" -A Win32 -DCMAKE_BUILD_TYPE=Debug -DSTATIC_MSVCRT=ON -DBUILD_STATIC=ON -DWITH_JDBC=ON -DCMAKE_INSTALL_PREFIX=C:/mysql-cppconn-8/ -DWITH_MYSQL="C:\mysql" -DWITH_BOOST=F:\boost_1_73_0 -DWITH_SSL="C:\OpenSSL\x86" ..
cmake --build . --target install --config Debug

4、使用
add_definitions(-DSTATIC_CONCPP)

include_directories(C:/mysql-cppconn-8/include/jdbc
					F:/boost_1_73_0
					${spdlog_SOURCE_DIR}/include
					${cpp-sdk_SOURCE_DIR}/include)

link_directories(C:/mysql-cppconn-8/lib/vs14/debug)

set(TARGETNAME connector_test)
	
file(GLOB CURRENT_SRC *.h *.cpp *.hpp)				  

add_executable(${TARGETNAME} ${CURRENT_SRC})

target_link_libraries(${TARGETNAME} mysqlcppconn8-static-mt.lib Dnsapi.lib User32.lib Advapi32.lib)

set_target_properties(${TARGETNAME} PROPERTIES FOLDER "test")
