##编译

####项目属性设置

将你安装jdk目录\Java\jdk1.8.0_111\include下的jni.h和win32文件夹下的jni_md.h, jawt_md.h一共三个文件拷贝到安装vs的include目录底下D:\software\VS2013\VC\include。
这是因为thosttraderapi_wrap.cpp文件中包含了<jni.h>,这是用于生成Java可调用接口的库文件。

    通过C++得到java可调用的dll动态库
    
    使用vs新建项目->32控制台应用程序
    项目名字->thosttraderapi_wrap
    应用程序类型->DLL
    附加选项->空项目
    右键项目属性：
    配置属性->c/c++->代码生成->运行库->多线程(/MT)
    配置管理器->release->平台新建64位
    配置属性-> 链接器 -> 命令行 添加： /NODEFAULTLIB:"libcmt.lib"
    
 ####添加文件
    ThostFtdcTraderApi.h
    ThostFtdcUserApiDataType.h
    ThostFtdcUserApiStruct.h
    thosttraderapi.lib
    thosttraderapi_wrap.cpp
    thosttraderapi_wrap.h
    libiconv.lib
    iconv.h
####获取产物
    结算单乱码的解决方案
    结算单乱码的主要原因是分段按byte截断传输，不考虑unicode可能多个byte的情况，因此中途转码会丢失数据，一个简单的解决方案，在Java中还原为byte[] 拼接，然后再new String ，使用编码GB2312,此问题便解决
    
    1.在生成的CPP中
    搜索
    CThostFtdcSettlementInfoField_1Content_1get
    将返回类型改为jbyteArray 
    将函数内容替换为
    /////////////////////////////////////
      jbyteArray jresult = 0 ;
      CThostFtdcSettlementInfoField *arg1 = (CThostFtdcSettlementInfoField *) 0 ;
      char *result = 0 ;
      
      (void)jenv;
      (void)jcls;
      (void)jarg1_;
      arg1 = *(CThostFtdcSettlementInfoField **)&jarg1; 
      result = (char *) ((arg1)->Content);
      {
    	  if (result) {
    		  jresult = jenv->NewByteArray( strlen(result));
    		  jenv->SetByteArrayRegion(jresult, 0, strlen(result), (jbyte*)result);
    	  }
    
      }
      return jresult;
    
    /////////////////////////////////////
    
    2.手动将CThostFtdcSettlementInfoField.java文件中的函数 getContent()方法的返回类型改为byte[],将其调用的其他类的方法的返回类型也改为byte[]直到无错为止
    
    3.在java中，在没有返回last标记之前，存储所有byte[],返回标记之后拼接为一个大byte[] 使用new String(contentBytes,"GBK")，便可得到完全正确的结算单

项目文件夹的64文件夹内及生成文件。目标dll
    
