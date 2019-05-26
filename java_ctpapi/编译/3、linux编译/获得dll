##编译
####环境
    linux系统机器一台
    
    
 ####添加文件
    thosttraderapi.so 重命名 加前缀lib
    使用刚才的thosttraderapi_wrap.cpp和thosttraderapi_wrap.h两个文件拷贝到linux相应的目录底下。现在linux目录底下的文件应该如下
    
    libiconv.a
    ThostFtdcTraderApi.h
    ThostFtdcUserApiStruct.h
    thosttraderapi_wrap.h
    libthosttraderapi.so
    makefile
    ThostFtdcUserApiDataType.h
    thosttraderapi_wrap.cpp
    iconv.h
其中makefile的内容如下：

    OBJS=thosttraderapi_wrap.o
    INCLUDE=-I./ -I${JAVA_HOME}/include -I${JAVA_HOME}/include/linux
    TARGET=libthosttraderapi_wrap.so
    CPPFLAG=-shared -fPIC
    CC=g++
    LDLIB=-L. -lthosttraderapi
    $(TARGET) : $(OBJS)
    	$(CC) $(CPPFLAG) $(INCLUDE) -o $(TARGET) $(OBJS) $(LDLIB) ./libiconv.a
    $(OBJS) : %.o : %.cpp
    	$(CC) -c -fPIC $(INCLUDE) $< -o $@ 
    clean:
    	-rm -f $(OBJS)
    	-rm -f $(TARGET)
    install:
    	cp $(TARGET) /usr/lib
    	
执行make命令，得到libthosttraderapi_wrap.so库，说明编译成功。

    
